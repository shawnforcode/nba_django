# 开发蓝图：Next.js + 本地 SQLite (现代化架构)

# 目标描述
将原 Django 项目重构为基于 **Next.js** 的现代化 Web 应用。该应用将在本地运行，直接读取位于 `NBAGameInsight` 目录下的 SQLite 数据库，提供极致的 UI 体验及响应速度。

## 用户审查
> [!IMPORTANT]
> **运行环境**: 需要安装 Node.js (推荐 v18+)。
> **数据访问**: Next.js 后端将直接读取 `C:\Users\ticbel\PycharmProjects\NBAGameInsight\data\database` 下的数据库文件。
> **原有代码**: 该操作将用新的 Next.js 项目替换原有的 Django 代码 (建议先备份原有 `nba_django` 文件夹)。

## 架构设计
- **框架**: Next.js 14 (App Router)
- **语言**: TypeScript (强类型保证数据库模式安全)
- **样式**: Tailwind CSS + Shadcn/ui (实现 "Premium" 高级感设计)
- **数据层**: `better-sqlite3` (高性能 Node.js SQLite 驱动)
- **部署**: 本地 `npm run dev` 启动，外网访问可选 Cloudflare Tunnel。

## 开发步骤

### 第一阶段：环境搭建与初始化
1.  **清理旧项目**: 备份并清空现有 `nba_django` 目录，或在其中创建新的 `web` 目录。
2.  **初始化 Next.js**:
    ```bash
    npx create-next-app@latest . --typescript --tailwind --eslint
    ```
3.  **安装依赖**:
    ```bash
    npm install better-sqlite3 clsx tailwind-merge lucide-react
    npm install -D @types/better-sqlite3
    ```
4.  **配置 Shadcn UI** (组件库):
    ```bash
    npx shadcn-ui@latest init
    ```

### 第二阶段：数据接入层 (Data Layer)
1.  **定义类型 (Types)**: 根据 `game.db`, `nba.db`, `videourl.db` 的 Schema 编写 TypeScript 接口 (Interfaces)。
    - `interface Game { ... }`
    - `interface Player { ... }`
    - `interface VideoUrl { ... }`
2.  **数据库连接器 (DbClient)**:
    - 编写 `lib/db.ts`，初始化 **只读** 模式的 `better-sqlite3` 实例，连接到绝对路径数据库。
3.  **Repository 模式**:
    - 创建 `lib/repos/games.ts`: 编写 SQL 查询获取比赛列表。
    - 创建 `lib/repos/players.ts`: 获取球员信息。
    - 创建 `lib/repos/videos.ts`: 关联查询视频链接。

### 第三阶段：核心页面开发
1.  **布局 (Layout)**:
    - 设计全局侧边栏/顶部导航 (Glassmorphism 玻璃拟态风格)。
    - 配置深色模式 (Dark Mode)。
2.  **首页/仪表盘 (Dashboard)**:
    - 展示 "今日焦点" 比赛卡片。
    - 数据概览 (使用 Recharts 绘制图表)。
3.  **比赛中心 (Game Hub)**:
    - **列表视图**: 带有过滤器的比赛列表 (按日期、球队)。
    - **详情视图**:
        - 记分板 (Scoreboard)。
        - 关键事件时间轴 (Timeline)。
        - **视频播放**: 集成 HTML5 Video Player，直接播放数据库中的 mp4 链接。

### 第四阶段：视觉打磨 (Polishing)
1.  **动效**: 使用 `framer-motion` 添加页面切换和加载动画。
2.  **字体**: 引入现代无衬线字体 (如 Inter 或 Geist Sans)。
3.  **SEO**: 虽是本地应用，但完善 Meta 标签有助于浏览器正确索引书签。

## 验证计划
1.  启动开发服务器: `npm run dev`
2.  测试数据库读取速度。
3.  验证视频播放流畅度。
