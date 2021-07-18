# learning-hub 第三版设计

## 系统设计

![系统结构](./draw/sys.png)

> 服务内部之间使用消息队列通信。外部使用 HTTP 通信。

### Nginx

负责静态网站的部署和路由分发。

### 权限网关

负责对用户的管理、用户角色的管理、角色权限的管理、角色权限的判断等，并且记录日志统计数据。

### 定时任务服务

处理一些定时的任务。

### 业务功能

#### 题库模块

负责题库的管理，并且与远程 Git 仓库实时同步。

负责调用判题机判题。

题库模型

- problems/ # 题库
  - 1000/ # 题号
    - desc.md # 描述文件
    - source.c # 源码
    - info.json # 其他信息 { tags:[], source: '来源', hand: 难度, timeLimit: 100, memoryLimit: 100, type:0 }
    - template # 输入用例模板
    - testcase/ # 测试用例
      - 1.in
      - 1.out
      - 2.in
      - 2.out

#### 比赛模块

负责个人赛和团队赛提供服务。

系统默认会创建一个永久的默认个人赛，相当于是排名功能。

#### 消息模块

可以使用邮箱和站内信，向用户发送消息。

#### 判题机集群

这里会做一次负载均衡，其他服务只需要调用一个消息队列就可以实现判题了。

## 数据模型

### 公共模型

```ts
class SchemaBase {
  id: number; // ID
  isDisable: boolean; // 是否禁用 默认:false
  deletedAt: Date | null; // 删除时间
  createdAt: Date; // 创建时间
  updatedAt: Date; // 更新时间
  order: number; //排序段
}
```

### 主服务

#### 用户模型

##### User

```ts
enum UserSex {
  UNKONWN,
  MAN,
  WOMAN,
}

class UserPrivate {
  realname: string; //姓名
  email: string; //电子邮箱
}

class User extends SchemaBase {
  extendAuth: Auth[];
  avatar: string; //头像
  nick: string; //昵称
  sex: UserSex; //性别
  private: UserPrivate; // 用户私密信息
  isPublicPrivate: boolean; // 是否公开私密消息
  note: string; //备注，个性签名
  username: string; //用户名
  ac_count: number; //ac数
  submit_count: number; //提交数
  game_count: number; //参加比赛的数
  exp: number; //经验值
  organs: Organ[]; // 属于组织
  roles: Role[]; // 角色
  isDisableGame: boolean; //是否禁赛
}
```

##### Role

```ts
class Role extends SchemaBase {
  name: string; //role名字
  description: string; //role介绍
  parent: Role | null; //role的父id
  rejects: Role[]; // 互斥角色
}
```

##### Organ

```ts
class Organ extends SchemaBase {
  name: string; // 组织名称
  description: string; // 描述
  maxUser: number; // 最多人
  expired: Date; // 关闭时间
  note: Date; // 公告
  parent: Organization | null; // 父组织
  createUser: User; // 创建人
  code: number; // 邀请码
}
```

##### Auth

```ts
enum AuthType {
  LOCAL, // 账号密码登录
  WX,
}

class Auth extends SchemaBase {
  type: AuthType;
  createdAt: number;
  secret: string; // 登录秘钥，算法 has128(rawJSON)
  rawJSON: string; // 组成：{ type, createdAt, ...其他信息 },
  /**
   * 如：
   * LOCAL登录
   * '{"password": "sdfeuhkakgbludo"}'
   *
   * 微信登录
   * '{"uniqueId": "adfasfekhkcbvuadhf"}'
   **/
}
```

#### 题库模型

##### Problem

```ts
enum ProblemType{
  CODE,
  SINGLE,
  MULTI,
  FILL
}
class Problem extends SchemaBase {
  title: string;   // 标题
  tags: string[];  // 标签
  source: string; // 来源
  acCount: number; // AC数
  submitCount: number; // 提交数
  type: ProblemType; // 类型
  isCache: boolean; // 是否缓存到前端
  isHidden: boolean; // 是否隐藏
}
```

**题的文件夹结构**

- 1000/ # 题号
  - desc.md # 描述文件
  - source.c # 源码 **程序题专属**
  - info.json # 其他信息 { tags:[], source: '来源', hand: 难度, timeLimit: 100, memoryLimit: 100, type:0 }
  - template # 输入用例模板 **程序题专属**
  - testcase/ # 测试用例 **程序题专属**
    - 1.in
    - 1.out
    - 2.in
    - 2.out

**这里最重要的`info.json`说明一下**

```json
// info.json
{
  "type": number, // 题的类型
  "tags": string[],	// 标签
  "source": string,	// 来源
  
  "hand": number,		// 难度
  "timeLimit": number, // 时间限制 ms
  "memoryLimit": number, // 空间限制 B
  
  "options":string[], // 单选多选的选择集
  "answer":string, // 单选的答案
  "answers":string[], // 多选的答案，填空的答案，这里可以是正则
}
```

#### 比赛模型

> 比赛分为团队赛和个人赛

##### Game

```ts
enum GameType{
  USER, // 个人
  ORGAN // 团队
}

class Game extends SchemaBase{
  title: string; // 比赛标题
  description: string; // 比赛描述
  type: GameType; // 类型
  createUser: User;  // 创建的人
  holdOrgan: Organ; // 举办的组织
  startTime: Date; // 开始时间
  duration: number; // 时间长度 s
  userLimit:number;  // 限制人数
  gameUsers: GameUser[] | null; // 比赛人员
  gameOrgans: GameOrgan[] | null; // 比赛组织
  isPrivate: boolean; // 是否私有
  code: number; // 参赛密码
  isHidden: boolean; // 是否隐藏
  gameProblem: GameProblem[]; //比赛题
}
```

##### GameUser

> 比赛人员名单

```ts
class GameUser extends SchemaBase{
  game: Game;
	user: User;
  acCount: number;
  acRate: number; // AC 正确率
  submitCount: number;
  lastAcTime: Date; // 最后一次AC时间
}
```

##### GameOrgan

> 比赛组织名单

```ts
class GameOrgan extends SchemaBase{
	game: Game;
	organ: Organ;
  acCount: number;
  acRate: number; // AC 正确率
  submitCount: number;
  lastAcTime: Date; // 最后一次AC时间
}
```

##### GameProblem

```ts
class GameProblem extends SchemaBase{
	game: Game;
  problem: Problem;
  acCount: number;
  submitCount: number;
}
```

##### Judger

> 判题结果表

```ts
enum ResultType{
/* 0 正确
  1 判题中
  2 格式错误
  3 答案错误
  4 时间超限
  5 内存超限
  6 运行错误
  7 编译错误
  8 系统错误
  9 队列中
*/
  AC, JUD_ING, OUT_ERR, ANS_ERR, TIME_OVER, MEM_OVER, RUN_ERR, COMP_ERR, SYS_ERR, QUE_ING
}

class Judger extends SchemaBase{
  user: User;  // 提交用户
  organ: Organ|null; // 团队ID，null表示个人赛
  ip: string; // 提交IP
  game: Game;		// 比赛
  gameProblem: GameProblem; // 比赛中的题
  result: ResultType; // 结果
  message: string; // 错误消息
  jobId: string; // 消息队列ID
}
```



#### 消息模型

##### Message

```ts
enum MessageType{
  SYS, // 系统消息
  USER // 用户消息
}

class Message extends SchemaBase{
  title: string;
  body: string;
  type: MessageType;
  createUser: User | null;
  acceptUser: User | null;
}
```



## APIv1 接口

## 核心功能设计

## 系统部署
