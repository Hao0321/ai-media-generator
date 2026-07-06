# Seedance — ByteDance 豆包影片模型

官方入口：Volcengine Ark / BytePlus ModelArk、即夢 (Jimeng)、豆包 (Doubao)、**OiiOii.ai**（聚合平台）。主力版本 **Seedance 2.0 pro**（最新）/ **Seedance 1.0 Pro** / **1.0 Lite**。Seedance 的殺手鐧是 **多鏡頭敘事 (multi-shot)** — 一個 prompt 內可指定多個鏡頭切換。

---

## 🆕 2026-05-18 社群證據版（推翻部分前期斷言）

**Source：** [`YouMind-OpenLab/awesome-seedance-2-prompts`](https://github.com/YouMind-OpenLab/awesome-seedance-2-prompts)（CC BY 4.0，107 個 curated prompts，3,172 tracked，2026-05-18 last update）

### ⚠️ 推翻：以下 2026-04-21 斷言是錯的（Seedance 2.0 已升級）

| 我之前說 | 實際是 | 證據 |
|---|---|---|
| ❌ `cinematic` 是 generic 不能用 | ✅ Seedance 2.0 **大量吃** `cinematic` | Featured #1 #3 #6 + Wing Chun + Coca-Cola + 50%+ 社群 prompt 都用 |
| ❌ 35mm / 50mm 鏡頭沒效果 | ✅ **焦段 token 有效** | Wing Chun: `close 35mm follow shot, 50mm lateral tracking, 28mm push-in shot` |
| ❌ 4+ shots 擠 15s = 災難 | ✅ **3-5 shots/15s 是甜蜜點**，前提是有時間區塊 | 6 Featured 全部 3 shots×5s 結構 |
| ❌ 長度 60-100 字 | ✅ **單鏡 60-100 / 多鏡 200-700 字** | Featured #1 ~600 字、#4 ~400 字、Wing Chun ~400 字 |
| ❌ 所有導演名都爛 | ⚠️ **分流**：藝術運動/品牌風格 ✅，個別 DP 名 ⚠️ | ✅ `Pixar style / Studio Ghibli / 90s anime film / Fast and Furious inspired / Bloom & Wild / Eric Chauvin`；⚠️ 純 DP 名仍弱證據 |

### ✅ 仍然正確

- ONE verb per shot（時間區塊內單動作）
- Constraints tail 必放
- Anchor-noun 一致
- 中文 > 英文（武打/古裝/romance）— Featured #1 #4 #5 都是中文
- `fast` 仍是垃圾詞 — 改用 `extreme speed / high-octane / rapid / kinetic / explosive acceleration`
- Subject 前 10-15 token early-weight

### 🆕 新發現的關鍵 pattern（**這次寫法重點**）

#### 1. Bracketed labels（方括號層級）是 Seedance 2.0 主流結構

```
[Style] <one-line stack of style tokens>
[Duration] 15 seconds
[Scene] <setting>
[Character] <consistent character card>
[Shot 1: 00:00-00:05] <title>
  Visuals: ...
  Action: ...
  Details: ...
  Special Effects Details: ...
[Shot 2: 00:05-00:10] ...
[Shot 3: 00:10-00:15] ...
[Audio] / Sound design: ...
[Constraints] / Camera / Negative: ...
```

時間區塊格式不限：`[00:00-00:05]` / `(0-2s)` / `[00–05s]` / `0-3 seconds:` / `[0–2 SEC]` 全部都吃。

#### 2. Sub-labels within shot — Seedance 解析力強

每個 shot 內可以分：`Visuals:` / `Action:` / `Details:` / `Atmosphere:` / `Special Effects Details:` — Seedance 會分別處理。

#### 3. Quoted dialogue 會自動 lip-sync + 加字幕

- Featured #1: `"What are you looking at?"` → 真的 lip-sync
- Featured #5（rapper MV）：`[Rap Lyrics] "Eighty-year-old legs..."` → 嘴型對得到
- Demon Lord 範例：日文 `「今日は世界を滅ぼさない。」` + `Subtitle: "Today, I'm not destroying the world."` → 雙語字幕

#### 4. Format anchor（學 Sora 2 的概念）— Seedance 2.0 也吃

- `ESPN-style 16:9 courtside camera`（→ 體育轉播質感）
- `Hollywood live-action anime adaptation`
- `Sony A7S3/cinema camera`
- `IMAX Fantasy Camera`
- `Dolby Atmos`
- `Unreal Engine 5 fluid rendering`
- `BBC documentary style`

#### 5. Format / Resolution 前置一行

```
Format: Vertical 9:16 • UHD 8K • IMAX Fantasy Camera • Dolby Atmos • Hyper Realism
```

`8K / 4K / UHD / 1080p` 都吃。⚠️ 2026-04-21 我說「`8K` 是 generic」是錯的，Seedance 2.0 對解析度 token 有反應。

#### 6. Audio Profile（4-modal Seedance 2.0 殺手鐧）

Seedance 2.0 支援**原生音訊**生成。Audio 段必寫：

```
Sound design: fabric friction, realistic wooden impacts, air swishes, breathing, 
stone footwork, subtle ambient wind, bamboo rustling.
```

或 sectioned：

```
Audio Profile (visualized):
- Ambient: deafening seismic roar, crackle of plasma-ignited ash
- SFX: sharp metallic ping of crystalline wings  
- Music: high-tempo taiko drum + electric guitar crescendo
- Dialogue: low-energy whispering, 200-400ms pauses
```

#### 7. Negative as exclusion list

```
No anime, no CGI look, no digital painting, no commercial aesthetic.
no deformation, drift, or artifacts.
no text, watermarks, or subtitles.
no historical costume drama feel.
no gore.
```

⚠️ 仍然不能用 `--no blur` 這種 flag 語法，但 `no X` 自然語言 OK。

#### 8. Lip-sync directive（角色 dialogue 時必加）

```
Lip synchronization is natural and precise, 
emotional micro-tremors and breathing are synchronized, 
dialogue is low-energy whispering with a shy tone, 
natural short pauses between 200-400 milliseconds, 
the mouth only moves slightly when speaking, 
without exaggeration or robotic feel.
```

#### 9. Author attribution = social-proof token

社群實戰 prompt 都註明創作者 X handle。雖然不影響生成品質，但**追蹤誰的 prompt 好用**是元學習。

### 📚 完整 community-validated playbook 範本

#### 範本 A：3-Shot Cinematic 15s（最常見格式）

```
[Style]
<stack of 5-7 style tokens: photorealistic, cinematic, 4K/8K, lens, lighting style, mood>

[Duration] 15 seconds

[Scene]
<location + atmosphere + key environmental elements>

[Character]
<consistent character card — appearance + clothing + key features>

[Shot 1: 00:00-00:05] <shot title>
Visuals: <wide/medium/close-up + key visual element>
Action: <ONE verb + subject behavior>
Details: <specific micro-detail>

[Shot 2: 00:05-00:10] <shot title>
Visuals: ...
Action: ...
Details: ...

[Shot 3: 00:10-00:15] <shot title>
Visuals: ...
Action: ...
Atmosphere: <ending feeling>

[Audio]
Sound design: <ambient + SFX + music + dialogue>

[Constraints]
no deformation, drift, no extra text/watermarks, locked horizon, stable temporal coherence.
```

#### 範本 B：Single-shot 5-10s（簡短描述）

```
<format anchor + style stack>. <Subject early-weight + 1 visual chip>. 
<Camera move + lens>. <Lighting + atmosphere>. <ONE action>. 
<no X negatives>. <resolution>.
```

例：

> `Cinematic action sequence on top of speeding armored trucks racing through a neon dystopian megacity during torrential rain, holographic billboards flickering through smog. Camera mounted inches above wet asphalt weaving through traffic. Rain streaks aggressively across the lens. Slow-motion impact shakes the truck violently. No distortion, no stretching. 4K ultra-realistic.`

### 🎬 範例：6 Featured Prompts 完整收錄

→ 見 [../templates/preset-packs.md §Seedance 2.0 Community Featured](../templates/preset-packs.md)

涵蓋：
- Japanese romance（中文）— Featured #1
- Hollywood haute couture fantasy（中文）— Featured #2
- Modern rural healing（中文）— Featured #3
- Live-action anime duel（中文）— Featured #4
- 80-year-old rapper MV（中文）— Featured #5
- Street racing（英文）— Featured #6

### 📊 107 Prompts 分類

| 類別 | 數量 | 代表 |
|---|---|---|
| Cinematic drama/romance | 12 | Japanese Romance, Wing Chun |
| Cinematic action/chase | 18 | Truck Chase, Street Racing, Sci-Fi |
| Anime style | 8 | 90s Demon Lord, Samurai duel |
| Sports broadcast | 7 | NBA, FIFA, IPL, Football |
| Music video | 6 | Rapper MV, K-pop, EDM |
| Commercials | 14 | Coca-Cola, Watch, Food, Luxury |
| 3D/Animation | 9 | Pixar Kitten, Stop Motion |
| Lifestyle/Vlog | 8 | Cooking, Fitness, Travel |
| Fantasy/VFX | 13 | Dragons, Magic, Transformations |
| Cyberpunk/Sci-fi | 7 | Tokyo 2099, Exosuit |
| Storyboard animation | 5 | Botanical, Fitness collage |

---

## ⚡ 2026-04-21 實戰鐵律（保留 — 已被 2026-05-18 更新部分推翻）

**4 次 Seedance 2.0 pro 戰鬥 prompt 實測 + 跨平台社群研究的結論：**

### 8 維 Prompt 公式（不是官方的 5 維）

```
Subject + Action + Scene + Light + Camera + Style + Quality + Constraints
```

前 5 維官方有說，**後 3 維（Light、Quality、Constraints）是社群實戰補的，特別是 Constraints tail 是 Seedance 穩定的關鍵**。

### Seedance 的 5 鐵律

1. **ONE verb per shot** — 多動詞同句 = 模型自選 = 災難（"charging + swinging + slashing" 三動詞同鏡必爛）
2. **中文 > 英文（武打/古裝）** — native multilingual encoder，`古代將軍騎黑馬` 比 `ancient general on black horse` 穩
3. **Constraints tail 必放** — `不抖動、不變形、不多肢、穩定地平線、穩定時間一致性` / `no shake, no warping, no extra limbs, locked horizon, stable temporal coherence`
4. **Subject 前 10-15 token** — 主體必須在開頭（early-weight）
5. **Anchor-noun 字串一致** — 多鏡頭時同物件用**完全相同**名稱（"紅披風" 不能下一鏡變 "紅色斗篷"）

### Seedance 七大垃圾模式（避開！）

| 垃圾模式 | 為何爛 | 正解 |
|---|---|---|
| 1. 英文導演名（`Kurosawa-style`）| Seedance 對英文導演名 weak signal | 改用具體視覺詞（墨色、留白、金色逆光、大遠景）|
| 2. 技術參數堆（`35mm T1.4 Kodak 500T`）| Seedance 沒對這些 token 訓練 | 改用語意詞（低角度、慢鏡頭、晨霧）|
| 3. 4+ shots 擠 15s | 每 shot <4s = carousel 感 | 15s = 1 個主 shot（Seedance 2.0 擅長單鏡長 take）|
| 4. 抽象動詞（`fights bravely`）| 模型不懂抽象勇敢 | 具體身體動作（`右手舉劍前刺，左手握韁`）|
| 5. `fast` 關鍵詞 | 最爛關鍵詞，degrades quality | `slow-motion / 緩慢 / smooth / sustained`|
| 6. Speed-ramp（`fast then slow`）| 不支援 | 分 shot 或捨棄 |
| 7. 多主體獨立動作（A 打 B 閃 C 射）| 模型三者都 fail | 單主體 + 背景群眾 |

### 騎馬戰鬥專屬 tip（今天實戰驗證）

- ✅ **低角度 tracking dolly 沿馬側**跑 + dust/steam 粒子 + locked horizon + 1 鏡慢動作
- ❌ **chaotic wide cavalry charge**（千軍衝鋒大遠景）= 失敗率最高
- ✅ **Reference video cheat**：上傳 ≤15s 動作參考 + `imitate the movements of @video1` — 中國創作者公認武打最大 quality jump

### 戰鬥 three-beat（多鏡頭結構）

```
Wide (建立間距) → Medium (跟拍節奏) → Close-up (衝擊 / 呼吸 / 腳步)
```

### 實戰範例（2026-04-21 第 4 次成功 prompt，~130 字）

```
古代中國將軍 Hao0321 主角，身披金色鱗甲、紅披風在風中翻飛，手持青銅寶劍前舉，騎黑色戰馬在黎明戰場上緩慢衝鋒。戰場遠處千軍列陣、紅色戰旗獵獵、塵霧漂浮。金色逆光晨霧、熒光輪廓。低角度手持 tracking 鏡頭沿馬側跟拍，輕微晃動、穩定地平線。墨色留白東方美學、史詩寫實電影感。慢動作、流暢、連貫、不僵硬、720p 高清。不抖動、不變形、不多肢、穩定地平線、穩定時間一致性。
```

對應 8 維：
- Subject: `古代中國將軍 Hao0321 主角` ✓
- Action: `騎黑色戰馬在黎明戰場上緩慢衝鋒`（ONE verb：衝鋒）✓
- Scene: `黎明戰場、千軍列陣、紅色戰旗、塵霧` ✓
- Light: `金色逆光晨霧、熒光輪廓` ✓
- Camera: `低角度手持 tracking、馬側跟拍、穩定地平線` ✓
- Style: `墨色留白東方美學、史詩寫實電影感` ✓
- Quality: `慢動作、流暢、連貫、不僵硬、720p 高清` ✓
- Constraints: `不抖動、不變形、不多肢、穩定地平線、穩定時間一致性` ✓

**長度甜蜜點：** 60-100 字（useful 30-200 字）。本例 ~130 字略長但因 Constraints tail 必要故 OK。

### 成本參考（OiiOii 即時計費）

- Seedance 2.0 pro 15s = **210 STAR**
- Seedance 2.0 pro 10s = **140 STAR**
- ⚠️ 「我想修改」flow 會 reset duration 到 10s（見 automation/site-profiles/oiioii.md §12.9.9）

詳完整社群研究證據：[community-prompt-patterns.md](community-prompt-patterns.md)

---

## Prompt 公式 (官方推薦)

最簡：`Subject + Action`
完整：`Subject + Action + Scene + Camera + Style`

⚠️ **2026-04-21 實戰補充：Constraints + Light + Quality 這 3 維官方沒寫但是必要**。見上面「8 維公式」。

混搭可自由組合，不用每段都寫。

```
A wolf (subject)
howls at the moon (action)
on a snowy mountain peak at night (scene)
low-angle static shot then slow dolly-in (camera)
cinematic, cold blue palette, volumetric moonlight (style)
```

## 多鏡頭語法 (Seedance 強項)

用 `Cut to`、`Camera cut to`、`Camera switching` 連接不同鏡頭。每個鏡頭獨立描述。

```
A fisherman mends his nets on the dock at dawn, medium shot, warm golden light.
Cut to: close-up of his weathered hands working the rope.
Cut to: wide shot of his small boat bobbing in the harbor, birds circling above.
Cut to: low-angle shot as he pushes the boat out, sun rising behind him.
```

**要點：**
- 每個 shot 都要自成段落 (subject + scene + camera)
- 保持主體一致 (同一個漁夫；不要第二鏡切成不同人)
- 鏡頭之間的邏輯要通 (時間順序、空間合理)
- 建議 3–5 shots，10s 片段最多 5 個

## 版本差異

| 版本 | 特色 | 費用 |
|---|---|---|
| **Seedance 1.0 Pro** | 高畫質、多鏡頭最準、運鏡最豐富 | 較貴 |
| **Seedance 1.0 Pro-Fast** | Pro 的加速版 | 中 |
| **Seedance 1.0 Lite** | 快、便宜、簡單場景可 | 最低 |
| Seedance 2.0 (若已推出) | 下一代，預計運鏡與長度提升 | — |

## 模式

- **T2V** / **I2V**
- **時長**：5s / 10s (Pro 支援 10s)
- **解析度**：720p / 1080p
- **FPS**：24
- **Aspect**：16:9 / 9:16 / 1:1

## 運鏡詞 (Seedance 支援很全)

Seedance 精準執行 standard 運鏡詞：
- `push in / pull out` (= dolly in/out)
- `pan left / right`
- `tilt up / down`
- `tracking shot` / `follow shot`
- `orbit / arc`
- `crane up / down`
- `zoom in / out`
- `aerial shot / drone`
- `static shot`
- `handheld`

**進階：** 長鏡頭可組合 (Seedance 原生支援)
```
Long take: starts with aerial wide shot descending, then transitions to
tracking shot following the subject through the market, ends on medium close-up.
```

## 風格 token (原生支援)

Seedance 直接生成多種藝術風格，不需要 "in the style of":
- `2D animation` / `3D animation`
- `voxel art`
- `pixel art`
- `felt craft` / `clay animation`
- `illustration` / `watercolor`
- `anime` / `cel-shaded`
- `photorealistic cinematic`
- `ink wash painting` (水墨) — 中英都懂

## 中英文支援

- **中文 prompt** 原生支援，對中國文化場景 (漢服、武俠、京劇、水墨) 效果比英文好
- **英文** 對西方場景、電影術語較準
- **混用策略**：主體+文化元素用中文，運鏡+風格 token 用英文
  ```
  一位穿漢服的少女在竹林中跳舞，袖子隨風飛舞，slow dolly-in tracking shot,
  cinematic, Wong Kar-wai style, warm golden hour
  ```

## 範例

**1. 多鏡頭短故事 (10s)**
```
A lonely lighthouse keeper climbs the stairs at dusk, medium shot from behind,
warm interior light. Cut to: close-up of his hands lighting the old lantern.
Cut to: wide shot of the lighthouse beam cutting through the storm, aerial view.
Cinematic, melancholic, deep blue and amber palette.
```

**2. 風格 — 黏土動畫**
```
A tiny clay robot walks across a kitchen counter, picking up a cookie with both
hands. Close-up tracking shot. Felt craft and clay animation style, warm
tabletop lighting, shallow depth of field.
```

**3. 中式武俠**
```
兩名武俠在竹林中對峙，長袍隨風飄動。
Cut to: close-up of the master's eyes narrowing.
Cut to: 武俠 A 拔劍出鞘，竹葉紛飛.
Cut to: low-angle wide shot as they leap toward each other.
Ink wash painting aesthetic, dramatic lighting, slow motion moments.
```

## 連結

- 官方 Prompt Guide (BytePlus)：https://docs.byteplus.com/en/docs/ModelArk/1631633
- Seedance 1.0 Lite Guide：https://docs.byteplus.com/en/docs/ModelArk/1587797
- 民間整理 (Atlabs 2026)：https://www.atlabs.ai/blog/ultimate-seedance-1-pro-prompting-guide
- WaveSpeedAI (Seedance 2.0 trailer page)：https://wavespeed.ai/landing/seedance
- Scenario help：https://help.scenario.com/en/articles/seedance-models-the-essentials
- Seedance 官方入口 (第三方彙整)：https://www.seedancepro.net/seedance/prompt-guide
- JoyPix AI Lite 指南：https://www.joypix.ai/tutorials/how-to-write-seedance-1.0-lite-prompt/
