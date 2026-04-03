# Layout Rules

## Coordinate System

| 属性 | 值 | 说明 |
|------|-----|------|
| 画布宽度 | 1440px | 固定宽度 |
| 画布高度 | 900-1200px | 根据内容调整 |
| 基准单位 | 10px | 所有坐标必须是 10 的倍数 |
| 安全边距 | 40px | 四边 |

## Area Division

### 水平分区
| 列 | 范围 | 用途 |
|----|------|------|
| Col 1 | 50-200px | 入口 |
| Col 2 | 220-400px | 网关 |
| Col 3 | 420-620px | 核心服务 |
| Col 4 | 640-800px | 存储/外部 |
| Col 5 | 820-1000px | 配置/治理 |
| 右侧 | right:40px, 260px | 信息卡片 |

### 垂直分区
| 行 | 范围 | 用途 |
|----|------|------|
| Row 0 | 20-80px | 标题区 |
| Row 1 | 90-200px | 第一层组件 |
| Row 2 | 200-280px | 连接通道 |
| Row 3 | 280-500px | 第二层组件 |
| Row 4 | 510-620px | 基础设施层 |

## Spacing

| 关系 | 最小间距 |
|------|---------|
| 同行相邻分组 | 30px |
| 同行相邻组件 | 20px |
| 分组内组件垂直间距 | 8px |
| 组件到分组边框（左右） | 16px |
| 组件到分组边框（上） | 26px（含标签空间） |
| 组件到分组边框（下） | 16px |
| 信息卡片垂直间距 | 20px |

## Group Internal Layout

```
分组宽度 = max(组件宽度) + 32px（左右内边距）
分组高度 = 组件高度之和 + (组件数-1)×8px + 42px（上下内边距+标签）

组件定位：
  Component.left = Group.left + 16
  Component[0].top = Group.top + 26
  Component[1].top = Component[0].bottom + 8
  ...
```

## Collision Detection

```
right = left + width
bottom = top + height

避免碰撞：A.right < B.left OR A.left > B.right
         A.bottom < B.top OR A.top > B.bottom
```

**边界检查：** 最大 group.right < 1140px（预留右侧信息区）

## Connection Rules

### 锚点（连线必须连接到组件边缘，不能是中心）

```
左中: (left, top + height/2)           -- 组件左边框中点
右中: (left + width, top + height/2)   -- 组件右边框中点
上中: (left + width/2, top)            -- 组件上边框中点
下中: (left + width/2, top + height)   -- 组件下边框中点
```

**示例**：组件 A (left=100, top=50, width=120, height=40)
- 右中锚点坐标：(220, 70) = (100+120, 50+20)
- 下中锚点坐标：(160, 90) = (100+60, 50+40)

### 路径类型

| 类型 | 用法 | 起点锚点 | 终点锚点 |
|------|------|---------|---------|
| 水平直达 | A在B左边，同行 | A右中 | B左中 |
| 垂直直达 | A在B上边，同列 | A下中 | B上中 |
| L型路径 | 跨行跨列 | A右中/下中 | B左中/上中 |

### 连线约束
- 起点/终点必须是组件边缘锚点，不能是组件中心
- 拐点坐标必须是 10 的倍数
- 连线不能穿过组件内部
- 平行连线间距 >= 10px
- 拐点必须放在空白区域

## Legend Rules

- 只显示实际使用的连线类型
- 颜色必须与连线 stroke 一致
- 标签根据架构类型适配（见 style-guide.md）
