# 微信公众号 HTML 排版技术说明

验证日期：2026-05-12，完全可用。

## 核心规则速查

| 规则 | 原因 |
|------|------|
| 块级容器用 `<section>` | 微信 sanitizer 保留 section 的 inline style，剥掉 div 的 |
| 内联元素用 `<span>` / `<p>` | 正常保留 |
| 禁止 `<div>` 带样式 | 所有 CSS 会被剥掉 |
| 禁止 `<style>` 块放内容样式 | 微信会完整剥掉 style 标签 |
| 剪贴板用 copy 事件监听 | 让微信走宽松过滤路径 |

## 剪贴板机制原理

微信按"来源"分两条过滤路径：
- `navigator.clipboard.write({text/html})` → 外部 HTML → 强过滤（背景色基本保留，字色/字号/letter-spacing 等大量丢失）
- **copy 事件 + `clipboardData.setData()`** → 富文本编辑器来源 → 宽松过滤（绝大多数 inline style 保留）✅

实现：创建 1x1 不可见 contenteditable div → 选中内容 → `execCommand('copy')` 触发 copy 事件 → 在事件中用 `clipboardData.setData('text/html', 完整HTML)` 注入 → 降级到 `clipboard.write()`

来源：mdeditor（GitHub: xiaobox/mdeditor）的 clipboard.js 实现。

## 验证可用的 CSS 属性

color、background-color（在 section 上）、font-size、font-weight、letter-spacing、line-height、text-align、padding、margin、border、border-radius、border-left、text-transform、display:inline-block、vertical-align、width/height（px）
