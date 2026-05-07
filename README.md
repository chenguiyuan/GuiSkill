# GuiSkill

桂圆的个人 Skill 库——收录我为 Claude Code 定制的学习与工作流技能。

## 安装

```bash
npx skills add chenguiyuan/GuiSkill
```

安装后在 Claude Code 里执行 `/reload-plugins` 激活。

**更新方式与安装相同，重新运行一次即可。**

---

## 技能列表

### [2sigma-learning](./2sigma-learning/SKILL.md) — AI 自适应学习法

基于布鲁姆 **2 Sigma 效应**的一对一学习技能。

普通课堂教学只能让学生达到平均水平，而一对一辅导可以让同一个学生超越 98% 的同班同学。2sigma-learning 把这个效应复现到 AI 学习场景里：

> AI 生成学习文件 → 你在文件末尾写真实反馈 → AI 读取反馈决定下一步，循环往复

已掌握的跳过，卡住的反复拆解，始终锚定你的真实理解盲区。

**适用场景：**
- 学习一套陌生方法论
- 啃需要反复咀嚼的文本
- 把理论转化成你自己的行动方案

**触发方式：**
```
用 2sigma-learning 开始学习 [材料名称或内容]
```

---

## 结构

```
GuiSkill/
├── .claude-plugin/
│   └── marketplace.json
└── 2sigma-learning/
    └── SKILL.md
```

---

## License

MIT
