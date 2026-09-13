# Style Evidence

## Corpus

Source: `Maoer233/astrbot_plugin_steam_status_monitor`, branch `main`, filtered to author `OLRainM` and analyzed through 2026-09-13.

All 66 returned commit pages were inspected for the full commit message, changed-file list, and diff statistics. Four merge commits were read but excluded from the personal-style rules, leaving 62 authored non-merge samples.

Observed distribution:

- Types: `fix` 31, `refactor` 11, `feat` 8, `docs` 5, `test` 3, `chore` 3, `perf` 1.
- Explicit scopes: 35 of 62. Frequent scopes were `session`, `price`, `monitor`, `store`, `render`, `fonts`, `通知`, `qq菜单`, and `heatmap`.
- Commit body present: 59 of 62.
- Two or more body paragraphs: 53 of 62.
- Bullet-list bodies: 5 of 62, used for issue fixes or multi-part performance work.
- Explicit issue references: 4 of 62.

The dominant pattern is therefore not merely a Conventional Commits header. It is a Chinese header plus a compact engineering explanation: mechanism first, then cause, constraint, or preserved behavior.

## Representative Patterns

### Focused Bug Fix

```text
fix(store): 把 appids 放进 appdetails 查询参数

商店详情和区价请求改为 params={"appids": gid, ...}，避免 httpx 用 cc/l 整段替换 URL 查询串。测试在缺 appids 时按 400 失败。

原先把 appids 写在 URL 上，httpx params 会盖掉查询串，实际请求变成 appdetails?cc=cn，Steam 直接 400，锁区回退全部失效。
```

This shows the usual ordering: exact implementation and verification, blank line, then causal failure path.

### Architectural Refactor

```text
refactor(session): 用 SessionService 接管检测循环

检测循环只投递快照，切游戏立即 close 上一局；真退出由主轮询每分钟 tick_due 到期结算。列表与状态命令改读 started_at，开始/结束通知和成就轮询订阅会话事件，并落盘 playing_sessions.json。

原先 delayed quit task 与检测循环内 pending 双路径会在 A→B 时丢时长；把所有权收到 SessionService 后，记账和结束通知绑定同一次 close，离线玩家也不再依赖下一次快照才到期。
```

Refactor bodies name the new owner, the responsibilities moved, the old ownership defect, and the semantic boundary preserved.

### Feature Constrained By Operations

```text
feat(fonts): 商店包改为运行时下载 CJK 字体

从 Git 跟踪中移除约 39MB 字体文件，启动后后台从 fonts-bundle 下载并校验 zip，解压到数据目录后再供渲染使用；新增 /steam fonts 状态、下载进度和清理命令。

商店 zip 不得超过 16MB，内置全量 CJK 字体无法过审；运行时下载可先启动插件，缺字时回退系统字体，开发目录若已有 bundled 字体则跳过下载。
```

Feature messages connect the capability to the real constraint and describe fallback behavior.

### Complex Multi-Cause Work

```text
perf: 优化管理接口高负载查询性能

- 为仪表盘、群组列表、玩家搜索和热力图接口增加 TTL 缓存与同键请求合并，避免并发请求重复执行昂贵统计
- 将统计构建逻辑提取为独立纯函数，并通过线程隔离执行 CPU 密集型聚合，防止阻塞事件循环
- 优化日期比较与玩家名称索引，减少大规模记录扫描和重复查找开销
- 增加缓存复用、失效、并发合并及统计响应兼容性测试，保障性能优化不改变接口行为

这些调整用于解决管理页面在大数据量和高并发下延迟升高、请求排队的问题，同时保留现有响应结构和业务语义。
```

Bullets are appropriate when several mechanisms are equally important; they are not the default format.

## Outliers Excluded From Rules

- Four Git-generated merge messages describe branch topology rather than personal prose.
- `8db3434` is an English title-only one-line fix; the rest of the corpus overwhelmingly prefers Chinese.
- `d4d6219` and `64ffd3e` use the redundant transitional form `fix: 修复(范围): ...`; later commits consistently use `fix(scope): ...`.
- `0ab64f5` and `f5bbca3` contain concatenated historical text in their bodies. They were read but not treated as intentional formatting examples.

Use the repeated majority pattern, not these historical anomalies, when generating new messages.
