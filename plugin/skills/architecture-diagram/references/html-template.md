# HTML Template

这是一个完整的 HTML 模板，包含所有样式定义。**生成架构图时必须基于此模板**，确保样式一致性。

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
  /* 背景色 */
  --bg-page: #f3f4f6;
  --bg-canvas: #ffffff;
  --bg-box: #ffffff;
  --bg-source: #f0fdfa;
  --bg-dashed: #f8fafb;
  --bg-highlight: #fffbeb;
  --bg-infra: #f0fdf4;

  /* 边框色 */
  --border-normal: #d1d5db;
  --border-source: #5eead4;
  --border-highlight: #f59e0b;
  --border-infra: #a7f3d0;

  /* 文字色 */
  --text-primary: #1e293b;
  --text-secondary: #64748b;
  --text-muted: #94a3b8;
}

/* ========== 重置样式 ========== */
* { margin: 0; padding: 0; box-sizing: border-box; }

/* ========== 页面布局 ========== */
body {
  background: var(--bg-page);
  display: flex;
  justify-content: center;
  align-items: flex-start;
  min-height: 100vh;
  padding: 24px;
}

.wrapper {
  width: 100%;
  max-width: 1480px;
}

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

/* 普通组件 */
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
}

/* 数据源组件（外部系统、数据库） */
.box.source {
  background: var(--bg-source);
  border-color: var(--border-source);
}

/* 虚线组件（子功能） */
.box.dashed {
  border-style: dashed;
  border-color: #cbd5e1;
  background: var(--bg-dashed);
  font-weight: 500;
  box-shadow: none;
}

/* 高亮组件（重要/实验性） */
.box.highlight {
  border-color: #f59e0b;
  background: var(--bg-highlight);
}

/* 基础设施组件 */
.box.infra {
  border-color: var(--border-infra);
  background: var(--bg-infra);
}

/* ========== 组件内文字 ========== */
.sub {
  font-size: 10px;
  color: var(--text-secondary);
  font-weight: 400;
  margin-top: 2px;
}

.tech {
  font-size: 9.5px;
  color: var(--text-muted);
  font-weight: 400;
  margin-top: 2px;
}

/* ========== 分组容器 ========== */
.group {
  position: absolute;
  border: 1.5px dashed #cbd5e1;
  border-radius: 14px;
  background: rgba(248, 250, 252, 0.6);
}

/* ========== 分组标签 ========== */
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

/* 标签颜色 */
.tag.teal { background: #0d9488; }
.tag.blue { background: #2563eb; }
.tag.green { background: #059669; }
.tag.red { background: #e11d48; }
.tag.orange { background: #d97706; }
.tag.violet { background: #7c3aed; }
.tag.cyan { background: #0891b2; }
.tag.slate { background: #64748b; }

/* ========== 包围框（基础设施层） ========== */
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

.info-card-title {
  font-size: 10px;
  font-weight: 700;
  color: #1e293b;
  margin-bottom: 6px;
  letter-spacing: 0.3px;
}

.info-card-content {
  font-size: 9.5px;
  color: #64748b;
  line-height: 1.6;
}

/* ========== 图例 ========== */
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
</style>
</head>
<body>
<div class="wrapper">
  <div class="canvas" id="canvas">

    <!-- ========== 标题 ========== -->
    <div style="position:absolute; top:22px; left:40px; color:#0f172a; font-size:17px; font-weight:700">
      {架构名称} <span style="color:#94a3b8; font-size:13px; font-weight:400; margin-left:10px">{副标题}</span>
    </div>

    <!-- ========== SVG 连线层 ========== -->
    <svg style="position:absolute; top:0; left:0; width:100%; height:100%; pointer-events:none" viewBox="0 0 1440 900">
      <defs>
        <!-- 箭头标记 -->
        <marker id="ah" markerWidth="7" markerHeight="5" refX="7" refY="2.5" orient="auto">
          <polygon points="0 0, 7 2.5, 0 5" fill="#94a3b8" />
        </marker>
        <marker id="ah-teal" markerWidth="7" markerHeight="5" refX="7" refY="2.5" orient="auto">
          <polygon points="0 0, 7 2.5, 0 5" fill="#0d9488" />
        </marker>
        <marker id="ah-green" markerWidth="7" markerHeight="5" refX="7" refY="2.5" orient="auto">
          <polygon points="0 0, 7 2.5, 0 5" fill="#059669" />
        </marker>
        <marker id="ah-blue" markerWidth="7" markerHeight="5" refX="7" refY="2.5" orient="auto">
          <polygon points="0 0, 7 2.5, 0 5" fill="#2563eb" />
        </marker>
        <marker id="ah-red" markerWidth="7" markerHeight="5" refX="7" refY="2.5" orient="auto">
          <polygon points="0 0, 7 2.5, 0 5" fill="#e11d48" />
        </marker>
        <marker id="ah-orange" markerWidth="7" markerHeight="5" refX="7" refY="2.5" orient="auto">
          <polygon points="0 0, 7 2.5, 0 5" fill="#d97706" />
        </marker>
        <marker id="ah-violet" markerWidth="7" markerHeight="5" refX="7" refY="2.5" orient="auto">
          <polygon points="0 0, 7 2.5, 0 5" fill="#7c3aed" />
        </marker>
      </defs>

      <!-- 连线示例：水平线 -->
      <!-- <line x1="起点X" y1="起点Y" x2="终点X" y2="终点Y" stroke="#94a3b8" stroke-width="1.5" marker-end="url(#ah)" /> -->

      <!-- 连线示例：带拐点的路径 -->
      <!-- <path d="M 起点X 起点Y L 拐点X 拐点Y L 终点X 终点Y" stroke="#0d9488" stroke-width="1.2" stroke-dasharray="4,3" fill="none" marker-end="url(#ah-teal)" /> -->

    </svg>

    <!-- ========== 组件区域 ========== -->

    <!-- 示例：数据源组件 -->
    <!-- <div class="box source" style="left:50px; top:116px; width:145px; height:48px">
      组件名称<span class="tech">技术说明</span>
    </div> -->

    <!-- 示例：分组 -->
    <!-- <div class="group" style="left:230px; top:90px; width:150px; height:108px">
      <span class="tag blue">分组名称</span>
    </div> -->

    <!-- 示例：分组内组件 -->
    <!-- <div class="box" style="left:246px; top:116px; width:118px; height:40px; border-color:#3b82f6">
      组件名称<span class="sub">说明</span>
    </div> -->

    <!-- 示例：虚线子组件 -->
    <!-- <div class="box dashed" style="left:246px; top:160px; width:118px; height:32px">
      子组件
    </div> -->

    <!-- ========== 基础设施层 ========== -->
    <!-- <div class="enclosure" style="left:50px; top:510px; width:910px; height:80px"></div>
    <div class="enclosure-label" style="left:65px; top:516px">基础设施层</div>
    <div class="box infra" style="left:65px; top:536px; width:110px; height:40px">组件名</div> -->

    <!-- ========== 右侧信息区 ========== -->
    <!-- <div style="position:absolute; right:40px; top:90px; width:260px">
      <div class="info-card">
        <div class="info-card-title">卡片标题</div>
        <div class="info-card-content">
          内容...
        </div>
      </div>
    </div> -->

    <!-- ========== 图例 ========== -->
    <div class="legend" style="bottom:20px; right:40px">
      <div class="legend-item">
        <div class="legend-line" style="border-top:1.5px solid #94a3b8"></div>
        请求流
      </div>
      <div class="legend-item">
        <div class="legend-line" style="border-top:1.5px dashed #0d9488"></div>
        服务发现
      </div>
    </div>

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

### 1. 必须复制完整 style 部分

生成 HTML 时，**必须完整复制上述模板中的 `<style>` 部分**，不得省略或简化。

### 2. 连线颜色对应关系

| 连线类型 | stroke 颜色 | marker 引用 |
|---------|------------|-------------|
| 请求流 | #94a3b8 | url(#ah) |
| 服务发现 | #0d9488 | url(#ah-teal) |
| 注册/配置 | #059669 | url(#ah-green) |
| 服务调用 | #2563eb | url(#ah-blue) |
| 流量治理 | #e11d48 | url(#ah-red) |
| 事务 | #d97706 | url(#ah-orange) |
| 消息 | #7c3aed | url(#ah-violet) |

### 3. 组件边框颜色对应关系

| 组件类型 | border-color | 背景色 |
|---------|-------------|--------|
| 网关类 | #3b82f6 (blue) | 默认 |
| 服务类 | #34d399 (green) | 默认 |
| 注册配置 | #14b8a6 (teal) | 默认 |
| 流量治理 | #e11d48 (red) | 默认 |
| 事务 | #f59e0b (orange) | 默认 |
| 消息 | #8b5cf6 (violet) | 默认 |
| 外部系统 | #5eead4 (source) | #f0fdfa |
| 基础设施 | #a7f3d0 | #f0fdf4 |

### 4. 连线类型

**实线（主流程）**
```html
<line x1="X1" y1="Y1" x2="X2" y2="Y2" stroke="#94a3b8" stroke-width="1.5" marker-end="url(#ah)" />
```

**虚线（次级流程）**
```html
<line x1="X1" y1="Y1" x2="X2" y2="Y2" stroke="#0d9488" stroke-width="1.2" stroke-dasharray="4,3" marker-end="url(#ah-teal)" />
```

**带拐点的路径**
```html
<path d="M 起点X 起点Y L 拐点X 拐点Y L 终点X 终点Y" stroke="#颜色" stroke-width="1.2" stroke-dasharray="4,3" fill="none" marker-end="url(#箭头)" />
```

**带圆弧的路径**
```html
<path d="M X1 Y1 Q 控制点X 控制点Y X2 Y2" stroke="#颜色" stroke-width="1.2" stroke-dasharray="4,3" fill="none" marker-end="url(#箭头)" />
```
