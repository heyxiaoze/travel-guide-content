# 📚 travel-guide-content

> [`travel-guide`](https://github.com/heyxiaoze/travel-guide) 站点的**内容仓库（公开）**，也是攻略内容的**唯一真相源**。

[![Format](https://img.shields.io/badge/Format-JSON-000000?logo=json&logoColor=white)](https://www.json.org)
[![Source of Truth](https://img.shields.io/badge/Role-Single%20Source%20of%20Truth-blue)](https://github.com/heyxiaoze/travel-guide)
[![License](https://img.shields.io/badge/License-CC%20BY--NC%204.0-lightgrey)](https://creativecommons.org/licenses/by-nc/4.0/)

所有攻略以 JSON 形式存放于此。主站**游客读取走运行时 Cloudflare Function**——每次请求从本仓库拉取已发布攻略（边缘缓存约 1 分钟），所以**只改本仓库、不部署主仓库，线上就会拉到最新**。同时，每篇攻略的独立 Git 提交历史，天然就是它的**版本历史**。

---

## 📂 目录结构

```
travel-guide-content/
├── guides/
│   ├── <id>.json      # 单篇攻略完整数据（含 status: "draft" | "published"）
│   └── index.json     # 攻略清单：id / status / 标题 / emoji / color / updatedAt 等
└── README.md
```

> ⚠️ **所有攻略的创建、更新、读取都必须在本仓库进行。** 主仓库 `travel-guide/src/data/` **不再存放攻略内容**，只保留 `registry.ts` 与构建期由 `scripts/sync-content.mjs` 自动生成的 `generated.ts` 快照（已被 gitignore，勿手改）。不要在主仓库 `src/data/` 里新增或修改攻略——那里只是本仓库的只读镜像快照。

## 🧱 一篇攻略的结构

`guides/<id>.json` 顶层字段（结构以主仓库 `src/types/guide.ts` 的 `Guide` 为准）：

| 字段 | 说明 |
|------|------|
| `id` | 唯一标识，同时作为 URL `#/guide/<id>` |
| `title` / `subtitle` | 标题 / 副标题 |
| `emoji` | 封面图标，用 `{{icon:name}}` 令牌（见主站 `src/lib/icons.tsx`） |
| `color` | 主题色，CSS `linear-gradient(...)` |
| `badge` | 标题旁标签（如「2026 国庆错峰版」），可用 `{{icon:name}}` |
| `facts` | 概览指标（天数 / 里程 / 花费等） |
| `meta` | 元信息（出行方式、同行、预算等） |
| `sections` | 正文区块数组（`day` / `text` / `place` / `food` / `budget` … 共 12 种） |
| `status` | `"draft"` 或 `"published"`（未发布不出现在线上） |

`guides/index.json` 是数组，每项含 `{ id, status, title, emoji, color, updatedAt, badge, modes, cities }`，供站点枚举卡片与排序。

## ✏️ 如何新增 / 更新一篇攻略

### 方式一：用 WorkBuddy 技能（推荐）

在主项目的 WorkBuddy 会话中说「**加一篇指南**」「**新增旅行笔记**」，技能会自动调研并落成 `guides/<id>.json`、登记 `index.json`（`status: "published"`）。

### 方式二：手动（在本仓库操作）

1. 新建 `guides/<你的id>.json`，字段同上，并加 `"status": "published"`。
2. 在 `guides/index.json` 数组追加一条登记信息。
3. 提交并推送到 `main`。站点经 `/guides` Function 在约 1 分钟内自动拉取，**无需重新部署主仓库**。
4. （可选）本地用主仓库 `npm run build` 对 JSON 做类型 / 结构校验。

## 🔄 与站点的关系

- **构建期**（`predev` / `prebuild`）：主站通过 `raw.githubusercontent.com` 拉取本仓库「已发布」攻略生成静态快照 `generated.ts`（离线兜底）。
- **运行时**：游客经 Cloudflare Pages Functions + GitHub PAT 直接读取本仓库，变更秒级（边缘缓存约 1 分钟）生效。
- **管理员写回**：登录后通过主站 `/api/guide/save` 把编辑结果写回本仓库并触发同步。

## 📄 许可

个人旅行记录，转载请注明出处。
