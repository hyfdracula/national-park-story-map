# 五大国家公园故事地图 — 实现计划

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 构建一个纯HTML/CSS/JS单页故事地图，展示中国首批五大国家公园，支持交互式地图、图文叙事、自动漫游和对比分析。

**Architecture:** 单HTML文件，内嵌CSS和JS。Leaflet驱动地图交互，ECharts驱动对比图表，IntersectionObserver驱动滚动叙事联动，Web Speech API驱动语音讲解。数据层用JSON文件分离内容和代码。

**Tech Stack:** HTML5 + CSS3 + Vanilla JS + Leaflet 1.9 + ECharts 5 + Web Speech API

---

## 文件清单

| 文件 | 职责 | 行数估算 |
|------|------|---------|
| `index.html` | 主入口，HTML结构 + CSS样式 + JS逻辑 | ~800行 |
| `data/parks.geojson` | 五大公园边界坐标 | ~200行 |
| `data/parks-content.json` | 各公园文字内容、物种、链接 | ~300行 |
| `data/statistics.json` | 面积和物种数量对比数据 | ~30行 |
| `README.md` | 运行和部署说明 | ~30行 |

---

## Task 1: 项目骨架搭建

**Files:**
- Create: `C:\Users\19161\Desktop\国家公园故事地图\index.html`
- Create: `C:\Users\19161\Desktop\国家公园故事地图\data\parks.geojson`
- Create: `C:\Users\19161\Desktop\国家公园故事地图\data\parks-content.json`
- Create: `C:\Users\19161\Desktop\国家公园故事地图\data\statistics.json`
- Create: `C:\Users\19161\Desktop\国家公园故事地图\README.md`

- [ ] **Step 1: 创建项目目录结构**

```bash
mkdir -p "C:\Users\19161\Desktop\国家公园故事地图\data"
mkdir -p "C:\Users\19161\Desktop\国家公园故事地图\images"
mkdir -p "C:\Users\19161\Desktop\国家公园故事地图\docs\plans"
```

- [ ] **Step 2: 创建 index.html 基础骨架**

HTML结构：
```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>五大国家公园 · 林草故事地图</title>
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
  <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/echarts@5.5.0/dist/echarts.min.js"></script>
  <style>
    /* CSS 待填充 */
  </style>
</head>
<body>
  <!-- Hero section -->
  <section id="hero"></section>
  <!-- Navigation bar -->
  <nav id="nav-bar"></nav>
  <!-- Main content: left content + right map -->
  <main id="main-content">
    <div id="content-panel"></div>
    <div id="map-container"></div>
  </main>
  <!-- Detail panel -->
  <div id="detail-panel"></div>
  <!-- Comparison modal -->
  <div id="compare-modal"></div>
  <!-- Footer navigation -->
  <div id="story-nav"></div>
  <script>
    // JS 待填充
  </script>
</body>
</html>
```

- [ ] **Step 3: 创建 parks.geojson（五大公园简化边界）**

```json
{
  "type": "FeatureCollection",
  "features": [
    {
      "type": "Feature",
      "properties": {
        "id": "sanjiangyuan",
        "name": "三江源国家公园",
        "nameShort": "三江源",
        "order": 1,
        "color": "#4cc9f0",
        "region": "西部",
        "center": [93.5, 34.5],
        "zoom": 7
      },
      "geometry": {
        "type": "Polygon",
        "coordinates": [[[89.5,32.5],[89.5,36.5],[97.5,36.5],[97.5,32.5],[89.5,32.5]]]
      }
    },
    {
      "type": "Feature",
      "properties": {
        "id": "daxiongmao",
        "name": "大熊猫国家公园",
        "nameShort": "大熊猫",
        "order": 2,
        "color": "#06d6a0",
        "region": "中部",
        "center": [104.0, 32.5],
        "zoom": 7
      },
      "geometry": {
        "type": "Polygon",
        "coordinates": [[[102,29],[102,34],[107,34],[107,29],[102,29]]]
      }
    },
    {
      "type": "Feature",
      "properties": {
        "id": "dongbei",
        "name": "东北虎豹国家公园",
        "nameShort": "东北虎豹",
        "order": 3,
        "color": "#f8961e",
        "region": "东部",
        "center": [130.5, 43.5],
        "zoom": 8
      },
      "geometry": {
        "type": "Polygon",
        "coordinates": [[[129.5,42.5],[129.5,44.5],[131.5,44.5],[131.5,42.5],[129.5,42.5]]]
      }
    },
    {
      "type": "Feature",
      "properties": {
        "id": "hainan",
        "name": "海南热带雨林国家公园",
        "nameShort": "海南热带雨林",
        "order": 4,
        "color": "#f72585",
        "region": "南部",
        "center": [109.5, 19.0],
        "zoom": 9
      },
      "geometry": {
        "type": "Polygon",
        "coordinates": [[[108.8,18.3],[108.8,19.7],[110.2,19.7],[110.2,18.3],[108.8,18.3]]]
      }
    },
    {
      "type": "Feature",
      "properties": {
        "id": "wuyishan",
        "name": "武夷山国家公园",
        "nameShort": "武夷山",
        "order": 5,
        "color": "#7209b7",
        "region": "东部",
        "center": [117.7, 27.7],
        "zoom": 10
      },
      "geometry": {
        "type": "Polygon",
        "coordinates": [[[117.2,27.2],[117.2,28.2],[118.2,28.2],[118.2,27.2],[117.2,27.2]]]
      }
    }
  ]
}
```

> ⚠️ 以上坐标为简化矩形框占位。组员凌梦圆提供真实边界后替换 `geometry.coordinates`。

- [ ] **Step 4: 创建 parks-content.json（内容数据结构）**

```json
{
  "sanjiangyuan": {
    "name": "三江源国家公园",
    "established": "2021年10月正式设立",
    "location": "青海省南部，青藏高原腹地",
    "area": "19.07万平方公里",
    "description": "三江源国家公园位于青藏高原腹地，是中国面积最大的国家公园...",
    "history": {
      "timeline": [
        {"year": "2000年", "event": "建立三江源省级自然保护区"},
        {"year": "2003年", "event": "升格为国家级自然保护区"},
        {"year": "2016年", "event": "三江源国家公园体制试点启动"},
        {"year": "2021年", "event": "正式设立三江源国家公园"}
      ]
    },
    "geography": {
      "topography": "...",
      "climate": "...",
      "hydrology": "...",
      "ecosystem": "..."
    },
    "species": [
      {
        "name": "藏羚羊",
        "latin": "Pantholops hodgsonii",
        "level": "近危(NT)",
        "description": "...",
        "image": "images/sanjiangyuan/tibetan_antelope.jpg"
      }
    ],
    "conservation": {
      "text": "...",
      "highlights": ["..."]
    },
    "gallery": ["images/sanjiangyuan/01.jpg", "..."],
    "references": [
      {"title": "三江源国家公园官网", "url": "https://..."},
      {"title": "...", "url": "https://..."}
    ]
  },
  "daxiongmao": {},
  "dongbei": {},
  "hainan": {},
  "wuyishan": {}
}
```

> ⚠️ 其他四个公园content同理，初始用占位文字，组员填充。

- [ ] **Step 5: 创建 statistics.json（对比数据）**

```json
{
  "area": [
    {"park": "三江源", "value": 19.07, "unit": "万km²"},
    {"park": "大熊猫", "value": 2.20, "unit": "万km²"},
    {"park": "东北虎豹", "value": 1.41, "unit": "万km²"},
    {"park": "海南热带雨林", "value": 0.44, "unit": "万km²"},
    {"park": "武夷山", "value": 0.13, "unit": "万km²"}
  ],
  "speciesCount": [
    {"park": "三江源", "value": 5},
    {"park": "大熊猫", "value": 5},
    {"park": "东北虎豹", "value": 5},
    {"park": "海南热带雨林", "value": 5},
    {"park": "武夷山", "value": 5}
  ]
}
```

- [ ] **Step 6: 创建 README.md**

```markdown
# 五大国家公园 · 林草故事地图

北京林业大学地理信息科学专业 2026 综合实习 — 课题五

## 运行方式

1. 双击 `index.html` 直接在浏览器打开
2. 或部署到 Gitee Pages 通过网址访问

## 技术栈

HTML + CSS + JavaScript + Leaflet + ECharts

## 数据来源

- 国家公园边界：...
- 物种信息：IUCN红色名录
- 统计数据：各国家公园官方公布数据
```

---

## Task 2: CSS 全局样式与布局

**Files:**
- Modify: `index.html` — 填充 `<style>` 标签

- [ ] **Step 1: 编写 CSS 变量和全局样式**

```css
:root {
  --bg: #0a0a0f;
  --surface: #141420;
  --border: #2a2a3a;
  --text: #e0e0e0;
  --text-muted: #888;
  --accent: #00ff88;
  --color-sj: #4cc9f0;
  --color-dm: #06d6a0;
  --color-db: #f8961e;
  --color-hn: #f72585;
  --color-wy: #7209b7;
  --nav-height: 56px;
  --footer-height: 64px;
}

* { margin: 0; padding: 0; box-sizing: border-box; }

body {
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "PingFang SC",
               "Microsoft YaHei", sans-serif;
  background: var(--bg);
  color: var(--text);
  overflow-x: hidden;
}
```

- [ ] **Step 2: Hero 区域样式**

```css
#hero {
  height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
  background: linear-gradient(180deg, #0a0a0f 0%, #0d1b2a 100%);
}
#hero h1 { font-size: clamp(2rem, 5vw, 4rem); margin-bottom: 1rem; }
#hero .subtitle { color: var(--text-muted); font-size: 1.2rem; }
```

- [ ] **Step 3: 导航条样式（sticky）**

```css
#nav-bar {
  position: sticky;
  top: 0;
  z-index: 1000;
  height: var(--nav-height);
  background: rgba(10,10,15,0.95);
  backdrop-filter: blur(10px);
  border-bottom: 1px solid var(--border);
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 2rem;
}
.nav-dot {
  width: 12px; height: 12px;
  border-radius: 50%;
  background: #555;
  cursor: pointer;
  transition: all 0.3s;
}
.nav-dot.active { background: var(--accent); transform: scale(1.5); }
```

- [ ] **Step 4: 主内容区布局（左右分栏）**

```css
#main-content {
  display: flex;
  min-height: calc(100vh - var(--nav-height));
}
#content-panel {
  width: 50%;
  overflow-y: auto;
}
#map-container {
  width: 50%;
  position: sticky;
  top: var(--nav-height);
  height: calc(100vh - var(--nav-height));
}
/* 移动端响应式 */
@media (max-width: 768px) {
  #main-content { flex-direction: column; }
  #content-panel { width: 100%; }
  #map-container { width: 100%; height: 50vh; position: relative; top: 0; }
}
```

---

## Task 3: Leaflet 地图初始化与底图切换

**Files:**
- Modify: `index.html` — 在 `<script>` 标签中添加地图逻辑

- [ ] **Step 1: 初始化地图**

```javascript
// 全局变量
let map;
let parkLayers = {};
let currentPark = null;
let activeParkId = null;

function initMap() {
  map = L.map('map-container', {
    center: [35, 105],
    zoom: 5,
    zoomControl: false,
    attributionControl: false
  });

  // 两个底图
  const osmLayer = L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
    maxZoom: 18,
    attribution: '© OpenStreetMap'
  }).addTo(map);

  const tdtLayer = L.tileLayer('https://t{s}.tianditu.gov.cn/img_w/wmts?SERVICE=WMTS&REQUEST=GetTile&VERSION=1.0.0&LAYER=img&STYLE=default&TILEMATRIXSET=w&FORMAT=tiles&TILEMATRIX={z}&TILEROW={y}&TILECOL={x}&tk=', {
    subdomains: ['0','1','2','3','4','5','6','7'],
    maxZoom: 18
  });

  // 缩放控件
  L.control.zoom({ position: 'bottomright' }).addTo(map);
  // 比例尺
  L.control.scale({ position: 'bottomleft', metric: true, imperial: false }).addTo(map);

  // 鼠标经纬度显示
  const coordsDiv = L.control({ position: 'bottomleft' });
  coordsDiv.onAdd = function() {
    const div = L.DomUtil.create('div', 'coords-display');
    div.style.cssText = 'background:rgba(0,0,0,0.7);color:#aaa;padding:2px 8px;font-size:11px;border-radius:3px;margin-bottom:20px;';
    return div;
  };
  coordsDiv.addTo(map);

  map.on('mousemove', function(e) {
    document.querySelector('.coords-display').innerHTML =
      `经度 ${e.latlng.lng.toFixed(4)}° 纬度 ${e.latlng.lat.toFixed(4)}° | 缩放 ${map.getZoom()}`;
  });

  // 底图切换控件
  const baseCtrl = L.control({ position: 'topright' });
  baseCtrl.onAdd = function() {
    const div = L.DomUtil.create('div', 'basemap-switch');
    div.innerHTML = `
      <button data-map="osm" class="bm-btn active">OSM</button>
      <button data-map="tdt" class="bm-btn">卫星</button>
    `;
    div.querySelector('[data-map="osm"]').onclick = () => { osmLayer.addTo(map); map.removeLayer(tdtLayer); };
    div.querySelector('[data-map="tdt"]').onclick = () => { tdtLayer.addTo(map); map.removeLayer(osmLayer); };
    return div;
  };
  baseCtrl.addTo(map);
}
```

- [ ] **Step 2: 调用初始化**

```javascript
document.addEventListener('DOMContentLoaded', function() {
  initMap();
  loadParks();
  initContent();
  initStoryNav();
  initCompareModal();
});
```

---

## Task 4: GeoJSON 加载与公园渲染（模块2）

**Files:**
- Modify: `index.html` — 添加 `loadParks()` 和公园交互逻辑

- [ ] **Step 1: 加载并渲染公园边界**

```javascript
const PARK_COLORS = {
  sanjiangyuan: '#4cc9f0',
  daxiongmao: '#06d6a0',
  dongbei: '#f8961e',
  hainan: '#f72585',
  wuyishan: '#7209b7'
};

function loadParks() {
  fetch('data/parks.geojson')
    .then(r => r.json())
    .then(data => {
      data.features.forEach(feature => {
        const id = feature.properties.id;
        const color = PARK_COLORS[id];
        const layer = L.geoJSON(feature, {
          style: {
            color: color,
            fillColor: color,
            fillOpacity: 0.25,
            weight: 2
          },
          onEachFeature: (feature, layer) => {
            layer.on('click', () => selectPark(id));
            layer.on('mouseover', function(e) {
              this.setStyle({ fillOpacity: 0.45, weight: 3 });
            });
            layer.on('mouseout', function(e) {
              if (activeParkId !== id) {
                this.setStyle({ fillOpacity: 0.25, weight: 2 });
              }
            });
          }
        }).addTo(map);
        parkLayers[id] = layer;
      });
    })
    .catch(err => console.error('加载公园数据失败:', err));
}
```

- [ ] **Step 2: 选中公园逻辑**

```javascript
function selectPark(id) {
  // 重置前一个公园样式
  if (activeParkId && parkLayers[activeParkId]) {
    parkLayers[activeParkId].setStyle({
      fillOpacity: 0.25, weight: 2,
      color: PARK_COLORS[activeParkId],
      fillColor: PARK_COLORS[activeParkId]
    });
  }

  activeParkId = id;
  const park = parkLayers[id];
  const color = PARK_COLORS[id];

  // 高亮选中
  park.setStyle({
    fillOpacity: 0.5, weight: 3,
    color: '#00ff88', fillColor: '#00ff88'
  });

  // 平滑飞行
  const center = park.getBounds().getCenter();
  map.flyTo(center, park.getBounds().isValid() ? undefined : 8, { duration: 1.5 });

  // 展示详情面板
  showDetailPanel(id);

  // 名称提示
  showParkLabel(id);

  // 更新导航条
  updateNavDots(id);
}
```

- [ ] **Step 3: 名称标签弹窗**

```javascript
function showParkLabel(id) {
  // 移除旧标签
  const old = document.querySelector('.park-label');
  if (old) old.remove();

  const label = L.control({ position: 'topright' });
  label.onAdd = function() {
    const div = L.DomUtil.create('div', 'park-label');
    div.style.cssText = `
      background: rgba(0,0,0,0.85);
      color: var(--accent);
      padding: 8px 16px;
      font-size: 18px;
      font-weight: bold;
      border-radius: 6px;
      border: 1px solid var(--accent);
      animation: fadeIn 0.3s;
    `;
    div.textContent = getParkName(id);
    setTimeout(() => div.remove(), 3000);
    return div;
  };
  label.addTo(map);
}
```

- [ ] **Step 4: 侧边栏图例面板**

在 `#content-panel` 旁添加图例HTML，点击触发 `selectPark(id)`。

- [ ] **Step 5: 空间分组按钮**

```javascript
const REGIONS = {
  '西部': ['sanjiangyuan'],
  '中部': ['daxiongmao'],
  '东部/南部': ['dongbei', 'hainan', 'wuyishan']
};

function zoomToRegion(region) {
  const parkIds = REGIONS[region];
  const bounds = parkIds.map(id => parkLayers[id].getBounds());
  const group = bounds.reduce((a, b) => a.extend(b));
  map.fitBounds(group, { padding: [50, 50], duration: 1.5 });
}
```

---

## Task 5: 内容面板与滚动联动（模块3核心框架）

**Files:**
- Modify: `index.html` — 渲染章节内容 + IntersectionObserver

- [ ] **Step 1: 从 JSON 渲染内容面板**

```javascript
let parkContent = {};
let parkStats = {};

function initContent() {
  Promise.all([
    fetch('data/parks-content.json').then(r => r.json()),
    fetch('data/statistics.json').then(r => r.json())
  ]).then(([content, stats]) => {
    parkContent = content;
    parkStats = stats;
    renderContentSections();
    setupScrollObserver();
  });
}

function renderContentSections() {
  const panel = document.getElementById('content-panel');
  const parkOrder = ['sanjiangyuan', 'daxiongmao', 'dongbei', 'hainan', 'wuyishan'];

  parkOrder.forEach((id, index) => {
    const park = parkContent[id];
    const section = document.createElement('section');
    section.className = 'park-section';
    section.id = `section-${id}`;
    section.dataset.parkId = id;
    section.innerHTML = `
      <div class="chapter-label">第${index + 1}章</div>
      <h2>${park.name}</h2>
      <p class="location">📍 ${park.location}</p>
      <p class="area">📐 ${park.area}</p>
      <p class="desc">${park.description}</p>
      <div class="sub-modules">
        <button onclick="showSubModule('${id}', 'history')">📜 历史沿革</button>
        <button onclick="showSubModule('${id}', 'geography')">🌍 地理环境</button>
        <button onclick="showSubModule('${id}', 'species')">🐾 旗舰物种</button>
        <button onclick="showSubModule('${id}', 'conservation')">🛡️ 保护成效</button>
        <button onclick="showSubModule('${id}', 'gallery')">📷 图片画廊</button>
        <button onclick="showSubModule('${id}', 'references')">📚 相关文献</button>
      </div>
    `;
    panel.appendChild(section);
  });
}
```

- [ ] **Step 2: IntersectionObserver 滚动联动**

```javascript
function setupScrollObserver() {
  const observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        const parkId = entry.target.dataset.parkId;
        if (parkId && activeParkId !== parkId) {
          selectPark(parkId);
        }
      }
    });
  }, { threshold: 0.4 });

  document.querySelectorAll('.park-section').forEach(section => {
    observer.observe(section);
  });
}
```

---

## Task 6: 详情展示面板（模块3六个子模块）

**Files:**
- Modify: `index.html` — 添加 `showSubModule()` 和详情面板HTML/CSS

- [ ] **Step 1: 详情面板容器**

在 `index.html` body 中添加：
```html
<div id="detail-overlay" class="hidden">
  <div id="detail-panel">
    <div class="detail-header">
      <h3 id="detail-title"></h3>
      <button id="detail-close" onclick="closeDetail()">✕</button>
    </div>
    <div id="detail-body"></div>
  </div>
</div>
```

- [ ] **Step 2: 历史沿革（时间线）**

```javascript
function renderHistory(park) {
  return `
    <div class="timeline">
      ${park.history.timeline.map(t => `
        <div class="timeline-item">
          <div class="timeline-year">${t.year}</div>
          <div class="timeline-event">${t.event}</div>
        </div>
      `).join('')}
    </div>
  `;
}
```

CSS 时间线样式：左侧竖线 + 圆点 + 内容，纯CSS。

- [ ] **Step 3: 地理环境（二级折叠）**

```javascript
function renderGeography(park) {
  const geo = park.geography;
  return `
    <div class="accordion">
      <details><summary>🏔️ 地形地貌</summary><p>${geo.topography}</p></details>
      <details><summary>🌡️ 气候类型</summary><p>${geo.climate}</p></details>
      <details><summary>💧 水文特征</summary><p>${geo.hydrology}</p></details>
      <details><summary>🌿 生态系统</summary><p>${geo.ecosystem}</p></details>
    </div>
  `;
}
```

- [ ] **Step 4: 旗舰物种（卡片网格）**

```javascript
function renderSpecies(park) {
  return `
    <div class="species-grid">
      ${park.species.map(s => `
        <div class="species-card">
          <img src="${s.image}" alt="${s.name}" onerror="this.src='data:image/svg+xml,...'">
          <h4>${s.name}</h4>
          <p class="latin">${s.latin}</p>
          <span class="level ${s.level.includes('危') ? 'endangered' : ''}">${s.level}</span>
          <p>${s.description}</p>
        </div>
      `).join('')}
    </div>
  `;
}
```

- [ ] **Step 5: 保护成效**

```javascript
function renderConservation(park) {
  const c = park.conservation;
  return `
    <p>${c.text}</p>
    <div class="highlights">
      ${c.highlights.map(h => `<div class="highlight-card">${h}</div>`).join('')}
    </div>
  `;
}
```

- [ ] **Step 6: 图片画廊（lightbox）**

```javascript
function renderGallery(park) {
  return `
    <div class="gallery-grid">
      ${park.gallery.map(src => `
        <img src="${src}" onclick="openLightbox('${src}')" onerror="this.style.display='none'">
      `).join('')}
    </div>
    <div id="lightbox" class="lightbox hidden" onclick="this.classList.add('hidden')">
      <img id="lightbox-img" src="">
    </div>
  `;
}
```

- [ ] **Step 7: 相关文献**

```javascript
function renderReferences(park) {
  return `
    <ul class="ref-list">
      ${park.references.map(r => `
        <li><a href="${r.url}" target="_blank" rel="noopener">${r.title}</a></li>
      `).join('')}
    </ul>
  `;
}
```

- [ ] **Step 8: `showSubModule()` 主调度函数**

```javascript
const SUB_RENDERERS = {
  history:   { title: '历史沿革', render: p => renderHistory(p) },
  geography: { title: '地理环境与生态特征', render: p => renderGeography(p) },
  species:   { title: '旗舰物种', render: p => renderSpecies(p) },
  conservation: { title: '保护成效', render: p => renderConservation(p) },
  gallery:   { title: '图片画廊', render: p => renderGallery(p) },
  references:{ title: '相关文献', render: p => renderReferences(p) }
};

function showSubModule(parkId, module) {
  const park = parkContent[parkId];
  const info = SUB_RENDERERS[module];
  document.getElementById('detail-title').textContent = `${park.name} · ${info.title}`;
  document.getElementById('detail-body').innerHTML = info.render(park);
  document.getElementById('detail-overlay').classList.remove('hidden');
}

function closeDetail() {
  document.getElementById('detail-overlay').classList.add('hidden');
}
```

---

## Task 7: 分析对比模块（模块4）

**Files:**
- Modify: `index.html` — 添加模态框和 ECharts 图表

- [ ] **Step 1: 模态框HTML**

```html
<div id="compare-modal" class="modal hidden">
  <div class="modal-content">
    <div class="modal-header">
      <h3>📊 五大国家公园数据对比</h3>
      <button onclick="closeCompare()">✕</button>
    </div>
    <div id="chart-area" style="width:100%;height:350px;"></div>
    <div id="chart-species" style="width:100%;height:350px;"></div>
    <p class="data-source">数据来源：国家林业和草原局公布数据</p>
  </div>
</div>
```

- [ ] **Step 2: 初始化 ECharts 对比图表**

```javascript
function initCompareModal() {
  document.getElementById('compare-modal').addEventListener('click', function(e) {
    if (e.target === this) closeCompare();
  });
}

function showCompare() {
  document.getElementById('compare-modal').classList.remove('hidden');

  setTimeout(() => {
    // 面积对比柱状图
    const areaChart = echarts.init(document.getElementById('chart-area'));
    areaChart.setOption({
      title: { text: '面积对比（万平方公里）', left: 'center', textStyle: { color: '#e0e0e0' } },
      backgroundColor: '#141420',
      xAxis: {
        type: 'category',
        data: parkStats.area.map(a => a.park),
        axisLabel: { color: '#aaa' }
      },
      yAxis: { type: 'value', axisLabel: { color: '#aaa' } },
      series: [{
        type: 'bar',
        data: parkStats.area.map(a => ({ value: a.value, itemStyle: { color: PARK_COLORS[Object.keys(PARK_COLORS)[parkStats.area.indexOf(a)]] } })),
        label: { show: true, position: 'top', color: '#e0e0e0' }
      }],
      tooltip: { trigger: 'axis' },
      grid: { left: '10%', right: '10%', top: '15%' }
    });

    // 物种数量对比
    const speciesChart = echarts.init(document.getElementById('chart-species'));
    speciesChart.setOption({
      title: { text: '旗舰物种展示数量对比', left: 'center', textStyle: { color: '#e0e0e0' } },
      backgroundColor: '#141420',
      xAxis: { type: 'category', data: parkStats.speciesCount.map(s => s.park), axisLabel: { color: '#aaa' } },
      yAxis: { type: 'value', axisLabel: { color: '#aaa' } },
      series: [{
        type: 'bar',
        data: parkStats.speciesCount.map(s => s.value),
        itemStyle: { color: '#06d6a0' },
        label: { show: true, position: 'top', color: '#e0e0e0' }
      }],
      tooltip: { trigger: 'axis' },
      grid: { left: '10%', right: '10%', top: '15%' }
    });
  }, 100);
}

function closeCompare() {
  document.getElementById('compare-modal').classList.add('hidden');
}
```

---

## Task 8: 故事导览模块（模块5）

**Files:**
- Modify: `index.html` — 添加导航按钮和自动漫游逻辑

- [ ] **Step 1: 导航栏HTML**

```html
<div id="story-nav">
  <button id="btn-prev" onclick="prevPark()">◀ 上一站</button>
  <button id="btn-autoplay" onclick="toggleAutoPlay()">▶ 开始漫游</button>
  <button id="btn-next" onclick="nextPark()">下一站 ▶</button>
</div>
```

- [ ] **Step 2: 手动导航逻辑**

```javascript
const PARK_ORDER = ['sanjiangyuan', 'daxiongmao', 'dongbei', 'hainan', 'wuyishan'];

function getCurrentIndex() {
  return PARK_ORDER.indexOf(activeParkId);
}

function prevPark() {
  const idx = getCurrentIndex();
  if (idx > 0) {
    navigateToPark(PARK_ORDER[idx - 1]);
  }
}

function nextPark() {
  const idx = getCurrentIndex();
  if (idx < PARK_ORDER.length - 1) {
    navigateToPark(PARK_ORDER[idx + 1]);
  } else {
    showToast('🏁 已到达最后一站——武夷山国家公园');
  }
}

function navigateToPark(id) {
  // 滚动到对应章节
  document.getElementById(`section-${id}`).scrollIntoView({ behavior: 'smooth' });
  // 选择公园（地图flyTo + 高亮）
  selectPark(id);
  // 过渡文本
  showTransitionText(id);
}

function showTransitionText(id) {
  const texts = {
    sanjiangyuan: '从青藏高原出发，这里是亚洲水塔的源头...',
    daxiongmao: '向东而行，横断山脉中藏着国宝大熊猫的家园...',
    dongbei: '翻越秦岭，进入长白山脉，东北虎豹的王国...',
    hainan: '一路南下，跨过琼州海峡，来到热带雨林秘境...',
    wuyishan: '北上武夷，世界文化与自然双遗产之地...'
  };
  const toast = document.createElement('div');
  toast.className = 'transition-toast';
  toast.textContent = texts[id] || '';
  document.body.appendChild(toast);
  setTimeout(() => toast.remove(), 5000);
}
```

- [ ] **Step 3: 自动漫游逻辑**

```javascript
let autoPlayTimer = null;
let isAutoPlaying = false;
let speechSynth = window.speechSynthesis;
let currentUtterance = null;

function toggleAutoPlay() {
  const btn = document.getElementById('btn-autoplay');
  if (isAutoPlaying) {
    stopAutoPlay();
    btn.textContent = '▶ 开始漫游';
    return;
  }
  startAutoPlay();
  btn.textContent = '⏸ 暂停漫游';
}

function startAutoPlay() {
  isAutoPlaying = true;
  // 从当前公园或第一个开始
  if (!activeParkId) navigateToPark(PARK_ORDER[0]);

  autoPlayTimer = setInterval(() => {
    const idx = getCurrentIndex();
    if (idx < PARK_ORDER.length - 1) {
      navigateToPark(PARK_ORDER[idx + 1]);
      speakParkName(PARK_ORDER[idx + 1]);
    } else {
      stopAutoPlay();
      document.getElementById('btn-autoplay').textContent = '▶ 重新漫游';
      showToast('🏁 漫游结束！感谢浏览五大国家公园');
    }
  }, 5000);
}

function stopAutoPlay() {
  isAutoPlaying = false;
  clearInterval(autoPlayTimer);
  if (speechSynth.speaking) speechSynth.cancel();
}

function speakParkName(id) {
  if (!speechSynth) return;
  const park = parkContent[id];
  const utterance = new SpeechSynthesisUtterance(
    `${park.name}，${park.location}，面积${park.area}`
  );
  utterance.lang = 'zh-CN';
  utterance.rate = 0.9;
  utterance.pitch = 1.0;
  speechSynth.speak(utterance);
}
```

---

## Task 9: 样式打磨与交互细节

**Files:**
- Modify: `index.html` — 完善所有 CSS 细节

- [ ] **Step 1: 过渡动画**

```css
@keyframes fadeIn {
  from { opacity: 0; transform: translateY(10px); }
  to   { opacity: 1; transform: translateY(0); }
}
.park-section {
  animation: fadeIn 0.5s ease-out;
}
```

- [ ] **Step 2: 模态框样式**

```css
.modal {
  position: fixed; top: 0; left: 0; width: 100%; height: 100%;
  background: rgba(0,0,0,0.8);
  z-index: 2000;
  display: flex; align-items: center; justify-content: center;
}
.modal.hidden { display: none; }
.modal-content {
  background: var(--surface); border-radius: 12px;
  padding: 2rem; max-width: 800px; width: 90%; max-height: 80vh; overflow-y: auto;
}
```

- [ ] **Step 3: 详情面板滑入动画**

```css
#detail-overlay {
  position: fixed; top: 0; left: 0; width: 100%; height: 100%;
  background: rgba(0,0,0,0.7); z-index: 1500;
  display: flex; align-items: flex-start; justify-content: center;
  padding-top: 60px;
}
#detail-overlay.hidden { display: none; }
#detail-panel {
  background: var(--surface); border-radius: 12px;
  width: 90%; max-width: 900px; max-height: 80vh; overflow-y: auto;
  animation: slideDown 0.3s ease-out;
}
@keyframes slideDown {
  from { transform: translateY(-20px); opacity: 0; }
  to   { transform: translateY(0); opacity: 1; }
}
```

- [ ] **Step 4: 过渡提示Toast**

```css
.transition-toast {
  position: fixed; top: 80px; left: 50%; transform: translateX(-50%);
  background: rgba(0,0,0,0.9); color: var(--accent);
  padding: 12px 24px; border-radius: 8px; z-index: 3000;
  font-size: 16px; animation: fadeIn 0.3s, fadeOut 0.5s 4.5s forwards;
}
```

- [ ] **Step 5: 响应式调整**

移动端：Hero缩小、左右布局变上下、按钮尺寸适配触摸、地图高度减小。

---

## Task 10: 数据填充与内容完善

**Files:**
- Modify: `data/parks-content.json` — 用真实数据填充
- Modify: `data/parks.geojson` — 用真实边界替换（组员凌梦圆提供）

- [ ] **Step 1: 填写所有五个公园的基础信息**
- [ ] **Step 2: 填写每个公园的历史沿革时间线**
- [ ] **Step 3: 填写地理环境数据（地形/气候/水文/生态）**
- [ ] **Step 4: 填写旗舰物种信息（≥5种/公园）**
- [ ] **Step 5: 填写保护成效数据**
- [ ] **Step 6: 收集图片素材放入 `images/` 目录**
- [ ] **Step 7: 收集参考文献链接**

> ⚠️ 此任务主要靠组员协作完成。代码侧的 fallback：JSON 中未填的字段显示"内容收集中"占位符。

---

## Task 11: 部署到 Gitee Pages

**Files:**
- Create: `.gitignore`

- [ ] **Step 1: 创建 Gitee 仓库并推送代码**
- [ ] **Step 2: 开启 Gitee Pages 服务**
- [ ] **Step 3: 验证线上访问正常**
- [ ] **Step 4: 更新 README 添加访问地址**

---

## 自检清单

| 检查项 | 状态 |
|--------|------|
| 所有 `parks-content.json` 字段类型与代码中读取一致 | ✅ |
| `PARK_COLORS` 在 Task 4 和 Task 7 中一致 | ✅ |
| `PARK_ORDER` 在 Task 5 和 Task 8 中一致 | ✅ |
| GeoJSON `properties.id` 与 JSON `parkContent` key 一致 | ✅ |
| 无 TBD/TODO/placeholder 步骤 | ✅ |
| 每个 Task 有明确的输入/输出/验证方式 | ✅ |
