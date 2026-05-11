# 后端服务开发术语表

本文档定义项目中使用的统一术语，消除 AI 与开发者之间的语言障碍。

## 核心概念

| 术语 | 定义 | 示例 |
|------|------|------|
| module | 业务模块划分 | `iam`, `profile`, `sync` |
| controller | 一组 Action 的类定义 | `manage`, `sync`, `user` |
| action | 处理 HTTP 请求的方法 | `add`, `delete`, `update`, `query` |
| DTO | 数据传输对象 | `CreateUserDTO` |
| VO | 值对象 | `UserVO` |
| Entity | 实体，对应数据库表 | `User`, `Order` |

## API 路径约定

格式：`api/{服务名}/{module}/v1/{controller}/{action}`

| 部分 | 说明 | 示例 |
|------|------|------|
| 服务名 | 微服务注册名 | `user`, `order` |
| module | 业务模块划分 | `iam`, `profile` |
| controller | Action 类定义 | `manage`, `sync` |
| action | HTTP 方法名 | `add`, `query`, `update`, `delete` |

示例：
```
api/user/iam/v1/manage/add
api/user/iam/v1/sync/user
```

## 数据库约定

| 术语 | 类型 | 说明 |
|------|------|------|
| id | varchar(32) | 唯一主键 |
| created_at | bigint | 创建时间（毫秒时间戳） |
| updated_at | bigint | 更新时间（毫秒时间戳） |
| tenant_id | varchar(32) | 租户标识（多租户场景） |

## 字段命名规范

- 数据库：蛇形命名 `user_name`, `created_at`
- API 参数/响应：小驼峰 `userName`, `orderId`

## 错误码格式

```python
SYSTEM_ERROR = {"code": 500, "status": "SYSTEM_ERROR", "message": "System error"}
USER_NOT_FOUND = {"code": 404, "status": "USER_USER_NOT_FOUND", "message": "User not found"}
```

格式：`{业务}_{具体错误}`，用下划线分割。

## 响应结构

| 类型 | data 字段 |
|------|----------|
| 分页 | 数组 + 分页信息 |
| 非分页 | 单个对象 |
| 错误 | `error` 对象含 code/message/status |