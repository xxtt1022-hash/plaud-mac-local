# PLAUD Mac Local

自有 **PLAUD Note Pro → Mac** 本地录音管理、原语言转写与 Codex 纪要工作流。

这是一次实际设备调试整理出的研究项目，已在一台 Note Pro 和一台 Mac 上完成 **23.4 秒真实录音验收**，包含接收、解密、完整解码、试听、本地转写、自动纪要和桌面入口。长录音、其他固件和长期稳定性尚未验收。

## 阅读与下载

- [本次完整操作总结（Markdown）](PLAUD-Note-Pro-Mac本地录音工作流-操作总结-2026-10-19.md)
- [完整脱敏源码包（77 个文本文件，保留原目录结构）](plaud-mac-local-source-2026-10-19.zip)
- [源码包 SHA-256](SOURCE-SHA256.txt)

源码以 ZIP 归档提供。下载并解压后，先阅读包内 `plaud-mac-local/README.md`、`docs/SETUP.md` 和 `docs/验收边界.md`，再配置自己的依赖和设备身份。本仓库没有发布可直接安装的通用 App 或 APK。

## 这次实现的功能

1. Mac 严格选择自己的 Note Pro 与指定录音，通过 BLE 接收后保留设备原件。
2. 本地解密、Ogg 音轨校验与提取、完整音频解码，以及关联录音的本地资料库。
3. 网页查看设备容量快照，管理试听、标题、项目、标签、备注、搜索、校验和导出。
4. 核验入库后，在 Mac 用 whisper.cpp 转写原语言文字，再由当前已登录的 Codex 生成待复核纪要。
5. 双击 Mac 桌面图标打开 Chrome 管理页；服务未运行时自动启动，已运行时复用。

```text
Note Pro → Mac BLE 接收 → 本地音频与资料库 → whisper.cpp 转写
                                           → 文字交给 Codex → 纪要草稿
```

## 源码包内容

| 目录 | 内容 |
| --- | --- |
| `mac-adapter/` | 认证、接收、音频处理、资料库、管理页、处理队列与测试 |
| `android-bootstrap/` | 无 ADB 引导、连接诊断、加密候选导出源码 |
| `launcher/` | Swift 桌面入口和图标源码 |
| `scripts/` | 环境准备、合成测试和本机入口构建辅助 |
| `docs/` | 安装说明、完整复盘和验收边界 |

脱敏副本在本次已有 Python 环境和固定上游版本下独立重跑 **174 项合成测试全部通过，18.512 秒**。这不等于清洁系统安装、安卓重新构建、长录音或跨固件已通过。

## 数据和功能边界

- 音频在 Mac 本地转写；纪要会把标题与转写文字发送给当前已登录的 Codex / OpenAI，需要网络、登录和账户额度。
- 新录音仍需显式选择接收，没有无人值守自动下载。
- 设备原件保留，管理页不发送删除录音命令；独立备份和自动清理尚未配置。
- 当前接收上限 128 MiB，传输和解码分别限 120 秒；真实验收只有 23.4 秒。
- 转写和纪要都是待复核结果，说话人分离未实现。
- 不包含实际账号配置、JWT、Client Secret、设备私钥、接收身份、录音、文字正文、数据库、模型、SDK 二进制或编译 App。

## 来源

初始研究基础为 [Shawn-TKD/plaud-direct-pc-research](https://github.com/Shawn-TKD/plaud-direct-pc-research)，固定提交 `4f52d432b2f4d838ef10c7674d6b49f9bafc7005`。本地转写使用 [whisper.cpp](https://github.com/ggml-org/whisper.cpp)。源码包内保留许可与第三方说明，专有 PLAUD SDK AAR 需要自行按官方许可获取。

本项目为独立研究原型，未获 PLAUD 或 OpenAI 官方背书。仅用于自己拥有并获授权操作的设备。
