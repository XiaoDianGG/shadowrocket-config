# Shadowrocket 手机配置

适用于 Shadowrocket，保留已验证的国内常用域名直连规则、Claude/Anthropic 代理规则，以及通过代理访问的 Cloudflare / Google DoH。默认 IPv6 关闭。

## 导入地址

```text
https://raw.githubusercontent.com/XiaoDianGG/shadowrocket-config/main/shadowrocket-mobile.conf
```

1. 手机打开 Shadowrocket → 配置 → 右上角 `+`，粘贴上述地址并下载。
2. 点选下载的 `shadowrocket-mobile.conf`，选择“使用配置”。
3. 首页 → 全局路由 → 选择“配置”。
4. 选择你已有的可用节点，关闭连接后重新开启。
5. 用浏览器打开 https://ip.net.coffee/ 检查国内、国外测试项的出口，并检查 Shadowrocket DNS 日志。

文件仅包含通用设置和分流规则，不包含节点、密码、订阅、设备标识、证书、脚本、HTTPS 解密或 URL 重写。导入不会为手机新增代理节点。

## 分流行为

- Claude / Anthropic 域名强制代理。
- 网易、字节、腾讯、淘宝、京东、哔哩哔哩等常用国内域名优先直连。
- 现有明确代理的特殊域名保留；其后 `.cn` 域名及中国大陆 IP 直连。
- 其余流量默认代理。国内服务的海外域名、第三方资源可能需要补充规则。
- 主 DNS：Cloudflare DoH；备用 DNS：Google DoH，均使用 `#proxy`。
- `dns-direct-system=false`；保留现配置的 `dns-direct-fallback-proxy=true`，直接解析失败时允许切换代理。

这是通用分流配置。系统流量、节点启动解析、浏览器自行使用的 DNS、VPN 断开或异常等场景仍需在手机实测；不能保证完全无泄漏或账号不被关联。

## 与桌面配置的区别

保留同一套分流规则和 DNS 参数，移除了桌面专用的 Apple 域名绕过项及 `bypass-tun` 列表，由客户端管理隧道路由。移除 HTTPS 解密与 Google.cn 重写，不需要导入或信任根证书。

## 来源与许可

基础规则由 Moshel / Johnshall 提供：
https://github.com/Johnshall/Shadowrocket-ADBlock-Rules-Forever/blob/release/sr_cnip.conf

在其基础上修改了 DNS、补充 Claude 与国内域名直连规则，并移除了 HTTPS 解密与 URL 重写。依据上游许可，采用 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/)，见 LICENSE。
