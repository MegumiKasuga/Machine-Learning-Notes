注册
```
{
	"service_code": 2
	"content": {
		"id": "UUID"
		"token": 1145141919810(用户token)
	}
}
```
登录
```
{
	"service_code": 1
	"content": {
		"id": "UUID",
		"token": 1145141919810(用户token)
	}
}
```
服务器返回值(成功)
```
{
	"state_code": 200
	"content": {
		"msg": "success"
	}
}
```
服务器返回值(失败)
```
{
	"state_code": 200
	"content": {
		"msg": "failed",
		"cause": "原因"
	}
}
```
服务器返回值(其他失败)
```
{
	"state_code": 4xx
	"content": {
		"msg": "错误信息"
	}
}
```

### 获取Chat
```
{
	"service_code": 3
	"content": {
		"token": 1145141919810(用户token),
		"chat": 1919810(所请求的chat的id),
		"index": 请求的消息的index(如要最近一条消息开始，填-1),
		"count": 20(请求的消息条数)
	}
}
```
返回(正确)
```
{
	"state_code": 200
	"content": {
		"values": [
			{
				"id": "消息发送者的UUID",
				"content": "消息的内容",
				"index": 114(消息的index)
				"time": 1919810(时间戳)
			}
			{
				"id": "消息发送者2的UUID",
				"content": "消息的内容2",
				"index": 113(消息的index)
				"time": 1919809(时间戳)
			}
			...
			{
				"id": "消息发送者n的UUID",
				"content": "消息的内容n",
				"index": 19x(消息的index)
				"time": 1919800(时间戳)
			}
		]
	}
}
```
### 找Chat
```
{
	"service_code": 4
	"content": {
		"token": 1145141919810(用户token)
	}
}
```
返回(正确)
```
{
	"state_code": 200
	"content": {
		"values": [
			{
				"id": 114514(chat的ID),
				"name": "会话名字",
				"type": "chat"/"user"/"service"
				"usrs": [
					{
						"id": "用户UUID"
					}
					{
						"id": "用户UUID"
					}
					...
					{
						"id": "用户UUID"
					}
				]
			}
			{
				"id": 1919810(chat的ID),
				"name": "会话名字2",
				"is_group": true/false
				"usrs": [
					{
						"id": "用户UUID"
					}
					{
						"id": "用户UUID"
					}
					...
					{
						"id": "用户UUID"
					}
				]
			}
			...
			{
				"id": 1145141919(chat的ID),
				"name": "会话名字3",
				"is_group": true/false
				"usrs": [
					{
						"id": "用户UUID"
					}
					{
						"id": "用户UUID"
					}
					...
					{
						"id": "用户UUID"
					}
				]
			}
		]
	}
}
```

### 收发
```
{
	"service_code": 5
	"content": {
		"token": 1145141919(userToken),
		"chat": 1919810(chatId),
		"msg": "message"
	}
}
```
收
```
{
	"state_code": 200
	"content": {
		"msg": "success",
		"index": 114(消息的index)
	}
}
```
### 加入群聊
```
{
	"service_code": 6
	"content": {
		"token": 1145141919(userToken)
		"chat": 1919810(chatId)
	}
}
```
收
```
{
	"state_code": 200
	"content": {
		"msg": "success"
	}
}
```

### 创建群聊
```
{
	"service_code": 7
	"content": {
		"token": 1145141919810(userToken)
		"name": "25岁，是__(chat的名字)"
		"is_group": true/false
		"member": [
			{
				"id": "uuid1"
			}
			{
				"id": "uuid2"
			}
			...
			{
				"id": "uuidn"
			}
		]
	}
}
```
### 接收消息
```
{
	"service_code": 9
	"content": {
		"token": 1145141919(userToken),
		"chat": 1919810(chatId),
		"index": 若从给定的index开始读，就填index号，若要从后往前拿，就填负数(如-1就是从最近的一个msg拿),
		"count": 要拿的消息条数
	}
}
```
收
```
{
	"state_code": 200
	"content": {
		"msg": "success"
		"values": [
			{
				"id": "uuid1",
				"name": "user_name",
				"content": "内容1",
				"index": 114,
				"time": 时间戳
			}
			{
				"id": "uuid2",
				"name": "user_name",
				"content": "内容2",
				"index": 115
				"time": 时间戳
			}
			...
			{
				"id": "uuidn",
				"name": "user_name",
				"content": "内容n",
				"index": 919,
				"time": 时间戳
			}
		]
	}
}
```