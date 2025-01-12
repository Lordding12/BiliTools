## 目录

- [目录](#目录)
- [获取 Cookie 的方法](#获取-cookie-的方法)
  - [Firefox](#firefox)
  - [Chrome/Chromium](#chromechromium)
- [配置相关](#配置相关)
  - [配置文件路径](#配置文件路径)
  - [配置介绍（开发查看）](#配置介绍开发查看)
  - [用户配置参考](#用户配置参考)
- [Account 配置项 （新版本）](#account-配置项-新版本)
- [功能配置](#功能配置)
- [Message 配置项](#message-配置项)
  - [Email](#email)
  - [投币任务配置](#投币任务配置)
  - [充电任务配置](#充电任务配置)
  - [竞猜任务配置](#竞猜任务配置)
  - [天选时刻任务配置](#天选时刻任务配置)
  - [直播间礼物任务配置](#直播间礼物任务配置)
- [Github Actions secrets](#github-actions-secrets)
- [环境变量](#环境变量)
  - [青龙面板相关](#青龙面板相关)

## 获取 Cookie 的方法

以 PC 端浏览器举例（推荐使用 Firefox/Chrome/Chromium Edge）

最终 Cookie 是这样的（为了演示方便换了行，实际只有一行）

```text
_uuid=D2282D0F-257B-845A-BDF5-C770ED288F4001440infoc; buvid3=BF17608E-FB87-4F49-A922-56FD2E284D6F18534infoc;
fingerprint=5502cd4fe9637738de04bd9c3d1bdbc5;
buvid_fp=BF17608E-FB87-4F49-A922-56FD2E284D6F18534infoc;
SESSDATA=21607773%2C1631089673%2C71a42%2A31; bili_jct=dd92c55a6d67041ce2f3fb1650889ea8;
DedeUserID=521268093; DedeUserID__ckMd5=47d541f04b605da9;
sid=ivie73r8; fingerprint3=792b32adfecbe31a4aca53ab7be1ad76;
fingerprint_s=bb6736758e7344a295c2ed6070cc642e;
buvid_fp_plain=BF17608E-FB87-4F49-A922-56FD2E284D6F18534infoc;
CURRENT_FNVAL=80; blackside_state=1; rpdid=|(kmJYYJ)lkR0J'uYu)llkJYJ; _dfcaptcha=a46d7562a42065d43a88c053e283e876;
LIVE_BUVID=AUTO8016188357987702; bsource=search_baidu; PVID=2
```

### Firefox

任意方式进入 b 站（搜索，收藏夹，地址访问等）
按 F12 （或者右键 --> 检查）打开开发者工具，切换到`网络` ( `network` )  
点击重新载入（或者按 F5，Ctrl + R 等）刷新页面

![firefox-network](./images/firefox-network.png)

点击某一个请求（通常为 `nav` ）

![firefox-net-bnav](./images/firefox-net-bnav.png)

### Chrome/Chromium

任意方式进入 b 站（搜索，收藏夹，地址访问等）  
按 F12 （或者右键 --> 检查）打开开发者工具，切换到 `网络` ( `network` )  
点击重新载入（或者按 F5，Ctrl + R 等）刷新页面  
点击某一个请求（通常为 nav ）

![chrome-net-bnav](./images/chrome-net-bnav.png)

## 配置相关

- 配置使用的 `json5`，兼容 `json` 且更加灵活，可以支持 `注释`。
- 务必使用 https://www.lddgo.net/string/json5 校验 json5 格式。
- 压缩配置地址（选择 gzip 压缩）：https://www.baidufe.com/fehelper/en-decode/

### 配置文件路径

- `config/config.json`
- `与 index.js 同目录/config/config.json`
- `index.js` 同级 `config.json`
- 以上配置后缀改为 `.json5` 同样有效
- 当然你可以不用文件配置，直接使用环境变量 `BILITOOLS_CONFIG`（配置按照要求 [gzip 压缩](https://www.baidufe.com/fehelper/en-decode/)）

### 配置介绍（开发查看）

所有配置都登记在 [`types/config.ts`](/src/types/config.ts) 文件中

<details>
<summary>解释：</summary>

- `number`: 数字 例如：`123`。
- `string`: 字符串 例如：`"ojbk"`。
- `boolean`: 布尔值 `true` 或者 `false`。
- `[]`: 数组 例如：`number[]` 具体可以是 `[1,2,3]`。
- `targetLevel?: number;` `?` 表示 `targetLevel` 是可选的配置。

</details>

### 用户配置参考

多用户配置参考 [config.example.json5](../config/config.example.json5)

单用户配置参考 [config.single.json](../config/config.single.json)

注：多用户就是单用户数组。

**多用户配置只用于部分情况，并不是所有都支持**

- 本地（如 npm）/docker 运行多个用户
- 本地（如 npm）/docker 推送多个到 scf

其他情况填写多个只会使用第一个, 不用担心

## Account 配置项 （新版本）

| Key          | 值类型                           | 默认值              | 说明                             |
| ------------ | -------------------------------- | ------------------- | -------------------------------- |
| message      | Message [↓↓](#message-配置项)    | 无配置              |                                  |
| **cookie**   | 字符串                           | 无，必须手动添加    | **必须** 完整的 Cookie           |
| function     | Function [↓↓](#功能配置)         | 详见具体介绍        |                                  |
| userAgent    | 字符串                           | 固定 Chrome         | 浏览器用户代理 （建议自行配置）  |
| dailyRunTime | 字符串                           | `17:30:00-23:40:00` | Serveless 随机运行的时间段       |
| heartRunTime | 字符串                           | `12:00:00-21:00:00` | 同上，不过是直播心跳的时间       |
| apiDelay     | `[数值, 数值]`或者数值           | `[2, 6]`            | 单位 S，区间中随机，或固定一个值 |
| coin         | 投币 [↓↓](#投币任务配置)         |                     | 投币相关                         |
| gift         | 礼物[↓↓](#直播间礼物任务配置)    |                     | 直播礼物相关                     |
| charge       | 充电 [↓↓](#充电任务配置)         |                     | 充电相关                         |
| match        | 竞猜 [↓↓](#竞猜任务配置)         |                     | 竞猜相关                         |
| lottery      | 天选时刻 [↓↓](#天选时刻任务配置) |                     | 天选时刻相关                     |

**重要配置说明**

- cookie 详见 [获取 Cookie 的方法](./readme.md#获取-cookie-的方法)
- userAgent - 固定唯一 UA `Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/88.0.4324.150 Safari/537.36` 请尽量自行设置

## 功能配置

| Key             | 默认值  | 说明                 |
| --------------- | ------- | -------------------- |
| silver2Coin     | `true`  | 银瓜子兑换硬币       |
| addCoins        | `true`  | 投币                 |
| liveSignTask    | `true`  | 直播间签到           |
| shareAndWatch   | `true`  | 观看和分享视频       |
| mangaSign       | `false` | 漫画签到（移动端）   |
| supGroupSign    | `false` | 应援团签到（移动端） |
| liveSendMessage | `false` | 每日直播间发送弹幕   |
| charging        | `false` | 给 UP 充电           |
| getVipPrivilege | `false` | 获取年度大会员权益   |
| giveGift        | `false` | 赠送过期礼物         |
| matchGame       | `false` | 赛事竞猜             |
| liveLottery     | `false` | 直播天选时刻         |
| ~~liveHeart~~   | `false` | 直播心跳（小心心）   |

- ~~liveHeart 在使用 Serverless 时即使配置为 true，也不会运行。但是在 SCF、FC 等环境中配置 `heart_bili_timer` 触发器即可正常运行，具体情况请参考对应平台的配置文档。若要强制使用请配置 `LIVE_HEART_FORCE` 环境变量为任意值（由此可能导致云函数超时等异常，一概不知）~~。

## Message 配置项

`[message]`

| Key            | 值类型                    | 说明                                                 |
| -------------- | ------------------------- | ---------------------------------------------------- |
| email          | Email [↓](#email)         |                                                      |
| ~~serverChan~~ | 字符串                    | 已过期 [官网](https://sct.ftqq.com/)获取 token       |
| pushplusToken  | 字符串                    | 即将过期 [官网](http://www.pushplus.plus/)获取 token |
| api            | 字符串                    | get 链接模板 例如：`http://xxxx.xxx/{title}/{text}`  |
| 从青龙面板移植 | [环境变量](#青龙面板相关) |                                                      |

### Email

`[email]`

| Key      | 值类型 | 默认值    | 举例             | 说明             |
| -------- | ------ | --------- | ---------------- | ---------------- |
| **from** | 字符串 |           | `mhtwrnm@qq.com` | 发件邮箱         |
| to       | 字符串 | from 的值 | `mhtwrnm@qq.com` | 接收邮箱         |
| **pass** | 字符串 |           |                  | 发件邮箱的授权码 |
| **host** | 字符串 |           | `smtp.163.com`   |                  |
| port     | 数值   | `465`     |                  | host 的端口      |

- 网易邮箱请参考 [什么是 POP3、SMTP 和 IMAP?](http://help.163.com/09/1223/14/5R7P6CJ600753VB8.html?servCode=6010376)
- QQ 邮箱请参考 [如何打开 POP3/SMTP/IMAP 功能？](https://service.mail.qq.com/cgi-bin/help?subtype=1&&no=166&&id=28)

### 投币任务配置

`[coin]`

| Key           | 值类型            | 默认值 | 说明                                           |
| ------------- | ----------------- | ------ | ---------------------------------------------- |
| targetLevel   | 数值              | `6`    | 目标等级，达到或大于将不投币、分享、观看视频   |
| stayCoins     | 数值              | `0`    | 账号至少保留的硬币数目，低于或等于将不投币     |
| targetCoins   | 数值              | `5`    | 每日投币目标（超过将不投币）                   |
| customizeUp   | 数值数组          | `[]`   | 自定义投币 UP， 在所填中随机选取               |
| retryNum      | 数值              | `4`    | 投币失败重试次数                               |
| upperAccMatch | `true`或者`false` | `true` | 自定义投币 UP 时，合作视频的 UP 必须为指定中的 |

### 充电任务配置

`[charge]`

| Key        | 值类型 | 默认值       | 说明                       |
| ---------- | ------ | ------------ | -------------------------- |
| mid        | 数值   | 自己的 mid   | 充电目标的 mid（默认自己） |
| presetTime | 数值   | 每月最后一天 | 每月充电的日期             |

### 竞猜任务配置

`[match]`

| Key       | 值类型             | 默认值 | 说明                                      |
| --------- | ------------------ | ------ | ----------------------------------------- |
| coins     | 数值               | `5`    | 每次竞猜的数量                            |
| selection | 数值               | `1`    | 压赔率低的（正压）大于 0 的数，反之等于 0 |
| diff      | 数值（可以是小数） | `0.0`  | 赔率需要达到的差距                        |

### 天选时刻任务配置

`[lottery]`

| Key          | 值类型     | 默认值                                                                                                 | 说明                                      |
| ------------ | ---------- | ------------------------------------------------------------------------------------------------------ | ----------------------------------------- |
| excludeAward | 字符串数组 | `["舰","船","航海","代金券","优惠券","自拍","照","写真","图","提督","车车一局","再来一局","游戏道具"]` | 奖品描述不能包含，比如“自拍一张”将被跳过  |
| includeAward | 字符串数组 | `["谢"]`                                                                                               | 奖品描述包含，如果满足则跳过 excludeAward |
| blackUid     | 数值数组   | `[65566781, 1277481241, 1643654862, 603676925]`                                                        | up 黑名单（up 的 id，不是房间号）         |
| moveTag      | 字符串     | `天选时刻`                                                                                             | 关注的用户统一移动到此                    |
| pageNum      | 数值       | `2`                                                                                                    | 扫描几页直播间                            |

### 直播间礼物任务配置

`[gift]`

| Key  | 值类型   | 默认值           | 说明                                 |
| ---- | -------- | ---------------- | ------------------------------------ |
| mids | 数值数组 | coin.customizeUp | 自定义投喂礼物 UP， 在所填中随机选取 |

## Github Actions secrets

配置地址 `https://github.com/github用户名/仓库名/settings/secrets/actions`

| 名字               | 说明                                           |
| ------------------ | ---------------------------------------------- |
| BILITOOLS_CONFIG   | Gzip 压缩后的配置                              |
| TENCENT_SECRET_ID  | 使用 SCF 必须的腾讯账号授权 ID                 |
| TENCENT_SECRET_KEY | 使用 SCF 必须的腾讯账号授权 KEY                |
| TIMEOUT_MINUTES    | 部分时机设置的 Action 超时时间（默认 15 分钟） |

## 环境变量

| 名字                       | 说明                                                                 |
| -------------------------- | -------------------------------------------------------------------- |
| TENCENT_SECRET_ID          | 使用 SCF 随机执行需要腾讯账号授权 ID                                 |
| TENCENT_SECRET_KEY         | 使用 SCF 随机执行需要腾讯账号授权 KEY                                |
| ALI_SECRET_ID              | 使用 FC 随机执行需要阿里账号授权 ID                                  |
| ALI_SECRET_KEY             | 使用 FC 随机执行需要阿里账号授权 KEY                                 |
| USE_NETWORK_CODE           | 直接通过网络请求代码，无需手动更新（云函数）                         |
| SERVERLESS_PLATFORM_VENDOR | Serverless 供应商，本地推送时必须，Docker 默认为`tencent`            |
| BILITOOLS_CONFIG           | [Gzip 压缩](#配置相关)后的单个用户配置，所有环境都可用               |
| RUN_SCF_ALL                | 运行全部云函数（ Docker 推送至 SCF 时使用，值为需要`y`）             |
| ~~SCF_MEMORY_SIZE~~        | scf 中运行的内存大小（默认 128M，范围为 64 以及 128 的 1-24 整数倍） |
| ~~BILITOOLS_FILE_NAME~~    | ~~给配置文件命名，主要为了防止青龙面板的配置冲突~~                   |

### 青龙面板相关

> 下列环境变量来自 `青龙面板`
>
> 具体介绍看这里 <https://github.com/whyour/qinglong/blob/develop/sample/config.sample.sh>

支持以下三种方式填写：

- 环境变量，如 `PUSH_PLUS_TOKEN`
- 通过在配置中的 message 字段中配置，以下两种方式都可以。
  ```json
  {
    "message": {
      "PUSH_PLUS_TOKEN": "xxxxxxxx",
      "pushPlusToken": "xxxxxxxx"
    }
  }
  ```

```js
[
  'GOBOT_URL',
  'GOBOT_TOKEN',
  'GOBOT_QQ',
  'SCKEY',
  'QQ_SKEY',
  'QQ_MODE',
  'BARK_PUSH',
  'BARK_SOUND',
  'BARK_GROUP',
  'TG_BOT_TOKEN',
  'TG_USER_ID',
  'TG_PROXY_AUTH',
  'TG_PROXY_HOST',
  'TG_PROXY_PORT',
  'TG_API_HOST',
  'DD_BOT_TOKEN',
  'DD_BOT_SECRET',
  'QYWX_KEY',
  'QYWX_AM',
  'IGOT_PUSH_KEY',
  'PUSH_PLUS_TOKEN',
  'PUSH_PLUS_USER',
];
```
