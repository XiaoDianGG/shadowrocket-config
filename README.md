# Shadowrocket 国内外分流与 DNS 修复配置

国内域名直连，国外域名使用所选代理节点。国内使用系统 DNS 保留本地 CDN 调度；国外代理连接由落地节点远端解析。本机需要进行的额外 DNS 查询及备用 DNS 使用通过代理的 Cloudflare DoH。这不是“所有域名都只用 Cloudflare”的配置。

## 手机导入

```text
https://raw.githubusercontent.com/XiaoDianGG/shadowrocket-config/main/shadowrocket-mobile.conf
```

1. Shadowrocket → 配置 → `+`，粘贴地址并下载；已导入的配置可选择“更新”。
2. 选择该配置 → 使用配置。
3. 首页 → 全局路由 → 配置，选用你已有的节点，重新连接。
4. 在手机上重新进行 DNS 深度测试，并检查微信、淘宝等国内服务的访问速度。

`PROXY` 使用应用中已选择的节点。文件不含节点、密码、订阅、设备标识、脚本、证书或 HTTPS 解密设置。

此版本同步本机 `sr_dnsfix.conf` 的全部 436 条规则及 General 设置，保留 `FINAL,PROXY`，尚未切换为“国外名单代理、其余直连”模式。

## 配置行为

- 保留原来的明确代理域名以及 Claude 项目的 20 条域名规则。
- 国内直连使用 ChinaMax 完整域名集，包含字节国内业务、B站、京东等，并补充国内共享 SDK 域名；TikTok、CapCut、Trae、Coze 国际域名优先代理。
- 增加国内域名直连覆盖；国内域名集固定在已核验的版本，不追踪第三方分支更新。
- 所有原 IP 规则及 `GEOIP,CN,DIRECT` 增加 `no-resolve`，避免国外域名为匹配 IP 规则先经国内 DNS 查询。已知中国 IP 仍可以直连。
- 未匹配的域名默认代理；未被国内域名集覆盖的国内域名也可能走代理，需要按实际连接日志补充。
- 主 DNS：`https://1.1.1.1/dns-query#proxy`；备用 DNS：`https://1.0.0.1/dns-query#proxy`。备用 DNS 不留空、不使用 system。
- `dns-direct-system=true` 是国内域名的有意例外。`dns-direct-fallback-proxy=false` 避免国内域名解析失败后自动改变连接出口。
- 代理端解析器的上游由落地服务器配置决定，本文件不能指定或证明服务器内部的上游 DNS。

## 验证范围

2026-10-09，在 macOS Shadowrocket 2.2.90（3378）和 Chrome 上验证了同一套 DNS 与分流规则：修改前 DNS 深度测试出现 6 个中国解析器；修改后只检测到美国落地节点解析器，Claude 检测页也显示“未检测到泄露”。字节检测请求已返回中国出口；B站及京东检测域名均匹配 DIRECT；国内资源请求已检查。

手机、不同节点、代理掉线及网络切换需要分别验证；该结果不代表所有环境下永远零泄漏，也不是平台的账号安全保证。导入前保留原配置，方便回退。

## 来源与许可

- 基础规则：Moshel / Johnshall，[Shadowrocket-ADBlock-Rules-Forever](https://github.com/Johnshall/Shadowrocket-ADBlock-Rules-Forever)，CC BY-SA 4.0，见 LICENSE。
- Claude 附加规则：[windery/claude-proxy-rules](https://github.com/windery/claude-proxy-rules)，审查版本 `518f81311cdcf841ba230c413532d3d23a70d620`，MIT，见 CLAUDE-RULES-LICENSE。规则已内联。
- 国内域名集：[blackmatrix7/ios_rule_script](https://github.com/blackmatrix7/ios_rule_script)，引用 `ChinaMax/ChinaMax_Domain.list`，固定提交 `036c097eb26c6a52c4f04ebcb6633043cb942669`，本次核验为 111,471 个域名条目。域名集由 Shadowrocket 下载，不在本仓库复制其内容。
