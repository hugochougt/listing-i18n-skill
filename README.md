# listing-i18n

将中文产品 Excel 翻译为 Amazon + Shopify 多语言本地化 Listing 的 [OpenClaw](https://openclaw.ai) Skill。

## 功能

- 读取中文产品 Excel 文件，自动识别字段映射
- 支持 Amazon 和 Shopify 双平台 Listing 格式
- 支持 7 个目标市场：en_US / en_UK / de_DE / ja_JP / es_ES / fr_FR / it_IT
- 本地化翻译（非直译），包含 SEO 关键词优化、文化适配、合规用词
- 输出按「平台_语言」分 sheet 的 Excel 文件
- 自动校验字符数 / 字节数限制

## 平台输出格式

| Amazon | Shopify |
|--------|---------|
| Title (200 chars) | Title (100 chars) |
| 5 Bullet Points (150-250 chars) | Description HTML |
| Description (150-300 words) | SEO Title (70 chars) |
| Backend Keywords (249 bytes) | SEO Description (320 chars) |
| | Tags, Product Type |

## 安装

```bash
npx clawhub install listing-i18n
```

或手动将 `SKILL.md`、`generate_template.py`、`validate.py` 放入 OpenClaw skill 目录。

## 使用

### 1. 生成产品模板

```bash
python3 generate_template.py
```

生成 `product_template.xlsx`，包含 Products 和 Instructions 两个 sheet。

### 2. 翻译产品

在 OpenClaw 中使用以下命令：

```
listing-i18n translate <your_products.xlsx>
listing-i18n translate <file> --platform amazon --lang en_US,ja_JP
listing-i18n preview <file> --product 1
```

### 3. 校验输出

```bash
python3 validate.py <output_file.xlsx>
```

自动识别 `Amazon_*` / `Shopify_*` sheet 并分别校验，输出 error（必须修复）和 warning（建议优化）。

## 模板字段

| 字段 | 说明 | 必填 |
|------|------|------|
| product_id | 产品编号 | Yes |
| brand | 品牌名 | Yes |
| product_name | 中文产品名 | Yes |
| category | 品类 | Yes |
| specs | 核心规格（分号分隔） | Yes |
| selling_points | 核心卖点（分号分隔） | Yes |
| keywords_cn | 中文关键词（逗号分隔） | Yes |
| package_includes | 包装清单 | Yes |
| custom_attributes | 自定义属性（key:value 格式） | No |
| images_note | 图片描述 | No |

## 依赖

- Python 3
- openpyxl（脚本自动安装）

## License

MIT
