# Architecture Diagram Generator / 架构图生成器

[English](#english) | [中文](#中文)

---

<a id="english"></a>

## Architecture Diagram Generator

A professional architecture diagram generation skill that creates beautiful, dark-themed system architecture diagrams as standalone HTML files with SVG graphics. Based on [Cocoon-AI/architecture-diagram-generator](https://github.com/Cocoon-AI/architecture-diagram-generator).

### Features

- **Dark Theme** — Slate-950 background with subtle grid pattern
- **Semantic Color Coding** — Consistent colors for frontend, backend, database, cloud, and security components
- **Self-contained Output** — Single HTML file with embedded CSS and inline SVG
- **No Dependencies** — Opens in any modern browser, no JavaScript required
- **Professional Typography** — JetBrains Mono for technical aesthetic
- **Smart Layering** — Arrows render cleanly behind component boxes

### Color Palette

| Component Type | Color | Use For |
|---|---|---|
| Frontend | Cyan | Client apps, UI, edge devices |
| Backend | Emerald | Servers, APIs, services |
| Database | Violet | Databases, storage, AI/ML |
| Cloud/AWS | Amber | Cloud services, infrastructure |
| Security | Rose | Auth, security groups, encryption |
| External | Slate | Generic, external systems |

### Project Structure

```
architecture-diagram/
├── SKILL.md              # Skill instructions & design system
└── assets/
    └── template.html     # Base HTML/SVG template
```

### How It Works

1. **Layout Planning** — Create an ASCII layout plan with component positions and connections
2. **Coordinate Calculation** — Use precise formulas for arrow coordinates
3. **SVG Generation** — Generate the final HTML/SVG following the design system

### Output

The generated diagram is a single self-contained HTML file that you can:

- Open directly in any browser
- Share with teammates
- Include in documentation
- Print or export to PDF
- Host on any static site

### Acknowledgements

This project is based on [Cocoon-AI/architecture-diagram-generator](https://github.com/Cocoon-AI/architecture-diagram-generator).

### License

MIT

---

<a id="中文"></a>

## 架构图生成器

一个专业的架构图生成技能，可以创建精美的深色主题系统架构图，输出为包含 SVG 图形的独立 HTML 文件。基于 [Cocoon-AI/architecture-diagram-generator](https://github.com/Cocoon-AI/architecture-diagram-generator) 开发。

### 特性

- **深色主题** — Slate-950 背景配合细微网格图案
- **语义化配色** — 前端、后端、数据库、云服务和安全组件使用一致的配色方案
- **自包含输出** — 单个 HTML 文件，内嵌 CSS 和 SVG
- **零依赖** — 在任何现代浏览器中打开即可，无需 JavaScript
- **专业排版** — 使用 JetBrains Mono 等宽字体，呈现技术美感
- **智能分层** — 箭头在组件框下方渲染，层次清晰

### 配色方案

| 组件类型 | 颜色 | 用途 |
|---|---|---|
| 前端 | 青色 (Cyan) | 客户端应用、UI、边缘设备 |
| 后端 | 翠绿 (Emerald) | 服务器、API、服务 |
| 数据库 | 紫罗兰 (Violet) | 数据库、存储、AI/ML |
| 云服务 | 琥珀 (Amber) | 云服务、基础设施 |
| 安全 | 玫瑰 (Rose) | 认证、安全组、加密 |
| 外部 | 石板灰 (Slate) | 通用、外部系统 |

### 项目结构

```
architecture-diagram/
├── SKILL.md              # 技能指令与设计系统
└── assets/
    └── template.html     # HTML/SVG 基础模板
```

### 工作原理

1. **布局规划** — 创建 ASCII 布局计划，确定组件位置和连接关系
2. **坐标计算** — 使用精确公式计算箭头坐标
3. **SVG 生成** — 按照设计系统生成最终的 HTML/SVG

### 输出

生成的架构图是一个自包含的 HTML 文件，你可以：

- 直接在任何浏览器中打开
- 与团队成员分享
- 嵌入到文档中
- 打印或导出为 PDF
- 部署到任何静态站点

### 致谢

本项目基于 [Cocoon-AI/architecture-diagram-generator](https://github.com/Cocoon-AI/architecture-diagram-generator) 开发。

### 许可证

MIT
