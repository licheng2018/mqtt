# mqtt
input command: git push -u origin master

install: 
 1. 安装与使用
1.1 在项目中安装

MQTT.jsnpm包名为mqtt，安装命令如下：

npm install mqtt --save

使用示例

使用MQTT.js实现一个消息发布者及订阅者：

var mqtt    = require('mqtt');
var client  = mqtt.connect('mqtt://test.mosquitto.org');
 
client.on('connect', function () {
  //订阅presence主题
  client.subscribe('presence');
  //向presence主题发布消息
  client.publish('presence', 'Hello mqtt');
});
 
client.on('message', function (topic, message) {
  //收到的消息是一个Buffer
  console.log(message.toString());
  client.end();
});

输出：

Hello mqtt

在上例中test.mosquitto.org是官方提供的MQTT测试服务器，还可以使用test.mosca.io网址进行测试。

了解更多关于MQTT相关介绍请参考：MQTT协议－MQTT协议简介及协议原理


1.2 做为命令行工具使用

MQTT.js支持以命令行模式与服务端通信，以命令行模式使用时，模块需要全局安装：

npm install mqtt -g

安装后，就可以在控制台使用：

mqtt sub -t 'hello' -h 'test.mosquitto.org' -v

下面是另一种使用方式：

mqtt pub -t 'hello' -h 'test.mosquitto.org' -m 'from MQTT.js'

更多命令行使用方法可以执行mqtt help 命令查看。


1.3 在浏览器中使用

在浏览器环境中什么MQTT.js首先需要按AMD或CommandJs规范导出。

Browserify导出

Browserify模块可以将MQTT.js绑定为浏览器可用的，也可以导出为一个单独的浏览器端模块。其导出的模块兼容AMD或CMD标准，且会在添加对象到全局空间。

npm install -g browserify // 安装 browserify 
cd node_modules/mqtt
npm install . // 安装 dev dependencies 
browserify mqtt.js -s mqtt > browserMqtt.js // 需要在客户端使用的mqtt


Webpack导出

与Browserify类似，该模块也会导出一个浏览器端使用的类库，且会在添加对象到全局空间，导出后可以像var mqtt = xxx这样使用MQTT.js。导出时可以设置AMD、CMD或其它导出格式。

npm install -g webpack // install webpack 
 
cd node_modules/mqtt
npm install . // install dev dependencies 
webpack mqtt.js ./browserMqtt.js --output-library mqtt

完成导出后，就可以像在Node中那样使用mqtt.js的API：

<html>
<head>
  <title>test Ws mqtt.js</title>
</head>
<body>
<script src="./browserMqtt.js"></script>
<script>
  var client = mqtt.connect(); // you add a ws:// url here
  client.subscribe("mqtt/demo");

  client.on("message", function(topic, payload) {
    alert([topic, payload].join(": "));
    client.end();
  });

  client.publish("mqtt/demo", "hello world!");
</script>
</body>
</html>

