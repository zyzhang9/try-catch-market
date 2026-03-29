# Notes

## Context

- 2022 年做 `CEX / DEX` 套利
- 对冲模块部署在 Tokyo
- 需要同时订阅多路币安行情
- 当时是在做多条 `websocket` 连接的连接测试

## Observation

- 同一个交易所、同一个 `region`、同一个 `websocket URL`，同时建立多条连接后，延迟表现并不一致
- 连接建立后不久就会出现稳定分层
- 其中一条通常快约 `1-2ms`
- 另外几条会持续慢一些
- 这种差异不是运行数小时后才累积出来的，而是在连接建立后的早期阶段就已经出现

## Possible Structure

- 一个 `hostname` 可能对应多组 `IP`
- 不同运行时环境可能选到不同入口
- 连接可能落入不同边缘节点或接入路径
- 即使解析到相同 `IP`，表现也往往不同
- 服务器端的 `fan-out` 分发位置也可能带来长期稳定差异
- 连接一旦进入某个分发位置，后续消息到达顺序可能在较长时间内保持相对稳定

## Engineering Response

- 同时维护一组 `websocket` 连接
- 周期性替换表现最差的一条
- 评价方法：
  - Binance 的 `depth` 行情自带交易所时间戳
  - 不同连接不需要彼此比对数据
  - 只要看消息到达本地时与交易所时间之间的差异，就可以直接判断各自的延迟表现
- 系统维持 `5` 条候选连接
- 每隔 `10` 分钟关闭延迟最差的一条，再建立新连接补入
- 为了不影响行情系统，这件事不在行情进程中做，而是在单独进程里做
- 一旦发现当前的行情连接组合还有明显改进空间，再重启行情进程切换进去
- 随着时间推移，连接组合会收敛到更稳定的区间

## Implementation Notes

- 具体的使用方法是 Miley 告诉我的
- 注意点：
  - 必须仍然使用域名，否则会被拒绝
  - 可以先用 `IP` 建立 `socket`，再把这个 `socket` 传给 `websockets.connect`

```python
import asyncio
import json
import socket
import time
from typing import List, Dict, Any
import websockets

DOMAIN = "api-pub.bitfinex.com"
PORT = 443
PATH = "/ws/2"

IP_LIST = [
    "104.16.164.90",
    "104.16.165.90",
    "104.16.166.90",
    "104.16.167.90",
    "104.16.168.90",
]

SUBSCRIPTION = {
    "event": "subscribe",
    "channel": "trades",
    "symbol": "tETHUSD"
}

async def subscribe_trades(ip: str, result_queue: asyncio.Queue):
    uri = f"wss://{DOMAIN}{PATH}"
    start_time = time.perf_counter()
    try:
        # 1️⃣ TCP socket 直连指定 IP
        raw_sock = socket.create_connection((ip, PORT))

        # 2️⃣ websockets 自动 TLS
        async with websockets.connect(
            uri,
            sock=raw_sock,
            open_timeout=5
        ) as ws:
            handshake_latency = time.perf_counter() - start_time

            # 3️⃣ 发送订阅消息
            await ws.send(json.dumps(SUBSCRIPTION))
            print(f"[{ip}] Subscribed to trades channel.")

            # 4️⃣ 接收消息
            while True:
                msg = await ws.recv()
                recv_time = time.perf_counter()

                try:
                    data = json.loads(msg)
                except Exception:
                    data = msg

                # 输出消息内容
                print(f"[{ip}] MESSAGE: {data}")

                # 首条 trades 消息统计 latency
                if isinstance(data, list) and len(data) > 1 and isinstance(data[1], list):
                    first_msg_latency = recv_time - start_time
                    await result_queue.put({
                        "ip": ip,
                        "handshake_latency_ms": handshake_latency * 1000,
                        "first_trade_msg_latency_ms": first_msg_latency * 1000
                    })
                    # 继续打印其他消息，不退出
    except Exception as e:
        await result_queue.put({
            "ip": ip,
            "error": str(e)
        })

async def main(ip_list: List[str]):
    result_queue = asyncio.Queue()
    tasks = [subscribe_trades(ip, result_queue) for ip in ip_list]
    # 并发运行，限制为 1 秒内先收首条消息统计 latency
    await asyncio.gather(*tasks)

    results = []
    while not result_queue.empty():
        results.append(await result_queue.get())

    results.sort(key=lambda x: x.get("handshake_latency_ms", float("inf")))
    print("=== Bitfinex ETHUSD Trades Latency Results ===")
    for r in results:
        if "error" in r:
            print(f"{r['ip']}: ERROR -> {r['error']}")
        else:
            print(
                f"{r['ip']}: handshake {r['handshake_latency_ms']:.2f} ms, "
                f"first_trade_msg {r['first_trade_msg_latency_ms']:.2f} ms"
            )

if __name__ == "__main__":
    asyncio.run(main(IP_LIST))
```

## Timeline

- 最开始的诊断大概用了 `1` 周
- 后来准备 rollout 之后，又持续做了大约 `2` 周
