# goodbility-shadowrocket

GoodNotes & Notability 内购会员解锁脚本 —— Shadowrocket（小火箭）本地化版本。

- 原作者：[@ddgksf2013](https://t.me/ddgksf2021)
- 原脚本：https://ddgksf2013.top/scripts/goodbility.vip.js
- 本仓库仅做 Shadowrocket 环境适配（UA/环境检测本地化），脚本解锁逻辑未做任何改动。

## 文件说明

| 文件 | 说明 |
| --- | --- |
| `goodbility.vip.js` | 主脚本（http-response 改写订阅回执），已适配 Shadowrocket |
| `deleteHeader.js` | 辅助脚本（http-request 移除 X-RevenueCat-ETag 头），Shadowrocket 原生兼容 |

## Shadowrocket 配置

在 Shadowrocket 配置文件中加入：

```ini
[Script]
goodbility_response = type=http-response,pattern=^https:\/\/isi\.csan.[a-z.]+\/.+\/(receipts$|subscribers\/[^/]+$),script-path=https://raw.githubusercontent.com/zhhshss/goodbility-shadowrocket/main/goodbility.vip.js,requires-body=true
goodbility_header = type=http-request,pattern=^https:\/\/isi\.csan.[a-z.]+\/.+\/(receipts|subscribers),script-path=https://raw.githubusercontent.com/zhhshss/goodbility-shadowrocket/main/deleteHeader.js
notability_global = type=http-response,pattern=^https?:\/\/notability\.com\/global,script-path=https://raw.githubusercontent.com/zhhshss/goodbility-shadowrocket/main/goodbility.vip.js,requires-body=true

[MITM]
hostname = %APPEND% isi.csan.*, notability.com
```

开启 HTTPS 解密（MITM）并信任证书后，恢复购买即可解锁。

## 适配说明（相对原脚本的改动）

1. `Env` 环境类新增 `isShadowrocket()` 检测（基于 `$environment` + `$httpClient` 特征判定）。
2. `done()` 在 Shadowrocket 环境下正确调用 `$done()` 返回改写后的响应体。
3. 头部注释改为 Shadowrocket `[Script]` / `[MITM]` 配置格式。

Shadowrocket 提供与 Surge 兼容的 `$httpClient` / `$persistentStore` / `$notification` API，
其余分支无需改动，同时已回归验证 Surge / Quantumult X 环境不受影响。

## 免责声明

仅供学习交流使用，请于下载后 24 小时内删除。脚本免费使用，收费请举报！
