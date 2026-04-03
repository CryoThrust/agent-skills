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
- 信息卡片为可选元素，按需添加

### Step 5: Connection Rules

**详细布局规则见 `references/layout-rules.md`**

#### 锚点计算

| 锚点 | X 坐标 | Y 坐标 |
|------|--------|--------|
| 左中 | left | top + height/2 |
| 右中 | left + width | top + height/2 |
| 上中 | left + width/2 | top |
| 下中 | left + width/2 | top + height |

#### 核心规则

1. **禁止斜线**：只能用水平线(x1=x2)或垂直线(y1=y2)
2. **禁止穿过组件**：连线必须绕过所有 box
3. **最小长度 40px**：确保虚线效果可见
4. **连线颜色**：主流程#94a3b8、数据流#059669、依赖#2563eb、配置#0d9488、控制#e11d48、事件#7c3aed

### Step 6: Pre-generation Checklist

```
[ ] 坐标是 10 的倍数
[ ] 同行分组间距 >= 30px
[ ] 分组底部内边距 >= 16px（Group.bottom - 最后组件.bottom >= 16）
[ ] 分组垂直间距 >= 30px（避免卡片挨在一起）
[ ] 最大 group.right < 1360px（有信息卡片时 < 1140px）
[ ] 连线最小长度 >= 40px
[ ] 无斜线（x1=x2 或 y1=y2）
[ ] 连线不穿过任何组件
[ ] 拐点在空白区域
```

**分组高度验证公式**：
```
每个分组都要验证：
  底部内边距 = Group.bottom - 最后组件.bottom
  ✅ 必须 >= 16px
```

### Step 7: Generate HTML

**必须使用 `references/html-template.md` 中的完整模板**

#### 核心 CSS 类
```css
.box / .box.source / .box.dashed  /* 组件 */
.group / .tag.teal/.blue/.green/.red/.violet  /* 分组 */
.enclosure  /* 基础设施层 */
.legend  /* 图例（与标题同高） */
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

#### 连线文字标签（可选）

**核心原则：文字必须紧贴连线，禁止漂移！**

```html
<!-- 水平连线：文字在连线上方，x取中点 -->
<text x="(x1+x2)/2" y="y1-5" text-anchor="middle" class="connection-label">HTTP</text>

<!-- 垂直连线：文字在连线右侧，y取中点 -->
<text x="x1+8" y="(y1+y2)/2" dominant-baseline="middle" class="connection-label">data</text>
```

**文字位置规则（优先级从高到低）**：

| 优先级 | 规则 | 位置 | 偏移量 |
|--------|------|------|--------|
| 1 | 水平连线，上方无遮挡 | 连线上方 | y = y1 - 5 |
| 2 | 水平连线，上方有组件 | 连线下方 | y = y1 + 12 |
| 3 | 垂直连线，右侧无遮挡 | 连线右侧 | x = x1 + 8 |
| 4 | 垂直连线，右侧有组件 | 连线左侧 | x = x1 - 8, text-anchor:end |

**文字重叠处理**：

当多条连线距离 < 30px 时，采用错位策略：

```
连线A: y = y1 - 5  (上移5px)
连线B: y = y1 + 12 (下移12px，避免重叠)

或水平错位：
连线A: x = mid - 20 (左偏)
连线B: x = mid + 20 (右偏)
```

**禁止事项**：
- ❌ 文字与连线距离 > 15px（禁止漂移）
- ❌ 文字遮挡组件或其它文字
- ❌ 文字内容过长（最多 8 个字符）

- 文字内容简短（建议 2-8 个字符）
- 必须使用 `text-anchor` 或 `dominant-baseline` 确保对齐

#### 层级关系
group(z-index:1) < svg(z-index:3) < box(z-index:5)

#### 图例规则
- 位置：与标题同高（top 与标题一致，默认 top:22px），横向单行排列
- 必须添加 `pointer-events:none` 避免阻挡连线悬停
- 显示所有连线类型（主流程/数据流/依赖/控制/事件），至少 5 类
- 颜色必须与连线 stroke 颜色一致
- 标签根据架构类型适配

### Step 8: Canvas Sizing

**画布尺寸根据内容自适应**：

| 内容规模 | 宽度 | 高度 | 判断标准 |
|----------|------|------|----------|
| 小型架构 | 1440px | 700-800px | 单层，<5 个分组 |
| 标准架构 | 1440px | 900px | 2-3 层，5-10 个分组 |
| 大型架构 | 1440px | 1000-1200px | 多层，>10 个分组 |

**高度计算公式**：

```
画布高度 = max(分组.bottom) + 80px

示例：
- 最底部分组 bottom = 620px
- 画布高度 = 620 + 80 = 700px（向上取整到百位）
```

**宽度规则**：
- 默认宽度：1440px
- 无信息卡片：最大 group.right < 1360px
- 有信息卡片：最大 group.right < 1140px，右侧预留 260px

### Step 9: Post-generation Verification

```
[ ] 所有组件可见，无遮挡
[ ] 连线清晰，无断裂
[ ] 图例与连线颜色一致
[ ] 图例与标题同高，横向单行排列（至少 5 类连线）
[ ] 标题下方有灰色分隔线
[ ] 所有 box 有 position:absolute, z-index:5
[ ] 分组间距 >= 30px（水平和垂直）
[ ] 分组底部内边距 >= 16px（验证：Group.bottom - 最后组件.bottom）
[ ] 连线 box 间距 >= 40px
[ ] 文字不换行（英文:字符数×6+24px，中文:字符数×8+24px）
[ ] 水平连线 y1=y2，垂直连线 x1=x2
[ ] 锚点在组件边缘（通过公式验证）
[ ] 连线文字紧贴连线，无漂移（距离 < 15px）
[ ] 连线文字无重叠
[ ] 画布高度适配内容（底部留白 >= 60px）
```

**关键验证**：
- **分组底部内边距**：逐个检查 `Group.bottom - 最后组件.bottom >= 16px`
- **连线文字位置**：检查 `|文字坐标 - 连线坐标| <= 15px`

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
