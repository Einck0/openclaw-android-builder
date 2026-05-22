# OpenClaw Android Builder

这个仓库只用于通过 GitHub Actions 编译 OpenClaw 官方 Android APK。

## 使用方法

1. 打开本仓库的 **Actions** 页面。
2. 选择 **Build OpenClaw Android APK**。
3. 点击 **Run workflow**。
4. 参数：
   - `ref`: OpenClaw 的分支或 tag，默认 `main`。
   - `flavor`: 默认 `thirdParty`，也可以选择 `play`。
5. 等 workflow 跑完后，在 run 页面底部下载 Artifacts。

## flavor 区别

- `thirdParty`: 侧载版，保留更多能力/权限，推荐自己测试最新版使用。
- `play`: 更接近 Google Play 版，会移除部分 Play 限制权限相关能力。

## 安装提示

GitHub Actions 编出来的是 debug APK，不是 Google Play 签名版。
如果手机已安装 Play 版 `ai.openclaw.app`，可能需要先卸载再安装。

```bash
adb uninstall ai.openclaw.app
adb install app-thirdParty-debug.apk
```
