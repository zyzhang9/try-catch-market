# Notes

## Starting Point

- 很多交易系统最开始不是“设计”出来的，而是先跑起来的
- 先有一个 `main`
- `main` 拉起交易所连接、订阅行情、初始化策略、挂上执行器
- 随着策略、执行逻辑、定价模型变多，系统里逐渐出现：
  - `strategy`
  - `executor`
  - `pricer`
- 真正决定它们能否长期共存的，往往不是类图，而是配置

## Why Configuration Matters

- 只要系统还停留在“代码里顺手 new 出来”的阶段，架构其实并未稳定
- 换执行方式要改 `main`
- 加参数要重新穿透初始化链路
- 同一套利逻辑换交易所、账户、参数组合时，会退化成一堆相似启动脚本
- 所谓架构，不只是模块如何调用模块，而是系统如何被组织起来
- 配置是这种组织方式最直白的外部表现

## Layered Shape

- `main` 做的事情通常很少
- 常见形状：读一份 `YAML`，取出 `trader` 段，然后 `Trader(**trader_config)`
- 系统形状开始写在配置里，而不是 `main` 里

### trader

- 最上层对象
- 负责装配和生命周期
- 决定：
  - 跑哪个 `strategy`
  - 接哪个 `executor`
  - 用哪个 `pricer`
  - 连哪些 venue
  - 账户和风险边界落在哪里

### strategy

- 决定目标本身
- 不直接下单
- 产生中间状态：
  - `fair value`
  - `signal`
  - `target position`
  - `inventory preference`
- 配置固定实验条件
- 常通过 `type` 字段动态加载

### executor

- 决定怎么把目标变成真实仓位
- 配置核心常是 `accounts`
- 账户对应不同 venue、权限、连接方式
- 在 maker-taker 系统中，可能是总执行器下挂多个 component
- 配置描述的不只是参数，而是执行结构和职责分配

### pricer

- 决定系统怎么理解当前市场
- 可能很简单，也可能包含：
  - `fee`
  - `queue position`
  - `fill probability`
  - `inventory penalty`
- 也可能通过 `type` 字段动态创建
- 一旦可替换，配置就在声明系统当前信任哪一种市场解释方式

## Configuration Consequence

- 配置化真正改变的，不是参数放在哪里
- 而是系统边界第一次被明确下来
- `legs` 和 `accounts` 把市场连接和交易资源真正落到实例上
- 只有能被配置稳定组装时，系统架构才算真正成形

## Dataclass Shape

- Python 里的 `dataclass` 是自然落点
- 未必需要先定义完整 `*Config` schema
- 常见折中形状：
  - `YAML` 读成 `dict`
  - 外层对象本身是 `dataclass`
  - 在 `__post_init__` 里继续展开嵌套配置
- 这很符合交易系统逐步长出来的过程

## Value

- 配置链路开始可见
- 配置像缩小版系统蓝图
- 价值不只是启动方便，而是实验能力：
  - 同一个 `strategy` 可以挂不同 `executor`
  - 同一个 `executor` 可以切不同 `pricer`
  - maker-taker 结构可以换 venue、费率、inventory limit
- 系统第一次具备“不修改主体代码就能重组自己”的能力

## Limits

- 误区：把一切都塞进配置
- 结果是复杂度从代码挪到 `YAML`
- 字段越来越多，默认值越来越模糊
- 重要约束关系没进入 schema
- 配置开始掩盖架构

## Working Conclusion

- 有价值的配置化不是“所有东西都能配”
- 组件边界应该写在代码里
- 组件实例和组合关系可以进入配置
- 实验参数应该进入配置
- 动态加载可以进入配置，但合法性校验必须留在代码里
- 不变量、约束和校验逻辑必须留在代码里
- 配置负责表达系统想怎么被组装，代码负责保证这种组装不会失去基本正确性
