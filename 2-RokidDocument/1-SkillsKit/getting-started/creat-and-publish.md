### 目录
* [一、准备工作](#一、准备工作)
* [二、创建技能](#二、创建技能)
* [三、填写技能信息](#三、填写技能信息)
* [四、语音交互](#四、语音交互)
  * [自定义语音交互](#自定义语音交互)
  * [预定义语音交互](#预定义语音交互)
* [五、后端服务配置](#五、后端服务配置)  
  * [选择后端服务方式](#选择后端服务方式)
  * [配置服务](#配置服务)
  * [授权设置](#授权设置)
* [六、集成测试](#六、集成测试)
  * [添加测试设备](#添加测试设备)
  * [SSML语音调试](#ssml语音调试)
  * [后端服务测试](#后端服务测试)
* [七、发布](#七、发布)
* [八、隐私与合规](#八、隐私与合规)

### 一、准备工作

#### 设计您的技能

一个好的技能离不开出色的语音交互，这需要您对自然语言、人类对话的基本原理有简单的理解。请阅读[Rokid语音交互指南](/2-RokidDocument/1-SkillsKit/rokid-voice-interaction-guidelines.md)以了解如何设计出色的语音交互体验。



### 二、创建技能

#### 注册Rokid开发者账号
在[Rokid开放平台](https://developer.rokid.com/)免费注册一个Rokid开发者账号。

![](images/00-注册.jpg)


#### 创建技能
登录 Rokid 开发平台后选择图中红框里标出的「创建技能」。

![](images/00-创建技能.jpg)

进入到如下界面，点击「开始创建」。

![](images/00-开始创建.jpg)



### 三、填写技能信息

**如下图所示：**
![](images/01-技能信息.jpg)


#### 设置技能的开放性
填写技能信息的第一步，就是设置所建技能的开放性。您可以设置创建的技能为**`公开技能`**，向所有设备开放；也可以设置为**`私有技能`**，仅面向经过您授权的设备开放。

**`公开技能`**：公开属性的技能将会对所有搭载 Rokid 语音解决方案的设备开放，终端用户可以通过技能商店轻松开启您开发的技能。

**`私有技能`**：私有属性的技能无法向所有用户开放，仅向经过您授权的设备开放。

- 注意：在创建**`私有技能`**时还需选择是创建**`本地私有技能`**还是**`云端私有技能`**。创建**`本地私有技能`**需要写一个apk推送到设备上（具体参照：https://github.com/Rokid/NewsDemo）；创建**`云端私有技能`**，则后台配置服务不在设备上而是在另外搭建的服务端上。不管本地还是云端，都需要遵守我们的[协议格式](/3-ApiReference/cloud-app-development-protocol_cn.md)进行通信。



#### 选择技能类型：自定义

**`自定义技能`**支持专业开发者设计符合自己业务场景的语音交互流程和业务处理逻辑，创建出功能强大的技能。

**注意**：此处用户只能选择**`自定义技能`**；若要调用**`预定义技能`**，请您在下一步设置【语音交互】时，点击【预定义语音交互】，然后选择您想要的【预定义语音交互配置】，点击【引用】按钮即可。具体操作见：[语音交互](voice-interaction.md)

#### 技能名称

技能的名称会在技能商店中向终端用户展示。名称长度应介于2-15个字之间，不能包含特殊字符。

#### 入口词

**`入口词(唤醒词)`**是用来唤醒技能的词语。如果用户想使用非 Rokid 官方技能，必须对若琪设备说：“若琪，打开‘**入口词**’”，技能才会被调用。

> 例如：使用「来个笑话」这个技能前，必须对若琪说：“若琪，打开来个笑话” ，该技能才会被启用。

#### 场景化展示：选择非场景化
技能在被对话打断后仍能继续恢复运行，即为**`场景化展示`**，否则即为**`非场景化展示`**。目前，非Rokid官方开发者目前只能选择非场景化展示。

> 比如播放音乐时，询问天气，天气播报完成以后仍会继续播放音乐。则该播放音乐的技能为场景化技能。 天气播报技能为非场景化技能。



### 四、语音交互

语音交互主要定义三个要素：[意图](../important-concept/intend.md)、[词表](../important-concept/word-list.md)、[用户语句](./important-concept/usersays.md) 

用户既可以选择【自定义语音交互】，自行配置意图、词表、用户语句等内容；也可以选择【预定义语音交互】，直接调用Rokid定义好的语音交互配置。如下图所示。

![](images/02-语音交互-自定义-预定义.png)



#### 自定义语音交互

##### 意图定义

自定义语音交互首先需要进行意图定义。

**`意图`**指用户说话的目的，即用户想要表达什么、想做什么。

> 如用户说“今天天气怎么样？”，意图就是“查询天气”；用户说“我想订一张火车票”，意图就是“买火车票”。

用户的对话中可能含有多个意图，这些意图按照概率的大小进行排序。

**定义一个意图要设置三个参数：**

- **`intent`**:表示意图名称。

  如下模板所示，必须用英文定义意图名称（`"intent": "play"`）。详细介绍见：[意图](../important-concept/intend.md)

- **`slots`**:表示词表，包括上述意图所依赖的词表名称和内容。

  用户既可以自定义词表内容，也可以直接引用预定义词表。详细介绍见：[词表](../important-concept/word-list.md)

- **`user_says`**:表示用户语句。用户对于某一个意图，可能有各种各样的表达方式，这些不同的表达就是用户语句。详细介绍见：[用户语句](../important-concept/usersays.md)

如下所示（填写时可复制图片下方的模板, 然后根据自己的需求修改`intent`、`slots`、`user_says`）：

![](images/02-语音交互.jpg)

**模版**

```
{
  "intents": [
    {
      "intent": "play",
      "slots": [],
      "user_says": [
        "开始播放",
        "开始说吧",
        "开始乐吧",
        "播放笑话"
      ]
    }
  ]
}
```



##### 自定义词表内容

如果您在上一步【意图定义】中选择了自定义词表内容，那么您还需要在【语音交互】配置页面添加自定义词表。

###### 进入自定义词表添加页面

在语音交互配置页面下方【自定义词表内容】处，点击【添加词条】即可进入自定义词表添加页面。

![](images/02-语音交互-词表1.png)



###### 添加自定义词表

用户可以通过【手动输入】和【文件导入】两种方式添加自定义词表。

**方式一：手动输入**

![](images/02-语音交互-词表2.png)

**方式二:通过文件导入**

该方式仅支持txt的文件导入，且文件格式编码须为UTF-8格式。

![](images/02-语音交互-词表3.png)

##### 语音交互测试

完成意图定义以后选择【保存】--->【开始编译】。编译通过后在右侧的“语音交互测试”中输入定义好的意图，单击“测试”开始测试，正确结果应该如下图所示。

![](images/02-语言交互with测试.jpg)



#### 预定义语音交互

在创建技能时，用户也可以直接使用【预定义语音交互配置】。

##### 具体操作

在【语音交互】配置页面，点击【预定义语音交互】，并选择想要的具体配置，直接点击引用即可。如下图所示：

![](images/02-语音交互-预定义.png)

使用【预定义语音交互配置】，用户不需要自己设置意图、词表、用户语句等内容，仅需在后端服务中直接实现对应的意图即可。



### 五、后端服务配置

#### 选择后端服务方式

后端服务可选择Rokid Force(无需后台服务器,详情请见[Rokid Force System使用指南](../rokid-force-system-tutorial.md))或者HTTPS([Java开发后端服务示例](https://github.com/Rokid/rokid-skill-sample/tree/master/rokid-skill-sample-java))。

为快速编写后端服务，优先选择Rokid Force为示例，并点击【配置服务】进入服务配置页面。
![](images/03-配置-RokidForce.jpg)



#### 配置服务

![](images/03-配置Nodejs代码-.jpg)

##### 1）填写【服务名称】和【服务描述】

在配置页面中，填写【服务名称】和【服务描述】。

##### 2）编辑脚本

将下方的模板复制到编辑框，在上图左下角的红框里依次修改自己的**`欢迎词` **和**`意图`**（注意:此处的意图名称需与语音交互里的意图名称一致）以及tts对应的文本。

##### 3）保存

点击页面右上角的会出现与技能关联成功的弹框，单击弹框里的技能名称返回技能创建列表。了解更多请参考 [Rokid Force JS指南](../rokid-force-js-tutorial.md)。

返回技能列表，配置完成。
![](images/03-配置完成.jpg)

##### 模版

```
  exports.handler = function(event, context, callback) {
      var rokid = Rokid.handler(event, context,callback);
      rokid.registerHandlers(handlers);
      rokid.execute();
  };

  var handlers = {
  'ROKID.INTENT.WELCOME':function() {
      try {
          this.setTts({tts:'立马讲个笑话'});
          this.emit(':done');
      } catch (e) {
          this.emit(':error', e);
      }
  },
  'play':function() {
      try {
          this.setTts({tts:'自然课老师问:“为什么人死后身体是冷的?”没人回答。老师又问:“没人知道吗?”这时,教室后面有人说:“那是因为心静自然凉”。'});
          this.emit(':done');
      } catch (e) {
          this.emit(':error', e);
      }
      }
  };
```



#### 授权设置

选择【是否需要用户授权】，默认为否，了解更多请参考[Rokid Oauth 使用指南](../rokid-oauth.md)。



### 六、集成测试

#### 添加测试设备

在“添加测试设备”下拉框选择“添加账号下绑定设备”将自动关联您手机账号下绑定的设备。然后您可以用自动关联的设备对技能进行测试。示例如下：
```
用户：若琪，打开讲个笑话。
若琪设备：立马讲个笑话。
用户：开始播放
若琪设备：自然课老师问:“为什么人死后身体是冷的?”没人回答。老师又问:“没人知道吗?”这时,教室后面有人说:“那是因为心静自然凉。”
```
#### SSML语音调试

SSML语音调试可以自定义语音输出内容的语音语调。如图所示，点击“播放语音”可以测试tts文本的语音语调。若需调整或者自定义语音语调可参照[SSML使用指南](../ssml-document.md), 然后在技能创建的第三步“配置”中修改“配置服务”里的代码。



#### 后端服务测试 

在后端服务测试的“输入语句”中输入用户语句，比如“开始播放”。如下图，将会得到Rokid语音服务器解析的Json服务请求和相应的服务返回数据。

![](images/04-集成测试.jpg)

### 七、发布

如图所示，按照要求将【技能商城分类】、【测试说明】、【国家和地区】、【技能摘要】、【技能描述】、【用户语句示例】、【关键词】、【技能图标】填写完整。然后选择【保存】，进入下一步。
![](images/05-发布.png)



### 八、隐私与合规

按需填写，勾选内容合规声明，保存后点击【提交审核】提交发布申请，等待Rokid审核通过后技能即可发布上线。
![](images/06-隐私与合规.png)

