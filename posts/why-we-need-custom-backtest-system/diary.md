# Notes

Title:
《为什么回测最后也要自己接起来》

Source line:

2016 香港 Cyberport，小团队，网格 + CTA。

Required incidents:

一个很有天分的犹太同事写策略，交易系统由另一人负责。
回测与实盘不完全错误，而是逐步 drift。
有些信号只在回测里出现，实盘没有；反过来也成立。
PnL 不是断裂，而是慢慢偏开。
后来意识到回测只是历史数据 replay 进函数。
实盘里有 queue、websocket delay、execution path、partial fill、cancel/retry、cross-exchange execution。

Backtrader:

尝试把策略放进 Backtrader。
tick vs kline 数据源先对不上。
适配数据时已经在外面先写了一层系统。
框架的执行模型太干净，不能承接真实执行过程。

End state:

策略留在框架里，执行在框架外。
同一笔 trade 同时存在于两个世界。
难以分清在对比结果还是在对比路径。

Writing:

保留 Drift。
不要和 2019 多交易所路由混在一起。
