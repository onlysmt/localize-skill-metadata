# Chinese openai.yaml guidelines

## Purpose

Use this reference when creating or reviewing `agents/openai.yaml` for a skill that should be easier to browse from the `/` invocation panel in Chinese.

## Field checklist

- `display_name`: Keep it recognizable and close to the original skill identity.
- `short_description`: Use Chinese, keep it between 25 and 64 characters, and optimize for fast scanning.
- `default_prompt`: Write a short Chinese example and include the exact `$skill-name`.

## Heuristics

- Prefer preserving established skill names like `docx`, `pdf`, or `frontend-design` in `display_name`.
- Translate the user value, not the implementation details.
- Mention the main artifact or workflow when the skill is specialized, such as `Word 文档`, `前端界面`, or `MCP 服务`.
- If the skill supports many tasks, summarize the common user goal instead of listing every feature.

## Good examples

```yaml
interface:
  display_name: "docx"
  short_description: "用于创建、编辑与整理 Word 文档内容"
  default_prompt: "使用 $docx 来生成一份结构清晰的中文会议纪要。"
```

```yaml
interface:
  display_name: "frontend-design"
  short_description: "用于设计并实现风格鲜明的高质量前端页面与组件"
  default_prompt: "使用 $frontend-design 来设计一个中文产品落地页。"
```

## Review checklist

- The copy matches the actual `SKILL.md` scope.
- The Chinese phrasing reads naturally in a compact UI panel.
- The `default_prompt` names the exact skill handle.
- No unsupported capability is implied.
- The file uses quoted string values.
