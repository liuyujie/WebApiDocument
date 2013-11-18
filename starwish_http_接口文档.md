###STARWISH HTTP 接口文档

	1.加密
		所有的接口采用salt加密方式来进行调用
		有两种接口：
			1）一种是采用固定salt加密
			2）另一种是采用服务器返回的固定salt加密
		详细如下：
		固定salt（客户端保存）：
			例如：salt ＝ 123456；
			要传入的参数：name＝wiper , age=12（key排序）
			加密方式：
			salt = md5(name=wiper&age=12&123456)
			调用：
			例如得出的salt为：aaaaddfg23
			调用时要附加上salt = aaaaddfg23
			
		2 加密方式和固定salt一样，区别在于如何获取动态salt
			利用固定salt调用注册（注册时已经同时登录）或者登录接口就会返回动态的salt
			客户端必须把其存到沙盒空间，下次调用使用动态salt调用接口
			

	2.分页
		分页参数：有分页的接口，只需要传page＝n就好。
		如有分页数据会带有这些信息：
		{
    	"data": "",
    	"elapse": 0,
    	"errorCode": 0,
    	"message": "",
    	"result": true,
    	"paging": {
        	total: 1,
        	size: 20,
        	page: 1
        	}
		}
	
	3.访问链接
		http://域名＋url（下方标注的）
		
	4.完成的返回参数
		如果调用正确
		{
    	"data": "",//这里为返回的业务数据
    	"elapse": 0,//这次调用消耗的时候，用于打点，暂时没用
    	"errorCode": 0,//错误码为0
    	"message": "",//message为空字符串
    	"result": true,//结果为true
    	"paging": {//分页参数
        	total: 1,
        	size: 20,
        	page: 1
        	}
		}
		
		如果调用错误
		
		{
    	"data": "",//这里为空
    	"elapse": 0,//这次调用消耗的时候，用于打点，暂时没用
    	"errorCode": 8888,//错误码为0
    	"message": "网络阻塞",//message错误信息的提示
    	"result": false,//结果为false
		}

	5.错误码（）
## 注册

### 提交账号信息(手机注册)
    url： /api/register/phone/step1
    method : post
    加密方式：固定salt
##### 接口描述
    手机用户注册第一步，提交账号信息

##### 输入参数：
field|desc
-----|----
account|手机号 


##### 输出：
field|desc
-----|----
result|true/false
code|状态码（当result为false时，该字段有效）
message|（当result为false时，该字段有效）
data|（当result为true时，该字段有效）｝

    data数据格式为：
    ｛
        type:"Cellphone",/*用户注册方式 */
        success:true|false,/*激活信息是否发送成功*/
        time:234234/* 下次发送激活信息时间 */
    ｝
       
### 重新发送激活信息
    url： /api/register/send-again
    method: post
	加密方式：固定salt
##### 接口描述
    用户注册时用于重新发布激活信息
    
##### 输入参数： 
field|desc
-----|----
account|手机号
##### 输出：
field|desc
-----|----
result|true/false
code|状态码（当result为false时，该字段有效）
message|（当result为false时，该字段有效）
data|（当result为true时，该字段有效）｝

    data数据格式为：
    ｛
        type:"Cellphone",/*用户注册方式 */
        success:true|false,/*激活信息是否发送成功*/
        time:234234/* 下次发送激活信息时间 */
    ｝

### 验证激活码
    url： /api/register/active
    method: post
	加密方式：固定salt
##### 接口描述
		验证手机激活码    
    
##### 输入参数：
field|desc
-----|----
account|账号（手机号）
token|激活码

##### 输出：
field|desc
-----|----
result|ture/false, true表示验证成功,false表示失败
code|状态码
message|提示信息
data|{}

	data数据格式为：
    ｛
        type:"Cellphone",/*用户注册方式 */
        success:true|false,/*注册是否成功*/
        salt:"slkj23423lk4j2oi34"/*登录后的salt*/
        id:"12345678"
    ｝
### 提交账号信息(第三方注册)
    url： /api/register/sns/step1
    method : post
    加密方式：固定salt
##### 接口描述
    第三方用户注册第一步，提交第三方账号信息

##### 输入参数：
field|desc
-----|----
token|token
expireTime|过期时间
type|(1/2/3 weibo/qzone/renren)


##### 输出：
field|desc
-----|----
result|true/false
code|状态码（当result为false时，该字段有效）
message|（当result为false时，该字段有效）
data|（当result为true时，该字段有效）｝

    data数据格式为：
    ｛
        type:"weibo/renren/qzone",/*用户注册方式 */
        salt:"slkj23423lk4j2oi34"/*登录后的salt*/
        id:"12345678"
    ｝


### 提交用户个人信息
    url： /api/register/step2
    method : post
##### 接口描述
    用户注册第二步，提交用户个人信息
    
##### 输入参数： 
field|desc
-----|----
nickname|昵称
gender|性别 （值为1/2,1为女，2为男）
avatar| 头像
get phoneinfo|下面是获取用户的手机信息
areaCode|城市编码（标准城市编码，如果没有则传0）
imei|手机imei
pf|平台（ios/android）
os|系统（例如 android 4.4）



##### 输出：
field|desc
-----|----
result|true/false
code|状态码（当result为false时，该字段有效）
message|（当result为false时，该字段有效）
data|（当result为true时，该字段有效）｝





###上传通讯录
	url： /api/user/contact/upload
	method : post
##### 接口描述
    上传通讯录，匹配手机好友
##### 输入参数：
field|desc
-----|----
contact|通讯录(号码1:号码2:号码3.....)

##### 输出：
field|desc
-----|----
result|true/false
code|状态码（当result为false时，该字段有效）
message|（当result为false时，该字段有效）
data|{}（当result为true时，该字段有效)


###获取手机通讯录好友
	url： /api/user/contact/list
	method : get
##### 接口描述
    获取手机通讯录好友
##### 输入参数：
field|desc
-----|----
id|用户id

##### 输出：
field|desc
-----|----
result|true/false
code|状态码（当result为false时，该字段有效）
message|（当result为false时，该字段有效）
data|{}（当result为true时，该字段有效)
paging|分页信息

	{
    "recommand": [
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
## 登录

### 手机登录
    url： /api/login/phone
    method : post
    加密方式：固定salt
##### 接口描述
    用户注册第一步，提交账号信息

##### 输入参数：
field|desc
-----|----
account|手机号 


##### 输出：
field|desc
-----|----
result|true/false
code|状态码（当result为false时，该字段有效）
message|（当result为false时，该字段有效）
data|（当result为true时，该字段有效）｝

    data数据格式为：
    ｛
        type:"Cellphone",/*用户登录方式 */
        success:true|false,/*激活信息是否发送成功*/
        time:234234/* 下次发送激活信息时间 */
    ｝
       
### 重新发送登录激活信息
    url： /api/login/send-again
    method: post
	加密方式：固定salt
##### 接口描述
    用户注册时用于重新发布激活信息
    
##### 输入参数： 
field|desc
-----|----
account|手机号 
##### 输出：
field|desc
-----|----
result|true/false
code|状态码（当result为false时，该字段有效）
message|（当result为false时，该字段有效）
data|（当result为true时，该字段有效）｝

    data数据格式为：
    ｛
        type:"Cellphone",/*用户注册方式 */
        success:true|false,/*激活信息是否发送成功*/
        time:234234/* 下次发送激活信息时间 */
    ｝

### 验证激活码
    url： /api/login/active
    method: post
	加密方式：固定salt
##### 接口描述
		验证手机激活码    
    
##### 输入参数：
field|desc
-----|----
account|账号（手机号）
token|激活码

##### 输出：
field|desc
-----|----
result|ture/false, true表示验证成功,false表示失败
code|状态码
message|提示信息
data|{}

	data数据格式为：
    ｛
        type:"Cellphone",/*用户注册方式 */
        success:true|false,/*注册是否成功*/
        salt:"slkj23423lk4j2oi34"/*登录后的salt*/
        id:123456/*用户id*/
    ｝
### 第三方登录
    url： /api/login/sns
    method : post
    加密方式：固定salt
##### 接口描述
    用户注册第一步，提交第三方账号信息

##### 输入参数：
field|desc
-----|----
token|token
type|(1/2/3 weibo/qzone/renren)

##### 输出：
field|desc
-----|----
result|true/false
code|状态码（当result为false时，该字段有效）
message|（当result为false时，该字段有效）
data|（当result为true时，该字段有效）｝

    data数据格式为：
    ｛
        type:"weibo/renren/qzone",/*用户注册方式 */
        salt:"slkj23423lk4j2oi34"/*登录后的salt*/
        id:123456/*用户id*/
    ｝
##用户信息

### 获取自己个人信息
	url： /api/user/get
	method : get
##### 接口描述
	获取自己个人信息
##### 输入参数：	
field|desc
-----|----
userId|用户id

##### 输出：
field|desc
-----|----
result|true/false
code|状态码（当result为false时，该字段有效）
message|（当result为false时，该字段有效）
data|（当result为true时，该字段有效）｝

    data数据格式为：
    {
    "type":0/1{0是普通用户，1是明星}
    "id": 123,
    "name": "三文鱼",
    "avatar": "url",
    "gender": 1,
    "birthday": "1989.08.01",
    "description": "我不去想是否能够成功，既然选择了远方便只顾风雨兼程",
    "fans":10,//type为1的时候返回 新增粉丝
    "income":1000,//type为1的时候返回 新增收入
    "groups":10//type为1的时候返回 我的粉丝团数
	}
### 获取其他用户得信息
	url： /api/user/get
	method : get
##### 接口描述
	获取其他用户得信息
##### 输入参数：	
field|desc
-----|----
userId|用户id
friendId|朋友id

##### 输出：
field|desc
-----|----
result|true/false
code|状态码（当result为false时，该字段有效）
message|（当result为false时，该字段有效）
data|（当result为true时，该字段有效）｝

    data数据格式为：
    {
    "type":0/1{0是普通用户，1是明星}
    "relation":0,//用户关系，1，你关注的人，2，关注你的人，3双向关系
    "id": 123,
    "name": "三文鱼",
    "rname":"备注名",
    "avatar": "url",
    "gender": 1,
    "birthday": "1989.08.01",
    "description": "我不去想是否能够成功，既然选择了远方便只顾风雨兼程",
	}

    
###修改用户信息
	url： /api/user/modify
	method : get
##### 接口描述
	获取用户得个人信息
##### 输入参数：	
field|desc
-----|----
userId|用户id
nickname|昵称
age|年龄
birthday|生日
area|区域
description|描述
gender｜性别
avatar|头像
##### 输出：
field|desc
-----|----
result|true/false
code|状态码（当result为false时，该字段有效）
message|（当result为false时，该字段有效）
data|（当result为true时，该字段有效）

###修改好友得备注名
	url： /api/user/modify/friend
	method : get
##### 接口描述
	修改好友得备注
##### 输入参数：	
field|desc
-----|----
userId|用户id
friendId|朋友id
rname|备注名
##### 输出：
field|desc
-----|----
result|true/false
code|状态码（当result为false时，该字段有效）
message|（当result为false时，该字段有效）
data|（当result为true时，该字段有效）

###获取我的收藏
	url： /api/user/list/favorite
	method : get
##### 接口描述
	获取我的收藏
##### 输入参数：	
field|desc
-----|----
userId|用户id

##### 输出：
field|desc
-----|----
result|true/false
code|状态码（当result为false时，该字段有效）
message|（当result为false时，该字段有效）
data|（当result为true时，该字段有效）
paging|分页信息

	[
    {
        "id": 123,
        "message": "我不去想是否能够成功，既然选择了远方便只顾风雨兼程",
        "type": 1,//1是文字，2，图片，3语音
        "createtime": 123981723
    },
    {
        "id": 123,
        "message": "我不去想是否能够成功，既然选择了远方便只顾风雨兼程",
        "type": 1,//1是文字，2，图片，3语音
        "createtime": 123981723
    }
	]

###获取我的团（创建的和参加的）
	url： /api/user/list/groups
	method : get
##### 接口描述
	获取我的团
##### 输入参数：	
field|desc
-----|----
userId|用户id

##### 输出：
field|desc
-----|----
result|true/false
code|状态码（当result为false时，该字段有效）
message|（当result为false时，该字段有效）
data|（当result为true时，该字段有效）
paging|分页信息

	[
    {
        "groupId": 123,
        "groupname": "杨幂官方粉丝团",
        "groupsum": 300,
        "groupmax": 500,
        "level": 3,
        "v":0,//是否认证团0否1是
        "description": "。。。。。。。"
    },
    {
        "groupId": 123,
        "groupname": "杨幂官方粉丝团",
        "groupsum": 300,
        "groupmax": 500,
        "level": 3,
        "v":0,//是否认证团0否1是
        "description": "。。。。。。。"
    }
	]

###获取我的好友列表
	url： /api/user/list/friends
	method : get
##### 接口描述
	获取我的好友列表
##### 输入参数：	
field|desc
-----|----
userId|用户id

##### 输出：
field|desc
-----|----
result|true/false
code|状态码（当result为false时，该字段有效）
message|（当result为false时，该字段有效）
data|（当result为true时，该字段有效）
paging|分页信息

	[
    {
        "id": 123,
        "name": "yangmi",
        "type": 1,
        "relation":0,//用户关系，1，你关注的人，2，关注你的人，3双向关系
        "avatar": "url",
        "description": "。。。。。。。"
    },
    {
        "id": 123,
        "name": "wiper",
        "type": 0,
        "relation":0,//用户关系，1，你关注的人，2，关注你的人，3双向关系
        "avatar": "url",
        "description": "。。。。。。。"
    }
	]


###获取我的关注明星
	url： /api/user/list/star
	method : get
##### 接口描述
	获取我的关注明星
##### 输入参数：	
field|desc
-----|----
userId|用户id

##### 输出：
field|desc
-----|----
result|true/false
code|状态码（当result为false时，该字段有效）
message|（当result为false时，该字段有效）
data|（当result为true时，该字段有效）
paging|分页信息
	
	[
    {
        "id": 123,
        "name": "杨幂",
        "fans":1999,
        "avatar": "url",
        "description": "。。。。。。。"
    },
    {
        "id": 123,
        "name": "郭采洁",
        "avatar": "url",
        "fans":1999,
        "description": "。。。。。。。"
    }
	]

###获取我的通讯录
	url： /api/user/list/contact
	method : get
##### 接口描述
	获取我的关注明星
##### 输入参数：	
field|desc
-----|----
userId|用户id

##### 输出：
field|desc
-----|----
result|true/false
code|状态码（当result为false时，该字段有效）
message|（当result为false时，该字段有效）
data|（当result为true时，该字段有效）
paging|分页信息
	
	{
    "star": [
        {
            "id": 123,
            "name": "杨幂",
            "fans": 1999,
            "avatar": "url",
            "description": "。。。。。。。"
        },
        {
            "id": 123,
            "name": "郭采洁",
            "avatar": "url",
            "fans": 1999,
            "description": "。。。。。。。"
        }
    ],
    "group": [
        {
            "groupId": 123,
            "groupname": "杨幂官方粉丝团",
            "groupsum": 300,
            "groupmax": 500,
            "level": 3,
            "v": 0,
            "description": "。。。。。。。"
        },
        {
            "groupId": 123,
            "groupname": "杨幂官方粉丝团",
            "groupsum": 300,
            "groupmax": 500,
            "level": 3,
            "v": 0,
            "description": "。。。。。。。"
        }
    ],
    "friend": [
        {
            "id": 123,
            "name": "杨幂",
            "type": 1,
            "relation": 0,
            "avatar": "url",
            "description": "。。。。。。。"
        },
        {
            "id": 123,
            "name": "wiper",
            "type": 0,
            "relation": 0,
            "avatar": "url",
            "description": "。。。。。。。"
        }
    ]
	}

###获取我的相册
	url： /api/user/list/gallery
	method : get
##### 接口描述
	获取我的关注明星
##### 输入参数：	
field|desc
-----|----
userId|用户id

##### 输出：
field|desc
-----|----
result|true/false
code|状态码（当result为false时，该字段有效）
message|（当result为false时，该字段有效）
data|（当result为true时，该字段有效）
paging|分页信息

	[
    {
        "userId": 123,
        "groupId": 12313,
        "picture": "url",
        "description": "。。。。。。。"
    },
    {
        "userId": 123,
        "groupId": 12313,
        "picture": "url",
        "description": "。。。。。。。"
    }
	]
###添加我的相册
	url： /api/user/add/gallery
	method : post
##### 接口描述
	添加我的相册
##### 输入参数：	
field|desc
-----|----
userId|用户id
groupId|团id
url|链接
type|类型

##### 输出：
field|desc
-----|----
result|true/false
code|状态码（当result为false时，该字段有效）
message|（当result为false时，该字段有效）
data|（当result为true时，该字段有效）
###删除我的相册
	url： /api/user/delete/gallery
	method : post
##### 接口描述
	添加我的相册
##### 输入参数：	
field|desc
-----|----
userId|用户id
groupId|团id
sid|资源id

##### 输出：
field|desc
-----|----
result|true/false
code|状态码（当result为false时，该字段有效）
message|（当result为false时，该字段有效）
data|（当result为true时，该字段有效）

###修改好友设置
	url： /api/user/config/update
	method : get
##### 接口描述
	修改好友设置（暂时只有申请好友权限设置）
##### 输入参数：	
field|desc
-----|----
userId|用户id
applyPremission|0/1/2 {0允许任何人添加，1发送好友申请，2不允许任何人添加}

##### 输出：
field|desc
-----|----
result|true/false
code|状态码（当result为false时，该字段有效）
message|（当result为false时，该字段有效）
data|（当result为true时，该字段有效）

###获取好友设置
	url： /api/user/config/get
	method : get
##### 接口描述
	获取好友设置（暂时只有申请好友权限设置）
##### 输入参数：	
field|desc
-----|----
userId|用户id
applyPremission|0/1/2 {0允许任何人添加，1发送好友申请，2不允许任何人添加}

##### 输出：
field|desc
-----|----
result|true/false
code|状态码（当result为false时，该字段有效）
message|（当result为false时，该字段有效）
data|（当result为true时，该字段有效）

	{
        "userId": 123,
        "applyPremission": 0,
        //{0允许任何人添加，1发送好友申请，2不允许任何人添加}
    }

###删除好友
	url： /api/user/delete/friend
	method : post
##### 接口描述
	删除好友
##### 输入参数：	
field|desc
-----|----
userId|用户id
friendId|朋友id

##### 输出：
field|desc
-----|----
result|true/false
code|状态码（当result为false时，该字段有效）
message|（当result为false时，该字段有效）
data|（当result为true时，该字段有效）

###添加好友
	url： /api/user/add/friend
	method : post
##### 接口描述
	添加好友
##### 输入参数：	
field|desc
-----|----
userId|用户id
friendId|朋友id
reply|回执
##### 输出：
field|desc
-----|----
result|true/false
code|状态码（当result为false时，该字段有效）
message|（当result为false时，该字段有效）
data|（当result为true时，该字段有效）

###确认添加好友
	url： /api/user/add/confirm
	method : post
##### 接口描述
	确认添加好友
##### 输入参数：	
field|desc
-----|----
userId|用户id
friendId|朋友id


##### 输出：
field|desc
-----|----
result|true/false
code|状态码（当result为false时，该字段有效）
message|（当result为false时，该字段有效）
data|（当result为true时，该字段有效）

###加入黑名单
	url： /api/user/add/blacklist
	method : post
##### 接口描述
	加入黑名单
##### 输入参数：	
field|desc
-----|----
userId|用户id
friendId|朋友id

##### 输出：
field|desc
-----|----
result|true/false
code|状态码（当result为false时，该字段有效）
message|（当result为false时，该字段有效）
data|（当result为true时，该字段有效）

##明星

	基本功能的接口和用户的一样，但是会多一些明星的特有功能
###获取明星的音乐
	url： /api/star/list/music
	method : get
##### 接口描述
	获取明星的音乐
##### 输入参数：	
field|desc
-----|----
userId|用户id

##### 输出：
field|desc
-----|----
result|true/false
code|状态码（当result为false时，该字段有效）
message|（当result为false时，该字段有效）
data|（当result为true时，该字段有效）
paging|分页信息

	[
    {
        "musicId": 123,
        "name": 12313,
        "avatar": "url",
        "type": 1//1是歌曲，2是mv
    },
     {
        "musicId": 123,
        "name": 12313,
        "avatar": "url",
        "type": 1//1是歌曲，2是mv
    }
	]



###获取我的关注明星
	url： /api/star/list/new-star
	method : get
##### 接口描述
	获取我的关注明星
##### 输入参数：	
field|desc
-----|----

##### 输出：
field|desc
-----|----
result|true/false
code|状态码（当result为false时，该字段有效）
message|（当result为false时，该字段有效）
data|（当result为true时，该字段有效）
paging|分页信息
	
	[
    {
        "id": 123,
        "name": "杨幂",
        "fans":1999,
        "avatar": "url",
        "description": "。。。。。。。"
    },
    {
        "id": 123,
        "name": "郭采洁",
        "avatar": "url",
        "fans":1999,
        "description": "。。。。。。。"
    }
	]
###获取明星的聊天内容
	url： /api/star/record
	method : get
##### 接口描述
	获取明星的群聊内容
##### 输入参数：	
field|desc
-----|----
userId|用户id

##### 输出：
field|desc
-----|----
result|true/false
code|状态码（当result为false时，该字段有效）
message|（当result为false时，该字段有效）
data|（当result为true时，该字段有效）
paging|分页信息

	[
    {
        "id": 123,
        "message": "我不去想是否能够成功，既然选择了远方便只顾风雨兼程",
        "type": 1,//1是文字，2，图片，3语音
        "createtime": 123981723
    },
    {
        "id": 123,
        "message": "我不去想是否能够成功，既然选择了远方便只顾风雨兼程",
        "type": 1,//1是文字，2，图片，3语音
        "createtime": 123981723
    }
	]

###获取明星的产品
	url： /api/star/list/product
	method : get
##### 接口描述
	获取明星的产品
##### 输入参数：	
field|desc
-----|----
userId|用户id

##### 输出：
field|desc
-----|----
result|true/false
code|状态码（当result为false时，该字段有效）
message|（当result为false时，该字段有效）
data|（当result为true时，该字段有效）
paging|分页信息v

	[
    {
        "productId": 123,
        "money": 1000,
        "avatar": "url",
        "description": "土豪耳机",
        "color": "red",
        "store": 10//仓存量
    },
    {
        "productId": 123,
        "money": 1000,
        "avatar": "url",
        "description": "土豪耳机",
        "color": "red",
        "store": 10//仓存量
    }
	]
###获取明星收入详情
	url： /api/star/list/income
	method : get
##### 接口描述
	获取明星收入详情
##### 输入参数：	
field|desc
-----|----
userId|用户id

##### 输出：
field|desc
-----|----
result|true/false
code|状态码（当result为false时，该字段有效）
message|（当result为false时，该字段有效）
data|（当result为true时，该字段有效）

	{
    "total": 20001,
    "list": [
        {
            "date": "2013-10-28",
            "money": 2000
        },
        {
            "date": "2013-10-27",
            "money": 1000
        }
    ]
	}
###获取明星粉丝详情
	url： /api/star/list/fans
	method : get
##### 接口描述
	获取明星粉丝详情
##### 输入参数：	
field|desc
-----|----
userId|用户id

##### 输出：
field|desc
-----|----
result|true/false
code|状态码（当result为false时，该字段有效）
message|（当result为false时，该字段有效）
data|（当result为true时，该字段有效）

	{
    "total": 20001,
    "list": [
        {
            "date": "2013-10-28",
            "fans": 2000
        },
        {
            "date": "2013-10-27",
            "fans": 1000
        }
    ]
	}

##粉丝团

###创建粉丝团
	url： /api/group/add
	method : post
##### 接口描述
	创建粉丝团
##### 输入参数：	
field|desc
-----|----
userId|用户id
name|名称
starName|所属明星
avatar|头像

##### 输出：
field|desc
-----|----
result|true/false
code|状态码（当result为false时，该字段有效）
message|（当result为false时，该字段有效）
data|（当result为true时，该字段有效）

###获取单个粉丝团
	url： /api/group/get
	method : get
##### 接口描述
	创建粉丝团
##### 输入参数：	
field|desc
-----|----
groupId|团id

##### 输出：
field|desc
-----|----
result|true/false
code|状态码（当result为false时，该字段有效）
message|（当result为false时，该字段有效）
data|（当result为true时，该字段有效）

	{
    "groupId": 123,
    "name": "粉丝团",
    "avatar": "url",
    "annonce": "公告",
    "description": "简介",
    "group_holder": {
        "avatar": "url",
        "name": "团主",
        "userId": 1234123
    },
    "vInformation": "",
    "v": 0,
    "creattime": "2013-09-08",
    "level": 5,
    "usersCount": 10,
    "usersMax": 500
	}

###邀请用户
	url： /api/group/invite
	method : post
##### 接口描述
	邀请用户
##### 输入参数：	
field|desc
-----|----
userId|用户id
groupId|群id
friends|(用户id1:用户id2)

##### 输出：
field|desc
-----|----
result|true/false
code|状态码（当result为false时，该字段有效）
message|（当result为false时，该字段有效）
data|（当result为true时，该字段有效）

###修改群资料
	url： /api/group/modify
	method : get
##### 接口描述
	修改群资料
##### 输入参数：	
field|desc
-----|----
userId|用户id
groupId|群id
avatar｜头像
description｜描述
annouce｜公告

##### 输出：
field|desc
-----|----
result|true/false
code|状态码（当result为false时，该字段有效）
message|（当result为false时，该字段有效）
data|（当result为true时，该字段有效）


###申请认证
	url： /api/group/verify
	method : post
##### 接口描述
	申请认证
##### 输入参数：	
field|desc
-----|----
userId|用户id
groupId|群id
description｜描述

##### 输出：
field|desc
-----|----
result|true/false
code|状态码（当result为false时，该字段有效）
message|（当result为false时，该字段有效）
data|（当result为true时，该字段有效）

###修改团员备注
	url： /api/group/modify/member-note
	method : post
##### 接口描述
	修改团员备注
##### 输入参数：	
field|described
-----|----
userId|用户id
groupId|群id
memberId|成员id

##### 输出：
field|desc
-----|----
result|true/false
code|状态码（当result为false时，该字段有效）
message|（当result为false时，该字段有效）
data|（当result为true时，该字段有效）

###粉丝团踢人
	url： /api/group/delete/member
	method : post
##### 接口描述
	粉丝团踢人
##### 输入参数：	
field|described
-----|----
userId|用户id
groupId|群id
memberId|成员id

##### 输出：
field|desc
-----|----
result|true/false
code|状态码（当result为false时，该字段有效）
message|（当result为false时，该字段有效）
data|（当result为true时，该字段有效）


###粉丝团解散
	url： /api/group/delete/group
	method : post
##### 接口描述
	修改团员备注
##### 输入参数：	
field|described
-----|----
userId|用户id
groupId|群id

##### 输出：
field|desc
-----|----
result|true/false
code|状态码（当result为false时，该字段有效）
message|（当result为false时，该字段有效）
data|（当result为true时，该字段有效）

##商品


###获取单个商品详细信息
	url： /api/product/get
	method : get
##### 接口描述
	获取单个商品详细信息
##### 输入参数：	
field|described
-----|----
productId|商品id

##### 输出：
field|desc
-----|----
result|true/false
code|状态码（当result为false时，该字段有效）
message|（当result为false时，该字段有效）
data|（当result为true时，该字段有效）

	{
        "productId": 123,
        "money": 1000,
        "avatar": "url",
        "description": "土豪耳机",
        "color": "red",
        "store": 10//仓存量
    }

###获取所有商品
	url： /api/product/list
	method : get
##### 接口描述
	获取所有商品
##### 输入参数：	

##### 输出：
field|desc
-----|----
result|true/false
code|状态码（当result为false时，该字段有效）
message|（当result为false时，该字段有效）
data|（当result为true时，该字段有效）
paging|分页信息	
	
	[
    {
        "productId": 123,
        "money": 1000,
        "avatar": "url",
        "name": "土豪耳机",
        "color": "red",
        "store": 10//仓存量
    },
    {
        "productId": 123,
        "money": 1000,
        "avatar": "url",
        "name": "土豪耳机",
        "color": "red",
        "store": 10//仓存量
    }
	]

###购买商品
	url： /api/user/add／order
	method : post
##### 接口描述
	购买商品
##### 输入参数：	
field|described
-----|----
productId|商品id
userId｜用户id
phone|电话
postcode|邮编
area|区域
description|描述
address|地址
count|购买数量
description｜购买描述
name|真实姓名

##### 输出：
field|desc
-----|----
result|true/false
code|状态码（当result为false时，该字段有效）
message|（当result为false时，该字段有效）
data|（当result为true时，该字段有效）


###获取我的订单（没有完成购买的）
	url： /api/user/list/order
	method : get
##### 接口描述
	获取我的订单（没有完成购买的）
##### 输入参数：	
field|desc
-----|----
userId|用户id

##### 输出：
field|desc
-----|----
result|true/false
code|状态码（当result为false时，该字段有效）
message|（当result为false时，该字段有效）
data|（当result为true时，该字段有效）

	[
    {
        "orderId": 12387,
        "userId": 12345,
        "product": {
            "avatar": "url",
            "name": "土豪耳机",
            "color": "red"
        },
        "money": 3000,
        "count": 3,
        "description": "很屌的耳机"
    },
    {
        "orderId": 12312,
        "userId": 2343,
        "product": {
            "avatar": "url",
            "name": "小清新耳机",
            "color": "red"
        },
        "money": 200,
        "count": 2,
        "description": "耳机"
    }
	]

###获取我单个订单
	url： /api/user/get/order
	method : get
##### 接口描述
	获取我单个订单
##### 输入参数：	
field|desc
-----|----
userId|用户id
orderId|订单id
	
	  {
        "orderId": 12387,
        "userId": 12345,
        "product": {
            "avatar": "url",
            "name": "土豪耳机",
            "color": "red"
        },
        "dealtime": "2013-14-5:19:30:00",
        "money": 3000,
        "count": 3,
        "status": 1,
        "address": "立水桥北京北小区",
        "name": " 张乐",
        "phone": 1828279874
    }

##### 输出：
field|desc
-----|----
result|true/false
code|状态码（当result为false时，该字段有效）
message|（当result为false时，该字段有效）
data|（当result为true时，该字段有效）


###删除购买订单
	url： /api/user/order/delete
	method : post
##### 接口描述
	删除购买商品
##### 输入参数：	
field|described
-----|----
orderId|订单id
userId｜用户id

##### 输出：
field|desc
-----|----
result|true/false
code|状态码（当result为false时，该字段有效）
message|（当result为false时，该字段有效）
data|（当result为true时，该字段有效）


##搜索

###获取我的订单（没有完成购买的）
	url： /api/user/list/order
	method : get
##### 接口描述
	获取我的订单（没有完成购买的）
##### 输入参数：	
field|desc
-----|----
key|关键字

##### 输出：
field|desc
-----|----
result|true/false
code|状态码（当result为false时，该字段有效）
message|（当result为false时，该字段有效）
data|（当result为true时，该字段有效）

	{
    "star": [
        {
            "id": 123,
            "name": "杨幂",
            "fans": 1999,
            "avatar": "url",
            "description": "。。。。。。。"
        },
        {
            "id": 123,
            "name": "郭采洁",
            "avatar": "url",
            "fans": 1999,
            "description": "。。。。。。。"
        }
    ],
    "group": [
        {
            "groupId": 123,
            "groupname": "杨幂官方粉丝团",
            "groupsum": 300,
            "groupmax": 500,
            "level": 3,
            "v": 0,
            "description": "。。。。。。。"
        },
        {
            "groupId": 123,
            "groupname": "杨幂官方粉丝团",
            "groupsum": 300,
            "groupmax": 500,
            "level": 3,
            "v": 0,
            "description": "。。。。。。。"
        }
    ],
    "friend": [
        {
            "id": 123,
            "name": "杨幂",
            "type": 1,
            "relation": 0,
            "avatar": "url",
            "description": "。。。。。。。"
        },
        {
            "id": 123,
            "name": "wiper",
            "type": 0,
            "relation": 0,
            "avatar": "url",
            "description": "。。。。。。。"
        }
    ]
	}

##SNS

### 第三方绑定
    url： /api/sns/bands
    method : post
##### 接口描述
    第三方绑定

##### 输入参数：
field|desc
-----|----
token|token
type|(1/2/3 weibo/qzone/renren)
userId|用户id

##### 输出：
field|desc
-----|----
result|true/false
code|状态码（当result为false时，该字段有效）
message|（当result为false时，该字段有效）
data|（当result为true时，该字段有效）

### 第三方绑定删除
    url： /api/sns/bands
    method : post
##### 接口描述
    第三方绑定删除

##### 输入参数：
field|desc
-----|----
type|(1/2/3 weibo/qzone/renren)
userId|用户id

##### 输出：
field|desc
-----|----
result|true/false
code|状态码（当result为false时，该字段有效）
message|（当result为false时，该字段有效）
data|（当result为true时，该字段有效）

