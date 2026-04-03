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

#### 连线绕行规则（禁止穿过组件！）

**连线必须绕过所有组件，不能穿过任何 box！**

当 A→C 的直线路径会穿过 B 时，必须使用折线绕行：

```
❌ 错误：直线穿过 B
   [A] ────────→ [B] ────────→ [C]  （连线穿过 B 内部）

✅ 正确：绕过 B
   [A] ──→ 拐点1 ──→ 拐点2 ──→ [C]
                    ↓
                   [B]          （连线从 B 上方/下方绕过）
```

**绕行策略**：
1. **上方绕行**：从 A 上方经过，适合 A、C 都在 B 上方或同层
2. **下方绕行**：从 B 下方经过，适合 A、C 在 B 下方
3. **侧边绕行**：从 B 左侧或右侧绕过，适合垂直方向的连接

**绕行间距**：连线与组件边缘至少保持 20px 距离

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
[ ] 分组底部内边距 >= 16px
[ ] 最大 right < 1140px
[ ] 连线最小长度 >= 40px
[ ] 无斜线（x1=x2 或 y1=y2）
[ ] 连线不穿过任何组件
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
- 位置：标题右侧（top:28px, right:40px）
- 必须添加 `pointer-events:none` 避免阻挡连线悬停
- 只显示实际使用的连线类型
- 颜色必须与连线 stroke 颜色一致
- 标签根据架构类型适配

### Step 8: Post-generation Verification

#### 基础检查
```
[ ] 所有组件可见，无遮挡
[ ] 连线清晰，无断裂
[ ] 图例与连线颜色一致
[ ] 响应式缩放正常
```

#### 样式检查

**box 样式完整性**：
```
[ ] 所有 box 都有 position:absolute
[ ] 所有 box 都有 border、border-radius、background
[ ] 所有 box 都有 z-index:5
[ ] 分组 box 都有 z-index:1
```

**标题分割线**：
```
[ ] 标题下方存在灰色分隔线（height:1px, background:#e2e8f0）
[ ] 分隔线位置：top:60px, left:40px, right:40px
```

**连线的 box 间距**：
```
[ ] 有连线关系的两个 box 水平间距 >= 40px
[ ] 有连线关系的两个 box 垂直间距 >= 40px
[ ] 间距过短会导致连线只剩箭头，不美观
```

**间距计算示例**：
```
A.right = A.left + A.width
B.left = B.left
间距 = B.left - A.right

❌ 错误：间距 10px
   A(right=200) → B(left=210)，连线长度只有 10px

✅ 正确：间距 40px 以上
   A(right=200) → B(left=240)，连线长度 40px
```

#### 文字宽度检查（防止换行）

**问题**：文字过长会导致换行，破坏布局

**检查方法**：
```
估算文字宽度 = 字符数 × 8px（中文）或 字符数 × 6px（英文）
box 宽度必须 > 文字宽度 + 24px（左右内边距各12px）

示例：
- "Vue Router" = 10字符 × 6px = 60px，box宽度 >= 84px ✓
- "Service Provider" = 16字符 × 6px = 96px，box宽度 >= 120px ✓
- "客户端请求" = 5字符 × 8px = 40px，box宽度 >= 64px ✓
```

**自检项**：
```
[ ] 所有 box 内文字单行显示，无换行
[ ] 英文单词 box 宽度 >= 字符数 × 6 + 24
[ ] 中文文字 box 宽度 >= 字符数 × 8 + 24
```

#### 锚点坐标验证（防止连线错位）

**问题**：模型计算错误会导致连线指向错误位置

**验证公式**：

| 锚点类型 | X 坐标 | Y 坐标 |
|---------|-------|-------|
| 右中 | left + width | top + height/2 |
| 左中 | left | top + height/2 |
| 下中 | left + width/2 | top + height |
| 上中 | left + width/2 | top |

**常见错误**：
```
❌ Y坐标用成 left 值：<line x1="164" y1="66" /> ← y1 应该是 136
❌ 忘记加 width：<line x1="66" /> ← 应该是 x1=66+width
❌ 垂直连线 x 不一致：<line x1="160" x2="155" /> ← x1≠x2 变成斜线
```

**自检项**：
```
[ ] 所有连线起点锚点在组件边缘（不是中心）
[ ] 所有连线终点锚点在组件边缘（不是中心）
[ ] 水平连线 y1 = y2
[ ] 垂直连线 x1 = x2
[ ] 锚点坐标通过公式计算验证
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
