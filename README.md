# Inline · 看到哪，理解到哪

一个单文件的阅读工具。选中文字就地展开问答，追问会挂成一层层的链，读完收起来，只在原文里留下一条淡淡的批注。

一个 `Inline.html`，零依赖，点两下就能用。

<p align="center">
  <img src="assets/logo.svg" alt="Inline" width="330">
</p>

<p align="center">
  <img src="assets/tagline.svg" alt="答案应该长在提问的地方，而不是别的地方。" width="700">
</p>

<p align="center"><sub><b>中文</b> · <a href="README.en.md">English</a></sub></p>

<p align="center">
  <a href="https://github.com/itzsu123/Inline/releases/latest/download/Inline.html">
    <img alt="下载 Inline.html" src="https://img.shields.io/badge/下载-Inline.html-2563eb?style=for-the-badge&logo=html5&logoColor=white">
  </a>
  &nbsp;
  <a href="https://itzsu123.github.io/Inline/Inline.html">
    <img alt="手机上直接打开" src="https://img.shields.io/badge/手机-直接打开-6d5bd0?style=for-the-badge">
  </a>
</p>

<p align="center"><sub>电脑:下载后双击打开 &nbsp;·&nbsp; 手机:点右边那个,不下载,数据按域名存着不会丢</sub></p>

<p align="center"><sub>需要一把 DeepSeek API key —— 填一次,只存在你自己的浏览器里</sub></p>


<p align="center">
  <img src="assets/demo-2.png" width="620" alt="选中「沉没成本」，答案就落在这一句下面">
</p>

<p align="center"><sub>选中一个词，答案落在这一句下面 —— 不是旁边，不是浮窗。</sub></p>

<p align="center">
  <img src="assets/demo-3.png" width="620" alt="在答案里再选一句，追问挂成一层">
</p>

<p align="center"><sub>在答案里再选一句，追问就挂成一层。缩进、浅紫，层级看得见。</sub></p>

<p align="center">
  <img src="assets/demo-4.png" width="620" alt="收起之后，正文里只剩一条带虚线的小字">
</p>

<p align="center"><sub>收起之后，正文还是正文，只多了一条带虚线的小字。</sub></p>
<!-- 动图待补：选中 → 出现「问」→ 点开 → 追问 → 收起 -->

**[English](README.en.md)**

---

## 它解决什么

读长文时想问一句，现在只有两个选择：

- **切出去搜索。** 回来要重新找到刚才那句话，注意力断一次。
- **开一个侧边栏或浮窗。** 答案被放进另一个平面，你得在两块地方之间来回看。

两者都要求你做同一件事：**把注意力从文字上搬走，再搬回来。**

Inline 的假设很简单：注意力在哪儿，答案就该出现在哪儿。

## 数据在哪

**全部在你自己的浏览器里。**

- 正文、每个窗口的内容，存在 `localStorage`，刷新不丢。
- API key 也只存在 `localStorage`，请求从浏览器直连 API，不经过任何中间服务器。
- 「导出」把整篇文章连同问答导出成 Markdown，可以带走。
- 「存档」导出 JSON，「导入」可以搬回另一个浏览器。

没有账号，没有后端，没有埋点。

## 换模型 / 换服务商

模型没有写死在业务代码里。文件顶部只有两个常量：

```js
const API_BASE  = "https://api.deepseek.com";
const API_MODEL = "deepseek-chat";
```

**任何兼容 OpenAI /chat/completions 接口的服务都能用。** 比如：

| 服务 | API_BASE | API_MODEL |
|---|---|---|
| DeepSeek（默认） | https://api.deepseek.com | deepseek-chat |
| 通义千问 | https://dashscope.aliyuncs.com/compatible-mode/v1 | qwen-plus |
| Kimi | https://api.moonshot.cn/v1 | moonshot-v1-8k |
| 本地 ollama | http://localhost:11434/v1 | qwen2.5:7b ¹ |

¹ **本地 ollama 要先放行浏览器**，否则请求会被 CORS 拦掉（表现是 Failed to fetch，看起来像网络问题）：

```bash
# macOS / Linux
OLLAMA_ORIGINS=* ollama serve

# Windows
set OLLAMA_ORIGINS=*
ollama serve
```

* 表示允许任意网页调用本机的模型。自己用没问题，用完关掉就好。

改完之后，页面上那个 key 输入框要填**对应服务商**的 key。

有两个已知的边界：

- 只用了最基础的 chat/completions，没有用各家私有参数。
- 模型名写错时，服务端返回的错误会被原样显示在窗口里（带重试）。

## 关于不做

不做账号，不做云同步，不做历史，不做移动 App，不做后端。

也不做 PDF —— 本质上就是内嵌问答,我们完全有更巧妙的实现方法

## 许可

MIT
