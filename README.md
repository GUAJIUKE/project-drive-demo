# 项目云盘演示

访问地址：https://guajiuke.github.io/project-drive-demo/

默认以“林悦 · 项目负责人”体验，无需真实账号。点击右上角“演示 · 模拟数据” → “体验成员” → “admin · 终极管理员”，再点击“admin 存储管理”，可查看 A/B 盘模拟容量、项目占用和项目文件表格。刷新恢复初始状态。

所有容量、状态、权限和文件记录都是模拟数据，不连接生产后端。下载仅生成 TXT 说明或清单，不包含真实文件；本地文件只读取名称和大小。模拟身份切换不是权限隔离。保留 A/B 盘备份演示，不包含异地备份。

## 构建与更新

这是仅存放静态构建产物的发布仓库，不包含 tasks 源码、服务配置或业务数据。原项目的独立入口仍是 `frontend/web_app/demos/project-drive/vite.config.js`，不使用正式应用的构建或部署脚本。

在有原项目及现有依赖的开发环境执行：

```bash
cd frontend/web_app
node --test demos/project-drive/feedback.test.js demos/project-drive/adminModel.test.js
PROJECT_DRIVE_BASE=/project-drive-demo/ npm exec -- vite build --config demos/project-drive/vite.config.js
PROJECT_DRIVE_BASE=/project-drive-demo/ npm exec -- vite preview --config demos/project-drive/vite.config.js
```

验收后，将 `demos/project-drive/dist/` 中的文件同步到本仓库 `site/`，清除 `site/assets/` 中旧构建的哈希文件，只提交本次产物。不要复制整个项目目录、public 目录、上传文件、环境变量或 source map。

推送 `main` 会触发 [Deploy project drive demo](https://github.com/GUAJIUKE/project-drive-demo/actions/workflows/deploy-demo.yml)。也可在 Actions 选择此工作流 → Run workflow → main，手动重新发布当前产物。Pages 的 Source 使用 GitHub Actions；工作流仅上传 `site/`，使用 `github-pages` 环境。构建发生在原项目环境，Actions 负责检查并发布产物，不从私有源码仓库拉取源码。

发布后应确认工作流的 deploy 成功，再打开首页并刷新，检查头像、CSS、文件操作和身份切换。此工作流与正式 tasks 发布流程无关联。

## 发布验证（2026-09-30）

[首次部署成功](https://github.com/GUAJIUKE/project-drive-demo/actions/runs/36660651752)。独立 Vite 构建和 12 项模拟测试通过；桌面浏览器检查 1920×1080、1366×768，验证首页与刷新、项目切换、普通成员与 admin 展示边界、容量异常、重命名、移动和替换。普通说明、所选清单及全部清单下载均已操作并检查 TXT 内容；全部下载不受搜索条件影响。线上 HTML、JS、CSS 和 WebP 均返回 HTTP 200，控制台无 error/warn。未访问或测试生产后端。
