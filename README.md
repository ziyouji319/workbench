# 工作台（灵感库 + 项目管理器 + 云同步）

这是由 Codex 为用户独立维护的网站项目（不再依赖 WorkBuddy）。
以后用户有新需求，直接在 Codex 对话里提出即可；在此目录中修改并重新部署。

## 文件说明
- `index.html` — 网站本体（所有功能、样式、逻辑都在单文件内）
  - 当前版本：v14（同步已修复）
  - 云端存储：Supabase 单行全量快照，本部署私有数据行 id=`wb_c725b68ea6a8a664`
  - 默认使用内置公共 Supabase（anon 可读写），建议用户按云同步页教程换用自己的项目
- `sw.js` — service worker（v14，已修复：只缓存同源静态资源，绝不缓存跨域 API 请求）
- `manifest.json` / `icons/` — PWA 清单与图标
- `.nojekyll` — 供 GitHub Pages 部署时跳过 Jekyll 处理

## 部署
- 本地预览：`ruby -run -ehttpd . -p 8896` → http://localhost:8896/
- GitHub Pages：把本目录推送到 GitHub 仓库，在仓库 Settings → Pages 选择分支启用（已含 .nojekyll，相对路径，可直接用子路径）
- Netlify：把本目录拖到 https://app.netlify.com/drop

## 修改要点（未来维护时注意）
- 改功能 → 改 `index.html` 内对应 JS/HTML/CSS，版本号 `APP_VERSION_NUM` 递增
- 改离线缓存 → 改 `sw.js` 时**务必同时把 `CACHE` 名称递增**（如 workbench-v14 → workbench-v15），否则旧缓存不更新
- 绝不恢复旧版 sw.js 的“缓存所有 GET”逻辑（会把云端接口也缓存，导致手机/电脑不同步）
- 部署新版本后，用户端需要刷新两次让新版 service worker 接管

## 数据安全
- 云端数据在 Supabase 的 `workbench_data` 表（内置项目 lfbnmalkcaceacqgojkt.supabase.co）
- 强烈建议用户创建自己的 Supabase 项目并在两端填写相同 URL/key
