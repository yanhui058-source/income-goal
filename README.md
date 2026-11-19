# IncomeGoal PWA（收入目标 网页 App）

「年度收入目标 + 轻量记账」的纯本地网页应用。免费、无需上架、无需签名，添加到主屏幕后像原生 App 一样全屏使用。

- 全部数据只存在手机浏览器的 localStorage，不联网、不上传
- 无任何依赖、无构建步骤，四个静态文件即可部署
- 覆盖原生版全部功能：引导、首页、明细、流水增删改、年度/月度目标、月度详情与复盘、年度复盘、达标口径切换、币种显示、JSON 备份恢复

## 文件结构

```
income-goal-pwa/
├── index.html            # 全部 UI 与业务逻辑（单文件）
├── manifest.webmanifest  # PWA 清单（添加到主屏幕、全屏、图标）
├── sw.js                 # Service Worker（离线可用，需 HTTPS）
└── icons/                # 180/192/512 图标
```

## 装到 iPhone 上使用

### 方式一：GitHub Pages（推荐，免费 + 永久 + 可离线）

1. 在 GitHub 创建一个仓库（Public）。
2. 把 `income-goal-pwa` 目录里的全部文件上传（网页界面拖拽上传即可）。
3. 仓库 Settings → Pages → Source 选 `main` 分支，保存。
4. 等待约 1 分钟，得到 `https://<用户名>.github.io/<仓库名>/`。
5. iPhone Safari 打开该地址 → 底部「分享」→「添加到主屏幕」→ 完成。
6. 以后从主屏幕图标打开，全屏运行；HTTPS 下 Service Worker 会缓存页面，无网也能用。

### 方式二：局域网临时访问（先用起来试试）

Mac 与手机连同一 Wi-Fi，在本目录运行：

```bash
cd income-goal-pwa
python3 -m http.server 8765
```

手机 Safari 打开 `http://<Mac 的局域网 IP>:8765`（Mac 设置 → Wi-Fi 可查 IP）。此方式仅适合临时体验；正式长期使用建议方式一（局域网 IP 会变，且 HTTP 下无离线缓存）。

## 注意事项

- 数据按浏览器和域名隔离：从不同地址（如局域网 IP 与 GitHub Pages）打开是两份独立数据，认准一个入口使用。
- 换手机/换浏览器前，先在「我的 → 导出备份」保存 JSON，再到新环境「从文件恢复」。
- localStorage 理论容量约 5MB，足够存十几万条流水；但 iOS 在「网站数据」极低空间时可能清理，重要数据请定期导出备份。
- 清理 Safari 网站数据会清空 App 数据，操作前务必先导出。
- 月底本地推送提醒在网页形态下不可用；次月首次打开时会自动弹出上月复盘替代。

## 开发说明

金额以整数「分」存储与计算，无浮点误差；目标分配尾差计入 12 月；修改年度目标默认保留过去月份、差额分摊到当前及未来月份。统计口径、状态规则与 `income-target-ios-development-spec.md` 一致。
