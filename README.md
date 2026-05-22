# OpenClaw Android Builder

自动编译 OpenClaw 官方 Android APK 的 GitHub Actions 仓库。

## 自动行为

每 **6 小时**自动检查 OpenClaw 官方仓库是否有新 commit，如果有则：
1. 拉取最新源码
2. 编译 `thirdParty` 和 `play` 两个 flavor 的 debug APK
3. 创建 GitHub Release 并附带 APK 下载

Release tag 格式：`v<OpenClaw version>`（如 `v2026.5.21`）

## 手动触发

也可以手动触发编译：

1. 打开仓库的 **Actions** 页面
2. 选择 **Build & Release OpenClaw Android APK**
3. 点击 **Run workflow**
4. 参数：
   - `ref`：OpenClaw 的分支或 tag，默认 `main`
   - `force`：是否强制编译（即使没有新 commit）

## APK 说明

- `thirdPartyDebug`：侧载版，保留完整能力/权限，推荐自己测试用
- `playDebug`：更接近 Google Play 版，移除部分 Play 限制权限相关能力

## 安装

GitHub Actions 编出来的是 debug APK，不是 Google Play 签名版。
如果手机已安装 Play 版 `ai.openclaw.app`，需要先卸载再安装：

```bash
adb uninstall ai.openclaw.app
adb install openclaw-android-<version>-thirdParty-debug.apk
```

## 为什么没有预编译 APK？

OpenClaw 官方目前不在 GitHub Release 里放 Android APK，只通过 Google Play 分发（且 Play 版本落后于源码 main 分支）。这个仓库用 GitHub Actions 从源码自动编译最新版。
