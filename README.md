# 够得着 / Reachable

> AI求职能力定位器 — 帮助大学生看清能力位置、找到岗位方向、补上能力差距

## 项目简介

够得着是一个专为大学生设计的智能求职助手，通过AI对话式交互，帮助用户：

- 生成六维能力画像（技术硬技能、项目经验、行业认知、沟通表达、实习经历、学习潜力）
- 基于RAG技术匹配20+真实岗位数据
- 获得个性化成长计划和提升路径
- 支持简历上传或手动描述背景

## 核心功能

| 功能 | 说明 |
|------|------|
| 对话式能力画像 | 通过自然对话采集背景，生成六维能力评估 |
| RAG岗位匹配 | 基于20+真实岗位数据，推荐稳拿/冲刺/梦想三类岗位 |
| 个性化成长计划 | 针对目标岗位生成可执行的技能学习、项目建议、时间规划 |
| 简历信息提取 | 支持文本文件上传，或直接对话描述背景 |

## 技术架构

```
前端：Next.js 16 + React 19 + TypeScript + shadcn/ui + Tailwind CSS 4
后端：Next.js API Routes
AI引擎：智谱GLM-5（流式对话）
RAG技术：向量数据库 + 语义搜索（coze-coding-dev-sdk）
```

## 本地开发

### 环境要求

- Node.js 18+
- pnpm 9+

### 快速启动

#### macOS / Linux

```bash
# 安装依赖
pnpm install

# 配置环境变量
cp .env.local.example .env.local
# 编辑 .env.local，填入你的智谱API Key

# 启动开发服务器
pnpm dev
```

#### Windows

```powershell
# 安装依赖
pnpm install

# 配置环境变量
copy .env.local.example .env.local
# 编辑 .env.local，填入你的智谱API Key

# 启动开发服务器
pnpm dev:win
```

### 环境变量配置

创建 `.env.local` 文件，必需配置：

```bash
# 智谱AI配置（必需）
ZHIPU_API_KEY=你的智谱API密钥
ZHIPU_BASE_URL=https://www.aiping.cn/api/v1
ZHIPU_MODEL=GLM-5
```

获取智谱API Key：https://open.bigmodel.cn/

### 访问地址

启动后访问：http://localhost:5000

## 项目结构

```
src/
├── app/                          # Next.js App Router
│   ├── page.tsx                  # 首页
│   ├── layout.tsx                # 根布局
│   ├── globals.css               # 全局样式
│   └── api/                      # API路由
│       ├── chat/route.ts         # 对话API（智谱GLM-5，流式输出）
│       ├── resume/route.ts       # 简历上传解析
│       └── jobs/                 # 岗位相关API
│           ├── import/route.ts   # 岗位数据导入
│           └── search/route.ts   # 岗位搜索（RAG语义搜索）
├── components/                   # React组件
│   ├── ui/                       # shadcn/ui基础组件
│   ├── LandingPage.tsx           # 首页落地页
│   └── chat/                     # 聊天相关组件
│       ├── Chat.tsx              # 聊天主界面
│       └── ChatMessage.tsx       # 消息组件
├── lib/                          # 工具函数
│   └── utils.ts                  # cn()等工具
└── types/                        # TypeScript类型定义
    └── chat.ts                   # 聊天相关类型
```

## API接口

| 接口 | 方法 | 说明 |
|------|------|------|
| `/api/chat` | POST | AI对话（SSE流式输出） |
| `/api/resume` | POST | 简历文件上传 |
| `/api/jobs/search` | POST | 岗位语义搜索（RAG） |

### 示例请求

**对话接口**
```bash
curl -X POST http://localhost:5000/api/chat \
  -H "Content-Type: application/json" \
  -d '{"messages":[{"role":"user","content":"你好"}]}'
```

**岗位搜索**
```bash
curl -X POST http://localhost:5000/api/jobs/search \
  -H "Content-Type: application/json" \
  -d '{"query":"前端开发","topK":5}'
```

## 已收录岗位

涵盖20+真实岗位，来自字节跳动、阿里巴巴、腾讯、美团、百度、小米、滴滴、网易、哔哩哔哩、快手、华为、蚂蚁集团、拼多多、商汤科技等公司：

| 类型 | 岗位示例 |
|------|----------|
| 技术开发 | 前端开发、Java后端、Python开发、Go开发、全栈开发 |
| 移动开发 | iOS开发、Android开发 |
| AI/算法 | AI算法工程师、机器学习工程师 |
| 数据方向 | 数据分析师、大数据开发工程师 |
| 产品运营 | 产品经理、产品运营、UI设计师 |
| 其他 | 测试工程师、安全工程师、运营专员 |
| 实习岗位 | 前端开发实习生、后端开发实习生、产品经理实习生 |

## 技术亮点

### 1. RAG检索增强

基于向量数据库的语义搜索，匹配真实岗位数据：

```
用户能力画像 → 向量检索 → 匹配真实岗位 → 精准推荐
```

### 2. 流式对话

使用SSE协议实现打字机式输出，提升用户体验。

### 3. 分步对话控制

严格的对话节奏控制，每轮只做一件事：
- 信息采集 → 能力画像 → 岗位推荐 → 成长计划

### 4. 隐私安全

- API Key存储在服务端环境变量
- 敏感信息不落盘

## 开发规范

### 包管理器

**必须使用 pnpm**：

```bash
# ✅ 正确
pnpm add package-name

# ❌ 错误
npm install package-name
yarn add package-name
```

### 组件开发

优先使用 shadcn/ui 基础组件：

```tsx
import { Button } from '@/components/ui/button';
import { Card, CardContent } from '@/components/ui/card';
```

### 样式开发

使用 Tailwind CSS + cn() 工具函数：

```tsx
import { cn } from '@/lib/utils';

<div className={cn("base-class", condition && "conditional-class")}>
```

## 构建与部署

### 构建

```bash
pnpm build
```

### 启动生产服务器

```bash
# macOS / Linux
pnpm start

# Windows
pnpm start:win
```

## 已知问题

| 问题 | 状态 | 解决方案 |
|------|------|----------|
| PDF简历解析 | 本地环境兼容性问题 | 建议手动描述简历内容或上传txt文件 |

## 参考文档

- [Next.js 官方文档](https://nextjs.org/docs)
- [shadcn/ui 组件文档](https://ui.shadcn.com)
- [智谱AI开放平台](https://open.bigmodel.cn/)
- [Tailwind CSS 文档](https://tailwindcss.com/docs)

## License

MIT
