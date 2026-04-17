# Architecture Diagram Generator / 架构图生成器

[English](#english) | [中文](#中文)

---

<a id="english"></a>

## Architecture Diagram Generator

A professional architecture diagram generation skill that creates beautiful, light-themed system architecture diagrams as standalone HTML and SVG files. Based on [Cocoon-AI/architecture-diagram-generator](https://github.com/Cocoon-AI/architecture-diagram-generator).

### Features

- **Light Theme** — White background with subtle grid pattern
- **Semantic Color Coding** — Consistent colors for frontend, backend, database, cloud, and security components
- **Chinese as Default Language** — All labels, titles, and legends use Chinese by default
- **Self-contained Output** — Both HTML and SVG files with embedded styles and inline graphics
- **Dual Format** — Generates HTML (full page with cards) and standalone SVG (for embedding)
- **No Dependencies** — Opens in any modern browser, no JavaScript required
- **Professional Typography** — SimHei for Chinese, JetBrains Mono for technical labels
- **Smart Layering** — Arrows render cleanly behind component boxes
- **Bus-style Connections** — Multiple connections from one component share a single exit point
- **Review Mechanism** — Built-in checklist to prevent overlapping, misrouting, and other errors

### Color Palette (Light Theme)

| Component Type | Fill | Stroke | Use For |
|---|---|---|---|
| Frontend | Light cyan | Cyan-600 | Client apps, UI, edge devices |
| Backend | Light emerald | Emerald-600 | Servers, APIs, services |
| Database | Light violet | Violet-600 | Databases, storage, AI/ML |
| Cloud/AWS | Light amber | Amber-600 | Cloud services, infrastructure |
| Security | Light rose | Rose-600 | Auth, security groups, encryption |
| Cache/Redis | Light purple | Purple-600 | Caching, session storage |
| Message Bus | Light orange | Orange-600 | Message queues, event streaming |
| External | Light slate | Slate-500 | Generic, external systems |

### Project Structure

```
architecture-diagram/
├── SKILL.md              # Skill instructions & design system
└── assets/
    └── template.html     # Base HTML/SVG template
```

### How It Works

1. **Layout Planning** — Create an ASCII layout plan with component positions and connections
2. **Coordinate Calculation** — Use precise formulas for arrow coordinates (bus-style for multi-connections)
3. **SVG Generation** — Generate the final HTML/SVG following the design system
4. **Review & Fix** — Systematic checklist to verify no overlaps, correct routing, and proper connections
5. **SVG Extraction** — Extract the pure SVG from HTML as a standalone `.svg` file

### Output

The skill generates **two** self-contained files:

**HTML file** (`.html`) — Full page with:
- Embedded CSS and inline SVG
- Summary cards and footer metadata
- Can be opened directly in any browser

**SVG file** (`.svg`) — Standalone vector graphic:
- Extracted from the HTML, no HTML wrapper
- All styles and definitions inlined
- Can be embedded in documents, presentations, or websites
- Scalable to any resolution without quality loss

Both files can be:
- Opened directly in any browser
- Shared with teammates
- Included in documentation
- Printed or exported to PDF
- Hosted on any static site

### Acknowledgements

This project is based on [Cocoon-AI/architecture-diagram-generator](https://github.com/Cocoon-AI/architecture-diagram-generator).

### License

MIT

---

<a id="中文"></a>

## 架构图生成器

一个专业的架构图生成技能，可以创建精美的浅色主题系统架构图，同时输出 HTML 和 SVG 两种格式的独立文件。基于 [Cocoon-AI/architecture-diagram-generator](https://github.com/Cocoon-AI/architecture-diagram-generator) 优化开发。

### 特性

- **浅色主题** — 白色背景配合细微网格图案
- **语义化配色** — 前端、后端、数据库、云服务和安全组件使用一致的配色方案
- **默认中文输出** — 所有标签、标题、图例默认使用中文
- **自包含输出** — 同时生成 HTML 和 SVG 文件，内嵌样式和图形
- **双格式输出** — HTML（含摘要卡片的完整页面）和 SVG（可嵌入的独立矢量图）
- **零依赖** — 在任何现代浏览器中打开即可，无需 JavaScript
- **专业排版** — 中文使用黑体（SimHei），技术标签使用 JetBrains Mono 等宽字体
- **智能分层** — 箭头在组件框下方渲染，层次清晰
- **总线式连线** — 同一组件的多条连线共享单一出口点，避免分散
- **审查机制** — 内置检查清单，防止重叠、错连等问题

### 配色方案（浅色主题）

| 组件类型 | 填充色 | 描边色 | 用途 |
|---|---|---|---|
| 前端 | 浅青色 | 青色-600 | 客户端应用、UI、边缘设备 |
| 后端 | 浅翠绿 | 翠绿-600 | 服务器、API、服务 |
| 数据库 | 浅紫罗兰 | 紫罗兰-600 | 数据库、存储、AI/ML |
| 云服务 | 浅琥珀 | 琥珀-600 | 云服务、基础设施 |
| 安全 | 浅玫瑰 | 玫瑰-600 | 认证、安全组、加密 |
| 缓存 | 浅紫色 | 紫色-600 | 缓存、会话存储 |
| 消息总线 | 浅橙色 | 橙色-600 | 消息队列、事件流 |
| 外部 | 浅石板灰 | 石板灰-500 | 通用、外部系统 |

### 项目结构

```
architecture-diagram/
├── SKILL.md              # 技能指令与设计系统
└── assets/
    └── template.html     # HTML/SVG 基础模板
```

### 工作原理

1. **布局规划** — 创建 ASCII 布局计划，确定组件位置和连接关系
2. **坐标计算** — 使用精确公式计算箭头坐标（多连线使用总线式路由）
3. **SVG 生成** — 按照设计系统生成最终的 HTML/SVG
4. **审查修复** — 系统化检查清单，验证无重叠、路由正确、连接无误
5. **SVG 提取** — 从 HTML 中提取纯 SVG 作为独立的 `.svg` 文件

### 输出

技能生成**两个**自包含文件：

**HTML 文件**（`.html`）— 完整页面，包含：
- 内嵌 CSS 和 SVG
- 摘要卡片和页脚元数据
- 可直接在浏览器中打开

**SVG 文件**（`.svg`）— 独立矢量图：
- 从 HTML 中提取，无 HTML 外壳
- 所有样式和定义内联
- 可嵌入文档、演示文稿或网站
- 任意缩放不失真

两个文件都可以：
- 直接在任何浏览器中打开
- 与团队成员分享
- 嵌入到文档中
- 打印或导出为 PDF
- 部署到任何静态站点

### 致谢

本项目基于 [Cocoon-AI/architecture-diagram-generator](https://github.com/Cocoon-AI/architecture-diagram-generator) 开发。

### 许可证

MIT
