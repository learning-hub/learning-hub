## 日志服务（待定）

### 需求

- 日志记录
  - 系统日志
  - 网站日志
  - 用户日志
  - 比赛日志
- 日志查询
- 数据统计

### 功能

- 添加用户日志
- 添加系统日志
- 创建数据统计模型（标题，描述，唯一标识，周期）
- 以某一个数据为ID进行数据统计

### 数据库设计

> 日志服务使用mysql存储统计的结果，使用redis存储日志

#### 日志设计

> 使用json字符串存储，并且日志分为三类，info、warn、err

```
{
	created_at:Number,
	state_id:Number,
	message:String, // 消息
	service:String,
	type:String, // 类型 表示value要存储什么，如user_id 表示value要存储用户ID
	value:String,
}
```

#### 数据统计

> 数据统计使用mysql做统计结果，通过周期进行统计
>
> 统计是通过日志进行

