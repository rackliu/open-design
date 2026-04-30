# 貢獻指南 · Contributing to Open Design

謝謝你願意參與。OD 是有意做小的 —— 大部分價值在 **檔案** 裡（skill、design system、提示詞片段），而不是框架程式碼。這意味著收益最高的貢獻往往就是一個資料夾、一份 Markdown，或者一個 PR 大小的 adapter。

這份指南會告訴你：每種貢獻該往哪裡看、合併之前 PR 需要過哪些線。

<p align="center"><a href="CONTRIBUTING.md">English</a> · <b>簡體中文</b></p>

---

## 一個下午就能交付的三件事

| 你想要…… | 你其實在加的是 | 它住在哪 | 體量 |
|---|---|---|---|
| 讓 OD 渲染一種新的 artifact（一份發票、一個 iOS 設定頁、一張 one-pager……） | 一個 **Skill** | [`skills/<your-skill>/`](skills/) | 一個資料夾，約 2 個檔案 |
| 讓 OD 說一種新品牌的視覺語言 | 一套 **Design System** | [`design-systems/<brand>/DESIGN.md`](design-systems/) | 一個 Markdown 檔案 |
| 接入一個新的 coding-agent CLI | 一個 **Agent adapter** | [`apps/daemon/src/agents.ts`](apps/daemon/src/agents.ts) | 一個數組裡 ~10 行 |
| 加功能、修 bug、從 [`open-codesign`][ocod] 移植一個 UX 模式 | 程式碼 | `apps/web/src/`、`apps/daemon/` | 普通 PR |
| 改文件、補中文翻譯、修錯別字 | 文件 | `README.md`、`README.zh-CN.md`、`docs/`、`QUICKSTART.md` | 一個 PR |

不確定自己想做的屬於哪一桶？[先開 issue / discussion](https://github.com/nexu-io/open-design/issues/new)，我們告訴你該改哪個面。

---

## 本地起跑

完整的一頁式 setup 在 [`QUICKSTART.md`](QUICKSTART.md)。給貢獻者的 TL;DR：

```bash
git clone https://github.com/nexu-io/open-design.git
cd open-design
corepack enable           # 使用 packageManager 固定的 pnpm
pnpm install
pnpm tools-dev run web    # daemon + web 前臺閉環
pnpm typecheck            # tsc -b --noEmit
pnpm build                # 生產構建
```

要求 Node `~24` 和 pnpm `10.33.x`。`nvm` / `fnm` 是可選路徑；如果你習慣用它們，先執行 `nvm install 24 && nvm use 24` 或 `fnm install 24 && fnm use 24`。macOS、Linux、WSL2 是主要路徑。Windows 原生應該能跑但不是主要目標 —— 跑不起來請開 issue。

**開發 OD 本身不需要在 `PATH` 上裝任何 agent CLI** —— daemon 會告訴你「找不到 agent」並落到 **Anthropic API · BYOK** 路徑，反而是最快的開發迴圈。

---

## 加一個 Skill

一個 skill 就是 [`skills/`](skills/) 下的一個資料夾，根目錄放一個 `SKILL.md`，遵循 Claude Code 的 [`SKILL.md` 規範][skill]，再加上我們可選的 `od:` 擴充套件。**沒有註冊步驟。** 資料夾丟進來、重啟 daemon、picker 裡就出現了。

### Skill 資料夾結構

```text
skills/your-skill/
├── SKILL.md                    # 必須
├── assets/template.html        # 可選但強烈推薦 —— seed 模板
├── references/                 # 可選 —— agent 在規劃階段會讀的知識檔案
│   ├── layouts.md
│   ├── components.md
│   └── checklist.md
└── example.html                # 強烈推薦 —— 一份手搓的真實樣例
```

### `SKILL.md` 的 frontmatter

前三個欄位是 Claude Code 的基礎規範 —— `name`、`description`、`triggers`。`od:` 下面所有欄位都是 OD 特有的、可選的，但 **`od.mode`** 決定 skill 出現在哪一組（Prototype / Deck / Template / Design system）。

```yaml
---
name: your-skill
description: |
  一段電梯演講。Agent 會原樣讀這段來判斷使用者的需求是否匹配。
  寫具體一點：surface、受眾、artifact 裡有什麼、沒有什麼。
triggers:
  - "your trigger phrase"
  - "another phrase"
  - "中文觸發詞"
od:
  mode: prototype           # prototype | deck | template | design-system
  platform: desktop         # desktop | mobile
  scenario: marketing       # 自由 tag，用來分組
  featured: 1               # 任何正整數都會讓它出現在「Showcase examples」
  preview:
    type: html              # html | jsx | pptx | markdown
    entry: index.html
  design_system:
    requires: true          # 這個 skill 是否會讀啟用的 DESIGN.md
    sections: [color, typography, layout, components]
  example_prompt: "一段可複製貼上的提示詞，最能體現這個 skill 的能力。"
---

# Your Skill

正文是自由 Markdown，描述 agent 應該走的工作流……
```

完整 grammar —— 型別化輸入、滑塊引數、能力 gating —— 在 [`docs/skills-protocol.md`](docs/skills-protocol.md)。

### 合併新 skill 的硬線

Skill 是使用者直接看到的面，所以我們對它挑剔。一個新 skill 必須：

1. **附一份真實的 `example.html`。** 手搓的、本地直接開啟就能看、像設計師真的會交付的東西。不要 lorem ipsum，不要 `<svg><rect/></svg>` 佔位 hero。如果你自己都不能搓出 example，這個 skill 大機率還沒準備好。
2. **過 anti-AI-slop checklist**（寫在 body 裡）。不準紫色漸變、不準通用 emoji 圖示、不準左 border 圓角卡片、不準把 Inter 當 *display* 字型、不準自編資料。完整黑名單看 README 的「Anti-AI-slop machinery」一節。
3. **誠實佔位。** Agent 沒真數字時寫 `—` 或一個標註的灰塊，絕不寫「快 10 倍」。
4. **附 `references/checklist.md`**，至少要有 P0 關卡（agent emit `<artifact>` 之前必須過的硬線）。格式照搬 [`skills/guizang-ppt/references/checklist.md`](skills/guizang-ppt/) 或 [`skills/dating-web/references/checklist.md`](skills/dating-web/)。
5. **如果是 featured skill，加一張截圖** 到 `docs/screenshots/skills/<skill>.png`。PNG 格式，約 1024×640 retina，從真實 `example.html` 上以縮小後的瀏覽器倍率截。
6. **是一個自包含資料夾。** CDN 引入不能超過其他 skill 已經引入的；不準用沒授權的字型；圖片不要超過約 250 KB。

如果你 fork 了一個現有 skill（比如從 `dating-web` 改成 `recruiting-web`），保留原 LICENSE 和原作者歸屬在 `references/` 裡，並在 PR 描述裡點出來。

### 已有的 skill —— 挑一個像的來抄

- 視覺 showcase、單屏原型：[`skills/dating-web/`](skills/dating-web/)、[`skills/digital-eguide/`](skills/digital-eguide/)
- 多屏移動流程：[`skills/mobile-onboarding/`](skills/mobile-onboarding/)、[`skills/gamified-app/`](skills/gamified-app/)
- 文件 / 模板（不需要 design system）：[`skills/pm-spec/`](skills/pm-spec/)、[`skills/weekly-update/`](skills/weekly-update/)
- Deck 模式：[`skills/guizang-ppt/`](skills/guizang-ppt/)（來自 [op7418/guizang-ppt-skill][guizang]，原樣捆綁）和 [`skills/simple-deck/`](skills/simple-deck/)

---

## 加一套 Design System

一套 design system 就是 `design-systems/<slug>/` 下的一個 [`DESIGN.md`](design-systems/README.md) 檔案。**一個檔案，零程式碼。** 丟進來、重啟 daemon、picker 按 category 分組顯示出來。

### Design system 資料夾結構

```text
design-systems/your-brand/
└── DESIGN.md
```

### `DESIGN.md` 形態

```markdown
# Design System Inspired by YourBrand

> Category: Developer Tools
> 一行總結，會顯示在 picker 的預覽裡。

## 1. Visual Theme & Atmosphere
…

## 2. Color
- Primary: `#hex` / `oklch(...)`
- …

## 3. Typography
…

## 4. Spacing & Grid
## 5. Layout & Composition
## 6. Components
## 7. Motion & Interaction
## 8. Voice & Brand
## 9. Anti-patterns
```

9 段式 schema 是固定的 —— skill body 會按這個結構 grep 內容。第一行 H1 會成為 picker 的標籤（`Design System Inspired by` 字首會被自動剝掉），`> Category: …` 那一行決定它落到哪個組。已有的 category 列表在 [`design-systems/README.md`](design-systems/README.md)；如果你的品牌真的塞不進任何一個，可以新增 category，但**優先嚐試現有 category**。

### 合併新 design system 的硬線

1. **9 個 section 都要在。** Section 內容空著可以（比如真的找不到 motion token），但標題必須保留，否則提示詞的 grep 會斷。
2. **Hex 是真的。** 直接從品牌官網或產品裡取色，不準從記憶裡掏，不準讓 AI 猜。README 裡那套 5 步「品牌資產協議」對維護者一樣適用。
3. **強調色給 OKLch 是加分項。** 讓色板在亮 / 暗模式之間能可預測地 lerp。
4. **不要營銷廢話。** 品牌的 tagline 不是設計 token。刪掉。
5. **slug 用 ASCII** —— `linear.app` 寫成 `linear-app`，`x.ai` 寫成 `x-ai`。已經匯入的 69 套都遵循這個約定，跟著寫。

我們內建的 69 套產品系統是透過 [`scripts/sync-design-systems.ts`](scripts/sync-design-systems.ts) 從 [`VoltAgent/awesome-design-md`][acd2] 匯入的。如果你的品牌應該歸屬在上游，**請先把 PR 發到那裡** —— 我們下一次同步會自動收上來。`design-systems/` 資料夾用來放那些**不適合歸到上游**的系統、加上我們手寫的兩套 starter。

---

## 接入一個新的 coding-agent CLI

接入一個新 agent（比如某個新 shop 的 `foo-coder` CLI）就是在 [`apps/daemon/src/agents.ts`](apps/daemon/src/agents.ts) 里加一項：

```javascript
{
  id: 'foo',
  name: 'Foo Coder',
  bin: 'foo',
  versionArgs: ['--version'],
  buildArgs: (prompt) => ['exec', '-p', prompt],
  streamFormat: 'plain',           // 如果它說 claude-stream-json 就寫那個
}
```

完事 —— daemon 會在 `PATH` 上檢測到它、picker 顯示出來、對話路徑就通了。如果這個 CLI 吐 **型別化事件**（像 Claude Code 的 `--output-format stream-json`），在 [`apps/daemon/src/claude-stream.ts`](apps/daemon/src/claude-stream.ts) 裡寫一個 parser，並把 `streamFormat` 設成 `'claude-stream-json'`。

合併硬線：

1. **真的跑通一次端到端會話** —— 把 daemon 日誌貼在 PR 描述裡，證明它流出了一個 artifact。
2. **更新 [`docs/agent-adapters.md`](docs/agent-adapters.md)**，寫清楚這個 CLI 的怪癖（要不要 key 檔案？支不支援圖片輸入？非互動模式的 flag 是什麼？）。
3. **README 的「Supported coding agents」表裡加一行**。

---

## 程式碼風格

格式我們不摳（儲存時跑 Prettier 就行），但有兩條不能讓 —— 因為它們出現在提示詞棧和使用者可見的 API 裡：

1. **JS/TS 用單引號。** 字串一律單引號，除非轉義太醜。程式碼庫已經是一致的，請保持一致。
2. **程式碼註釋用英文。** 即使 PR 是把某段翻譯成中文，程式碼註釋也保留英文，這樣我們能維護一份可 grep 的引用集。

除此之外：

- **不要寫廢話註釋。** 不要 `// 引入這個模組`、不要 `// 遍歷元素`。如果程式碼本身一眼能讀，註釋就是噪音。註釋只用來說明非顯而易見的意圖、或者程式碼本身表達不出來的約束。
- **`apps/web/src/` 用 TypeScript。** Daemon (`apps/daemon/`) 是純 ESM JavaScript，型別重要的地方用 JSDoc —— 保持這樣。
- **不要隨便加頂層依賴。** PR 描述裡至少要有一段，說明引入它能換到什麼、又新增了多少 bundle 位元組。[`package.json`](package.json) 的依賴少是有意為之。
- **推之前跑 `pnpm typecheck`。** CI 會跑；掛了會換來一句「請修一下」。

---

## Commit 與 PR

- **一個 PR 只做一件事。** 加 skill + 重構 parser + 升依賴，是三個 PR。
- **標題用動詞起頭 + 範圍。** `add dating-web skill`、`fix daemon SSE backpressure when CLI hangs`、`docs: clarify .od layout`。
- **正文解釋 why。** 「這個 PR 改了什麼」從 diff 一般能看出來；「為什麼要改」很少能。
- **如果有 issue，引用它。** 沒有、且改動非平凡，請先開 issue 讓我們先就「值不值得做」達成一致，再投入時間。
- **Review 期間不要 squash。** 推 fixup commit；merge 時我們會 squash。
- **不要 force-push 共享分支**，除非 reviewer 主動讓你這麼做。

我們不強制 CLA。Apache-2.0 已經覆蓋；你的貢獻按同樣的 license 授權。

---

## 報 bug

開 issue 時請帶上：

- 你跑的命令（精確到 `pnpm tools-dev ...`）。
- 選中的 agent CLI 是哪個（或者你走的是 BYOK 路徑）。
- 觸發問題時的 skill + design system 組合。
- 相關的 **daemon stderr 末尾幾行** —— 大多數「artifact 沒渲染出來」的報告，看到 `spawn ENOENT` 或 CLI 實際報錯後 30 秒就能定位。
- UI 問題貼一張截圖。

提示詞棧相關的 bug（「agent 吐了一個紫色漸變 hero，slop 黑名單不是禁了嗎」），請貼 **完整的助手訊息**，方便我們判斷違規來自模型還是提示詞。

---

## 提問

- 架構問題、設計問題、「這是 bug 還是誤用」 → 請用 [GitHub Discussions](https://github.com/nexu-io/open-design/discussions)（首選 —— 下一個人能搜到）。
- 「我想寫一個幹 X 的 skill 怎麼寫」 → 開一個 discussion。我們會回答，且如果是缺失的模式，答案會被收進 [`docs/skills-protocol.md`](docs/skills-protocol.md)。

---

## 我們不接收的 PR

為了保持專案聚焦，請不要發以下型別的 PR：

- **Vendor 一個模型執行時。** OD 整個賭注就是「你已有的 CLI 就夠了」。我們不帶 `pi-ai`、不帶 OpenAI key、不帶模型載入器。
- **未經討論不要把前端重寫到別的棧。** Next.js 16 App Router + React 18 + TS 是當前底線。不要隨手改成 Astro / Solid / Svelte 或其他框架。
- **把 daemon 換成 serverless function。** Daemon 的存在意義就是擁有真實的 `cwd` 和 spawn 真實的 CLI。SPA 部署 Vercel 沒問題，daemon 仍然是 daemon。
- **加 telemetry / 分析 / phone-home。** OD 是 local-first。唯一的對外請求是使用者明確配置的 provider。
- **打包二進位制** 而沒有附 license 檔案和原作者歸屬。

不確定自己的想法合不合適？開個 discussion 再寫程式碼。

---

## License

提交貢獻即代表你同意你的貢獻按本倉庫的 [Apache-2.0 License](LICENSE) 授權。例外是 [`skills/guizang-ppt/`](skills/guizang-ppt/) 下的所有檔案，保留它們原始的 MIT license 和原作者 [op7418](https://github.com/op7418) 的歸屬。

[skill]: https://docs.anthropic.com/en/docs/claude-code/skills
[guizang]: https://github.com/op7418/guizang-ppt-skill
[acd2]: https://github.com/VoltAgent/awesome-design-md
[ocod]: https://github.com/OpenCoworkAI/open-codesign
