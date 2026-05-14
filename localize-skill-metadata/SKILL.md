---
name: localize-skill-metadata
description: Generate or refresh Chinese UI metadata for Codex skills by creating or updating `agents/openai.yaml` with localized `interface.display_name`, `short_description`, and `default_prompt`. Use when Codex creates a new skill or updates an existing skill and the panel-facing metadata is missing, stale, or not suitable for Chinese users.
---

# Localize Skill Metadata

## Overview

Create or refresh Chinese `agents/openai.yaml` metadata for a skill so the `/` invocation panel is easier to understand in a Chinese-first workflow.

Prefer reusing the existing `skill-creator` scripts rather than inventing a parallel format or generator.

## Workflow

### 1. Read the source skill first

Read the target skill's `SKILL.md` frontmatter and body before writing UI metadata.

Infer:
- What the skill actually helps with
- Which user requests should trigger it
- Whether the current panel copy is missing, stale, or too English-centric

Do not translate blindly from the first paragraph if the rest of the skill narrows or expands the scope.

### 2. Generate the three required interface fields

Always provide:
- `interface.display_name`
- `interface.short_description`
- `interface.default_prompt`

Write them with these rules:
- Keep `display_name` recognizable. Prefer the original skill name unless a clearer Chinese-facing title is obviously better.
- Keep `short_description` in Chinese and optimized for quick scanning in the invocation panel.
- Keep `short_description` between 25 and 64 characters.
- Make `default_prompt` a short example prompt in Chinese.
- Always include the exact skill handle in `default_prompt`, for example `使用 $skill-name ...`
- Keep wording aligned with the real scope of the skill. Do not promise unsupported capabilities.

### 3. Reuse the existing generator

Prefer the existing scripts from `C:/Users/11753/.codex/skills/.system/skill-creator/scripts/`:
- `init_skill.py` when creating a brand-new skill
- `generate_openai_yaml.py` when creating or refreshing `agents/openai.yaml`
- `quick_validate.py` when validating the finished skill folder

Pass explicit interface overrides so the final file is deterministic.

Typical flow:
1. Read the target `SKILL.md`
2. Draft Chinese `display_name`, `short_description`, and `default_prompt`
3. Run `generate_openai_yaml.py <skill-dir> --interface key=value`
4. Re-open `agents/openai.yaml` and verify it matches the skill
5. Run `quick_validate.py <skill-dir>` if the skill folder was edited

### 4. Audit mode for multiple skills

When the request covers more than one skill:
- Identify skills that have no `agents/openai.yaml`
- Flag skills whose Chinese panel copy is stale or misleading
- Prioritize breadth first: fill obvious gaps before refining copy
- Summarize what was created versus what still needs review

If a skill already has good Chinese metadata, preserve it unless the user asked for a rewrite.

## Writing Guidance

Use imperative wording.

Prefer concise Chinese phrasing such as:
- `用于...`
- `帮助...`
- `为...生成...`

Avoid:
- Literal machine translation that keeps awkward English structure
- Overly broad promises like `处理所有技能问题`
- Descriptions that mention implementation details instead of user value
- `default_prompt` text that omits the `$skill-name`

## Output Pattern

Use this structure:

```yaml
interface:
  display_name: "skill-name-or-title"
  short_description: "25到64个字符的中文面板简介"
  default_prompt: "使用 $skill-name 来完成某个与该技能相关的任务。"
```

## Reference

Read [references/openai-yaml-zh-guidelines.md](references/openai-yaml-zh-guidelines.md) when you need examples, naming heuristics, or a review checklist before finalizing the metadata.
