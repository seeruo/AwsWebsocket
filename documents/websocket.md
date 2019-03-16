# AWS WebSocket使用探索笔记

## 测试代码部署
1. 使用 [simple-websockets-chat-app](https://serverlessrepo.aws.amazon.com/applications/arn:aws:serverlessrepo:us-east-1:729047367331:applications~simple-websockets-chat-app) 模版直接部署了一个测试样本

2. 选择Api-gateway产品, 今日之后选择创建api

3. 选择websocket协议,取名"MyChat", 设置路由表达式(Route Selection Expression)为$request.body.active，保存

4. 添加一条自定义route取名“sendmessage”，并指定请求执行的lambda函数(函数在第一步中已经自动部署了); 
其他的$connect等预留函数也需要执行响应函数, 如果没有指定，会抛错误。

5. 在设置完API之后，需要部署WebSocket API
在控制面板中选择"MyChat"，在显示"Active"的地方选择 Deploy Api，并部署为dev阶段；
到此为止，aws上的配置完成，可以在"MyChat"的控制面板中插件wss地址

## 测试
1. 安装测试工具wscat
```bash
$ npm install -g wscat
```

2. 在控制台上，通过执行以下命令连接到已发布的API端点：
```bash
$ wscat -c wss://{YOUR-API-ID}.execute-api.{YOUR-REGION}.amazonaws.com/dev
```

3. 如果连接通了就可以向服务器发送消息了,其中要包含action(前面部署3设置的表达式)
```
{"action":"sendmessage", "data":"this is send code"}
```

