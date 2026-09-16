---
name: web-clipper
description: "将网页内容剪藏并自动保存为金山文档智能文档（.otl）。 当用户提供 URL 并要求「保存网页」、「收藏网页」、「剪藏」、「网页存到文档」时使用。 若需要存入知识库，请使用 knowledge-save 技能。
"
homepage: 
version: 1.0.0
metadata: {"openclaw":{"category":"kdocs","emoji":"🌐"}}
---

# 网页收藏

网页收藏技能可以将任意网页内容保存为金山文档。

> 本技能依赖 `kdocs` 技能的基础文档操作能力（认证、文件管理等），请确保已安装该技能。详见 `references/core/` 目录。

---

## 能力范围

### 通用工具总览

#### 文档创建与上传
| 工具 | 用途 |
|------|------|
| `create_file` | 在云盘下新建文件或文件夹 |
| `scrape_url` | 网页剪藏，抓取网页内容并自动保存为智能文档 |
| `scrape_progress` | 查询网页剪藏任务进度 |
| `upload_file` | 全量上传写入文件（更新已有 docx/pdf 或新建并上传本地文件） |

#### 文档读取与下载
| 工具 | 用途 |
|------|------|
| `list_files` | 获取指定文件夹下的子文件列表 |
| `download_file` | 获取文件下载信息 |
| `read_file_content` | 文档内容抽取为 Markdown/纯文本 |

#### 文件组织
| 工具 | 用途 |
|------|------|
| `move_file` | 批量移动文件(夹) |
| `rename_file` | 重命名文件（夹） |

#### 分享与访问
| 工具 | 用途 |
|------|------|
| `share_file` | 开启文件分享 |
| `set_share_permission` | 修改分享链接属性 |
| `cancel_share` | 取消文件分享 |
| `get_share_info` | 获取分享链接信息 |
| `get_file_link` | 获取文件的云文档在线访问链接 |

#### 搜索
| 工具 | 用途 |
|------|------|
| `search_files` | 文件（夹）搜索 |


---

## 操作指南

### 执行指南

| 操作类型 | 指南文件 | 何时阅读 |
|----------|----------|----------|
| 获取文件标识指南 | `references/file-locating-guide.md` | 需要搜索或浏览文件时 |

### 高频流程指引

#### 网页剪藏

> 🎯 **当用户要求保存网页/URL 到金山文档时，直接调用 `scrape_url`。禁止先用 `web_fetch`、`web_search` 或浏览器抓取内容。**

**触发识别**：用户消息中同时包含 **URL**（非金山文档链接）+ **保存/存到/收藏/剪藏** 等意图词时，走此流程。

```
步骤 1: scrape_url(url="https://example.com")
        → 返回 job_id

步骤 2: scrape_progress(job_id=xxx)
        → 轮询（每 2-5 秒），status 判定：
          1  = 完成 → 获得 scrape_file_id（剪藏专用标识）
          -1 = 失败 → 检查 URL 或重试
          其他 = 进行中 → 继续轮询

步骤 3: get_file_link(file_id=scrape_file_id)
        → 返回文档在线链接
```

---
## 风险控制

以下工具不可逆，调用前必须向用户确认（详细约束见各工具参考文档的「操作约束」区）：

`cancel_share`

---

## 工具组合速查

| 用户需求 | 推荐工具组合 |
|----------|-------------|
| 将网页内容保存为金山文档 | `scrape_url` → `scrape_progress`（轮询至完成）→ `get_file_link` |
