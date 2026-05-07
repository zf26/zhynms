# 毕业旅行协调小应用 — SPEC.md

## 1. Concept & Vision

一款专为毕业室友设计的旅行协商工具，让四个性格迥异、需求不同的人能在一个页面上实时协调旅行计划。它不是冰冷的表单工具，而是一个温暖的小黑板——每个人都能看到彼此的想法，冲突被温柔地标出来，投票清晰地推进共识达成。核心理念：让每个人都感到被倾听，在妥协中找到最好的方案。

## 2. Design Language

- **Aesthetic**: 手账风格 + 轻拟物卡片，温暖友好，像在白板上贴便利贴
- **Color Palette**:
  - Background: `#f5f0e8`（暖米色）
  - Card: `#ffffff`
  - Primary: `#5b8a72`（薄荷绿）
  - Accent: `#e8925a`（珊瑚橙）
  - Danger: `#e05c5c`（柔红）
  - Success: `#5b8a72`
  - Text: `#2d2a26`
  - Muted: `#8a8478`
- **Typography**: `Noto Sans SC` (Google Fonts), fallback `system-ui, sans-serif`
- **Spatial System**: 8px grid, cards with 20px padding, 16px border-radius, subtle shadows
- **Motion**: Smooth 300ms transitions for panels, 200ms for interactions, shake animation for conflicts
- **Icons**: Inline SVG, consistent 20px stroke-based style

## 3. Layout & Structure

### 4-Step Wizard Flow

```
[Step 1: 创建/加入房间] → [Step 2: 填写个人信息] → [Step 3: 协商冲突 & 投票] → [Step 4: 达成共识]
```

- **Header**: 顶部标题栏，显示当前步骤 + 房间号
- **Main Area**: 中央内容区，步骤切换
- **Footer**: 底部操作栏，前进/返回按钮
- **Responsive**: 桌面端 4 列卡片布局，手机端单列堆叠

## 4. Features & Interactions

### Step 1: 创建/加入房间
- 输入房间号（6位字母数字）加入已有房间，或一键创建新房间
- 房间号显示在顶部，可复制分享给室友
- LocalStorage 存储，同一浏览器刷新不丢失

### Step 2: 填写旅行需求
每个人填写：
- 姓名（用于标识）
- 出发城市（下拉选择城市列表）
- 可用时间（日期范围选择）
- 人均预算（数字输入，RMB）
- 兴趣偏好（多选：海边、古城、山水、城市、海岛）
- 优先级标注（哪个需求最重要）
- 禁忌条件（文字描述）

### Step 3: 冲突高亮 & 投票
- 实时检测四大冲突类型：时间、预算、兴趣、出发地
- 冲突以红色边框 + 抖动动画高亮，弹出提示说明冲突内容
- 让步建议以灰色气泡显示，标注"仅参考"
- 发起投票：可选目的地方案（或手动输入方案）
- 实时进度条展示投票状态，支持随时改票

### Step 4: 共识确认
- 得票 ≥ 3 票自动弹出共识确认框
- 显示最终方案（目的地、时间、预算估算、交通建议）
- 标注所有让步点，感谢室友的包容

## 5. Component Inventory

| Component | States |
|-----------|--------|
| RoomCard | default, hover, active, disabled |
| PersonCard | empty, filled, conflict (red border + shake), completed |
| ConflictBadge | hidden, visible (pulse) |
| CompromiseBubble | collapsed, expanded |
| VoteOption | unselected, selected, passed, rejected |
| ProgressBar | 0-4 votes filled |
| StepIndicator | inactive, active, completed |
| Button | default, hover, active, disabled |

## 6. Technical Approach

- **Pure HTML + CSS + JavaScript** (no framework, no build step)
- **Single HTML file** with embedded `<style>` and `<script>`
- **State management**: Plain JS object, re-render on state change
- **Persistence**: LocalStorage for room data
- **Room sharing**: Room ID + URL hash for direct link joining
- **Conflict detection**: Simple rule-based comparison across all members
- **No backend**: All data stored in browser (suitable for local demo/MVP)
