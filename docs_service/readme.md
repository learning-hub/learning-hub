# 设计文档

## 仓库架构

- learning-hub: 主仓库
  - learning-hub-serive: 后端主业务 *
  - learning-hub-web: 前端主业务
  - learning-hub-admin: 后台管理系统
  - learning-hub-service-judger：判题服务 *
  - learning-hub-service-auth：认证服务 *
  - learning-hub-service-message：消息服务 *
  - learning-hub-service-logger：日志服务
  - learning-hub-service-configure：配置服务
  - learning-hub-service-game：竞赛服务 *

## 系统架构

![image-20200513133838500](%E8%AE%BE%E8%AE%A1%E6%96%87%E6%A1%A3.assets/image-20200513133838500.png)

## 状态码

#### HTTP状态码

| 分类 | 分类描述                                       |
| :--- | :--------------------------------------------- |
| 1**  | 信息，服务器收到请求，需要请求者继续执行操作   |
| 2**  | 成功，操作被成功接收并处理                     |
| 3**  | 重定向，需要进一步的操作以完成请求             |
| 4**  | 客户端错误，请求包含语法错误或无法完成请求     |
| 5**  | 服务器错误，服务器在处理请求的过程中发生了错误 |

| 状态码 | 英文名称                        | 中文描述                                                     |
| :----- | :------------------------------ | :----------------------------------------------------------- |
| 100    | Continue                        | 继续。[客户端](http://www.dreamdu.com/webbuild/client_vs_server/)应继续其请求 |
| 101    | Switching Protocols             | 切换协议。服务器根据客户端的请求切换协议。只能切换到更高级的协议，例如，切换到HTTP的新版本协议 |
|        |                                 |                                                              |
| 200    | OK                              | 请求成功。一般用于GET与POST请求                              |
| 201    | Created                         | 已创建。成功请求并创建了新的资源                             |
| 202    | Accepted                        | 已接受。已经接受请求，但未处理完成                           |
| 203    | Non-Authoritative Information   | 非授权信息。请求成功。但返回的meta信息不在原始的服务器，而是一个副本 |
| 204    | No Content                      | 无内容。服务器成功处理，但未返回内容。在未更新网页的情况下，可确保浏览器继续显示当前文档 |
| 205    | Reset Content                   | 重置内容。服务器处理成功，用户终端（例如：浏览器）应重置文档视图。可通过此返回码清除浏览器的表单域 |
| 206    | Partial Content                 | 部分内容。服务器成功处理了部分GET请求                        |
|        |                                 |                                                              |
| 300    | Multiple Choices                | 多种选择。请求的资源可包括多个位置，相应可返回一个资源特征与地址的列表用于用户终端（例如：浏览器）选择 |
| 301    | Moved Permanently               | 永久移动。请求的资源已被永久的移动到新URI，返回信息会包括新的URI，浏览器会自动定向到新URI。今后任何新的请求都应使用新的URI代替 |
| 302    | Found                           | 临时移动。与301类似。但资源只是临时被移动。客户端应继续使用原有URI |
| 303    | See Other                       | 查看其它地址。与301类似。使用GET和POST请求查看               |
| 304    | Not Modified                    | 未修改。所请求的资源未修改，服务器返回此状态码时，不会返回任何资源。客户端通常会缓存访问过的资源，通过提供一个头信息指出客户端希望只返回在指定日期之后修改的资源 |
| 305    | Use Proxy                       | 使用代理。所请求的资源必须通过代理访问                       |
| 306    | Unused                          | 已经被废弃的HTTP状态码                                       |
| 307    | Temporary Redirect              | 临时重定向。与302类似。使用GET请求重定向                     |
|        |                                 |                                                              |
| 400    | Bad Request                     | 客户端请求的语法错误，服务器无法理解                         |
| 401    | Unauthorized                    | 请求要求用户的身份认证                                       |
| 402    | Payment Required                | 保留，将来使用                                               |
| 403    | Forbidden                       | 服务器理解请求客户端的请求，但是拒绝执行此请求               |
| 404    | Not Found                       | 服务器无法根据客户端的请求找到资源（网页）。通过此代码，网站设计人员可设置"您所请求的资源无法找到"的个性页面 |
| 405    | Method Not Allowed              | 客户端请求中的方法被禁止                                     |
| 406    | Not Acceptable                  | 服务器无法根据客户端请求的内容特性完成请求                   |
| 407    | Proxy Authentication Required   | 请求要求代理的身份认证，与401类似，但请求者应当使用代理进行授权 |
| 408    | Request Time-out                | 服务器等待客户端发送的请求时间过长，超时                     |
| 409    | Conflict                        | 服务器完成客户端的 PUT 请求时可能返回此代码，服务器处理请求时发生了冲突 |
| 410    | Gone                            | 客户端请求的资源已经不存在。410不同于404，如果资源以前有现在被永久删除了可使用410代码，网站设计人员可通过301代码指定资源的新位置 |
| 411    | Length Required                 | 服务器无法处理客户端发送的不带Content-Length的请求信息       |
| 412    | Precondition Failed             | 客户端请求信息的先决条件错误                                 |
| 413    | Request Entity Too Large        | 由于请求的实体过大，服务器无法处理，因此拒绝请求。为防止客户端的连续请求，服务器可能会关闭连接。如果只是服务器暂时无法处理，则会包含一个Retry-After的响应信息 |
| 414    | Request-URI Too Large           | 请求的URI过长（URI通常为网址），服务器无法处理               |
| 415    | Unsupported Media Type          | 服务器无法处理请求附带的媒体格式                             |
| 416    | Requested range not satisfiable | 客户端请求的范围无效                                         |
| 417    | Expectation Failed              | 服务器无法满足Expect的请求头信息                             |
|        |                                 |                                                              |
| 500    | Internal Server Error           | 服务器内部错误，无法完成请求                                 |
| 501    | Not Implemented                 | 服务器不支持请求的功能，无法完成请求                         |
| 502    | Bad Gateway                     | 作为网关或者代理工作的服务器尝试执行请求时，从远程服务器接收到了一个无效的响应 |
| 503    | Service Unavailable             | 由于超载或系统维护，服务器暂时的无法处理客户端的请求。延时的长度可包含在服务器的Retry-After头信息中 |
| 504    | Gateway Time-out                | 充当网关或代理的服务器，未及时从远端服务器获取请求           |
| 505    | HTTP Version not supported      | 服务器不支持请求的HTTP协议的版本，无法完成处理               |

### 注意

- 列表查询为空不属于错误。

## 主要服务

#### 主服务

> 功能：服务监控，服务转发，网关

#### 判题服务

> 功能：选择、填空、判断、程序填空、程序运行的题型判断。

#### 竞赛服务

> 功能：竞赛相关。

#### 认证服务

> 功能：用户的身份认证、包括登录注册、权限分配

#### 配置服务

> 功能：配置管理，配置注入

#### 消息服务

> 功能：消息通知、邮件通告

#### 日志服务

> 功能：日志记录、数据统计

## JWT设计

```js
{
    payload:{
        roleId:Number, //角色ID
        userId:Number, //用户ID
    },
}
```

## API说明

### 公共响应

```js
// 错误
{
    errcode:ErrCode, //错误码
    errmsg:String, //错误信息
}
    
// 成功
{
    // 数据
}
```

### 分页与过滤

所有API的响应中如果存在数组类型的数据都应该有分页功能,具体格式如下。

请求：

```js
params = {
	pageNum:Number, //页码,[1,∞)
    pageSize:Number, //每页的个数
    order:String, // <field>[#{ASC|DESC}]
    filter:String, // <field>[:like]#<查询字符串>
}
```

响应：

```js
{
    list:[],
	page:{
        num,
        size,
        total,//总页数
        count,//总条数
    }
}
```

## API（草稿，后期要删除）

#### 登录

接口: POST /api/v1/login

条件: 无

请求:

```js
{
	username:String,//*用户名
    password:String,//*密码
    code:String,//验证码
}
```

响应:

```js
{
		user:{
            id:Int
            schoolId:Int,//学校ID
            roleId:String, //角色ID
            name:String, //姓名
            nick:String, //昵称
            sex:String,//性别
            email:String, //电子邮箱
            author:String, //头像
            note:String, // 备注说明，或者个性签名,
            username:String, //用户名
            isOnline:Boolean, //是否在线
            acCount:Int, //AC题数
            submitCount:Int, //提交数
            gameCount:Int, //参加比赛数
            reward:Int, //积分
            exp:Int, //经验值
        },
        role:{
			router:{
                <api>:<weigth>
            }
        }
}
```

#### 注册

接口: POST /api/v1/register

条件：null

请求:

```js
{
    nick:String, //昵称
    username:String, //学号
    password:String, //密码
    email:String. // 电子邮箱
    sex:String, //性别
    code:String, //验证码
}
```

响应

```js
{
    data:{
		user:{
            id:Int
            schoolId:Int,//学校ID
            roleId:String, //角色ID
            name:String, //姓名
            nick:String, //昵称
            sex:String,//性别
            email:String, //电子邮箱
            author:String, //头像
            note:String, // 备注说明，或者个性签名,
            username:String, //用户名
            isOnline:Boolean, //是否在线
            acCount:Int, //AC题数
            submitCount:Int, //提交数
            gameCount:Int, //参加比赛数
            reward:Int, //积分
            exp:Int, //经验值
        },
        role:{
			router:{
                <api>:<weigth>
            }
        }
    }
}
```

#### 获取用户列表

接口: GET /api/v1/users

条件：指定权限的用户才可操作

请求：

```js
{
	...
} 
```

响应:

```js
{
    data:[
        {
            id:Int,
            schoolId:Int,//学校ID
            roleId:String, //角色ID
            name:String, //姓名
            nick:String, //昵称
            sex:String,//性别
            email:String, //电子邮箱
            author:String, //头像
            note:String, // 备注说明，或者个性签名,
            username:String, //用户名
            isOnline:Boolean, //是否在线
            acCount:Int, //AC题数
            submitCount:Int, //提交数
            gameCount:Int, //参加比赛数
            reward:Int, //积分
            exp:Int, //经验值
        } 
    ],
    ...
}
```

#### 获取指定用户信息

接口: GET /api/v1/users/:userId

条件：指定权限的用户才可操作

请求：null

响应:

```js
{
    data:{
        id:Int,
        schoolId:Int,//学校ID
        roleId:String, //角色ID
        name:String, //姓名
        nick:String, //昵称
        sex:String,//性别
        email:String, //电子邮箱
        author:String, //头像
        note:String, // 备注说明，或者个性签名,
        username:String, //用户名
        isOnline:Boolean, //是否在线
        acCount:Int, //AC题数
        submitCount:Int, //提交数
        gameCount:Int, //参加比赛数
        reward:Int, //积分
        exp:Int, //经验值
    }
} 
```

#### 用户修改个人信息

接口: PUT /api/v1/users/:userId

条件：指定权限的用户才可操作

请求：

```js
{
        nick:String, //昵称
        email:String, //电子邮箱
        author:String, //头像
        note:String, // 备注说明，或者个性签名,
}
```

响应:

```js
{
    data:{
        id:Int,
        schoolId:Int,//学校ID
        roleId:String, //角色ID
        name:String, //姓名
        nick:String, //昵称
        sex:String,//性别
        email:String, //电子邮箱
        author:String, //头像
        note:String, // 备注说明，或者个性签名,
        username:String, //用户名
        isOnline:Boolean, //是否在线
        acCount:Int, //AC题数
        submitCount:Int, //提交数
        gameCount:Int, //参加比赛数
        reward:Int, //积分
        exp:Int, //经验值
    }
}
```

#### 获取题库列表

接口：GET /api/v1/problems

条件：拥有相应的权限

请求：

```js
{
    type:String // 题类型
    ...
}
```

相应：

```js
{
	data:[
       {
            id:Int,
            title:String,
            content:String, //内容
            hint:String, //提示
            source:String, //来源
            tags:String, //标签 ["算法","普通"]
            hard:Int, //难度
            acCount:Int, //AC数
            submitCount:Int, //提交数
            order:Int, //排序值
        }
  	],
    ...
}
```

#### 获取指定ID的题

接口：GET /api/v1/problems/:problemId

条件：拥有相应的权限

请求：null

相应：

```js
{
	data:{
            id:Int,
            title:String,
            content:String, //内容
            hint:String, //提示
            source:String, //来源
            tags:String, //标签 ["算法","普通"]
            hard:Int, //难度
            acCount:Int, //AC数
            submitCount:Int, //提交数
            order:Int, //排序值
            ... // 根据不同的题类型返回， 但是不要返回与答案有关的内容
   }
}
```



#### 获取竞赛列表

接口： GET /api/v1/games

条件：拥有相应的权限

请求：

```js
{
    ...
}
```

响应：

```js
{
	data:[
        {
            id:Int,
            title:String,
            context:String,
            startTime:String,
            endTime:String,
            isPrivate:String, //是否私有
            password:String, //进入密码
            message:String, //公共消息
            otherContext:OtherContext, //其他
            limitNum:String， //限制人数
        }
	]
}
```



#### 获取竞赛列表

接口： GET /api/v1/games/:gameId

条件：拥有相应的权限

请求：

```js
{
    ...
}
```

响应：

```js
{
	data:[
       {
            id:Int,
            title:String,
            content:String, //内容
            hint:String, //提示
            source:String, //来源
            tags:String, //标签 ["算法","普通"]
            hard:Int, //难度
            acCount:Int, //AC数
            submitCount:Int, //提交数
            order:Int, //排序值
        }
  	],
}
```



#### 获取指定竞赛id的竞赛题目

接口：GET /api/v1/games/:gameId/problems

条件：拥有相应的权利

请求：

```
{
	...
}
```

响应：

```js
{
	data:[
       {
            id:Int,
            title:String,
            content:String, //内容
            hint:String, //提示
            source:String, //来源
            tags:String, //标签 ["算法","普通"]
            hard:Int, //难度
            acCount:Int, //AC数
            submitCount:Int, //提交数
            order:Int, //排序值
        }
  	],
    ...
}
```

#### 操作指定竞赛id的竞赛题目

接口：GET /api/v1/games/:gameId/problems/:problemId

条件：拥有相应权力

请求：null

响应：

```js
{
	data:{
            id:Int,
            title:String,
            content:String, //内容
            hint:String, //提示
            source:String, //来源
            tags:String, //标签 ["算法","普通"]
            hard:Int, //难度
            acCount:Int, //AC数
            submitCount:Int, //提交数
            order:Int, //排序值
            ... // 根据不同的题类型返回， 但是不要返回与答案有关的内容
   }
   ...
}
```

#### 获取指定竞赛id的竞赛人员

接口：GET /api/v1/games/gameId/users

条件：需要相应的权限

请求：

```
{
	...
}
```

响应：

```js
{
	data:[{
            id:Int,
            name:String, //姓名
            nick:String, //昵称
            sex:String,//性别
            author:String, //头像
            note:String, // 备注说明，或者个性签名,
            username:String, //用户名
            acCount:Int, //AC题数
            submitCount:Int, //提交数
	}]
}
```

#### 获取公告信息

接口：GET	/api/v1/messages

条件：拥有响应的权限

请求：

```js
{
    ...
}
```

响应：

```js
{
	data:[
		{
            id:Int,
            type:String,
            sendUserName:String,
            title:String,
            createdAt:String
		}
	],
    ...
}
```

#### 获取指定ID公告信息

接口：GET	/api/v1/messages/:messageId

条件：拥有响应的权限

请求：

```js
{
    ...
}
```

响应：

```js
{
	data:[
		{
            id:Int,
            type:String,
            sendUserName:String,
            title:String,
            context:String,
            createdAt:String
		}
	],
    ...
}
```



#### 发送信息

接口：POST /api/v1/sendMessage

条件：拥有相应权力

请求：

```js
{
    userId:Int, //给哪个用户发送
}
```

响应：

```js
{
	data:{
            id:Int,
            type:String,
            acceptUserId:String,
            title:String,
            context:String,
            createdAt:String
	}
}
```

#### 提交程序题

接口：POST /api/v1/submitProblem

条件：拥有权限

请求：

```js
{
	problemId:Int,
	language:Int, //使用语言
    type:String, //题的类型，select,fill,code
	ip:String,
	gameId:Int,//竞赛的id 如果等于0 则表示普通提交
	text:String,//源码
}
```

响应：

```js
{
	data:{
		resultId:Int, //运行id
	}
}
```

#### 获取程序运行列表

接口：GET /api/v1/results

条件：权限

请求：

```js
{
    ...
}
```

响应：

```js
{
	data:[
		{
            id:Int(),
            problemId: Int(10), //题Id
            userId: Int(10), // 用户ID
            gameId: Int(), //竞赛ID
            lang:Int(), //使用的语言
            code:TEXT(), //用户代码，或者填空，选择填的内容
            codeLen:Int(), //代码长度
            passRate:Int(), //通过率
            type:Varchar(10), //题的类型，select,fill,code
            time:Int(10), //耗费多少毫秒
            memory:Int(10), //耗费多少B空间
            status:Int(), //状态码
            ip:Varchar(), //IP地址
		}
	]
}
```

#### 获取程序运行

接口：GET /api/v1/results/:resultId

条件：权限

请求：

```js
{
    ...
}
```

响应：

```js
{
	data:
		{
            id:Int,
            problemId: Int(10), //题Id
            userId: Int(10), // 用户ID
            gameId: Int(), //竞赛ID
            lang:Int(), //使用的语言
            code:TEXT(), //用户代码，或者填空，选择填的内容
            codeLen:Int(), //代码长度
            passRate:Int(), //通过率
            type:Varchar(10), //题的类型，select,fill,code
            time:Int(10), //耗费多少毫秒
            memory:Int(10), //耗费多少B空间
            status:Int(), //状态码
            ip:Varchar(), //IP地址
		}
}
```


