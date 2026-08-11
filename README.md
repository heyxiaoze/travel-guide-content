# travel-guide-content

Travel Guide 站点的**内容仓库（公开）**，也是攻略内容的**唯一真相源**。

- `guides/<id>.json` —— 单篇攻略完整数据（含 `status: "draft" | "published"`）
- `guides/index.json` —— 攻略清单（id / status / 标题 / emoji / color / updatedAt 等），供站点枚举

> ⚠️ **所有攻略的创建、更新、读取都必须在本仓库进行**。主仓库 `travel-guide/src/data/` **不再存放攻略内容**，
> 只保留 `registry.ts` 与构建期由 `scripts/sync-content.mjs` 自动生成的 `generated.ts` 快照（已被 gitignore，勿手改）。
> 不要在主仓库 `src/data/` 里新增或修改攻略——那里只是本仓库的只读镜像快照。

站点构建期（`predev` / `prebuild`）通过 `raw.githubusercontent.com` 拉取本仓库「已发布」攻略生成静态快照；
管理员经 Cloudflare Pages Functions + GitHub PAT 写回本仓库并触发重新部署。
每篇攻略的独立提交历史即为该攻略的**版本历史**。
