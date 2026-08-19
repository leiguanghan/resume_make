# 简历撰写助手（Resume Builder）

一个纯前端、零依赖、可离线运行的简历撰写工具。数据仅保存在本机浏览器，不需要服务器、不注册账号、不采集任何信息。

## 功能

- A4 一页纸分页预览，超出一页自动分页，支持自定义缩放
- 模块化内容编辑：添加 / 删除 / 拖拽排序 / 撤销 / 清空
- 每项内容支持左对齐 / 居中 / 右对齐，字号对齐 Word 的 23 档字号
- 支持选中文字局部加粗
- 手动在任意两个内容之间添加 / 删除横实线
- AI 润色（可选）：自带 OpenAI 兼容接口，DeepSeek / 通义千问 / Moonshot / GLM / 混元 / OpenAI 等均可
- 导出高清 PDF（无损）与 DOCX（保留字号、对齐、加粗、横线）
- 导入 DOCX：解析本地 Word 文档文字并写入编辑区

## 本地运行

### 桌面端

直接双击 `index.html` 用浏览器打开即可，无需安装任何东西。

### 移动端 / 局域网

移动端不便直接打开本地文件，推荐二选一：

1. 部署到静态托管（见下文），手机浏览器直接访问网址；
2. 本地起一个静态服务器，手机与电脑连同一 Wi-Fi 访问：

```bash
# 在项目目录下
python -m http.server 8080
# 或
npx serve .
```

然后访问 `http://localhost:8080`（同一局域网手机访问 `http://<电脑IP>:8080`）。

## 本地存储与读取

- 所有数据保存在浏览器的 `localStorage` 中，编辑时实时写入，关闭后再打开会自动恢复。
- 存储范围：**按“浏览器 + 设备 + 访问地址(origin)”隔离**。同一台设备换浏览器、或换设备，数据不会自动同步。
- 清空浏览器站点数据 / 无痕模式会丢失数据，请定期用「导出 PDF / 导出 DOCX」备份。
- 点击「导入 DOCX」会解析文档并覆盖当前编辑区内容。

> 说明：localStorage 大小通常约 5MB，对本项目足够；如需跨设备同步，需要后端或云存储（本项目刻意保持纯前端，不含后端）。

## 开源 / 部署到线上（让其他用户直接用）

本项目就是一个普通静态站点，托管到任意静态平台即可：

### GitHub + GitHub Pages

1. 在 GitHub 新建仓库，把本目录文件全部上传（`index.html`、`manifest.webmanifest`、`sw.js`、`icon.svg`、`README.md`、`LICENSE`）。
2. 仓库 `Settings → Pages`，Source 选择 `Deploy from a branch`，分支选 `main`、目录选 `/ (root)`，保存。
3. 几分钟后得到网址 `https://<用户名>.github.io/<仓库名>/`，手机电脑都能直接打开。
4. 移动端打开后用浏览器「添加到主屏幕」，即可以独立 App 形式离线运行（PWA，需 HTTPS，GitHub Pages 自带 HTTPS）。

### 其他托管

Netlify / Vercel / Cloudflare Pages 均可：把目录拖拽或关联仓库部署，零配置。

## AI 设置

1. 点击右上角「AI设置」。
2. 填写你自有的 OpenAI 兼容接口：API Base URL、API Key、Model Name（可点「常用平台」预设快速填入）。
3. 密钥只保存在你本机浏览器 localStorage，不会上传到任何服务器。
4. 可在「全局 Prompt」中自定义润色提示词，并让 AI 帮你优化 Prompt。

## 目录结构

```
resume-builder-open-source/
├── index.html           # 应用主体（单文件，零依赖）
├── manifest.webmanifest # PWA 清单
├── sw.js                # Service Worker（离线缓存）
├── icon.svg             # 应用图标
├── README.md
└── LICENSE
```

## 技术栈

纯 HTML + CSS + JavaScript，无构建步骤、无第三方依赖。PDF 用 Canvas + 无损 FlateDecode 生成；DOCX 用原生 ZIP 写入；DOCX 导入用原生 ZIP 解析 + DOMParser。

## 隐私

所有数据仅保存在用户本地浏览器，不上传。AI 请求直接由用户浏览器发往用户自行填写的 API 地址。
