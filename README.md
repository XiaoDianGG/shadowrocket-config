# Shadowrocket 手机配置

保留原来的国内外分流和阿里 / 腾讯 DoH，追加 Claude 项目的 20 条域名代理规则。没有把所有 DNS 绕到海外，没有添加国内域名覆盖规则。

## 手机导入

```text
https://raw.githubusercontent.com/XiaoDianGG/shadowrocket-config/main/shadowrocket-mobile.conf
```

1. Shadowrocket → 配置 → `+`，粘贴地址并下载。
2. 选择下载的配置 → 使用配置。
3. 首页 → 全局路由 → 配置，选用你已有的节点。
4. 重新连接，并实际检查国内、国外分流及 DNS 日志。

`PROXY` 使用应用中已选择的节点。文件不含节点、密码、订阅、设备标识、脚本、证书或 HTTPS 解密设置。手机效果需要在手机上验证。

## 行为与验证范围

- Claude 主站、API、Artifact、MCP、监控、客服、验证及付款域名使用 PROXY；共享服务的其他使用者也会匹配。
- 原明确代理规则保留；中国大陆 IP 直连；其余流量默认代理。
- DNS 保留 `https://dns.alidns.com/dns-query, https://doh.pub/dns-query`，不使用海外代理 DNS 覆写。
- 手机版本 412 条分流规则与桌面已保存版本核对一致。保留原 General 配置，仅增加本仓库 update-url；去掉桌面的 HTTPS 解密与 URL 重写。
- 原始 URL 的更新可能覆盖本地编辑；建议自定义前另存副本。
- 规则匹配、DNS、节点启动解析和 VPN 异常是不同的检查项，本配置不承诺零泄漏或账号安全。

## 来源与许可

基础规则：Moshel / Johnshall，https://github.com/Johnshall/Shadowrocket-ADBlock-Rules-Forever ，CC BY-SA 4.0（见 LICENSE）。

Claude 附加规则：windery/claude-proxy-rules，审查版本 `518f81311cdcf841ba230c413532d3d23a70d620`（2026-10-03），https://github.com/windery/claude-proxy-rules ，MIT（见 CLAUDE-RULES-LICENSE）。规则已内联，不会随第三方仓库自动改变。

此仓库曾发布过未充分验证的海外代理 DNS 草稿，已撤回；当前版本保留原 DNS。旧草稿保留在 Git 历史中，当前导入地址已替换为保留原 DNS 的版本。
