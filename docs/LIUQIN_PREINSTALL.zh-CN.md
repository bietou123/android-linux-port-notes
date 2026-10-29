# 小米 Pad 6 Pro：安装前核对

2026-10-29。参考项目：[yzddmr6/xiaomipad-6pro-mainline v0.6.0](https://github.com/yzddmr6/xiaomipad-6pro-mainline/releases/tag/v0.6.0)。本文不是刷入授权或操作脚本。

## 已核对的上游信息

- 目标仅为 Xiaomi Pad 6 Pro，代号 `liuqin`，SoC 为 SM8475；不能用于 Pad 6、6S Pro 或其他型号。
- 上游发布提供 Ubuntu 26.04 / GNOME、匹配 boot 与 installer、分片 rootfs、安装程序及校验清单。
- 上游安装器针对已知 256 GB 分区布局，其他容量和修改过的布局不能推定兼容。
- **首次安装会擦除用户数据并重新划分磁盘尾部。** 双系统保留 Android 不等于保留现有 Android 数据和设置。
- 双系统不是“随便刷 B 槽”：该方案两个系统从 A 槽启动，通过回读校验后替换 `boot_a` 来切换；Android 返回 Ubuntu 的 KernelSU 路线有具体测试范围和限制。
- Android 恢复仍需本机独立核验。社区某台设备成功，不代表另一台相同型号的恢复已通过。

## 实机接入后先只读核对

1. 明确唯一目标序列号，确认产品确实为 liuqin；后续设备命令始终指定目标，不接触其他手机。
2. 核对容量、磁盘实际 sector/LBA、GPT、活动槽和当前 ROM，不能只看商品标称 256 GB。
3. 核对 BL 解锁状态和必要读取权限；没有这些权限时先解释取证方案，不擅自安装 Root 或启动 recovery。
4. 准备匹配的原厂 ROM、boot 和 persist 等必要恢复材料；本机校准与设备地址必须来自同一平板。
5. 确认平板数据备份范围及擦除授权。其他手机上取消个人备份的决定，不自动转移为本平板的擦除许可。
6. 明确 Linux/Android 空间分配、双系统切换方法和实际回退入口，核对完整版本包与 SHA256。
7. 再决定是否进入授权的安装阶段。不能绕过安装器布局检查或把 `--yes` 当成安全验证。

## 硬件限制必须提前说清楚

上游有桌面、显示、触控、GPU、Wi-Fi、扬声器、单麦录音、旋转、部分 USB、指纹及 DP 显示测试证据。
但前置相机和自动亮度仍不支持；后置相机、挂起恢复及快充存在限制，手写笔、部分传感器、多麦、热点及部分视频/USB 场景未完整验证。
原生指纹目前限首个账号与一枚指纹。最终以所用发布版本的完整支持表及本机验收为准。

**因此，现成方案也不满足“原生 Linux 所有功能已经完整”的承诺。** 若仍要求补齐全部功能，应把这些缺口作为后续开发任务，而不是静默删掉；Android 共存仅提供另一个使用环境。

## ROS 2 版本匹配

上游系统为 Ubuntu 26.04（resolute），不能照搬为 Ubuntu 24.04（noble）配置的 ROS 2 Jazzy 源。
官方 [rosdistro lyrical/distribution.yaml](https://github.com/ros/rosdistro/blob/master/lyrical/distribution.yaml) 的 Ubuntu 发布目标包含 resolute；后续核对 ROS 2 Lyrical 的 ARM64 软件包和官方安装说明，再配置运行环境。

验收至少覆盖 C++/Python 节点互通、桌面工具所需图形环境、跨设备 DDS 联网以及重启后的环境配置。模拟器测试不能替代这些真机结果。
