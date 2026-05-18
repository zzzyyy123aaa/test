# 项目上下文

## 项目概述
GEO 内容分发与优化系统管理平台，基于多 Agent 协同工作流（Planner/Executor/Reviewer），实现内容生成—评估—分发—反馈的闭环。

### 版本技术栈

- **Framework**: Next.js 16 (App Router)
- **Core**: React 19
- **Language**: TypeScript 5
- **UI 组件**: shadcn/ui (基于 Radix UI)
- **Styling**: Tailwind CSS 4
- **Database**: Supabase (PostgreSQL)
- **Charts**: Recharts
- **Icons**: Lucide React

## 目录结构

```
├── public/                 # 静态资源
├── src/
│   ├── app/                # 页面路由与布局
│   │   ├── page.tsx        # Dashboard 数据看板
│   │   ├── layout.tsx      # 全局布局（侧边栏 + 内容区）
│   │   ├── agents/         # Agent 管理页面
│   │   ├── tasks/          # 任务中心页面
│   │   ├── content/        # 内容管理页面
│   │   ├── ab-tests/       # A/B 测试页面
│   │   ├── analytics/      # 数据分析页面
│   │   ├── prompts/        # Prompt 管理页面
│   │   ├── api/            # 后端 API 路由
│   │   │   ├── seed/       # 种子数据初始化
│   │   │   ├── dashboard/  # 看板聚合数据
│   │   │   ├── agents/     # Agent CRUD
│   │   │   ├── tasks/      # 任务 CRUD
│   │   │   ├── content/    # 内容 CRUD
│   │   │   ├── ab-tests/   # A/B 测试 CRUD
│   │   │   ├── analytics/  # 分析数据查询
│   │   │   ├── prompts/    # Prompt CRUD
│   │   │   └── platforms/  # 平台管理
│   │   ├── globals.css
│   │   └── favicon.ico
│   ├── components/         # 公共组件
│   │   ├── app-sidebar.tsx # 侧边栏导航
│   │   └── ui/             # Shadcn UI 组件库
│   ├── hooks/              # 自定义 Hooks
│   ├── lib/                # 工具库
│   │   └── utils.ts        # 通用工具函数 (cn)
│   └── storage/database/   # 数据库
│       ├── supabase-client.ts  # Supabase 客户端
│       └── shared/schema.ts    # Drizzle 表定义
├── next.config.ts
├── package.json
└── tsconfig.json
```

## 数据库表

| 表名 | 说明 | 核心字段 |
|------|------|----------|
| agents | Agent 配置 | type (planner/executor/reviewer), status, config |
| tasks | 工作流任务 | type (topic_selection/content_generation/optimization/distribution), status, priority |
| content | 生成内容 | type (article/social_post/seo_page/video_script), platform, status, seo_keywords |
| ab_tests | A/B 测试 | content_id_a/b, clicks/conversions, winner, confidence |
| content_analytics | 内容分析数据 | impressions, clicks, conversions, click_rate, conversion_rate |
| prompts | Prompt 模板 | category, agent_type, template, variables, performance_score |
| platforms | 分发平台 | code, is_active, daily_limit |

## 包管理规范

**仅允许使用 pnpm**，常用命令：
- 安装依赖：`pnpm install`
- 添加依赖：`pnpm add <package>`
- 移除依赖：`pnpm remove <package>`

## API 路由

| 路由 | 方法 | 说明 |
|------|------|------|
| /api/seed | POST | 初始化种子数据 |
| /api/dashboard | GET | 看板聚合数据 |
| /api/agents | GET/PATCH | Agent 查询与更新 |
| /api/tasks | GET/POST/PATCH | 任务查询、创建与更新 |
| /api/content | GET/POST/PATCH/DELETE | 内容 CRUD |
| /api/ab-tests | GET/POST/PATCH | A/B 测试 CRUD |
| /api/analytics | GET | 分析数据查询 |
| /api/prompts | GET/POST/PATCH/DELETE | Prompt CRUD |
| /api/platforms | GET/PATCH | 平台管理 |

## 开发规范

- TypeScript strict 模式，禁止隐式 any
- Supabase SDK 操作必须检查 `{ data, error }` 并 throw
- 字段名统一 snake_case
- 所有 CRUD 的 delete/update 必须带 filter
- 前端页面使用 'use client' + useEffect + useState 模式

## Hydration 问题防范

1. 严禁在 JSX 中直接使用 typeof window、Date.now()、Math.random()
2. 使用 'use client' + useEffect + useState 确保客户端渲染
3. 禁止使用 head 标签，使用 metadata
