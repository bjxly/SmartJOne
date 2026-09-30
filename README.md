# SmartJOne - 基于 Spring Boot 4 的企业级开发框架

<div align="center">

**SmartJOne** 是一个现代化的企业级 Java 开发框架，基于 Spring Boot 4、Spring Cloud 和 MyBatis-Flex 构建。提供完整的多租户、权限管理、认证授权等企业级功能，支持单体和微服务两种部署模式。

[快速开始](#快速开始) • [文档](#文档) • [开发指南](#新项目开发指南) • [Docker 部署](#docker-部署)

</div>

---

## 项目简介

SmartJOne 是一个**开箱即用**的企业应用开发框架，用于快速构建后台管理系统和微服务应用。框架内置了企业级应用所需的核心功能，包括：

- **多租户支持**：一套代码服务多个租户，完整的租户隔离机制
- **权限管理**：细粒度权限控制，支持菜单权限、按钮权限、数据权限
- **认证授权**：基于 JWT + OAuth2.1+ 的现代认证体系
- **用户管理**：用户、角色、部门、菜单等完整的身份管理系统
- **操作日志**：自动记录业务操作，便于审计和追溯
- **对象存储**：内置 S3/MinIO/本地多种存储方案
- **国际化**：完整的 i18n 支持
- **灵活部署**：支持单体和微服务两种模式

## 技术栈

### 后端
| 技术 | 版本 | 说明 |
|------|------|------|
| Java | 21 | OpenJDK 21，虚拟线程支持 |
| Spring Boot | 4.0.6 | 最新稳定版本 |
| Spring Cloud | 2025.1.1 | 微服务支持 |
| Spring Security | 6.4.1 | 安全框架 |
| MyBatis-Flex | 1.11.7 | 轻量级 ORM |
| Flyway | 10.22.0 | 数据库版本管理 |
| SpringDoc OpenAPI | 3.0.3 | API 文档（适配 Spring Boot 4.x） |

### 前端
| 技术 | 版本 | 说明 |
|------|------|------|
| Vue | 3.5.0 | 渐进式框架 |
| TypeScript | 5.3.3 | 类型安全 |
| Vite | 5.0.8 | 现代构建工具 |
| Element Plus | 2.12.0 | UI 组件库 |
| Tailwind CSS | 3.4.17 | 原子化 CSS |
| Pinia | 2.0.32 | 状态管理 |

### 基础设施
| 组件 | 版本 | 说明 |
|------|------|------|
| MySQL | 8.4 | 关系型数据库 |
| Redis | 7 | 缓存 |
| MinIO | latest | 对象存储 |
| Docker | 24.0+ | 容器化部署 |

## 项目结构

```
smartjone/
├── smartj-backend/                    # 后端项目
│   ├── smartj-common/                 # 公共模块
│   │   ├── smartj-common-core/        # 核心工具和常量
│   │   ├── smartj-common-web/         # Web 框架集成
│   │   ├── smartj-common-cache/       # 缓存 Redis 集成
│   │   ├── smartj-common-db/           # 数据源 + MyBatis-Flex 集成
│   │   ├── smartj-common-security/    # Spring Security
│   │   ├── smartj-common-auth/        # JWT 认证
│   │   ├── smartj-common-oauth2-server/ # OAuth2 服务
│   │   ├── smartj-common-tenant/      # 多租户支持
│   │   ├── smartj-common-oss/         # 对象存储
│   │   ├── smartj-common-log/         # 日志系统
│   │   └── smartj-common-feign/       # 微服务调用
│   ├── smartj-api/                    # API 契约层（DTO / VO / Query）
│   ├── smartj-biz/                    # 业务实现层（Entity / Service /Mapper）
│   ├── smartj-bootstrap/              # 启动模块
│   │   ├── smartj-bootstrap-boot/     # 单体模式启动 ⭐
│   │   ├── smartj-bootstrap-auth/     # 认证服务启动
│   │   ├── smartj-bootstrap-sys/      # 系统服务启动
│   │   └── ...                        # 其他微服务启动器
│   ├── smartj-gateway/                # Spring Cloud Gateway
│   ├── smartj-samples/                # 示例项目
│   └── smartj-archetype/              # Maven 原型
├── smartj-ui/                         # 前端项目
│   └── src/
│       ├── api/                       # API 请求
│       ├── components/                # 全局组件（SmCrudTable、SmSearchForm 等）
│       ├── composables/               # 组合式函数（useDict、usePagination 等）
│       ├── layouts/                   # 页面布局
│       ├── router/                    # 路由配置
│       ├── stores/                    # Pinia 状态管理
│       ├── styles/                    # 全局样式
│       ├── types/                     # TypeScript 类型定义
│       └── views/                     # 业务页面
```

## 环境要求

- **JDK 21**
- **Maven 3.9+**
- **Node.js 18+**
- **MySQL 8.4+**（可选，Docker 环境自带）
- **Redis 7+**（可选，Docker 环境自带）
- **Docker 24.0+**（部署用）

## 快速开始

### 方式一：Docker Compose 一键启动（推荐）

```powershell
cd deploy

# Windows 一键编译 + 启动
.\start.ps1

# 或分步操作
.\build.ps1           # 编译后端和前端
docker compose up -d  # 启动所有服务
```

```bash
# Linux / macOS
cd deploy
chmod +x build.sh && ./build.sh
docker compose up -d
```

| 服务 | URL |
|------|-----|
| 前端应用 | http://localhost |
| 后端 API | http://localhost:8080/api |
| MinIO 控制台 | http://localhost:9011 |

默认账号：**root** / **123456**

---

### 方式二：本地开发模式

#### 1. 启动依赖服务

```bash
cd deploy
docker compose up -d mysql redis minio
```

#### 2. 启动后端

**IDE 中启动**：运行 `SmartjApplication.java`（smartj-bootstrap-boot 模块）

**命令行**：
```bash
cd smartj-backend
mvn clean install -DskipTests
cd smartj-bootstrap/smartj-bootstrap-boot
mvn spring-boot:run
```

后端地址：`http://localhost:8080/api`

#### 3. 启动前端

```bash
cd smartj-ui
npm install    # 首次执行
npm run dev
```

前端地址：`http://localhost:5173`

---

## 微服务模式

微服务模式将各业务模块拆分为独立服务，通过 Spring Cloud Gateway 统一入口。

### 启动步骤

1. 启动注册中心（如 Nacos）
2. 分别启动各微服务：
   - `smartj-bootstrap-auth`（认证服务）
   - `smartj-bootstrap-sys`（系统服务）
   - `smartj-bootstrap-tenant`（租户服务）
   - `smartj-bootstrap-oss`（OSS 服务）
   - `smartj-bootstrap-log`（日志服务）
3. 启动网关：`smartj-gateway`

```bash
# 以认证服务为例
cd smartj-backend/smartj-bootstrap/smartj-bootstrap-auth
mvn spring-boot:run
```

---

## 新项目开发指南

### 使用 Maven 原型生成新项目

```bash
mvn archetype:generate \
  -DarchetypeGroupId=com.smartj \
  -DarchetypeArtifactId=smartj-archetype \
  -DarchetypeVersion=1.0.0 \
  -DgroupId=com.example \
  -DartifactId=my-project
```

### 手动新增业务模块

> **推荐方式**：提供 DDL 建表语句，使用 `ddl-to-code` Skill 一键生成全部前后端代码。
> 详见 [`.qoder/skills/ddl-to-code/`](.qoder/skills/ddl-to-code/)。

#### 1. 创建模块结构

以订单模块为例，需要在 **api** 和 **biz** 两层分别创建：

```
smartj-api/smartj-api-sys/
└── src/main/java/com/smartj/{module}/order/
    ├── dto/
    │   ├── OrderDTO.java         # 新增/编辑请求体
    │   └── OrderQuery.java       # 分页查询条件
    └── vo/
        └── OrderVO.java          # 响应视图

smartj-biz/smartj-biz-sys/
└── src/main/java/com/smartj/{module}/order/
    ├── entity/
    │   └── Order.java            # 继承 BaseEntity
    ├── mapper/
    │   └── OrderMapper.java      # 继承 BaseMapper<Order>
    ├── converter/
    │   └── OrderConverter.java   # Entity → VO (MapStruct)
    ├── service/
    │   ├── OrderService.java     # 接口
    │   └── impl/
    │       └── OrderServiceImpl.java
    └── controller/
        └── OrderController.java  # REST 接口
```

#### 2. 数据库 DDL

建表语句遵循 BaseEntity 规范（`id` 使用雪花算法，不含 AUTO_INCREMENT）：

```sql
CREATE TABLE biz_order (
    id           BIGINT       NOT NULL COMMENT '主键',
    tenant_id    BIGINT       NOT NULL COMMENT '租户ID',
    order_no     VARCHAR(64)  NOT NULL COMMENT '订单号',
    amount       DECIMAL(15,2) NOT NULL COMMENT '金额',
    order_status TINYINT      NOT NULL DEFAULT 0 COMMENT '订单状态',
    status       TINYINT      NOT NULL DEFAULT 1 COMMENT '状态 0=禁用 1=启用',
    del_flg      TINYINT      NOT NULL DEFAULT 0 COMMENT '逻辑删除 0=正常 1=删除',
    create_by    VARCHAR(64)  NOT NULL DEFAULT '' COMMENT '创建人',
    create_time  DATETIME     NOT NULL COMMENT '创建时间',
    update_by    VARCHAR(64)  NOT NULL DEFAULT '' COMMENT '更新人',
    update_time  DATETIME     NOT NULL COMMENT '更新时间',
    version      INT          NOT NULL DEFAULT 0 COMMENT '乐观锁版本号',
    remark       VARCHAR(500) NULL     COMMENT '备注',
    PRIMARY KEY (id),
    KEY idx_tenant (tenant_id),
    UNIQUE KEY uk_order_no (order_no)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci COMMENT='订单表';
```

迁移脚本放入 `deploy/init-db/`（Docker 环境），或通过 Flyway 管理。

#### 3. 后端代码

**Entity**（继承 BaseEntity，只声明业务字段）：

```java
@Data
@EqualsAndHashCode(callSuper = true)
@Table("biz_order")
public class Order extends BaseEntity {
    @Serial
    private static final long serialVersionUID = 1L;

    private String orderNo;
    private BigDecimal amount;
    private Integer orderStatus;
}
```

> BaseEntity 已包含 `id`、`tenantId`、`status`、`delFlg`、`createBy`、`createTime`、`updateBy`、`updateTime`、`version`、`remark`，子类无需重复声明。

**DTO / VO / Query** 遵循 [sm-backend.md](.qoder/rules/sm-backend.md) 中定义的模式。

**完整代码示例**见现有模块（如 `smartj-biz-sys/post/`），或使用 `ddl-to-code` Skill 从 DDL 一键生成。

#### 4. 注册模块

编辑 `smartj-backend/pom.xml` 添加 module，编辑 `smartj-bootstrap-boot/pom.xml` 添加依赖。

#### 5. 前端页面

前端使用 `SmSearchForm` + `SmCrudTable` 框架组件构建 CRUD 页面，遵循 [sm-frontend.md](.qoder/rules/sm-frontend.md) 规范。

在 `smartj-ui/src/views/order/index.vue` 创建页面，在后台菜单管理中注册菜单路由。

---

## Docker 部署

### 编译

```powershell
cd deploy
.\build.ps1    # Windows
./build.sh     # Linux/Mac
```

### 启动

```bash
docker compose up -d
```

### 常用命令

```bash
docker compose ps                    # 查看状态
docker compose logs -f smartj-backend # 查看日志
docker compose restart smartj-backend # 重启后端
docker compose stop                   # 停止（数据保留）
docker compose down -v                # 完全清理（⚠️慎用）
```

### 环境变量

编辑 `deploy/.env` 调整：数据库密码、Redis 配置、MinIO 凭证、JVM 内存等。

---

## 常见问题

| 问题 | 解决方案 |
|------|----------|
| Docker 启动后数据库连接失败 | 等待 MySQL 完全初始化（约 30s），查看 `docker compose logs mysql` |
| 前端 API 返回 401 | Token 过期，重新登录；或检查 JWT 密钥文件是否存在 |
| 服务重启后 Token 失效 | 确认 `config/smartj-jwt.key` 文件存在（首次启动自动生成） |
| 如何重置 JWT 密钥 | 删除 `config/smartj-jwt.key` 后重启应用 |

## 代码质量检查

### 前端
```bash
cd smartj-ui
npm run lint        # ESLint 修复模式
npm run lint:check  # ESLint 仅检查
npm run format      # Prettier 格式化全部
npm run type-check  # TypeScript 类型检查
```

### 后端
```bash
cd smartj-backend
mvn checkstyle:check  # Checkstyle 代码规范检查
```

> Git 提交时 Husky + lint-staged 自动对前端暂存文件执行 ESLint + Prettier 检查。

---

## 许可证
Apache 2.0
