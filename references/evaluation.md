# 开源项目评估（2026-10-13）

## 获取方式与限制

- 本机 `github.com` 的 git 协议不可达，无法 `git clone`。已通过 GitHub API 下载各仓库默认分支的源码归档，解压到 `references/repos/<项目名>/`。
- 归档**不含 `.git` 历史**。各仓库对应的 commit 如下，可据此追溯：

| 目录 | 仓库 | commit |
|---|---|---|
| chatwoot | chatwoot/chatwoot | 5974756fe74d |
| tghost | Biryukovlex/tghost | 6666a66390a0 |
| teleadmin | Dionid/teleadmin | f669f2130f60 |
| BotAlto | ProKaiiddo/BotAlto | da621fb61cd2 |
| ChatbotX | ChatbotXIO/ChatbotX | 93050031ac41 |
| telegram-analytics | yevhens-hue/telegram-analytics | 74d0765b3061 |
| channel-radar | Vladislav200603/channel-radar | 36d257849933 |
| samurai | Priler/samurai | 2c8638bbbac9 |
| susmessagebot | 0mgABear/susmessagebot | 66c42e5d07e3 |

- 未运行任何项目，只做了静态阅读和模式扫描。
- 未拉取：`lenin5558dif/telegram_crm`、`ItsOrv/Telegram-Panel`、`madara896/foggygramm`、`feel90d/chatwoot-messenger-gateway`。这四个是多个人号的群发、私信或群控工具，与"n 台电脑 × m 个账号"的引流设计直接相关，按前面约定不做评估。

## 总表

| 项目 | Stars | 许可证 | 最近推送 | 语言 | 有效代码行 | 结论 |
|---|---|---|---|---|---|---|
| Chatwoot | 37,734 | MIT（`enterprise/` 另有商业许可） | 2026-10-12 | Ruby | 很大 | **采用**，作总控收件箱 |
| TGhost | 1 | Apache-2.0 | 2026-10-11 | Python + 前端 | ~50K | **采用**，作内容中台 |
| ChatbotX | 897 | MIT（`apps/builder/src/enterprise` 商业许可） | 2026-10-12 | TypeScript | 很大 | 备选，二选一，偏营销自动化 |
| susmessagebot | 0 | MIT | 2026-10-12 | Python | ~1.7K | **采用**，作自有群守护 |
| samurai | 304 | **无许可证** | 2026-10-08 | Python | 未统计 | 暂不用，仅参考 |
| channel-radar | 0 | **无许可证** | 2026-09-22 | Python | ~3.9K | 仅参考思路 |
| telegram-analytics | 0 | **无许可证** | 2026-10-10 | Python + TS | ~8.6K | **不建议** |
| teleadmin | 4 | **无许可证** | 2022-08-07 | TypeScript | ~5K | 不建议，已停更 |
| BotAlto | 18 | **无许可证** | 2025-08-15 | JS | ~300 | **不要部署** |

## 逐个评估

### 1. Chatwoot：采用（总控台）
- 原生 Telegram bot 接入成熟（`app/models/channel/telegram.rb`、`app/services/telegram/*`），支持坐席分配、标签、私有备注、自动化规则。
- 许可：根目录 MIT，`enterprise/` 目录受单独商业许可约束。**只用 CE 功能，不要碰 enterprise 目录。**
- 风险：Rails 全家桶，部署和运维成本高；它管的是 Bot API 收件箱，不是多账号矩阵，这一点正好符合合规边界。
- 对应需求：多员工总控、统一收件箱。

### 2. TGhost：采用（内容中台 + 频道数据）
- 功能与我们的设计高度重合：频道数据看板、AI 写稿（可按自己的风格）、发布日历。README 明确"发布必须人工确认"，并支持"已接受的定时消息即使 TGhost 离线也能发出"。
- 许可 Apache-2.0，可商用、可改。
- 局限：**单机单用户（single-owner）**，不支持多用户账号和权限，所以"多员工总控"需要自己加一层，或只用它做内容侧。Star 只有 1，要自己做代码审计。
- 对应需求：多方向内容规划、发布、数据回收。

### 3. ChatbotX：备选（营销自动化）
- AhaChat 出品，897 star，TypeScript，支持可视化流程、AI 代理、人工接管、多渠道（含 Telegram）。
- 与 Chatwoot 功能有重叠。**二选一即可**：要"收件箱 + 员工协作"选 Chatwoot；要"可视化营销流程"选 ChatbotX。
- 注意 `apps/builder/src/enterprise` 是商业许可。

### 4. susmessagebot：采用（自有群守护）
- MIT，专门做语义级反诈骗检测，比关键词规则强。有 Ollama（自托管）和 Groq（免费 API）两个分支。
- 代码量小（约 1.7K 行），容易审计和二次开发。0 star，需要实测。
- 对应需求：保护自有群和流量池，不被广告或诈骗账号挖走。

### 5. samurai：暂不用
- 功能强（ML 反垃圾、NSFW 检测、多语言脏话过滤），维护活跃。
- **没有许可证文件**。默认是"保留所有权利"，不能合法复制、修改或分发代码，只能以"看作者是否同意"为前提使用。
- 安全点：`libs/gender_extractor/extractor.py:29` 用 `pickle.loads` 读取包内数据文件。只读随包文件风险不大，但不应接受外部传入的 pickle。
- 建议：先联系作者要许可，或直接用 susmessagebot 替代。

### 6. channel-radar：仅参考思路
- 无许可证。做的是公开频道的指标快照（订阅、浏览、转发、反应）和 AI 周报，用匿名的 `t.me/s/` 页面抓取，不需要账号。
- 对我们的价值：可借鉴"同行频道监测"的思路，帮助选方向和找互推对象。直接用代码需要作者许可。

### 7. telegram-analytics：不建议
- 无许可证。用 Telethon/Pyrogram 以 **MTProto 个人账号**抓取"公开和私有频道"，并有"Session Pool & Rate Limiter"（多会话轮换）。
- 抓取非自己管理的私有频道，违反 Telegram 条款，也涉及隐私。多会话轮换的模式，正是我们前面明确排除的账号矩阵。
- 代码里还有 `db.ts:23` 用 `eval("require('better-sqlite3')")` 加载模块，属于不必要的动态执行。

### 8. teleadmin：不建议
- 无许可证，最后推送 2022-08-07，已停更；4 star。
- 功能是"添加信源（频道/群）→ 进退群分析 → 信源用户 CRM"，只能给流量池提供设计参考。依赖的 FDD 框架也很小众，维护成本高。

### 9. BotAlto：不要部署，存在严重安全问题
已在源码中核实：
- `Backend/server.js:9-11` 只有 `express.json()`、`cors({ origin: '*' })`、静态文件，**没有任何鉴权中间件**。
- `/createBot`、`/addCommand`、`/setToken`、`/startBot` 等路由全部开放。
- `server.js:92` 执行 `new Function('ctx', botCommands.commands[cmd])(ctx)`，即**把用户保存的命令当 JavaScript 直接执行**。
- 任何能访问该服务的人，都可以写入代码并在服务器上执行，等同于远程代码执行。
- 另外无许可证，最后更新 2025-08，只有约 300 行代码。
- 结论：不要部署，也不要以它为基础。

## 漏掉的覆盖面

按我们的设计，各模块目前的覆盖情况：

| 模块 | 覆盖 |
|---|---|
| 多员工总控收件箱 | Chatwoot ✅ |
| 内容规划、发布、频道数据 | TGhost ✅（单用户，需补权限层） |
| 自有群守护 | susmessagebot ✅ |
| 营销自动化流程 | ChatbotX（与 Chatwoot 二选一） |
| 流量池 / CRM（来源归因、跨群去重、分层） | ❌ 无合适开源项目，需要自研 |
| 多账号群控 | 不做（超出边界） |

**流量池是唯一需要自研的核心模块，也是业务价值所在。**

## 建议的下一步

1. 用 Docker 在隔离环境中跑 TGhost 和 Chatwoot，只用官方 bot token 做测试，不接任何个人号。
2. 审计 susmessagebot 的代码和 prompt，确认没有外发数据的逻辑，再决定是否上自有群。
3. 联系 samurai 作者确认许可，否则不采用。
4. 流量池 CRM 自研，数据模型以 Chatwoot 的联系人与标签为基础扩展。
