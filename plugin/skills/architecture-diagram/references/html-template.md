# HTML Template

生成架构图时**必须使用此完整模板**。

## 完整模板代码

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>{架构名称} — 架构图</title>
<style>
/* ========== 基础变量 ========== */
:root {
  --bg-page: #f3f4f6;
  --bg-canvas: #ffffff;
  --bg-box: #ffffff;
  --bg-source: #f0fdfa;
  --bg-dashed: #f8fafb;
  --bg-highlight: #fffbeb;
  --bg-infra: #f0fdf4;
  --border-normal: #d1d5db;
  --border-source: #5eead4;
  --border-highlight: #f59e0b;
  --border-infra: #a7f3d0;
  --text-primary: #1e293b;
  --text-secondary: #64748b;
  --text-muted: #94a3b8;
}

* { margin: 0; padding: 0; box-sizing: border-box; }

body {
  background: var(--bg-page);
  display: flex;
  justify-content: center;
  align-items: flex-start;
  min-height: 100vh;
  padding: 24px;
}

.wrapper { width: 100%; max-width: 1480px; }

.canvas {
  width: 1440px;
  height: 900px;
  position: relative;
  overflow: hidden;
  background: var(--bg-canvas);
  border-radius: 12px;
  border: 1px solid var(--border-normal);
  font-family: -apple-system, BlinkMacSystemFont, 'Inter', 'Segoe UI', 'PingFang SC', 'Microsoft YaHei', sans-serif;
  transform-origin: top left;
  box-shadow: 0 1px 3px rgba(0,0,0,0.06), 0 4px 16px rgba(0,0,0,0.04);
}

/* ========== 组件样式 ========== */
.box {
  position: absolute;
  border: 1.5px solid var(--border-normal);
  border-radius: 9px;
  background: var(--bg-box);
  color: var(--text-primary);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  font-size: 13px;
  font-weight: 600;
  text-align: center;
  line-height: 1.35;
  padding: 4px 12px;
  box-shadow: 0 1px 2px rgba(0,0,0,0.04);
  z-index: 5;
}

.box.source { background: var(--bg-source); border-color: var(--border-source); }
.box.dashed { border-style: dashed; border-color: #cbd5e1; background: var(--bg-dashed); font-weight: 500; box-shadow: none; }
.box.highlight { border-color: #f59e0b; background: var(--bg-highlight); }
.box.infra { border-color: var(--border-infra); background: var(--bg-infra); }

.sub { font-size: 10px; color: var(--text-secondary); font-weight: 400; margin-top: 2px; }
.tech { font-size: 9.5px; color: var(--text-muted); font-weight: 400; margin-top: 2px; }

/* ========== 分组容器 ========== */
.group {
  position: absolute;
  border: 1.5px dashed #cbd5e1;
  border-radius: 14px;
  background: rgba(248, 250, 252, 0.6);
  z-index: 1;
}

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

.tag.teal { background: #0d9488; }
.tag.blue { background: #2563eb; }
.tag.green { background: #059669; }
.tag.red { background: #e11d48; }
.tag.orange { background: #d97706; }
.tag.violet { background: #7c3aed; }
.tag.cyan { background: #0891b2; }
.tag.slate { background: #64748b; }

/* ========== 包围框 ========== */
.enclosure {
  position: absolute;
  border: 1px solid #e2e8f0;
  border-radius: 18px;
  background: rgba(236, 253, 245, 0.3);
}

.enclosure-label {
  position: absolute;
  color: #059669;
  font-size: 9px;
  font-weight: 500;
  letter-spacing: 0.4px;
  font-family: 'SF Mono', 'Menlo', monospace;
}

/* ========== 信息卡片 ========== */
.info-card {
  border: 1px solid #e2e8f0;
  border-radius: 10px;
  padding: 12px 14px;
  background: rgba(248, 250, 252, 0.8);
}

.info-card-title { font-size: 10px; font-weight: 700; color: #1e293b; margin-bottom: 6px; letter-spacing: 0.3px; }
.info-card-content { font-size: 9.5px; color: #64748b; line-height: 1.6; }

/* ========== 图例 ========== */
.legend { position: absolute; display: flex; gap: 16px; }
.legend-item { display: flex; align-items: center; gap: 6px; color: #94a3b8; font-size: 10px; }
.legend-line { width: 24px; height: 0; }

/* ========== 连线流动动画 ========== */
@keyframes flow {
  0% { stroke-dashoffset: 16; }
  100% { stroke-dashoffset: 0; }
}

.flow {
  stroke-dasharray: 6 10;
  animation: flow 2s linear infinite;
}

/* ========== 连线悬停高亮 ========== */
.connection {
  cursor: pointer;
  stroke-width: 1.5px;
  transition: stroke-width 0.2s, filter 0.2s;
}

.connection:hover {
  stroke-width: 3px;
  filter: drop-shadow(0 0 6px currentColor);
}
</style>
</head>
<body>
<div class="wrapper">
  <div class="canvas" id="canvas">

    <!-- 标题 -->
    <div style="position:absolute; top:22px; left:40px; color:#0f172a; font-size:17px; font-weight:700">
      {架构名称} <span style="color:#94a3b8; font-size:13px; font-weight:400; margin-left:10px">{副标题}</span>
    </div>

    <!-- 分隔线 -->
    <div style="position:absolute; top:60px; left:40px; right:40px; height:1px; background:#e2e8f0"></div>

    <!-- 组件区域 -->

    <!-- SVG 连线层（层级：group < svg < box） -->
    <!-- 注意：不要设置 pointer-events:none，否则悬停高亮无法生效 -->
    <svg class="connections" style="position:absolute; top:0; left:0; width:100%; height:100%; z-index:3" viewBox="0 0 1440 900">
      <defs>
        <marker id="ah" markerWidth="5" markerHeight="4" refX="5" refY="2" orient="auto">
          <polygon points="0 0, 5 2, 0 4" fill="#94a3b8" />
        </marker>
        <marker id="ah-teal" markerWidth="5" markerHeight="4" refX="5" refY="2" orient="auto">
          <polygon points="0 0, 5 2, 0 4" fill="#0d9488" />
        </marker>
        <marker id="ah-green" markerWidth="5" markerHeight="4" refX="5" refY="2" orient="auto">
          <polygon points="0 0, 5 2, 0 4" fill="#059669" />
        </marker>
        <marker id="ah-blue" markerWidth="5" markerHeight="4" refX="5" refY="2" orient="auto">
          <polygon points="0 0, 5 2, 0 4" fill="#2563eb" />
        </marker>
        <marker id="ah-red" markerWidth="5" markerHeight="4" refX="5" refY="2" orient="auto">
          <polygon points="0 0, 5 2, 0 4" fill="#e11d48" />
        </marker>
        <marker id="ah-orange" markerWidth="5" markerHeight="4" refX="5" refY="2" orient="auto">
          <polygon points="0 0, 5 2, 0 4" fill="#d97706" />
        </marker>
        <marker id="ah-violet" markerWidth="5" markerHeight="4" refX="5" refY="2" orient="auto">
          <polygon points="0 0, 5 2, 0 4" fill="#7c3aed" />
        </marker>
      </defs>
      <!-- 连线示例：class="connection flow" -->
      <!-- <line x1="起点X" y1="起点Y" x2="终点X" y2="终点Y" stroke="#颜色" stroke-width="1.5" marker-end="url(#ah)" class="connection flow" /> -->
    </svg>

    <!-- 图例（虚线 + 箭头） -->
    <svg class="legend" style="position:absolute; bottom:20px; right:40px; width:280px; height:20px" viewBox="0 0 280 20">
      <defs>
        <marker id="leg-ah" markerWidth="5" markerHeight="4" refX="5" refY="2" orient="auto">
          <polygon points="0 0, 5 2, 0 4" fill="#94a3b8" />
        </marker>
        <marker id="leg-ah-green" markerWidth="5" markerHeight="4" refX="5" refY="2" orient="auto">
          <polygon points="0 0, 5 2, 0 4" fill="#059669" />
        </marker>
        <marker id="leg-ah-blue" markerWidth="5" markerHeight="4" refX="5" refY="2" orient="auto">
          <polygon points="0 0, 5 2, 0 4" fill="#2563eb" />
        </marker>
      </defs>
      <!-- 主流程 -->
      <line x1="0" y1="10" x2="24" y2="10" stroke="#94a3b8" stroke-width="1.5" stroke-dasharray="4,3" marker-end="url(#leg-ah)" />
      <text x="30" y="13" fill="#94a3b8" font-size="10">主流程</text>
      <!-- 数据流 -->
      <line x1="90" y1="10" x2="114" y2="10" stroke="#059669" stroke-width="1.5" stroke-dasharray="4,3" marker-end="url(#leg-ah-green)" />
      <text x="120" y="13" fill="#94a3b8" font-size="10">数据流</text>
      <!-- 依赖 -->
      <line x1="180" y1="10" x2="204" y2="10" stroke="#2563eb" stroke-width="1.5" stroke-dasharray="4,3" marker-end="url(#leg-ah-blue)" />
      <text x="210" y="13" fill="#94a3b8" font-size="10">依赖</text>
    </svg>

  </div>
</div>

<script>
function resize() {
  const wrapper = document.querySelector('.wrapper');
  const canvas = document.querySelector('.canvas');
  const scale = Math.min(wrapper.clientWidth / 1440, 1);
  canvas.style.transform = `scale(${scale})`;
  wrapper.style.height = (900 * scale) + 'px';
}
window.addEventListener('resize', resize);
resize();
</script>
</body>
</html>
```

---

## 使用规范

1. **必须复制完整 style 部分**
2. **连线类**：`class="connection flow"`
   - `connection`：悬停时加粗 + 发光高亮
   - `flow`：流动动画
3. **层级**：group(z-index:1) < svg(z-index:3) < box(z-index:5)
4. **图例**：使用 SVG 绘制虚线 + 箭头，参考模板中的 legend 部分
5. **分隔线**：标题下方添加灰色分隔线

## 锚点计算（关键！）

**连线必须连接到组件边缘锚点，不能是中心！**

| 场景 | 起点 | 终点 | 代码示例 |
|------|------|------|---------|
| 水平连线：A在B左边 | A右中 | B左中 | `x1="A.right" y1="A.top+A.h/2" x2="B.left" y2="B.top+B.h/2"` |
| 垂直连线：A在B上边 | A下中 | B上中 | `x1="A.left+A.w/2" y1="A.bottom" x2="B.left+B.w/2" y2="B.top"` |

**示例**：组件A(100,50,120,40) → 组件B(100,150,120,40) 垂直连线
- A下中：(100+60=160, 50+40=90)
- B上中：(100+60=160, 150)
```html
<line x1="160" y1="90" x2="160" y2="150" stroke="#94a3b8" stroke-width="1.5" marker-end="url(#ah)" class="connection flow" />
```
