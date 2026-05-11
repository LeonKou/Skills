---
name: backend-dev
description: 后端开发规范，提供微服务接口设计、请求响应结构、数据库字段、项目工程结构的统一标准。适用于后端项目开发参考与约束。
---

# 通用后端开发规范

本规范提供后端项目开发的标准约定，适用于微服务架构下的RESTful API设计。

## 何时使用

当用户请求以下操作时调用此skill：
- 创建或审查新的后端API接口
- 设计数据库表结构
- 规划项目目录结构
- 定义请求响应格式
- 代码审查时检查是否符合规范

---

## 一、接口路径设计规范

### 1.1 URL模板

```
api/{服务名}/{module}/v1/{controller}/{action}
```

#### 各部分说明

| 部分 | 说明 | 示例 |
|------|------|------|
| 服务名 | 微服务注册名 | `user`, `order`, `account` |
| module | 业务模块划分，与未来拆分的微服务对应 | `iam`, `profile`, `sync` |
| 版本 | 固定v1，重构时新增v2，保留旧版本 | `v1`, `v2` |
| controller | 一组Action的类定义 | `manage`, `sync`, `user` |
| action | 处理HTTP请求的方法 | `add`, `delete`, `update`, `query` |

#### 完整示例

```
# 用户服务 -> IAM模块 -> 企业用户管理 -> 添加
api/user/iam/v1/manage/add

# 用户服务 -> IAM模块 -> IAM数据同步 -> 用户同步
api/user/iam/v1/sync/user

# 用户服务 -> IAM模块 -> 企业用户管理 -> 批量添加
api/user/iam/v1/manage/batch-add
```

### 1.2 路径命名规则

- **全部小写**：路径使用小写字母
- **单词分隔**：多个单词使用 `-` 连接
- **无尾部斜杠**：URL不以 `/` 结尾
- **无动词**：路径中不包含动词，动词在action中体现

### 1.3 HTTP方法规范

| 场景 | 方法 | 示例 |
|------|------|------|
| 查询/获取 | GET | `GET api/user/iam/v1/manage/query` |
| 创建 | POST | `POST api/user/iam/v1/manage/add` |
| 编辑/更新 | POST | `POST api/user/iam/v1/manage/update` |
| 删除 | DELETE | `DELETE api/user/iam/v1/manage/delete` |
| 批量操作 | POST | `POST api/user/iam/v1/manage/batch-delete` |
| 文件上传 | POST | `POST api/user/iam/v1/manage/upload-avatar` |

### 1.4 参数位置规则

- **公共参数**：放在query中（便于问题排查和测试）
  - 例如：token, tenantId, language
- **业务参数**：
  - GET请求：query参数
  - POST请求：body参数
- **path参数**：仅在网关统一处理不了的情况下使用

### 1.5 内部接口标识

仅限后端服务之间访问的接口，在 `api` 后追加 `inner`：

```
api/inner/{服务名}/{module}/v1/{controller}/{action}
```

示例：
```
api/inner/user/iam/v1/manage/add
```

### 1.6 版本控制策略

- **默认版本**：v1
- **重大变更**：新增v2路径，保留v1供旧客户端使用
- **版本号**：使用正整数，不使用Beta等标识

---

## 二、请求参数设计规范

### 2.1 命名规范

- **参数命名**：小驼峰（camelCase）
- 示例：`userName`, `orderId`, `pageIndex`

### 2.2 参数位置

```http
# GET查询 - 参数在query
GET api/user/iam/v1/manage/query?userName=张三&tenantId=xxx&token=xxx

# POST创建 - 参数在body
POST api/user/iam/v1/manage/add
Content-Type: application/json

{
  "userName": "张三",
  "email": "zhangsan@example.com",
  "tenantId": "xxx"
}
```

### 2.3 公共参数

| 参数 | 说明 | 位置 |
|------|------|------|
| token | 认证令牌 | query |
| tenantId | 租户标识（多租户场景） | query |
| lang / locale | 多语言标识 | query/cookie |

---

## 三、响应设计规范

### 3.1 命名规范

- **字段命名**：小驼峰（camelCase）

### 3.2 成功响应 - 分页

```json
{
  "pageIndex": 1,
  "pageSize": 10,
  "totalCount": 5690,
  "totalPage": 569,
  "data": [
    { "id": "xxx", "userName": "张三" }
  ]
}
```

**类型要求**：
- `pageIndex`: int32 或 int64
- `pageSize`: int32 或 int64
- `totalCount`: int32 或 int64
- `totalPage`: int32 或 int64
- 必须是数值类型，**禁止字符串**

### 3.3 成功响应 - 游标分页

适用于无限滚动场景：

```json
{
  "data": [
    { "id": "xxx", "userName": "张三" }
  ],
  "nextCursor": "eyJpZCI6MTAwfQ==",
  "prevCursor": "eyJpZDowOX0="
}
```

**说明**：
- 首次请求cursor可为空
- `pageSize`通过query传递
- 方向默认next，可通过query指定prev

### 3.4 成功响应 - 非分页

```json
{
  "data": {
    "id": "xxx",
    "userName": "张三",
    "email": "zhangsan@example.com"
  }
}
```

**说明**：
- 业务数据通过 `data` 字段承载
- 增加全局字段只需在data同级添加

### 3.5 成功响应 - 包含Request_ID

```json
{
  "data": {...},
  "requestId": "req_abc123"
}
```

用于问题排查和日志追踪。

### 3.6 错误响应

**所有HTTP错误统一格式**：

```json
{
  "error": {
    "code": 400,
    "message": "Permission denied on resource \"organizations/dd\" (or it may not exist)",
    "status": "PERMISSION_DENIED"
  },
  "requestId": "req_abc123"
}
```

**字段说明**：

| 字段 | 类型 | 说明 |
|------|------|------|
| code | int | 与HTTP状态码一致 |
| message | string | 简单描述，用于开发排查 |
| status | string | 业务错误码（字符串），唯一性标识，用于多语言转换 |
| requestId | string | 请求追踪ID |

**HTTP状态码使用**：

| 状态码 | 场景 |
|--------|------|
| 200 | 成功 |
| 400 | 请求参数不符合规范（如密码缺失、账号格式错误） |
| 401 | 未认证（网关处理） |
| 403 | 无权限（网关处理） |
| 404 | 资源不存在 |
| 500 | 未catch的异常 |

**说明**：401、403等认证授权类错误通常由网关统一处理。

---

## 四、数据库字段规范

### 4.1 通用字段（每个表必须包含）

| 字段 | 类型 | 说明 |
|------|------|------|
| id | varchar(32) | 唯一主键，字符串类型 |
| created_at | bigint | 创建时间，时间戳（毫秒） |
| updated_at | bigint | 更新时间，时间戳（毫秒） |

### 4.2 软删除
不支持软删除

### 4.3 多租户字段（有多租户业务才执行本条规范）

```sql
ALTER TABLE {table_name} ADD COLUMN tenant_id varchar(32) NOT NULL DEFAULT '';

-- 查询时必须带tenant_id条件
SELECT * FROM {table_name} WHERE tenant_id = 'xxx';
```

### 4.4 布尔字段命名

| 字段 | 说明 |
|------|------|
| is_active | 是否激活 |
| is_deleted | 是否删除（软删除用） |
| is_enabled | 是否启用 |
| is_verified | 是否验证 |

### 4.5 字段命名规范

- **小写下划线**：user_name, created_at
- **禁止**：userName, createTime, isEnable

### 4.6 数据类型规范

| 场景 | 类型 | 说明 |
|------|------|------|
| 主键ID | varchar(32) | 字符串类型 |
| 时间 | bigint | 时间戳（毫秒），接口返回时转为字符串 |
| 金额 | int | 最小单位（人民币为分，美元为美分） |
| 百分比 | decimal(5,2) | 0-100范围 |
| 经纬度 | decimal(10,8) | 坐标 |
| 短文本 | varchar(255) | 姓名、标题等 |
| 长文本 | text | 描述、备注等 |
| JSON | json | 结构化数据 |

**大字段单独成表**：为提高数据库效率，大字段（如长文本、备注、JSON）单独成表，通过id与业务表关联。

### 4.7 索引规范

| 类型 | 命名规范 | 示例 |
|------|----------|------|
| 主键 | pk_{table} | pk_users |
| 唯一 | uk_{table}_{field} | uk_users_email |
| 普通 | idx_{table}_{field} | idx_users_tenant_id |
| 复合 | idx_{table}_{field1_field2} | idx_users_tenant_deleted |

---

## 五、项目工程结构规范

### 5.1 根目录结构

```
项目根目录/
├── README.md              # 项目说明
├── docs/                  # 各类说明文档
│   ├── api-docs/         # API文档
│   ├── database/         # 数据库设计文档
│   └── deployment/       # 部署文档
└── src/                  # 源码
    ├── backend/          # 后端源码
    │   └── src/          # 后端源码目录
    └── frontend/         # 前端源码
        └── src/          # 前端源码目录
```

### 5.2 后端源码结构（按模块划分）

```
backend/src/
├── main                  # 入口包
│   └── main.go / Main.java / main.ts
├── config/               # 配置
├── modules/              # 按业务模块划分（核心目录）
│   ├── user/             # 用户模块
│   │   ├── controller/  # 控制器层
│   │   ├── service/     # 业务逻辑层
│   │   ├── repository/  # 数据访问层
│   │   ├── model/       # 数据模型
│   │   └── dto/         # 数据传输对象
│   ├── order/           # 订单模块
│   │   ├── controller/
│   │   ├── service/
│   │   ├── repository/
│   │   ├── model/
│   │   └── dto/
│   └── product/         # 商品模块
│       ├── controller/
│       ├── service/
│       ├── repository/
│       ├── model/
│       └── dto/
├── common/               # 公共模块（各模块共用）
│   ├── middleware/      # 中间件
│   ├── utils/           # 工具函数
│   ├── constant/        # 常量定义
│   └── error/           # 错误码定义
└── router/               # 路由注册（若框架需要）
```

**说明**：每个业务模块（如 user, order）下独立包含 controller、service、repository、model、dto，便于模块独立开发和未来拆分。

### 5.3 配置文件结构

```
configs/
├── dev.yaml             # 开发环境
├── test.yaml            # 测试环境
├── staging.yaml         # 预发布环境
└── prod.yaml            # 生产环境
```

**敏感信息管理**：禁止硬编码，必须通过环境变量读取。

---

## 六、错误码定义规范

### 6.1 错误码文件位置

错误码定义在后端工程中，代码响应时引用对应错误码。

### 6.2 错误码分类规则

status 错误码的分类通过下划线分割，如 `SYSTEM_ERROR` 中的 `system` 代表系统类，`USER_USER_NOT_FOUND` 表示用户业务下报的错误是用户未找到。

```python
# error_codes.py
SYSTEM_ERROR = {"code": 500, "status": "SYSTEM_ERROR", "message": "System error"}
PERMISSION_DENIED = {"code": 400, "status": "PERMISSION_DENIED", "message": "Permission denied"}
USER_USER_NOT_FOUND = {"code": 404, "status": "USER_USER_NOT_FOUND", "message": "User not found"}
USER_INVALID_PASSWORD = {"code": 400, "status": "USER_INVALID_PASSWORD", "message": "Invalid password"}
```

---

## 七、安全与认证

- **认证授权**：由网关统一处理，不在业务代码中实现
- **参数校验**：所有用户输入必须校验
- **SQL注入**：使用参数化查询
- **敏感数据**：加密存储
- **限流保护**：网关统一处理

---

## 八、最佳实践

1. **保持一致**：整个项目保持统一的命名和结构
2. **文档先行**：接口开发前先编写API文档
3. **版本控制**：API变更必须版本化，保留旧版本
4. **渐进增强**：新功能保持向后兼容
5. **问题追踪**：每个响应带requestId便于排查
6. **国际规范**：时间、金额、时区等使用国际标准