# metasota-xiezuocat-java
对智能纠错、智能改写、AI写作、单点登录签名算法进行封装<br />
关于具体接口的调用方法与参数说明，请访问[写作猫API文档中心](https://apidocs.xiezuocat.com/guide/)
## 一、maven引入
```xml
<dependency>
    <groupId>com.xiezuocat</groupId>
    <artifactId>metasota-xiezuocat-java</artifactId>
    <version>1.0.0</version>
</dependency>
```

## 二、调用示例
### 1、智能纠错
#### 调用示例
```java
Xiezuocat xiezuocat = new Xiezuocat({your-secretKey});
String result = xiezuocat.check("{\"texts\":[\"河北省赵县的洨河上，有一座世界文明的石拱桥，叫安济桥，又叫赵州桥。\", \"它是隋朝的石匠李春涉及和参加建造的，到现在已经有一千三百多年了。\"]}");
System.out.println(result);
```
#### 返回结果
```json
{
  "errCode" : 0,
  "errMsg" : "",
  "data" : null,
  "alerts" : [ [ {
    "alertType" : 4,
    "alertMessage" : "建议用“闻名”替换“文明”。",
    "sourceText" : "文明",
    "replaceText" : "闻名",
    "start" : 15,
    "end" : 16,
    "errorType" : 1,
    "advancedTip" : false
  } ], [ {
    "alertType" : 4,
    "alertMessage" : "建议用“设计”替换“涉及”。“涉及”指关联到，牵涉到。“设计”指根据一定要求﹐对某项工作预先制定图样﹑方案。",
    "sourceText" : "涉及",
    "replaceText" : "设计",
    "start" : 9,
    "end" : 10,
    "errorType" : 1,
    "advancedTip" : false
  } ] ],
  "checkLimitInfo" : {
    "checkWordCountLeftToday" : "997085",
    "totalCheckWordCountLeft" : "997084",
    "expireDate" : "2024-02-02 00:00:00"
  }
}
```
### 2、智能改写
##### 调用示例
```java
Xiezuocat xiezuocat = new Xiezuocat({your-secretKey});
String result = xiezuocat.rewrite("{\"items\":[\"河北省赵县的洨河上，有一座世界文明的石拱桥，叫安济桥，又叫赵州桥。它是隋朝的石匠李春涉及和参加建造的，到现在已经有一千三百多年了。\"], \"level\": \"middle\"}");
```
##### 返回结果
```json
{
  "errcode" : 0,
  "errmsg" : null,
  "items" : [ "河北省赵县境内，有一座名为“安济桥”的“赵州桥”，是一座举世闻名的“世界文化遗产”。这座桥由李春参与修建，距今已有1300年之久。" ],
  "stat" : "997020"
}
```

### 3、AI写作
#### 创建生成任务
##### 调用示例
```java
Xiezuocat xiezuocat = new Xiezuocat({your-secretKey});
JSONObject postData = new JSONObject();
postData.put("type", "Step");
postData.put("title", "仿生人会梦见电子羊吗");
postData.put("length", "default");
String result = xiezuocat.generate(JSON.toJSONString(postData));
System.out.println(result);
```
##### 返回结果
```json
{
  "errCode" : 0,
  "errMsg" : "success",
  "data" : {
    "docId" : "3f8c9722-8683-4f93-8745-26cef524ee70"
  }
}
```

#### 获取生成结果
##### 调用示例
```java
Xiezuocat xiezuocat = new Xiezuocat({your-secretKey});
String docId = "3f8c9722-8683-4f93-8745-26cef524ee70"; // 此处docId为第一步生成的结果
String result = xiezuocat.getGenerateResult(docId);
System.out.println(result);
```
##### 返回结果
```json
{
  "errCode" : 0,
  "errMsg" : "success",
  "data" : {
    "status" : "FINISHED",
    "result" : "仿生人会梦见电子羊吗\n如果说仿生人是人类智慧的结晶，那么仿生人究竟是什么呢？简单地说，仿生人就是通过模仿人类特征，达到和人类一样的感知、思维能力的机器。随着人工智能技术的发展，很多人工智能装置不断问世，比如仿生人。从字面意思上理解可能很简单，但它和普通人类有什么不同呢？我们都知道自然界中动物是用四肢行走、说话、唱歌等方式进行交流的，而如果我们让动物来做类似的事情呢？显然是不现实的。\n1.人和动物的区别\n人是一个独立的生物，从解剖学上看，人不是一个整体的存在，它有两个不同的部分：躯体和大脑。人是由两个部分组成。除了身体之外，大脑是人类思维的中心，它负责记忆、思考等工作。而人体则由四根神经和两根血管组成，这四根神经分别负责身体四肢以及全身。这就造成了人类只能感知到有限的信息（感知信息和理解信息），而仿生人则能更全面地感知、思考这些信息。\n2.仿生人的大脑\n仿生人的大脑，是从动物大脑中模仿出来的，但不属于自然动物。比如德国仿生人的大脑，它与人脑结构极为相似，但它却有着人类大脑无法比拟的优势。仿生人的大脑分为三个部分：视觉皮层、触觉皮层。它的每一个部分都是一个微型系统，主要负责处理视觉和听觉信号。仿生人利用这一系统对图像进行分析和判断后输出指令。目前这一系统已经成功应用于临床手术中，能够完成许多复杂动作：抓取物体、识别形状、寻找物品，甚至识别人本身等。\n3.仿生人的大脑是如何工作的\n仿生人的大脑其实很简单，只有四个小的芯片，其中一个用于处理来自各个传感器的信号。这就是为什么在大脑工作时，它能够准确地感知周围环境的变化。在这里我们就不介绍仿生人的大脑是如何工作的了，它也只能模拟动物，却不能完全地模拟人。\n4.仿生人是否会梦见电子羊\n有很多关于仿生人梦见电子羊的传闻，比如：仿生人看到了电子羊的身体，以为它是个机械人；仿生人梦见了电子羊，于是就将它当成了人类；仿生人梦见了电子羊，结果就真的变成了人类；如果说仿生人有什么特征的话，那就是感知能力和思维方式。虽然这些传闻都是以讹传讹，但我们可以看到一个事实——很多事情并非想象中那么简单！其实在自然界中，动物也经常做一些类似的梦。比如：动物会做一些梦，如狼会做梦等等。但是仿生人却不一样，我们看到的不是真实的自己；而是在脑海中做出来的梦境；而这一切都与仿生人所处环境和身体构造有关。\n",
    "wordCount" : "948",
    "restCount" : "118683"
  }
}
```
### 4、单点登录签名算法
##### 调用示例
```java
Xiezuocat xiezuocat = new Xiezuocat({your-secretKey});
String result = xiezuocat.getSSOSignature({your-appId}, {your-uid});
```
##### 返回结果
```json
eyJzaWduIjoiMjNkZDA2ZDJjOWUyN2M1MGY2OWQyMTU2MGY5ZWZhY2I2NTRiMTg4MWRkNjZhNjE3ZTViYzJmZGUxMTJkZjA2NiIsInVpZCI6ImEiLCJhcHBJZCI6Inh4eCIsInRpbWVzdGFtcCI6IjE2ODA1MDIwMzQ1MzYifQ==
```
##### 拿到签名之后访问下述URL即可登录写作猫
```js
// p为签名算法生成的结果
https://xiezuocat.com/api/open/login?p=eyJzaWduIjoiMjNkZDA2ZDJjOWUyN2M1MGY2OWQyMTU2MGY5ZWZhY2I2NTRiMTg4MWRkNjZhNjE3ZTViYzJmZGUxMTJkZjA2NiIsInVpZCI6ImEiLCJhcHBJZCI6Inh4eCIsInRpbWVzdGFtcCI6IjE2ODA1MDIwMzQ1MzYifQ==
```
