---
name: ai-media-generator
description: 為使用者產生高品質的 AI 生圖、生影片、生音樂提示詞，並在需要時透過瀏覽器自動化實際送到目標平台。涵蓋 OiiOii、Kling 3.0/O-series、Seedance 2.0/2.5、Suno v5.5、Seedream 5.0/4.0、Vidu Q3、Midjourney V8.1、Flux 1.1 Pro / Kontext、Runway Gen-4.5 / Aleph、Google Veo 3.1、Ideogram 3、Nano Banana Pro、Stable Diffusion 3.5（⚠️ OpenAI Sora 2 已於 2026-04-26 停運，API 撐到 2026-09-24，預設改推 Runway/Veo/Kling）。只要使用者提到「AI 生圖」「AI 影片」「AI 音樂」「做 MV」「做 storyboard」「寫 prompt 給 XXX」「我想用 Kling/Suno/Midjourney/Runway/Veo...」「幫我操作 OiiOii / 即夢 / 可靈」「txt2img / img2video / 文生圖 / 文生影片 / 圖生影片」「角色一致性」「多鏡頭分鏡」「運鏡」「結果有瑕疵 / 不夠精緻 / 怎麼修」，或任何跟上述平台或影像/影片/音樂生成工作流相關的任務，都要用這個 skill。即使他們沒講明平台，只要任務是要餵給某個生成模型的 prompt，就用這個 skill 幫他們選對的平台、寫對的格式。
---

# AI Media Generator

> **Codex compatibility:** References that still say “Claude in Chrome” or “Chrome MCP” describe legacy browser behavior. On Codex, load `chrome:control-chrome` and use the currently available Chrome tools; never invent or call legacy `mcp__Claude_in_Chrome__*`, `javascript_tool`, or `browser_batch` names.

幫使用者把想法變成高品質的 AI 生成內容 (圖片、影片、音樂)，核心工作是 **寫對每個平台的 prompt** 以及 **必要時自動操作網站**。

**AI 短劇邊界：** 使用者要做 AI 短劇／漫劇、重生／系統／逆襲／隱藏大佬、分集連載或整季架構時，先由 [`ai-short-drama`](https://github.com/Hao0321/ai-short-drama) 建題材、Bible、pilot、分集狀態與 production pack；本 Skill 只接下游角色圖、場景圖、逐鏡影片／聲音 prompt 與模型執行，不自行重寫季弧。

## 🤖 Auto-Pilot Mode (超傻瓜一句話到成品)

**觸發：** 使用者說「幫我做 X」「做一個 X」「生個 X」「我要 X」這類命令式、且 X 含媒體類型 (圖/影片/音樂/MV/短片/海報/動畫) 或風格關鍵字 → **直接進 auto-pilot 不問**。

**流程 (見 [templates/auto-pilot.md](templates/auto-pilot.md) 完整版)：**
1. **Intent Parser** — 一句話拆 9 slot (媒體/長度/畫面比/主題/風格/角色/場景/音訊/語言)
2. **Fill Defaults** — 沒講的用預設 (video 預設 10s 16:9；動漫預設 Shinkai+Ghibli；電影預設 Deakins DP…)
3. **Platform Decider** — 按 [references/selector.md](references/selector.md) + quick pick 矩陣自動選
4. **Script + Storyboard Auto-gen** — 自動寫 logline + 角色卡 + 分鏡
5. **Prompt Crafting** — 強制套語彙庫 (見下方硬規則)
6. **Preview + Go** — 30-秒 override 視窗，使用者回「go/確認/OK」才執行；無回 + 需花錢 → 停；無回 + 免費 → 30 秒後自動繼續
7. **Execution + Report** — 按 [automation/click-protocol.md](automation/click-protocol.md) + [site-profiles/](automation/site-profiles/) 協議操作

**Auto-Pilot 必停的 checkpoint (不代做)：**
- ⛔ 付費 paywall / 升級提示
- ⛔ 送出前最終一次確認
- ⛔ 下載 / 分享 / 公開發布
- ⛔ 敏感內容 (真人照片疑似裸露/暴力/政治人物)
- ⛔ click-protocol.md 定義的「irreversible action」

**Auto-Pilot 禁止行為：**
- ❌ 幫 Claude 已知的事問使用者 (「你要幾秒的影片？」— 10s 預設就好)
- ❌ 一次拋多選項 — 選 1 個最佳 commit，讓使用者在 Preview 階段才決定改不改
- ❌ 寫純 generic 填詞 prompt (`beautiful / masterpiece / detailed`) — 違反下方硬規則（注意：`cinematic / 4K / 8K` 在新一代模型如 Seedance 2.0 是有效 token，看平台分流）
- ❌ 代付款

**使用者自然語言 flags 可 override 預設：** 使用者一句話可帶「用 Kling / 5 秒 / 豎屏 / 免費 / Ghibli 風 / 有對白」等自然語言 flag，auto-pilot 掃 [templates/user-flags.md](templates/user-flags.md) 對照表自動套用。使用者不懂術語也 OK — 「做個抖音」「可愛一點」「夢幻」都有對應翻譯。

**⚡ Token-Efficient Mode (大專案必讀)：** 本 skill 55+ 檔 / 14000+ 行，全量讀會爆 context。Auto-Pilot 預設套 [templates/token-efficient-mode.md](templates/token-efficient-mode.md) 的 7 層策略 (lazy load / grep / 子代理 / preset 跳過 / cache polling)。**一般任務目標 ~25-40k tokens，不是 100k+**。豪華模式 (全量讀) 只在 benchmark / 學 skill / 陌生平台探索時啟用。

## 🎯 快查表：3 秒找對檔案（更直覺入口）

**先看這張表 → 對到使用者說的話 → 直接讀那個檔。** 找不到再往下走完整流程。

| 使用者說… | 讀這個檔 | 為什麼 |
|---|---|---|
| 「AI短劇/AI漫劇/爽劇/重生/系統/逆襲/隱藏大佬/分集連載」 | [`ai-short-drama`](https://github.com/Hao0321/ai-short-drama) | 先鎖故事、角色、揭露梯與 state；媒體 prompt 再回本 Skill |
| 「結果有瑕疵/爛掉/不夠精緻/閃爍/變形/塑膠感/文字怪」 | [quality-control.md](references/quality-control.md) | 7+1 類瑕疵系統診斷 + 對症修法 |
| 「prompt 沒主題/畫面很空/不知道在拍什麼/效果差」 | [concept-first-prompting.md](references/concept-first-prompting.md) | 先定 concept 再加技術詞 |
| 「怎麼把想法組成強 prompt/太慢/每次從零想/要打分」 | [prompt-craft-engine.md](references/prompt-craft-engine.md) | 6層骨架 + 填空模板 + 10分評分閘（更快更強）|
| 「成品要更高級/更精美/更貴/像 editorial/反塑膠感」 | [aesthetic-grade.md](references/aesthetic-grade.md) | 打光比/調色/材質/鏡頭 DP 級配方（更精美）|
| 「不知道用哪個模型/該用哪個平台/不熟各家」 | [model-picker.md](references/model-picker.md) | 決策樹 + 各家最強情境 |
| 「角色要一致/同一個人出現多鏡/風格鎖定」 | [asset-library.md](templates/asset-library.md) + [multimodel-video-cheatsheet.md](references/multimodel-video-cheatsheet.md) | 角色卡 + @element 身份鎖 |
| 「Seedance 2.5/30秒/社群案例/UGC/Vlog/多參考/硬切/提示詞案例」 | [seedance-2-5-community-playbook.md](references/seedance-2-5-community-playbook.md) + [seedance.md](references/seedance.md) | 證據分級 + capability gate + 社群案例蒸餾 + 六類模板 |
| 「Seedance 商業/企業/建商/建築/房地產/廣告/產品/汽車/美食/電影/動畫」 | [seedance-domain-playbook.md](references/seedance-domain-playbook.md) + [seedance.md](references/seedance.md) | 八個垂直領域的 reference map、truth lock、物理帳本、原創模板與 QA |
| 「Seedance 長片/參考圖角色/首鏡後鎖定/時間凍結或倒轉/複雜事件連續性」 | [seedance-longform-continuity.md](references/seedance-longform-continuity.md) + [seedance.md](references/seedance.md) | 不變量帳本 + 時間碼 + 事件狀態機 + 15 秒切段 |
| 「圖生影片/i2v/img2video/參考圖鎖死/產品廣告鎖形狀」 | [image-to-video-workflow.md](references/image-to-video-workflow.md) | i2v 鎖形狀 SOP（產品廣告最穩） |
| 「寫歌/純音樂」→[suno.md](references/suno.md)｜「對白/音效/配樂/原生音訊」→[sound-design.md](references/sound-design.md) | 二選一 | 純音樂 vs 影片內音訊分流 |
| 「做 MV/卡點/轉場/剪接節奏」 | [music-video.md](templates/music-video.md) + [editing-transitions.md](references/editing-transitions.md) | BPM 同步 + match cut/J-cut |
| 「要現成 prompt/範本/某某風格(Wes Anderson/賽博龐克)」 | [preset-packs.md](templates/preset-packs.md) | 30+ preset 換占位符即用 |
| 「想抄某導演/DP/品牌/製作公司風格」 | [director-style-library.md](references/director-style-library.md) | 簽名 token（⚠️ Flux/Nano 洗掉導演名）|
| 「超短廣告/TikTok/10秒快剪」 | [nano-banana.md](references/nano-banana.md) | <30 token 超精簡 prompt |
| 「Wan 2.7/Kling Omni/HappyHorse/拉片復刻/多模型省成本」 | [multimodel-video-cheatsheet.md](references/multimodel-video-cheatsheet.md) | 2026 新模型招牌技巧 |
| 「幫我直接操作 OiiOii/Flow/Kling」 | [click-protocol.md](automation/click-protocol.md) + [site-profiles/](automation/site-profiles/README.md) | 自動化協議 + 各站 UI 地圖 |
| 「用 Atlas Cloud API 生圖/生影片」 | [atlas-cloud-api.md](references/atlas-cloud-api.md) + 目標模型 reference | 即時 schema、單次提交、prediction polling |
| 「免費/不想花錢」 | [selector.md](references/selector.md) Cost 欄 + 各平台檔 Free tier | 免費額度速查 |

> **🔴 OiiOii 專用：** OiiOii **2026-06-08 大改版**，最新 UI/流程/成本/i2v/選模型/新模型（Oii Image 2 / Gemini Omni / Oii X Imagine）一律以 [`automation/site-profiles/oiioii.md §0`](automation/site-profiles/oiioii.md) 為準（**SOURCE OF TRUTH**）。`references/oiioii.md` 只看 prompt 哲學，UI 別照它。

## 🔴 硬規則 (Mandatory)

### Meta 優先序（2026-04-21 實戰鐵律）

**Prompt 寫對一次 ≫ 操作快 10 次。**

根因：寫錯 prompt → 重做 → 等 8-10 分鐘。操作 25 秒 vs 5 分鐘差距（~4.5 分鐘），**遠遠小於「一次 prompt 失敗」成本（~10 分鐘等待 + token 浪費）**。所以速度優化的**真正**優先序：

1. **第一優先：查平台簽名 + 寫對 prompt** → [references/community-prompt-patterns.md](references/community-prompt-patterns.md)（跨 X/Threads/Reddit/小紅書/Bilibili 社群驗證，單一 source of truth）
2. **第二優先：單次提交極速化** → [automation/site-profiles/](automation/site-profiles/)
3. **第三優先：等待不 polling** → `Bash run_in_background:true + sleep 400`

順序反了 = 浪費 40+ 分鐘做 4-5 次嘗試才生出可用的。

### Prompt 必備語彙

**每次產 image / video / music prompt，都必須從 skill 進階語彙庫挑 token — 不是選配，是必做。**

**流程：**
1. **最優先查**：[references/community-prompt-patterns.md](references/community-prompt-patterns.md) — 按**目標模型**查簽名 token + 長度甜蜜點 + 禁忌（⚠️ 平台吃不同 token，Seedance/Wan 吃導演名 = 災難）
2. 決定任務類型 (攝影感/電影感/廣告/時尚/MV/VFX/社群短片)
3. **挑對應 reference 檔讀：**
   - 電影/攝影類 → [cinematic-direction.md](references/cinematic-direction.md) 必讀
   - 廣告/時尚/MV/品牌 → [commercial-direction.md](references/commercial-direction.md)
   - 特效/物理/大氣 → [vfx-effects.md](references/vfx-effects.md)
   - 原生音訊 (Veo/Sora/Vidu Q3) → [sound-design.md](references/sound-design.md)
   - 多鏡/剪接/節奏 → [editing-transitions.md](references/editing-transitions.md)
4. **快速路徑**：先查 [preset-packs.md](templates/preset-packs.md) 找最近的 preset，換占位符即可
5. **每個 prompt 至少嵌入 5-8 個高訊號 token**，從下列層挑（**看平台分流**）：
   - 導演/DP 名 (Deakins、Lubezki、Hoytema、王家衛、新海誠…) — ✅ MJ/Sora 2/Veo；❌ Flux/Nano Banana Pro/Seedance/Wan
   - 鏡頭/焦段 (Panavision anamorphic / 85mm / C-Series 等) — 通吃
   - 底片/感光 (Kodak Vision3 500T / Cinestill 800T 等) — ✅ Flux/MJ；❌ Seedance
   - 光比/燈光 (Rembrandt / 4:1 contrast / volumetric god rays 等) — 通吃
   - 色彩分級 (teal-orange / bleach bypass / A24 indie 等) — 通吃
   - 構圖/景別 (rule of thirds / medium close-up 等) — 通吃
   - VFX/大氣 (halation / Tyndall effect / particles 等) — 通吃
   - (Veo/Sora) 音訊三層 (Dialogue / SFX / Soundtrack)

### 禁用模式（通用 + 平台特定）

**通用原則：** **寧可 5 個具體 token，不要 20 個泛詞。** 但 generic 與否**看平台**。

**真正全平台垃圾（任何時候都別用）：**
- `beautiful / masterpiece / detailed / high quality / professional`（這幾個從沒在新一代模型有實證效果）
- `--no blur` 等 flag-style 負面 prompt（多數模型不吃，用自然語言 `no blur` 反而 OK）

**⚠️ 平台特定（注意「2026-04-21 vs 2026-05-18 推翻」— 模型升級會改變斷言）：**
- ⚠️ `cinematic` / `4K` / `8K` / `35mm-50mm-85mm` — **舊版 Seedance 1.0 弱，Seedance 2.0 大量吃**（v1.1.0 修正）。Kling / Sora 2 / Veo 3.1 / MJ v7 / Flux 全吃。
- ❌ **Seedance 2.0**：`fast`（改 `extreme speed / kinetic / rapid`）、多動詞同句、多主體獨立動作、chaotic wide 無時間區塊、個別 DP 名（藝術運動 / 品牌風格 OK）
- ❌ **Flux / Nano Banana Pro**：artist names（訓練時被 scrub）、`--ar` 語法
- ❌ **Runway Gen-4**：>60 字 prompt（最短的模型）、synonym drift
- ❌ **Kling**：stacking 多個相機運動、>4-5 distinct nouns

**先查 [community-prompt-patterns.md](references/community-prompt-patterns.md)** — 該檔每個模型都有「禁忌」section，且註明 cross-platform 推翻歷史。

### 驗證自檢（送出前必過 10 分評分閘）

**每個 prompt 送出前，用 [prompt-craft-engine.md §3 的 10 分品質評分表](references/prompt-craft-engine.md) 打分 —— ≥8 才送，<8 回去補。** 每個跨平台垃圾詞（beautiful/masterpiece/detailed/high quality）額外 −1。

10 項速記：①具體主體 ②可視動詞 ③2-4場景元素 ④鏡頭精度 ⑤單一風格錨 ⑥長度甜蜜點 ⑦無自相矛盾 ⑧平台簽名對 ⑨負面對症 ⑩存在性測試。

**快速 4 題版（趕時間）：** 查過 community-prompt-patterns.md 目標模型？Token 符合平台簽名？避開禁忌？長度甜蜜點？—— 4 過才送。

## 核心原則

1. **先問清楚「要什麼」，再決定「用哪個」**。同一個想法送到不同模型，prompt 寫法完全不同。先釐清：
   - 媒體類型：靜態圖 / 影片 / 音樂 / 複合 (MV、分鏡動畫)
   - 用途：社群貼文 / 廣告 / 電影感短片 / 角色一致性專案 / 文字海報
   - 手上資源：有沒有參考圖、首尾幀、角色圖、歌詞
   - 預算/可用性：免費網站 / 付費會員 / API 金鑰

2. **讀對應的 reference 檔**。本 skill 的知識是分散式的。不要從腦中記憶硬編 prompt — 每次都先讀目標平台的 reference 檔，因為各模型版本更新很快，檔案裡有最新的語法、參數、連結。

3. **中英混寫時有規則**：主體名詞、運鏡術語、模型參數用英文；情感描述、文化元素 (漢服、水墨、國風)、旁白/歌詞用中文。Seedream 與 Kling 的中文支援最好；Midjourney、Flux、Runway、Veo 英文效果明顯較佳。

4. **Prompt 長度**。多數模型在 60–150 字 / tokens 之間最佳；Flux Kontext 上限 512 tokens；Veo 建議 3–6 句話；Sora 偏好「分鏡式」描述。reference 檔有每個模型的具體上限。

5. **輸出格式**。除非使用者明說只要 prompt 本文，否則給他們：
   - 一個 **可複製的 prompt 區塊** (通常英文)
   - 一段 **繁中說明**：這個 prompt 為什麼這樣寫、哪些 token 可以換掉、預期輸出會長怎樣
   - 一組 **建議參數** (aspect ratio、時長、model variant、seed 等)
   - 如果有平台特殊語法 (tag、metatag、參考圖槽位)，把它結構化呈現

## 工作流

### Step 1 — 選平台

如果使用者已指定平台 (「用 Kling」「幫我寫 Suno prompt」)，直接跳 Step 2。

否則讀 [references/selector.md](references/selector.md) 按「媒體類型 × 用途 × 資源」選出 1–2 個最佳候選平台，並把推薦理由用 2–3 句話告訴使用者。如果落差很大 (例如「免費 vs 付費最佳」)，給使用者選擇。

### Step 2 — 讀對應的 reference 檔

根據選定平台，**一定要** 讀對應檔案，不要憑記憶寫 prompt：

> **🧭 不確定選哪個模型？先讀 [references/model-picker.md](references/model-picker.md)** — OiiOii 全模型 + 各平台「選誰 / 招牌 prompt 技巧」總表 + 30 秒決策樹。**使用者嫌「不熟模型」「該用哪個」時必讀。**

**影片 (Video)**
- Kling 3.0 / O-series（V3 畫質 / Omni 參考驅動 / O1 首尾幀，三者不同）→ [references/kling.md](references/kling.md)
- Seedance 2.0 / 2.5 / 1.x（`@AssetName` 標記；2.5 先做 provider capability gate）→ [references/seedance.md](references/seedance.md)。**2.5 社群案例、30 秒、多參考、UGC／Vlog、硬切或 prompt 拆解**另讀 [references/seedance-2-5-community-playbook.md](references/seedance-2-5-community-playbook.md)；**商業、建商、廣告、產品、汽車、美食、電影或動畫**另讀 [references/seedance-domain-playbook.md](references/seedance-domain-playbook.md)；**參考圖角色、首鏡後鎖定、時間凍結／倒轉、複雜物理或 15 秒以上故事**另讀 [references/seedance-longform-continuity.md](references/seedance-longform-continuity.md)
- Vidu Q3 Mix/Ref/Pro（動漫 + 多角色一致，`@tag` 多參 + HEX 鎖色）→ [references/vidu.md](references/vidu.md)
- Runway Gen-4.5 / Aleph / Act-Two → [references/runway.md](references/runway.md)
- Google **Veo 3.1** → [references/veo.md](references/veo.md)｜🆕 **Gemini Omni**（對話式多模態、多輪編輯、any-to-any）→ [references/gemini-omni.md](references/gemini-omni.md)（Google 雙旗艦：4K 畫質線 + 對話編輯線）
- 🆕 **Hailuo 2.3 Pro/Std**（角色表演/微表情旗艦，5s）→ [references/hailuo.md](references/hailuo.md)
- Wan 2.7·2.6（複雜場景/對白/Thinking Mode）/ HappyHorse（敘事音畫）→ [references/multimodel-video-cheatsheet.md](references/multimodel-video-cheatsheet.md) + [model-picker.md](references/model-picker.md)
- ⚠️ OpenAI Sora 2 → [references/sora.md](references/sora.md)（**已停運** — app 2026-04-26 關，API 2026-09-24 關。讀此檔只為了解替代方案；新任務改用 Veo/Gemini Omni/Runway/Kling）

**圖片 (Image)**
- **🆕 Oii Image 2 [Best]**（OiiOii 圖片旗艦：文字渲染+寫實最強，代替已移除的 GPT-Image2）→ 見 [site-profiles/oiioii.md §0](automation/site-profiles/oiioii.md)。⚠️ **GPT-Image2 已從 OiiOii 移除 2026-06-08**；圖內精準文字改用 Oii Image 2 / Ideogram 3 / Nano Banana Pro
- Seedream 5.0 / 4.0 → [references/seedream.md](references/seedream.md)
- Midjourney V8.1 / niji 7（⚠️ `--oref` 為 V7-only） → [references/midjourney.md](references/midjourney.md)
- Flux 1.1 Pro / Kontext → [references/flux.md](references/flux.md)
- Ideogram 3（文字/海報/Logo 精準渲染）→ [references/ideogram.md](references/ideogram.md)
- Nano Banana Pro（超短 10s 廣告 / TikTok，<30 token 精簡 prompt）→ [references/nano-banana.md](references/nano-banana.md)
- Stable Diffusion 3.5 / SDXL → [references/stable-diffusion.md](references/stable-diffusion.md)

**音樂 (Music)**
- Suno v5.5（Personas 已改名 Voices） → [references/suno.md](references/suno.md)

**複合 / 多智能體**
- **OiiOii.ai** → 自動化/UI/最新模型：[site-profiles/oiioii.md §0](automation/site-profiles/oiioii.md)（**SOURCE OF TRUTH**）｜prompt 哲學：[references/oiioii.md](references/oiioii.md)（⚠️ §1-§12 舊 UI 勿照）

**跨平台共通**
- 鏡頭語言 (影片類都適用) → [references/camera-language.md](references/camera-language.md)

**進階導演 / VFX / 音效 / 剪接 級別 prompt 設計** (當使用者要「電影級」「廣告級」「奢侈品級」「完整影視團隊」時**必讀**)
- 電影導演 / 攝影指導 / 底片 / 燈光 / 構圖 / meta tokens → [references/cinematic-direction.md](references/cinematic-direction.md)
- 廣告 / 時尚 / MV 導演 / 品牌調性 / 社群短影音 → [references/commercial-direction.md](references/commercial-direction.md)
- VFX 總監 / 大氣 / 物理 / 特效 recipes → [references/vfx-effects.md](references/vfx-effects.md)
- **音效設計** (對白 / SFX / Foley / 配樂) → [references/sound-design.md](references/sound-design.md) — Veo 3.1 / Vidu Q3 原生音訊必讀
- **剪接 / 轉場 / 節奏** (match cut / whip pan / J-L cut / ASL 律動) → [references/editing-transitions.md](references/editing-transitions.md) — 多鏡故事、storyboard、MV 必讀
- **🎬 概念先行 Prompting** (prompt 沒主題/畫面很空/效果差/不知道在拍什麼) → [references/concept-first-prompting.md](references/concept-first-prompting.md) — **使用者嫌「prompt 沒主題」「有些畫面不知道在幹嘛」「影片效果差」時必讀**。治「技術詞堆疊但空洞」：先定一句話 concept → 3-beat 敘事弧 → 每個鏡頭做存在性測試 → 才加技術詞。含 6 個產品廣告概念框架
- **⚙️ Prompt 組裝引擎**（有 concept 了但不知怎麼組成強 prompt / 想更快 / 要品質評分）→ [references/prompt-craft-engine.md](references/prompt-craft-engine.md) — **更強+更快的 META 引擎**：通用 6 層解剖（弱形容詞→強具體事實 before/after）+ 6 種媒材 <60 秒填空骨架 + **10 分品質評分閘（≥8 才送）**。流程位置：concept-first →（本檔組裝+評分）→ vocab 檔填字 → quality-control 修
- **💎 精緻度分級（更精美）**（成品要像 editorial/commercial-award 級 / 反 AI 塑膠感 / 要「貴」）→ [references/aesthetic-grade.md](references/aesthetic-grade.md) — DP 級**可貼入配方**：打光 setup（方向+質感+光比）、調色科學（Kelvin/hue）、材質表面渲染（玻璃/金屬/皮膚/食物反塑膠 phrase）、鏡頭光學、大氣。比 cinematic-direction 的選單更深、給整段配方
- **🔧 反瑕疵品質控制** (結果爛了/有瑕疵/不夠精緻怎麼修) → [references/quality-control.md](references/quality-control.md) — **使用者嫌結果有瑕疵、產品變形、塑膠感、閃爍、文字鬼影、動得很假時必讀**。系統化分類 7 類瑕疵 + 對症修法（鎖時間/鎖物理/鎖主體）+ 平台物理強弱速查
- **🧭 模型選擇大全** (該用哪個模型 / 不熟各家模型) → [references/model-picker.md](references/model-picker.md) — OiiOii 全模型 + Gemini Omni / Kling O-series / Vidu / Hailuo / Wan / Seedance 各家「最強情境 + 招牌 prompt 技巧 + 何時選」+ 命名混淆對照（Gemini Omni vs Kling Omni）
- **🎬 多模型影片操作手冊** (Wan 2.7 / Kling 3.0 Omni / HappyHorse 怎麼下 prompt / 拉片復刻 / 多模型省成本) → [references/multimodel-video-cheatsheet.md](references/multimodel-video-cheatsheet.md) — Wan 2.7 Thinking Mode 完整 brief + Kling Omni 身份鎖(@element)+物理事件 + HappyHorse 短 prompt+原生音訊 + 拉片復刻先選模式 + 多模型 tier-by-cost/frame-chaining/per-model 重寫 + shot-type 選模型表。**用 OiiOii 新影片模型或要省成本跑多模型時必讀（2026-06 研究+實測）**
- **📋 經驗證 Prompt 庫** (要現成高品質 prompt / 不想從零寫 / 要範本) → [references/proven-prompts.md](references/proven-prompts.md) — **26+ 個逐字可貼的高品質 prompt**（Google 官方 Veo/Omni guide、Kling 官方、Seedance/Vidu @語法實例、廣告/美食/汽車/精品/多鏡頭/對白），按用途分類 + 來源 + 跨模型語法速查。**使用者要「更好的 prompt / 範例 / 範本」時必讀**

這三個進階檔是 **語彙庫**，不是流程手冊。使用方式：
1. 先照平台的 reference (kling.md / flux.md 等) 確定該平台的 prompt 結構
2. 再從進階檔挑 **5-8 個高質量 token** 填進結構裡
3. 不要整段 copy — 挑對情緒、對平台、對故事的那幾個詞

一個人身高不同，鏡頭焦段/燈光/底片的「關鍵詞組合」就不同。「超級資深影視導演」的 prompt = 四層堆疊 `[導演/DP] + [鏡頭/底片] + [燈光/色調] + [動作/構圖]`，每層挑最契合的 1-2 個 token。

### Step 3 — 根據任務類型參考 template

- **快速：抽一個現成 preset 套用** (30 個電影/廣告/MV/VFX/短影音 preset) → [templates/preset-packs.md](templates/preset-packs.md)
- **複雜多步驟任務 (> 15s 影片 / 多角色一致 / 品牌包 / 專輯 / MV 一條龍)** → [templates/advanced-recipes.md](templates/advanced-recipes.md) 13 個 chain workflow (Extend / Persona / Motion Control / Cameo 等進階功能組合)
- **重用角色/風格/場景卡** → [templates/asset-library.md](templates/asset-library.md)
- 多鏡頭 / 分鏡故事片 → [templates/storyboard.md](templates/storyboard.md)
- 音樂影片 / MV → [templates/music-video.md](templates/music-video.md)
- 常用負面提示詞 → [templates/negative-bank.md](templates/negative-bank.md)
- **社群多版本 / 預算 flag / 平台 flag / 風格 flag** → [templates/user-flags.md](templates/user-flags.md)
- **大專案省 token** → [templates/token-efficient-mode.md](templates/token-efficient-mode.md)

**preset-packs.md 的用法：** 使用者要「Wes Anderson 風」「Nike 廣告感」「賽博龐克雨夜」「水下夢境」等明確風格時，**先到 preset-packs.md 找最近的 preset**，換占位符即可，不用每次從零組 prompt。若使用者要的風格不在 preset 裡，再從 [cinematic-direction.md](references/cinematic-direction.md) / [commercial-direction.md](references/commercial-direction.md) / [vfx-effects.md](references/vfx-effects.md) 現場組。

### Step 4 — 組出 prompt 並輸出

依 reference 檔給的公式組出 prompt。每個平台有自己的 order、特殊符號、tag — **嚴格按 reference 檔的格式寫**。

**輸出格式**：可複製的 prompt 區塊 + negative + 參數 + 繁中 why (每個 token 為什麼選)。完整範例見 [preset-packs.md](templates/preset-packs.md) 的 30 個 preset，每個都是 ready-to-use 格式。

### Step 5 (選配) — 瀏覽器自動化執行

如果使用者要求「幫我直接做」「幫我貼上去按產生」「操作 OiiOii/Kling/Suno」等，這套檔要讀：

**1. 通用 click 協議 (所有網站都要遵循)**
- [automation/click-protocol.md](automation/click-protocol.md) — Click 決策樹、ref 保質期、座標精準技巧、驗證 selected 的方法、scroll-aware 座標、paywall 偵測、等待策略、驗證循環、速度優化

**2. 網站特定 profile (該站的 UI 地圖 + 座標 + 陷阱)** — 索引見 [site-profiles/README.md](automation/site-profiles/README.md)

**✅ 完整驗證（可放心自動化）：**
  - [oiioii.md](automation/site-profiles/oiioii.md) — OiiOii 整合，**§0 = 2026-06-08 大改版 SOURCE OF TRUTH**（新入口/Agent▾選模型/i2v/一致性/新模型）
  - [flow.md](automation/site-profiles/flow.md) — Google Veo 3.1 + Gemini Omni Flash（完整 Phase 1-6 + 生成偵測看 gallery tile）
  - [kling.md](automation/site-profiles/kling.md) — Kling 3.0（完整 Phase 1-7 + credits + Fast-Track）

**⚠️ 部分/stub（有 UI 地圖但未全驗）：** suno / midjourney / seedream / runway / vidu / ideogram

**🔴 停運：** sora（不支援自動化，新案改 Veo/Kling/Runway）

**📝 [_template.md](automation/site-profiles/_template.md)** — 新網站 profile 模板

> 使用者說「幫我直接操作 XXX」時：先確認該站在上列。**未列出者告知「該平台暫不支援全自動化，但我可代寫 prompt 讓你貼上去」**。

**3. 高層流程速查 (每站登入與主流程概述)**
- [automation/browser-guide.md](automation/browser-guide.md)

**送出前必做的安全檢查 (見 click-protocol.md 詳細)：**
- 確認使用者已登入目標站 (**絕不代輸入密碼**)
- 把即將輸入的 prompt 完整貼給使用者，**等確認再送**
- 首次操作某站 → screenshot 對照 site-profile，UI 改版就更新 profile
- 送出前 screenshot (before)，送出後 screenshot (after)，**比對 UI 變化確認真送出**
- 撞到 paywall / modal / 非預期彈窗 → **立刻停下問使用者**，不代付款

**reliability 核心原則 (來自 OiiOii demo 踩坑紀錄)：**
1. **不信任記憶中的座標** — 5 秒前的 screenshot 可能已過期，click 前必須剛截圖
2. **ref 很短命** — `find` 完 **立刻** click，中間不插其他 tool call
3. **selected state 看填底色，不是粉色 ring** (那是 hover)
4. **click 沒中別亂重試** — 先 screenshot 診斷 (paywall？disabled？座標偏？)
5. **Polling 不要 < 60s 也不要 > 3min** — 互動 UI 60-90s，純背後等待 3-5min

**⚡ Chain workflow 強制節省 (來自 Suno 5 首歌 chain 慘痛教訓 2026-04-20)：**
連跑多任務 (做 5 首歌、5 張圖、多支影片) 時：
1. **中間禁 screenshot** — 只第 1 task 後 + 全部完成各 1 張 (浪費點 #1)
2. **中間禁 TodoWrite** — chain 完成後一次性更新即可 (浪費點 #2)
3. **clear field 用 1-click** — 點 trash icon 或 ctrl+a 覆寫，禁用 triple-click + Ctrl+A + Delete 三步走
4. **內容寫標準長度** — Suno 歌詞 ~25 行 / Veo prompt ~80 字 / MJ keyword 30-50 字 / Seedream 80-120 字，超寫不加分
5. **平台有 Series Mode/Multi-Shot/Multi-Reference → 用內建批次** 不要 N 次 chain (Suno Persona / Kling Multi-Shot / Seedream Series / MJ omni-ref / Vidu Q3 multi-entity)

**檢測標準：** 5 task chain 應 ≤ 36 tool calls + ≤ 1.5k token + < 5 分鐘。違反任一就要修。詳見 [click-protocol.md §「Token + 時間最佳化」](automation/click-protocol.md)。

## 關鍵品質檢查 (在交付 prompt 前跑一遍)

- [ ] 有沒有「具體的主體描述」— 不只是 "a person" 而是 "a woman in her 30s with chin-length silver hair"
- [ ] 動作有沒有可視化的動詞 — "walks slowly" / "tilts her head" 優於 "is moving"
- [ ] 場景有沒有 2–4 個具體元素 — 時間、地點、天氣、物件
- [ ] 鏡頭 (如果是影片) — 運鏡類型 + 速度 + 距離
- [ ] 風格與光影 — 一到兩個清楚的風格錨 (例如 "cinematic, teal-and-orange grade")
- [ ] 長度適當 — 檢查目標平台的 sweet spot
- [ ] 沒有自相矛盾 — 不要同時寫 "close-up" 和 "wide establishing shot"
- [ ] 負面提示詞 (如該模型支援) 有列常見缺陷
- [ ] **產品/品牌類** — 有沒有「主體完整性鎖」(rigid form / no morphing / 形狀不變)？產品優先走 i2v 鎖死 hero 圖。見 [quality-control.md §1](references/quality-control.md)
- [ ] **複雜物理** (液體/布料/煙/碰撞) — 有沒有挑對強物理平台 (Runway Gen-4.5 / Veo 3.1)？弱物理模型硬做必爛

## 常見反模式

- **拿 Midjourney 的 `--ar 16:9 --s 250` 語法餵 Flux / Kling / Sora** — 只會被當成雜訊。每個平台有自己的參數欄位。
- **在 Suno Lyrics 欄寫 "Verse:"** — 會被當成歌詞唱出來。要寫 `[Verse]`。
- **Seedream 叫它寫文字時用單引號或不加引號** — 官方建議用雙引號包住要顯示的字：`"Seedream 5"`。
- **Kling / Seedance 運鏡疊太多** — "zoom in while panning right and tilting up" 會爛掉。一次一到兩個運鏡。
- **Vidu ref2v 時 prompt 又重複描述參考圖的外觀** — 會互相打架。描述「動作與互動」就好。
- **SD 3.5 用 `(word:1.3)` 權重語法** — 3.5 不吃這個，改用自然語言。SDXL 才吃。

## 關於「萬用」的邊界

此 skill 設計為 prompt engineering + 瀏覽器自動化雙層。不直接呼叫 API (API key 管理、計費都是額外議題)，但若使用者明確要走 API，reference 檔裡有各平台的官方 API 端點連結，可以協助組 payload。使用者指定 Atlas Cloud 時，讀 [atlas-cloud-api.md](references/atlas-cloud-api.md)，先即時核對 model/schema，再遵守「生成 POST 不重送、prediction GET 有界重試」的規則。

## 版本資訊

**平台知識最後校準：2026-08（Seedance 2.5 社群案例、長敘事一致性與跨產業應用擴充）。** 各 reference 檔末尾有官方文件連結 — **若要執行會花錢/產生後果的操作前，優先拿 reference 連結當最終依據**，因為版本/定價變動快。

**外部資源（官方文檔 / API / 社群 / 緊急更新連結）** → [references/external-resources.md](references/external-resources.md)。**2026 模型發布時間軸**（誰新誰舊/替代對象/何時選）見該檔的 Model Release Timeline 表。

**2026-06 重大變動（已驗證）：**
- 🔴 **OpenAI Sora 2 停運** — app/web 2026-04-26 已關，API 2026-09-24 關，資料永久刪除。新任務勿用，改 Runway Gen-4.5 / Veo 3.1 / Kling 3.0。
- 🆕 **Runway Gen-4.5**（2025-12-01）— Video Arena 榜首 1247 Elo，勝 Veo 3 / Sora 2 Pro，物理最強。
- 🆕 **Midjourney V8.1**（2026-04-30）— 原生 2K HD、快 3-5×；⚠️ `--oref` Omni Reference 為 **V7-only**，V8 不支援。
- 🆕 **Suno v5.5**（2026-03）— Personas 改名 **Voices** + Custom Models + Suno Studio DAW。
- 🆕 **Kling 3.0 / O-series**、**Seedream 5.0 Lite**、**Vidu Q3**、**Ideogram 3.0** 為各家當前主力。
- 🆕 **Google Gemini Omni**（I/O 2026, 2026-05-19）— any-to-any 多模態影片「影片版 Nano Banana」，對話式編輯，Flash 版已上 Flow PRO。Google 影片變雙旗艦（Veo 3.1 + Gemini Omni）。⚠️ ≠ Kling 3.0 Omni（同名不同家）。
- 各模型「最強情境 + 招牌 prompt 技巧」見 [references/model-picker.md](references/model-picker.md)；「prompt 沒主題/畫面空/效果差」見 [references/concept-first-prompting.md](references/concept-first-prompting.md)。

**2026-08 Seedance 擴充：**
- Seedance 2.5 採 provider capability gate，不把社群介面標籤誤當所有供應商都已支援；送出前核對實際模型名稱、時長、音訊與參考素材能力。
- 新增 30 秒、多參考、UGC／Vlog、硬切、提示詞拆解、首鏡後鎖定、角色／髮飾／服裝／道具一致性、時間凍結／倒轉與複雜物理的專用手冊。
- 新增商業、建商、廣告、產品、汽車、美食、電影與動畫的領域骨架；AI 短劇的故事架構仍交由 `ai-short-drama`，本 Skill 負責下游媒體提示詞。

各模型「禁忌 / 版本推翻歷史」見 [references/community-prompt-patterns.md](references/community-prompt-patterns.md)；結果有瑕疵的修法見 [references/quality-control.md](references/quality-control.md)。
