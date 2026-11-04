# 小梦 · 住在手机里的 AI 桌宠

一个住在 Android 手机/平板里的 AI 桌宠：它看得见你的屏幕、点得动任何 App、会说话会听话。
这一版给它加了**随 APK 打包的离线语音包**（拔了网线也能开口），并把默认形象重画了一遍。

**介绍与下载页：** https://qianqianlaifeng.github.io/xiaomeng-pet/
**安装包（Releases）：** https://github.com/qianqianlaifeng/xiaomeng-pet/releases

## 这是什么

- 上游项目：[kuafuai/andee](https://github.com/kuafuai/andee)（© 2026 KUAFUAI，Apache-2.0 + Part A 附加条款）
- 本站点分发的是它的**修改版**，改动清单见 [Releases 里的 NOTICE.md](https://github.com/qianqianlaifeng/xiaomeng-pet/releases/download/v0.9.1-xiaomeng/NOTICE.md)
- 按上游许可证 A.1(a)，**产品名、图标、版权信息保持上游原样**：桌面图标仍然叫 Andee
- 「小梦」是**这只宠物的名字**（应用本身支持给宠物起名），不是产品名
- 本仓库不是 KUAFUAI 官方发布

## 要求

- Android 11 及以上（`minSdk 30` 是承重的：`AccessibilityService.takeScreenshot` 与 `ACTION_IME_ENTER` 都是 API 30+）
- 需要自备 OpenAI 兼容的大模型接口，或走官方的 CodeFlying 一键路线
- 需要授予悬浮窗 + 无障碍服务；不授无障碍它只能聊天，干不了活

## 自己构建

**本仓库只放站点文件**（`index.html` + `assets/`）。源码 = 上游仓库 + 本版改动补丁：

1. `git clone https://github.com/kuafuai/andee`
2. 按 [Releases 里的 `xiaomeng-changes.patch`](https://github.com/qianqianlaifeng/xiaomeng-pet/releases/download/v0.9.1-xiaomeng/xiaomeng-changes.patch) 应用改动（21 个文件）
3. 补上补丁里不含的二进制配料（补丁头部写了来源与应有字节数）：
   - sherpa-onnx **v2.0.1** android tarball → `libsherpa-onnx-jni.so` + `libonnxruntime.so`（arm64-v8a / armeabi-v7a / x86_64）放进 `app/src/main/jniLibs/<abi>/`
   - `csukuangfj/vits-zh-aishell3` → 7 个文件放进 `app/src/main/assets/tts-model/`，其中 `vits-aishell3.int8.onnx` 应为 **39,870,124 字节**（下残缺文件会得到 26 MB 的坏包，引擎报 `Protobuf parsing failed`）

```bash
# 需要 JDK 17 与 Android SDK（platform 34 + build-tools 34.0.0）
./gradlew :app:assembleDebug
# 产物：app/build/outputs/apk/debug/app-<abi>-debug.apk
```

Windows 上工程路径不能含非 ASCII 字符，否则 AGP 会拒绝构建。

## 站点文件

`index.html` + `assets/voices/*.wav`（六个音色的试听样本，用 APK 里同一份模型在 PC 上直接跑出来的原声）。
GitHub Pages 从仓库根目录发布。
