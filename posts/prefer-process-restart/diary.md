# Notes

## Core Trigger

- 主题不是“如何优雅重连”
- 而是为什么团队最后更愿意把恢复边界抬到整个进程
- 对比对象：
  - 某个连接的断线重连
  - 某个访问的局部重试
  - 某张单的补下或重下
- 结论方向：
  - 与其在局部补过程连续性，不如让进程退出再由 supervisor 拉起

## Failure Texture

- `kraken` account websocket 会出现 `ConnectionClosedError: no close frame received or sent`
- 一个 `ws recv task` 异常后，任务链会依次收掉：
  - `rest request task canceled`
  - `Strategy task canceled`
  - `ws send task canceled`
  - `Trader task finished`
- 还出现过更深层的 `REST` 故障：
  - `BalanceEx` 连续返回 `502 Bad Gateway`
  - `retry 1/3` 到 `retry 3/3` 后仍失败

## Runtime Shape

- 执行器是单进程、单线程、多协程
- 协程之间更接近按 `asyncio.wait(..., return_when=asyncio.FIRST_COMPLETED)` 这种方式收拢
- 一个关键协程终止，会把停止一级级向上传递

## Why Local Repair Is Distrusted

- 策略协程停掉之后，交易所挂单还在，是正常现实
- 简单断线重连不安全
- 重连窗口里外部世界发生了什么并不确定
- 本地协程树已经开始收掉，外部世界却还在继续推进
- 真正危险的是继续假设“某一小段还可以原地续上”
- 继续在局部恢复，就会在多处分别补逻辑：
  - `balance`
  - `position`
  - `working orders`
  - 最近成交与过程语义

## Supervisor Reality

- supervisor 自动拉起进程
- 不需要预期每一种错误类型
- 重启之间有时间间隔
- 这不是手工应急动作，而是正常恢复路径
- 重点不是“某类错误该怎么补”
- 而是“只要进程整体已经不干净，就统一退出再回来”

## Ending Direction

- 第一篇只讲“为什么更愿意重启整个进程”
- 第二篇再讲“允许重启的程序应该长成什么样”
