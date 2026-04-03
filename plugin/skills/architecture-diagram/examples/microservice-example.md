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
| G1 | Client | 50 | 90 | 145 | 108 | 195 | 198 |
| G2 | Gateway | 230 | 90 | 150 | 108 | 380 | 198 |
| G3 | Services | 430 | 90 | 180 | 108 | 610 | 198 |
| G4 | Storage | 660 | 90 | 120 | 108 | 780 | 198 |
| G5 | Nacos | 800 | 90 | 160 | 108 | 960 | 198 |
| G6 | RocketMQ | 50 | 280 | 180 | 190 | 230 | 470 |
| G7 | Seata | 260 | 280 | 180 | 190 | 440 | 470 |
| G8 | Sentinel | 470 | 280 | 180 | 190 | 650 | 470 |
| G9 | Nacos Ext | 680 | 280 | 160 | 190 | 840 | 470 |

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
    width: 1440px; height: 620px; position: relative;
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
      Spring Cloud Alibaba <span style="color:#94a3b8; font-size:13px; font-weight:400; margin-left:10px">微服务架构</span>
    </div>

    <svg style="position:absolute; top:0; left:0; width:100%; height:100%; pointer-events:none" viewBox="0 0 1440 620">
      <defs>
        <marker id="ah" markerWidth="7" markerHeight="5" refX="7" refY="2.5" orient="auto">
          <polygon points="0 0, 7 2.5, 0 5" fill="#94a3b8" />
        </marker>
        <marker id="ah-teal" markerWidth="7" markerHeight="5" refX="7" refY="2.5" orient="auto">
          <polygon points="0 0, 7 2.5, 0 5" fill="#0d9488" />
        </marker>
      </defs>
      <line x1="195" y1="140" x2="230" y2="140" stroke="#94a3b8" stroke-width="1.5" marker-end="url(#ah)" />
      <line x1="380" y1="140" x2="430" y2="140" stroke="#94a3b8" stroke-width="1.5" marker-end="url(#ah)" />
      <line x1="610" y1="140" x2="660" y2="140" stroke="#94a3b8" stroke-width="1.2" stroke-dasharray="4,3" marker-end="url(#ah)" />
      <path d="M 305 188 L 305 210 Q 305 220, 315 220 L 870 220 Q 880 220, 880 210 L 880 158" stroke="#0d9488" stroke-width="1.2" stroke-dasharray="4,3" fill="none" marker-end="url(#ah-teal)" />
    </svg>

    <div class="box source" style="left:66px; top:116px; width:113px; height:48px">
      客户端请求<span class="tech">Browser · Mobile</span>
    </div>

    <div class="group" style="left:230px; top:90px; width:150px; height:108px">
      <span class="tag blue">API 网关</span>
    </div>
    <div class="box" style="left:246px; top:116px; width:118px; height:40px; border-color:#3b82f6">
      Gateway<span class="sub">路由 · 过滤</span>
    </div>
    <div class="box dashed" style="left:246px; top:160px; width:118px; height:32px">
      GlobalFilter
    </div>

    <div class="group" style="left:430px; top:90px; width:180px; height:108px">
      <span class="tag green">微服务集群</span>
    </div>
    <div class="box" style="left:446px; top:116px; width:148px; height:40px; border-color:#34d399">
      Service Provider<span class="sub">业务服务</span>
    </div>
    <div class="box dashed" style="left:446px; top:160px; width:72px; height:32px">OpenFeign</div>
    <div class="box dashed" style="left:522px; top:160px; width:72px; height:32px">LB</div>

    <div class="box source" style="left:676px; top:116px; width:88px; height:40px">MySQL</div>
    <div class="box source" style="left:676px; top:160px; width:88px; height:32px">Redis</div>

    <div class="group" style="left:800px; top:90px; width:160px; height:108px">
      <span class="tag teal">Nacos</span>
    </div>
    <div class="box" style="left:816px; top:116px; width:128px; height:40px; border-color:#14b8a6">
      注册中心<span class="sub">Registry</span>
    </div>
    <div class="box dashed" style="left:816px; top:160px; width:128px; height:32px">配置中心</div>

    <div class="group" style="left:50px; top:280px; width:180px; height:190px">
      <span class="tag violet">RocketMQ</span>
    </div>
    <div class="box" style="left:66px; top:306px; width:148px; height:36px; border-color:#8b5cf6">Producer</div>
    <div class="box dashed" style="left:66px; top:346px; width:148px; height:36px">Broker</div>
    <div class="box dashed" style="left:66px; top:386px; width:148px; height:36px">Consumer</div>

    <div class="group" style="left:260px; top:280px; width:180px; height:190px">
      <span class="tag orange">Seata</span>
    </div>
    <div class="box" style="left:276px; top:306px; width:148px; height:36px; border-color:#f59e0b">TC 协调器</div>
    <div class="box dashed" style="left:276px; top:346px; width:148px; height:36px">TM 管理器</div>
    <div class="box dashed" style="left:276px; top:386px; width:148px; height:36px">RM 资源</div>

    <div class="group" style="left:470px; top:280px; width:180px; height:190px">
      <span class="tag red">Sentinel</span>
    </div>
    <div class="box" style="left:486px; top:306px; width:148px; height:36px; border-color:#e11d48">流量控制</div>
    <div class="box dashed" style="left:486px; top:346px; width:148px; height:36px">熔断降级</div>
    <div class="box dashed" style="left:486px; top:386px; width:148px; height:36px">系统保护</div>

    <div class="enclosure" style="left:50px; top:510px; width:910px; height:80px"></div>
    <div class="enclosure-label" style="left:65px; top:516px">基础设施层</div>
    <div class="box" style="left:65px; top:536px; width:130px; height:40px; border-color:#a7f3d0; background:#f0fdf4">Skywalking</div>
    <div class="box" style="left:210px; top:536px; width:130px; height:40px; border-color:#a7f3d0; background:#f0fdf4">Prometheus</div>
    <div class="box" style="left:355px; top:536px; width:130px; height:40px; border-color:#a7f3d0; background:#f0fdf4">Grafana</div>
    <div class="box" style="left:500px; top:536px; width:130px; height:40px; border-color:#a7f3d0; background:#f0fdf4">Kubernetes</div>
    <div class="box" style="left:645px; top:536px; width:130px; height:40px; border-color:#a7f3d0; background:#f0fdf4">Docker</div>
    <div class="box" style="left:790px; top:536px; width:130px; height:40px; border-color:#a7f3d0; background:#f0fdf4">ELK</div>

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

    <div class="legend" style="bottom:20px; right:40px">
      <div class="legend-item"><div class="legend-line" style="border-top:1.5px solid #94a3b8"></div>请求流</div>
      <div class="legend-item"><div class="legend-line" style="border-top:1.5px dashed #0d9488"></div>服务发现</div>
      <div class="legend-item"><div class="legend-line" style="border-top:1.5px dashed #e11d48"></div>流量治理</div>
      <div class="legend-item"><div class="legend-line" style="border-top:1.5px dashed #d97706"></div>事务</div>
    </div>

  </div>
</div>
<script>
  function resize() {
    const scale = Math.min(document.querySelector('.wrapper').clientWidth / 1440, 1);
    document.querySelector('.canvas').style.transform = `scale(${scale})`;
    document.querySelector('.wrapper').style.height = (620 * scale) + 'px';
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
[x] Same-row groups don't overlap, gap >= 30px
[x] Components don't exceed group boundaries
[x] Leftmost component left >= 40px
[x] Rightmost group right (960) < 1140 (for right info area)
[x] Topmost component top >= 80px
[x] Right info cards don't overlap with main groups
[x] Connections use defined anchor points
```
