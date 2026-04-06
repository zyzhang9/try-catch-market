# Notes

Title:
《他们不是一开始就自己写 driver》

Source line:

交易系统的演化通常不是 “ccxt or 全自研” 的二选一。
更真实的路径是：
先用 ccxt 接起来，
再包自己的 adapter，
最后把关键路径逐步收回成自写 driver。

Core point:

不要把 ccxt 写成错误选择。
ccxt 解决的是前期“先接起来”的问题。
真正失效的不是统一接入本身，而是它无法承接最终系统语义。

Stages:

Toy / 原型阶段：
快速连 Binance。
快速拿到行情、余额、下单接口。
先把 toy、原型、回测外壳、最小实盘链路跑通。

早期系统阶段：
继续用 ccxt。
但在业务代码外面包自己的 adapter。
不要让业务代码在各处直接调用 ccxt。

生产演化阶段：
逐步替换关键路径成自写 driver。
不是一下子把 ccxt 全扔掉。
也不是一直停在 ccxt。

Key split point:

真正先出问题的通常不是 REST。
`fetch_balance`、`create_order` 这类接口，前期用 ccxt 很合适。
差异会在这些地方迅速放大：

- websocket
- 用户数据流
- 订单回报顺序
- partial fill
- 补快照
- retry / reconnect / idempotency
- exchange-specific exceptions

System semantics:

后期真正关心的是：

- 本地订单状态怎么流转
- 成交、余额、持仓谁先谁后
- websocket 断线后怎么补
- 某个交易所的特殊字段和异常怎么解释

这些越来越像“系统自己的定义”，不是统一库能替你决定好的。

Possible narrative:

起初 ccxt 让团队很快把接口接起来。
后来业务代码开始不愿意直接碰 ccxt，需要一层自己的 adapter。
再后来不是所有东西都改写，而是先把最关键的 execution path 收回来。
最终 ccxt 还在，但位置下沉，变成接入材料。

Writing:

保留 Drift。
不要写成建议文，不要写成“三阶段教程”。
不要说“最佳实践”。
要写成他们在生产推进里，如何逐步失去统一，又如何慢慢收回关键路径。

Balance with sibling post:

这篇写：
为什么他们不是一开始就自己写 driver。
重点是演化路径、分层替换、ccxt 的合适边界。

另一篇《为什么他们后来都自己写交易系统》写：
为什么统一框架最后兜不住真实执行行为。
重点是多路径执行、交易所事件顺序、统一语义失效。
