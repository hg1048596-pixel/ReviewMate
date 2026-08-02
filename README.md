# ReviewMate — AI 驱动的职场复盘助手

ReviewMate 是一款基于 AI 的职场绩效考评与工作复盘智能代理工具。导入工作记录文件，通过 AI 对话进行工作分析、生成周报/月报/总结报告，并可导出为 PPT 和 Word 文档。

---

## 快速开始

### 环境要求

- **Node.js 22 LTS**（[下载地址](https://nodejs.org)）
- Windows / macOS / Linux 均可运行

### 一键启动

1. 解压 `ReviewMate-v1.3.zip` 到任意目录
2. 双击运行 `start.bat`
   - 首次运行会自动执行 `npm install` 安装依赖（约 1-2 分钟）
   - 安装完成后自动启动服务并打开浏览器
3. 访问 `http://localhost:3001`

> macOS / Linux 用户：在 `server/` 目录执行 `npm install`，然后 `node src/index.js`

### 配置 AI

启动后进入 **设置** 页面，配置以下内容以启用 AI 功能：

| 配置项 | 说明 | 示例 |
|--------|------|------|
| AI API URL | OpenAI 兼容的 API 地址 | `https://api.openai.com/v1` |
| AI API Key | API 密钥 | `sk-...` |
| AI Model | 模型名称 | `gpt-4o-mini` |

> 未配置 AI 也可使用，系统会降级为内置模板生成报告和文档。

---

## 核心功能

### 1. 仪表盘
- 工作记录汇总、完成率、总投入时长
- 近期绩效趋势一览
- 快速导航至各功能模块

### 2. 工作记录
- 新增 / 编辑 / 删除工作记录
- 支持按时间范围、项目、标签筛选
- 从文件目录批量导入（`.md` / `.txt` / `.json` / `.csv`）

### 3. 绩效看板
- 任务完成率统计
- 时间投入分布
- 产出量趋势对比（同比 / 环比）
- 关键词 / 标签频率分析

### 4. 复盘报告
- 自动生成周报 / 月报 / 季度总结
- 套用结构化模板：亮点 → 不足 → 改进 → 计划
- 引用数据支撑结论

### 5. 目标管理
- 创建和管理 OKR / KPI 目标
- 设置权重和目标值
- 跟踪实际完成进度

### 6. AI 助手
- **智能对话**：基于你的工作数据进行分析和答疑
- **文件导入**：拖拽或粘贴路径导入文件（支持 Word / PPT / Excel / PDF / Markdown 等），文件内容直接发送给 AI 学习
- **思考过程展示**：实时显示 AI 的推理过程，避免长时间等待无反馈
- **导出 PPT**：AI 全权设计配色、布局和页面结构，一键生成并下载
- **导出 Word**：AI 生成结构化文档，一键下载
- **对话持久化**：对话记录保存在浏览器中，关闭服务前始终保留

### 7. 设置
- AI 模型配置
- 文件下载路径自定义（默认 `C:\Users\<用户名>\Downloads\ReviewMate\`）
- 数据库文件位置管理（迁移 / 新建 / 切换）

---

## 支持的文件格式

### AI 助手导入（发送给 AI 学习）

| 格式 | 解析方式 |
|------|----------|
| `.md` / `.txt` | 直接读取文本 |
| `.json` / `.csv` | 直接读取文本 |
| `.docx` | mammoth 提取纯文本 |
| `.pptx` | 解析幻灯片 XML 提取文本 |
| `.xlsx` | 逐 sheet 转为 CSV 文本 |
| `.pdf` | pdf-parse 提取文本 |

### 工作记录批量导入

| 格式 | 说明 |
|------|------|
| `.md` | 解析 Markdown 任务列表 |
| `.json` | 解析工作条目数组 |
| `.csv` | 解析表格数据 |
| `.txt` | 按行解析 |

---

## 技术栈

| 层 | 技术 |
|----|------|
| 前端 | React + Vite + Tailwind CSS + Lucide Icons |
| 后端 | Node.js + Express |
| 数据库 | SQLite (better-sqlite3) |
| AI | OpenAI 兼容 API（SSE 流式传输） |
| 文档生成 | pptxgenjs (PPT) + docx (Word) |
| 文件解析 | mammoth / xlsx / adm-zip / pdf-parse |

---

## 项目结构

```
ReviewMate/
├── start.bat                  # Windows 一键启动脚本
├── README.md                  # 本文档
├── client/                    # 前端
│   ├── dist/                  # 构建产物（Express 直接 serve）
│   ├── src/
│   │   ├── components/
│   │   │   ├── ChatPanel.jsx       # AI 助手聊天面板
│   │   │   ├── SplashScreen.jsx    # 开屏界面
│   │   │   ├── Layout.jsx          # 侧边栏布局
│   │   │   └── ErrorBoundary.jsx
│   │   ├── pages/
│   │   │   ├── Dashboard.jsx       # 仪表盘
│   │   │   ├── WorkRecords.jsx     # 工作记录
│   │   │   ├── Performance.jsx     # 绩效看板
│   │   │   ├── Reports.jsx         # 复盘报告
│   │   │   ├── Goals.jsx           # 目标管理
│   │   │   └── Settings.jsx        # 设置
│   │   ├── api.js                  # API 封装
│   │   └── main.jsx
│   ├── vite.config.js
│   └── tailwind.config.js
└── server/                    # 后端
    ├── src/
    │   ├── index.js           # Express 入口（端口 3001）
    │   ├── db/
    │   │   ├── init.js        # 数据库初始化
    │   │   └── seed.js        # 种子数据
    │   ├── routes/
    │   │   ├── ai.js          # AI 聊天 + 文件导入 + PPT/Word 生成
    │   │   ├── workRecords.js # 工作记录 CRUD
    │   │   ├── reports.js     # 报告生成
    │   │   ├── analysis.js    # 统计分析
    │   │   ├── metrics.js     # 绩效指标
    │   │   ├── goals.js       # 目标管理
    │   │   ├── settings.js    # 设置
    │   │   └── feishu.js      # 飞书集成
    │   ├── services/
    │   │   ├── collector.js       # 文件采集与文本提取
    │   │   ├── analyst.js         # 统计分析引擎
    │   │   ├── reviewer.js        # 报告生成引擎
    │   │   └── documentGenerator.js # PPT/Word 生成
    │   └── utils/
    │       └── aiConfig.js        # AI 配置读取
    └── db-config.json         # 数据库路径配置
```

---

## API 概要

| 端点 | 方法 | 说明 |
|------|------|------|
| `/api/work-records` | GET / POST | 查询 / 新增工作记录 |
| `/api/work-records/import` | POST | 从文件批量导入 |
| `/api/analysis/stats` | GET | 获取统计数据 |
| `/api/analysis/trends` | GET | 获取趋势数据 |
| `/api/reports/generate` | POST | 生成复盘报告 |
| `/api/reports` | GET | 查询历史报告 |
| `/api/metrics` | GET | 查询绩效指标 |
| `/api/goals` | CRUD | 目标管理 |
| `/api/ai/chat` | POST | AI 对话（SSE 流式） |
| `/api/ai/import-file` | POST | 导入文件提取文本供 AI 学习 |
| `/api/ai/generate-ppt` | POST | AI 生成 PPT |
| `/api/ai/generate-docx` | POST | AI 生成 Word 文档 |
| `/api/settings` | GET / PUT | 读取 / 更新设置 |

---

## 常见问题

### PPT / Word 下载后找不到文件？
Windows 的"受控文件夹访问"可能阻止写入 Downloads 目录。系统会自动降级保存到临时目录 `%TEMP%\ReviewMate\`，下载仍可正常使用。如需直接保存到 Downloads，可在 Windows 安全中心关闭"受控文件夹访问"或添加 Node.js 到白名单。

### AI 对话一直显示"思考中..."？
推理模型（如 deepseek-v4-pro）会先输出思考过程再输出正式回复。系统会实时显示思考内容，请耐心等待。如果超过 4 分钟无响应，请检查网络连接和 API 密钥。

### AI 生成 PPT 报 504 超时？
系统已使用流式调用避免 CDN 超时。如仍出现，可能是 API 服务端负载过高，请稍后重试。

### 如何修改下载路径？
进入 **设置** 页面，在"文件下载位置"中修改路径，默认为 `C:\Users\<用户名>\Downloads\ReviewMate\`。

---

## 版本历史

| 版本 | 日期 | 主要更新 |
|------|------|----------|
| v1.3 | 2026-07-31 | AI 全权设计 PPT；文件导入改为发送给 AI；流式调用避免 504；思考过程展示 |
| v1.2 | 2026-07-31 | 拖拽导入；支持 Word/PPT/Excel/PDF；开屏动画；对话持久化 |
| v1.1 | 2026-07-30 | PPT/Word 生成；下载路径配置；EPERM 修复 |
| v1.0 | 2026-07-30 | 初始版本 |
