# Notes

Title:
《为什么他们后来都自己写交易系统》

Source line:

2019 北京，加密钱包公司，后端交易系统。

Required incidents:

钱包用户产生交易需求，订单被路由到外部交易所执行，公司不做内部撮合。
系统从 strategy-driven 变成 flow-driven。
同时连多家交易所，多个 websocket / REST 连接，多个执行路径。
连接只要慢几十毫秒，报价和路由就会偏。
一个交易所不可用，流量重新分配，库存与成交路径一起变形。
大部分开源框架默认单执行路径，不自然表达多路径执行系统。

CCXT:

统一 API 最初很方便。
Binance futures 支持不完整，字段不一致，状态 lag。
fork、patch、本地维护、提 PR，无响应。
debug 时混淆：上游行为还是本地 patch。
library 最后更像原材料。

Additional source note:

`ccxt -> adapter -> 自写 driver` 这条演化线已拆到独立 dossier：
`posts/they-did-not-write-the-driver-on-day-one/`

当前这篇只保留和主线直接相关的部分：

- CCXT 作为统一抽象在生产语义前开始失效
- fork / patch 之后库逐渐变成原材料
- 真正压垮统一抽象的是多路径执行与交易所事件顺序

Exchange timing:

Binance 先 trade execution，后 balance update。
Kraken 先 balance，再 trade。
几十到几百毫秒的窗口内，本地 position / balance 不一致。
如果统一处理，一定会对某一类交易所长期判断错误。
系统需要编码 exchange-specific behavior。

Writing:

保留 Drift。
正文里让 Drift 作为观察者出现 1-2 次。
不要写成框架评论。

Boundary note:

当前 `post.md` 更偏“为什么最后得自己掌握交易系统行为”，
重点是多路径执行、交易所时序差异、统一抽象失效。

上面那条 ccxt -> adapter -> 自写 driver 的材料，
已经单独开篇，不再继续并入当前 `post.md`。

两篇平衡方式：

这一篇保留：
为什么统一框架最终兜不住生产里的真实执行行为。

另一篇单独写：
他们并不是从第一天就全自研，而是在 ccxt 之上逐步把关键路径收回自己手里。
