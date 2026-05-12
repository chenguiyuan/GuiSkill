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

### [wechat-format](./wechat-format/SKILL.md) — 微信公众号 HTML 排版

将文章 Markdown 或纯文本一键生成可粘贴到公众号编辑器的 HTML 文件，所有样式（背景色、字色、圆角、badge、引用块）完整保留。

核心原理：忠实翻译原文已有的结构，不强加模板。有编号标题就生成 badge 组件，有引用块就生成金句块，纯叙述文章就只生成段落——原文是什么结构，HTML 就是什么结构。

**技术亮点：**
- 用 copy 事件监听绕过微信 sanitizer 的强过滤路径，样式保留率接近 100%
- 所有块级容器使用 `<section>`（微信保留其 inline style），禁用 `<div>`

**适用场景：**
- 公众号文章排版
- 快速生成带格式的公众号草稿

**触发方式：**
```
帮我排版 / 公众号排版 / 微信排版 / 做成公众号格式
```

---

## 结构

```
GuiSkill/
├── .claude-plugin/
│   └── marketplace.json
├── 2sigma-learning/
│   └── SKILL.md
└── wechat-format/
    ├── SKILL.md
    └── references/
        └── tech-notes.md
```

---

## License

MIT
