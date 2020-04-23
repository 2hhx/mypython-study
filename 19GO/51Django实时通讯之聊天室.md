先上图看效果（加867284461群获取demo）

![](https://img2018.cnblogs.com/blog/1825659/201911/1825659-20191111230802358-715746604.gif)

## 第一步：安装模块

    
    
    Django==2.1.4 #channels2.0最低django版本要求是1.11+，python3.5+
    channels==2.1.7
    channels-redis==2.3.3

## 第二步：配置settings.py

    
    
    #1 加入app
    INSTALLED_APPS = [
        ...
        'app01.apps.App01Config',
        'channels',
    ]
    #2 配置channels layer
    ASGI_APPLICATION = 'django_websocket.routing.application' #自己routing的路径
    CHANNEL_LAYERS = {
        'default': {
            'BACKEND': 'channels_redis.core.RedisChannelLayer',
            'CONFIG': {
                "hosts": [('127.0.0.1', 6379)], #需修改redis的地址
            },
        },
    }

## 第三步：配置路由

### 在项目settings文件同级目录中新增routing.py

    
    
    from channels.auth import AuthMiddlewareStack
    from channels.routing import ProtocolTypeRouter, URLRouter
    from app01 import routing
    
    application = ProtocolTypeRouter({
        'websocket': AuthMiddlewareStack(
            URLRouter(
                routing.websocket_urlpatterns# 指明路由文件是django_websocket/routing.py,类似于路由分发
            )
        ),
    })

### 最后在app里配置路由和对应的消费者，(我是app01下的routing.py)：

    
    
    from django.urls import path
    from . import consumers
    
    websocket_urlpatterns = [
        path('ws/chat', consumers.Chatting), #consumers.Chatting 是该路由的消费者
    ]

## 第四步:在app目录下编写consumers.py

    
    
    from channels.generic.websocket import AsyncWebsocketConsumer
    import json
    class Chatting(AsyncWebsocketConsumer):
        async def connect(self):
            self.room_group_name = 'xiaoyuanqujing'
            # 加入聊天室
            await self.channel_layer.group_add(
                self.room_group_name,
                self.channel_name
            )
    
            await self.accept()
    
        async def disconnect(self, close_code):
            # 离开聊天室
            await self.channel_layer.group_discard(
                self.room_group_name,
                self.channel_name
            )
    
        # 通过WebSocket，接收数据
        async def receive(self, text_data):
            text_data_json = json.loads(text_data)
            message = text_data_json['message']
            print(message)
            # Send message to room group
            await self.channel_layer.group_send(
                self.room_group_name,
                {
                    'type': 'chat_message',
                    'message': message
                }
            )
    
        # Receive message from room group
        async def chat_message(self, event):
            message = '匿名用户：' + event['message']
            print('返回')
            # 通过WebSocket发送
            await self.send(text_data=json.dumps({
                'message': message
            }))
    

## 第五步：前端发起websocket请求

    
    
     var chatSocket = new WebSocket(
                'ws://' + window.location.host + '/ws/chat');
    
            chatSocket.onmessage = function (e) {
                var data = JSON.parse(e.data);
                var message = data['message'];
                console.log(message)
                var datamsg=$('#chat-log').val()+message+'\n'
                $('#chat-log').val(datamsg)
            };
    
            chatSocket.onclose = function (e) {
                console.error('Chat socket closed unexpectedly');
            };
            $('#chat-message-submit').click(function () {
                if (chatSocket.readyState === 1) {
                    var message = $('#chat-message-input').val()
                    chatSocket.send(JSON.stringify({
                        'message': message
                    }));
                    $('#chat-message-input').val("")
                } else {
                    console.log("还没有连接")
                }
    
    
            })

## 第六步：启动redis，启动项目

愉快的聊天吧

![image-20191111224741714](https://tva1.sinaimg.cn/large/006y8mN6gy1g8uhi2xaquj31cu0u047c.jpg)

