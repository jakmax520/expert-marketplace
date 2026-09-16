---
name: doc-classify
description: "自动按内容分类创建文件夹、移动文件，支持标签管理与按标签检索。 当用户要求「分类整理」、「自动归类」、「打标签」、「按标签查找」、「文件归档」时使用。 若仅需搜索定位文件，请使用 doc-search 技能。
"
homepage: 
version: 1.0.0
metadata: {"openclaw":{"category":"kdocs","emoji":"📂"}}
---

# 文档分类整理

文档分类整理技能支持智能分类归档和标签化管理。

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

#### 智能分类整理

```
步骤 1: 定位目标目录
        - 指定文件夹 → search_files(keyword="文件夹名", file_type="folder", type="file_name")
        - 根目录 → search_files(file_type="folder", type="all", scope="personal_drive", page_size=1) 获取 drive_id

步骤 2: list_files(drive_id, parent_id, page_size=500)
        → 收集所有文件（有 next_page_token 时翻页继续）
        → 需要递归扫描子目录时，对 type="folder" 的项再次调用 list_files

步骤 3: read_file_content(format="markdown") 批量读取文档内容

步骤 4: AI 按用户指定维度分类（按内容/类型/部门/项目等）
        → 生成分类方案并向用户确认

步骤 5: create_file(name="分类文件夹名", file_type="folder") 创建分类目录
        move_file(file_ids=[...], dst_parent_id=分类文件夹ID)
        → ⚠️ 批量移动前需向用户确认
```

#### 标签列表、打标与按标检索

`list_labels`（或已知系统标签 ID）→ `search_files` / `list_files` 收集 `file_id` → `batch_add_label_objects`；查看某标签下文件：`get_label_objects(label_id, object_type="file")`；需确认标签定义时 `get_label_meta`。


> 场景：自定义分类标签、批量给文档打星标/项目标签，或列出「星标」「待办」等系统标签下的文件

#### 标签归类与检索

`list_labels` → `create_label`（如需新标签）→ `batch_add_label_objects`；按标签浏览 → `get_label_objects`


> 场景：自定义标签整理文件。系统标签 ID（星标、待办等）见 `references/drive.md` 中 `get_label_meta` / `get_label_objects` 说明。

---
## 风险控制

以下工具不可逆，调用前必须向用户确认（详细约束见各工具参考文档的「操作约束」区）：

`cancel_share`

---

## 工具组合速查

| 用户需求 | 推荐工具组合 |
|----------|-------------|
| 列出目录，按内容、类型、部门等维度分类创建文件夹并归档。⚠️ `move_file` 前需向用户确认分类方案 | `search_files` → `list_files`（递归/分页）→ `read_file_content`（批量）→ AI 分类 → `create_file(folder)` → `move_file` |
| 自定义分类标签、批量给文档打星标/项目标签，或列出「星标」「待办」等系统标签下的文件 | `list_labels`（或已知系统标签 ID）→ `search_files` / `list_files` 收集 `file_id` → `batch_add_label_objects`；查看某标签下文件：`get_label_objects(label_id, object_type="file")`；需确认标签定义时 `get_label_meta`。 |
| 自定义标签整理文件。系统标签 ID（星标、待办等）见 `references/drive.md` 中 `get_label_meta` / `get_label_objects` 说明。 | `list_labels` → `create_label`（如需新标签）→ `batch_add_label_objects`；按标签浏览 → `get_label_objects` |
