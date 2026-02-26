# ZKGame Instant Game SDK 对接说明文档

## 概述

ZKGame Instant Game SDK 是一个通用的 Instant Game 游戏 SDK，支持 Facebook Instant Games。本文档详细介绍了所有可用接口的使用方法。

## 📋 SDK 版本信息

| 版本     | 发布日期   | 作者 | 主要更新                                 |
| -------- | ---------- | ---- | ---------------------------------------- |
| **v1.0** | 2025-12-25 | BIN  | 初始版本，支持 FB Instant Games 完整功能 |
| **v1.1** | 2026-02-26 | BIN  | 修复支付订单中存在用户ID为空的问题，完善支付失败的相关错误提示多语言 |

## 目录

1. [资料准备](#资料准备)
2. [获取 SDK](#获取SDK)
3. [初始化配置](#初始化配置)
4. [用户管理](#用户管理)
5. [数据存储](#数据存储)
6. [广告系统](#广告系统)
7. [支付系统](#支付系统)
8. [游戏管理](#游戏管理)
9. [事件追踪](#事件追踪)

## 资料准备

### 1. Facebook Instant Game 游戏提交资料清单

#### 📋 游戏基本信息

| 项目         | 要求       | 说明               | 示例                                            |
| ------------ | ---------- | ------------------ | ----------------------------------------------- |
| **游戏名称** | 必填       | 游戏的正式名称     | "超级消消乐"                                    |
| **游戏标语** | ≤ 20 字符  | 简短的游戏描述     | "经典三消益智游戏"                              |
| **游戏简介** | ≤ 200 字符 | 详细的游戏介绍     | "畅玩经典三消玩法，挑战数百个精心设计的关卡..." |
| **游戏类型** | 必选       | 游戏分类           | 益智、动作、策略、休闲等                        |
| **收益模式** | 必选       | 盈利方式           | IAA (广告)、IAP (内购)、IAA+IAP (混合)          |
| **支持语言** | 必填       | 游戏支持的语言列表 | 中文、英文、日文等                              |

#### 🎨 游戏图片资源（所有图片需要高质量 PNG 格式）

| 资源类型     | 尺寸要求                               | 必要性  | 用途说明                     |
| ------------ | -------------------------------------- | ------- | ---------------------------- |
| **应用图标** | 1024 × 1024                            | 🔴 必需 | 游戏主图标，显示在游戏列表中 |
| **小图标**   | 16 × 16                                | 🔴 必需 | 浏览器标签页图标             |
| **横幅图像** | 1200 × 627                             | 🔴 必需 | 社交分享和推广使用           |
| **封面图像** | 1600 × 300                             | 🔴 必需 | 游戏详情页顶部展示           |
| **闪屏图片** | 竖屏: 1080 × 1920<br>横屏: 1920 × 1080 | 🟡 建议 | 游戏加载时显示               |
| **预览视频** | 比例: 16:9 / 9:16 / 1:1                | 🔴 必需 | 游戏玩法演示视频             |

#### 📱 图片制作要求

- **格式**：PNG 格式，支持透明背景
- **质量**：高清晰度，避免模糊或像素化
- **内容**：图片内容需符合 Facebook 社区准则
- **文字**：避免在图片中使用过多文字
- **品牌**：保持视觉风格统一，体现游戏特色

#### 📺 游戏内需要的广告位信息，用于申请广告位

在Facebook Instant Games中需要提前申请广告位，请根据游戏需要选择合适的广告类型：

| 广告类型 | 广告位名称 | 使用场景 | 收益特点 | 建议申请数量 |
|----------|------------|----------|----------|--------------|
| **Banner横幅广告** | Banner Ad | 游戏主界面、暂停界面 | 持续展示，收益稳定 | 2-3个 |
| **激励视频广告** | Rewarded Video | 获得奖励、复活、加速 | 单次收益高，用户接受度高 | 3-5个 |
| **插屏广告** | Interstitial | 关卡结束、功能切换 | 收益较高，需控制频率 | 2-3个 |

#### 💰 游戏内的商品信息列表，主要是提供商品价格，以美金（USD）计价，用于创建应用内购商品

## 获取 SDK

### 📦 SDK 文件下载

**官方下载地址：**

```
https://docs.zencodegame.com/js/zkgame-sdk-v1.1.min.js
```

### 🔗 多种引入方式

#### 方式 1：直接下载到本地（推荐）

```bash
# 下载SDK文件到项目目录
curl -O https://docs.zencodegame.com/js/zkgame-sdk-v1.1.min.js

# facebook instant game还需要引入：
curl -O https://connect.facebook.net/en_US/fbinstant.8.0.js



# 或使用wget
wget https://docs.zencodegame.com/js/zkgame-sdk-v1.1.min.js
# facebook instant game还需要引入：
wget https://connect.facebook.net/en_US/fbinstant.8.0.js
```

#### 方式 2：CDN 直接引用

```html
<script src="https://docs.zencodegame.com/js/zkgame-sdk-v1.1.min.js"></script>
//facebook instant game还需要引入：
<script src="https://connect.facebook.net/en_US/fbinstant.8.0.js"></script>
```

#### 方式 3：HTML 本地引入

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Your Game</title>
  </head>
  <body>
    <!-- 游戏内容 -->

    <!-- 在页面底部引入SDK -->
    <script src="./js/zkgame-sdk-v1.1.min.js"></script>
    <!--facebook instant game还需要引入-->
    <script src="https://connect.facebook.net/en_US/fbinstant.8.0.js"></script>
    <script>
      // 初始化SDK
      const gameSDK = new window.GameSDK({
        APP_ID: "your_app_id",
        SECRET_KEY: "your_secret_key",
      });
    </script>
  </body>
</html>
```

## 初始化配置

### 基本配置

```javascript
const cfg = {
  APP_ID: "10040", // 应用ID，必填 我们提供
  SECRET_KEY: "your_secret_key", // 应用密钥，必填 我们提供
};
```

### 获取 SDK 文件

### 初始化 SDK

```javascript
gameSDK = new window.GameSDK(cfg);
await gameSDK.init();
```

**说明：**

- 必须在使用任何其他功能前调用 `init()` 方法
- 初始化会自动检测平台类型
- 支持 Facebook Instant Games 和 Web 平台自动切换

## 用户管理

### 1. 用户登录

```javascript
const userInfo = await gameSDK.login();
```

**返回值：**

```javascript
{
    userId: "12345",           // 用户ID
    name: "玩家昵称",           // 用户名
    avatar: "avatar_url"      // 头像URL
}
```

**说明：**

- 登录后会自动保存用户 token 到本地存储
- 支持自动重新登录机制
- 登录成功后会自动上报登录事件

### 2. 分享功能

```javascript
await gameSDK.share({
  title: "游戏分享标题",
  image: "",
  data: { level: 5, score: 1000 },
});
```

**参数说明：**

- `title`: 分享标题
- `image`: 分享图片:base64 or encoded image
- `data`: 附加数据，会传递给接收分享的用户

### 3. 邀请功能

```javascript
await gameSDK.inviteAsync({
  title: "游戏分享标题",
  image: "",
  data: { level: 5, score: 1000 },
});
```

**参数说明：**

- `title`: 邀请标题
- `image`: 邀请图片:base64 or encoded image
- `data`: 附加数据，会传递给接收邀请的用户

### 4. 创建快捷图标

```javascript
await gameSDK.createShortcut();
```

**说明：**

- 仅在支持的平台上生效
- 可以在用户桌面创建游戏快捷方式


### 5. 获取游戏启动参数
```javascript
const params = await gameSDK.getEntryPointData();
```

**说明：**

-- 获取包括分享、邀请、广告跳转等相关的参数


## 数据存储

### 1. 保存数据

```javascript
await gameSDK.saveData({
  level: 10,
  coins: 1000,
  achievements: ["first_win", "level_5"],
});
```

### 2. 读取数据

```javascript
const data = await gameSDK.loadData(["level", "coins", "achievements"]);
console.log(data); // { level: 10, coins: 1000, achievements: [...] }
```

**说明：**

- 数据会保存在平台的云存储中
- 支持跨设备同步
- 数据格式支持基本类型和 JSON 对象

## 广告系统

### 1. Banner 横幅广告

```javascript
// 显示Banner广告
await gameSDK.showBannerAd("banner_ad_id", "bottom");

// 隐藏Banner广告
await gameSDK.hideBannerAd();
```

**参数说明：**

- `placementId`: 广告位 ID
- `position`: 位置，'top'或'bottom'

### 2. 激励视频广告

```javascript
// 方法1：分步加载和显示
const rewardedAd = await gameSDK.loadRewardedVideoAd("rewarded_ad_id");
// 如果rewardedAd为空 则不展示广告，并提示用户暂无广告
const result = await gameSDK.showRewardedVideoAd(rewardedAd);

// 方法2：一步加载并显示
const result = await gameSDK.loadShowRewardedVideoAd("rewarded_ad_id");
```

**result返回值说明：**

```javascript
//加载展示成功
{
    success: true,      // true是成功显示 false表示展示失败
    rewarded: true      // 用户是否获得奖励
}
// 加载或展示失败示例
{
  success: false,      // true=成功展示，false=展示失败
  rewarded: false,     // 用户是否获得奖励（激励视频需完整观看才为 true）
  error: "错误信息",   // 调试/日志用的文本，建议上报到后台，用户提示使用友好文案
  errorCode: "错误码"   // 详见下方错误码说明
}

错误码说明：
- **ADS_NOT_LOADED**：广告未加载或当前没有可用的广告。处理：可在后台记录并尝试重新加载（`loadRewardedVideoAd`），前端提示“暂无广告，稍后再试”。
- **INVALID_PARAM**：调用参数非法（例如广告位 ID 错误或传入参数格式不对）。处理：检查调用参数并在开发/测试环境中记录详细错误，上线时友好提示用户。
- **NETWORK_FAILURE**：网络请求失败（超时、断网等）。处理：实现重试机制（指数退避），提示用户网络异常或离线。建议上报网络类型与失败详情便于诊断。
- **INVALID_OPERATION**：当前操作无效（例如在已展示广告时再次请求展示）。处理：避免重复调用；在状态允许时重试展示。
- **RATE_LIMITED**：请求频率过高被限流。处理：降低请求频率，加入重试间隔，并统计限流触达频次以调整策略。
- **USER_INPUT**：用户主动中断或取消（例如关闭视频）。处理：不发放奖励，记录事件用于分析用户流失点，可向用户展示取消确认或激励引导。

示例处理（伪代码）：
  const res = await gameSDK.loadShowRewardedVideoAd("rewarded_ad_id");
  if (res.success && res.rewarded) {
    // 发放奖励
  } else if (!res.success) {
    // 根据 errorCode 做细化处理或通用提示
    switch (res.errorCode) {
      case 'ADS_NOT_LOADED':
      case 'NETWORK_FAILURE':
        // 尝试重新加载或提示用户稍后再试
        break;
      case 'INVALID_PARAM':
        // 检查调用参数
        break;
      case 'USER_INPUT':
        // 用户取消，不发放奖励
        break;
      default:
        // 上报并展示友好错误提示
    }
  }
```

### 3. 插屏广告

```javascript
// 方法1：分步加载和显示
const interstitialAd = await gameSDK.loadInterstitialAd("interstitial_ad_id");
// 如果interstitialAd为空 则不展示广告，并提示用户暂无广告
const result = await gameSDK.showInterstitialAd(interstitialAd);
// 方法2：一步加载并显示
const result = await gameSDK.loadShowInterstitialAd("interstitial_ad_id");
// 
```

**返回值：**

```javascript
{
  success: true; // true表示成功显示 false表示展示失败
}
```

**注意事项：**

- 建议在游戏启动时预加载广告以提高展示速度
- 激励视频广告需要用户完整观看才能获得奖励
- 广告展示可能失败，需要处理异常情况

## 支付系统

### 发起支付

```javascript
const order = {
  orderId: "20251222_001", // 订单ID，唯一
  amount: "9.99", // 价格
  currency: "USD", // 货币代码
  country: "US", // 国家代码
  prodName: "金币包", // 商品名称
  description: "1000金币礼包", // 商品描述
  extra: "level_5_reward", // 额外信息
  callbackUrl: "https://your-server.com/callback", // 回调URL
};

const result = await gameSDK.toPay(order);
```

**返回值：result信息如下**

```javascript
{
    success: true,              // 支付是否成功 true表示成功 false表示失败
    message: 'Pay success'      // 结果信息 如果失败需要处理提示用户支付订单创建失败
}
//success：fasle的情况下，message的消息如下：
1. "CreateOrder failed: Payments not ready!" //支付方式暂不支持
2. "Network error!" //网络异常
3. "Pay fail" //支付失败/取消支付
4. "Pay fail:Network error!" //网络异常
5. "Purchase consume fail" //支付确认失败
6. "Purchase check fail" //支付检查失败
```

### 💰 服务端回调接口

当用户完成支付后，系统会通过设置的回调地址异步通知您的服务端，确保支付结果能够安全可靠地传递。

#### 📋 接口基本信息

| 项目             | 说明                               |
| ---------------- | ---------------------------------- |
| **请求方式**     | `POST`                             |
| **Content-Type** | `application/json`                 |
| **接口地址**     | 您在支付接口中设置的 `callbackUrl` |
| **字符编码**     | `UTF-8`                            |
| **超时时间**     | 30 秒                              |

#### 📤 请求参数

| 参数名         | 类型   | 必须 | 说明                                                                     | 示例值           |
| -------------- | ------ | ---- | ------------------------------------------------------------------------ | ---------------- |
| **userId**     | string | ✅   | 用户唯一标识                                                             | "14251"          |
| **payOrderId** | string | ✅   | 支付订单 ID                                                              | "4501189717"     |
| **currency**   | string | ✅   | 货币类型<br>目前仅支持 USD                                               | "USD"            |
| **amount**     | string | ✅   | 支付金额                                                                 | "50"             |
| **extra**      | string | ✅   | 订单备注信息<br>透传客户端传入的 extra 字段                              | "level_5_reward" |
| **status**     | string | ✅   | 支付状态<br>- `PAID`: 支付成功<br>- `FAIL`: 支付失败<br>- `REFUND`: 退款 | "PAID"           |
| **sign**       | string | 🟡   | 数字签名<br>部分支付方式需要验证签名<br>STAR 支付暂无签名                | "aaccdd..."      |

#### 📥 响应参数

您的服务器必须返回以下 JSON 格式的响应：

| 参数名      | 类型   | 必须 | 说明                                                             |
| ----------- | ------ | ---- | ---------------------------------------------------------------- |
| **code**    | int    | ✅   | 返回码<br>- `0`: 处理成功，停止重试<br>- `非0`: 处理失败，将重试 |
| **message** | string | ✅   | 返回消息说明                                                     |

#### 🔄 重试机制

- 当返回 `code ≠ 0` 时，系统将自动重试
- 重试间隔：1 分钟、5 分钟、15 分钟、30 分钟、1 小时...
- 最大重试次数：10 次
- 超过重试次数后将停止通知

#### 📝 代码示例

**回调请求示例：**

```bash
curl -X POST "https://your-game-server.com/payment/callback" \
  -H "Content-Type: application/json" \
  -H "User-Agent: ZKGame-PaymentCallback/1.0" \
  -d '{
    "userId": "14251",
    "payOrderId": "4501189717",
    "amount": "50",
    "currency": "STAR",
    "extra": "level_5_reward",
    "status": "PAID",
    "sign": "aaccdd3d67266847adf0765f1638141432a3c9b14c34ea4d9dcafd845ed81d52"
  }'
```

**服务端响应示例：**

```json
{
  "code": 0,
  "message": "支付回调处理成功"
}
```

**服务端处理示例 (Node.js)：**

```javascript
app.post('/payment/callback', (req, res) => {
  try {
    const { userId, payOrderId, amount, currency, status, extra, sign } = req.body;

    // 1. 验证签名（如果有）
    if (sign && !verifySignature(req.body, sign)) {
      return res.json({ code: -1, message: '签名验证失败' });
    }

    // 2. 验证订单状态
    if (status === 'PAID') {
      // 处理支付成功逻辑
      await processPaymentSuccess(userId, payOrderId, amount, extra);
    } else if (status === 'REFUND') {
      // 处理退款逻辑
      await processRefund(userId, payOrderId, amount);
    }

    // 3. 返回处理成功
    res.json({ code: 0, message: 'ok' });
  } catch (error) {
    console.error('支付回调处理失败:', error);
    res.json({ code: -1, message: '处理失败' });
  }
});
```

#### 🔐 签名验证机制

> **注意：** STAR 支付目前不提供签名，以下说明仅适用于需要签名验证的支付方式。

**签名生成步骤：**

1. **参数排序**：将请求参数按 key 的 Unicode 升序排列
2. **字符串拼接**：按 `key1=value1&key2=value2` 格式拼接
3. **添加密钥**：在拼接字符串末尾添加 `&key=your_secret_key`
4. **SHA256 加密**：对完整字符串进行 SHA256 哈希
5. **转换格式**：将哈希结果转为 16 进制小写字符串

**签名验证示例：**

```javascript
function verifySignature(params, receivedSign) {
  const { sign, ...signParams } = params;

  // 1. 过滤空值参数
  const filteredParams = Object.keys(signParams)
    .filter((key) => signParams[key] !== "" && signParams[key] !== null)
    .sort()
    .reduce((obj, key) => {
      obj[key] = signParams[key];
      return obj;
    }, {});

  // 2. 拼接参数字符串
  const paramStr = Object.keys(filteredParams)
    .map((key) => `${key}=${filteredParams[key]}`)
    .join("&");

  // 3. 添加密钥
  const originStr = `${paramStr}&key=${SECRET_KEY}`;

  // 4. 计算签名
  const calculatedSign = crypto
    .createHash("sha256")
    .update(originStr, "utf8")
    .digest("hex");

  return calculatedSign === receivedSign;
}
```

**完整签名示例：**

```javascript
// 回调参数
const params = {
  userId: "14251",
  payOrderId: "3342384641",
  amount: "50",
  currency: "USD",
  extra: "50-3342384641-14251",
  status: "PAID",
};

// 拼接字符串（按key排序）
// amount=50&currency=USD&extra=50-3342384641-14251&payOrderId=3342384641&status=PAID&userId=14251

// 添加密钥
// originStr = "amount=50&currency=USD&extra=50-3342384641-14251&payOrderId=3342384641&status=PAID&userId=14251&key=fe68e63bea35f8edeae04daec0ecb724"

// 生成签名
// sign = "8b132d7b91cbeea0abc231afb93f3929f94e9acde04b5ca1dee9ae63564a4979"
```

#### ⚠️ 安全注意事项

1. **验证来源**：建议验证请求来源 IP，确保来自可信服务器
2. **幂等性**：同一订单可能收到多次回调，需要实现幂等性处理
3. **签名验证**：在生产环境中务必验证签名（当提供时）
4. **日志记录**：记录所有回调请求和处理结果，便于问题排查
5. **错误处理**：合理处理异常情况，避免资源泄露
6. **超时设置**：设置合适的数据库和外部服务超时时间

**支付流程：**

1. 创建订单
2. 调用平台支付接口
3. 验证支付结果
4. 消费购买项目
5. 返回最终结果

**注意事项：**

- 订单 ID 必须唯一
- 支付成功后会自动上报购买事件
- 需要服务器端验证支付结果

## 游戏管理

### 1. 切换游戏

```javascript
const result = await gameSDK.switchGame("other_game_id", { level: 5 });
```

### 2. 设置会话数据

```javascript
await gameSDK.setSessionData({ currentLevel: 10, gameMode: "endless" });
```

### 3. 游戏暂停监听

```javascript
gameSDK.onPause(() => {
  console.log("游戏暂停，执行暂停逻辑");
  // 暂停游戏音效、动画等
});
```

### 4. 退出游戏

```javascript
gameSDK.quitGame();
```

## 事件追踪

### 自定义事件追踪

```javascript
gameSDK.trackEvent("level_complete", "level_5", "100", {
  level: 5,
  score: 1000,
  time: 120,
  stars: 3,
});
```

**参数说明：**

- `eventName`: 事件名称
- `eventId`: 事件 ID（可选）
- `valueToSum`: 数值（可选）
- `payload`: 事件数据（对象）

**常用事件类型：**

- `level_complete`: 关卡完成
- `achievement_unlocked`: 成就解锁
- `item_purchased`: 物品购买
- `social_share`: 社交分享
- `tutorial_complete`: 教程完成

## 错误处理

所有异步方法都可能抛出异常，建议使用 try-catch 处理：

```javascript
try {
  const result = await gameSDK.someMethod();
  console.log("成功:", result);
} catch (error) {
  console.error("错误:", error.message);
  // 处理错误情况
}
```

## 最佳实践

### 1. 初始化顺序

```javascript
// 1. 创建SDK实例
const gameSDK = new window.GameSDK(config);

// 2. 初始化SDK
await gameSDK.init();

// 3. 登录用户
const user = await gameSDK.login();

// 4. 预加载广告
const rewardedAd = await gameSDK.loadRewardedVideoAd("ad_id");
```

### 2. 错误处理

- 所有 API 调用都应该有错误处理
- 网络请求失败时提供重试机制
- 给用户友好的错误提示

### 3. 性能优化

- 预加载广告以提高展示速度
- 批量保存游戏数据而非频繁保存
- 合理使用事件追踪，避免过于频繁

### 4. 用户体验

- 在适当时机展示广告
- 提供清晰的支付信息

## 平台兼容性

| 功能     | Facebook Instant | 说明             |
| -------- | ---------------- | ---------------- |
| 用户登录 | ✅               | 仅 Facebook 支持 |
| 数据存储 | ✅               | 仅 Facebook 支持 |
| 广告系统 | ✅               | 仅 Facebook 支持 |
| 支付系统 | ✅               | 仅 Facebook 支持 |
| 分享功能 | ✅               | 仅 Facebook 支持 |
| 事件追踪 | ✅               | 仅 Facebook 支持 |

## 技术支持

如有问题，请联系技术支持团队或查看项目文档。

- **技术支持**：jinbinbin@zencodegame.com
