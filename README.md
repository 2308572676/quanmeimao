# 全媒猫 QuanmeiMao
全媒猫是一款面向自媒体博主、技术创作者与品牌内容团队的内容营销工具。 当你需要持续产出内容、希望作品被豆包、DeepSeek、千问等 AI 引擎推荐、又要铺到全网平台时， 它把 AI 创作、GEO 生成式引擎优化与多平台分发收在同一处：先用 AI 起草与改写，再经 GEO 诊断优化， 最后一键把文章、视频与音频分发到 39 个平台，从微信公众号、知乎、掘金到抖音、视频号、哔哩哔哩、喜马拉雅、小宇宙都能覆盖。 全程在你的浏览器本地完成，内容不上传、默认存草稿，不用再开几十个后台反复搬运。全媒猫官网：https://quanmeimao.lingzhutech.com

> AI 营销引流获客，一次创作分发全网。

全媒猫是一款面向**自媒体博主、技术创作者与品牌内容团队**的内容营销工具。它把内容营销链路上最费力的三件事做在了一起：

1. **AI 创作** —— 结合品牌名与官网信息起草母稿，再按各平台调性派生适配子稿；
2. **GEO 生成式引擎优化** —— 品牌诊断、站点诊断、关键词与投放矩阵，让内容更容易被豆包、DeepSeek、千问等 AI 引擎提及与引用；
3. **一键分发** —— 文章、短视频、播客音频一次排版、多平台同步发布。

内容数据全程留在用户自己的浏览器里，不上传服务端，默认存为草稿。

| | |
| --- | --- |
| 官网 | https://quanmeimao.lingzhutech.com |
| 当前版本 | v3.4.0 |
| 产品定位 | 浏览器扩展 + 官网工作台（Web 端 + 扩展端同构） |

---

## 一、仓库结构

本仓库是一个多端 monorepo，各目录可独立开发与部署。

| 目录 | 说明 | 技术栈 |
| --- | --- | --- |
| [frontend](frontend) | 官网：营销页面 + GEO 优化中心 + 立即同步台 + 推广中心 | Nuxt 3 / Vue 3 / Tailwind CSS |
| [extension](extension) | 浏览器扩展「全媒猫内容同步助手」 | Plasmo / Vue 3 / ant-design-vue |
| [trizEngine](trizEngine) | GEO 引擎与 Skill Agent 后端（GEO 创作、诊断、股票分析） | Python FastAPI / LangGraph |
| [geo-server](geo-server) | GEO 内容分发系统服务端（多 Agent 全自动流水线） | Spring Boot 2.4.4 / MyBatis-Plus / MySQL |
| [backend](backend) | 自建后端 MVP 骨架（会员登录态、媒体任务接口） | Spring Boot 2.7.18 |

> 主要业务代码集中在 `frontend` / `extension` / `trizEngine` / `geo-server` 四个目录

---

## 二、核心能力

### 1. AI 创作

- 由品牌名 + 官网地址起草「母稿」，再从母稿派生各平台适配的「子稿」；
- 内置平台字数上限、Markdown 语法支持度标注，发布前即可预知兼容性。

### 2. GEO 生成式引擎优化（官网 `/geo`）

工作台按左侧菜单分七个板块：

| 板块 | 作用 |
| --- | --- |
| 概览 | AI 收录潜力、覆盖关键词、分发平台与内容增长指标 |
| GEO 品牌诊断 | 评估品牌内容在生成式引擎中的可被提及与引用程度 |
| GEO 网站诊断 | 面向站点内容输出可被引用的优化建议 |
| SEO 诊断 | 标题、层级、关键词、内链等技术因素排查，先保障收录 |
| 母稿管理 | 按四类触发问句（推荐 / 评测 / 对比 / 场景）创作 GEO 内容 |
| 子稿管理 | 母稿派生的平台适配子稿与改写版本 |
| 投放矩阵 | 内容类型 × 平台投放策略（2 主力 / 1 辅助 / 0 不建议） |

### 3. 多平台一键分发（官网 `/sync`）

- 复用浏览器本地登录态直接发布，不托管账号密码；
- 内置网页智能阅读器，正文一键提取并转 Markdown；
- 支持本地 Markdown 文件夹导入、历史记录按文档归档与「追加平台」二次发布。

### 4. 分发平台覆盖

平台清单见 [frontend/data/sync-platforms.ts](frontend/data/sync-platforms.ts)：

| 内容类型 | 平台数 | 代表平台 |
| --- | --- | --- |
| 文章 / 图文 | 33 | 微信公众号、头条号、小红书、知乎、微博、百家号、企鹅号、抖音、快手、B 站、掘金、CSDN、博客园、雪球、东方财富号、Reddit 等 |
| 短视频 | 9 | 抖音、快手、哔哩哔哩、视频号、头条号、企鹅号、小红书、微博、知乎 |
| 播客音频 | 6 | 网易云音乐、QQ 音乐、喜马拉雅、蜻蜓 FM、荔枝 FM、小宇宙 |

去重后共覆盖 **39 个平台**（文章 / 视频 / 音频三类型合计）。

---

## 三、快速开始

### 3.1 前端官网（frontend）

```bash
cd frontend
npm install
npm run dev          # http://localhost:3000（端口占用时自动切 3001）
```

其他脚本：`npm run build` / `npm run generate` / `npm run preview` / `npm run typecheck`。

关键环境变量（均可在部署时覆盖）：

| 变量 | 说明 | 默认值 |
| --- | --- | --- |
| `NUXT_PUBLIC_SITE_URL` | 站点正式域名 | `https://quanmeimao.lingzhutech.com` |
| `NUXT_PUBLIC_API_BASE_V2` | `/v2/` 前缀接口域名（用户、短信、套餐） | `https://www.stockapi.com.cn` |
| `NUXT_PUBLIC_API_BASE` | 其余接口域名 | `https://quanmeimao.lingzhutech.com` |
| `NUXT_PUBLIC_API_BASE_TRIZ` | `/triz/` 前缀接口域名（GEO 引擎） | 开发 `http://localhost:8000`，生产 `https://trizengine.lingzhutech.com` |

站点品牌 / TDK / 版本等信息集中在 [frontend/data/site.ts](frontend/data/site.ts)，改动即同步影响全局 `head`、结构化数据与页脚。

### 3.2 浏览器扩展（extension）

```bash
cd extension
npm install
npm run dev          # plasmo dev，产物 build/chrome-mv3-dev
npm run build        # plasmo build，产物 build/chrome-mv3-prod
npm run package      # 打包发布zip
```

安装方式：浏览器「扩展程序」页开启开发者模式 → 「加载已解压的扩展程序」→ 选择对应 `build/` 产物目录。

扩展通过 `qmm_token` Cookie 与官网共享登录态，并使用 `chrome.cookies.onChanged` 实时监听登录 / 退出。

### 3.3 GEO 引擎后端（trizEngine/backend）

```bash
cd trizEngine/backend
pip install -r requirements.txt
uvicorn app.main:app --host 0.0.0.0 --port 8000 --reload
```

- 运行于 `http://localhost:8000`，所有业务接口以 `/triz` 为前缀；
- 接口文档 `/docs` 使用 HTTP Basic Auth 保护（账号密码在 `app/config.py` 中配置，请按环境自行调整）；
- 请求鉴权：`/triz` 下的接口会携带 token 调用业务后端 `GET /v2/user` 复核身份，未通过统一返回业务码 `60022`。

Windows 下也可使用脚本管理服务：

```powershell
./trizEngine/scripts/backend-start.ps1
./trizEngine/scripts/backend-stop.ps1
```

### 3.4 GEO 内容分发服务端（geo-server）

```bash
cd geo-server
# 首次：建表 + 字典初始化（含 16 张表 / 1 个视图）
mysql -h<host> -uroot -p <db> < src/main/resources/db/geo_schema.sql

mvn spring-boot:run           # 默认端口 8001
# 或
mvn clean package -DskipTests && java -jar target/geo-content-server-1.0.0.jar
```

七阶段流水线：`选题 → 检索 → 母稿 → 变体 → 合规 → 人工终审 → 发布`。人工终审是硬闸口，发布侧含五道真实门禁（标题分、跨平台相似度、母稿 GEO 分、平台红线、账号日配额）。

接口样例与端到端冒烟脚本见 `geo-server/http/geo-api.http`、`geo-server/http/smoke.sh`。

### 3.5 自建后端 MVP（backend）

```bash
cd backend
mvn spring-boot:run           # 默认端口 8080
```

为扩展提供会员登录态与媒体任务等基础接口（当前为 MVP 阶段，返回与插件消费端兼容的 mock 结构），跨域已放行 `localhost:3000` 与 `chrome-extension://*`。

---

## 四、接口约定（前端）

前端按**路径前缀**决定请求域名，规则见 [frontend/data/api.ts](frontend/data/api.ts)：

| 前缀 | 域名 | 用途 |
| --- | --- | --- |
| `/v2/` | stockapi 域名 | 用户登录、短信验证码、会员套餐 |
| `/triz/` | GEO 引擎域名 | GEO 诊断与创作、技能调用 |
| 其他 | 主站后端域名 | 其余业务接口 |

业务成功码为 `20000`；登录失效统一返回 `60022`，前端据此唤起登录框。

---

## 五、工程约定

- **视觉语言**：全站统一为「象牙纸 · 翡翠极光」——铺底细网格（`grid-paper`）+ 顶部极光光场（`glow-field`）；
- **移动端适配**：全站页面均需 H5 适配，屏幕宽度 < 768px 时基准字号由 125% 收敛至 100%，复杂宽表转为卡片流布局；
- **SEO**：页面标题由 `useSeo` 统一拼接（页面 title 内不写品牌名，避免重复），站点地图与 `robots.txt` 由 Nuxt server route 生成；
- **安全**：扩展复用浏览器本地登录态，不托管用户账号密码；仓库中的配置示例请勿填入真实生产密钥。

---

## 六、相关文档

- 浏览器扩展说明：[extension/README.md](extension/README.md)
- GEO 内容分发系统设计：[geo-server/README.md](geo-server/README.md)
- Skill Agent 后端说明：[trizEngine/README.md](trizEngine/README.md)
- 开源参考项目：`stockAapi-frontend/README.md`

各子目录遵循其自身的 LICENSE 与开源协议（扩展目录的授权信息见 [extension/LICENSE](extension/LICENSE)）。
