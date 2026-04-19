# Restartable Split Plan

This note records the current split decision for the restartable-systems topic.

The goal is to separate:

- why the team prefers whole-process restart over local repair
- what a restart-friendly process should look like once restart is accepted

The split exists to keep each article centered on one cognitive move.

---

## Article 1

Working title:

- 为什么他们更愿意重启整个进程

Core question:

- 为什么不是某个连接断线重连
- 为什么不是某个访问重试
- 为什么不是某张单补下或重下
- 为什么最后更愿意把恢复边界抬到整个进程

Primary material:

- `kraken` account websocket abnormal close
- `ws recv task` exception causing task-chain shutdown
- deep `REST 502` on `BalanceEx`
- single-process / single-thread / multi-coroutine runtime
- propagation closer to `asyncio.wait(..., return_when=asyncio.FIRST_COMPLETED)`
- open orders remaining on exchange after strategy task stops
- distrust of simple reconnect during uncertain recovery window
- supervisor auto-restart with interval

Structural shape:

1. Start from concrete failure texture
2. Show why local repair spreads complexity across layers
3. Contrast reconnect / retry / re-place with process-level restart
4. Show supervisor restart as the team's chosen recovery boundary
5. End by handing off to the next question:
   if restart is normal, what should the process itself look like?

Ending bridge:

- process-level restart only works if the restarted program is light enough to rebuild from scene reality

---

## Article 2

Working title:

- 从重启中恢复必须是轻量级的

Core question:

- 如果允许进程重启，这个进程本身应该长成什么样
- 为什么恢复必须轻量
- 为什么要从现场重建
- 为什么信号必须幂等

Primary material:

- order / working order / trade / cancel as different semantic layers
- restart should restore scene state rather than continue action chain
- startup recovery path: reconnect -> account state -> engine -> idempotent signal -> order adjustment
- executor as convergence layer
- target state preferred over action state

Structural shape:

1. Clarify dataflow objects and semantic layers
2. Show why action semantics fracture under interruption
3. Define startup-time scene recovery
4. Introduce idempotent signal as recovery-safe boundary
5. Land on lightweight restart-friendly executor shape

Ending:

- restart becomes ordinary only after the program stops treating old in-process continuity as sacred
