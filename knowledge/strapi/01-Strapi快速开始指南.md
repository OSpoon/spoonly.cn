# Strapi快速使用指南

## 摘要

本文概述了使用Strapi快速启动内容管理系统的过程，包括环境配置、项目创建、内容类型定义、数据添加及权限设置等关键步骤，展示了Strapi作为无头CMS的灵活性和易用性。

[strapi](https://strapi.io/) 是基于 JavaScript 和 Typescript 且完全可定制的无前端界面的内容管理开源框架。

开始之前请确认开发环境是否符合一下要求：

* Node.js：应选择当前的长期支持（LTS）版本或维护长期支持（LTS）版本（ `v18` 和 `v20` ）；
* Package manager：[npm](https://docs.npmjs.com/cli/v6/commands/npm-install) v6+、[yarn](https://yarnpkg.com/getting-started/install)；
* Python：默认数据库选用 SQLite，需要 [Python](https://www.python.org/downloads/) 环境；

## 使用 Strapi 创建新项目

1. 终端中运行以下命令开始创建 Strapi 项目，期间会提示是否创建或登录 Strapi Cloud 账户，这里可以选择跳过，继续等待依赖安装：

```shell
npx create-strapi-app@latest my-strapi-project --quickstart
```

2. 安装完成将自动跳转到注册管理员(`/admin/auth/register-admin`) 页面，根据页面提示注册第一个管理原账户，你可以按照下面的信息填写：

![image-20240822141438294](https://raw.githubusercontent.com/OSpoon/ImageStorage/2024/uPic/image-20240822141438294.png)

## 搭建内容数据结构

管理员注册成功就自动跳转到管理后台(`/admin`) 首页，搭建内容数据主要用到两个菜单：

* 创建内容数据结构需要用到 Content-Type Builder(`/admin/plugins/content-type-builder/`) 菜单；
* 具体的内容管理需要用到 Content Mamager(`/admin/content-manager/`) 菜单。



1. 管理员注册成功就自动跳转到管理后台(`/admin`) 首页，点击 Content-Type Builder(`/admin/plugins/content-type-builder/`) 菜单开始搭建 Restaurant 的内容类型结构：

| 类型 | 字段（Basic settings） | 要求（Advanced settings） |
| ---- | ---------------------- | ------------------------- |
| Text | Name                   | 必填、唯一                |
| Rich | Description            |                           |

创建名称为 `Restaurant` 的内容类型结构，点击 **Continue** 添加 `Name` 和 `Description` 两个字段，最后点击 **Save** 菜单保存（自动重启项目）：

![image-20240822142951124](https://raw.githubusercontent.com/OSpoon/ImageStorage/2024/uPic/image-20240822142951124.png)

![image-20240822143905006](https://raw.githubusercontent.com/OSpoon/ImageStorage/2024/uPic/image-20240822143905006.png)

![image-20240822143918172](https://raw.githubusercontent.com/OSpoon/ImageStorage/2024/uPic/image-20240822143918172.png)

![image-20240822143930482](https://raw.githubusercontent.com/OSpoon/ImageStorage/2024/uPic/image-20240822143930482.png)

2. 重复前面的步骤创建 `Category` 的内容类型结构，再次点击 **Save** 菜单保存（自动重启项目）：

| 类型     | 字段                                                         | 要求                 |
| -------- | ------------------------------------------------------------ | -------------------- |
| Text     | Name                                                         | 必填、唯一           |
| Relation | ![image-20240822144615416](https://raw.githubusercontent.com/OSpoon/ImageStorage/2024/uPic/image-20240822144615416.png) | 与 Restaurant 多对多 |

![image-20240822144729799](https://raw.githubusercontent.com/OSpoon/ImageStorage/2024/uPic/image-20240822144729799.png)

## 添加具体内容数据

打开内容管理(`/admin/content-manager/collection-types/`) 菜单按下表为 `Restaurant` 和 `Category` 添加数据：

* Restaurant

| Name                | Description                                                  |
| ------------------- | ------------------------------------------------------------ |
| Biscotte Restaurant | Welcome to Biscotte restaurant! Restaurant Biscotte offers a cuisine based on fresh, quality products, often local, organic when possible, and always produced by passionate producers. |

* Category

| Name        |
| ----------- |
| French Food |
| Brunch      |

![image-20240822145613423](https://raw.githubusercontent.com/OSpoon/ImageStorage/2024/uPic/image-20240822145613423.png)

内容数据添加完成后，接着为 `Restaurant` 和 `Category` 建立关系，也就是为 Biscotte Restaurant 添加类别为 `French Food`  和  `Brunch` ：

![image-20240822150013916](https://raw.githubusercontent.com/OSpoon/ImageStorage/2024/uPic/image-20240822150013916.png)

## 设置角色与权限范围

打开角色(`/admin/settings/users-permissions/roles`) 菜单，在 Public 角色下的权限列表分别找到 Restaurant 和 Category，并分别为其勾选 `find` 和 `findOne` 权限：

![image-20240822150614727](https://raw.githubusercontent.com/OSpoon/ImageStorage/2024/uPic/image-20240822150614727.png)

## 发布内容数据

返回内容管理(`/admin/content-manager/`) 菜单，分别打开 Restaurant 和 Category 添加的数据，点击右上角的发布按钮，数据状态将由 `Draft` 切换到 `Published` ：

![image-20240822151338248](https://raw.githubusercontent.com/OSpoon/ImageStorage/2024/uPic/image-20240822151338248.png)

## 使用API获取内容数据

### Get All Restaurants：

GET /api/restaurants

> 成功

```json
{
  "data": [
    {
      "id": 1,
      "attributes": {
        "Name": "Biscotte Restaurant",
        "Description": [
          {
            "type": "paragraph",
            "children": [
              {
                "type": "text",
                "text": "Welcome to Biscotte restaurant! Restaurant Biscotte offers a cuisine based on fresh, quality products, often local, organic when possible, and always produced by passionate producers."
              }
            ]
          }
        ],
        "createdAt": "2024-08-22T06:53:04.569Z",
        "updatedAt": "2024-08-22T07:14:35.267Z",
        "publishedAt": "2024-08-22T07:14:35.263Z"
      }
    }
  ],
  "meta": {
    "pagination": {
      "page": 1,
      "pageSize": 25,
      "pageCount": 1,
      "total": 1
    }
  }
}
```

### Get Restaurant By ID：

GET /api/restaurants/1

> 成功

```json
{
  "data": {
    "id": 1,
    "attributes": {
      "Name": "Biscotte Restaurant",
      "Description": [
        {
          "type": "paragraph",
          "children": [
            {
              "type": "text",
              "text": "Welcome to Biscotte restaurant! Restaurant Biscotte offers a cuisine based on fresh, quality products, often local, organic when possible, and always produced by passionate producers."
            }
          ]
        }
      ],
      "createdAt": "2024-08-22T06:53:04.569Z",
      "updatedAt": "2024-08-22T07:14:35.267Z",
      "publishedAt": "2024-08-22T07:14:35.263Z"
    }
  },
  "meta": {}
}
```

## 总结

本指南详细介绍了使用Strapi快速搭建内容管理系统的方法，包括创建项目、定义内容类型、添加数据及设置权限等步骤，帮助读者快速掌握Strapi的基本功能和操作流程。
