# Style Guide

## Color System

### Background Colors

```css
:root {
  /* Page & Canvas */
  --bg-page: #f3f4f6;        /* Page background */
  --bg-canvas: #ffffff;       /* Canvas background */

  /* Component Backgrounds */
  --bg-box: #ffffff;          /* Normal component */
  --bg-source: #f0fdfa;       /* Data source / External system */
  --bg-dashed: #f8fafb;       /* Dashed/sub component */
  --bg-highlight: #fffbeb;    /* Highlighted / Experimental */
  --bg-infra: #f0fdf4;        /* Infrastructure layer */
}
```

### Border Colors

| Type | Color | Hex | Usage |
|------|-------|-----|-------|
| Normal | Gray | #d1d5db | Default component border |
| Source | Teal | #5eead4 | Data source, external system |
| Highlight | Orange | #f59e0b | Important, experimental |
| Infrastructure | Green | #a7f3d0 | Infrastructure components |

### Text Colors

| Type | Color | Hex | Usage |
|------|-------|-----|-------|
| Primary | Slate 800 | #1e293b | Main text, titles |
| Secondary | Slate 500 | #64748b | Subtitles, descriptions |
| Muted | Slate 400 | #94a3b8 | Tech notes, hints |

### Tag Colors (Group Labels)

| Color | Hex | Usage Scenarios |
|-------|-----|-----------------|
| teal | #0d9488 | Registry, Config, Core infrastructure |
| blue | #2563eb | Gateway, Network, Routing |
| green | #059669 | Service cluster, Business modules |
| red | #e11d48 | Traffic control, Circuit breaker, Security |
| orange | #d97706 | Transaction, Scheduling |
| violet | #7c3aed | Message queue, Event |
| cyan | #0891b2 | Monitoring, Logging |
| slate | #64748b | Tools, Utilities |

### Connection Line Colors

| Color | Hex | Usage Scenarios |
|-------|-----|-----------------|
| Gray | #94a3b8 | Main request flow, Default calls |
| Teal | #0d9488 | Service discovery, Registration |
| Green | #059669 | Config sync, State |
| Blue | #2563eb | Service-to-service calls |
| Red | #e11d48 | Governance, Rate limiting, Circuit breaker |
| Orange | #d97706 | Transaction |
| Violet | #7c3aed | Message, Event |

---

## Component Types

### Type 1: Normal Component

**Visual:** Solid border, white background

**Usage:** Core functional modules

```html
<div class="box" style="left:246px; top:116px; width:128px; height:40px; border-color:#3b82f6">
  Component Name<span class="sub">Description</span>
</div>
```

### Type 2: Source Component

**Visual:** Teal tinted background, teal border

**Usage:** External systems, databases, APIs

```html
<div class="box source" style="left:50px; top:116px; width:145px; height:48px">
  MySQL<span class="tech">Database</span>
</div>
```

### Type 3: Dashed Component

**Visual:** Dashed border, light gray background

**Usage:** Sub-functions, internal components

```html
<div class="box dashed" style="left:246px; top:160px; width:128px; height:32px">
  SubComponent
</div>
```

### Type 4: Highlighted Component

**Visual:** Orange tinted background, orange border

**Usage:** Important, experimental, or flagged features

```html
<div class="box" style="border-color:#f59e0b; background:#fffbeb">
  Feature<span class="sub">Experimental</span>
</div>
```

### Type 5: Infrastructure Component

**Visual:** Green tinted background, green border

**Usage:** Infrastructure layer components

```html
<div class="box" style="border-color:#a7f3d0; background:#f0fdf4">
  Kubernetes<span class="sub">Container Orchestration</span>
</div>
```

---

## Component Sizes

### Size Templates

| Type | Width | Height | Usage |
|------|-------|--------|-------|
| Small | 100-120px | 32-36px | Sub-functions, labels |
| Standard | 130-160px | 40-48px | Core modules |
| Large | 170-220px | 50-60px | Main services |
| Info Card | 260px | Auto | Right side info cards |

### Text Sizes

| Element | Size | Weight | Class |
|---------|------|--------|-------|
| Main title | 17px | 700 | - |
| Component name | 13px | 600 | - |
| Subtitle | 10px | 400 | `.sub` |
| Tech note | 9.5px | 400 | `.tech` |
| Tag label | 9.5px | 700 | `.tag` |

---

## Group (Container) Styles

### Basic Group

```css
.group {
  border: 1.5px dashed #cbd5e1;
  border-radius: 14px;
  background: rgba(248, 250, 252, 0.6);
}
```

### Tag Positioning

```css
.tag {
  position: absolute;
  top: -10px;
  left: 14px;
  font-size: 9.5px;
  font-weight: 700;
  padding: 2px 10px;
  border-radius: 10px;
  letter-spacing: 0.6px;
  text-transform: uppercase;
  color: #ffffff;
}
```

---

## Enclosure Styles

### Infrastructure Enclosure

```css
.enclosure {
  border: 1px solid #e2e8f0;
  border-radius: 18px;
  background: rgba(236, 253, 245, 0.3);
}
```

### Label Positioning

```css
.enclosure-label {
  position: absolute;
  color: #059669;
  font-size: 9px;
  font-weight: 500;
  letter-spacing: 0.4px;
  font-family: 'SF Mono', 'Menlo', monospace;
}
```

---

## Info Card Styles

```html
<div style="position:absolute; right:40px; top:90px; width:260px">
  <div style="border:1px solid #e2e8f0; border-radius:10px; padding:12px 14px; background:rgba(248,250,252,0.8)">
    <div style="font-size:10px; font-weight:700; color:#1e293b; margin-bottom:6px">Card Title</div>
    <div style="font-size:9.5px; color:#64748b; line-height:1.6">
      Content here...
    </div>
  </div>
</div>
```

---

## Legend Styles

```css
.legend {
  position: absolute;
  display: flex;
  gap: 16px;
}

.legend-item {
  display: flex;
  align-items: center;
  gap: 6px;
  color: #94a3b8;
  font-size: 10px;
}

.legend-line {
  width: 24px;
  height: 0;
}
```

---

## Font Stack

```css
font-family: -apple-system, BlinkMacSystemFont, 'Inter', 'Segoe UI', 'PingFang SC', 'Microsoft YaHei', sans-serif;
```

For code/monospace:

```css
font-family: 'SF Mono', 'Menlo', monospace;
```
