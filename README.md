# Telegram 群 AI 运营

Telegram 社区的 AI 内容运营与私域流量池系统。用官方 Bot API 做自有频道和群的内容规划、发布、群管、客服总控和流量沉淀。

> **合规前提：** 本项目只做"自有阵地"的自动化和合规获客。**不做**多账号冒充真人、批量进入他人社区发引流内容、规避平台检测的群控。详见 [docs/compliance-boundary.md](docs/compliance-boundary.md)。

## 目标架构

```
选题源（RSS / 行业新闻 / 公开频道 / 群内高频问题）
   │
   ▼
内容中台（TGhost）── AI 规划 → 写稿 → 人工审核 → 定时发布
   │
   ▼
自有频道 / 群 × N
   │  每群一个官方 bot：欢迎引导、FAQ 答疑、裂变链接、反诈骗守护
   │
   ├────────────────────────┬──────────────────────────┐
   ▼                        ▼                          ▼
总控收件箱（Chatwoot）   群守护（susmessagebot）    流量池 CRM（自研）
多员工、坐席分配、        语义反诈骗、反广告          来源归因、跨群去重、
AI 建议回复                                          用户分层、转化漏斗
```

## 模块与选型

| 模块 | 方案 | 状态 |
|---|---|---|
| 多员工总控收件箱 | [Chatwoot](https://github.com/chatwoot/chatwoot)（MIT，仅用 CE） | 采用 |
| 内容规划 / 发布 / 频道数据 | [TGhost](https://github.com/Biryukovlex/tghost)（Apache-2.0，单用户，需补权限层） | 采用 |
| 自有群守护 | [susmessagebot](https://github.com/0mgABear/susmessagebot)（MIT） | 采用 |
| 营销自动化流程 | [ChatbotX](https://github.com/ChatbotXIO/ChatbotX) | 备选，与 Chatwoot 二选一 |
| 流量池 / CRM | 自研 | 待开发（核心资产） |
| 多账号群控 | — | 不做（超出合规边界） |

选型依据和逐个项目的安全评估见 [references/evaluation.md](references/evaluation.md)。

## 文档

- [docs/architecture.md](docs/architecture.md) — 系统架构与数据流
- [docs/compliance-boundary.md](docs/compliance-boundary.md) — 合规边界：做什么、不做什么、为什么
- [docs/roadmap.md](docs/roadmap.md) — MVP 落地路线
- [references/evaluation.md](references/evaluation.md) — 开源项目评估报告
- [references/repos/README.md](references/repos/README.md) — 第三方源码说明（不随仓库提交）

## 目录结构

```
.
├── README.md
├── docs/
│   ├── architecture.md
│   ├── compliance-boundary.md
│   └── roadmap.md
└── references/
    ├── evaluation.md          # 开源项目评估（提交）
    └── repos/                 # 第三方源码快照（.gitignore，不提交）
```

## 下一步

1. Docker 隔离环境试跑 TGhost 和 Chatwoot，只接官方 bot token。
2. 审计 susmessagebot 的外发逻辑后决定是否上自有群。
3. 自研流量池 CRM，数据模型基于 Chatwoot 的联系人与标签扩展。
