# listing-i18n Skill 开发任务

## 需求

制作 OpenClaw skill，将中文产品 Excel 翻译为 Amazon + Shopify 多语言 Listing。

## Todo

- [x] 创建 generate_template.py — 生成 .xlsx 产品信息输入模板
- [x] 创建 SKILL.md — Skill 核心定义（双平台 workflow + 翻译规则）
- [x] 创建 validate.py — 输出文件校验（支持 Amazon/Shopify 两种 sheet 格式）

## Review

### 完成的文件

1. **generate_template.py** — 运行后生成 `product_template.xlsx`，包含：
   - Products sheet：10列字段（product_id 到 images_note），带示例数据
   - Instructions sheet：每个字段的填写说明和示例

2. **SKILL.md** — 核心 skill 定义，包含：
   - 5步 workflow：确认输入 → 确认平台和市场 → 逐产品翻译 → 生成输出 → 确认
   - Amazon 输出规则：Title、5 Bullet Points、Description、Backend Keywords
   - Shopify 输出规则：Title、Description(HTML)、SEO Title、SEO Description、Tags、Product Type
   - 翻译质量要求：搜索优先、文化适配、合规用词、本地计量单位、平台风格差异
   - 输出 Excel 按 `{Platform}_{lang}` 分 sheet

3. **validate.py** — 自动识别 Amazon_*/Shopify_* sheet 并分别校验：
   - Amazon：title 字符数、bullet 字符数、keywords 字节数、description 词数
   - Shopify：seo_title 字符数、seo_description 字符数、description 非空、tags 非空
   - 区分 error（必须修复）和 warning（建议优化）
