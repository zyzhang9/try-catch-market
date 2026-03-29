# Notes

## Context

- 上一篇已经到达一个结论：如果目标是生产，海底光缆流量方案最后一定会落到网络层
- 应用继续像平常一样发请求
- 路径选择不再散落在 `SDK`、代理参数和连接代码里
- 真正麻烦的部分从网络层落地才开始

## Real Problems

- 不再只是“能不能让 `api.binance.com` 走 London 出口”
- 变成：
  - 节点怎么分工
  - `GRE` 隧道挂在哪里
  - `route table` 怎么切
  - 域名解析出来的新 `IP` 由谁接住
  - `UFW` 和 `NAT` 放在哪一侧
  - 这些动作靠人记得执行，还是已经被 `systemd` 接成长期系统

## Topology

- 最后稳定下来只用了两个核心节点
- Tokyo 是交易流量真正发起的地方
- 策略、下单、行情连接大多在 Tokyo
- 默认出口尽量保持简单
- 少数确定要借 London 出口的流量被单独分出去
- London 是远端出口节点
- Tokyo 送来的流量由 London 接住，再用自己的公网网卡发到交易所
- 对 Tokyo 来说，London 不是代理，而是另一张远端出口卡
- 对 London 来说，职责是：
  - 解 `GRE`
  - 转发
  - 做 `NAT`
  - 把回包送回隧道

## Tunnel

- 最后选的是 `GRE`
- 原因不是最时髦，而是足够直接
- Tokyo / London 之间起固定名字的接口，例如 `gre-lon` / `gre-tok`
- 隧道地址是一小段内网，例如 `10.254.12.1/30` 和 `10.254.12.2/30`
- 价值：
  - `route table`
  - `UFW`
  - 监控脚本
  - `systemd service`
  都可以围绕这对确定接口展开
- Tokyo 决定哪些流量进隧道
- London 保证进隧道的流量能出去再回来

## Routing

- `route table` 才更像生产系统骨架
- 不改主路由表，单独保留一个表，例如 `200 subsea`
- `main` 继续服务默认流量
- `subsea` 只服务走海缆出口的目标地址
- 这样“特殊路径”被收束在局部系统里
- 动作可以限制在这个表里，不拖动整台机器的出网行为
- 实际分流方式：
  - 把目标域名解析出的 `IP` 按 `/32` 写进 `subsea`
  - 用 `ip rule` 把命中特定目标集的流量导向这个表
- 系统看起来像按域名分流，但内核真正理解的仍然只是 `IP`

## DNS Sync

- 域名会变，路由不会自己变
- 在网络层生产化后，这不再只是观察，而是必须接住的维护动作
- 不能把解析逻辑塞进一次性部署脚本
- 需要做成持续运行的更新器，例如 `subsea-route-sync`
- 每轮去解析目标域名，写当前快照，再和内核现有 `/32` 路由做 reconcile
- 生产里真正重要的是：
  - 更新过程是否原子
  - 旧地址是否立刻删
  - DNS 短时抖动时是否会把整张表洗空
  - 多个 `hostname` 解析到同一个 `IP` 时谁来去重
- 一个稳定结构可能是：
  - `/etc/subsea/targets.yml`
  - `/usr/local/bin/subsea-route-sync`
  - `/var/lib/subsea/routes.json`
  - `ip route show table subsea` 只作为结果视图，不作为状态源

## UFW and NAT

- 问题常在“基本能通”之后才暴露
- London 侧最容易忽略
- 包能从 Tokyo 送到 London，但交易所不回
- 往往不是隧道问题，而是转发链路没有放开
- London 侧明确职责：
  - 允许 `gre-tok` 进入
  - 允许从 `GRE` 到公网网卡的 `forward`
  - 允许已建立连接返回
- 类似规则：
  - `ufw route allow in on gre-tok out on ens5`
  - `ufw route allow in on ens5 out on gre-tok`
- `NAT` 需要在 London 侧把 Tokyo 发出的隧道内地址变成 London 的公网身份
- 例如：
  - `iptables -t nat -A POSTROUTING -o ens5 -j MASQUERADE`

## Serviceization

- 没有 `systemd`，方案很容易停留在危险状态：
  - 隧道手工起
  - 路由靠人补
  - DNS 更新靠 `cron`
  - `UFW` 重启后是否恢复没人确定
- 比较稳定的一版会把部件拆成明确 service：
  - `subsea-gre@london.service`
  - `subsea-route-sync.service`
  - `subsea-route-sync.timer`
  - `subsea-healthcheck.service`
- 重要的不是名字，而是系统开始以服务为单位表达自己
- 路径控制第一次从工程经验变成机器上的可见状态

## Working Conclusion

- “让流量走海底光缆”最后不是网络技巧，而是一套服务化过的路径控制系统
- 生产不是消灭不确定性，而是把不确定性装进一套还能继续运行的组织方式里
