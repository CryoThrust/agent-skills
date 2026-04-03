# Style Guide

## Color System

### Component Colors

| 用途 | 背景色 | 边框色 |
|------|--------|--------|
| 普通组件 | #ffffff | #d1d5db |
| 数据源/外部系统 | #f0fdfa | #5eead4 |
| 虚线子组件 | #f8fafb | #cbd5e1 |
| 高亮/实验特性 | #fffbeb | #f59e0b |
| 基础设施层 | #f0fdf4 | #a7f3d0 |

### Tag Colors (分组标题)

| 颜色 | 色值 | 适用场景 |
|------|------|---------|
| teal | #0d9488 | 核心基础设施、配置中心、Store |
| blue | #2563eb | 网关、路由、API层 |
| green | #059669 | 业务模块、服务集群、数据层 |
| red | #e11d48 | 控制、安全、守卫 |
| orange | #d97706 | 协调、调度、一致性 |
| violet | #7c3aed | 事件、消息、异步通信 |

### Connection Line Colors

| 语义 | 颜色 | 色值 | 适用场景 |
|------|------|------|---------|
| 主流程 | 灰色 | #94a3b8 | 请求流、页面跳转、组件调用 |
| 数据流 | 绿色 | #059669 | 数据库、缓存、状态管理 |
| 依赖 | 蓝色 | #2563eb | 模块引用、服务调用 |
| 配置 | 青色 | #0d9488 | 配置中心、服务发现、Store |
| 控制 | 红色 | #e11d48 | 权限、熔断、路由守卫 |
| 事件 | 紫色 | #7c3aed | 消息队列、事件总线 |

**架构适配：**

| 架构 | 主流程(灰) | 数据流(绿) | 依赖(蓝) | 事件(紫) |
|------|-----------|-----------|---------|---------|
| Vue/React | 页面→组件 | Props/State | Import引用 | EventEmitter |
| 微服务 | 请求→网关 | 数据库 | 服务调用 | MQ消息 |
| 后端分层 | Controller→Service | DAO | 模块依赖 | 异步事件 |

## Component Sizes

| 类型 | 宽度 | 高度 | 用途 |
|------|------|------|------|
| 小型 | 100-120px | 32-36px | 子功能、标签 |
| 标准 | 130-160px | 40-48px | 核心模块 |
| 大型 | 170-220px | 50-60px | 主服务 |

## Text Sizes

| 元素 | 字号 | 字重 | Class |
|------|------|------|-------|
| 主标题 | 17px | 700 | - |
| 组件名 | 13px | 600 | - |
| 副标题 | 10px | 400 | `.sub` |
| 技术说明 | 9.5px | 400 | `.tech` |
| 分组标签 | 9.5px | 700 | `.tag` |
| 连线文字 | 9px | 400 | `.connection-label` |

## Connection Label Styles

```css
.connection-label {
  font-size: 9px;
  fill: #64748b;
  font-family: -apple-system, BlinkMacSystemFont, 'Inter', 'PingFang SC', sans-serif;
  pointer-events: none;  /* 不阻挡鼠标事件 */
}
```

**位置规则**：
- 水平连线：上方 5px 或下方 12px（遮挡时）
- 垂直连线：右侧 8px 或左侧 8px（遮挡时）
- 最大偏移：15px（禁止漂移）

**长度限制**：
- 纯英文：最多 8 字符
- 纯中文：最多 6 字符
- 混合：总宽度 < 50px
