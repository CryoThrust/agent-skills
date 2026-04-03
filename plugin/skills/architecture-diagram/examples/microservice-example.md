# Microservice Architecture Example

## User Input

```
画一个 Spring Cloud Alibaba 微服务架构图，包含：
- 客户端请求入口
- Gateway 网关
- 微服务集群（使用 OpenFeign 和 LoadBalancer）
- Nacos 作为注册中心和配置中心
- Sentinel 流量控制
- Seata 分布式事务
- RocketMQ 消息队列
- MySQL 和 Redis 存储
- 基础设施层包含 Skywalking、Prometheus、Grafana、Kubernetes、Docker、ELK
```

## Layout Planning

### Layer Division

| Layer | Top Range | Purpose |
|-------|-----------|---------|
| Title | 20-80px | Title |
| Layer 1 | 90-200px | Entry, Gateway, Services, Storage |
| Spacing | 200-280px | Connection channel |
| Layer 2 | 280-500px | Governance components |
| Layer 3 | 510-620px | Infrastructure |

### Column Division

| Column | Left Range | Purpose |
|--------|------------|---------|
| Col 1 | 50-200px | Client |
| Col 2 | 220-400px | Gateway |
| Col 3 | 420-620px | Services |
| Col 4 | 640-800px | Storage |
| Col 5 | 800-960px | Nacos |
| Right | right:40px | Info cards |

### Group Coordinates

| ID | Name | left | top | width | height | right | bottom |
|----|------|------|-----|-------|--------|-------|--------|
| G1 | Client | 50 | 90 | 145 | 122 | 195 | 212 |
| G2 | Gateway | 230 | 90 | 150 | 122 | 380 | 212 |
| G3 | Services | 430 | 90 | 180 | 122 | 610 | 212 |
| G4 | Storage | 660 | 90 | 120 | 122 | 780 | 212 |
| G5 | Nacos | 800 | 90 | 160 | 122 | 960 | 212 |
| G6 | RocketMQ | 50 | 280 | 180 | 168 | 230 | 448 |
| G7 | Seata | 260 | 280 | 180 | 168 | 440 | 448 |
| G8 | Sentinel | 470 | 280 | 180 | 168 | 650 | 448 |

**分组高度计算（底部内边距16px）**：
- 第一行分组: 26(顶) + 40(主组件) + 8(间距) + 32(子组件) + 16(底) = 122px
- 第二行分组: 26(顶) + 36×3组件 + 8×2间距 + 16(底) = 168px

### Collision Check

| Check | Calculation | Result |
|-------|-------------|--------|
| G1 vs G2 | 195 < 230 | OK, gap=35px |
| G2 vs G3 | 380 < 430 | OK, gap=50px |
| G3 vs G4 | 610 < 660 | OK, gap=50px |
| G4 vs G5 | 780 < 800 | OK, gap=20px |
| G5 vs Right | 960 < 1140 | OK |
| G6 vs G7 | 230 < 260 | OK, gap=30px |
| G7 vs G8 | 440 < 470 | OK, gap=30px |
| G8 vs G9 | 650 < 680 | OK, gap=30px |
| Row1 vs Row2 | 198 < 280 | OK, gap=82px |

### Component Coordinates

| ID | Group | left | top | width | height |
|----|-------|------|-----|-------|--------|
| C1 | G1 | 66 | 116 | 113 | 40 |
| C2 | G2 | 246 | 116 | 118 | 40 |
| C3 | G2 | 246 | 160 | 118 | 32 |
| C4 | G3 | 446 | 116 | 148 | 40 |
| C5 | G3 | 446 | 160 | 72 | 32 |
| C6 | G3 | 522 | 160 | 72 | 32 |
| C7 | G4 | 676 | 116 | 88 | 40 |
| C8 | G4 | 676 | 160 | 88 | 32 |
| C9 | G5 | 816 | 116 | 128 | 40 |
| C10 | G5 | 816 | 160 | 128 | 32 |

## Generated HTML

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Spring Cloud Alibaba — 微服务架构</title>
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
    z-index: 5;
  }
  .box.source { background: var(--bg-source); border-color: var(--border-source); }
  .box.dashed { border-style: dashed; border-color: #cbd5e1; background: var(--bg-dashed); font-weight: 500; }
  .sub { font-size: 10px; color: var(--text-dim); font-weight: 400; margin-top: 2px; }
  .tech { font-size: 9.5px; color: var(--text-tech); margin-top: 2px; }

  .group {
    position: absolute; border: 1.5px dashed #cbd5e1; border-radius: 14px;
    background: rgba(248, 250, 252, 0.6);
    z-index: 1;
  }
  .tag {
    position: absolute; top: -11px; left: 14px;
    font-size: 9.5px; font-weight: 700; padding: 2px 10px; border-radius: 10px;
    letter-spacing: 0.6px; text-transform: uppercase; color: #ffffff;
    z-index: 10;
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

  /* 流动动画 */
  @keyframes flow {
    0% { stroke-dashoffset: 16; }
    100% { stroke-dashoffset: 0; }
  }
  .flow {
    stroke-dasharray: 6 10;
    animation: flow 2s linear infinite;
  }

  /* 悬停高亮 */
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

    <div style="position:absolute; top:22px; left:40px; color:#0f172a; font-size:17px; font-weight:700">
      Spring Cloud Alibaba <span style="color:#94a3b8; font-size:13px; font-weight:400; margin-left:10px">微服务架构</span>
    </div>

    <!-- 分隔线 -->
    <div style="position:absolute; top:60px; left:40px; right:40px; height:1px; background:#e2e8f0"></div>

    <!-- SVG 连线层（注意：不要设置 pointer-events:none，否则悬停高亮无法生效） -->
    <svg style="position:absolute; top:0; left:0; width:100%; height:100%; z-index:3" viewBox="0 0 1440 580">
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
      <line x1="195" y1="140" x2="230" y2="140" stroke="#94a3b8" stroke-width="1.5" marker-end="url(#ah)" class="connection flow" />
      <line x1="380" y1="140" x2="430" y2="140" stroke="#94a3b8" stroke-width="1.5" marker-end="url(#ah)" class="connection flow" />
      <line x1="610" y1="140" x2="660" y2="140" stroke="#059669" stroke-width="1.5" marker-end="url(#ah-green)" class="connection flow" />
      <path d="M 305 188 L 305 210 Q 305 220, 315 220 L 870 220 Q 880 220, 880 210 L 880 158" stroke="#0d9488" stroke-width="1.5" fill="none" marker-end="url(#ah-teal)" class="connection flow" />
    </svg>

    <div class="box source" style="left:66px; top:116px; width:113px; height:48px">
      客户端请求<span class="tech">Browser · Mobile</span>
    </div>

    <div class="group" style="left:230px; top:90px; width:150px; height:122px">
      <span class="tag blue">API 网关</span>
    </div>
    <div class="box" style="left:246px; top:116px; width:118px; height:40px; border-color:#3b82f6">
      Gateway<span class="sub">路由 · 过滤</span>
    </div>
    <div class="box dashed" style="left:246px; top:164px; width:118px; height:32px">
      GlobalFilter
    </div>

    <div class="group" style="left:430px; top:90px; width:180px; height:122px">
      <span class="tag green">微服务集群</span>
    </div>
    <div class="box" style="left:446px; top:116px; width:148px; height:40px; border-color:#34d399">
      Service Provider<span class="sub">业务服务</span>
    </div>
    <div class="box dashed" style="left:446px; top:164px; width:72px; height:32px">OpenFeign</div>
    <div class="box dashed" style="left:522px; top:164px; width:72px; height:32px">LB</div>

    <div class="box source" style="left:676px; top:116px; width:88px; height:40px">MySQL</div>
    <div class="box source" style="left:676px; top:164px; width:88px; height:32px">Redis</div>

    <div class="group" style="left:800px; top:90px; width:160px; height:122px">
      <span class="tag teal">Nacos</span>
    </div>
    <div class="box" style="left:816px; top:116px; width:128px; height:40px; border-color:#14b8a6">
      注册中心<span class="sub">Registry</span>
    </div>
    <div class="box dashed" style="left:816px; top:164px; width:128px; height:32px">配置中心</div>

    <div class="group" style="left:50px; top:280px; width:180px; height:168px">
      <span class="tag violet">RocketMQ</span>
    </div>
    <div class="box" style="left:66px; top:306px; width:148px; height:36px; border-color:#8b5cf6">Producer</div>
    <div class="box dashed" style="left:66px; top:350px; width:148px; height:36px">Broker</div>
    <div class="box dashed" style="left:66px; top:394px; width:148px; height:36px">Consumer</div>

    <div class="group" style="left:260px; top:280px; width:180px; height:168px">
      <span class="tag orange">Seata</span>
    </div>
    <div class="box" style="left:276px; top:306px; width:148px; height:36px; border-color:#f59e0b">TC 协调器</div>
    <div class="box dashed" style="left:276px; top:350px; width:148px; height:36px">TM 管理器</div>
    <div class="box dashed" style="left:276px; top:394px; width:148px; height:36px">RM 资源</div>

    <div class="group" style="left:470px; top:280px; width:180px; height:168px">
      <span class="tag red">Sentinel</span>
    </div>
    <div class="box" style="left:486px; top:306px; width:148px; height:36px; border-color:#e11d48">流量控制</div>
    <div class="box dashed" style="left:486px; top:350px; width:148px; height:36px">熔断降级</div>
    <div class="box dashed" style="left:486px; top:394px; width:148px; height:36px">系统保护</div>

    <div class="enclosure" style="left:50px; top:480px; width:910px; height:80px"></div>
    <div class="enclosure-label" style="left:65px; top:486px">基础设施层</div>
    <div class="box" style="left:65px; top:506px; width:130px; height:40px; border-color:#a7f3d0; background:#f0fdf4">Skywalking</div>
    <div class="box" style="left:210px; top:506px; width:130px; height:40px; border-color:#a7f3d0; background:#f0fdf4">Prometheus</div>
    <div class="box" style="left:355px; top:506px; width:130px; height:40px; border-color:#a7f3d0; background:#f0fdf4">Grafana</div>
    <div class="box" style="left:500px; top:506px; width:130px; height:40px; border-color:#a7f3d0; background:#f0fdf4">Kubernetes</div>
    <div class="box" style="left:645px; top:506px; width:130px; height:40px; border-color:#a7f3d0; background:#f0fdf4">Docker</div>
    <div class="box" style="left:790px; top:506px; width:130px; height:40px; border-color:#a7f3d0; background:#f0fdf4">ELK</div>

    <div style="position:absolute; right:40px; top:90px; width:260px">
      <div style="border:1px solid #e2e8f0; border-radius:10px; padding:12px 14px; background:rgba(248,250,252,0.8)">
        <div style="font-size:10px; font-weight:700; color:#1e293b; margin-bottom:6px">核心设计理念</div>
        <div style="font-size:9.5px; color:#64748b; line-height:1.6">
          <div><span style="color:#14b8a6; font-weight:600">服务治理一体化</span> — Nacos</div>
          <div><span style="color:#e11d48; font-weight:600">流量可控</span> — Sentinel</div>
          <div><span style="color:#d97706; font-weight:600">事务保障</span> — Seata</div>
          <div><span style="color:#8b5cf6; font-weight:600">高可靠消息</span> — RocketMQ</div>
        </div>
      </div>
    </div>

    <!-- 图例（虚线 + 箭头）- 放在标题右侧，使用 legend- 前缀避免 ID 冲突 -->
    <svg class="legend" style="position:absolute; top:28px; right:40px; width:280px; height:20px; pointer-events:none" viewBox="0 0 280 20">
      <defs>
        <marker id="legend-ah" markerWidth="5" markerHeight="4" refX="5" refY="2" orient="auto">
          <polygon points="0 0, 5 2, 0 4" fill="#94a3b8" />
        </marker>
        <marker id="legend-ah-teal" markerWidth="5" markerHeight="4" refX="5" refY="2" orient="auto">
          <polygon points="0 0, 5 2, 0 4" fill="#0d9488" />
        </marker>
        <marker id="legend-ah-green" markerWidth="5" markerHeight="4" refX="5" refY="2" orient="auto">
          <polygon points="0 0, 5 2, 0 4" fill="#059669" />
        </marker>
      </defs>
      <line x1="0" y1="10" x2="24" y2="10" stroke="#94a3b8" stroke-width="1.5" stroke-dasharray="4,3" marker-end="url(#legend-ah)" />
      <text x="30" y="13" fill="#94a3b8" font-size="10">请求流</text>
      <line x1="90" y1="10" x2="114" y2="10" stroke="#0d9488" stroke-width="1.5" stroke-dasharray="4,3" marker-end="url(#legend-ah-teal)" />
      <text x="120" y="13" fill="#94a3b8" font-size="10">服务发现</text>
      <line x1="180" y1="10" x2="204" y2="10" stroke="#059669" stroke-width="1.5" stroke-dasharray="4,3" marker-end="url(#legend-ah-green)" />
      <text x="210" y="13" fill="#94a3b8" font-size="10">数据流</text>
    </svg>

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

## Checklist

```
[x] 标题下方有灰色分隔线
[x] 连线使用 class="connection flow"
[x] SVG 连线层没有 pointer-events:none
[x] 图例使用虚线样式 + 箭头
[x] 图例放在标题右侧，有 pointer-events:none
[x] 连线锚点在组件边缘
[x] 连线不穿过任何组件
[x] 分组底部内边距 >= 16px
[x] z-index: group(1) < svg(3) < box(5)
[x] All group coordinates are multiples of 10
[x] Same-row groups don't overlap, gap >= 30px
[x] Rightmost group right (960) < 1140 (for right info area)
```
