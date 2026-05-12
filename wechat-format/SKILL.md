---
name: wechat-format
description: 将文章 Markdown 或纯文本转换为可一键复制到微信公众号编辑器的 HTML 文件，完整保留背景色、字色、圆角、引用块、badge 等所有格式。当用户说"公众号排版"、"排版HTML"、"生成排版"、"帮我排版"、"微信排版"、"做成公众号格式"时必须使用这个 skill，即使用户只是给了一段文字没有明确说"skill"也要触发。
---

# 公众号排版 Skill

将文章内容生成一个 HTML 文件，用 Chrome 打开后点一个按钮就能复制到微信公众号编辑器，所有样式完整保留。

## 核心原则

**忠实原文结构，不主动新增层级。**
原文有什么结构标记，翻译成对应组件；原文没有的，不添加。
一篇没有任何标题的文章，就只生成 `<p>` 段落，最多提取一处最有力的句子作为金句块。

---

## 执行步骤

### Step 1: 逐行扫描原文，识别结构信号

按以下规则，将原文每个元素分类：

| 原文中发现 | 识别条件 | 翻译成 |
|-----------|---------|--------|
| `**01. 标题**` 独立成行 | 粗体行，带数字编号 | 编号 badge 标题（01/02/03）|
| `**标题**` 独立成行 | 粗体行，无数字编号 | 左竖线标题（无 badge）|
| `> 引用文字` | Markdown 引用块 | 居中金句块 |
| `---` | 章节分隔线 | `· · ·` 分隔线 |
| `第一，...第二，...` | 嵌在段落里的散文列举 | 保持 `<p>`，`第X，` 用 `<strong>` 加粗 |
| `**加粗词**` 嵌在段落内 | 粗体在段落中间 | `<strong>` 内联加粗 |
| 其余所有文字 | 普通段落 | `<p>` |

**判断"独立成行"的标准**：该行只有这个标题，前后是空行，不是段落中间的加粗。

### Step 2: 生成 HTML

硬规则（不可违反）：
- **所有块级容器用 `<section>`，禁止 `<div>`**
- **所有样式写 inline style，禁止 `<style>` 块里放内容样式**

用下方组件库中的暖橙配色，按 Step 1 的分类结果逐一组装。

#### 文件骨架

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>[文章标题]</title>
<style>
  #copy-btn {
    position:fixed;top:20px;right:20px;z-index:999;
    background:#1a1814;color:#fff;border:none;
    padding:12px 24px;border-radius:8px;font-size:15px;
    cursor:pointer;font-family:-apple-system,'PingFang SC',sans-serif;
    box-shadow:0 4px 12px rgba(0,0,0,0.2);
  }
  #copy-btn:hover{background:#b07250;}
  #copy-btn.done{background:#4a8c5c;}
</style>
<script>
function copyArticle(){
  const article=document.getElementById('article');
  const html=article.outerHTML;
  const plainText=article.innerText;
  const btn=document.getElementById('copy-btn');
  function onSuccess(){
    btn.textContent='✓ 已复制，去公众号粘贴';
    btn.classList.add('done');
    setTimeout(()=>{btn.textContent='复制到公众号';btn.classList.remove('done');},3000);
  }
  function copyViaEvent(){
    return new Promise((resolve,reject)=>{
      let succeeded=false;
      const onCopy=(e)=>{
        try{e.clipboardData.setData('text/html',html);e.clipboardData.setData('text/plain',plainText);e.preventDefault();succeeded=true;}catch(err){}
      };
      document.addEventListener('copy',onCopy,true);
      const ghost=document.createElement('div');
      ghost.setAttribute('contenteditable','true');
      ghost.style.cssText='position:fixed;top:0;left:0;width:1px;height:1px;opacity:0;overflow:hidden;pointer-events:none;z-index:-1;';
      ghost.textContent='.';
      document.body.appendChild(ghost);
      const sx=window.scrollX,sy=window.scrollY;
      const range=document.createRange();
      range.selectNodeContents(ghost);
      const sel=window.getSelection();
      sel.removeAllRanges();sel.addRange(range);
      try{
        const ok=document.execCommand('copy');
        if(!ok&&!succeeded)throw new Error('execCommand failed');
        resolve();
      }catch(err){reject(err);}
      finally{
        sel.removeAllRanges();
        if(ghost.parentNode)ghost.parentNode.removeChild(ghost);
        window.scrollTo(sx,sy);
        document.removeEventListener('copy',onCopy,true);
      }
    });
  }
  function copyViaClipboardAPI(){
    const blobHtml=new Blob([html],{type:'text/html'});
    const blobText=new Blob([plainText],{type:'text/plain'});
    return navigator.clipboard.write([new ClipboardItem({'text/html':blobHtml,'text/plain':blobText})]);
  }
  copyViaEvent().then(onSuccess).catch(()=>copyViaClipboardAPI().then(onSuccess).catch(()=>alert('复制失败，请手动 Ctrl+A 全选后复制')));
}
</script>
</head>
<body style="margin:0;padding:0;background:#f5f3ee;">
<button id="copy-btn" onclick="copyArticle()">复制到公众号</button>

<section id="article" style="max-width:677px;margin:0 auto;padding:28px 20px 80px;background:#faf9f5;font-family:-apple-system,'PingFang SC','Hiragino Sans GB','Microsoft YaHei',sans-serif;color:#3b3730;font-size:16px;line-height:1.95;">

[正文内容]

<!-- 页脚 -->
<section style="margin-top:56px;padding-top:22px;border-top:1px solid #e8e2d8;text-align:center;font-size:13px;color:#9c9287;line-height:2.4;">
  <p style="margin:0;">感谢阅读</p>
</section>

</section>
</body>
</html>
```

---

## 组件库（暖橙配色）

配色：底色 `#faf9f5`，页面底色 `#f5f3ee`，主题橙 `#b07250`，淡橙背景 `#f0e8df` / `#f7f0ea`，标题色 `#1a1814`，正文色 `#3b3730`

**正文段落**
```html
<p style="font-size:16px;color:#3b3730;line-height:1.95;margin:0 0 18px 0;">文字</p>
```

**编号 badge 标题**（对应 `**01. 标题**` 独立成行）
```html
<section style="margin-bottom:22px;">
  <span style="background:#f0e8df;color:#b07250;font-size:15px;font-weight:700;letter-spacing:0.12em;padding:4px 10px;border-radius:4px;margin-right:12px;display:inline-block;vertical-align:middle;">01</span>
  <span style="font-size:18px;font-weight:600;color:#1a1814;letter-spacing:0.02em;vertical-align:middle;">章节标题</span>
</section>
```

**左竖线标题**（对应 `**标题**` 独立成行，无编号）
```html
<section style="margin-bottom:22px;padding-left:14px;border-left:3px solid #b07250;">
  <span style="font-size:18px;font-weight:600;color:#1a1814;letter-spacing:0.02em;">章节标题</span>
</section>
```

**居中金句块**（对应 `> 引用文字`）
```html
<section style="border-left:3px solid #b07250;background:#f7f0ea;border-radius:0 8px 8px 0;padding:18px 20px;margin:24px 0;">
  <p style="font-size:16px;color:#3b3730;line-height:1.95;margin:0;">引用内容</p>
</section>
```

**· · · 分隔线**（对应 `---`）
```html
<section style="text-align:center;color:#c8c0b5;letter-spacing:0.4em;margin:24px 0;font-size:14px;">· · ·</section>
```

**内联加粗**（对应段落内的 `**文字**`）
```html
<strong style="color:#1a1814;">加粗文字</strong>
```

---

### Step 3: 保存文件

路径：当前工作目录下的 `YYYYMMDD-[标题简写]-wechat.html`

### Step 4: 告知使用方式

```
✅ 排版文件已生成：[文件名]

用法：
1. Chrome 打开该文件
2. 点右上角「复制到公众号」
3. 打开公众号编辑器，Ctrl+V 粘贴
```

---

## 参考文档

`references/tech-notes.md` — 完整技术原理和验证过的 CSS 属性清单
