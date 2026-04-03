---
name: Architecture Diagram
description: This skill should be used when the user asks to "draw an architecture diagram", "create architecture diagram", "generate architecture", "画架构图", "生成架构图", "绘制架构图", or mentions architecture, microservice architecture, system architecture, deployment architecture, technology architecture, or needs to visualize system structure with components and connections.
version: 1.0.0
author: 李红彦
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

Use standardized colors, sizes, and component types defined in `references/style-guide.md`.

### Step 5: Apply Layout Constraints

Follow coordinate, spacing, and capacity rules defined in `references/layout-rules.md`.

### Step 6: Apply Connection Rules

Use anchor points and path types defined in `references/layout-rules.md`.

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

**重要：必须基于 `references/html-template.md` 中的完整模板生成 HTML**

要求：
1. 完整复制模板中的 `<style>` 部分，不得省略
2. 使用模板中定义的 CSS 类：`.box`, `.source`, `.dashed`, `.group`, `.tag` 等
3. 连线颜色必须使用模板中定义的颜色对应关系
4. 组件边框颜色必须使用模板中定义的颜色对应关系

Output complete HTML file after passing all checks.

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
| Microservice | Gateway, Service, Governance, Infrastructure | Gateway, Service Cluster, Registry, Config |
| Business System | Access, Application, Domain, Data | API, App Service, Domain Service, Database |
| Technology | Presentation, Control, Service, Persistence | Frontend, Controller, Service, DAO |
| Deployment | Access, Compute, Storage | LB, Server Cluster, DB Cluster |
| Data | Collection, Processing, Storage, Application | Data Source, Compute Engine, Storage, BI |

## Quick Reference

| Resource | Purpose |
|----------|---------|
| `references/html-template.md` | **完整 HTML 模板（必须使用）** |
| `references/style-guide.md` | Color system, component sizes, style rules |
| `references/layout-rules.md` | Coordinate constraints, spacing, collision rules |
| `examples/microservice-example.md` | Complete microservice architecture example |

## Key Principles

1. **Plan first, code second** - Always output coordinate tables before HTML
2. **Verify collisions** - Calculate right/bottom values and check gaps
3. **Use standard sizes** - Follow size templates for consistency
4. **Keep connections clean** - Use defined anchor points and path types
5. **Reserve space** - Always reserve 260px for right info area
