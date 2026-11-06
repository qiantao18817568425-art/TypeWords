<h1 align=center>
  <img src="https://github.com/user-attachments/assets/9d626e0f-0601-4640-8981-ad66d8ac4853" alt="TypeWords" style="width: 500px;"/>
</h1>

<p align="center">
  <a href="/README.md">English</a> |
  <a href="/docs/README.es.md">Español</a> |
  <a href="/docs/README.de.md">Deutsch</a> |
  <a href="/docs/README.fr.md">Français</a> |
  <a href="/docs/README.pt.md">Português</a> |
  <a href="/docs/README.ru.md">Русский</a> |
  <a href="/docs/README.uk.md">Українська</a> |
  <a href="/docs/README.ja.md">日本語</a> |
  <a href="/docs/README.ko.md">한국인</a> |
  <a href="/docs/README.th.md">ไทย</a> |
  <a href="/docs/README.vi.md">Tiếng Việt</a> |
  <a href="/docs/README.id.md">Bahasa Indonesia</a> |
  <a href="/docs/README.zh-TW.md">繁體中文</a> |
  <a href="/docs/README.zh-CN.md">简体中文</a> 
</p>

<p align="center">
  <b>Learn English, one keystroke at a time; smarter memorization, more efficient learning - an open-source word and article practice tool</b>
</p>

## 从零基础到座舱 TPM 英语

[打开练习网站](https://qiantao18817568425-art.github.io/TypeWords/)

此版本基于 [zyronon/TypeWords](https://github.com/zyronon/TypeWords)，保留项目许可证和署名。
学习路线包含 8 本词库、759 条练习，每条附中文释义和双语例句。

| 顺序 | 词库 | 条目 | 练完后尝试做的事 |
| --- | --- | ---: | --- |
| 1 | 零基础核心 | 120 | 从人称、数字、时间和常用动作开始，练习用短句自我介绍和提问。 |
| 2 | 生活场景词汇 | 108 | 认得家里、商店、餐厅、车站和就医时常见的词，能用简单英语表达基本需求。 |
| 3 | 日常英语对话 | 70 | 练习问候、购物、点餐、出行、预约和礼貌求助时能直接说的短句。 |
| 4 | 工作英语沟通 | 90 | 从读懂邮件和会议邀请开始，练习约时间、请求澄清、回复和跟进。 |
| 5 | 座舱 TPM 入门 | 86 | 掌握项目推进、问题分诊、责任分配、跨团队协作和验收的基础术语。 |
| 6 | TPM 项目与交付 | 110 | 练习需求、排期、供应商协作、风险、验证、版本管理和交付用语。 |
| 7 | 座舱技术与诊断 | 125 | 练习描述座舱系统、软硬件接口、故障现象和取证动作。 |
| 8 | TPM 闭环与汇报 | 50 | 用可直接复用的句型说清影响、证据、责任、下一步、风险和关闭标准。 |

打开「单词 → 选择词典」，从 01 开始。有基础时可直接选择对应场景。先跟打并读出例句，再用默写复习。到 08 时，尝试用英文说明一个问题的影响、已有证据、责任域和下一步。

学习记录保存在当前浏览器。换电脑时可通过设置导出和导入。

用 Node.js 24 和 pnpm 安装锁定依赖后，设置环境变量 `NUXT_APP_BASE_URL=/TypeWords/` 和 `NITRO_PRESET=github-pages`，执行 `pnpm run generate`。将 `.output/public` 发布到 GitHub Pages。

## Project Introduction

<https://www.bilibili.com/video/BV1QwYv6eEAS>

## Online Access

<https://typewords.cc>

<img width="1920" height="1440" alt="practice words" src="/public/imgs/words.png" />
<img width="1920" height="1440" alt="practice articles" src="/public/imgs/articles.png" />

## Features

### Word Practice

- Practice modes: Follow-along / Dictation / Self-test / Spelling from memory
- Smart mode: Automatically calculates learning words based on memory curves, deepening memory through dictation
- Free mode: No restrictions, plan your own learning
- Provides phonetics, pronunciation (American/British), example sentences, phrases, synonyms, root words, etymology, error statistics, and more

### Article Memorization

- Built-in classic textbooks; you can also add or import articles with one-click translation and bilingual comparison
- Follow-along + dictation dual modes, sentence-by-sentence input with automatic pronunciation for more efficient memorization
- Supports listening while writing from memory to reinforce learning

### Favorites, Wrong Words, Mastered

- Words typed incorrectly while learning are automatically added to your wrong word book for later review
- Actively add words to mastered to automatically skip them in future sessions
- Add words to favorites for consolidation and review

### Highly Customizable

- Rich keyboard sound effects
- Customizable shortcuts
- Highly configurable settings

### Clean and Efficient

- Clean design, modern UI, ad-free
- Refreshing interface, simple operation
- No forced subscription to any platform

### Vocabulary Library

Built-in commonly used vocabulary including CET-4, CET-6, GMAT, GRE, IELTS, SAT, TOEFL, Graduate English, TEM-4, TEM-8, and more (official website only).
Designed to meet most users' vocabulary learning needs. Community contributions of additional vocabulary are welcome.

## Running the Project

#### Note: This project can run standalone with data saved locally. Manual backup is required when switching devices; this does not affect normal usage.

This project is built with `Nuxt` and requires a Node.js environment.

1. Install NodeJS, refer to the [official documentation](https://nodejs.org/en/download)
2. The project is large. It's recommended to use `git clone --depth 1 https://github.com/zyronon/TypeWords.git` to clone only the latest commit. GitHub's Download ZIP feature will not work properly.
3. In the project root directory, open a terminal and run `pnpm install` to download dependencies.
4. Run `pnpm run dev` to start the project. The default address is [`http://localhost:5567`](http://localhost:5567)
5. Open [`http://localhost:5567`](http://localhost:5567) in your browser to access the project.
6. Run `pnpm run generate` to build the project files.

## Features and Suggestions

The project is currently in early development, with new features being added continuously. If you have any suggestions or feature requests, feel free to open an `Issue`.
If you like the design philosophy of this software, please submit a `PR`. Thank you for your support!

## Contributing Guide

[Contributing Guidelines](/docs/CONTRIBUTING.md)

If you're interested in this project, we welcome your contributions and will provide as much help as possible.

Before contributing, please communicate with the developers to avoid code conflicts.

Thank you again for your contributions!


## ❤️ Support TypeWords
If TypeWords has been helpful to you, feel free to sponsor the project to support server operation and future development.  
Of course, sponsorship is not required for use—sharing the project, submitting feedback, or contributing code are also highly valuable forms of support.

<img width="300" height="390" alt="practice words" src="/public/imgs/zhifubao.png" />
<img width="300" height="390" alt="practice words" src="/public/imgs/weixin.png" />

