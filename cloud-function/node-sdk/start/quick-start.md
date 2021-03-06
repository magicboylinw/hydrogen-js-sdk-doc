# 云函数快速上手

## 如何使用

1.创建和编写云函数：

1.如何创建和编写云函数：

开发者可以登录到知晓云控制台，进入到**引擎-云函数**界面，知晓云提供了一个在线代码编辑器，你可以在上面编写你的云函数。除了使用在线代码编辑器编写云函数，我们还提供了[命令行工具](/cloud-function/cli.md)等渠道，方便用户在本地编辑器编写云函数之后，将云函数文件上传到知晓云。

创建完云函数后为其编写逻辑代码，目前支持的编程语言仅有 `Node.js`，后续我们也会增加对其它语言的支持。

编写完代码，点击提交，你的第一个云函数就诞生啦。

你也可通过命令行工具来进行云函数的创建、编写、编译打包、上传、执行调试。了解更多命令行工具，看[这里](/cloud-function/cli.md)。

2.执行云函数

知晓云提供了多种方式执行云函数——

- 可以在小程序中使用我们的 JS SDK 进行执行；
- 也可以在知晓云控制台中定义触发器，即可将数据表操作、文件操作、支付回调等常见的事件绑定到指定的云函数，当这些事件发生时，对应的云函数即可被执行；
- 还可使用定时任务定时执行。

下面，就让我们通过创建和使用一个 helloWorld 云函数，来展示一下如何使用知晓云的云函数吧。

## hello world 示例

### 在控制台创建云函数

进入知晓云控制台，选择一个应用进入，依次点击「 引擎 」、「 云函数 」、「 添加 」。

![首次进入云函数控制台面板](/images/cloud-function/dashboard-into-first.png)
![云函数控制台面板](/images/cloud-function/dashboard-into.png)

在弹出添加云函数模态框中，选择 `Hello World` 模板，函数命名为 `helloWorld`，点击确认，即可完成云函数的创建。

![创建 hello world 函数](/images/cloud-function/dashboard-hello-world.png)

创建好云函数后，在代码编辑器中，默认会添加以下一段代码，它的作用是当调用该方法后，函数会返回 'hello world'。

```js
exports.main = async function helloWorld(event) {
  try {
    return "hello world"
  } catch (err) {
    throw err
  }
}
```


### 在小程序中触发云函数

云函数创建好后，就可以在你的微信小程序代码中，触发该函数了。在此之前，你得先下载我们的 JS SDK，导入项目并进行初始化，关于这块的内容，可以查看我们的 [JS SDK 文档](/js-sdk/README.md)。触发云函数的示例代码如下：

```js
wx.BaaS.invoke('helloWorld').then(res => {
  console.log(res.data)  // 'hello world'
})
```

### 云函数中使用 Node.js SDK

上面只是简单地返回了一个 'hello world' 字符串，现在让我们使用知晓云的数据存储功能，做些更有意义的事吧。

按照上面创建云函数的方法，我们创建一个 `getArticles` 的函数。然后在数据面板创建一张 article 表，为它添加几条记录，调用我们的云函数 `getArticles` 获取 article 表中的记录。

修改云函数的代码如下：

```js
exports.main = async function getArticles(event, callback) {
  try {
    const tableName = 'article'
    const Article = new BaaS.TableObject(tableName)
    const res = await Article.limit(100).find()
    return res.data.objects
  } catch (err) {
    throw err
  }
}
```

修改小程序中的代码：

```js
wx.BaaS.invoke('getArticles').then(res => {
  console.log(res.data) // articles
})
```
