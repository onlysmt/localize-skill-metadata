**简单说来说，这个skill的作用是，在codex中按“/”调用skill时，将其描述翻译为中文，方便英文苦手直接阅读。**

这个 skill 用来给其他 Codex skill 生成或更新中文界面的元数据。
它主要服务于技能调用面板 / 的展示层，而不是直接扩展业务能力。
目标是让中文用户更容易看懂某个 skill 是干什么的、什么时候该调用、怎么发起调用。

核心产出：
为目标 skill 创建或刷新 agents/openai.yaml
重点生成 3 个字段：
interface.display_name
interface.short_description
interface.default_prompt

它解决的问题：
某个 skill 没有中文面板说明
现有说明太英文、太生硬、太旧
面板里的 skill 名称和用途不够直观
默认提示词不适合中文用户直接使用

适用场景：
新建了一个 skill，想补齐中文展示信息
已有 skill 的 agents/openai.yaml 缺失或内容过时
需要把 skill 的面板文案改得更适合中文团队使用
一次性审查多个 skill，找出谁缺中文元数据、谁的描述不准确

它不会做什么：
不负责实现 skill 的核心功能逻辑
不会替代 SKILL.md 本体说明文档
不应该夸大 skill 能力，不能写出实际不支持的功能描述
不是通用翻译器，它要求先理解目标 skill 的真实范围再写文案

标准工作流程：
先读取目标 skill 的 SKILL.md
理解这个 skill 真正解决什么问题、触发条件是什么
生成中文的 display_name、short_description、default_prompt
优先复用现有生成脚本，而不是自己随意拼格式
回头检查生成后的 agents/openai.yaml 是否和 skill 实际能力一致
如果改过 skill 目录，建议再做一次快速校验

它依赖/推荐复用的外部脚本：
init_skill.py
generate_openai_yaml.py
quick_validate.py

文案规则：
display_name：尽量保留原 skill 名，除非中文标题明显更清晰
short_description：必须是中文，适合面板快速扫读，长度控制在 25 到 64 字符
default_prompt：必须是中文示例提示，并且要包含精确 skill 句柄，比如 localize-skill-metadata

文案必须贴合真实能力，不能乱承诺
多 skill 审计模式下的职责：
找出没有 agents/openai.yaml 的 skill
找出中文文案过时或误导的 skill
优先先补齐缺失项，再优化已有文案
区分“已创建”和“仍需人工复核”的项

当前这个 skill 的现有元数据状态：
agents/openai.yaml 已存在
文件内容是正常 UTF-8 中文，不是文件损坏

我看到的当前文案是：
display_name: localize-skill-metadata
short_description: 为技能生成或补齐中文调用面板说明与默认提示词
default_prompt: 使用 $localize-skill-metadata 为一个 skill 生成或更新中文 agents/openai.yaml。


配套参考文件作用：
references/openai-yaml-zh-guidelines.md 是写中文面板文案的规范说明
它提供字段检查项、命名启发、好例子和最终复核清单
作用相当于这个 skill 的“文案质量标准”
