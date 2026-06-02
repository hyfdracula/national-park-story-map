# 五大国家公园 · 林草故事地图

北京林业大学地理信息科学专业 2026 综合实习 — 课题五

## 运行方式

1. **本地运行**: 双击 `index.html`，浏览器直接打开（需联网加载 Leaflet 和 ECharts CDN）
2. **在线访问**: 部署到 Gitee Pages 后通过网址访问

## 文件结构

```
国家公园故事地图/
├── index.html              # 主文件（HTML + CSS + JS）
├── data/
│   ├── parks.geojson       # 五大公园边界数据
│   ├── parks-content.json  # 公园内容（文字/物种/图片/链接）
│   └── statistics.json     # 对比统计数据
├── images/                 # 图片素材目录
├── docs/
│   ├── design-spec.md      # 设计规格说明书
│   └── plans/              # 实现计划
└── README.md
```

## 技术栈

- **HTML5 + CSS3 + Vanilla JS** — 零框架，零构建
- **Leaflet 1.9** — 交互地图
- **ECharts 5** — 对比图表
- **Web Speech API** — 语音讲解

## 功能

- 交互式地图浏览（OSM + 卫星图切换）
- 五大公园边界展示与定位
- 详情面板（历史沿革/地理环境/旗舰物种/保护成效/图片画廊/相关文献）
- 多公园数据对比图表
- 故事导览（手动导航 + 自动漫游 + 语音讲解）
- 响应式布局（桌面端 + 移动端）

## 数据来源

- 国家公园边界：组员整理
- 物种信息：IUCN 红色名录
- 统计数据：各国家公园官方公布数据
- 图片：picsum.photos 占位（正式版替换为真实图片）

## 维护说明

修改公园文字内容：编辑 `data/parks-content.json`
修改公园边界：编辑 `data/parks.geojson`
添加图片：放入 `images/` 目录，更新 `parks-content.json` 中的路径
