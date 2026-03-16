# Kids Code Ops

少儿编程机构运营管理平台 (Monorepo)

## 目录结构

| 目录 | 说明 | 技术栈 |
| --- | --- | --- |
| `backend/` | 后端微服务 | Java 17, Spring Boot 3, Spring Cloud |
| `web-admin/` | 教务/运营管理后台 | Vue 3, Vite, TypeScript |
| `web-teacher/` | 教师端 Web | Vue 3, Vite, Mobile First |
| `mini-parent/` | 家长端小程序 | 微信小程序原生 或 Uniapp |
| `docs/` | 项目文档 | OpenAPI, 架构图, 论文资料 |
| `scripts/` | 运维/数据库脚本 | SQL, Shell |

## 端口规划

### 后端服务

| 服务名称 | 模块名 | 端口 | 说明 |
| --- | --- | --- | --- |
| **API Gateway** | `gateway` | **8080** | 统一入口，路由转发 |
| Auth Service | `auth-service` | 8081 | 认证与授权服务 (OAuth2/JWT) |
| RBAC Service | `rbac-service` | 8082 | 用户、角色、权限管理 |
| Student Service | `student-service` | 8083 | 学员管理 |
| Course Service | `course-schedule-service` | 8084 | 课程与排课管理 |
| Finance Service | `finance-service` | 8085 | 订单、支付、财务 |
| File Service | `file-service` | 8086 | 文件上传与存储 (OSS/MinIO) |
| Growth Service | `work-growth-service` | 8087 | 作业与成长档案 |
| Notify Service | `notify-service` | 8088 | 消息通知 (短信/邮件/微信) |
| Analytics Service | `analytics-service` | 8089 | 数据报表与分析 |

### 前端开发服务

| 应用名称 | 目录 | 端口 |
| --- | --- | --- |
| Admin Portal | `web-admin` | 3000 |
| Teacher Portal | `web-teacher` | 3001 |

## 启动顺序

1.  **基础设施 (Infrastructure)**
    
    使用 Docker Compose 一键启动所有中间件：
    ```bash
    docker-compose up -d
    ```
    
    | 服务 | 端口 | 账号 | 密码 | 管理界面/说明 |
    | --- | --- | --- | --- | --- |
    | **MySQL** | 3306 | `root` | `root` | 数据库 `kids_code_db` |
    | **Redis** | 6379 | - | `root` | - |
    | **Nacos** | 8848 | `nacos` | `nacos` | [http://localhost:8848/nacos](http://localhost:8848/nacos) |
    | **RabbitMQ** | 5672 / 15672 | `root` | `root` | [http://localhost:15672](http://localhost:15672) |
    | **MinIO** | 9000 / 9001 | `root` | `rootpassword` | [http://localhost:9001](http://localhost:9001) |

    **健康检查**:
    - 确保所有容器状态为 `Up`。
    - 访问 Nacos 控制台确认能登录。
    - 访问 RabbitMQ 控制台确认能登录。

2.  **后端服务 (Backend)**
    *   优先启动: `gateway`, `auth-service`, `rbac-service`
    *   随后启动: 其他业务微服务 (`student`, `course`, etc.)

3.  **端应用 (Frontend)**
    *   `web-admin`: `npm run dev`
    *   `web-teacher`: `npm run dev`
    *   `mini-parent`: 导入微信开发者工具启动
