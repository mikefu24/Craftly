# Craftly — AI 应用构建器

类 Lovable 的对话式应用构建器：描述需求 → AI 生成完整单文件应用 → 沙盒实时预览 → 对话迭代修改。支持语音输入、PWA 离线安装。

**本仓库不包含任何 API key。** 首次使用时点击首页的「⚙ AI 引擎」徽章，选择服务商预设（DeepSeek / Agnes AI / 自定义 OpenAI 兼容服务），填入你自己的 API key（仅保存在本机浏览器 localStorage，不会上传）。不配置 key 时为本地模拟演示模式。

## 四种产物

### 1. 网页版（GitHub Pages 自动部署）
推送到 `main` 后，`Deploy Web` workflow 自动把 `www/` 发布到 GitHub Pages。
首次需在仓库 **Settings → Pages → Source** 选择 **GitHub Actions**。
手机浏览器打开后「添加到主屏幕」即为 PWA 离线应用。

### 2. Android APK
`Build Android APK` workflow 在 CI 中用 Capacitor 生成原生工程并编译 debug APK：
- 每次推送后在 **Actions → Build Android APK → Artifacts** 下载 `Craftly-APK`
- 打 tag（如 `v1.0.0`）会自动附到 GitHub Release
- debug 签名，可直接安装（需允许「安装未知来源应用」）；上架 Google Play 需自行配置 release 签名

### 3. iOS IPA（未签名）
`Build iOS IPA` workflow 在 macOS runner 上编译未签名 IPA：
- 在 Artifacts 下载 `Craftly-IPA-unsigned`
- 未签名 IPA **不能直接安装**，需通过 AltStore / Sideloadly 等工具用你的 Apple ID 重签名侧载；上架 App Store 需 Apple Developer 账号与正式签名

### 4. macOS DMG（未签名）
`Build macOS DMG` workflow 在 macOS runner 上用 Electron + electron-builder 把 `www/` 打包成桌面应用并生成 `.dmg`：
- 每次推送后在 **Actions → Build macOS DMG → Artifacts** 下载 `Craftly-DMG-unsigned`
- 打 tag（如 `v1.0.0`）会自动附到 GitHub Release
- 未签名/未公证，首次打开需在 **系统设置 → 隐私与安全性** 点「仍要打开」，或右键 App 选「打开」；分发他人或上架需 Apple Developer 签名 + 公证（notarization）
- 桌面壳代码在 `desktop/`（`main.js` + `package.json`）；本地有 macOS 时可 `cd desktop && cp -r ../www www && npm install && npm run dist:mac`

## 本地开发

直接用浏览器打开 `www/index.html` 即可（语音识别与 Service Worker 需 HTTPS 或 localhost）。

```bash
# 本地起服务
cd www && python3 -m http.server 8000
```

## 技术栈

单文件 HTML/CSS/JS（零依赖）· PWA（manifest + Service Worker）· Capacitor 6（移动原生壳）· Electron（macOS 桌面壳）· GitHub Actions（CI/CD）
