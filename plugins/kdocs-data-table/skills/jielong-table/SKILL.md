---
name: jielong-table
description: "自动识别接龙文本内容，提取结构化数据并生成在线表格（.ksheet）。 当用户粘贴接龙文本或提到「接龙转表格」、「整理接龙」、「接龙统计」、「文字转表格」时使用。
"
homepage: 
version: 1.0.0
metadata: {"openclaw":{"category":"kdocs","emoji":"📊"}}
---

# 接龙转表格

接龙转表格技能可以将群聊接龙、文字信息自动转换为结构化表格。

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

#### 工作表管理
| 工具 | 用途 |
|------|------|
| `sheet.create_airsheet_file` | 新建智能表格文件 |

#### 数据操作
| 工具 | 用途 |
|------|------|
| `sheet.update_range_data` | 批量更新选区数据 |

### 详细参考

| 文档类型 | 参考文件 | 说明 |
|----------|----------|------|
| 表格文档/智能表格（xlsx & ksheet） | `references/sheet_references.md` | 工作表管理、范围数据获取、批量更新 |

---

## 操作指南

### 执行指南

| 操作类型 | 指南文件 | 何时阅读 |
|----------|----------|----------|
| 获取文件标识指南 | `references/file-locating-guide.md` | 需要搜索或浏览文件时 |

### 高频流程指引

#### 接龙转表格

**步骤 1**：识别接龙场景 → 根据场景信息和接龙信息，推断表格名称(`sheetName`)和表头(`headerList`)字段

**步骤 2**：通过 `sheet.create_airsheet_file` 创建智能表格(.ksheet)，表名为 `sheetName`，通过 `sheet.update_range_data` 写入表头数据

**步骤 3**：按照接龙顺序和表头字段，依次提取接龙信息(`infoList`)，通过 `sheet.update_range_data` 写入数据

**步骤 4（可选 - 汇总统计）**：若用户要求按品类/分类汇总数量，在数据区域下方通过 `sheet.update_range_data(op_type=cell_operation_type_formula)` 写入汇总公式（如 `=SUMIF(品类列, "苹果", 数量列)`），生成分类汇总行

**步骤 5**：创建成功，回复"已将接龙转为表格"，输出表格统计信息和新表格链接

---
## 风险控制

以下工具不可逆，调用前必须向用户确认（详细约束见各工具参考文档的「操作约束」区）：

`sheet.delete_sheets`、`sheet.delete_range`、`cancel_share`、`sheet.delete_protection_ranges`、`sheet.delete_data_validations`、`sheet.delete_conditional_format_rules`、`sheet.delete_float_images`、`sheet.delete_filters`

---

## 工具组合速查

| 用户需求 | 推荐工具组合 |
|----------|-------------|
| 用户粘贴接龙内容或意图将文字转表格 | `sheet.create_airsheet_file` → `sheet.update_range_data`（表头）→ `sheet.update_range_data`（数据）→ `get_file_link` |
