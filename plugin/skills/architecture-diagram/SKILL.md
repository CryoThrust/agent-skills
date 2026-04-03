---
name: architecture-diagram
description: This skill should be used when the user asks to "draw an architecture diagram", "create architecture diagram", "generate architecture", "画架构图", "生成架构图", "绘制架构图", or mentions architecture, microservice architecture, frontend architecture (Vue/React), system architecture, deployment architecture, technology architecture, or needs to visualize system structure with components and connections.
version: 1.0.0
author: lihongyan
email: Y0hanes@Outlook.com
---

# Architecture Diagram Generator

Generate professional HTML architecture diagrams with collision-free layout, clear connections, and unified styling.

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

| Group Pair | Result |
|------------|--------|
| G1 vs G2 | G1.right(390) < G2.left(420) OK, gap=30px |

Max group right must < 1140px (for right info area)
```

### Step 4: Apply Style Rules

**详细样式规范见 `references/style-guide.md`**

核心规则：
- 组件尺寸：小型 100-120px，标准 130-160px，大型 170-220px
- 坐标必须是 10 的倍数
- 右侧预留 260px 给信息卡片

### Step 5: Connection Rules

**详细布局规则见 `references/layout-rules.md`**

#### 锚点计算（连线必须连接到组件边缘，不能是中心）

| 锚点 | X 坐标 | Y 坐标 | 说明 |
|------|--------|--------|------|
| 左中 | left | top + height/2 | 组件左边框中点 |
| 右中 | right (=left+width) | top + height/2 | 组件右边框中点 |
| 上中 | left + width/2 | top | 组件上边框中点 |
| 下中 | left + width/2 | bottom (=top+height) | 组件下边框中点 |

**示例**：组件 left=100, top=50, width=120, height=40
- 右中锚点：(100+120=220, 50+40/2=70)
- 下中锚点：(100+120/2=160, 50+40=90)

#### 路径规则（禁止斜线！）

**只能使用水平线 + 垂直线，不能有斜线！**

| 场景 | 起点 | 终点 | 路径 |
|------|------|------|------|
| A在B左边，同行 | A右中 | B左中 | 水平直线 |
| A在B上边，同列 | A下中 | B上中 | 垂直直线 |
| 跨行跨列 | A右中/下中 | B左中/上中 | 折线（水平→垂直→水平） |

**折线示例**：
```
A右中 → 向右水平延伸 → 拐点1 → 垂直向下/向上 → 拐点2 → 水平向左 → B左中
```

#### 连线最小长度

**连线必须有足够的线段长度显示虚线效果！**
- 最小线段长度：40px（确保至少显示 2-3 个虚线周期）
- 如果组件间距 < 40px，需要调整组件位置或使用折线绕行

**禁止**：`<line x1="100" y1="50" x2="105" y2="55" />` （斜线 + 太短）

**正确**：
```html
<!-- 水平直线，长度足够 -->
<line x1="100" y1="50" x2="160" y2="50" ... />

<!-- 折线：水平 → 垂直 → 水平 -->
<path d="M 100 50 L 140 50 L 140 100 L 200 100" ... />
```

#### 连线颜色（根据语义选择）
| 语义 | 颜色 | 色值 |
|------|------|------|
| 主流程 | 灰色 | #94a3b8 |
| 数据流 | 绿色 | #059669 |
| 依赖 | 蓝色 | #2563eb |
| 配置 | 青色 | #0d9488 |
| 控制 | 红色 | #e11d48 |
| 事件 | 紫色 | #7c3aed |

### Step 6: Pre-generation Checklist

```
[ ] 坐标是 10 的倍数
[ ] 同行组件间距 >= 30px
[ ] 组件不超出分组边界
[ ] 最大 right < 1140px
[ ] 拐点在空白区域
```

### Step 7: Generate HTML

**必须使用 `references/html-template.md` 中的完整模板**

#### 核心 CSS 类
```css
.box / .box.source / .box.dashed  /* 组件 */
.group / .tag.teal/.blue/.green/.red/.violet  /* 分组 */
.enclosure  /* 基础设施层 */
.legend  /* 图例 */
.connection  /* 连线悬停高亮 */
.flow  /* 连线流动动画 */
```

#### 连线生成
```html
<!-- 水平连线：A右中 → B左中 -->
<line x1="A.left+A.width" y1="A.top+A.height/2"
      x2="B.left" y2="B.top+B.height/2"
      stroke="#94a3b8" stroke-width="1.5" marker-end="url(#ah)"
      class="connection flow" />

<!-- 垂直连线：A下中 → B上中（注意：终点Y是B.top，不是B.left） -->
<line x1="A.left+A.width/2" y1="A.top+A.height"
      x2="B.left+B.width/2" y2="B.top"
      stroke="#94a3b8" stroke-width="1.5" marker-end="url(#ah)"
      class="connection flow" />
```
- `connection`：悬停时加粗 + 发光高亮
- `flow`：2秒平滑流动动画
- **关键**：垂直连线终点是 `B.top`（上边框），x坐标是 `B.left+B.width/2`（水平中点）

#### 层级关系
group(z-index:1) < svg(z-index:3) < box(z-index:5)

#### 图例规则
- 只显示实际使用的连线类型
- 颜色必须与连线 stroke 颜色一致
- 标签根据架构类型适配

### Step 8: Post-generation Verification

```
[ ] 所有组件可见，无遮挡
[ ] 连线清晰，无断裂
[ ] 图例与连线颜色一致
[ ] 响应式缩放正常
```

## Architecture Type Adaptation

| Architecture Type | Layer Suggestion | Core Components |
|-------------------|------------------|-----------------|
| Frontend (Vue/React) | Entry, Router, Pages, Components, State, Utils | Router, Views, Components, Store, API |
| Microservice | Gateway, Service, Governance, Infrastructure | Gateway, Service Cluster, Registry, Config |
| Backend Layered | Presentation, Control, Service, Persistence | Controller, Service, DAO, Database |
| Deployment | Access, Compute, Storage | LB, Server Cluster, DB Cluster |

## Quick Reference

| Resource | Purpose |
|----------|---------|
| `references/html-template.md` | 完整 HTML 模板 |
| `references/style-guide.md` | 颜色系统、组件尺寸 |
| `references/layout-rules.md` | 坐标约束、碰撞检测 |
| `examples/vue-frontend-example.md` | Vue 3 前端架构示例 |
| `examples/microservice-example.md` | 微服务架构示例 |

## Key Principles

1. **先规划后编码** - 输出坐标表再写 HTML
2. **验证碰撞** - 计算 right/bottom 并检查间距
3. **使用标准尺寸** - 遵循尺寸模板
4. **保持连线整洁** - 使用定义的锚点和路径类型
5. **预留空间** - 右侧预留 260px 信息区
