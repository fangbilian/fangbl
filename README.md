# 多租户管理系统 (Multi-Tenant Management System)

## 项目概述

这是一个基于 Spring Boot + Vue 3 + MySQL 的企业级多租户SaaS平台，支持：
- ✅ 多租户数据隔离
- ✅ 动态租户切换
- ✅ 灵活的套餐管理和功能权限控制
- ✅ 租户级别的个性化配置

## 技术栈

### 后端
- Java 8+
- Spring Boot 2.7.x / 3.x
- Spring Security
- MyBatis Plus
- MySQL 8.0+
- Redis
- Swagger 3.0

### 前端
- Vue 3
- Element Plus
- TypeScript
- Axios

### 数据库
- MySQL 8.0+
- Redis (缓存和租户上下文)

## 项目结构

```
fangbl/
├── backend/                          # 后端项目
│   ├── pom.xml
│   ├── src/
│   │   ├── main/
│   │   │   ├── java/com/fangbl/
│   │   │   │   ├── config/          # 配置类（租户配置、安全配置等）
│   │   │   │   ├── context/         # 租户上下文
│   │   │   │   ├── controller/      # 控制层
│   │   │   │   ├── service/         # 业务逻辑层
│   │   │   │   ├── mapper/          # 数据访问层
│   │   │   │   ├── entity/          # 实体类
│   │   │   │   ├── dto/             # 数据传输对象
│   │   │   │   ├── interceptor/     # 拦截器（租户识别）
│   │   │   │   ├── exception/       # 异常处理
│   │   │   │   └── FangblApplication.java
│   │   │   └── resources/
│   │   │       ├── application.yml
│   │   │       ├── application-dev.yml
│   │   │       ├── application-prod.yml
│   │   │       └── db/
│   │   │           └── migration/   # 数据库迁移脚本
│   │   └── test/
│   └── docs/
│       └── api.md                   # API 文档
│
├── frontend/                         # 前端项目
│   ├── package.json
│   ├── vite.config.ts
│   ├── tsconfig.json
│   ├── src/
│   │   ├── api/                     # API 请求
│   │   ├── components/              # 组件
│   │   ├── pages/                   # 页面
│   │   ├── store/                   # Pinia 状态管理
│   │   ├── types/                   # TypeScript 类型定义
│   │   ├── App.vue
│   │   └── main.ts
│   └── public/
│
└── docs/                             # 文档
    ├── ARCHITECTURE.md               # 架构文档
    ├── TENANT_ISOLATION.md           # 租户隔离方案
    └── SETUP.md                      # 部署指南
```

## 核心功能

### 1. 租户隔离机制
- **数据库级隔离**: 每个租户拥有独立的数据库 Schema 或表空间
- **行级隔离**: 共享数据库表时，通过 tenant_id 字段进行数据隔离
- **Redis 缓存隔离**: 缓存键包含租户标识

### 2. 租户管理
- 租户注册和认证
- 租户信息维护
- 租户状态管理（激活、暂停、注销）

### 3. 套餐管理
- 多种套餐方案（基础版、专业版、企业版）
- 按套餐限制功能模块
- 按套餐限制用户数、数据量等资源

### 4. 用户和权限
- 租户下的用户管理
- 基于角色的访问控制 (RBAC)
- 细粒度的功能权限控制

### 5. 个性化配置
- 租户级别的系统配置
- 品牌定制（Logo、配色等）
- 功能开关

## 快速开始

### 后端启动

```bash
cd backend

# 安装依赖
mvn clean install

# 启动应用
mvn spring-boot:run
```

应用将在 `http://localhost:8080` 启动。

### 前端启动

```bash
cd frontend

# 安装依赖
npm install

# 开发环境运行
npm run dev
```

前端将在 `http://localhost:5173` 启动。

## 环境配置

### 数据库配置

```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/fangbl?useUnicode=true&characterEncoding=utf8&serverTimezone=UTC
    username: root
    password: your_password
    driver-class-name: com.mysql.cj.jdbc.Driver
```

### Redis 配置

```yaml
spring:
  redis:
    host: localhost
    port: 6379
    password: 
```

## API 文档

访问 `http://localhost:8080/swagger-ui.html` 查看完整的 API 文档。

## 核心代码实现

### 租户上下文

租户识别通过以下方式进行：
1. **HTTP Header**: `X-Tenant-ID`
2. **URL 参数**: `?tenantId=xxx`
3. **JWT Token**: 令牌中包含租户信息
4. **子域名**: `tenant1.example.com`

### 数据库隔离策略

- 使用 MyBatis Plus 的拦截器自动添加 `tenant_id` 条件
- 所有 SELECT、UPDATE、DELETE 操作自动过滤
- 防止跨租户数据泄露

## 测试

```bash
# 后端单元测试
cd backend
mvn test

# 前端单元测试
cd frontend
npm run test
```

## 部署

详见 [部署指南](./docs/SETUP.md)

## 许可证

MIT
