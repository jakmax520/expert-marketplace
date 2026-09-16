---
name: doc-writer
description: "搜索多份云文档，提取信息并自动生成总结报告或新文档内容。可浏览目录定位目标文档。 当用户要求「汇总报告」、「多文档总结」、「定期播报」、「内容整合」时使用。 若只需读取单篇文档，请使用 doc-to-markdown 技能。
"
homepage: 
version: 1.0.0
metadata: {"openclaw":{"category":"kdocs","emoji":"✍"}}
---

# 文档总结与内容生成

文档总结与内容生成技能支持多文档信息提取和智能写作。

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

### 详细参考

| 文档类型 | 参考文件 | 说明 |
|----------|----------|------|
| 智能文档（otl） | `references/otl_references.md` | 页面、文本、标题、待办等元素操作 |

---

## 操作指南

### 执行指南

| 操作类型 | 指南文件 | 何时阅读 |
|----------|----------|----------|
| 获取文件标识指南 | `references/file-locating-guide.md` | 需要搜索或浏览文件时 |
| 文件读取指南 | `references/file-reading-guide.md` | 需要获取文档内容时 |
| 文件创建与写入指南 | `references/file-writing-guide.md` | 需要创建或编辑文档时 |

### 高频流程指引

#### 搜索-读取-汇报撰写

`search_files` → `read_file_content`（多次）→ AI 分析 → `create_file` → `upload_file` → `get_file_link`


> 场景：搜索多份文档、提取信息、汇总撰写新报告

#### 定期读取与播报

`search_files` → `read_file_content` → AI 摘要 → `get_file_link`


> 场景：定期读取指定文档，提取关键信息生成摘要

---
## 风险控制

以下工具不可逆，调用前必须向用户确认（详细约束见各工具参考文档的「操作约束」区）：

`otl.block_delete`、`cancel_share`

---

## 工具组合速查

| 用户需求 | 推荐工具组合 |
|----------|-------------|
| 搜索多份文档、提取信息、汇总撰写新报告 | `search_files` → `read_file_content`（多次）→ AI 分析 → `create_file` → `upload_file` → `get_file_link` |
| 定期读取指定文档，提取关键信息生成摘要 | `search_files` → `read_file_content` → AI 摘要 → `get_file_link` |
