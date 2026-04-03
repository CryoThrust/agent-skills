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

### 锚点
```
左中: (left, top + height/2)
右中: (right, top + height/2)
上中: (left + width/2, top)
下中: (left + width/2, bottom)
```

### 路径类型

| 类型 | 用法 | 代码 |
|------|------|------|
| 水平直达 | 同行相邻 | `<line x1="起点右中" y1="Y" x2="终点左中" y2="Y" />` |
| 垂直直达 | 同列不同行 | `<line x1="X" y1="起点下中" x2="X" y2="终点上中" />` |
| L型路径 | 跨行跨列 | `<path d="M 起点 L 拐点 L 终点" />` |

### 连线约束
- 拐点坐标必须是 10 的倍数
- 连线不能穿过组件内部
- 平行连线间距 >= 10px
- 拐点必须放在空白区域

## Legend Rules

- 只显示实际使用的连线类型
- 颜色必须与连线 stroke 一致
- 标签根据架构类型适配（见 style-guide.md）
