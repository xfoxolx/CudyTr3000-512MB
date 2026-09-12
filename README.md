[定制教程](https://xiabee.eu.org/customize.html) | [刷写教程](https://xiabee.eu.org/install.html)

<div align=center>
<img src="tr3000.png" height=200px align="center">
</div>

---

## immortalwrt 源码

编译自 https://github.com/padavanonly/immortalwrt-mt798x-6.6 ，适用于 Cudy TR3000 512M Flash。

512M 版本使用新增的 `cudy_tr3000-v1-512mb` 设备目标，UBI 分区约 506MiB；其硬件布局沿用 256M 版本，刷写时使用对应的 512M 固件。

---

## DHCP uboot

编译自 https://github.com/weekdaycare/bl-mt798x-dhcpd 感谢大佬开源，兼容新 flash

![](/uboot.png)

本项目使用单分区 DHCP U-Boot，发布文件命名为 `cudy_tr3000-v1-512mb`。其 UBI 使用剩余全部 Flash 空间，因此适用于 512M Flash。

---

## USB 供电控制

上游的最新源码已经打开了默认供电，具体可以见这条 [commit](https://github.com/padavanonly/immortalwrt-mt798x-6.6/commit/86356f8a2f796e5808fda25ce3e3bf6b3cc3278e)

若你想关闭 USB 供电执行命令

```bash
echo 0 > /sys/class/gpio/modem_power/value
```

恢复供电执行命令

```bash
echo 1 > /sys/class/gpio/modem_power/value
```

---

## 第三方软件包

- [Nikki](https://github.com/nikkinikki-org/OpenWrt-nikki)
- [Bandix](https://github.com/timsaya/luci-app-bandix)
- [luci-theme-aurora](https://github.com/eamonxg/luci-theme-aurora)
- [luci-app-aurora-config](https://github.com/eamonxg/luci-app-aurora-config)
- luci-app-ttyd
- luci-app-upnp
- kmod-usb-net-cdc-ether
- kmod-usb-net-rndis
- kmod-mtd-rw

---

## SSH 连接 Action

可以通过 ssh 连接到 Action 工作流来配置 `menuconfig` 。

---

## 编译注意事项

GitHub Actions 存储有限，大型软件包（如 sing-box 或 alist）建议使用预编译方式，而不是源码编译，即在编译过程中加入已经编译好现成软件包。否则你应该会碰到超长编译时间 + 超出 Action 储存。示例：

```sh
# 创建存储二进制文件的目录
BIN_DIR="$GITHUB_WORKSPACE/openwrt/files/usr/bin"
mkdir -p "$BIN_DIR"

# -------- 下载并解压 xray-core ARM64 -------
echo "Downloading xray-core..."
curl -L -o xray.zip https://github.com/XTLS/Xray-core/releases/download/v25.10.15/Xray-linux-arm64-v8a.zip
unzip -o xray.zip -d "$BIN_DIR"
chmod +x "$BIN_DIR/xray"
rm xray.zip

# -------- 下载并解压 sing-box ARM64 -------
echo "Downloading sing-box..."
curl -L -o sing-box.tar.gz https://github.com/SagerNet/sing-box/releases/download/v1.12.12/sing-box-1.12.12-linux-arm64.tar.gz
TMP_DIR=$(mktemp -d)
tar -xzf sing-box.tar.gz -C "$TMP_DIR"
mv "$TMP_DIR"/sing-box-1.12.12-linux-arm64/sing-box "$BIN_DIR"/sing-box
chmod +x "$BIN_DIR/sing-box"
rm -rf "$TMP_DIR"
rm sing-box.tar.gz
```

---

## Credits

- [bl-mt798x-dhcpd](https://github.com/weekdaycare/bl-mt798x-dhcpd)
- [bl-mt798x](https://github.com/hanwckf/bl-mt798x)
- [immortalwrtwrt](https://github.com/padavanonly/immortalwrt-mt798x-6.6)
- [P3TERX](https://github.com/P3TERX)
- [Microsoft Azure](https://azure.microsoft.com)
- [GitHub Actions](https://github.com/features/actions)
- [OpenWrt](https://github.com/openwrt/openwrt)
- [coolsnowwolf/lede](https://github.com/coolsnowwolf/lede)
- [Mikubill/transfer](https://github.com/Mikubill/transfer)
- [softprops/action-gh-release](https://github.com/softprops/action-gh-release)
- [Mattraks/delete-workflow-runs](https://github.com/Mattraks/delete-workflow-runs)
- [dev-drprasad/delete-older-releases](https://github.com/dev-drprasad/delete-older-releases)
- [peter-evans/repository-dispatch](https://github.com/peter-evans/repository-dispatch)

---

## License

[MIT](https://github.com/P3TERX/Actions-OpenWrt/blob/main/LICENSE) © [**P3TERX**](https://p3terx.com)
