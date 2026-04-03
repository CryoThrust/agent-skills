---
name: architecture-diagram
description: This skill should be used when the user asks to "draw an architecture diagram", "create architecture diagram", "generate architecture", "画架构图", "生成架构图", "绘制架构图", or mentions architecture, microservice architecture, frontend architecture (Vue/React), system architecture, deployment architecture, technology architecture, or needs to visualize system structure with components and connections.
version: 1.0.0
author: lihongyan
email: Y0hanes@Outlook.com
---

# Architecture Diagram Generator

Generate professional HTML architecture diagrams with collision-free layout, clear connections, and unified styling.

## Overview

This skill generates a complete HTML architecture diagram page based on user-provided architecture content, ensuring:
- No component collisions
- Clear connection lines
- Unified styling
- Responsive design

## Execution Steps

### Step 1: Understand Architecture Content

Analyze user input to identify:
1. Architecture modules and components
2. Hierarchical relationships (entry layer, core layer, infrastructure layer)
3. Connections between components

### Step 2: Layout Planning (Required Output)

Before writing HTML, output layout planning tables:

```
## Layout Planning

### Layer Division
| Layer | Top Range | Purpose |
|-------|-----------|---------|
| Title | 20-80px | Title, process bar |
| Layer 1 | 90-200px | Entry/Gateway components |
| Spacing | 200-280px | Connection channel |
| Layer 2 | 280-500px | Core components |
| Layer 3 | 510-620px | Infrastructure |

### Column Division
| Column | Left Range | Purpose |
|--------|------------|---------|
| Col 1 | 50-200px | Entry |
| Col 2 | 220-400px | Gateway |
| Col 3 | 420-620px | Core services |
| Col 4 | 640-800px | Storage/External |
| Col 5 | 820-1000px | Config/Governance |
| Right | right:40px, width 260px | Info cards |

### Group Coordinates
| ID | Name | left | top | width | height | right | bottom |
|----|------|------|-----|-------|--------|-------|--------|
| G1 | xxx | 230 | 90 | 160 | 120 | 390 | 210 |

### Component Coordinates
| ID | Group | left | top | width | height |
|----|-------|------|-----|-------|--------|
| C1 | G1 | 246 | 116 | 128 | 40 |
```

### Step 3: Collision Detection (Required)

```
## Collision Detection

### Group Collision Check
Formula: A.right < B.left OR A.left > B.right (horizontal)
         A.bottom < B.top OR A.top > B.bottom (vertical)

| Group Pair | Result |
|------------|--------|
| G1 vs G2 | G1.right(390) < G2.left(420) OK, gap=30px |

### Component Boundary Check
| Component | Group Range | Component Coords | Result |
|-----------|-------------|------------------|--------|
| C1 | G1: 230-390, 90-210 | 246-374, 116-156 | OK |

### Right Area Check
Max group right = xxx
Right info area left = 1440 - 40 - 260 = 1140
Check: xxx < 1140 OK
```

### Step 4: Apply Style Rules

**必须使用以下样式规范：**

#### 颜色系统

| 用途 | 背景色 | 边框色 |
|------|--------|--------|
| 普通组件 | #ffffff | #d1d5db |
| 数据源/外部系统 | #f0fdfa | #5eead4 |
| 虚线子组件 | #f8fafb | #cbd5e1 |
| 高亮/实验特性 | #fffbeb | #f59e0b |
| 基础设施层 | #f0fdf4 | #a7f3d0 |

#### 标签颜色（分组标题）

| 颜色 | 色值 | 适用场景 |
|------|------|---------|
| teal | #0d9488 | 注册中心、配置中心 |
| blue | #2563eb | 网关、网络 |
| green | #059669 | 服务集群、业务模块 |
| red | #e11d48 | 流量治理、熔断 |
| orange | #d97706 | 事务、调度 |
| violet | #7c3aed | 消息队列、事件 |

#### 连线颜色

**根据连线语义选择颜色，颜色含义通用化：**

| 语义类型 | 颜色 | 色值 | 适用场景示例 |
|---------|------|------|-------------|
| 主流程/请求流 | 灰色 | #94a3b8 | 用户请求→网关→服务、页面跳转、组件调用 |
| 数据流/存储 | 绿色 | #059669 | 读写数据库、缓存访问、数据同步 |
| 事件/消息 | 紫色 | #7c3aed | 消息队列、事件总线、发布订阅 |
| 依赖/引用 | 蓝色 | #2563eb | 模块依赖、组件引用、服务调用 |
| 治理/控制 | 红色 | #e11d48 | 熔断限流、权限控制、路由规则 |
| 配置/元数据 | 青色 | #0d9488 | 配置中心、服务发现、环境变量 |
| 事务/一致性 | 橙色 | #d97706 | 分布式事务、状态同步、一致性保证 |

**不同架构类型的使用示例：**

| 架构类型 | 主流程(灰) | 数据流(绿) | 事件(紫) | 依赖(蓝) |
|---------|-----------|-----------|---------|---------|
| Vue/React | 页面→组件 | Props/State | EventBus | Import引用 |
| 微服务 | 请求→网关 | 数据库 | MQ消息 | 服务调用 |
| 后端分层 | Controller→Service | DAO | 异步事件 | 模块依赖 |
| 部署架构 | LB→服务器 | 数据同步 | 告警通知 | 服务依赖 |

#### 组件尺寸

| 类型 | 宽度 | 高度 |
|------|------|------|
| 小型 | 100-120px | 32-36px |
| 标准 | 130-160px | 40-48px |
| 大型 | 170-220px | 50-60px |

详细样式见 `references/style-guide.md`。

### Step 5: Apply Layout Constraints

Follow coordinate, spacing, and capacity rules defined in `references/layout-rules.md`.

### Step 6: Connection Rules (重要)

**必须严格按照以下规则绘制连线：**

#### 锚点计算

每个组件有 4 个锚点（基于组件坐标计算）：

| 锚点 | X 坐标 | Y 坐标 |
|------|--------|--------|
| 左中 | left | top + height/2 |
| 右中 | right | top + height/2 |
| 上中 | left + width/2 | top |
| 下中 | left + width/2 | bottom |

#### 路径类型选择

根据起点和终点位置选择正确的路径类型：

| 场景 | 路径类型 | 说明 |
|------|---------|------|
| 起点→终点：起点在终点左边，同一行 | 水平直达 | 起点右中 → 终点左中 |
| 起点→终点：起点在终点上边，同一列 | 垂直直达 | 起点下中 → 终点上中 |
| 起点→终点：跨行跨列 | L型路径 | 需要一个拐点 |
| 起点→终点：多层组件连接 | 中间层路径 | 通过中间层（Y=200-280px）中转 |

#### 直达路径代码

```html
<!-- 水平直达：从 A 右中 到 B 左中 -->
<line x1="A.right" y1="A.top + A.height/2" x2="B.left" y2="B.top + B.height/2" stroke="#94a3b8" stroke-width="1.5" marker-end="url(#ah)" />

<!-- 垂直直达：从 A 下中 到 B 上中 -->
<line x1="A.left + A.width/2" y1="A.bottom" x2="B.left + B.width/2" y2="B.top" stroke="#94a3b8" stroke-width="1.5" marker-end="url(#ah)" />
```

#### L型路径代码（必须用 path）

```html
<!-- L型：从 A 右中 → 拐点 → B 上中/下中/左中 -->
<!-- 拐点X 选择：起点 right + 20 或 终点 left - 20 或 中间空白区域 -->
<!-- 拐点Y 选择：起点 Y 或 终点 Y -->

<!-- 示例：A(左上) → B(右下)，拐点在 A 右边 -->
<path d="M A.right A.top+A.height/2 L (A.right+20) (A.top+A.height/2) L (A.right+20) (B.top+B.height/2) L B.left (B.top+B.height/2)" stroke="#0d9488" stroke-width="1.2" stroke-dasharray="4,3" fill="none" marker-end="url(#ah-teal)" />
```

#### 中间层路径（跨行连接）

```html
<!-- 通过中间层（Y=240px）中转 -->
<!-- A 下中 → (A.x, 240) → (B.x, 240) → B 上中 -->
<path d="M (A.left+A.width/2) A.bottom L (A.left+A.width/2) 240 L (B.left+B.width/2) 240 L (B.left+B.width/2) B.top" stroke="#0d9488" stroke-width="1.2" stroke-dasharray="4,3" fill="none" marker-end="url(#ah-teal)" />
```

#### 连线避让规则

1. **连线不能穿过任何组件内部**
2. **拐点必须放在空白区域**（组件之间的间隙）
3. **多条连线平行时间距 >= 10px**
4. **所有坐标必须是具体数值**，不能用变量

#### 连线规划表（生成前必须输出）

```
## Connection Planning

| ID | From | To | Type | Anchor | Path |
|----|------|-----|------|--------|------|
| L1 | C1 | C2 | 直达 | C1右中→C2左中 | line(195,140)-(230,140) |
| L2 | C2 | C3 | L型 | C2下中→C3上中 | path via (X,240) |
```

### Step 7: Pre-generation Checklist

```
## Pre-generation Checklist

### Coordinate Check
[ ] All group coordinates are multiples of 10
[ ] All component coordinates are multiples of 10
[ ] All sizes are multiples of 10 or close values

### Collision Check
[ ] Same-row groups don't overlap, gap >= 30px
[ ] Same-group components don't overlap, gap >= 8px
[ ] Components don't exceed group boundaries

### Boundary Check
[ ] Leftmost component left >= 40px
[ ] Rightmost group right <= 1100px (for right info area)
[ ] Topmost component top >= 80px (for title area)
[ ] Bottom component/group doesn't overlap with legend

### Connection Check
[ ] Connection拐点 in blank area
[ ] Connections don't pass through component interior
[ ] Connection arrow direction correct

### Info Area Check
[ ] Right info cards right: 40px
[ ] Info cards don't overlap with main groups
[ ] Info cards vertical gap >= 20px
```

### Step 8: Generate HTML

**重要：必须使用以下 CSS 类和结构生成 HTML**

#### 必须使用的 CSS 类

```css
.box          /* 普通组件 */
.box.source   /* 数据源/外部系统组件 */
.box.dashed   /* 虚线子组件 */
.group        /* 分组容器 */
.tag          /* 分组标签 */
.tag.teal/.blue/.green/.red/.orange/.violet  /* 标签颜色 */
.enclosure    /* 基础设施层包围框 */
.legend       /* 图例 */
```

#### 组件内文字样式

```css
.sub          /* 次要说明文字，10px，灰色 */
.tech         /* 技术说明文字，9.5px，浅灰色 */
```

#### SVG 连线箭头定义（放在 <defs> 中）

```html
<defs>
  <marker id="ah" markerWidth="7" markerHeight="5" refX="7" refY="2.5" orient="auto">
    <polygon points="0 0, 7 2.5, 0 5" fill="#94a3b8" />
  </marker>
  <marker id="ah-teal" markerWidth="7" markerHeight="5" refX="7" refY="2.5" orient="auto">
    <polygon points="0 0, 7 2.5, 0 5" fill="#0d9488" />
  </marker>
  <marker id="ah-red" markerWidth="7" markerHeight="5" refX="7" refY="2.5" orient="auto">
    <polygon points="0 0, 7 2.5, 0 5" fill="#e11d48" />
  </marker>
  <!-- 其他颜色类似 -->
</defs>
```

#### 连线流动动画

模板已内置流动动画 CSS，只需添加 `class="flow"` 即可：

```html
<!-- 带流动效果的线 -->
<line x1="X1" y1="Y1" x2="X2" y2="Y2" stroke="#94a3b8" stroke-width="1.5" marker-end="url(#ah)" class="flow" />

<!-- 带流动效果的路径 -->
<path d="M ..." stroke="#0d9488" stroke-width="1.2" fill="none" marker-end="url(#ah-teal)" class="flow" />
```

| 类名 | 速度 | 适用场景 |
|-----|------|---------|
| `flow` | 1.5s | 标准流动 |
| `flow-slow` | 2.5s | 长连线、次要流程 |
| `flow-fast` | 1s | 短连线、主要流程 |

#### 连线生成要求

**Step 6 已经定义了锚点和路径规则，生成 HTML 时必须：**

1. **先输出连线规划表**（见 Step 6）
2. **计算具体坐标值**：把组件的 left/top/width/height 代入锚点公式
3. **选择正确的路径类型**：直达用 `<line>`，L型用 `<path>`
4. **拐点放在空白区域**：通常是组件之间或中间层（Y=240px）

#### 图例对应规则

**重要：图例必须与实际连线颜色和标签完全对应！**

1. **只显示实际使用的连线类型**
2. **颜色必须匹配**：图例颜色 = 实际连线 stroke 颜色
3. **标签适配架构类型**：

| 连线颜色 | 通用标签 | Vue/React | 微服务 | 后端分层 |
|---------|---------|-----------|--------|---------|
| #94a3b8 (灰) | 主流程 | 页面跳转 | 请求流 | API调用 |
| #059669 (绿) | 数据流 | 状态流 | 数据访问 | 数据流 |
| #2563eb (蓝) | 依赖 | 组件引用 | 服务调用 | 模块依赖 |
| #0d9488 (青) | 配置 | Store | 服务发现 | 配置 |
| #e11d48 (红) | 控制 | 路由守卫 | 流量治理 | 权限 |
| #7c3aed (紫) | 事件 | EventEmitter | 消息 | 异步事件 |

#### 响应式缩放脚本

```javascript
function resize() {
  const scale = Math.min(wrapper.clientWidth / 1440, 1);
  canvas.style.transform = `scale(${scale})`;
  wrapper.style.height = (canvasHeight * scale) + 'px';
}
```

完整模板见 `references/html-template.md`，**必须完整复制其 `<style>` 部分**。

### Step 9: Post-generation Verification

```
## Post-generation Verification

Open HTML in browser and check:
[ ] All components visible, no occlusion
[ ] All group labels fully displayed
[ ] Connections clear, no breaks
[ ] No text overflow
[ ] Right info area fully displayed
[ ] Legend fully displayed
[ ] Responsive scaling works
```

## Architecture Type Adaptation

| Architecture Type | Layer Suggestion | Core Components |
|-------------------|------------------|-----------------|
| Frontend (Vue/React) | Entry, Router, Pages, Components, State, Utils | Router, Views, Components, Store(Pinia/Redux), API |
| Microservice | Gateway, Service, Governance, Infrastructure | Gateway, Service Cluster, Registry, Config |
| Business System | Access, Application, Domain, Data | API, App Service, Domain Service, Database |
| Backend Layered | Presentation, Control, Service, Persistence | Controller, Service, DAO, Database |
| Deployment | Access, Compute, Storage | LB, Server Cluster, DB Cluster |
| Data Pipeline | Collection, Processing, Storage, Application | Data Source, Compute Engine, Storage, BI |

## Quick Reference

| Resource | Purpose |
|----------|---------|
| `references/html-template.md` | **完整 HTML 模板（必须使用）** |
| `references/style-guide.md` | Color system, component sizes, style rules |
| `references/layout-rules.md` | Coordinate constraints, spacing, collision rules |
| `examples/vue-frontend-example.md` | Vue 3 frontend architecture example |
| `examples/microservice-example.md` | Spring Cloud microservice architecture example |

## Key Principles

1. **Plan first, code second** - Always output coordinate tables before HTML
2. **Verify collisions** - Calculate right/bottom values and check gaps
3. **Use standard sizes** - Follow size templates for consistency
4. **Keep connections clean** - Use defined anchor points and path types
5. **Reserve space** - Always reserve 260px for right info area
