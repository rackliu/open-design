# Open Design

> **[Claude Design][cd] 的開源替代品。** 本地優先、可部署到 Vercel、每一層都 BYOK —— 你機器上已經裝好的 coding agent（Claude Code、Codex、Cursor Agent、Gemini CLI、OpenCode、Qwen、GitHub Copilot CLI）就是設計引擎，由 **19 個可組合 Skills** 和 **71 套品牌級 Design System** 驅動。

<p align="center">
  <img src="docs/assets/banner.png" alt="Open Design 封面：與本地 AI 智慧體共同設計" width="100%" />
</p>

<p align="center">
  <a href="LICENSE"><img alt="License" src="https://img.shields.io/badge/license-Apache%202.0-blue.svg" /></a>
  <a href="#支援的-coding-agent"><img alt="Agents" src="https://img.shields.io/badge/agents-Claude%20%7C%20Codex%20%7C%20Cursor%20%7C%20Gemini%20%7C%20OpenCode%20%7C%20Qwen%20%7C%20Copilot-black" /></a>
  <a href="#design-system"><img alt="Design systems" src="https://img.shields.io/badge/design%20systems-71-orange" /></a>
  <a href="#內建-skills"><img alt="Skills" src="https://img.shields.io/badge/skills-19-teal" /></a>
  <a href="QUICKSTART.md"><img alt="Quickstart" src="https://img.shields.io/badge/quickstart-3%20commands-green" /></a>
</p>

<p align="center"><a href="README.md">English</a> · <b>簡體中文</b> · <a href="README.ko.md">한국어</a></p>

---

## 為什麼要做這個

Anthropic 的 [Claude Design][cd]（2026-04-17 釋出，基於 Opus 4.7）讓大家第一次看到：當一個 LLM 不再寫廢話、開始直接交付設計成品，會是什麼樣子。它瞬間出圈 —— 然後保持**閉源**、付費、只跑在雲上、繫結 Anthropic 的模型和 Anthropic 的內部 skill。沒有 checkout，沒有自託管，沒有 Vercel 部署，也換不了自己的 agent。

**Open Design（OD）就是它的開源替代品。** 同一套 loop、同一種「artifact-first」心智模型，但沒有鎖定。我們不做 agent —— 你筆記本上最強的 coding agent 已經裝好了。我們要做的，是把它接進一個 skill 驅動的設計工作流：本地用 `pnpm tools-dev` 跑完整本地閉環，雲端可單獨部署 Web 層，每一層都 BYOK（自帶 Key）。

輸入「幫我做一份雜誌風的種子輪 pitch deck」。在模型揮灑第一個畫素之前，**初始化問題表單**已經先跳出來。Agent 從 5 套精挑的視覺方向裡選一個。一張活的 `TodoWrite` 計劃卡片即時流入 UI。Daemon 在磁碟上構建出一個真實的專案目錄，裡面有 seed 模板、佈局庫、自檢 checklist。Agent **強制 pre-flight** 讀取它們，對自己的輸出跑一輪**五維評審**，幾秒後吐出一個 `<artifact>`，渲染在沙盒 iframe 裡。

這不是「AI 試圖做點設計」。這是一個被提示詞棧訓練得像高階設計師一樣工作的 AI —— 有可用的檔案系統、有確定性的色板庫、有 checklist 文化 —— 也就是 Claude Design 立下的那條線，只是這次它開源、歸你。

OD 站在四個開源專案的肩膀上：

- [**`alchaincyf/huashu-design`**（花叔的畫術）](https://github.com/alchaincyf/huashu-design) —— 設計哲學的指南針。Junior-Designer 工作流、5 步品牌資產協議、anti-AI-slop checklist、五維自評審、以及方向選擇器背後的「5 流派 × 20 種設計哲學」思路 —— 全部蒸餾進 [`apps/web/src/prompts/discovery.ts`](apps/web/src/prompts/discovery.ts)。
- [**`op7418/guizang-ppt-skill`**（歸藏的雜誌風 PPT skill）](https://github.com/op7418/guizang-ppt-skill) —— Deck 模式。原樣捆綁在 [`skills/guizang-ppt/`](skills/guizang-ppt/) 下，原 LICENSE 保留；雜誌版式、WebGL hero、P0/P1/P2 checklist。
- [**`OpenCoworkAI/open-codesign`**](https://github.com/OpenCoworkAI/open-codesign) —— UX 北極星，也是我們最接近的同類。第一個開源的 Claude-Design 替代品。我們借鑑了它的流式 artifact 迴圈、沙盒 iframe 預覽模式（自帶 React 18 + Babel）、即時 agent 面板（todos + tool calls + 可中斷生成）、5 種匯出格式列表（HTML / PDF / PPTX / ZIP / Markdown）。我們刻意在形態上分流 —— 它是桌面 Electron 應用，把 [`pi-ai`][piai] 打包進去做 agent；我們是 Web 應用 + 本地 daemon，把 agent 執行時**委託**給你已經裝好的 CLI。
- [**`multica-ai/multica`**](https://github.com/multica-ai/multica) —— Daemon 與執行時架構。PATH 掃描式 agent 檢測，本地 daemon 作為唯一的特權程序，agent-as-teammate 的世界觀。

## 一眼概覽

| | 你拿到的 |
|---|---|
| **支援的 coding agent** | Claude Code · Codex CLI · Cursor Agent · Gemini CLI · OpenCode · Qwen Code · GitHub Copilot CLI · Anthropic API（BYOK 兜底） |
| **內建 design system** | **71 套** —— 2 套手寫起手 + 69 套從 [`awesome-design-md`][acd2] 匯入的產品系統（Linear、Stripe、Vercel、Airbnb、Tesla、Notion、Anthropic、Apple、Cursor、Supabase、Figma…） |
| **內建 skill** | **19 個** —— 原型 / deck / 移動端 / dashboard / pricing / docs / blog / SaaS landing，外加 10 個文件與辦公產物模板（PM 規範、週報、OKR、runbook、看板…） |
| **視覺方向** | 5 套精選流派（Editorial Monocle · Modern Minimal · Tech Utility · Brutalist · Soft Warm），每一套自帶 OKLch 色板 + 字型棧 |
| **裝置外殼** | iPhone 15 Pro · Pixel · iPad Pro · MacBook · Browser Chrome —— 畫素級精確，跨 skill 共享 |
| **Agent 執行時** | 本地 daemon 在你的專案目錄裡 spawn CLI —— agent 擁有真實的 `Read` / `Write` / `Bash` / `WebFetch`，作用在真實磁碟上 |
| **部署目標** | 本地 `pnpm tools-dev` · Vercel Web 層 |
| **License** | Apache-2.0 |

[acd2]: https://github.com/VoltAgent/awesome-design-md

## 效果展示

<table>
<tr>
<td width="50%">
<img src="docs/screenshots/01-entry-view.png" alt="01 · 入口頁" /><br/>
<sub><b>入口頁</b> —— 選 skill、選 design system、寫一行需求。同一個表面服務原型、deck、移動端、dashboard、editorial 頁面所有 mode。</sub>
</td>
<td width="50%">
<img src="docs/screenshots/02-question-form.png" alt="02 · 初始化問題表單" /><br/>
<sub><b>初始化問題表單</b> —— 模型動筆之前，OD 先把需求鎖住：surface、受眾、調性、品牌上下文、規模。30 秒勾選項秒殺 30 分鐘來回返工。</sub>
</td>
</tr>
<tr>
<td width="50%">
<img src="docs/screenshots/03-direction-picker.png" alt="03 · 方向選擇器" /><br/>
<sub><b>方向選擇器</b> —— 使用者沒有品牌上下文時，agent 自動跳第二個表單，5 套精選方向（Monocle / Modern Minimal / Tech Utility / Brutalist / Soft Warm）一個 radio 選完，色板 + 字型棧直接鎖定，沒有 freestyle 空間。</sub>
</td>
<td width="50%">
<img src="docs/screenshots/04-todo-progress.png" alt="04 · 即時 todo 進度" /><br/>
<sub><b>即時 todo 進度</b> —— Agent 的計劃以活卡片形式流入 UI。<code>in_progress</code> → <code>completed</code> 即時切換。使用者能在中途以極低成本介入糾偏。</sub>
</td>
</tr>
<tr>
<td width="50%">
<img src="docs/screenshots/05-preview-iframe.png" alt="05 · 沙盒預覽" /><br/>
<sub><b>沙盒預覽</b> —— 每個 <code>&lt;artifact&gt;</code> 都在乾淨的 srcdoc iframe 裡渲染。可在檔案工作區裡就地編輯；可下載為 HTML / PDF / ZIP。</sub>
</td>
<td width="50%">
<img src="docs/screenshots/06-design-systems-library.png" alt="06 · 71 套 design system 庫" /><br/>
<sub><b>71 套 design system 庫</b> —— 每套產品系統都展示 4 色色卡。點進去看完整的 <code>DESIGN.md</code>、色板網格、live showcase。</sub>
</td>
</tr>
<tr>
<td width="50%">
<img src="docs/screenshots/07-magazine-deck.png" alt="07 · 雜誌風 deck" /><br/>
<sub><b>Deck 模式（guizang-ppt）</b> —— 內建的 <a href="https://github.com/op7418/guizang-ppt-skill"><code>guizang-ppt-skill</code></a> 原樣接入。雜誌版式、WebGL hero 背景、單檔案 HTML 輸出、可導 PDF。</sub>
</td>
<td width="50%">
<img src="docs/screenshots/08-mobile-app.png" alt="08 · 移動端原型" /><br/>
<sub><b>移動端原型</b> —— 畫素級精確的 iPhone 15 Pro chrome（靈動島、狀態列 SVG、Home Indicator）。多屏原型直接複用 <code>/frames/</code> 共享資源，agent 永遠不需要重新畫一遍手機。</sub>
</td>
</tr>
</table>

## 內建 Skills

19 個 skill，每個一個資料夾，都遵循 Claude Code 的 [`SKILL.md`][skill] 規範，併疊加 OD 的 `od:` frontmatter（`mode`、`platform`、`scenario`、`preview`、`design_system`）。

### 示例展示（Showcase examples）

視覺表現最強、最適合上手第一跑的幾條 skill。每條都附帶可直接開啟的 `example.html` —— 不用登入、不用配置，先看產出再下單。

<table>
<tr>
<td width="50%" valign="top">
<a href="skills/dating-web/"><img src="docs/screenshots/skills/dating-web.png" alt="dating-web" /></a><br/>
<sub><b><a href="skills/dating-web/"><code>dating-web</code></a></b> · <i>prototype</i><br/>消費級約會 / 婚戀儀表盤 —— 左側欄、社群動態 ticker、頭部 KPI、30 天雙向匹配柱狀圖，editorial 字型，剋制點綴色。</sub>
</td>
<td width="50%" valign="top">
<a href="skills/digital-eguide/"><img src="docs/screenshots/skills/digital-eguide.png" alt="digital-eguide" /></a><br/>
<sub><b><a href="skills/digital-eguide/"><code>digital-eguide</code></a></b> · <i>template</i><br/>兩頁數字 e-guide —— 封面（標題、作者、TOC 預告）+ 內文跨頁（pull-quote + 步驟列表），創作者 / 生活方式風。</sub>
</td>
</tr>
<tr>
<td width="50%" valign="top">
<a href="skills/email-marketing/"><img src="docs/screenshots/skills/email-marketing.png" alt="email-marketing" /></a><br/>
<sub><b><a href="skills/email-marketing/"><code>email-marketing</code></a></b> · <i>prototype</i><br/>品牌新品釋出郵件 —— 頂部 wordmark、hero 圖、標題鎖排、主 CTA、規格網格。居中單列 + 表格降級，郵件客戶端安全。</sub>
</td>
<td width="50%" valign="top">
<a href="skills/gamified-app/"><img src="docs/screenshots/skills/gamified-app.png" alt="gamified-app" /></a><br/>
<sub><b><a href="skills/gamified-app/"><code>gamified-app</code></a></b> · <i>prototype</i><br/>三屏遊戲化移動 app 原型，黑色舞臺 —— 封面 / 今日任務（XP 緞帶 + 等級條）/ 任務詳情。</sub>
</td>
</tr>
<tr>
<td width="50%" valign="top">
<a href="skills/mobile-onboarding/"><img src="docs/screenshots/skills/mobile-onboarding.png" alt="mobile-onboarding" /></a><br/>
<sub><b><a href="skills/mobile-onboarding/"><code>mobile-onboarding</code></a></b> · <i>prototype</i><br/>三屏移動端引導流 —— splash、價值主張、登入。狀態列、滑動點、主 CTA。</sub>
</td>
<td width="50%" valign="top">
<a href="skills/motion-frames/"><img src="docs/screenshots/skills/motion-frames.png" alt="motion-frames" /></a><br/>
<sub><b><a href="skills/motion-frames/"><code>motion-frames</code></a></b> · <i>prototype</i><br/>單幀 motion 設計 hero，CSS 迴圈動畫 —— 旋轉字環、地球、計時器。可直接交給 HyperFrames 等關鍵幀匯出。</sub>
</td>
</tr>
<tr>
<td width="50%" valign="top">
<a href="skills/social-carousel/"><img src="docs/screenshots/skills/social-carousel.png" alt="social-carousel" /></a><br/>
<sub><b><a href="skills/social-carousel/"><code>social-carousel</code></a></b> · <i>prototype</i><br/>1080×1080 三連社媒輪播圖 —— 三張電影感面板，標題前後呼應，品牌標識、loop 標記。</sub>
</td>
<td width="50%" valign="top">
<a href="skills/sprite-animation/"><img src="docs/screenshots/skills/sprite-animation.png" alt="sprite-animation" /></a><br/>
<sub><b><a href="skills/sprite-animation/"><code>sprite-animation</code></a></b> · <i>prototype</i><br/>畫素 / 8-bit 動畫直譯器單幀 —— 米白通屏、畫素吉祥物、動感日文標題、迴圈 CSS keyframes，可直接錄屏成豎版影片。</sub>
</td>
</tr>
</table>

### 設計交付類

| Skill | Mode | 預設場景 | 產出 |
|---|---|---|---|
| [`web-prototype`](skills/web-prototype/) | prototype | 桌面 | 單頁 HTML —— landing、營銷、hero |
| [`saas-landing`](skills/saas-landing/) | prototype | 桌面 | hero / features / pricing / CTA 營銷版式 |
| [`dashboard`](skills/dashboard/) | prototype | 桌面 | 帶側欄 + 資料密集型的後臺 |
| [`pricing-page`](skills/pricing-page/) | prototype | 桌面 | 獨立定價頁 + 對比表 |
| [`docs-page`](skills/docs-page/) | prototype | 桌面 | 三欄文件版式 |
| [`blog-post`](skills/blog-post/) | prototype | 桌面 | 長文 editorial |
| [`mobile-app`](skills/mobile-app/) | prototype | 移動 | 帶 iPhone 15 Pro / Pixel 外殼的 app 屏 |
| [`simple-deck`](skills/simple-deck/) | deck | 桌面 | 極簡橫滑 deck |
| [`guizang-ppt`](skills/guizang-ppt/) | deck | **deck 預設** | 雜誌風網頁 PPT —— 來自 [op7418/guizang-ppt-skill][guizang] |

### 文件與辦公產物類

| Skill | Mode | 產出 |
|---|---|---|
| [`pm-spec`](skills/pm-spec/) | template | PM 規範文件 + 目錄 + 決策日誌 |
| [`weekly-update`](skills/weekly-update/) | template | 團隊週報：進度 / 阻塞 / 下一步 |
| [`meeting-notes`](skills/meeting-notes/) | template | 會議決策紀要 |
| [`eng-runbook`](skills/eng-runbook/) | template | 故障 runbook |
| [`finance-report`](skills/finance-report/) | template | 高管財務摘要 |
| [`hr-onboarding`](skills/hr-onboarding/) | template | 崗位入職計劃 |
| [`invoice`](skills/invoice/) | template | 單頁發票 |
| [`kanban-board`](skills/kanban-board/) | template | 看板快照 |
| [`team-okrs`](skills/team-okrs/) | template | OKR 計分表 |

新增一個 skill 就是新增一個資料夾。讀 [`docs/skills-protocol.md`](docs/skills-protocol.md) 瞭解擴充套件 frontmatter，fork 一個現有 skill，重啟 daemon 即生效。

## 六個底層設計

### 1 · 我們不帶 agent，你的就夠好

Daemon 啟動時掃 `PATH`，找 [`claude`](https://docs.anthropic.com/en/docs/claude-code)、[`codex`](https://github.com/openai/codex)、[`cursor-agent`](https://www.cursor.com/cli)、[`gemini`](https://github.com/google-gemini/gemini-cli)、[`opencode`](https://opencode.ai/)、[`qwen`](https://github.com/QwenLM/qwen-code)、[`copilot`](https://github.com/features/copilot/cli)。哪個在就用哪個 —— 透過 stdio 驅動，每個 CLI 一個 adapter。靈感來自 [`multica`](https://github.com/multica-ai/multica) 和 [`cc-switch`](https://github.com/farion1231/cc-switch)。一個 CLI 都沒有？`Anthropic API · BYOK` 就是同一條管線減去 spawn。

### 2 · Skill 是檔案，不是外掛

遵循 Claude Code [`SKILL.md` 規範](https://docs.anthropic.com/en/docs/claude-code/skills)，每個 skill = `SKILL.md` + `assets/` + `references/`。把一個資料夾丟進 [`skills/`](skills/)，重啟 daemon，picker 裡就能看到。內建的 `magazine-web-ppt` 就是 [`op7418/guizang-ppt-skill`](https://github.com/op7418/guizang-ppt-skill) **原樣**捆綁 —— 原 LICENSE 保留、原作者歸屬保留。

### 3 · Design System 是可移植的 Markdown，不是 theme JSON

[`VoltAgent/awesome-design-md`][acd2] 的 9 段式 `DESIGN.md` —— color、typography、spacing、layout、components、motion、voice、brand、anti-patterns。每個 artifact 都從啟用的 system 裡讀 token。切換 system → 下一次渲染就用新的 token。下拉框裡現成的有：**Linear、Stripe、Vercel、Airbnb、Tesla、Notion、Apple、Anthropic、Cursor、Supabase、Figma、Resend、Raycast、Lovable、Cohere、Mistral、ElevenLabs、X.AI、Spotify、Webflow、Sanity、PostHog、Sentry、MongoDB、ClickHouse、Cal、Replicate、Clay、Composio…** 共 71 套。

### 4 · 初始化問題表單幹掉 80% 的來回返工

OD 的提示詞棧把 `RULE 1` 寫死了：每個新設計任務都從 `<question-form id="discovery">` 開始，**不是程式碼**。Surface · 受眾 · 調性 · 品牌上下文 · 規模 · 約束。一段寫得很長的需求裡仍然有大量留白：視覺調性、色彩立場、規模 —— 而表單恰恰把這些用 30 秒勾選項鎖死。錯方向的代價是一輪對話，不是一份做完的 deck。

這就是從 [`huashu-design`](https://github.com/alchaincyf/huashu-design) 蒸餾出來的 **Junior-Designer 模式**：開工前一次性批次問完，儘早 show 出一些可見的東西（哪怕只是灰色方塊的 wireframe），讓使用者用最低成本介入糾偏。再疊加品牌資產協議（定位 · 下載 · `grep` hex · 寫 `brand-spec.md` · 複述），這是輸出從「AI freestyle」跳到「先看資料再畫圖的設計師」最關鍵的一步。

### 5 · Daemon 讓 agent 感覺自己就在你筆記本上 —— 因為它就是

Daemon `spawn` CLI 時，`cwd` 設到該專案在 `.od/projects/<id>/` 下的 artifact 資料夾。Agent 拿到的 `Read` / `Write` / `Bash` / `WebFetch` 都是真工具，作用在真檔案系統上。它能 `Read` skill 的 `assets/template.html`，能 `grep` 你的 CSS 拿 hex，能寫一份 `brand-spec.md`，能落地生成的圖片，能產出 `.pptx` / `.zip` / `.pdf` —— 這些檔案在 turn 結束的時候作為下載 chip 出現在檔案工作區裡。Session、對話、訊息、tab 都持久化在本地 SQLite 裡 —— 明天再開啟這個專案，agent 的 todo 卡片還在你昨天停下的地方。

### 6 · 提示詞棧本身就是產品

傳送時拼裝的不是「system + user」。它是：

```
DISCOVERY 指令         （turn-1 表單、turn-2 品牌分支、TodoWrite、五維評審）
  + 身份與工作流憲章   （OFFICIAL_DESIGNER_PROMPT、anti-AI-slop、Junior Designer 模式）
  + 啟用的 DESIGN.md   （71 套備選）
  + 啟用的 SKILL.md    （19 套備選）
  + 專案元資料          （kind、fidelity、speakerNotes、animations、靈感 system id）
  + Skill 副檔案       （自動注入 pre-flight：先讀 assets/template.html + references/*.md）
  + （deck kind 且無 skill 種子時） DECK_FRAMEWORK_DIRECTIVE   （nav / counter / scroll / print）
```

每一層都可組合。每一層都是一個你能改的檔案。看 [`apps/web/src/prompts/system.ts`](apps/web/src/prompts/system.ts) 和 [`apps/web/src/prompts/discovery.ts`](apps/web/src/prompts/discovery.ts) 就知道真實契約長什麼樣。

## 技術架構

```
┌────────────────────────── 瀏覽器 ──────────────────────────────┐
│                                                                │
│   Next.js 16 App Router  （chat · 檔案工作區 · iframe 預覽）   │
│                                                                │
└──────────────┬───────────────────────────────────┬─────────────┘
               │ /api/* （dev 走 rewrites）        │ direct (BYOK)
               ▼                                   ▼
   ┌──────────────────────┐              ┌──────────────────────┐
   │   本地 daemon         │              │   Anthropic SDK      │
   │   （Express + SQLite）│              │   （瀏覽器兜底）      │
   │                      │              └──────────────────────┘
   │   /api/agents        │
   │   /api/skills        │
   │   /api/design-systems│
   │   /api/projects/...  │
   │   /api/chat (SSE)    │
   │                      │
   └─────────┬────────────┘
             │ spawn(cli, [...], { cwd: .od/projects/<id> })
             ▼
   ┌────────────────────────────────────────────────────────────────────┐
   │  claude · codex · cursor-agent · gemini · opencode · qwen · copilot│
   │  讀 SKILL.md + DESIGN.md，把 artifact 寫到磁碟                     │
   └────────────────────────────────────────────────────────────────────┘
```

| 層 | 技術棧 |
|---|---|
| 前端 | Next.js 16 App Router + React 18 + TypeScript |
| Daemon | Node 24 · Express · SSE 流 · `better-sqlite3` 存專案/對話/訊息/tab |
| Agent 傳輸層 | `child_process.spawn`，Claude Code 走 `claude-stream-json` 解析器、Copilot CLI 走 `copilot-stream-json`，其餘走 line-buffered plain stdout |
| 儲存 | 純檔案 `.od/projects/<id>/` + SQLite `.od/app.sqlite`（已 gitignore，daemon 啟動自建） |
| 預覽 | 沙盒 iframe（`srcdoc`）+ 每個 skill 的 `<artifact>` parser |
| 匯出 | HTML（內聯資源）· PDF（瀏覽器列印）· PPTX（skill 自定義）· ZIP（archiver） |

## Quickstart

```bash
git clone https://github.com/nexu-io/open-design.git
cd open-design
corepack enable
corepack pnpm --version   # 應輸出 10.33.2
pnpm install
pnpm tools-dev run web
# 開啟 tools-dev 輸出的 web URL
```

環境要求：Node `~24`，pnpm `10.33.x`。`nvm` / `fnm` 只是可選輔助工具，不是專案必需步驟；如果使用它們，先執行 `nvm install 24 && nvm use 24` 或 `fnm install 24 && fnm use 24`，再執行 `pnpm install`。

桌面端/後臺啟動、固定埠重啟，以及 media 生成派發器檢查（`OD_BIN`、`OD_DAEMON_URL`、`apps/daemon/dist/cli.js`）見 [`QUICKSTART.md`](QUICKSTART.md)。

第一次載入會：

1. 檢測你 `PATH` 上有哪些 agent CLI，自動選一個。
2. 載入 19 個 skill + 71 套 design system。
3. 彈歡迎對話方塊，讓你貼 Anthropic key（僅 BYOK 兜底路徑需要）。
4. **自動建立 `./.od/`** —— 本地執行時目錄，存放 SQLite 專案庫、各專案工作區、儲存下來的 artifact。**沒有** `od init` 這一步，daemon 啟動時會自己 `mkdir`。

輸入需求，回車，看 question form 跳出來，填，看 todo 卡片流動，看 artifact 渲染。點 **Save to disk** 或匯出整個專案 ZIP。

### 第一次跑起來（`./.od/` 解釋）

Daemon 在倉庫根下維護一個隱藏目錄，裡面所有內容都已 gitignore，純本機資料，**不要** commit。

```
.od/
├── app.sqlite                 ← 專案 · 對話 · 訊息 · 開啟的 tab
├── artifacts/                 ← Save to disk 一次性渲染（帶時間戳）
└── projects/<id>/             ← 每個專案的工作目錄，也是 agent 的 cwd
```

| 想做什麼 | 怎麼做 |
|---|---|
| 看一眼裡面有啥 | `ls -la .od && sqlite3 .od/app.sqlite '.tables'` |
| 完全清空，從零再來 | `pnpm tools-dev stop`，再 `rm -rf .od`，然後重新 `pnpm tools-dev run web` |
| 換到別的位置 | 暫不支援 —— 路徑是相對倉庫根寫死的 |

完整檔案地圖、指令碼、排錯 → [`QUICKSTART.md`](QUICKSTART.md)。

## 倉庫結構

```
open-design/
├── README.md                      ← 英文
├── README.zh-CN.md                ← 本檔案
├── QUICKSTART.md                  ← 跑 / 構建 / 部署
├── package.json                   ← 單 bin: od
│
├── apps/
│   ├── daemon/                    ← Node + Express，唯一的服務端
│   │   ├── src/                   ← TypeScript daemon 原始碼
│   │   │   ├── cli.ts             ← `od` bin 原始碼，編譯到 dist/cli.js
│   │   │   ├── server.ts          ← /api/* 路由（projects、chat、files、exports）
│   │   │   ├── agents.ts          ← PATH 掃描器 + 各 CLI 的 argv 拼裝
│   │   │   ├── claude-stream.ts   ← Claude Code stdout 流式 JSON 解析
│   │   │   ├── skills.ts          ← SKILL.md frontmatter 載入器
│   │   │   └── db.ts              ← SQLite schema（projects/messages/templates/tabs）
│   │   ├── sidecar/               ← tools-dev daemon sidecar wrapper
│   │   └── tests/                 ← daemon 包測試
│   │
│   └── web/                       ← Next.js 16 App Router + React 客戶端
│       ├── app/                   ← App Router 入口
│       ├── next.config.ts         ← dev rewrites + 生產 out/ 靜態匯出
│       └── src/                   ← React + TS 客戶端模組
│           ├── App.tsx            ← 路由、bootstrap、設定
│           ├── components/        ← chat、composer、picker、preview、sketch…
│           ├── prompts/           ← system、discovery、directions、deck framework
│           ├── artifacts/         ← streaming <artifact> parser + manifest
│           ├── runtime/           ← iframe srcdoc、markdown、匯出輔助
│           ├── providers/         ← daemon SSE + BYOK API 傳輸
│           └── state/             ← localStorage + daemon-backed 專案狀態
│
├── e2e/                           ← Playwright UI + 外部整合/Vitest harness
│
├── packages/
│   ├── contracts/                 ← web/daemon 共享 app contracts
│   ├── sidecar-proto/             ← Open Design sidecar protocol contract
│   ├── sidecar/                   ← 通用 sidecar runtime primitives
│   └── platform/                  ← 通用 process/platform primitives
│
├── skills/                        ← 19 個 SKILL.md skill 包
│   ├── web-prototype/             ← 原型預設
│   ├── saas-landing/              ← 營銷頁（hero / features / pricing / CTA）
│   ├── dashboard/                 ← 後臺 / 資料看板
│   ├── pricing-page/              ← 獨立定價頁 + 對比
│   ├── docs-page/                 ← 三欄文件
│   ├── blog-post/                 ← 長文 editorial
│   ├── mobile-app/                ← 帶手機外殼的 app 屏
│   ├── simple-deck/               ← 極簡橫滑 deck
│   ├── guizang-ppt/               ← 內建 magazine-web-ppt（deck 預設）
│   │   ├── SKILL.md
│   │   ├── assets/template.html   ← seed
│   │   └── references/{themes,layouts,components,checklist}.md
│   ├── pm-spec/                   ← PM 規範文件
│   ├── weekly-update/             ← 團隊週報
│   ├── meeting-notes/             ← 會議紀要
│   ├── eng-runbook/               ← 故障 / runbook
│   ├── finance-report/            ← 財務摘要
│   ├── hr-onboarding/             ← 入職計劃
│   ├── invoice/                   ← 單頁發票
│   ├── kanban-board/              ← 看板快照
│   ├── mobile-onboarding/         ← 多屏移動流
│   └── team-okrs/                 ← OKR 計分表
│
├── design-systems/                ← 71 套 DESIGN.md
│   ├── default/                   ← Neutral Modern（起手）
│   ├── warm-editorial/            ← Warm Editorial（起手）
│   ├── linear-app/  vercel/  stripe/  airbnb/  notion/  cursor/  apple/  …
│   └── README.md
│
├── assets/
│   └── frames/                    ← 跨 skill 共享裝置外殼
│       ├── iphone-15-pro.html
│       ├── android-pixel.html
│       ├── ipad-pro.html
│       ├── macbook.html
│       └── browser-chrome.html
│
├── templates/
│   └── deck-framework.html        ← deck 基線（nav / counter / print）
│
├── scripts/
│   └── sync-design-systems.ts     ← 從上游 awesome-design-md tarball 重新匯入
│
├── docs/
│   ├── spec.md                    ← 產品定義、場景、差異化
│   ├── architecture.md            ← 拓撲、資料流、元件
│   ├── skills-protocol.md         ← 擴充套件 SKILL.md 的 od: frontmatter
│   ├── agent-adapters.md          ← 各 CLI 檢測 + 派發
│   ├── modes.md                   ← prototype / deck / template / design-system
│   ├── references.md              ← 詳盡的引用與師承
│   ├── roadmap.md                 ← 分階段交付
│   ├── schemas/                   ← JSON schema
│   └── examples/                  ← 標準 artifact 樣例
│
└── .od/                           ← 執行時資料，已 gitignore，daemon 啟動自建
    ├── app.sqlite                 ← 專案 / 對話 / 訊息 / tab
    ├── projects/<id>/             ← 每個專案的工作目錄（agent 的 cwd）
    └── artifacts/                 ← 單次儲存的 artifact
```

## Design System

<p align="center">
  <img src="docs/assets/design-systems-library.png" alt="71 套 Design Systems 庫 — 編輯版式雙頁" width="100%" />
</p>

71 套開箱即用，每套一個 [`DESIGN.md`](design-systems/README.md)：

<details>
<summary><b>完整目錄</b>（點選展開）</summary>

**AI & LLM** —— `claude` · `cohere` · `mistral-ai` · `minimax` · `together-ai` · `replicate` · `runwayml` · `elevenlabs` · `ollama` · `x-ai`

**開發者工具** —— `cursor` · `vercel` · `linear-app` · `framer` · `expo` · `clickhouse` · `mongodb` · `supabase` · `hashicorp` · `posthog` · `sentry` · `warp` · `webflow` · `sanity` · `mintlify` · `lovable` · `composio` · `opencode-ai` · `voltagent`

**生產力** —— `notion` · `figma` · `miro` · `airtable` · `superhuman` · `intercom` · `zapier` · `cal` · `clay` · `raycast`

**金融科技** —— `stripe` · `coinbase` · `binance` · `kraken` · `mastercard` · `revolut` · `wise`

**電商 / 出行** —— `shopify` · `airbnb` · `uber` · `nike` · `starbucks` · `pinterest`

**媒體** —— `spotify` · `playstation` · `wired` · `theverge` · `meta`

**汽車** —— `tesla` · `bmw` · `ferrari` · `lamborghini` · `bugatti` · `renault`

**其他** —— `apple` · `ibm` · `nvidia` · `vodafone` · `sentry` · `resend` · `spacex`

**起手** —— `default`（Neutral Modern）· `warm-editorial`

</details>

整個庫透過 [`scripts/sync-design-systems.ts`](scripts/sync-design-systems.ts) 從 [`VoltAgent/awesome-design-md`][acd2] 匯入。重新執行即可重新整理。

## 視覺方向

當用戶沒有品牌資產時，agent 會跳第二個表單，5 套精選方向 —— 這是 [`huashu-design` 的「設計方向顧問 · 5 流派 × 20 種設計哲學」 fallback](https://github.com/alchaincyf/huashu-design#%E8%AE%BE%E8%AE%A1%E6%96%B9%E5%90%91%E9%A1%BE%E9%97%AE-fallback) 在 OD 裡的落地。每一套都是確定性 spec —— OKLch 色板、字型棧、版式姿態、參考列表 —— agent 直接把它**原樣**綁進 seed 模板的 `:root`。一個 radio 選完，整套視覺系統全部鎖定。零 freestyle，零 AI slop。

| 方向 | 調性 | 參考 |
|---|---|---|
| Editorial — Monocle / FT | 印刷雜誌，墨水 + 米色紙 + 暖紅強調 | Monocle · FT Weekend · NYT Magazine |
| Modern minimal — Linear / Vercel | 冷調、結構化、剋制強調 | Linear · Vercel · Stripe |
| Tech utility | 資訊密度、等寬、終端感 | Bloomberg · Bauhaus 工具 |
| Brutalist | 粗糲、巨字、無陰影、刺眼強調 | Bloomberg Businessweek · Achtung |
| Soft warm | 大方、低對比、桃色中性 | Notion 營銷頁 · Apple Health |

完整 spec → [`apps/web/src/prompts/directions.ts`](apps/web/src/prompts/directions.ts)。

## 反 AI Slop 機制

下面整套機制都是 [`huashu-design`](https://github.com/alchaincyf/huashu-design) 的 playbook，被移植進 OD 的提示詞棧，並透過 skill 副檔案 pre-flight 讓每個 skill 都能落地執行。看 [`apps/web/src/prompts/discovery.ts`](apps/web/src/prompts/discovery.ts) 是真實文案：

- **先表單。** Turn 1 必須是 `<question-form>`，**不準** thinking、不準 tools、不準旁白。使用者用 radio 速度選預設。
- **品牌資產協議。** 使用者貼截圖或 URL 時，agent 走 5 步流程（定位 · 下載 · grep hex · 寫 `brand-spec.md` · 複述）才能開始寫 CSS。**絕不從記憶裡猜品牌色**。
- **五維評審。** 在吐 `<artifact>` 之前，agent 默默給自己 1–5 分打分，五個維度：哲學 / 層級 / 執行 / 具體度 / 剋制。任一維 < 3/5 視為退步 —— 修完再評。兩輪是常態。
- **P0/P1/P2 checklist。** 每個 skill 都自帶 `references/checklist.md`，含硬性 P0。Agent 必須 P0 全過才能 emit。
- **Slop 黑名單。** 暴力紫漸變、通用 emoji 圖示、左 border 圓角卡片、手繪 SVG 真人臉、Inter 當 *display* 字型、自編指標 —— 提示詞裡全部明令禁止。
- **誠實佔位 > 假資料。** Agent 沒真數字時寫 `—` 或一個標註的灰塊，絕不寫「快 10 倍」。

## 橫向對比

| 維度 | [Claude Design][cd]（Anthropic） | [Open CoDesign][ocod] | **Open Design** |
|---|---|---|---|
| License | 閉源 | MIT | **Apache-2.0** |
| 形態 | Web (claude.ai) | 桌面 (Electron) | **Web 應用 + 本地 daemon** |
| 可部署 Vercel | ❌ | ❌ | **✅** |
| Agent 執行時 | 內建 (Opus 4.7) | 內建 ([`pi-ai`][piai]) | **委託給使用者已裝好的 CLI** |
| Skill | 私有 | 12 套自定義 TS 模組 + `SKILL.md` | **19 套基於檔案的 [`SKILL.md`][skill]，可丟入** |
| Design system | 私有 | `DESIGN.md`（v0.2 路線圖） | **`DESIGN.md` × 71 套，開箱即有** |
| Provider 靈活度 | 僅 Anthropic | 7+（[`pi-ai`][piai]） | **取決於你的 agent** |
| 初始化問題表單 | ❌ | ❌ | **✅ 硬規則 turn 1** |
| 方向選擇器 | ❌ | ❌ | **✅ 5 套確定性方向** |
| 即時 todo 進度 + tool 流 | ❌ | ✅ | **✅**（UX 模式來自 open-codesign） |
| 沙盒 iframe 預覽 | ❌ | ✅ | **✅**（模式來自 open-codesign） |
| 評論模式手術刀編輯 | ❌ | ✅ | 🚧 路線圖（移植自 open-codesign） |
| AI 自吐 tweaks 面板 | ❌ | ✅ | 🚧 路線圖（移植自 open-codesign） |
| 檔案系統級工作區 | ❌ | 部分（Electron 沙盒） | **✅ 真 cwd、真工具、SQLite 持久化** |
| 五維自評審 | ❌ | ❌ | **✅ Emit 前必跑** |
| 匯出格式 | 受限 | HTML / PDF / PPTX / ZIP / Markdown | **HTML / PDF / PPTX / ZIP / Markdown** |
| PPT skill 複用 | N/A | 內建 | **[`guizang-ppt-skill`][guizang] 直接接入** |
| 計費門檻 | Pro / Max / Team | BYOK | **BYOK** |

[cd]: https://x.com/claudeai/status/2045156267690213649
[ocod]: https://github.com/OpenCoworkAI/open-codesign
[piai]: https://github.com/mariozechner/pi-ai
[acd]: https://github.com/VoltAgent/awesome-claude-design
[guizang]: https://github.com/op7418/guizang-ppt-skill
[skill]: https://docs.anthropic.com/en/docs/claude-code/skills

## 支援的 Coding Agent

Daemon 啟動時從 `PATH` 自動檢測，無需配置。

| Agent | 二進位制 | 流式 | 備註 |
|---|---|---|---|
| [Claude Code](https://docs.anthropic.com/en/docs/claude-code) | `claude` | `--output-format stream-json`（型別化事件） | 一等公民，最佳保真度 |
| [Codex CLI](https://github.com/openai/codex) | `codex` | line-buffered | `codex exec <prompt>` |
| [Cursor Agent](https://www.cursor.com/cli) | `cursor-agent` | line-buffered | `cursor-agent -p` |
| [Gemini CLI](https://github.com/google-gemini/gemini-cli) | `gemini` | line-buffered | `gemini -p` |
| [OpenCode](https://opencode.ai/) | `opencode` | line-buffered | `opencode run` |
| [Qwen Code](https://github.com/QwenLM/qwen-code) | `qwen` | line-buffered | `qwen -p` |
| [GitHub Copilot CLI](https://github.com/features/copilot/cli) | `copilot` | `--output-format json`（型別化事件） | `copilot -p <prompt> --allow-all-tools --output-format json` |
| Anthropic API · BYOK | n/a | SSE 直連 | 沒裝任何 CLI 時的瀏覽器兜底 |

加一個新 CLI = 在 [`apps/daemon/src/agents.ts`](apps/daemon/src/agents.ts) 里加一項。流式格式從 `claude-stream-json`（型別化事件）和 `plain`（原始文字）兩種裡選一個。

## 引用與師承

每一個被借鑑的開源專案都列在這裡。點連結可以驗證師承。

| 專案 | 在這裡的角色 |
|---|---|
| [`Claude Design`][cd] | 本倉庫為之提供開源替代的閉源產品。 |
| [**`alchaincyf/huashu-design`**（花叔的畫術）](https://github.com/alchaincyf/huashu-design) | 設計哲學的核心。Junior-Designer 工作流、5 步品牌資產協議、anti-AI-slop checklist、五維自評審、以及方向選擇器背後的「5 流派 × 20 種設計哲學」庫 —— 全部蒸餾進 [`apps/web/src/prompts/discovery.ts`](apps/web/src/prompts/discovery.ts) 與 [`apps/web/src/prompts/directions.ts`](apps/web/src/prompts/directions.ts)。 |
| [**`op7418/guizang-ppt-skill`**（歸藏）][guizang] | Magazine-web-PPT skill 原樣捆綁在 [`skills/guizang-ppt/`](skills/guizang-ppt/) 下，原 LICENSE 保留。Deck 模式預設。P0/P1/P2 checklist 文化也被借給了所有其他 skill。 |
| [**`multica-ai/multica`**](https://github.com/multica-ai/multica) | Daemon + adapter 架構。PATH 掃描式 agent 檢測、本地 daemon 作為唯一特權程序、agent-as-teammate 世界觀。我們採納模型，不 vendor 程式碼。 |
| [**`OpenCoworkAI/open-codesign`**][ocod] | 第一個開源的 Claude-Design 替代品，也是我們最接近的同類。已採納的 UX 模式：流式 artifact 迴圈、沙盒 iframe 預覽（自帶 React 18 + Babel）、即時 agent 面板（todos + tool calls + 可中斷）、5 種匯出格式列表（HTML/PDF/PPTX/ZIP/Markdown）、本地優先的 designs hub、`SKILL.md` 品味注入。路線圖上的 UX 模式：評論模式手術刀編輯、AI 自吐 tweaks 面板。**我們刻意不 vendor [`pi-ai`][piai]** —— open-codesign 把它打包成 agent 執行時；我們則委託給使用者已經裝好的 CLI。 |
| [`VoltAgent/awesome-claude-design`][acd] / [`awesome-design-md`][acd2] | 9 段式 `DESIGN.md` schema 的來源，69 套產品系統透過 [`scripts/sync-design-systems.ts`](scripts/sync-design-systems.ts) 匯入。 |
| [`farion1231/cc-switch`](https://github.com/farion1231/cc-switch) | 跨多個 agent CLI 的 symlink 式 skill 分發靈感來源。 |
| [Claude Code skills][skill] | `SKILL.md` 規範原樣採納 —— 任何 Claude Code skill 丟進 `skills/` 都能被 daemon 識別。 |

詳盡的師承說明（每一項我們採納了什麼、刻意沒采納什麼）在 [`docs/references.md`](docs/references.md)。

## Roadmap

- [x] Daemon + agent 檢測 + skill registry + design-system 目錄
- [x] Web 應用 + 對話 + question form + todo progress + 沙盒預覽
- [x] 19 個 skill + 71 套 design system + 5 套視覺方向 + 5 個裝置外殼
- [x] SQLite 後端的 projects · conversations · messages · tabs · templates
- [ ] 評論模式手術刀編輯（點元素 → 指令 → 區域性 patch）—— 模式來自 [`open-codesign`][ocod]
- [ ] AI 自吐 tweaks 面板（模型自己丟擲值得調的引數）—— 模式來自 [`open-codesign`][ocod]
- [ ] Vercel + 隧道部署食譜（Topology B）
- [ ] 一行 `npx od init` 腳手架帶 `DESIGN.md`
- [ ] Skill 市場（`od skills install <github-repo>`）

分階段交付計劃在 [`docs/roadmap.md`](docs/roadmap.md)。

## 專案狀態

這是一個早期實現 —— 閉環（檢測 → 選 skill + design system → 對話 → 解析 `<artifact>` → 預覽 → 儲存）已經端到端跑通。提示詞棧和 skill 庫是價值最重的部分，目前已穩定。元件級 UI 仍在每天迭代。

## 給我們點個 Star

<p align="center">
  <a href="https://github.com/nexu-io/open-design"><img src="docs/assets/star-us.png" alt="給 Open Design 點個 Star —— github.com/nexu-io/open-design" width="100%" /></a>
</p>

如果這套東西幫你省了半小時，給它一個 ★。Star 不付房租，但它告訴下一個設計師、Agent 和貢獻者：這個實驗值得他們的注意力。一次點選、三秒鐘、真實訊號：[github.com/nexu-io/open-design](https://github.com/nexu-io/open-design)。

## 貢獻

歡迎 issue、PR、新 skill、新 design system。收益最高的貢獻往往就是一個資料夾、一份 Markdown，或者一個 PR 大小的 adapter：

- **加一個 skill** —— 往 [`skills/`](skills/) 丟一個資料夾，遵循 [`SKILL.md`][skill] 規範。
- **加一套 design system** —— 往 [`design-systems/<brand>/`](design-systems/) 丟一份 `DESIGN.md`，用 9 段式 schema。
- **接入一個新的 coding-agent CLI** —— 在 [`apps/daemon/src/agents.ts`](apps/daemon/src/agents.ts) 里加一項。

完整流程、合併硬線、程式碼風格、我們不接收的 PR 型別 → [`CONTRIBUTING.zh-CN.md`](CONTRIBUTING.zh-CN.md)（[English](CONTRIBUTING.md)）。

## License

Apache-2.0。內建的 [`skills/guizang-ppt/`](skills/guizang-ppt/) 保留它原始的 [LICENSE](skills/guizang-ppt/LICENSE)（MIT）和原作者 [op7418](https://github.com/op7418) 的歸屬。
