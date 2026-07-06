# Community-Validated Prompt Patterns (研究證據版)

**Source:** 2026-04-21 三 agent 平行研究（X/Threads/Reddit/小紅書/Bilibili/官方 cookbook + fal.ai / Google Cloud / OpenAI Cookbook 等）
**用法：** 任何時候生 prompt 前先查這檔找目標模型的**簽名 token + 長度甜蜜點 + 禁忌**。

---

## ⭐ 跨模型 5 條 META rule（通吃）

1. **鏡頭詞彙是通用貨幣** — `35mm / 85mm / dolly / handheld / low-angle / rack focus / teal-orange` 幾乎所有模型都吃（MJ/Flux/Nano/Sora/Veo/Seedance/Kling/Runway）。想要寫實感就一定用。
2. **Subject 放最前 10-15 token** — 所有模型都 early-weight 開頭。"ancient Chinese general" 一定 BEFORE "in battlefield fog"。
3. **導演名分裂** — 不是所有模型都吃：
   - ✅ 吃：MJ V8.1、SDXL、Sora 2、Veo 3.1
   - ❌ 刷掉：Flux（BFL 訓練時 scrubbed）、Nano Banana Pro
   - ⚠️ 不穩：Ideogram、Seedance（弱證據）、Kling（可能 over-weight 出現 "in the style of" artifact）
   - **對策**：中文模型（Seedance / Wan / Hailuo）改用具體視覺詞（墨色、留白、金色逆光、大遠景、慢鏡頭）
4. **ONE verb per shot** — 兩個以上動詞 = 模型自選 = 混亂。Seedance 特別嚴重（"combining two motion verbs makes the model choose chaos"）。想要複雜動作？分 shot 用 `Cut to:`。
5. **Anchor-noun 字串一致** — 多鏡頭時同一物件用**完全相同的名稱**。"olive sweater" 不能下一鏡變 "green knit" — Runway、Wan 會 break。

---

## 🎥 影片模型 per-model 簽名

### Seedance 2.0 pro / 1.5（2026-05-18 大更新 — 107 prompts 證據）

> **Source：** [YouMind-OpenLab/awesome-seedance-2-prompts](https://github.com/YouMind-OpenLab/awesome-seedance-2-prompts) (CC BY 4.0, 6 Featured + 101 curated)

- **結構（首選 — 多鏡頭）：** Bracketed labels
  ```
  [Style] + [Duration] + [Scene] + [Character] + 
  [Shot N: time] {Visuals: / Action: / Details:} × 3-5 + 
  [Audio] + [Constraints]
  ```
- **結構（單鏡頭 5-10s）：** 一句 format anchor → subject → camera → action → no-X negatives → resolution
- **長度：** **單鏡 60-100 字 / 多鏡 200-700 字**（前期我說 30-200 是錯的，多鏡需要更長）
- **時間區塊格式**（全部都吃）：`[00:00-00:05]` / `(0-2s)` / `[00–05s]` / `0-3 seconds:` / `[0–2 SEC]`
- **簽名 token：**
  - **Bracketed labels：** `[Style]`, `[Scene]`, `[Character]`, `[Shot N]`, `[Duration]`, `[Audio]`, `[Constraints]`
  - **Sub-labels in shot：** `Visuals:`, `Action:`, `Details:`, `Atmosphere:`, `Special Effects Details:`
  - **Format anchor：** `ESPN-style 16:9`, `Hollywood live-action`, `Sony A7S3/cinema camera`, `IMAX Fantasy Camera`, `Dolby Atmos`, `Unreal Engine 5 fluid rendering`, `90s anime film style`, `Pixar style`, `Fast and Furious inspired`
  - **鏡頭/焦段（**有效**）：** `35mm follow shot`, `50mm lateral tracking`, `28mm push-in`, `Macro lens`, `IMAX whip-pan`, `360-degree rotation`, `low-angle circling`, `orbit`, `handheld shake`
  - **解析度（有效）：** `4K`, `8K`, `UHD`, `1080p`
  - **動態詞：** `extreme speed`, `high-octane`, `kinetic`, `explosive acceleration`, `rapid`, `slow-motion impact`
  - **物理詞：** `realistic weight transfer, momentum, recovery, sweat, fabric movement`
  - **視覺：** `sword light trails, splashing water, flying rubble, sparks, shockwaves, dust plumes`
  - **Quoted dialogue（會 lip-sync）：** `"What are you looking at?"` / 日文 `「今日は世界を滅ぼさない。」` + `Subtitle:` 雙語
  - **Lip-sync directive：** `200-400ms pauses, mouth only moves slightly when speaking, emotional micro-tremors synchronized with breathing`
- **Constraints tail（必放）：** `no deformation, no drift, no artifacts, no extra text/watermarks, locked horizon, stable temporal coherence`
- **Audio Profile（Seedance 2.0 殺手鐧 — 4-modal 支援原生音訊）：**
  ```
  Sound design: <ambient SFX>, <action SFX>, <breathing>, <music genre + instrument>
  ```
- **中文 vs 英文：** **武打/古裝/romance/MV** 寫中文 > 英文（Featured #1 #4 #5 全中文）；西方場景英文較順
- **禁忌（更新版）：**
  - ❌ `fast` — 仍是最爛詞，改 `extreme speed / high-octane / kinetic / rapid`
  - ❌ 多動詞同句 — 仍 true（一個 time block 一個 ONE verb）
  - ❌ 多主體獨立動作（A 打 B 閃 C 射）
  - ❌ `--no blur` flag 語法（不支援，但 `no X` 自然語言 OK）
  - ❌ Chaotic wide 戰場無時間區塊 → 失敗率最高（**3-5 shots 有時間區塊 OK**）
  - ⚠️ 個別 DP 名（Deakins、Lubezki）— 弱證據；**藝術運動/品牌風格名（Pixar / Ghibli / 90s anime）✅ 有效**
- **戰鬥 three-beat**：wide 建立間距 → medium 跟拍節奏 → close-up 衝擊/呼吸/腳步
- **騎馬專 tip：** 低角度 tracking dolly **沿馬側**跑 + dust/steam 粒子 + locked horizon + 1 鏡慢動作
- **Reference video cheat：** 上傳 ≤15s 動作參考 + `imitate the movements of @video1` — 武打最大 quality jump
- **2026-04-21 vs 2026-05-18 推翻清單**（重要）：
  - ❌ 之前說「`cinematic` 是 generic」→ ✅ Seedance 2.0 大量吃
  - ❌ 之前說「35mm 沒效」→ ✅ 焦段 token 有效
  - ❌ 之前說「4+ shots 擠 15s」→ ✅ 3-5 shots/15s 是甜蜜點（有時間區塊前提）
  - ❌ 之前說「`8K` 是 generic」→ ✅ Seedance 2.0 對解析度 token 有反應

### Kling 3.0 / 2.1（快手）
- **結構：** Scene → Characters → Action → Camera → Audio
- **長度：** 40-80 字（comma-separated film-crew shorthand）
- **簽名 token：** aspect ratio 後綴 `16:9 / 2.39:1`、`35mm handheld`、`85mm`、`Steadicam weaving`、`whip pans`、`rack focus`、`teal-orange grade`
- **中文 vs 英文：** 平手無明顯差
- **禁忌：** >4-5 distinct nouns break it；"slow motion" 放負面 prompt 沒用；stacking 相機運動

### Sora 2（OpenAI）
> 🔴 2026-04-26 app/web 已停運，API 撐到 2026-09-24；以下知識僅供 API 用戶或歷史參考，新任務改用 Runway/Veo/Kling
- **結構：** Shot-list briefing a cinematographer
- **長度：** 40-100 字
- **簽名 token：**
  - **Format anchor**（Sora 最強）：`bodycam`, `surveillance`, `ring doorbell`, `TED Talk stage`
  - 冒號對白：`Detective: "You're lying."`
  - 光線方向明確：`warm key from left, cool rim from neon`
  - 語氣詞：`professional`, `deadpan`, `dramatic`
  - **🆕 導演 style 強支援**（31 位範本見 [director-style-library.md](director-style-library.md)）：
    - `in the style of Wong Kar-wai` — neon + slow-motion + tracking
    - `in the style of Denis Villeneuve` — epic landscape + atmospheric
    - `in the style of Wes Anderson` — symmetric + pastel
    - `in the style of Christopher Nolan` — IMAX + practical effects
- **Trick：** 2×4s stitched > 1×8s（官方 cookbook 推薦）
- **禁忌：** 會話語氣 ("please make")、模糊形容詞、over-direct physics

### Veo 3.1（Google Flow）
- **結構：** [Cinematography] + [Subject] + [Action] + [Context] + [Style & Ambiance]
- **🆕 結構 v2（2026-05-19 發現）：JSON-structured prompt** — Veo3 直接解析結構化 JSON，比自由文本更精準。8 個 key：
  ```json
  {
    "shot_name": "Aerial Shot - Car on Dirt Road",
    "camera": {"type": "Wide Shot", "movement": "...", "lens": "Panavision", "focus": "...", "lighting_direction": "..."},
    "setting": {"environment": "...", "time_of_day": "Night", "atmosphere": "..."},
    "subject": {"main_subject": "...", "details": "..."},
    "visual_style": {"genre": "...", "film_stock": "Kodak 35mm Eastman 100T 5247", "color_grade": "...", "rating_tone": "PG-13", "overall_feel": "..."},
    "composition": {"elements": "..."},
    "implied_elements": {"sound": "..."}
  }
  ```
  Source: [`liu-kaining/Awesome-Veo3-Prompts`](https://github.com/liu-kaining/Awesome-Veo3-Prompts) 68⭐ — 31 個 JSON-formatted 範本，雙語版（EN+中文 JSON）。
- **長度：** 1-2 句 simple / 4-8 複雜 / timecode blocks 150+ 字 / **JSON 200-400 字最穩**
- **簽名 token（唯一 load-bearing audio）：**
  - `SFX: rustle of leaves, distant bird calls`
  - `Ambient: swelling orchestral score`
  - 對白用 `"quoted"`
  - Timecode：`[00:00-00:02] Medium shot from behind ...`
  - **JSON 模式**：`film_stock` 可填 `Kodak 35mm Eastman 100T 5247` / `Fuji Eterna` / `Cinestill 800T`
- **禁忌：** 負面 `no buildings` → 反寫 `desolate landscape`；under-specified camera；5+ SFX lines

### Runway Gen-4.5 / Aleph
- **結構：** 主角-動作-相機 子句
- **長度：** **25-60 字（最短）**
- **簽名 token：** `camera tracks from behind`, `trailing position`, **Novel View verbs**（`reverse shot`, `low angle`, `medium full shot`）
- **Aleph 特色：** 可對既有影片下 re-light / re-frame 指令
- **禁忌：** conversational greeting、synonym drift、stacking camera moves

### Hailuo 2.3（MiniMax）
- **結構：** [Camera Shot + Motion] + [Subject] + [Action] + [Scene] + [Lighting] + [Mood]
- **長度：** 30-60 字
- **簽名 token：** hyphenated 相機動詞 — `walk-right`, `pan-down`, `static shot`, `dolly zoom`；perspective `bird's eye view / macro view`
- **強項：** micro-expressions、ink-painting / CGI stylizations
- **禁忌：** 多主體 blocking（2.3 仍偏 single-subject）、dense 英文、abstract concept

### Wan 2.6 / 2.7（阿里通義萬相）
- **結構：** `Shot N (label, Ns): ...` 區塊；支援 up to 15s + native audio
- **長度：** 20w（with `prompt_extend=true`）or 60-120w 結構 shot list
- **簽名 token：** shot block、camera verbs (`track`, `push-in`, `orbit`)、**anchor 重複**嚴格
- **中文 > 英文** — 明顯優勢（native Alibaba 中文訓練）
- **禁忌：** synonym drift、maximum motion（slow push-in > whip pans）、too many beats per shot

---

## 🖼 圖片模型 per-model 簽名

### Midjourney V8.1
- **結構：** `[subject], [5-8 details], [lighting], [camera/lens], --ar X:Y --stylize N --style raw --sref/--oref`
- **長度：** 30-60 comma chips
- **🆕 V7 sweet spot params（2026-05-19 確認 from [`Pixmind-io/awesome-midjourney-v7-example-prompts`](https://github.com/Pixmind-io/awesome-midjourney-v7-example-prompts)）：**
  - **Portrait/Editorial：** `--ar 4:5 --s 250-350 --v 7`
  - **Cinematic/Documentary：** `--ar 16:9 --s 300-400 --style raw --v 7`
  - **Anime/Concept Art：** `--ar 9:16 --s 400-750 --v 7`
  - **Architecture：** `--ar 3:2 --s 200-300 --v 7`
  - **3D/Isometric：** `--ar 1:1 --s 250 --v 7`
- **🆕 廣告級 anchor token（V7 證據版）：**
  - `Editorial fashion portrait` / `Cinematic portrait` / `Documentary photography style`
  - `Hasselblad medium format` / `shot on 35mm film` / `Leica`
  - `Natural window light from left` / `Soft diffused lighting`
  - `Desaturated color palette` / `Warm morning atmosphere`
- **Params：**
  - `--ar` 3:4/9:16/16:9
  - `--stylize 60-150` 寫實 portrait / 400-750 fantasy
  - `--style raw` 給寫實用，消 MJ house style
  - `--chaos 0-25`、`--exp 5-25`（v7 新）
  - `--sref` 風格 ref + `--sw 50-500`
  - `--oref` 角色 ref + `--ow 100`（⚠️ Omni Reference 為 V7-only，V8.1 未沿用；如需角色一致改用 `--sref` 或 V8 對應功能）
- **範例（古代戰士）：**
```
cowboy shot, realistic, male warrior, ancient chinese, beard, long black hair, han_armor, chinese_armor, battleground, volume fog, cinematic pose --ar 97:128 --style raw --stylize 750
```
- **禁忌：** 長散文、`--stylize 1000 --chaos 100` 同開（mush）、`--style raw` 配 "artistic/dreamy"（自相矛盾）

### Nano Banana Pro / Nano（Gemini 3 Pro Image）
- **結構：** 自然段落，director-style 敘事
- **長度：** 80-150 字
- **簽名：**
  - `"quoted text"` = 字面渲染
  - **多圖 reference = superpower**（直接描述組合）
  - NO `--params`
- **禁忌：** 10-字短 prompt under-deliver、`--ar` 語法會被忽略

### Flux 1.1 Pro / Kontext
- **結構：** 長自然段 + 攝影技術詞
- **長度：** 80-200 字
- **簽名：** `Kodak Ektachrome 64 cross-processed`, `35mm spherical lens at f/5.6`, `Dappled forest light at 1/125`
- **Kontext edit：** `"Change her hairstyle to ..., maintain the exact same character and facial features"`
- **禁忌：** **Artist names 無效**（BFL 訓練時刷掉）、`--ar` 語法不支援

### Ideogram 3
- **結構：** `[scene], "quoted text" in [font] typography, [style]`
- **長度：** 40-80 字
- **簽名：** `"text"` 字面渲染（早放）、style preset (Auto/Realistic/Design/3D/Anime)
- **禁忌：** >5 字文字會 degrade、quoted text 放 prompt 尾

### Seedream 5.0（字節跳動）
- **結構：** 重要詞在前（early-weight）
- **長度：** 30-100 字
- **反直覺：** **英文 > 中文 for photoreal**；但中文 prompt 做中文字渲染更準
- **範例：**
```
Corporate headshot of a middle-aged Asian businessman, navy suit, neutral gray background, soft box lighting from 45-degree angle, sharp focus on eyes.
```

### 🆕 Nano Banana Pro（2026-05-19 補充 from [`Banana-Prompts/awesome-nano-banana-prompts`](https://github.com/Banana-Prompts/awesome-nano-banana-prompts) 51⭐）
- **核心發現：** Nano Banana 短句也能 work（單 sentence + style anchor）
- **短式範例：**
  ```
  3D floor plan: 2-bedroom apartment. Top-down view. Realistic furniture.
  ```
  ```
  Library design. Floor-to-ceiling shelves, rolling ladders. Dark wood. "Dark Academia."
  ```
  ```
  Tiny House interior. Efficient layout, loft bed, wood finish. Wide angle.
  ```
- **長式範例（複雜場景）：** 80-150 字自然段落仍 work
- **Style anchor 列表：** `Dark Academia`, `Modern Luxury`, `Brutalist`, `Bauhaus`, `Mid-Century Modern`, `Japandi`, `Coastal`, `Minimalist`, `Maximalist`
- **`"quoted text"` 字面渲染** — 確認仍是 Nano Banana superpower

---

## 🎵 音樂模型 per-model 簽名

### Suno v5.5（2026-05-19 大更新 — 1000+ prompts 證據）

> **Source：** [`naqashmunir21/awesome-suno-prompts`](https://github.com/naqashmunir21/awesome-suno-prompts) 37⭐ (CC0)，1000+ genre-organized prompts

- **結構：** 4-line body — `主風格 vocals/style` + `製作元素 production tags` + `情緒 energy` + `BPM + Key`
- **長度：** 30-80 字 / 4 行
- **🆕 BPM + Key 必標**（這是 v5 sweet spot）：
  - `BPM: 128, Key: C Major` — Pop / Dance
  - `BPM: 90, Key: A Minor` — Hip-Hop / Trap
  - `BPM: 140, Key: F# Minor` — EDM Festival
  - `BPM: 75, Key: D Major` — Ballad / R&B
  - `BPM: 170, Key: E Minor` — Drum & Bass / Hardcore
- **簽名 token by genre：**
  - **Pop：** `infectious pop anthem`, `female powerhouse vocals`, `layered harmonies`, `radio-ready polish`, `stadium-ready energy`
  - **EDM：** `massive drop-ready`, `EDM-influenced production`, `soaring vocal hooks`, `festival main stage energy`, `sub-bass that punches`
  - **Hip-Hop：** `boom-bap drums`, `crispy hi-hats`, `vinyl-sampled bass`, `confident delivery`, `street narrative`
  - **Rock：** `crunchy guitars`, `power chord riffs`, `pounding drums`, `arena-ready production`
  - **Country：** `acoustic guitar lead`, `slide guitar`, `storytelling vocal`, `pickup truck imagery`
  - **R&B / Soul：** `silky smooth vocal`, `Rhodes piano`, `808 bass`, `sensual mood`, `late-night vibe`
  - **Indie / Alt：** `lo-fi production`, `dreamy reverb`, `whisper vocal`, `bedroom pop`
  - **Jazz / Blues：** `walking bass line`, `swing rhythm`, `brass section`, `improvised solo`
- **Use Case + Energy tag 約定**：
  ```
  **Use Case:** TikTok viral hits, summer anthems
  **Energy:** 9/10
  **Notable Feature:** Perfect for social media campaigns
  ```
  雖然不會送進 Suno，但**標註幫助你日後 reuse**。
- **禁忌：** 同一行寫 >3 種衝突 genre（"jazz + EDM + country"）、模糊形容詞（"good music"）、無 BPM 反而易隨機

---

## 🎯 跨模型 workflow 組合（實戰）

### 寫實戰鬥短片（當前 use case）
1. **Nano Pro / Midjourney V8.1 + Hao0321 ref** → 生 key frame 圖（含武打氛圍）
2. **Kling 3.0 / Runway Gen-4.5 / Veo 3.1** 用該圖做 i2v（Sora 2 🔴 已停運，不再列為活平台）
   - Kling：40-80 字、handheld + whip pans
   - Runway Gen-4.5：25-60 字、`camera tracks from behind` + low-angle（接替 Sora 2 bodycam 寫實感）
   - Veo 3.1：加 `SFX: horse hooves, steel clash` + `Ambient: battle drums`
3. 需要更長敘事 → Wan 2.6 multi-shot `Shot 1/2/3` 結構（中文寫）

### 電商產品 15s
1. Nano Banana Pro 多圖（產品 + 背景 + 模特）
2. Veo 3.1 用圖做動畫 + SFX

### Poster / Logo（要文字準確）
1. Ideogram 3 `"PRODUCT NAME" in art deco typography`
2. 需要多語言文字（中日）→ 用該語言寫 prompt

---

## 📌 重要踩坑記（2026-04-21 實戰驗證）

1. **Seedance 英文戰鬥 prompt = 失敗** — 4 shot 擠 15s、導演名堆、fast 動詞、chaotic wide → 效果差
2. **修正方向**：中文 + 8 維公式 + 單鏡單動詞 + Constraints tail + 低角度跟馬側
3. **Cost 預覽 sanity check**：Seedance 15s pro = 210，10s = 140。顯示 140 → duration 被 reset
4. **"我想修改" flow 會 reset duration** → 一律用底部 prompt 工具列 settings 手動設

---

## 🔗 Sources（研究時的權威站）

- OpenAI Sora 2 Prompting Guide Cookbook（🔴 平台已停運，僅存歷史參考）
- Google Cloud - Ultimate Veo 3.1 Guide
- Runway Gen-4.5 / Aleph Prompting Guides
- fal.ai Kling 3.0 / Wan 2.6 / Seedream 5.0 prompt guides
- BFL Flux Kontext docs
- Ideogram prompting docs
- Midjourney V8.1 official docs (docs.midjourney.com)
- Google Cloud Nano Banana Ultimate Prompting Guide
- Atlas Cloud Seedance Library
- awesome-seedance-2-prompts (GitHub)
- 知乎 / 腾讯新聞 / CSDN 即夢 Seedance 官方手册全拆解
- Bilibili 鏡頭語言提示詞教學
