# goodbility-shadowrocket

墨鱼（@ddgksf2013）脚本的 Shadowrocket（小火箭）本地化适配仓库。

- 原作者：[@ddgksf2013](https://t.me/ddgksf2021)
- 脚本仅供学习交流使用，禁止转载售卖。

## 一、GoodNotes & Notability 内购会员解锁

- 原脚本：https://ddgksf2013.top/scripts/goodbility.vip.js
- 本仓库对 `goodbility.vip.js` 做了 Shadowrocket 环境适配（解锁逻辑未做任何改动）。

| 文件 | 说明 |
| --- | --- |
| `goodbility.vip.js` | 主脚本（http-response 改写订阅回执），已适配 Shadowrocket |
| `deleteHeader.js` | 辅助脚本（http-request 移除 X-RevenueCat-ETag 头），Shadowrocket 原生兼容 |

### Shadowrocket 配置

```ini
[Script]
goodbility_response = type=http-response,pattern=^https:\/\/isi\.csan.[a-z.]+\/.+\/(receipts$|subscribers\/[^/]+$),script-path=https://raw.githubusercontent.com/zhhshss/goodbility-shadowrocket/main/goodbility.vip.js,requires-body=true
goodbility_header = type=http-request,pattern=^https:\/\/isi\.csan.[a-z.]+\/.+\/(receipts|subscribers),script-path=https://raw.githubusercontent.com/zhhshss/goodbility-shadowrocket/main/deleteHeader.js
notability_global = type=http-response,pattern=^https?:\/\/notability\.com\/global,script-path=https://raw.githubusercontent.com/zhhshss/goodbility-shadowrocket/main/goodbility.vip.js,requires-body=true

[MITM]
hostname = %APPEND% isi.csan.*, notability.com
```

开启 HTTPS 解密（MITM）并信任证书后，恢复购买即可解锁。

### 适配说明（相对原脚本的改动）

1. `Env` 环境类新增 `isShadowrocket()` 检测（基于 `$environment` + `$httpClient` 特征判定）。
2. `done()` 在 Shadowrocket 环境下正确调用 `$done()` 返回改写后的响应体。
3. 头部注释改为 Shadowrocket `[Script]` / `[MITM]` 配置格式。

Shadowrocket 提供与 Surge 兼容的 `$httpClient` / `$persistentStore` / `$notification` API，
其余分支无需改动，同时已回归验证 Surge / Quantumult X 环境不受影响。

## 二、哔哩哔哩繁体 CC 字幕转简体

- 原脚本：https://raw.githubusercontent.com/ddgksf2013/Scripts/refs/heads/master/bilibili_cc.js
- 原重写：https://raw.githubusercontent.com/ddgksf2013/Rewrite/refs/heads/master/Function/Bilibili_CC.conf
- `bilibili_cc.js` 仅使用 `$response.body` / `$done()`，Shadowrocket 原生兼容，**无需任何改动**，直接收录。

### Shadowrocket 配置

```ini
[Script]
bilibili_cc = type=http-response,pattern=^https?:\/\/.*\.hdslb\.com\/bfs\/subtitle\/.+\.json,script-path=https://raw.githubusercontent.com/zhhshss/goodbility-shadowrocket/main/bilibili_cc.js,requires-body=true

[MITM]
hostname = %APPEND% aisubtitle.hdslb.com, i0.hdslb.com
```

## 免责声明

仅供学习交流使用，请于下载后 24 小时内删除。脚本免费使用，收费请举报！
