# Strapi用户与权限插件

在上一篇《Strapi快速使用指南》中，Restaurant 和 Category 被添加到了 Public 角色，这样的数据可以任意的被 API 获取，但在实际的应用中大多数的数据是需要用户授权后才能获取，这一篇内容就主要来搞定用户与授权这块内容。

## 前置内容

在管理员页面（`/admin`）设置菜单中的 **Users & Permissions plugin** 实际是由默认安装的 **Users & Permissions plugin** 插件提供支持，此插件提供了完整的基于 JWT 的身份验证流程，用来保护 API。还提供了访问控制列表（ACL）策略，使得可以管理用户组之间的权限。此插件默认提供了 Public 和 Authenticated 两大角色，通过编辑角色可以调整不同角色下具体的路由访问权限。

在设置角色与权限菜单中将 Restaurant 和 Category 从 Public 角色下移除，添加到 Authenticated 角色：

![image-20240823095320448](https://raw.githubusercontent.com/OSpoon/ImageStorage/2024/uPic/image-20240823095320448.png)

## 注册用户

POST /api/auth/local/register

在数据库中创建一个默认角色为“registered”的新用户。

> Body 请求参数

```json
{
  "username": "Strapi user",
  "email": "user@strapi.io",
  "password": "strapiPassword"
}
```

### 请求参数

| 名称       | 位置 | 类型   | 必选 | 说明   |
| ---------- | ---- | ------ | ---- | ------ |
| body       | body | object | 否   | none   |
| » username | body | string | 是   | 用户名 |
| » email    | body | string | 是   | 邮箱   |
| » password | body | string | 是   | 密码   |

> 返回示例

> 成功

```json
{
  "jwt": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6MiwiaWF0IjoxNzI0Mzc5OTE2LCJleHAiOjE3MjY5NzE5MTZ9._b8R-GeMyntAnUARH2iDkVGKpU0Y8HoGs9ZTWL1jDEI",
  "user": {
    "id": 2,
    "username": "Strapi user",
    "email": "user@strapi.io",
    "provider": "local",
    "confirmed": true,
    "blocked": false,
    "createdAt": "2024-08-23T02:25:16.190Z",
    "updatedAt": "2024-08-23T02:25:16.190Z"
  }
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | 成功 | Inline   |

### 返回数据结构

状态码 **200**

| 名称         | 类型    | 必选 | 约束 | 中文名 | 说明 |
| ------------ | ------- | ---- | ---- | ------ | ---- |
| » jwt        | string  | true | none |        | none |
| » user       | object  | true | none |        | none |
| »» id        | integer | true | none |        | none |
| »» username  | string  | true | none |        | none |
| »» email     | string  | true | none |        | none |
| »» provider  | string  | true | none |        | none |
| »» confirmed | boolean | true | none |        | none |
| »» blocked   | boolean | true | none |        | none |
| »» createdAt | string  | true | none |        | none |
| »» updatedAt | string  | true | none |        | none |

按上述接口文档操作后将在内容管理菜单查询到第一条注册用户的信息，虽然响应接口返回了 jwt 信息，但暂时忽略它，我们用刚注册的用户信息通过登录接口来获取：

![image-20240823103242020](https://raw.githubusercontent.com/OSpoon/ImageStorage/2024/uPic/image-20240823103242020.png)

## 登录认证

POST /api/auth/local

提交用户标识符和密码凭据进行身份验证。在成功验证后，响应数据将包含用户信息以及一个身份验证令牌。

> Body 请求参数

```json
{
  "identifier": "user@strapi.io",
  "password": "strapiPassword"
}
```

### 请求参数

| 名称         | 位置 | 类型   | 必选 | 说明                   |
| ------------ | ---- | ------ | ---- | ---------------------- |
| body         | body | object | 否   | none                   |
| » identifier | body | string | 是   | 可以是电子邮件或用户名 |
| » password   | body | string | 是   | none                   |

> 返回示例

> 成功

```json
{
  "jwt": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6MiwiaWF0IjoxNzI0MzgwNTMwLCJleHAiOjE3MjY5NzI1MzB9.6YeNqqklmBWNO_1kSG0IgbOAXd25vHwY1AgwnzrXXIY",
  "user": {
    "id": 2,
    "username": "Strapi user",
    "email": "user@strapi.io",
    "provider": "local",
    "confirmed": true,
    "blocked": false,
    "createdAt": "2024-08-23T02:25:16.190Z",
    "updatedAt": "2024-08-23T02:25:16.190Z"
  }
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | 成功 | Inline   |

### 返回数据结构

状态码 **200**

| 名称         | 类型    | 必选 | 约束 | 中文名 | 说明 |
| ------------ | ------- | ---- | ---- | ------ | ---- |
| » jwt        | string  | true | none |        | none |
| » user       | object  | true | none |        | none |
| »» id        | integer | true | none |        | none |
| »» username  | string  | true | none |        | none |
| »» email     | string  | true | none |        | none |
| »» provider  | string  | true | none |        | none |
| »» confirmed | boolean | true | none |        | none |
| »» blocked   | boolean | true | none |        | none |
| »» createdAt | string  | true | none |        | none |
| »» updatedAt | string  | true | none |        | none |

## 获取 Restaurant 数据

在上一篇 《Strapi快速使用指南》获取 Restaurant 数据 API 接口的基础上通过请求头携带 Token 信息，重新获取被权限保护的数据信息：

```typescript
const token = resp.jwt; // 登录成功返回的 jwt 信息
const headers =  {
      Authorization: `Bearer ${token}`,
  }
}
```

## 更改密码

POST /api/auth/change-password

更新已认证用户的密码

> Body 请求参数

```json
{
  "currentPassword": "string",
  "password": "string",
  "passwordConfirmation": "string"
}
```

### 请求参数

| 名称                   | 位置 | 类型   | 必选 | 说明 |
| ---------------------- | ---- | ------ | ---- | ---- |
| body                   | body | object | 否   | none |
| » currentPassword      | body | string | 是   | none |
| » password             | body | string | 是   | none |
| » passwordConfirmation | body | string | 是   | none |

> 返回示例

> 成功

```json
{
  "jwt": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6MiwiaWF0IjoxNzI0Mjk0NDk3LCJleHAiOjE3MjY4ODY0OTd9.Y3gLzjVlBU19TJvkGmF1JatImYi4PsiMl8AAgZRejGs",
  "user": {
    "id": 2,
    "username": "strapi.io",
    "email": "user@strapi.io",
    "provider": "local",
    "confirmed": true,
    "blocked": false,
    "createdAt": "2024-08-22T01:33:59.892Z",
    "updatedAt": "2024-08-22T01:50:15.353Z"
  }
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | 成功 | Inline   |

### 返回数据结构

状态码 **200**

| 名称         | 类型    | 必选 | 约束 | 中文名 | 说明 |
| ------------ | ------- | ---- | ---- | ------ | ---- |
| » jwt        | string  | true | none |        | none |
| » user       | object  | true | none |        | none |
| »» id        | integer | true | none |        | none |
| »» username  | string  | true | none |        | none |
| »» email     | string  | true | none |        | none |
| »» provider  | string  | true | none |        | none |
| »» confirmed | boolean | true | none |        | none |
| »» blocked   | boolean | true | none |        | none |
| »» createdAt | string  | true | none |        | none |
| »» updatedAt | string  | true | none |        | none |

## 忘记密码

1. 在设置菜单中 **Users & Permissions plugin** 插件的**高级设置**，在其中的**重置密码**里面设置重置密码页面的 URL 地址；
2. 在设置菜单中 **Users & Permissions plugin** 插件的**电子邮件模板**，在其中填写**发件人电子邮件**；

POST /api/auth/forgot-password

忘记密码将向用户发送一封电子邮件，其中包含指向您的重置密码页面的链接。

> Body 请求参数

```json
{
  "email": "string"
}
```

### 请求参数

| 名称    | 位置 | 类型   | 必选 | 说明 |
| ------- | ---- | ------ | ---- | ---- |
| body    | body | object | 否   | none |
| » email | body | string | 是   | none |

> 返回示例

> 成功

```json
{
  "ok": true
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | 成功 | Inline   |

### 返回数据结构

状态码 **200**

| 名称 | 类型    | 必选 | 约束 | 中文名 | 说明 |
| ---- | ------- | ---- | ---- | ------ | ---- |
| » ok | boolean | true | none |        | none |

## 重置密码

POST /api/auth/reset-password

更新用户密码

> Body 请求参数

```json
{
  "code": "string",
  "password": "string",
  "passwordConfirmation": "string"
}
```

### 请求参数

| 名称                   | 位置 | 类型   | 必选 | 说明 |
| ---------------------- | ---- | ------ | ---- | ---- |
| body                   | body | object | 否   | none |
| » code                 | body | string | 是   | none |
| » password             | body | string | 是   | none |
| » passwordConfirmation | body | string | 是   | none |

> 返回示例

> 成功

```json
{
  "jwt": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6NSwiaWF0IjoxNzI0Mjk2ODUzLCJleHAiOjE3MjY4ODg4NTN9.6O55RjddMJQz5rIuhl7lt4n8OjCNZ1rdwsj2QC4632I",
  "user": {
    "id": 5,
    "username": "1825203636",
    "email": "1825203636@qq.com",
    "provider": "local",
    "confirmed": false,
    "blocked": false,
    "createdAt": "2024-08-22T03:08:02.411Z",
    "updatedAt": "2024-08-22T03:16:51.577Z"
  }
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | 成功 | Inline   |

### 返回数据结构

状态码 **200**

| 名称         | 类型    | 必选 | 约束 | 中文名 | 说明 |
| ------------ | ------- | ---- | ---- | ------ | ---- |
| » jwt        | string  | true | none |        | none |
| » user       | object  | true | none |        | none |
| »» id        | integer | true | none |        | none |
| »» username  | string  | true | none |        | none |
| »» email     | string  | true | none |        | none |
| »» provider  | string  | true | none |        | none |
| »» confirmed | boolean | true | none |        | none |
| »» blocked   | boolean | true | none |        | none |
| »» createdAt | string  | true | none |        | none |
| »» updatedAt | string  | true | none |        | none |

## 电子邮件验证

POST /api/auth/send-email-confirmation

重新发送确认邮件

> Body 请求参数

```json
{
  "email": "string"
}
```

### 请求参数

| 名称    | 位置 | 类型   | 必选 | 说明 |
| ------- | ---- | ------ | ---- | ---- |
| body    | body | object | 否   | none |
| » email | body | string | 是   | none |

> 返回示例

> 成功

```json
{
  "email": "1825203636@qq.com",
  "sent": true
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | 成功 | Inline   |

### 返回数据结构

状态码 **200**

| 名称    | 类型    | 必选 | 约束 | 中文名 | 说明 |
| ------- | ------- | ---- | ---- | ------ | ---- |
| » email | string  | true | none |        | none |
| » sent  | boolean | true | none |        | none |
