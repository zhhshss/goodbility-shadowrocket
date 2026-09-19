# goodbility-shadowrocket

墨鱼（@ddgksf2013）VIP 解锁脚本的 Shadowrocket（小火箭）本地化仓库。

其中 `goodbility.vip.js` 做了 Shadowrocket 环境适配，其余脚本经沙箱验证均为零改动原生兼容（YAGNI：不需要的适配一行都不加）。

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

## 二、BuyiTunes 多合一解锁

- 原脚本：https://ddgksf2013.top/scripts/buyitunes.vip.js
- 仅用 `$response.body` / `$done()`，Shadowrocket 原生兼容，**零改动收录**
- ⚠️ 注意：使用此脚本会导致 App Store 无法切换账户（需切换时先关闭该脚本/MITM）
- 解锁列表：https://appraven.net/collection/77331175

```ini
[Script]
buyitunes = type=http-response,pattern=^https?:\/\/buy\.itunes\.apple\.com\/verifyReceipt$,script-path=https://raw.githubusercontent.com/zhhshss/goodbility-shadowrocket/main/buyitunes.vip.js,requires-body=true

[MITM]
hostname = %APPEND% buy.itunes.apple.com
```

## 三、RevenueCat 多合一解锁

- 原脚本：https://ddgksf2013.top/scripts/revenuecat.vip.js
- 脚本内部已含 `$rocket` 环境判断，Shadowrocket 原生兼容，**零改动收录**
- 解锁列表：https://appraven.net/collection/77299969

```ini
[Script]
revenuecat_response = type=http-response,pattern=^https:\/\/api\.(revenuecat|rc-backup)\.com\/.+\/(receipts$|subscribers\/[^/]+$),script-path=https://raw.githubusercontent.com/zhhshss/goodbility-shadowrocket/main/revenuecat.vip.js,requires-body=true
revenuecat_header = type=http-request,pattern=^https:\/\/api\.(revenuecat|rc-backup)\.com\/.+\/(receipts|subscribers),script-path=https://raw.githubusercontent.com/zhhshss/goodbility-shadowrocket/main/deleteHeader.js

[MITM]
hostname = %APPEND% api.revenuecat.com, api.rc-backup.com
```

## 四、瓜子视频净化（去广告）

- 原配置：https://ddgksf2013.top/rewrite/GuaZiVideoAds.conf （QX `jsonjq-response-body` 格式）
- 模块文件：`GuaZiVideoAds.module` —— 已转换为 Shadowrocket 模块格式（`[Body Rewrite]` 的 `http-response-jq`）
- 功能：底栏仅保留首页和我的、去独立广告/首页悬浮/评论区/播放页跑马灯广告

### 模块安装（推荐，可自动更新）

1. Shadowrocket → 配置 → 模块 → 右上角「+」（或「添加模块」）
2. 粘贴模块 URL：

```
https://raw.githubusercontent.com/zhhshss/goodbility-shadowrocket/main/GuaZiVideoAds.module
```

3. 保存后开启该模块，并确保「HTTPS 解密」已开启且证书已信任

### 手动粘贴（备选）

打开 `GuaZiVideoAds.module`，复制 `[Body Rewrite]` 与 `[MITM]` 两段到配置文件的纯文本编辑中。

## 五、哔哩哔哩繁体 CC 字幕转简体

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

## 沙箱验证记录

全部在 Node 模拟环境（`$environment`+`$httpClient`+`$rocket` / `$task`）中验证通过：

| 脚本 | Shadowrocket | Quantumult X | 改动量 |
| --- | --- | --- | --- |
| `goodbility.vip.js` | PASS（回执改写含 entitlements） | PASS（回归） | Env 类 +`isShadowrocket()` |
| `deleteHeader.js` | PASS | PASS | 零改动 |
| `bilibili_cc.js` | PASS（繁转简） | — | 零改动 |
| `buyitunes.vip.js` | PASS（expires_date→2099） | PASS | 零改动 |
| `revenuecat.vip.js` | PASS（entitlements 注入） | PASS | 零改动 |
| `GuaZiVideoAds.module` | jq 表达式语法校验 PASS | — | QX→SR 模块格式转换 |

## 免责声明

仅供学习交流使用，请于下载后 24 小时内删除。脚本免费使用，收费请举报！
