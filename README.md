
# 异星迷航 · 数据查询工具

> 纯静态 · 零依赖 · 本地 JSON 驱动的迷你世界辅助查询系统

本工具专为《异星迷航》（迷你世界老版本 0.7.5）玩家设计，提供物品、状态、食物、工具、附魔、矿石、方块、群系、熔炼、交易、成就等全方位数据查询。  
**所有数据均来自 `data/` 目录下的 JSON 文件**，无需后端，开箱即用。

---

## ✨ 功能概览

- **物品查询** – 按中文名或 ID 搜索，直接复制物品 ID 或生成 `/give` 指令。
- **合成配方** – 查看物品合成所需材料，材料支持点击复制 ID 或生成指令。
- **状态/食物/工具/附魔/矿石** – 详细展示属性、效果、修复材料、生成条件等。
- **方块群系查询** – 查询方块硬度、掉落物、挖掘工具；群系地形、温度、降雨；熔炼配方、交易价格、成就树形图等。
- **主题定制** – 内置 12 种主题色，支持 URL 参数 `?color=#hex&radius=0-25` 嵌入定制。
- **双页面联动** – `index.html`（ID 查询）与 `other.html`（方块/群系）通过按钮互跳，数据独立。

---

## 📁 项目结构



/
├── index.html          # 主页面：物品/状态/食物/工具/附魔/矿石查询
├── other.html          # 第二页面：方块/群系/熔炼/交易/成就查询
├── data/               # 所有 JSON 数据文件（静态）
│   ├── items.json      # 物品数据
│   ├── status.json     # 状态效果
│   ├── food.json       # 食物属性
│   ├── tool.json       # 工具参数
│   ├── enchant.json    # 附魔效果
│   ├── ore.json        # 矿石生成
│   ├── crafting.json   # 合成配方（与 items.json 联动）
│   ├── block.json      # 方块属性
│   ├── biome.json      # 群系特征
│   ├── furnace.json    # 熔炼配方
│   ├── npctrade.json   # 交易数据
│   └── achievement.json# 成就数据（含前置关系）
├── assets/             # 可选资源（如 tb.css）
└── README.md           # 本文档



---

## 🔧 使用说明

### 1. 本地运行
直接双击 `index.html` 在浏览器中打开（支持所有现代浏览器）。  
由于加载本地 JSON，**请勿使用 `file://` 协议导致跨域**，推荐使用 VS Code 的 Live Server 或其他静态服务器。

### 2. 搜索与复制
- 在输入框键入关键词（中文名或数字 ID），实时/点击搜索过滤。
- 每个结果项右侧有按钮：
  - 📋 复制物品 ID
  - ⌨️ 弹出指令生成器（可调整数量）
  - 🔧 查看合成配方（仅物品）

### 3. 切换数据类型
- `index.html` 顶部按钮：物品 | 状态 | 食物 | 工具 | 附魔 | 矿石
- `other.html` 顶部按钮：方块 | 群系 | 熔炼 | 交易 | 成就
- 切换后自动加载对应 JSON 并更新界面标题与提示。

### 4. 主题与圆角
- 点击 🎨 图标可切换 12 种预设主题色，并调整卡片圆角（0–25px）。
- 或通过 URL 参数固定：


index.html?color=%23ec4899&radius=12
other.html?color=%233b82f6&radius=8


  > 注意 `#` 需编码为 `%23`，支持 3/4/6/8 位十六进制。

---

## 🧩 数据格式规范

### items.json
json
[
  { "ID": 0, "Name": "空气", "Desc": "" },
  { "ID": 200, "Name": "果木", "Desc": "果木" }
]


crafting.json

json
[
  {
    "ID": 1,
    "ResultID": 206,
    "ResultName": "果木板",
    "ResultCount": 4,
    "GridX": 2,
    "GridY": 2,
    "Materials": [
      { "MaterialID": 200, "MaterialCount": 1 }
    ]
  }
]


other.html 各类数据字段

· block.json：包含 ID, Name, Hardness, ToolMineDrop1, HandMineDrop, ...（详见源码 renderExtra 函数）。
· biome.json：TypeName, MinHeight, MaxHeight, Heat, Humid, EnableRain, ...
· furnace.json：Result, Heat, Exp, ...
· npctrade.json：NpcID, TradeType, Price, Weight, ...
· achievement.json：ID, Name, Desc, Goal, GridX, GridY, FrontIDs, Rewards, ...

所有 JSON 文件均支持 col_2 作为备用名称字段，Desc 支持颜色代码 #cRRGGBB内容#n 或 #G内容#n。

---

🚀 自定义与扩展

增加新物品/数据

1. 按格式编辑对应 JSON 文件（如 items.json）。
2. 保存后刷新页面即可生效（无需重启服务）。

增加新主题色

修改 index.html 中 .theme-color-option 元素，复制结构并设置 data-color、data-primary、data-light。

增加新查询类型

· 在 index.html 的 version-section 中添加按钮，data-type 自定义。
· 在 loadData() 函数中增加对应 URL 映射，并在 setContentType() 中更新标题/副标题。
· 在 renderExtra() 中添加渲染逻辑。

---

🌐 部署建议

· GitHub Pages / Cloudflare Pages：直接上传仓库即可。
· Nginx / Apache：将整个目录放置于 webroot，配置 CORS 宽松（本地 JSON 无跨域问题）。
· 本地开发：使用 python -m http.server 或 VS Code Live Server。

---

📚 开发文档

页面内置了完整的 开发参考 页面（点击底部 “开发文档”），包含：

· 所有数据接口的请求示例
· URL 主题参数说明
· 数据字段详解
· 代码片段（JavaScript / cURL）

---

📄 版权与致谢

· 数据整理：毗毘个人工作室
· 仅供学习与游戏辅助使用，数据版权归游戏官方所有。
· 欢迎 Fork 和 PR，共同完善数据准确性。

---

🔗 相关链接

· 主页面：index.html
· 副页面：other.html
· 数据目录：data/

---

Made with ❤️ by 毗毘
2026 · 纯静态 · 零依赖

