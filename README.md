# travel-guide-content

Travel Guide 站点的**内容仓库（公开）**，作为站点唯一真相源。

- `guides/<id>.json` —— 单篇攻略完整数据（含 `status: "draft" | "published"`）
- `guides/index.json` —— 攻略清单（id / status / 标题 / emoji / color / updatedAt 等），供站点枚举

站点构建期通过 `raw.githubusercontent.com` 拉取本仓库「已发布」攻略生成静态快照；
管理员经 Cloudflare Pages Functions + GitHub PAT 写回本仓库并触发重新部署。
每篇攻略的独立提交历史即为该攻略的**版本历史**。
