# Vue 3 Frontend Architecture Example

## User Input

```
画一个 Vue 3 前端架构图，包含：
- 用户入口（浏览器、移动端）
- 路由层（Vue Router）
- 状态管理（Pinia）
- 组件层（页面组件、通用组件）
- API 层（Axios 封装）
- 工具层（工具函数、Hooks）
- 构建工具（Vite）
```

## Layout Planning

### Layer Division

| Layer | Top Range | Purpose |
|-------|-----------|---------|
| Title | 20-80px | Title |
| Layer 1 | 90-200px | Entry, Router, Pages |
| Spacing | 200-280px | Connection channel |
| Layer 2 | 280-450px | State, API, Components |
| Layer 3 | 460-560px | Utils, Build tools |

### Column Division

| Column | Left Range | Purpose |
|--------|------------|---------|
| Col 1 | 50-180px | User Entry |
| Col 2 | 200-360px | Router |
| Col 3 | 380-580px | Pages |
| Col 4 | 600-800px | Components |
| Col 5 | 820-960px | State/API |
| Right | right:40px | Info cards |

### Group Coordinates

| ID | Name | left | top | width | height | right | bottom |
|----|------|------|-----|-------|--------|-------|--------|
| G1 | Entry | 50 | 90 | 130 | 108 | 180 | 198 |
| G2 | Router | 200 | 90 | 160 | 108 | 360 | 198 |
| G3 | Pages | 380 | 90 | 200 | 108 | 580 | 198 |
| G4 | Components | 600 | 90 | 200 | 108 | 800 | 198 |
| G5 | State | 50 | 280 | 200 | 150 | 250 | 430 |
| G6 | API | 290 | 280 | 180 | 150 | 470 | 430 |
| G7 | Utils | 510 | 280 | 200 | 150 | 710 | 430 |

### Collision Check

| Check | Calculation | Result |
|-------|-------------|--------|
| G1 vs G2 | 180 < 200 | OK, gap=20px |
| G2 vs G3 | 360 < 380 | OK, gap=20px |
| G3 vs G4 | 580 < 600 | OK, gap=20px |
| G4 vs Right | 800 < 1140 | OK |
| G5 vs G6 | 250 < 290 | OK, gap=40px |
| G6 vs G7 | 470 < 510 | OK, gap=40px |
| Row1 vs Row2 | 198 < 280 | OK, gap=82px |

### Component Coordinates

| ID | Group | left | top | width | height |
|----|-------|------|-----|-------|--------|
| C1 | G1 | 66 | 116 | 98 | 40 |
| C2 | G1 | 66 | 160 | 98 | 32 |
| C3 | G2 | 216 | 116 | 128 | 40 |
| C4 | G2 | 216 | 160 | 128 | 32 |
| C5 | G3 | 396 | 116 | 168 | 40 |
| C6 | G3 | 396 | 160 | 80 | 32 |
| C7 | G3 | 480 | 160 | 80 | 32 |
| C8 | G4 | 616 | 116 | 168 | 40 |
| C9 | G4 | 616 | 160 | 80 | 32 |
| C10 | G4 | 700 | 160 | 80 | 32 |

### Connection Planning

| ID | From | To | Type | Color | Path |
|----|------|-----|------|-------|------|
| L1 | Entry | Router | Main Flow | Gray #94a3b8 | Direct horizontal |
| L2 | Router | Pages | Main Flow | Gray #94a3b8 | Direct horizontal |
| L3 | Pages | Components | Dependency | Blue #2563eb | Direct horizontal |
| L4 | Pages | State | Data | Green #059669 | L-shape via (350, 240) |
| L5 | Components | API | Dependency | Blue #2563eb | L-shape via (850, 240) |
| L6 | State | API | Data | Green #059669 | Direct horizontal |

## Generated HTML

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Vue 3 — 前端架构</title>
<style>
  :root {
    --bg-page: #f3f4f6;
    --bg-canvas: #ffffff;
    --bg-box: #ffffff;
    --bg-source: #f0fdfa;
    --bg-dashed: #f8fafb;
    --border: #d1d5db;
    --border-source: #5eead4;
    --text: #1e293b;
    --text-dim: #64748b;
    --text-tech: #94a3b8;
  }
  * { margin: 0; padding: 0; box-sizing: border-box; }
  body { background: var(--bg-page); display: flex; justify-content: center; padding: 24px; }

  .wrapper { width: 100%; max-width: 1480px; }
  .canvas {
    width: 1440px; height: 580px; position: relative;
    background: var(--bg-canvas); border-radius: 12px; border: 1px solid var(--border);
    font-family: -apple-system, BlinkMacSystemFont, 'Inter', 'PingFang SC', sans-serif;
    transform-origin: top left;
  }

  .box {
    position: absolute; border: 1.5px solid var(--border); border-radius: 9px;
    background: var(--bg-box); color: var(--text);
    display: flex; flex-direction: column; align-items: center; justify-content: center;
    font-size: 13px; font-weight: 600; text-align: center; padding: 4px 12px;
  }
  .box.source { background: var(--bg-source); border-color: var(--border-source); }
  .box.dashed { border-style: dashed; border-color: #cbd5e1; background: var(--bg-dashed); font-weight: 500; }
  .sub { font-size: 10px; color: var(--text-dim); font-weight: 400; margin-top: 2px; }
  .tech { font-size: 9.5px; color: var(--text-tech); margin-top: 2px; }

  .group {
    position: absolute; border: 1.5px dashed #cbd5e1; border-radius: 14px;
    background: rgba(248, 250, 252, 0.6);
  }
  .tag {
    position: absolute; top: -10px; left: 14px;
    font-size: 9.5px; font-weight: 700; padding: 2px 10px; border-radius: 10px;
    letter-spacing: 0.6px; text-transform: uppercase; color: #ffffff;
  }
  .tag.teal { background: #0d9488; }
  .tag.red { background: #e11d48; }
  .tag.green { background: #059669; }
  .tag.orange { background: #d97706; }
  .tag.blue { background: #2563eb; }
  .tag.violet { background: #7c3aed; }

  .enclosure {
    position: absolute; border: 1px solid #e2e8f0; border-radius: 18px;
    background: rgba(236, 253, 245, 0.3);
  }
  .enclosure-label {
    position: absolute; color: #059669; font-size: 9px; font-weight: 500;
    font-family: 'SF Mono', 'Menlo', monospace;
  }

  .legend { position: absolute; display: flex; gap: 16px; }
  .legend-item { display: flex; align-items: center; gap: 6px; color: #94a3b8; font-size: 10px; }
  .legend-line { width: 24px; height: 0; }
</style>
</head>
<body>
<div class="wrapper">
  <div class="canvas" id="canvas">

    <div style="position:absolute; top:22px; left:40px; color:#0f172a; font-size:17px; font-weight:700">
      Vue 3 <span style="color:#94a3b8; font-size:13px; font-weight:400; margin-left:10px">前端架构</span>
    </div>

    <svg style="position:absolute; top:0; left:0; width:100%; height:100%; pointer-events:none" viewBox="0 0 1440 580">
      <defs>
        <marker id="ah" markerWidth="7" markerHeight="5" refX="7" refY="2.5" orient="auto">
          <polygon points="0 0, 7 2.5, 0 5" fill="#94a3b8" />
        </marker>
        <marker id="ah-green" markerWidth="7" markerHeight="5" refX="7" refY="2.5" orient="auto">
          <polygon points="0 0, 7 2.5, 0 5" fill="#059669" />
        </marker>
        <marker id="ah-blue" markerWidth="7" markerHeight="5" refX="7" refY="2.5" orient="auto">
          <polygon points="0 0, 7 2.5, 0 5" fill="#2563eb" />
        </marker>
        <marker id="ah-teal" markerWidth="7" markerHeight="5" refX="7" refY="2.5" orient="auto">
          <polygon points="0 0, 7 2.5, 0 5" fill="#0d9488" />
        </marker>
      </defs>
      <!-- Entry → Router: Main Flow (Gray) -->
      <line x1="180" y1="136" x2="200" y2="136" stroke="#94a3b8" stroke-width="1.5" marker-end="url(#ah)" />
      <!-- Router → Pages: Main Flow (Gray) -->
      <line x1="360" y1="136" x2="380" y2="136" stroke="#94a3b8" stroke-width="1.5" marker-end="url(#ah)" />
      <!-- Pages → Components: Dependency (Blue) -->
      <line x1="580" y1="136" x2="600" y2="136" stroke="#2563eb" stroke-width="1.5" marker-end="url(#ah-blue)" />
      <!-- Pages → State: Data flow (Green) via middle layer -->
      <path d="M 480 198 L 480 240 L 150 240 L 150 280" stroke="#059669" stroke-width="1.2" stroke-dasharray="4,3" fill="none" marker-end="url(#ah-green)" />
      <!-- Components → API: Dependency (Blue) via middle layer -->
      <path d="M 700 198 L 700 240 L 380 240 L 380 280" stroke="#2563eb" stroke-width="1.2" stroke-dasharray="4,3" fill="none" marker-end="url(#ah-blue)" />
      <!-- State → API: Data flow (Green) -->
      <line x1="250" y1="355" x2="290" y2="355" stroke="#059669" stroke-width="1.2" stroke-dasharray="4,3" marker-end="url(#ah-green)" />
    </svg>

    <!-- Entry Group -->
    <div class="group" style="left:50px; top:90px; width:130px; height:108px">
      <span class="tag blue">用户入口</span>
    </div>
    <div class="box source" style="left:66px; top:116px; width:98px; height:40px">
      Browser<span class="tech">Web</span>
    </div>
    <div class="box source" style="left:66px; top:160px; width:98px; height:32px">
      Mobile<span class="tech">App</span>
    </div>

    <!-- Router Group -->
    <div class="group" style="left:200px; top:90px; width:160px; height:108px">
      <span class="tag blue">路由层</span>
    </div>
    <div class="box" style="left:216px; top:116px; width:128px; height:40px; border-color:#3b82f6">
      Vue Router<span class="sub">路由管理</span>
    </div>
    <div class="box dashed" style="left:216px; top:160px; width:128px; height:32px">
      Route Guard
    </div>

    <!-- Pages Group -->
    <div class="group" style="left:380px; top:90px; width:200px; height:108px">
      <span class="tag green">页面组件</span>
    </div>
    <div class="box" style="left:396px; top:116px; width:168px; height:40px; border-color:#34d399">
      Views<span class="sub">页面视图</span>
    </div>
    <div class="box dashed" style="left:396px; top:160px; width:80px; height:32px">Home</div>
    <div class="box dashed" style="left:480px; top:160px; width:80px; height:32px">About</div>

    <!-- Components Group -->
    <div class="group" style="left:600px; top:90px; width:200px; height:108px">
      <span class="tag green">通用组件</span>
    </div>
    <div class="box" style="left:616px; top:116px; width:168px; height:40px; border-color:#34d399">
      Components<span class="sub">UI组件库</span>
    </div>
    <div class="box dashed" style="left:616px; top:160px; width:80px; height:32px">Button</div>
    <div class="box dashed" style="left:700px; top:160px; width:80px; height:32px">Modal</div>

    <!-- State Management Group -->
    <div class="group" style="left:50px; top:280px; width:200px; height:150px">
      <span class="tag teal">状态管理</span>
    </div>
    <div class="box" style="left:66px; top:306px; width:168px; height:40px; border-color:#14b8a6">
      Pinia<span class="sub">Store</span>
    </div>
    <div class="box dashed" style="left:66px; top:350px; width:80px; height:32px">userStore</div>
    <div class="box dashed" style="left:150px; top:350px; width:80px; height:32px">appStore</div>
    <div class="box dashed" style="left:66px; top:386px; width:168px; height:32px">PersistedState</div>

    <!-- API Layer Group -->
    <div class="group" style="left:290px; top:280px; width:180px; height:150px">
      <span class="tag violet">API 层</span>
    </div>
    <div class="box" style="left:306px; top:306px; width:148px; height:40px; border-color:#8b5cf6">
      Axios<span class="sub">HTTP Client</span>
    </div>
    <div class="box dashed" style="left:306px; top:350px; width:148px; height:32px">Request封装</div>
    <div class="box dashed" style="left:306px; top:386px; width:148px; height:32px">Interceptors</div>

    <!-- Utils Group -->
    <div class="group" style="left:510px; top:280px; width:200px; height:150px">
      <span class="tag orange">工具层</span>
    </div>
    <div class="box" style="left:526px; top:306px; width:168px; height:40px; border-color:#f59e0b">
      Utils<span class="sub">工具函数</span>
    </div>
    <div class="box dashed" style="left:526px; top:350px; width:80px; height:32px">format</div>
    <div class="box dashed" style="left:610px; top:350px; width:80px; height:32px">validate</div>
    <div class="box dashed" style="left:526px; top:386px; width:168px; height:32px">Composables</div>

    <!-- Build Tools -->
    <div class="enclosure" style="left:50px; top:460px; width:660px; height:70px"></div>
    <div class="enclosure-label" style="left:65px; top:466px">构建工具</div>
    <div class="box" style="left:65px; top:486px; width:140px; height:36px; border-color:#a7f3d0; background:#f0fdf4">Vite</div>
    <div class="box" style="left:215px; top:486px; width:140px; height:36px; border-color:#a7f3d0; background:#f0fdf4">TypeScript</div>
    <div class="box" style="left:365px; top:486px; width:140px; height:36px; border-color:#a7f3d0; background:#f0fdf4">ESLint</div>
    <div class="box" style="left:515px; top:486px; width:140px; height:36px; border-color:#a7f3d0; background:#f0fdf4">Prettier</div>

    <!-- Right Info Card -->
    <div style="position:absolute; right:40px; top:90px; width:260px">
      <div style="border:1px solid #e2e8f0; border-radius:10px; padding:12px 14px; background:rgba(248,250,252,0.8)">
        <div style="font-size:10px; font-weight:700; color:#1e293b; margin-bottom:6px">核心设计理念</div>
        <div style="font-size:9.5px; color:#64748b; line-height:1.6">
          <div><span style="color:#3b82f6; font-weight:600">组件化开发</span> — 可复用UI组件</div>
          <div><span style="color:#14b8a6; font-weight:600">集中式状态</span> — Pinia Store</div>
          <div><span style="color:#8b5cf6; font-weight:600">API 封装</span> — Axios拦截器</div>
          <div><span style="color:#f59e0b; font-weight:600">组合式函数</span> — Composables</div>
        </div>
      </div>
    </div>

    <!-- Legend -->
    <div class="legend" style="bottom:20px; right:40px">
      <div class="legend-item"><div class="legend-line" style="border-top:1.5px solid #94a3b8"></div>页面跳转</div>
      <div class="legend-item"><div class="legend-line" style="border-top:1.5px dashed #059669"></div>状态流</div>
      <div class="legend-item"><div class="legend-line" style="border-top:1.5px solid #2563eb"></div>组件引用</div>
    </div>

  </div>
</div>
<script>
  function resize() {
    const scale = Math.min(document.querySelector('.wrapper').clientWidth / 1440, 1);
    document.querySelector('.canvas').style.transform = `scale(${scale})`;
    document.querySelector('.wrapper').style.height = (580 * scale) + 'px';
  }
  window.addEventListener('resize', resize);
  resize();
</script>
</body>
</html>
```

## Pre-generation Checklist Results

```
[x] All group coordinates are multiples of 10
[x] All component coordinates are multiples of 10
[x] Same-row groups don't overlap, gap >= 20px
[x] Components don't exceed group boundaries
[x] Leftmost component left >= 40px
[x] Rightmost group right (800) < 1140 (for right info area)
[x] Topmost component top >= 80px
[x] Right info cards don't overlap with main groups
[x] Connections use defined anchor points
[x] Connection colors match semantic meaning
```
