- 使用第一性原理思考，先确认真实目标、约束和成功标准，不要套用惯例
- Fix a bug at its cause, reasoning from first principles and established best practice. Do not patch over the symptom
- 避免不必要的支持性文本、描述性文案和冗余的 UI 微文案。
- Avoid unnecessary secondary labels and descriptions. Let the layout do the explaining.
- 删掉重复解释布局的文案，但保留必要的表单标签和控件名称。
- 遇到不熟悉的功能，不要立即从零实现。先检索 GitHub 上维护活跃、许可证明确的同类开源项目，比较实现思路、依赖和已知问题。优先借鉴经过验证的设计，不要直接复制不兼容或来源不明的代码。（别搜到个三年前就停更的库当宝贝。）
- 先搜 issue 再动手，从零实现之前先看别人怎么踩坑
- Before adding complexity for a speculative or vanishingly unlikely race or security edge case, explain the concrete failure mode, likelihood, and complexity cost to the user and get their buy-in. Do not silently expand scope for theoretical robustness.
- 在拥有边界处一次性验证未知值。传递类型化的值向内，而不是重复 `typeof value === "object"` 和属性存在性检查。
- 不要对已由 schema、构造函数或内部类型保证的值进行防御性重新验证。
- 不要过早提取单次使用的辅助函数。除非辅助函数被重用、隐藏了真正的复杂边界，或有清晰的独立名称能改善调用者，否则直接在调用点内联逻辑。
- 尽可能避免 `try`/`catch`

- 不要去猜测几乎不会发生的竞态、边界问题，额外增加复杂度。真要加防护，先说清楚会怎么出错、概率多大、增加多少代码，我同意之后再做，不许私自扩大改动范围。
外部传入的数据，只在入口校验一次。内部流转已经确定类型的值，不要再反复做防御检查。

- 不要提前抽取只用一次的函数。只有逻辑会复用、封装了复杂逻辑，或是代表一个独立概念的时候，才抽成函数。
主函数只写正常业务流程，校验、细节逻辑放到子函数里面。

- 注释只写不明显的约束、反直觉的逻辑，显而易见的代码不用写注释。

- AI 自动生成的文件，不要手动修改，改源头，重新生成。
测试代码测真实实现，不要直接复制业务逻辑进测试，尽量少用 mock。

- 跟着仓库现有的流程规范走，不要私自引入仓库没有在用的工具和流程。

## Writing artifacts

An artifact is anything you produce for people other than the person you're working with: an app's interface and its text, user docs, prompts, skills, config. Its reader wasn't in your conversation, so write only what that reader needs.

- Write every artifact for its own reader, not for the person who asked for it. Leave out:
  - decisions and the reasons for them;
  - conversation history, and options that were rejected or removed;
  - implementation details the reader doesn't need;
  - evidence that a requirement was met.

  For example, no "data is preserved when you delete an item" note because that was a design goal, and no counter added only to show that a feature works.
- When something is removed, remove every trace of it, including any instruction that forbids it. For example, after dropping an export format, no "don't use the old export format" line remains in a prompt or doc.
- In prompts, skills and other instructions, write a rule only for behavior its reader would actually show. When a rule is too broad, narrow it instead of adding an exception: "ask before renaming user-visible commands", not "ask before renaming anything" plus "except internal names".
- In an interface, every element and string must help the end user act. Don't repeat what's already shown, such as a "Healthy" label beside a green status dot, and make the interface clear enough to need no instructions or hints.
- Use the end user's words, not the names you used while building.
- When a design changes, delete whatever in the affected area no longer serves the end user, whoever wrote it.
- Show new interface elements and new user-facing text (labels, help text, notices) as a prototype, and get approval before merging them. Error messages and log lines don't need this.
- Before reporting interface work, list each visible element and string in the changed area, with one line on why the end user needs it.

## 沟通

- 面向用户的叙述默认使用简体中文；代码、命令和技术标识保持英文。
- 先给影响与结论，再给行动、待决策和必要证据；没有对应内容就省略。
- 使用简洁、连贯的段落；只有确实适合并列比较或按步骤执行时才使用列表。
- 使用具体、简单的词，避免无意义术语、套话、重复总结和未经请求的对比。
- 技术细节只保留对理解结论、判断风险或复现结果有帮助的部分。
- 对可读性、命名和组织方式的询问，先说明判断与取舍，不直接当成修改指令。

## 指令优先级

- 遵守系统、平台和安全约束。
- 用户当前明确指令优先于 Skill、历史记忆和默认偏好。
- 项目目录中的 `AGENTS.md` 只在该项目范围内补充或覆盖全局规则。

## 执行方式

- 用户表示要开始新工作或修复现有问题时，持续推进，直到用户的目标完成，在目标方向上自主推进。
- 向用户提问之前，先完成上下文里已经授权、并且能把下一步变成可审查结果的工作。用户批准的应该是具体、可检查的结果。
- 用户建议不适合目标时直接说，不要迎合。
- 不要因为假想风险，主动加警告、免责声明、审批流程或安全/合规清单。

## 原生控件与 UI 验收

- 简单单选、日期、文件等交互，在原生控件能满足需求时优先使用原生控件；采用组件库不意味着每个控件都要额外封装。仅在有明确功能或设计需求时引入自定义控件。
- 调整组件先检查已有的 size、variant 和主题接口。高度、行高、内边距、边框、圆角及焦点样式要一起核对，不能零散覆盖后默认其余状态仍然协调。
- 对齐问题先区分布局位置与 outline、ring、shadow 等视觉外扩，确认实际原因后再改。不得用偏移、负边距或强制覆盖掩盖尚未查明的原因。
- 控件视觉改动按目标端的常用操作方式验收，PC 端默认用鼠标，移动端默认用触控。查看默认状态和本次改动直接影响的交互状态，涉及对齐时与相邻控件对照。键盘操作属于该流程的常用方式，或本次改动直接影响键盘行为时，验证对应操作。由子代理验收时，需报告实际观察结果及应验但未验的项目。

## 测试与验证

- Web 项目禁止执行 `npm run build`，也禁止通过 pnpm、Yarn 或 Bun 运行同一构建脚本。
- Web 项目启动 dev 服务前，先检查同一项目的同一服务是否已在运行。已有实例直接复用，禁止重复启动。本次任务启动的 dev 进程必须在任务结束前终止，并确认已退出。
- 除非用户明确要求，否则不新增或修改自动化测试代码，包括单元测试、集成测试、端到端测试和快照；修复 bug、增加功能和重构均遵守此规则。
- 不通过临时脚本或内联断言变相编写测试。
- 按改动风险做最小必要验收。从仓库已有的测试、lint、类型检查和构建中选择与改动相关的检查。需要浏览器验收时，检查改动点和直接受影响的关键流程，通过后收尾。
- 修复后复验改动点及直接受影响的流程。仅在用户明确要求或定位已发现的问题时扩大验收范围。专题清单按本次改动取用。
- 工具或环境限制导致某项无法验证时，如实说明未检查的内容。
- 验证失败时如实说明，不通过删除、跳过测试或弱化断言使其通过。
- 收尾删掉本次产生、之后用不上的临时文件。

## 工具与并行

- 搜索文件或文本优先使用 `rg`、`rg --files`。独立的读取和查询尽量批量执行。
- 浏览器验收与 Web 页面操作默认由当前代理使用 `ego-browser` CLI 完成，包括打开页面、点击、输入、查看 DOM 快照和截图验收。使用前先读取 `~/.agents/skills/ego-browser/SKILL.md` 并运行 `ego-browser skill`。
- 用户指定浏览器时，直接使用指定的浏览器。
- 在 Codex 中，若默认沙箱拦截 `ego-browser` 启动或连接 `ego lite.app`，使用已授权的 `ego-browser` 命令前缀在沙箱外重试同一操作。
- `ego-browser` 确认不可用时，Codex 优先使用内置浏览器。需要独立桌面浏览器时，优先使用 Microsoft Edge。
- 网页控制台优先使用可用的 CLI 或 API，飞书优先使用 `lark-cli`。需要操作界面时优先复用已登录的会话。
- 除下节所述的桌面操作外，只有存在独立的工作流，且委派能节省时间或提升质量时才使用子代理。主代理负责共享状态、连续决策和最终验收。委派时明确输入、输出和完成判据。

## Computer Use 默认委派

- 有专用工具、连接器或 API 时优先使用。需要操作原生桌面应用或浏览器外部的系统弹窗时，支持 Computer Use 子代理的环境默认由主代理委派。该委派已获持续授权，操作权限和高风险审批沿用现有规则。
- 同一任务尽量复用同一个 Computer Use 子代理，避免多个代理同时操作同一界面。主代理交代当前界面状态、目标、操作边界和验收条件，子代理直接执行，主代理核对结果。
- 仅在 Codex 中，主代理调用 `collaboration.spawn_agent` 委派 Computer Use 时指定 `model="gpt-6.1-terra"`、`fork_turns="none"`，默认使用 `reasoning_effort="low"`。仅当复杂的跨应用操作在 `low` 下无法稳定定位时，改用 `reasoning_effort="medium"`。

## 规则来源

- 共享的全局规则维护在 `~/.codex/AGENTS.md`。`CLAUDE.md` 和 Antigravity 的 `GEMINI.md` 作为兼容入口引用共享规则。
- 项目事实、生产状态、历史决策和对外契约以项目级 `AGENTS.md` 及其指定的脚本、探针、决策记录和合同文件为准。

## Git 提交规范

- 分支名按实际任务或改动命名，不添加 `ai`、AI 模型或 AI 编辑器相关标识（如 `gpt`、`claude`、`codex`、`cursor`、`copilot`），包括前缀、后缀和其他命名片段。
- Commit 使用中文，必须包含 title 和 body，中间空一行。
- Title 格式：`type(scope): 改动摘要`；scope 可省略。
- Type 按实际改动选择 feat、fix、refactor、docs、test、chore 等。
- Title 简洁说明主要结果；body 默认 2–4 条，说明关键改动、行为变化及必要原因。
- 根据最终 diff 编写，不记录对话过程、操作流水账，不逐个罗列文件。
- 不包含项目 ID、账号、邮箱、本地绝对路径、内部链接、凭据等隐私或内部环境信息。
- 验证结果和环境限制默认在回复中说明；仅在影响代码使用或维护时写入 body。
- 提交和推送需有用户授权；普通代码修改请求不自动包含提交、推送或重写历史的授权。
