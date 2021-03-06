# 获取服务器时间

`BaaS.getServerDate()`

通过该接口获取服务器时间，可以防止前端由于用户时间设置错误而导致拿到错误的时间，主要有以下应用场景：

  1. 时间校准。例如前端显示倒计时时，用做基准时间。

  2. 数据查询。防止由于前端拿到错误时间，导致查询到错误的数据。

**示例代码**

```dart
var data = await BaaS.getServerDate();
print(data.time);
```

**返回值说明**

| 属性   | 类型   | 说明     |
|----------|--------|----------|
| time     | string | 服务器时间（ISO 8601），含时区信息，时区信息和应用设置的时区一致 |
 

err 对象结构请参考[错误码和 HError 对象](./error.md)
