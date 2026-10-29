# 小米 Pad 6 Pro：安装前核对

更新于 2026-10-30。参考项目：[yzddmr6/xiaomipad-6pro-mainline v0.6.0](https://github.com/yzddmr6/xiaomipad-6pro-mainline/releases/tag/v0.6.0)。本文不是刷入授权或操作脚本；以下均非本机真机验收。

## 已核对的上游信息

- 目标仅为 Xiaomi Pad 6 Pro，代号 `liuqin`，SoC 为 SM8475；不能用于 Pad 6、6S Pro 或其他型号。
- 上游发布提供 Ubuntu 26.04 / GNOME、匹配 boot 与 installer、分片 rootfs、安装程序及校验清单。
- 上游在 256 GB 机型有真机验证；其指南允许符合规则的 128 GB 和 512 GB 原厂布局，但没有逐一真机验证。实际 GPT/扇区大小/容量必须核实，自定义布局会被拒绝。
- **首次安装会擦除用户数据并重新划分磁盘尾部。** 双系统保留 Android 不等于保留现有 Android 数据和设置。
- 双系统不是“随便刷 B 槽”：该方案两个系统从 A 槽启动，通过回读校验后替换 `boot_a` 来切换；Android 返回 Ubuntu 的 KernelSU 路线有具体测试范围和限制。
- Android 恢复仍需本机独立核验。社区某台设备成功，不代表另一台相同型号的恢复已通过。

## 双系统不等于只改 Linux 分区

按上游固定 v0.6.0 安装器和 ROM 镜像表静态复核：首次双系统安装先擦除 `userdata`、`metadata` 头部，再改 GPT；固定 ROM `OS2.0.6.0.VMYCNXM` 的 sparse `super` **每次都会刷入**，而 `bluetooth_a`、`modem_a`、`dsp_a`、`vendor_boot_a`、`dtbo_a`、`vbmeta_a`、`vbmeta_system_a` 在内容或尾部与固定镜像不同时也会刷入。项目 boot 则写两槽，最后激活 A 槽。`--android-boot` 只更换存入 Ubuntu 切换仓库的 Android boot，不能阻止上述 ROM 写入。

安装器默认备份本机两槽 boot、persist、GPT，并按需备份前三个通信/无线固件分区；**没有自动备份 `super` 及其余四个条件刷写的 Android 分区**。GPT 改写失败时错误信息要求操作者另行恢复，程序不会自动回滚；恢复入口本身仍依赖 RAM 安装器能启动并识别 `persist` 分区，恢复 GPT 也恢复不了已擦除的 Android 数据或已刷写的 `super`。因此下载固定 ROM、拿到 `--check` 成功或提供 Android boot 都不足以证明当前 Android 仍可启动或可退回原版本。当前设备的 ROM、防回滚/AVB、镜像与恢复路径必须单独核对，不可用旧包直接降级。

## 实机接入后先只读核对

1. 明确唯一目标序列号，确认产品确实为 liuqin；后续设备命令始终指定目标，不接触其他手机。
2. 核对容量、磁盘实际 sector/LBA、GPT、活动槽和当前 ROM，不能只看商品标称 256 GB。
3. 核对 BL 解锁状态和必要读取权限；没有这些权限时先解释取证方案，不擅自安装 Root 或启动 recovery。
4. 准备匹配当前 Android 版本的原厂 ROM、boot 和必要恢复镜像；覆盖前另行备份并复核本机 persist/GPT 等设备专属材料，不使用他机校准。
5. 个人数据擦除的单独授权不等于恢复分区备份豁免，也不代表已经确认具体安装阶段。
6. 明确 Linux/Android 空间分配、上述全部刷写分区、双系统切换方法和实际回退入口，核对完整版本包与 SHA256。
7. 再决定是否进入授权的安装阶段。不能绕过安装器布局检查或把 `--yes` 当成安全验证。

## 硬件限制必须提前说清楚

上游有桌面、显示、触控、GPU、Wi-Fi、扬声器、单麦录音、旋转、部分 USB、指纹及 DP 显示测试证据。
但前置相机和自动亮度仍不支持；后置相机、挂起恢复及快充存在限制，手写笔、部分传感器、多麦、热点及部分视频/USB 场景未完整验证。
原生指纹目前限首个账号与一枚指纹。最终以所用发布版本的完整支持表及本机验收为准。

**因此，现成方案也不满足“原生 Linux 所有功能已经完整”的承诺。** 若仍要求补齐全部功能，应把这些缺口作为后续开发任务，而不是静默删掉；Android 共存仅提供另一个使用环境。

## ROS 2 版本匹配

上游系统为 Ubuntu 26.04（resolute），不能照搬为 Ubuntu 24.04（noble）配置的 ROS 2 Jazzy 源。
官方 [rosdistro lyrical/distribution.yaml](https://github.com/ros/rosdistro/blob/master/lyrical/distribution.yaml) 的 Ubuntu 发布目标包含 resolute。已离线验签官方 ARM64 包索引并校验所需 ROS 2 Lyrical 软件包缓存；这不代表在本机已安装或运行 ROS。

验收至少覆盖 C++/Python 节点互通、桌面工具所需图形环境、跨设备 DDS 联网以及重启后的环境配置。模拟器测试不能替代这些真机结果。
