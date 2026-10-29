# Android 设备原生 Linux 移植经验

整理日期：2026-10-29。本仓库记录一次 Redmi Note 11T Pro 原生 Ubuntu 移植尝试的成果、未完成项和教训，并记录转向已有 Xiaomi Pad 6 Pro 方案的安装前检查。

**这不是刷机包，也不是可以照着执行的通用刷机教程。**

## 当前结论

- Redmi Note 11T Pro（xaga / MT6895）项目已按用户要求暂停，尚未在本机启动或安装 Ubuntu。
- 已构建 Ubuntu 24.04 ARM64 + GNOME + ROS 2 Jazzy 离线候选，以及受限诊断 init 的内核；没有通过真机全功能验收的发行包。
- 最近完整工具回归为 WSL 179 项通过；Windows 179 项中 22 项 Linux 专用测试跳过。这个数字不是硬件功能通过数量。
- 当前改为核对已有的 Xiaomi Pad 6 Pro（liuqin / SM8475）Ubuntu 项目。上游存在真机测试过的发布，但只覆盖特定设备布局，不能推定另一台平板也已通过。
- 保留 Android、恢复准备和用户明确的阶段授权仍是要求；没有零风险刷机或保证救回任何硬砖的承诺。

## 文档

- [xaga 移植复盘与实际成果](docs/XAGA_RETROSPECTIVE.zh-CN.md)
- [Pad 6 Pro 安装前核对清单](docs/LIUQIN_PREINSTALL.zh-CN.md)

## 参考项目

- [MT6895-Mainline/linux](https://github.com/MT6895-Mainline/linux)
- [MT6895-Mainline/rootfs](https://github.com/MT6895-Mainline/rootfs)
- [MT6895-Mainline/initramfs](https://github.com/MT6895-Mainline/initramfs)
- [yzddmr6/android-mainline-port](https://github.com/yzddmr6/android-mainline-port)
- [yzddmr6/xiaomipad-6pro-mainline](https://github.com/yzddmr6/xiaomipad-6pro-mainline)
- [ilovecnxx/xiaomi13-fuxi-linux](https://github.com/ilovecnxx/xiaomi13-fuxi-linux)
- [KunBuFenZi/progt13-ubuntu](https://github.com/KunBuFenZi/progt13-ubuntu)

上述项目是参考，不代表本仓库作者已验证其所有功能或取得第三方固件的再分发权。
本仓库仅发布原创经验文档，不上传 ROM、设备校准、启动分区备份、账户信息或私有测试资料。

## 许可

原创文档采用 [CC BY 4.0](LICENSE.md)。第三方项目及其代码、固件、商标和文档保留各自的权利及许可。
