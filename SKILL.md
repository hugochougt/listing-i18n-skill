---
name: listing-i18n
description: 将中文产品 Excel 翻译为多语言 Amazon/Shopify Listing。当用户需要把中文产品信息翻译成多语言 Listing 时使用。适用于「翻译产品」「多语言Listing」「产品本地化」「Amazon listing」「Shopify listing」「translate listing」「localize products」等场景。
version: 0.1.0
metadata:
  openclaw:
    emoji: "🌐"
    requires:
      bins: ["python3"]
---

# Listing I18n — 中文产品多语言 Listing 翻译工具

将中文产品 Excel 文件翻译为 Amazon 和 Shopify 双平台、多语言本地化的电商 Listing。

## 核心能力

- 读取中文产品 Excel/CSV 文件
- 支持 Amazon 和 Shopify 两个平台的 Listing 格式
- 为每个目标语言生成平台专属的本地化内容
- 输出按「平台_语言」分 sheet 的 Excel 文件

## Workflow

当用户提供中文产品文件并要求翻译时，按以下步骤执行：

### Step 1: 确认输入

1. 如果用户**没有产品文件**：运行 `python3 generate_template.py` 生成空白模板，告知用户填写后再来，结束流程
2. 如果用户**提供了产品文件**（Excel 或 CSV）：
   - 用 `python3` 读取文件，列出所有列名和前 3 行数据
   - 向用户确认以下字段的对应关系：

| 必填字段 | 说明 |
|---------|------|
| product_id | 产品编号 |
| brand | 品牌名 |
| product_name | 中文产品名 |
| category | 品类 |
| specs | 核心规格参数 |
| selling_points | 核心卖点（中文） |
| keywords_cn | 中文关键词 |
| package_includes | 包装清单 |

| 可选字段 | 说明 |
|---------|------|
| custom_attributes | 品类自定义属性（key:value 格式） |
| images_note | 图片描述 |

3. 如果文件中缺少必填字段，提示用户补充

### Step 2: 确认目标平台和市场

向用户确认两个选择：

**平台选择：**

| 选项 | 说明 |
|------|------|
| Amazon | 生成 Title、Bullet Points、Description、Backend Keywords |
| Shopify | 生成 Title、Description(HTML)、SEO Title、SEO Description、Tags |
| Both | 同时生成两个平台的内容 |

默认：Both

**目标市场选择：**

| 代号 | 语言 | Amazon 站点 | Shopify |
|------|------|------------|---------|
| en_US | 英语(美国) | Amazon.com | 支持 |
| en_UK | 英语(英国) | Amazon.co.uk | 支持 |
| de_DE | 德语 | Amazon.de | 支持 |
| ja_JP | 日语 | Amazon.co.jp | 支持 |
| es_ES | 西班牙语 | Amazon.es | 支持 |
| fr_FR | 法语 | Amazon.fr | 支持 |
| it_IT | 意大利语 | Amazon.it | 支持 |

用户可选择一个或多个目标市场。默认：en_US。

### Step 3: 逐产品翻译

对每个产品，为每种目标语言，按所选平台生成对应内容。

---

#### Amazon Listing 输出规则

##### Title（标题）

- 长度上限 200 字符（含空格），建议控制在 150 字符以内
- 格式：`Brand + Core Product Name + Key Feature 1 + Key Feature 2 + Specification + Use Case/Audience`
- 不要直译中文，按目标市场消费者的搜索习惯重写
- 英语每个单词首字母大写，介词和冠词除外
- 不使用促销词（Best、#1、Hot Sale、Guaranteed）

##### Bullet Points（五点描述）

- 共 5 条，每条 150-250 字符
- 每条以大写核心卖点关键词开头，如 `【NOISE CANCELLATION】` 或 `【LONG BATTERY LIFE】`
- 内容安排：
  - 第 1 条：核心差异化卖点（为什么选我而不是竞品）
  - 第 2 条：核心功能/技术参数
  - 第 3 条：使用场景/适用人群
  - 第 4 条：品质/材质/认证
  - 第 5 条：售后保障/包装清单
- 日语市场：使用です/ます体，卖点前缀用【】
- 德语市场：注意名词大写和复合词

##### Product Description（产品描述）

- 150-300 词的自然段落描述
- 不要重复 Bullet Points 的内容，补充使用体验和品牌故事
- 可使用简单 HTML 标签（`<br>`, `<b>`）
- 结尾加一句号召性用语（CTA）

##### Backend Keywords（后台搜索词）

- Amazon Search Terms 上限 249 bytes
- 全部小写，空格分隔，不用逗号
- 不重复 Title 和 Bullet Points 中已有的词
- 包含：同义词、缩写、常见拼写错误、相关使用场景词
- 不包含品牌名（自己和竞品的都不要）
- 日语：使用全角空格分隔，包含片假名和平假名变体

---

#### Shopify Listing 输出规则

##### Title（标题）

- 简洁明了，格式：`Brand + Product Name + Key Differentiator`
- 控制在 100 字符以内
- 注重可读性，不必像 Amazon 那样堆关键词

##### Description（产品描述 HTML）

- 使用 HTML 格式，允许的标签：`<h3>`, `<p>`, `<ul>`, `<li>`, `<table>`, `<tr>`, `<td>`, `<b>`, `<br>`
- 不使用内联样式或 class
- 推荐结构：
  1. `<h3>` 一句话核心卖点
  2. `<ul><li>` 列出 4-6 个卖点
  3. `<p>` 详细描述段落（使用体验、品牌故事）
  4. `<table>` 规格参数表
  5. `<p>` 包装清单
- 总词数 100-400 词

##### SEO Title（SEO 标题）

- 最长 70 字符
- 关键词前置，包含品牌名
- 格式：`Primary Keyword + Brand + Secondary Keyword`

##### SEO Description（SEO 描述）

- 最长 320 字符
- 简洁的产品概述 + 核心卖点 + CTA
- 包含 1-2 个目标关键词

##### Tags（标签）

- 逗号分隔，8-15 个标签
- 混合使用：品类词、功能词、使用场景词、长尾词
- 全部小写

##### Product Type（产品类型）

- 英文品类路径，用 > 分隔
- 例如：`Electronics > Headphones > Wireless Headphones`

---

### Step 4: 生成输出文件

使用 `python3` 和 `openpyxl` 生成 Excel 文件。

**输出文件名：** `products_listing_i18n_YYYYMMDD.xlsx`

**Sheet 命名规则：** `{Platform}_{lang_code}`

例如：`Amazon_en_US`, `Amazon_ja_JP`, `Shopify_en_US`, `Shopify_de_DE`

**Amazon sheet 列结构：**

```
A: product_id
B: brand
C: title
D: bullet_point_1
E: bullet_point_2
F: bullet_point_3
G: bullet_point_4
H: bullet_point_5
I: description
J: backend_keywords
K: custom_attributes（翻译后的自定义属性）
L: source_product_name（中文原名，方便对照）
```

**Shopify sheet 列结构：**

```
A: product_id
B: brand
C: title
D: description_html
E: seo_title
F: seo_description
G: tags
H: product_type
I: custom_attributes（翻译后的自定义属性）
J: source_product_name（中文原名，方便对照）
```

另外生成一个 `Source_CN` sheet，复制原始中文数据作为对照参考。

**生成代码示例：**

```python
# pip install openpyxl (如未安装)
import openpyxl

wb = openpyxl.Workbook()
# 为每个 平台+语言 组合创建 sheet
# 写入表头和翻译后的数据
# 最后创建 Source_CN sheet 存放原文
wb.save("products_listing_i18n_20260330.xlsx")
```

### Step 5: 输出确认

翻译完成后：

1. 运行 `python3 validate.py <output_file>` 检查限制
2. 向用户报告：
   - 共处理了多少个产品
   - 生成了哪些平台和语言版本
   - 输出文件路径
   - 验证结果（是否有超标项）
3. 提醒用户检查：
   - Amazon 标题是否超过 200 字符
   - Amazon 后台关键词是否超过 249 bytes
   - Shopify SEO Title 是否超过 70 字符
   - 品牌名拼写是否正确
   - 日语/德语建议母语人士做最终审核

## 翻译质量要求

**不是直译，是本地化。** 遵循以下原则：

1. **搜索优先**：标题和关键词使用目标市场消费者实际搜索的词汇。例如中文「降噪耳机」，英语用「Noise Cancelling Headphones」而不是「Noise Reduction Headphones」。
2. **文化适配**：不同市场强调不同卖点。美国消费者关注 value for money 和 warranty；日本消费者关注品质细节和包装；德国消费者关注技术参数和认证。
3. **合规用词**：避免目标市场禁止的营销词汇。Amazon 禁止在标题中使用「best」「#1」「guaranteed」等主观宣传词。
4. **本地计量单位**：美国用 inch/oz/°F，英国用 mm/g/°C，日本用 cm/g/°C。
5. **平台风格差异**：Amazon listing 偏搜索导向，关键词密集；Shopify listing 偏品牌叙事，注重阅读体验和视觉排版。

## Commands

| 命令 | 说明 |
|------|------|
| `listing-i18n template` | 生成空白产品信息 Excel 模板 |
| `listing-i18n translate <file>` | 翻译产品文件（默认双平台 + en_US） |
| `listing-i18n translate <file> --platform amazon --lang en_US,ja_JP` | 指定平台和语言 |
| `listing-i18n preview <file> --product 1` | 预览第 1 个产品的翻译结果（不生成文件） |
| `listing-i18n validate <output_file>` | 检查输出文件的字符数/字节数限制 |

## Notes

- 需要 python3 和 openpyxl 库（脚本会自动安装）
- 翻译基于 LLM 能力，日语和德语建议让母语者做最终审核
- 每次翻译建议不超过 20 个产品，避免上下文过长导致质量下降
- 超过 20 个产品会自动分批处理
- custom_attributes 的 key 保持英文不变，value 翻译为目标语言

---

Feedback: 如有问题或功能建议，请在 ClawHub 评论区留言。
