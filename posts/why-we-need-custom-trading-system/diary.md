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
