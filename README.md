# DshanPi A1 (RK3576) — Android 14 TV + GMS 固件

东山派 A1（RK3576 / 8GB LPDDR4X / 256GB eMMC）的 Android 14 **Android TV** 固件，内置 Google 服务（GMS / Play 商店 / GSF / Android TV Launcher）。

## 固件文件

- 名称：`update-dshanpi_a1-ATV-GMS-20261016.img`
- 大小：2,070,991,491 字节（1.93 GiB）
- MD5：`08404FBEBA4296255325C9683AA9F7AC`
- SHA256：`544096b6d93baf383e0ba2dc95b4a0300e47210903b414315f51b5d9d44400b1`
- 压缩包：`update-dshanpi_a1-ATV-GMS-20261016.7z`（756,280,512 字节，7z LZMA2）
  - MD5：`3AA21088F137188460BFD0D04AF2F775`
  - SHA256：`f7bd579b8ac9ece1a38725f0f1beefc8d378994a14feea6e989cb3f15f7bcac4`

## 烧录方法（RKDevTool）

1. RKDevTool v3.32 及以上；工具目录 `config.ini` 保持 `CLOSE_CHECK_IDB=TRUE`
2. 板子进入 MASKROM：按住 MASKROM 键 → 插入电源 → 2~3 秒后松开
3. 「升级固件」页签 → 「固件」选择 `.img` → 点「升级」，等待完成
4. 完成后长按电源键 10 秒重启

## 已验证项目（真机实测）

- `ro.build.characteristics=tv`，固件指纹 `rockchip/dshanpi_a1/dshanpi_a1:14/.../eng.builde.20261016.221423`
- 默认桌面为 `com.google.android.tvlauncher`（isDefault=true）
- Google 组件全部安装并运行：Play 商店 53.0.12-34、GMS 23.48.16、GSF、Katniss、Chromecast(mediashell)、SetupWraith
- Play 商店（TV 版）可打开并正常加载内容（HBO max / hulu / tubi 等）；登录账号可用
- DroidGuard 服务正常拉起，日志无「设备未认证」报错
- 32 位应用兼容：`debug.wm.disable_deprecated_abi_dialog=1` 已写入 `vendor/build.prop`，实测纯 32 位应用（B 站 TV 版）启动无 ABI 弹窗
- 硬件识别：8GB 内存（可用 7.7G）、/data 约 225G、千兆以太网、HDMI 输出、ADB

## 已知限制（重要）

- **没有 Widevine**（仅 ClearKey）→ Netflix / Disney+ / HBO 等 DRM 应用无法使用；这是固件层缺失，非配置问题
- Play 商店「设备认证」状态未经正式确认；如装受保护应用提示未认证，可用 GSF ID 到 <https://www.google.com/android/uncertified> 注册
  - 本机 GSF ID（十进制）：`3814548240114936854`（十六进制 `34efff69fea46c16`）
- HDR / 4K 输出未验证（受测试显示器限制：LG 27EA33 为 1080p SDR）
- GMS 来自 MindTheGapps 14.0.0 arm64 ATV（2024-05-23 构建），未验证 GMS 自更新

## 构建信息

- 底座：立创泰山派 3M（RK3576）Android 14 SDK
- 产品目录：`device/rockchip/rk3576/dshanpi_a1`（`TARGET_BOARD_PLATFORM_PRODUCT := atv`、`PRODUCT_CHARACTERISTICS := tv`）
- 内核 DTS：`rk3576-100ask-dshanpi-a1.dts`（U-Boot 沿用 SDK 的 `rk3576_defconfig`，loader `rk3576_spl_loader_v1.09.107.bin`）
- GApps：`device/rockchip/rk3576/dshanpi_a1/gapps/`（29 个模块，含 privapp 权限白名单）
- 编译需修补三处瑞芯微老代码：`audio.mk` 的 `USE_CUSTOM_AUDIO_POLICY` 仅对 box 生效、`external/rktoolbox/dr-g/Android.mk` 关 ELF 校验、预编译 APK 必须用 `android_app_import`

## 开源说明

本仓库仅分发构建产物与说明。固件内含 Google 移动服务（闭源授权件）与 Rockchip 专有二进制，仅供个人测试使用，请勿用于商业分发。

## 许可证

说明文档 MIT；固件内各组件版权归其各自所有者。
