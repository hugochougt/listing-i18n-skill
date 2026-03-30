# listing-i18n

OpenClaw Skill：将中文产品 Excel 翻译为 Amazon/Shopify 多语言本地化 Listing。

## 项目结构

```
SKILL.md                 # Skill 定义（workflow、翻译规则、输出格式）
generate_template.py     # 生成产品信息 Excel 输入模板
inspect_input.py         # 检查输入文件结构并输出字段映射建议
build_output.py          # 根据翻译 JSON 生成最终 Excel
validate.py              # 校验翻译输出的字符/字节限制
product_template.xlsx    # 预生成的模板文件
```

## 关键约定

- 这是 OpenClaw skill，不是独立翻译引擎
- 翻译由 OpenClaw 的 LLM 完成，脚本只做确定性辅助工作
- Python 3 + openpyxl，依赖缺失时直接报错，不再尝试静默安装
- SKILL.md 中的 `version` 字段是唯一版本源，修改版本时同步更新
- 输出 Excel sheet 命名规则：`{Platform}_{lang_code}`（如 `Amazon_en_US`、`Shopify_ja_JP`）
- `Source_CN` sheet 保留中文原文对照
- validate.py 自动识别 `Amazon_*` / `Shopify_*` sheet 分别校验

## 平台限制速查

| 字段 | 限制 |
|------|------|
| Amazon Title | 200 chars（建议 150） |
| Amazon Bullet Point | 500 chars（建议 250） |
| Amazon Backend Keywords | 249 bytes |
| Amazon Description | 50-500 words |
| Shopify SEO Title | 70 chars |
| Shopify SEO Description | 320 chars |

## 开发注意

- 修改 SKILL.md 中的 workflow 或输出格式时，同步更新 `validate.py` 和 `build_output.py`
- generate_template.py 的字段顺序必须与 SKILL.md 中定义的列结构一致
- 如果改动影响 skill 对外行为、说明文档、脚本能力或版本号，完成后要同步发布到 GitHub 和 ClawHub

## 发布流程

发布前检查：

- 确认 `SKILL.md` 里的 `version` 已更新
- 确认需要发布的改动已提交到 git
- 如有必要，先推送到 GitHub

GitHub：

```bash
gh repo view --web
gh auth status
git push origin main
```

优先级：

- GitHub 相关操作优先使用 `gh`
- 代码推送本身仍可使用 `git push origin main`
- 如果后续涉及 PR、release、workflow、issue 等 GitHub 动作，默认先考虑 `gh`

ClawHub：

```bash
npx clawhub@latest whoami
npx clawhub@latest publish . --slug listing-i18n --name "Listing I18n" --version <SKILL.md中的version> --changelog "<简短变更说明>" --tags latest
```

注意：

- 当前可用的 ClawHub CLI 发布命令是顶层 `publish`
- 不要使用过时的 `clawhub skill publish`
