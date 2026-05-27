# OpenClaw Android Builder

自动编译 OpenClaw 官方 Android APK 的 GitHub Actions 仓库。

## 自动行为

每 **6 小时**自动检查 OpenClaw 官方仓库是否有新 commit，如果有则：

1. 拉取最新源码
2. 使用固定 release keystore 编译 `thirdParty` 和 `play` 两个 flavor 的 signed APK
3. 创建 GitHub Release 并附带 APK 下载

Release tag 格式：`v<OpenClaw version>-<short sha>`（如 `v2026.5.26-abcdef0`）

## GitHub Secrets

为了保证每次更新都能覆盖安装，必须在仓库 Settings → Secrets and variables → Actions 里配置同一把签名：

- `OPENCLAW_ANDROID_KEYSTORE_B64`：release keystore 文件的 base64 内容
- `OPENCLAW_ANDROID_STORE_PASSWORD`：keystore 密码
- `OPENCLAW_ANDROID_KEY_ALIAS`：key alias
- `OPENCLAW_ANDROID_KEY_PASSWORD`：key 密码

示例生成方式：

```bash
keytool -genkeypair \
  -v \
  -storetype PKCS12 \
  -keystore openclaw-android-release.keystore \
  -alias openclaw-einck \
  -keyalg RSA \
  -keysize 4096 \
  -validity 10000 \
  -dname "CN=Einck OpenClaw Android,O=Einck,OU=OpenClaw,C=CN"

base64 -w 0 openclaw-android-release.keystore
```

> 注意：如果手机上现在安装的是 GitHub Actions 旧 debug 签名包，第一次切换到 release 签名包仍然需要卸载一次。之后只要 Secrets 不变，就可以直接覆盖安装。

## 手动触发

也可以手动触发编译：

1. 打开仓库的 **Actions** 页面
2. 选择 **Build & Release OpenClaw Android APK**
3. 点击 **Run workflow**
4. 参数：
   - `ref`：OpenClaw 的分支或 tag，默认 `main`
   - `force`：是否强制编译（即使没有新 commit）

## APK 说明

- `openclaw-<version>.apk`：`thirdPartyRelease`，侧载版，保留完整能力/权限，推荐自己测试用
- `openclaw-play-<version>.apk`：`playRelease`，更接近 Google Play 版
- `<version>` 取自 OpenClaw 源码的 `versionName`，如 `2026.5.26`

## 安装

如果手机已安装 Play 版 `ai.openclaw.app` 或旧 debug 签名包，需要先卸载一次：

```bash
adb uninstall ai.openclaw.app
adb install openclaw-2026.5.26.apk
```

之后 GitHub Actions 新构建的同签名包可以直接覆盖安装：

```bash
adb install -r openclaw-2026.5.27.apk
```

## 为什么没有预编译 APK？

OpenClaw 官方目前不在 GitHub Release 里放 Android APK，只通过 Google Play 分发（且 Play 版本落后于源码 main 分支）。这个仓库用 GitHub Actions 从源码自动编译最新版。
