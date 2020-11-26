#  Learning Hub权限管理开发文档

### 需求

- 实现用户登陆注册
- 实现用户组的功能
- 实现JWT验证
- 实现路由权限验证
- 实现角色继承

### 功能

- [x] 登陆
- [x] 注册
- [x] 验证码
- [x] JWT验证
- [x] 路由权限验证
- [x] 更新JWT
- [x] 获取用户（群查，单查）
- [x] 用户修改自己的账号信息
- [x] 创建用户组
- [x] 添加成员加入用户组
- [x] 获取用户组（群查，单查）
- [x] 设置用户组
- [x] 删除用户组
- [x] 获取用户组里的用户（群查，单查）
- [x] 获取角色（群查，单查）
- [x] 添加角色
- [x] 删除角色
- [x] 更改角色
- [x] 为某个角色添加权限
- [x] 删除某个角色的权限
- [x] 获取某个角色的权限
- [x] 更改某个用户的角色
- [x] 获取路由
- [x] 添加路由
- [x] 删除路由
- [x] 更新路由

### 错误码

```js
1000 未知故障
1001 用户名不存在
1002 密码错误
1003 验证码错误
1004 token无效
1005 token过期
1006 登录过期
1007 重复请求
1008 用户名已存在
1009 邮箱不正确
1010 账号被禁用
1011 原密码错误
1012 没有权限
1013 角色不存在
1014 父角色不存在
1015 路由不存在
```

### 角色与权限

#### 角色

> 权限的载体

##### 管理员

> 管理系统，管理用户、权限分配、管理数据、管理资源
>
> 拥有教师的权限，可管理所有教师的班级，可拥有学生的权限
>
> 互斥角色：匿名

##### 教师

> 管理班级、管理题目、管理考试
>
> 注意：只能管理自己创建的数据 
>
> 互斥角色：学生、匿名

##### 学生

> 修改个人信息、参加考试、提交题目
>
> 互斥角色：匿名

##### 匿名

> 查看题目、查看公开的考试、查看公开的竞赛、查看公告
>
> 互斥角色：其他角色

#### 权限结构

- 路由权限
  - 请求类型
  - 路由
  - URL参数
  - Body参数

### 注意

数据库名与数据库文件名同名



### API `/auth`

#### 获取验证码

接口: GET ` /api/v1/getVerification?uuId`

条件:无

请求:无

响应:定义response输出类型为image/jpeg类型，使用response输出流输出图片的byte数组

#### 登录

接口:  POST `/api/v1/login?uuId`

条件: 无

请求:

```js
{
		username:String,		//*用户名
    password:String,		//*密码
    verification:String,	//验证码  
}
```

响应:

```js
{
  	errcode:int 					//状态码
  	errmsg:String					//状态信息
  	data:{
        user: {
            id: int,					//user_id
            is_disabled: int,	//默认0可以用，1禁用
            is_del: int,				//默认0可以用，1删除
            created_at: Date,	//创建时间
            updated_at: Date, //更新时间
            school_id: int,		//学校id
            role_id: int,				//角色id
            realname: String,	//姓名
            nick: String,			//昵称
            sex: char,				//性别
            email: String,		//电子邮箱	
            avatar: String,		//头像	
            note: String,			//备注，个性签名
            username: String,	//用户名
            ac_count: int,		//ac数
            submit_count: int,//提交数
            game_count: int,	//参加比赛的数
            reward: int,			//积分
            exp: int					//经验值
  					role:{
  							id:int						//roleId
  							is_disabled:int		//默认0可以用，1禁用
  							is_del: int,			//默认0可以用，1删除
            		created_at: Date,	//创建时间
            		updated_at: Date, //更新时间
  							name:String				//role名字
  							info:String				//role介绍
  							parent_id:int			//role的父id
						}
        },
        /**   protocol 					type     router  数字
              协议如:http      请求类型		路由正则  0表示可以用，1表示禁用了   	**/	
        permission:"HTTP/1.1 GET /api/v1/?/a 0, HTTP/1.1 GET /api/v1/getTi 0"
		}
  	token:String			//token
}
```

#### 注册

接口: POST `/api/v1/register`

条件：null

请求:

```js
{
        realname: String,	//姓名
    		nick: String,			//昵称
        sex: char,				//性别
        email: String,		//电子邮箱	
        avatar: String,		//头像	
        note: String,			//备注，个性签名
        username: String,	//用户名
        password: String	//密码
}
```

响应

```js
{
  	errcode:int 					//状态码
  	errmsg:String					//状态信息
    data:{
  			user: {
            id: int,					//user_id
            is_disabled: int,	//默认0可以用，1禁用
            is_del: int,				//默认0可以用，1删除
            created_at: Date,	//创建时间
            updated_at: Date, //更新时间
            school_id: int,		//学校id
            role_id: int,				//角色id
            realname: String,	//姓名
            nick: String,			//昵称
            sex: char,				//性别
            email: String,		//电子邮箱	
            avatar: String,		//头像	
            note: String,			//备注，个性签名
            username: String,	//用户名
            ac_count: int,		//ac数
            submit_count: int,//提交数
            game_count: int,	//参加比赛的数
            reward: int,			//积分
            exp: int					//经验值
  					role:{
  							id:int						//roleId
  							is_disabled:int		//默认0可以用，1禁用
  							is_del: int,			//默认0可以用，1删除
            		created_at: Date,	//创建时间
            		updated_at: Date, //更新时间
  							name:String				//role名字
  							info:String				//role介绍
  							parent_id:int			//role的父id
						}
        }
		}	
}
```

#### 添加用户

接口：POST `/api/v1/user`

条件：NULL

请求：

```js
{
				role_id:int				//角色id
        realname: String,	//姓名
    		nick: String,			//昵称
        sex: char,				//性别
        email: String,		//电子邮箱	
        avatar: String,		//头像	
        note: String,			//备注，个性签名
        username: String,	//用户名
        password: String	//密码
}
```

响应：

```js
{
  	errcode:int 					//状态码
  	errmsg:String					//状态信息
    data:{
  			user: {
            id: int,					//user_id
            is_disabled: int,	//默认0可以用，1禁用
            is_del: int,				//默认0可以用，1删除
            created_at: Date,	//创建时间
            updated_at: Date, //更新时间
            school_id: int,		//学校id
            role_id: int,				//角色id
            realname: String,	//姓名
            nick: String,			//昵称
            sex: char,				//性别
            email: String,		//电子邮箱	
            avatar: String,		//头像	
            note: String,			//备注，个性签名
            username: String,	//用户名
            ac_count: int,		//ac数
            submit_count: int,//提交数
            game_count: int,	//参加比赛的数
            reward: int,			//积分
            exp: int					//经验值
  					role:{
  							id:int						//roleId
  							is_disabled:int		//默认0可以用，1禁用
  							is_del: int,			//默认0可以用，1删除
            		created_at: Date,	//创建时间
            		updated_at: Date, //更新时间
  							name:String				//role名字
  							info:String				//role介绍
  							parent_id:int			//role的父id
						}
        }
		}	
}
```

#### 获取用户列表

接口: GET `/api/v1/users`

条件：指定权限的用户才可操作

请求：无

响应:

```js
{
  	errcode:int 					//状态码
  	errmsg:String					//状态信息
		data:{
  			users: [{
            id: int,					//user_id
            is_disabled: int,	//默认0可以用，1禁用
            is_del: int,				//默认0可以用，1删除
            created_at: Date,	//创建时间
            updated_at: Date, //更新时间
            school_id: int,		//学校id
            role_id: int,				//角色id
            realname: String,	//姓名
            nick: String,			//昵称
            sex: char,				//性别
            email: String,		//电子邮箱	
            avatar: String,		//头像	
            note: String,			//备注，个性签名
            username: String,	//用户名
            ac_count: int,		//ac数
            submit_count: int,//提交数
            game_count: int,	//参加比赛的数
            reward: int,			//积分
            exp: int					//经验值
  					role:{
  							id:int						//roleId
  							is_disabled:int		//默认0可以用，1禁用
  							is_del: int,			//默认0可以用，1删除
            		created_at: Date,	//创建时间
            		updated_at: Date, //更新时间
  							name:String				//role名字
  							info:String				//role介绍
  							parent_id:int			//role的父id
						}
        }]	
		}
  	...,
}
```

#### 获取指定用户信息

接口: GET `/api/v1/user/:userId`

条件：指定权限的用户才可操作

请求：无

响应:

```js
{
  	errcode:int 					//状态码
  	errmsg:String					//状态信息
  	data:{
  			user: {
            id: int,					//user_id
            is_disabled: int,	//默认0可以用，1禁用
            is_del: int,				//默认0可以用，1删除
            created_at: Date,	//创建时间
            updated_at: Date, //更新时间
            school_id: int,		//学校id
            role_id: int,				//角色id
            realname: String,	//姓名
            nick: String,			//昵称
            sex: char,				//性别
            email: String,		//电子邮箱	
            avatar: String,		//头像	
            note: String,			//备注，个性签名
            username: String,	//用户名
            ac_count: int,		//ac数
            submit_count: int,//提交数
            game_count: int,	//参加比赛的数
            reward: int,			//积分
            exp: int					//经验值
  					role:{
  							id:int						//roleId
  							is_disabled:int		//默认0可以用，1禁用
  							is_del: int,			//默认0可以用，1删除
            		created_at: Date,	//创建时间
            		updated_at: Date, //更新时间
  							name:String				//role名字
  							info:String				//role介绍
  							parent_id:int			//role的父id
						}
        }
		}
} 
```

#### 用户修改个人信息

接口: PUT `/api/v1/user/:userId`

条件：指定权限的用户才可操作

请求：

```js
{
        realname: String,	//姓名
    		nick: String,			//昵称
        sex: char,				//性别
        email: String,		//电子邮箱	
        avatar: String,		//头像	
        note: String,			//备注，个性签名
        username: String,	//用户名
}
```

响应:

```js
{
    errcode:int 					//状态码
 		errmsg:String					//状态信息
		data:{
    		user: {
            id: int,					//user_id
            is_disabled: int,	//默认0可以用，1禁用
            is_del: int,				//默认0可以用，1删除
            created_at: Date,	//创建时间
            updated_at: Date, //更新时间
            school_id: int,		//学校id
            role_id: int,				//角色id
            realname: String,	//姓名
            nick: String,			//昵称
            sex: char,				//性别
            email: String,		//电子邮箱	
            avatar: String,		//头像	
            note: String,			//备注，个性签名
            username: String,	//用户名
            ac_count: int,		//ac数
            submit_count: int,//提交数
            game_count: int,	//参加比赛的数
            reward: int,			//积分
            exp: int					//经验值
  					role:{
  							id:int						//roleId
  							is_disabled:int		//默认0可以用，1禁用
  							is_del: int,			//默认0可以用，1删除
            		created_at: Date,	//创建时间
            		updated_at: Date, //更新时间
  							name:String				//role名字
  							info:String				//role介绍
  							parent_id:int			//role的父id
						}
        } 	
    }
}
```

#### 管理员修改用户信息

接口: PUT `/api/v1/addUser/:userId`

条件：指定权限的用户才可操作

请求：

```js
{
        realname: String,	//姓名
    		nick: String,			//昵称
        sex: char,				//性别
        email: String,		//电子邮箱	
        avatar: String,		//头像	
        note: String,			//备注，个性签名
        username: String,	//用户名
        password:String		//密码
}
```

响应：

```js
{
    errcode:int 					//状态码
 		errmsg:String					//状态信息
		data:{
    		user: {
            id: int,					//user_id
            is_disabled: int,	//默认0可以用，1禁用
            is_del: int,				//默认0可以用，1删除
            created_at: Date,	//创建时间
            updated_at: Date, //更新时间
            school_id: int,		//学校id
            role_id: int,				//角色id
            realname: String,	//姓名
            nick: String,			//昵称
            sex: char,				//性别
            email: String,		//电子邮箱	
            avatar: String,		//头像	
            note: String,			//备注，个性签名
            username: String,	//用户名
            ac_count: int,		//ac数
            submit_count: int,//提交数
            game_count: int,	//参加比赛的数
            reward: int,			//积分
            exp: int					//经验值
  					role:{
  							id:int						//roleId
  							is_disabled:int		//默认0可以用，1禁用
  							is_del: int,			//默认0可以用，1删除
            		created_at: Date,	//创建时间
            		updated_at: Date, //更新时间
  							name:String				//role名字
  							info:String				//role介绍
  							parent_id:int			//role的父id
						}
        } 	
    }
}
```

#### 用户修改密码

接口: PUT `/api/v1/user/:userId/password`

条件：指定权限的用户才可操作

请求：

```js
{
  	oldPassword:String	//原密码
    newPassword:String	//新密码
}
```

响应

```js
{
  	errcode:int 					//状态码
  	errmsg:String					//状态信息
}
```

#### 删除用户

接口：DELETE `/api/v1/usre/:userId`

条件：拥有响应的权限

请求：NUL l

响应：

```js
{
  		errcode:int 				//状态码
      errmsg:String				//状态信息
}
```

####JWT验证

接口：GET `/api/v1/tokenVerification`

条件：拥有响应的权限

请求头header：

```js
{
		token:String //token
}
```

响应：

```js
{
  	errcode:int 					//状态码
  	errmsg:String					//状态信息
}
```

#### JWT更新

接口：GET `/api/v1/updateToken`

条件：拥有响应的权限

请求头header：

```js
{
		token:String 	//token
}
```

响应：

```js
{
  	errcode:int 					//状态码
  	errmsg:String					//状态信息
    data:{
      	token:String 					//token
    }
}
```

#### 路由权限验证

接口：POST `/api/v1/routerVerification`

条件：拥有响应的权限

请求：

```js
{
  		protocol:String	//协议
      type:String			//请求类型
      router:String		//路由
}
```

响应：

```js
{
  	 	errcode:int 					//状态码
  		errmsg:String					//状态信息
}
```

#### 创建用户组

接口：POST `/api/v1/addGroup`

条件：拥有相应的权限

请求：

```js
{
  		name:String		//姓名
      info:String 	//介绍
}
```

响应：

```js
{
  	errcode:int 				//状态码
    errmsg:String				//状态信息
    data:{
      	group:{
          id:int							//用户组id
          is_disabled: int,		//默认0，1禁用
          is_del: int,				//默认0，1删除
          created_at: Date,		//创建时间
          updated_at: Date, 	//更新时间
          name:String					//姓名
          info:String 				//介绍
          create_user_id:int 	//userId
      	}  
    }   
}
```

#### 获取用户组列表

接口：GET `/api/v1/groups`

条件：拥有相应的权限

请求：无

响应：

```js
{
  	errcode:int 				//状态码
    errmsg:String				//状态信息
		data:{
      groups:[{
        id:int							//用户组id
        is_disabled: int,		//默认0，1禁用
        is_del: int,				//默认0，1删除
        created_at: Date,		//创建时间
        updated_at: Date, 	//更新时间
        name:String					//姓名
        info:String 				//介绍
        create_user_id:int 	//userId
      }]  
    }	
  	...,
}
```

#### 获取指定用户组

接口：GET `/api/v1/group/:groupId`

条件：拥有相应的权限

请求：无

响应：

```js
{
  	errcode:int 				//状态码
    errmsg:String				//状态信息
    data:{
      group:{
        id:int							//用户组id
        is_disabled: int,		//默认0，1禁用
        is_del: int,				//默认0，1删除
        created_at: Date,		//创建时间
        updated_at: Date, 	//更新时间
        name:String					//姓名
        info:String 				//介绍
        create_user_id:int 	//userId
      } 
    } 
}
```

#### 添加成员到用户组

接口：POST `/api/v1/addUserToGroup`

条件：拥有相应的权限

请求：

```js
{
  		group_id:int		//组id
      user_id:int	//用户id			
}
```

响应：

```js
{
  		errcode:int 				//状态码
      errmsg:String				//状态信息
}
```

#### 删除用户组中的某个用户

接口：DELETE `/api/v1/deleteUserToGroup`

条件：拥有相应的权限

请求：

```js
{
  		group_id:int		//组id
      user_id:int	//用户id			
}
```

响应：

```js
{
  		errcode:int 				//状态码
      errmsg:String				//状态信息
}
```

#### 设置用户组

接口：PUT `/api/v1/group/:groupId`

条件：拥有相应的权限

请求：

```js
{
  			name:String					//姓名
        info:String 				//介绍
        create_user_id:int 	//userId
}
```

响应：

```js
{
  		errcode:int 				//状态码
      errmsg:String				//状态信息
      group:{
        id:int							//用户组id
        is_disabled: int,		//默认0，1禁用
        is_del: int,				//默认0，1删除
        created_at: Date,		//创建时间
        updated_at: Date, 	//更新时间
        name:String					//姓名
        info:String 				//介绍
        create_user_id:int 	//userId
      }  
}
```

#### 删除用户组

接口：DELETE `/api/v1//group/:groupId`

条件：拥有相应的权限

请求：无

响应：

```js
{
  		errcode:int 				//状态码
      errmsg:String				//状态信息
}
```

#### 获取单个用户组中的用户

接口：GET  `/api/v1/userGroup/:groupId`

条件：拥有相应的权限

请求：无

响应：

```js
{
  	errcode:int 					//状态码
  	errmsg:String					//状态信息
    users:[
        {
          id: int,					//user_id
          is_disabled: int,	//默认0，1禁用
          is_del: int,				//默认0，1删除
          created_at: Date,	//创建时间
          updated_at: Date, //更新时间
          school_id: int,		//学校id
          role_id: int,				//角色id
          realname: String,	//姓名
          nick: String,			//昵称
          sex: char,				//性别
          email: String,		//电子邮箱	
          avatar: String,		//头像	
          note: String,			//备注，个性签名
          username: String,	//用户名
          ac_count: int,		//ac数
          submit_count: int,//提交数
          game_count: int,	//参加比赛的数
          reward: int,			//积分
          exp: int					//经验值
      	}
    ]
}
```

#### 获取角色列表

接口：GET  `/api/v1/roles`

条件：拥有相应的权限

请求：无

响应：

```js
{
  	errcode:int 					//状态码
  	errmsg:String					//状态信息
		data:{
      	roles:[{
          id: int,					//角色id
          is_disabled: int,	//默认0，1禁用
          is_del: int,				//默认0，1删除
          created_at: Date,	//创建时间
          updated_at: Date, //更新时间
          name:String				//角色名字
          info:String				//角色介绍
          parent_id:int			//角色父id
      	}]
    }
  ...,
}
```

#### 获取指定角色

接口：GET  `/api/v1/role/:roleId`

条件：拥有相应的权限

请求：无

响应：

```js
{
  	errcode:int 					//状态码
  	errmsg:String					//状态信息
		data:{
      	role:{
          id: int,					//角色id
          is_disabled: int,	//默认0，1禁用
          is_del: int,				//默认0，1删除
          created_at: Date,	//创建时间
          updated_at: Date, //更新时间
          name:String				//角色名字
          info:String				//角色介绍
          parent_id:int			//角色父id
    		}
    }
}
```

#### 添加角色

接口：POST  `/api/v1/addRole`

条件：拥有相应的权限

请求：

```js
{
  		name:String		//角色名字
      info:String		//角色介绍
      parent_id:int	//角色父id
}
```

响应：

```js
{
  	errcode:int 					//状态码
  	errmsg:String					//状态信息
		data:{
      	 role:{
          id: int,					//角色id
          is_disabled: int,	//默认0，1禁用
          is_del: int,				//默认0，1删除
          created_at: Date,	//创建时间
          updated_at: Date, //更新时间
          name:String				//角色名字
          info:String				//角色介绍
          parent_id:int			//角色父id
    		}
    }
}
```

#### 删除角色

接口：DELETE ` /api/v1/role/:roleId`

条件：拥有相应的权限

请求：

响应：

```js
{
  		errcode:int 				//状态码
      errmsg:String				//状态信息
}
```

#### 更新角色

接口：PUT  `/api/v1/role/:roleId`

条件：拥有相应的权限

请求：

```js
{
  		name:String		//角色名字
      info:String		//角色介绍
      parent_id:int	//角色父id
}
```

响应：

```js
{
  	errcode:int 					//状态码
  	errmsg:String					//状态信息
		data:{
      role:{
          id: int,					//角色id
          is_disabled: int,	//默认0，1禁用
          is_del: int,				//默认0，1删除
          created_at: Date,	//创建时间
          updated_at: Date, //更新时间
          name:String				//角色名字
          info:String				//角色介绍
          parent_id:int			//角色父id
    	}
    }
}
```

#### 为某个角色添加权限

接口：POST  `/api/v1/addPermission`

条件：拥有相应的权限

请求：

```js
{
  		role_id:int		//角色id
      router_id:int	//路由id
}
```

响应：

```js
{
		errcode:int 					//状态码
  	errmsg:String					//状态信息
		data:{
      	permission:{
          id: int,					//权限id
          is_disabled: int,	//默认0，1禁用
          is_del: int,				//默认0，1删除
          created_at: Date,	//创建时间
          updated_at: Date, //更新时间
          user_id:int				//userId
          router_id:int			//routerId
   			}
    }
}
```

#### 删除某个角色的权限

接口：DELETE  `/api/v1/permission/:permissionId`

条件：拥有相应的权限

请求：无

响应：

```js
{
  		errcode:int 				//状态码
      errmsg:String				//状态信息
}
```

#### 获取某个角色的权限

接口：GET ` /api/v1/permission/:roleId`

条件：拥有相应的权限

请求：无

响应：

```js
{
		errcode:int 					//状态码
  	errmsg:String					//状态信息
		data:{
      	permission:[{
          id: int,					//id
          is_disabled: int,	//默认0，1禁用
          is_del: int,				//默认0，1删除
          created_at: Date,	//创建时间
          updated_at: Date, //更新时间
          user_id:int				//userId
          router_id:int			//routerId
     		}] 
    }
}
```

#### 更新某个用户的角色

接口：PUT  `/api/v1/user/:userId/role`

条件：拥有相应的权限

请求：

```js
{
  		roleId:int 
}
```

响应：

```js
{
  	errcode:int 					//状态码
  	errmsg:String					//状态信息
  	data:{
  			user: {
            id: int,					//user_id
            is_disabled: int,	//默认0可以用，1禁用
            is_del: int,				//默认0可以用，1删除
            created_at: Date,	//创建时间
            updated_at: Date, //更新时间
            school_id: int,		//学校id
            role_id: int,				//角色id
            realname: String,	//姓名
            nick: String,			//昵称
            sex: char,				//性别
            email: String,		//电子邮箱	
            avatar: String,		//头像	
            note: String,			//备注，个性签名
            username: String,	//用户名
            ac_count: int,		//ac数
            submit_count: int,//提交数
            game_count: int,	//参加比赛的数
            reward: int,			//积分
            exp: int					//经验值
  					role:{
  							id:int						//roleId
  							is_disabled:int		//默认0可以用，1禁用
  							is_del: int,			//默认0可以用，1删除
            		created_at: Date,	//创建时间
            		updated_at: Date, //更新时间
  							name:String				//role名字
  							info:String				//role介绍
  							parent_id:int			//role的父id
						}
        }
		}
} 
```

#### 获取所有路由

接口：GET  `/api/v1/routers`

条件：拥有相应的权限

请求：

响应：

```js
{
  	errcode:int 					//状态码
  	errmsg:String					//状态信息
		data:{
	    routers:[{
          id: int,					//路由id
          is_disabled: int,	//默认0，1禁用
          is_del: int,				//默认0，1删除
          created_at: Date,	//创建时间
          updated_at: Date, //更新时间
          protocol:String		//协议
      		type:String				//请求类型
      		router:String			//路由
    	}]
    }
  ...,
}
```

#### 获取某个路由

接口：GET  `/api/v1/router/:routerId`

条件：拥有相应的权限

请求：

响应：

```js
{
  	errcode:int 					//状态码
  	errmsg:String					//状态信息
		data:{
       router:{
          id: int,					//路由id
          is_disabled: int,	//默认0，1禁用
          is_del: int,				//默认0，1删除
          created_at: Date,	//创建时间
          updated_at: Date, //更新时间
          protocol:String		//协议
      		type:String				//请求类型
      		router:String			//路由
    		}
    }
}
```

#### 添加路由

接口：POST  `/api/v1/addRouter`

条件：拥有相应的权限

请求：

```js
{
  				protocol:String		//协议
      		type:String				//请求类型
      		router:String			//路由
}
```

响应：

```js
{
  	errcode:int 					//状态码
  	errmsg:String					//状态信息
		data:{
        router:{
          id: int,					//路由id
          is_disabled: int,	//默认0，1禁用
          is_del: int,				//默认0，1删除
          created_at: Date,	//创建时间
          updated_at: Date, //更新时间
          protocol:String		//协议
      		type:String				//请求类型
      		router:String			//路由
     		}
    }
}
```

#### 删除路由

接口：POST  `/api/v1/router/:routerId`

条件：拥有相应的权限

请求：无

响应：

```js
{
  		errcode:int 				//状态码
      errmsg:String				//状态信息
}
```

#### 更新路由

接口：PUT  `/api/v1/router/:routerId`

条件：拥有相应的权限

请求：

```js
{
  	 			protocol:String		//协议
      		type:String				//请求类型
      		router:String			//路由
      	
}
```

响应：

```js
{
  	errcode:int 					//状态码
  	errmsg:String					//状态信息
		data:{
      	 router:{
          id: int,					//路由id
          is_disabled: int,	//默认0，1禁用
          is_del: int,				//默认0，1删除
          created_at: Date,	//创建时间
          updated_at: Date, //更新时间
          protocol:String		//协议
      		type:String				//请求类型
      		router:String			//路由
     		}
    }
}
```
