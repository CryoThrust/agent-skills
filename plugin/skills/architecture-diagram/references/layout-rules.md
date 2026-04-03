# Layout Rules

## Coordinate System

### Canvas Specification

| Property | Value | Description |
|----------|-------|-------------|
| Width | 1440px | Fixed canvas width |
| Height | 900-1200px | Adjustable based on content |
| Origin | (0, 0) | Top-left corner |
| Safe margin | 40px | All sides |

### Grid System

- Base unit: **10px**
- All coordinates must be multiples of 10
- Recommended values: 40, 50, 60, 80, 100, 120, 140, 160, 180, 200...

---

## Area Division

### Horizontal Division (Columns)

| Column | Left Range | Purpose |
|--------|------------|---------|
| Col 1 | 50-200px | Entry point (Client, User) |
| Col 2 | 220-400px | Gateway, Front-door |
| Col 3 | 420-620px | Core services |
| Col 4 | 640-800px | Storage, External |
| Col 5 | 820-1000px | Config, Governance |
| Right | right:40px, width:260px | Info cards |

### Vertical Division (Rows)

| Row | Top Range | Purpose |
|-----|-----------|---------|
| Row 0 | 20-80px | Title area |
| Row 1 | 90-200px | Layer 1 components |
| Row 2 | 200-280px | Spacing / Connection channel |
| Row 3 | 280-500px | Layer 2 components |
| Row 4 | 510-620px | Infrastructure layer |
| Row 5 | bottom 20-50px | Legend area |

---

## Spacing Constraints

### Minimum Spacing

| Relationship | Minimum Gap |
|--------------|-------------|
| Same-row adjacent groups | 30px |
| Same-row adjacent components | 20px |
| Component to group border | 16px |
| Group to group (different rows) | 20px |
| Component to canvas edge | 40px |
| Info cards vertical gap | 20px |

### Group Internal Padding

| Side | Padding |
|------|---------|
| Top (with tag) | 26px |
| Top (no tag) | 16px |
| Bottom | 16px |
| Left | 16px |
| Right | 16px |

---

## Group Size Constraints

### Minimum Dimensions

- Minimum width: 150px
- Minimum height: 100px

### Size Calculation

```
Group width >= max(component widths) + 32px (left/right padding)
Group height = sum(component heights) + (component_count - 1) × 8px + 42px (top/bottom padding + tag)
```

### Capacity Constraint

```
Total width of same-row groups = Σ(group widths) + (group_count - 1) × 30px
Constraint: Total width <= Canvas width - 80px - Right info area width
Constraint: Total width <= 1100px
```

---

## Collision Detection

### Absolute Prohibition

For any two elements A and B:

```
Prohibited: A.left < B.right AND A.right > B.left
            AND A.top < B.bottom AND A.bottom > B.top
```

### Detection Formula

Calculate for each element:
- `right = left + width`
- `bottom = top + height`

Check pairs:
- Horizontal: `A.right < B.left` OR `A.left > B.right`
- Vertical: `A.bottom < B.top` OR `A.top > B.bottom`

At least one must be true to avoid collision.

---

## Component Placement Rules

### Within Group

```
Component.left = Group.left + 16
Component.top = Group.top + 26 (space for tag)

For vertical stacking:
  Component[0].top = Group.top + 26
  Component[1].top = Component[0].bottom + 8
  Component[2].top = Component[1].bottom + 8
  ...
```

### Boundary Check

```
Component.left >= Group.left + 16
Component.right <= Group.right - 16
Component.top >= Group.top + 26
Component.bottom <= Group.bottom - 16
```

---

## Connection Rules

### Anchor Points

Each component has 4 anchor points:

```
Left-center:   (left, top + height/2)
Right-center:  (right, top + height/2)
Top-center:    (left + width/2, top)
Bottom-center: (left + width/2, bottom)
```

### Path Types

#### Type A: Horizontal Direct

```
Start.right-center → End.left-center

Usage: Same-row adjacent components

<line x1="start_right" y1="start_y" x2="end_left" y2="end_y" />
```

#### Type B: Vertical Direct

```
Start.bottom-center → End.top-center

Usage: Same-column components in different rows

<line x1="start_x" y1="start_bottom" x2="end_x" y2="end_top" />
```

#### Type C: L-Shape Path

```
Start.right-center → (拐点X, Start.Y) → (拐点X, End.Y) → End.left-center

Usage: Cross-row, cross-column connections

拐点X should be in blank area (usually between columns)
```

#### Type D: Middle Layer Path

```
Start.bottom-center → (Start.X, MiddleY) → (End.X, MiddleY) → End.top-center

Usage: Multi-layer component connections
MiddleY = 200-280px (spacing channel)
```

### Connection Constraints

- 拐点 coordinates must be multiples of 10
- Connections must not pass through any component interior
- Parallel connections must have gap >= 10px
- Use dashed lines for secondary connections (discovery, config, etc.)

---

## Right Info Area Rules

### Positioning

```
Position: absolute
Right: 40px
Width: 260px
Top range: 90px to (canvas height - 100px)
```

### Content Layout

- Each info card: width 260px
- Vertical gap between cards: 20px
- Cards don't overlap with main area groups

### Boundary Check

```
Main area max right = max(all groups.right)
Info area left = Canvas width - 40 - 260 = 1140px
Constraint: Main area max right < 1140px
```

---

## Legend Rules

### Positioning

```
Position: absolute
Bottom: 20px
Right: 40px
```

### Content

Include legend items for all connection types used. Use universal semantic names:

| Semantic Type | Style | Example Use Cases |
|---------------|-------|-------------------|
| Main Flow / Request | Solid gray | User request, Page navigation, Component call |
| Config / Metadata | Dashed teal | Config, Service discovery, Store, Environment |
| Data / State | Dashed green | Database, Cache, State management, Props |
| Dependency / Reference | Dashed blue | Module import, Service call, Component ref |
| Control / Guard | Dashed red | Auth, Permission, Route guard, Validation |
| Coordination | Dashed orange | Transaction, State sync, Lifecycle |
| Event / Message | Dashed violet | Event bus, Message queue, Pub/Sub |

**Adapt legend labels to architecture context:**
- Vue/React: "Component call", "State flow", "Event"
- Microservice: "Request flow", "Service call", "Message"
- Backend: "API call", "Data flow", "Async event"
- Deployment: "Traffic", "Data sync", "Alert"

---

## Responsive Scaling

```javascript
function resize() {
  const wrapper = document.querySelector('.wrapper');
  const canvas = document.querySelector('.canvas');
  const scale = Math.min(wrapper.clientWidth / 1440, 1);
  canvas.style.transform = `scale(${scale})`;
  wrapper.style.height = (canvasHeight * scale) + 'px';
}
window.addEventListener('resize', resize);
resize();
```
