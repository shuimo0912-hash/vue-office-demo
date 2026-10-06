# Vue 文档预览 Demo

基于 Vue 3 + Vite + `@vue-office` 系列组件的在线文档预览示例，支持：

- Word（`.docx`）— `@vue-office/docx`
- Excel（`.xlsx` / `.xls`）— `@vue-office/excel`
- PDF（`.pdf`）— `@vue-office/pdf`
- PPTX（`.pptx`）— `@vue-office/pptx`

## 快速开始

```bash
npm install
npm run dev
```

打开终端输出的本地地址（默认 http://localhost:5173）即可。

## 使用方式

1. 点击顶部标签切换文档类型（Word / Excel / PDF / PPTX）。
2. 点击“选择文件”上传本地文件，或在输入框粘贴远程文件 URL。
3. 文件会以 ArrayBuffer 或 URL 形式传给对应的预览组件。

## 依赖说明

`@vue-office/*` 依赖 `vue-demi` 以兼容 Vue 2 / Vue 3。本示例将 `vue-demi` 锁定为 `0.14.6`，避免版本不匹配导致的运行时错误（例如 `Cannot read properties of undefined (reading 'onMounted')`）。

## 核心代码位置

- `src/App.vue` — 演示页面，包含标签切换、文件上传、URL 预览
- `src/main.js` — Vue 应用入口
- `vite.config.js` — Vite 配置
