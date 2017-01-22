我的接口文档

[TOC]

## HTTP 接口文档

### 1.全局参数介绍

```
1.访问链接:
	域名: api.tom.com
	http://域名＋URL（下方标注的）
	示例: http://api.tom.com/app/userlogin
	
2.返回数据JSON示例:
	如果返回数据正确:
	{
	    "head": {
	        "status": 1,
	        "msg": "OK!"
	    },
	    "data": {  
	    }
	}	

	如果返回数据错误:
	{	
	    "head": {
	        "status": 0,
	        "errorCode":404
	        "msg": "接口不存在!"
	    }
	}

3.错误码(errorCode):
	404：接口不存在
	500：服务器代码错误
	
4.接口加密:
	所有的接口采用salt加密方式来进行调用
	有两种接口：
		(1)一种是采用固定salt加密
		(2)另一种是采用服务器返回的固定salt加密
	详细如下：
		(1)固定salt（客户端保存）：
			例如：salt ＝ 123456；
			要传入的参数：name＝wiper,age=12（key排序升序）
			加密方式：
			salt = md5(age=12&name=wiper&123456)
			调用：
			例如得出的salt为：aaaaddfg23
			调用时要附加上salt = aaaaddfg23
		(2)加密方式2和固定salt一样，区别在于如何获取动态salt
			利用固定salt调用注册（注册时已经同时登录）或者登录接口就会返回动态的salt。
			客户端必须把其存到沙盒空间，下次调用使用动态salt调用接口。
	
5.分页数据JSON示例:
	分页参数：有分页的接口，只需要传 page＝n 就好。
	如有分页数据会带有这些信息：
	{
	    "head": {
	        "status": 1,
	        "msg": "OK!"
	    },
	    "data": {
	        "list": [
	            {
	                "name": "name1",
	                "image": "http://f.chechong.com/",
	                "url": "http://itunes.appl"
	            },
	            {
	                "name": "2name",
	                "image": "http://f.chechong.cog",
	                "url": "https://itunes"
	            }
	        ],
	        "paging": {
	            "total": 200,
	            "size": 20,
	            "page": 1,
	            "hasnext": true
	        }
	    }
	}
```

### 公共返回参数说明

Parameter|Type|Description
-----|-----|-----
status|int|0或1（0 代表失败，1 代表成功）
errorCode|int|状态码（当result为 0 时，该字段有效）
msg|string|错误信息（当result为 0 时，该字段有效）
data|object|数据（当result为 1 时，该字段有效）


--------------------------------------------------------------		
## 注册模块

### 1.提交账号信息(手机注册)

```
URL :  /api/register/phone/step1
Method :  Get
加密方式 : 固定salt
    
接口描述:
手机用户注册第一步，提交账号信息
```

**输入参数：**

Parameter|Required|Type|Description
-----|-----|-----|-----
account|YES|string|手机号 

**输出参数：**

Parameter|Type|Description
-----|-----|-----
type |int|用户注册方式 
success|bool|激活信息是否发送成功(true 或 false)
time |string|下次发送激活信息时间

**JSON：**

```
"data":
{
   "type":"Cellphone",
   "success":true,
   "time":234234
}
```
--------------------------------------------------------------		
### 2.获取用户信息

```
URL: /api/user/getinfo
Method : Get
加密方式: 动态salt
  
接口描述:
通过用户ID获得用户信息
```

**输入参数：**

Parameter|Required|Type|Description
-----|-----|-----|-----
account|YES|string|手机号 

**输出参数：**

Parameter|Type|Description
-----|-----|-----
type |int|用户注册方式 
success|bool|激活信息是否发送成功(true 或 false)
time |string|下次发送激活信息时间

**JSON：**

```
"data":
{
    "list": [
        {
            "id": 123,
            "name": "wiper",
            "avatar": "url",
            "gender": 1,
            "phoneName": "wiper"
        },
        {
            "id": 123,
            "name": "三文鱼",
            "avatar": "url",
            "gender": 1,
            "phoneName": "三文鱼"
        }
    ]
}    
```
--------------------------------------------------------------

### 3.获得评估师信

```
URL: /api/user/getinfo
Method : Get
加密方式 : 动态salt
    
接口描述:
通过用户ID获得用户信息
```

**输入参数：**

Parameter|Required|Type|Description
-----|-----|-----|-----
account|YES|string|手机号 

**输出参数：**

Parameter|Type|Description
-----|-----|-----
type |int|用户注册方式 
success|bool|激活信息是否发送成功(true 或 false)
time |string|下次发送激活信息时间

**JSON：**

```
"data":
{
    "list": [
        {
            "id": 123,
            "name": "wiper",
            "avatar": "http://img.tom.com/user21.jpg",
            "gender": 1,
            "phoneName": "wiper"
        },
        {
            "id": 123,
            "name": "三文鱼",
            "avatar": "http://img.tom.com/user12.jpg",
            "gender": 1,
            "phoneName": "三文鱼"
        }
    ]
}    
```
--------------------------------------------------------------

