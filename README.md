# expert-marketplace

专家市场拉取专家数据与技能文件。

## 这是什么

| 目录 / 文件 | 内容 |
|---|---|
| `expert_center.json` | 专家元数据（427 条 / 15 分类） |
| `featuredScenes.json` | 精选场景（10 个） |
| `avatars/` | 专家头像（PNG） |
| `scene-images/` | 场景配图（PNG） |
| `plugins/<plugin>/` | 每位专家一个目录：`.codebuddy-plugin/plugin.json`、`agents/*.md`、`skills/**` |
| `MANIFEST.json` | 本次同步的计数、来源与失败清单 |

目录结构刻意与源站 CDN 保持一致，消费方只需替换 base URL。

## 来源与同步

内容由 `sync-expert-mirror.ts`（在 Logexus 仓库的 `scripts/` 下）从以下两个公开源抓取：

- `https://acc-1258344699.cos.accelerate.myqcloud.com/workbuddy/expert-marketplace`
- `https://raw.githubusercontent.com/infometa/workbuddyskills/main`

同步是**一次性快照**，不是持续同步。上游 `lastUpdated` 见 `MANIFEST.json`。

## 已知缺口

- `MANIFEST.json` 的 `failures` 列出了上游两个源都取不到的文件
- 其中 18 个专家的 `.codebuddy-plugin/plugin.json` 在上游缺失（这批专家比归档仓新）
- `avatar` 字段有 15 条写成 `/plugins/<plugin>/avatars/` 而非顶层 `/avatars/`，
  这些只存在于归档仓，镜像时已按回退路径取回


各专家自带技能的许可文件随目录一并保留（`skills/**/LICENSE*`）。
