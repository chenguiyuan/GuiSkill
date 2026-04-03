# GuiSkill

桂圆的个人 Skill 库——收录我为 Claude Code 定制的学习与工作流技能。

每个 Skill 都是一个独立的文件夹，包含 `SKILL.md`（技能定义文件）。

---

## 技能列表

### [2sigma-learning](./2sigma-learning/SKILL.md) — AI 自适应学习法

基于布鲁姆 2 Sigma 效应的 1 对 1 学习技能。

**核心机制**：AI 生成学习文件 → 你在文件末尾写反馈 → AI 读取反馈决定下一步，循环往复。跳过你已知的内容，深挖你卡住的地方。

**适用场景**：
- 学习一套方法论（如商业写作、对标分析）
- 啃硬核书籍或哲学著作
- 把理论概念转化为你自己的具体行动

**如何使用**：

1. 把 [`2sigma-learning/SKILL.md`](./2sigma-learning/SKILL.md) 放入你的 Claude Code 项目的 `.agents/skills/2sigma-learning/` 目录
2. 在聊天框发送：
   > "我要开始学习 [材料名称]，请调用 2sigma-learning 技能帮我开始。"
3. AI 会自动初始化学习项目，生成第一个学习文件

详细的执行流程、AI 行为约束见 [SKILL.md](./2sigma-learning/SKILL.md)。

---

## 使用方式

将任意 Skill 的文件夹复制到你的项目 `.agents/skills/` 目录下即可激活。
