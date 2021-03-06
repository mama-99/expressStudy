[toc]

# expressStudy

这是本人学习express的一个仓库，默认启动在3000端口，本项目并未做跨域处理，如使用vue/cli可直接将`vue.config.js`文件复制到项目根目录并重启项目通过反向代理解决跨域问题(2021-02-22已添加跨域中间件)。

拉取代码后请运行一次`expressdb.sql`文件以确保数据库表结构的同步，可在`config/config.js`中修改数据库配置

```
npm install  //安装依赖
npm run dev  //启动项目
```

### 目录结构

- bin
  - www  入口文件
- config
  - config.js  配置文件
- middleware 中间件文件夹
- public 静态文件文件夹
- routes 路由文件夹
- utils 
  - auth.js  token工具文件
  - db.js  封装的数据库工具文件
- views 模板文件夹
- app.js express实例文件
- package.json 依赖管理文件

### 状态码规范

状态码|含义
-|-
200|请求成功
400|请求参数错误
401|未登录
402|其他错误
500|服务器错误

## 接口文档

### 登录接口

- 接口地址

  ```
  /api/users/login
  ```

- 请求方式

  POST
  
- 入参

  | 字段     | 参数类型 | 说明   | 必传 |
  | -------- | -------- | ------ | ---- |
  | username | string   | 用户名 | true |
  | password | string   | 密码   | true |

- 出参

  ```
  {
      "code": 200,
      "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6InVzZXJzIiwiaWQiOjQsImlhdCI6MTYxMzc4ODI5OSwiZXhwIjoxNjEzNzkwMDk5fQ.7-QFXLJgJrkvBloWAPG40W1-NE_jcTGeIHD0onyO6Dc",
      "message": "登录成功"
  }
  ```

### 添加管理员接口

- 接口地址

  ```
  /api/users/register
  ```

- 请求方式

  POST
  
- 入参

  | 字段     | 参数类型 | 说明   | 必传 |
  | -------- | -------- | ------ | ---- |
  | username | string   | 用户名 | true |
  | password | string   | 密码   | true |

- 出参

  ```
  {
      "code": 200,
      "message": "注册成功"
  }
  ```

### 获取用户信息

- 接口地址

  ```
  /api/users/getUserInfo
  ```

- 请求方式

  GET

- 入参无

- 出参

  ```
  {
      "code": 200,
      "data": {
          "username": "admin",
          "id": 4,
          "role_id": 1,
          "role_name": "admin",
          "iat": 1613806589,
          "exp": 1613808389
      },
      "message": "获取用户信息成功"
  }
  ```

### 修改用户权限

- 接口地址

  ```
  /api/role/setUserRole
  ```

- 请求方式

  POST
  
- 入参

  | 字段    | 类型   | 说明           | 必传 |
  | ------- | ------ | -------------- | ---- |
  | user_id | number | 用户id         | true |
  | role_id | number | 修改后的角色id | true |

- 出参

  ```
  {
      "code": 200,
      "message": "修改成功"
  }
  ```

### 获取职位列表

- 接口地址

  ```
  /api/job/getJobList
  ```

- 请求方式

  GET

- 入参 无

- 出参

  ```
  {
      "code": 200,
      "data": [
          {
              "job_id": 1,
              "job_name": "web大前端",
              "deleted": 0
          },
          {
              "job_id": 2,
              "job_name": "php后端开发",
              "deleted": 0
          }
      ],
      "message": "请求成功"
  }
  ```

### 添加职位

- 接口地址

  ```
  /api/job/addJob
  ```

- 请求方式

  POST

- 入参

  | 字段     | 类型   | 说明     | 必传 |
  | -------- | ------ | -------- | ---- |
  | job_name | string | 职位名称 | true |

- 出参

  ```
  {
      code: 200,
      message:"添加成功"
  }
  ```

  

### 删除岗位

- 接口地址

  ```
  /api/job/delJob
  ```

- 请求方式

  POST

- 入参

  | 字段    | 类型               | 说明                                     | 必传 |
  | ------- | ------------------ | ---------------------------------------- | ---- |
  | job_ids | string \|\| number | 删除职位id，批量删除中间使用英文逗号隔开 | true |

- 出参

  ```
  {
      code: 200,
      message:"删除成功"
  }
  ```

  

### 修改职位

- 接口地址

  ```
  /api/job/setJob
  ```

- 请求方式

  POST

- 入参

  | 字段     | 类型   | 说明             | 必传 |
  | -------- | ------ | ---------------- | ---- |
  | job_id   | number | 职位id           | true |
  | job_name | string | 修改后的职位名称 | true |

  

### 获取员工列表

- 接口地址

  ```
  /api/personnel/getPersonnels
  ```

- 请求方式

  GET
  
- 入参

  | 字段     | 类型   | 说明         | 必传 |
  | -------- | ------ | ------------ | ---- |
  | page_num  | number | 当前页       | true |
  | page_size | number | 每页数据条数 | true |

  

- 出参

  ```
  {
      "code": 200,
      "data": [
          {
              "personnel_id": 6,
              "personnel_name": "上官翠花",
              "create_time": "2021-02-22T01:10:42.000Z",
              "job_id": 1,
              "job_name": "web前端"
          }
      ],
      "page": {
          "pageNum": "2",
          "pageSize": "5",
        "total": 6
      },
      "message": "查询成功"
  }
  ```

### 添加员工

- 接口地址

  ```
  /api/personnel/addPersonnel
  ```

- 请求方式

  POST

- 入参

  | 字段           | 类型   | 说明      | 必传 |
  | -------------- | ------ | --------- | ---- |
  | personnel_name | string | 员工姓名  | true |
  | job_id         | number | 员工职位  | true |
  | sex            | number | 0 男，1女 | true |

- 出参

  ```
  {
      code: 200,
      message:"添加成功"
  }
  ```

### 删除员工

- 接口地址

  ```
  /api/personnel/delPersonnel
  ```

- 请求方式

  POST

- 入参

  | 字段          | 类型               | 说明                                 | 必传 |
  | ------------- | ------------------ | ------------------------------------ | ---- |
  | personnel_ids | string \|\| number | 删除员工id，批量删除使用英文逗号隔开 | true |

- 出参

  ```
  {
      code: 200,
      message:"删除成功"
  }
  ```

### 编辑员工信息

- 接口地址

  ```
  /api/personnel/setPersonnel
  ```

- 请求方式

  POST

- 入参

  | 字段         | 类型   | 说明   | 必传 |
  | ------------ | ------ | ------ | ---- |
  | personnel_id | number | 员工id | true |
  | dept_id      | number | 部门id | true |
  | job_id       | number | 岗位id | true |

- 出参

  ```
  {
      code:200,
      message:"编辑成功"
  }
  ```

### 查询部门列表

- 接口地址

  ```
  /api/dept/getDeptList
  ```

- 请求方式

  GET

- 入参 无

- 出参

  ```
  {
      "code": 200,
      "data": [
          {
              "dept_id": 1,
              "dept_name": "研发组"
          },
          {
              "dept_id": 2,
              "dept_name": "人事管理部"
          },
          {
              "dept_id": 3,
              "dept_name": "综合部"
          }
      ],
      "message": "查询成功"
  }
  ```

### 添加部门

- 接口地址

  ```
  /api/dept/addDept
  ```

- 请求方式

  POST

- 入参

  | 字段      | 类型   | 说明     | 必传 |
  | --------- | ------ | -------- | ---- |
  | dept_name | string | 部门名称 | true |

- 出参

  ```
  {
      code: 200,
      message: "添加成功"
  }
  ```

### 删除部门

- 接口地址

  ```
  /api/dept/delDept
  ```

- 请求方式

  POST

- 入参

  | 字段     | 类型               | 说明                     | 必传 |
  | -------- | ------------------ | ------------------------ | ---- |
  | dept_ids | string \|\| number | 部门id，多个id用逗号隔开 | true |

- 出参

  ```
  {	
      code:200,
      message:"删除成功"
  }
  ```

### 编辑部门

- 接口地址

  ```
  /api/dept/setDept
  ```

- 请求方式

  POST

- 入参

  | 字段      | 类型   | 说明       | 必传 |
  | --------- | ------ | ---------- | ---- |
  | dept_id   | number | 部门id     | true |
  | dept_name | string | 新部门名称 | true |

- 出参

  ```
  {
      code: 200,
      message: "编辑成功"
  }
  ```

  