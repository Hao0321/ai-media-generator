# 海螺 Hailuo 2.3 Pro/Std 參考

官方 https://hailuo.ai/。**當前主力 = Hailuo 2.3 Pro / Std**（MiniMax）。擅長 **微表情 / 角色表演 / 動作戲**，市場認可的「角色演員 AI」，5s 影片畫面內容密度與情緒飽和度頂級。

## 目錄
1. [版本差異矩陣](#版本差異-20265-時點--能力矩陣)
2. [Prompt 公式](#prompt-公式)
3. [簽名詞彙](#簽名詞彙--微表情--動作--情境)
4. [參數](#參數)
5. [強項 / 弱項](#強項--弱項)
6. [常見失敗 + 修法](#常見失敗模式--修法)
7. [3 個實戰範例](#高品質範例)
8. [連結](#連結)

---

## 版本差異（2026/5 時點）— 能力矩陣

| 版本 | 時長 | 解析度 | 強項 | 何時用 |
|---|---|---|---|---|
| **Hailuo 2.3 Pro** | 5s | 1080p | 角色微表情精細度極高、單人情緒戲 | 人物特寫、演員表演、表情細節 |
| **Hailuo 2.3 Std** | 6s / 10s | 720p | CP 值、多秒數、動作戲 | 動作序列、佛系便宜跑 |
| **Hailuo 2.2** | 5s / 10s | 720p | 舊版，漸棄（仍可用，不推） | — |

⚠️ **OiiOii 上 Hailuo 時長數字待驗 —— 首次用以站內 UI 實測為準。**

---

## Prompt 公式

**核心心法：「導演的 AI」，要劇本不要清單。**

**最簡：** `現在式敘述 + 一個形容詞`

```
A woman gazes out a rain-soaked window, sadness in her eyes, rain streaking the glass.
```

**完整五段：**

```
[主體 + 開場姿態]
[單個核心動作（現在式）]
[環境 + 時間 + 氛圍]
[鏡頭 + 打光]
[風格 + 色調]
```

**Hailuo 獨家重點：**
- **用現在式動詞**（`walks / turns / reaches / closes`），避免摘要式形容（❌ `confident stride` → ✅ `walks forward, shoulders back, eyes fixed`）
- **只寫「變化的部分」** —— 起始姿態 + 結束動作，中間過渡讓模型補。
- **微表情是主力 token** —— `subtle lip quiver / eyebrow furrow / slight head tilt / tear forming` 生效率極高。
- **不要堆疊數量詞彙**（避 `8k masterpiece high quality`），改用**情感節奏** + **自然敘事**。

**Hailuo Prompt 實例：**

```
A young man in a grey hoodie sits alone in a dim bedroom. He slowly reaches for his phone on the nightstand. His hand hesitates. After a beat, he looks at his reflection in the dark screen, and a faint, melancholic smile crosses his face. Warm amber bedside light casts soft shadows. Slow dolly-in on his face. Cinematic, intimate, film grain.
```

---

## 簽名詞彙 — 微表情 / 動作 / 情境

**微表情詞（Hailuo 極敏感）：**
- `subtle blink / double-take`
- `eyebrow raises slowly / furrows / quirks`
- `lip pursed / quivering / parted in breath`
- `tear forming / rolling down`
- `nostrils flare / breathing visible`
- `jaw clenches / relaxes`
- `slight head tilt / nod`
- `eyes widen / narrow / soften / harden`
- `micro-smile / knowing glance`

**動作詞（一個鏡頭一動詞）：**
- `walks / runs / stumbles / sways`
- `reaches / grasps / releases / extends`
- `turns / pivots / spins (slow/abrupt)`
- `sits / stands / kneels / crouches`
- `speaks / whispers / gasps / sighs`
- `dances / stumbles / gestures`
- `falls / catches / slides`

**運鏡慣用語（Hailuo 穩定的組合）：**
- `slow dolly-in on face` — 最穩，帶張力
- `tracking shot from behind` — 跟隨角色
- `static wide shot, subject walks into frame` — 經典入鏡
- `handheld micro-movement` — 紀實感
- `rack focus from background to face` — 情緒聚焦

**情緒/時間 tag（支援但勿濫用）：**
- `melancholic / contemplative / tense / joyful / conflicted`
- `dawn / dusk / late night / golden hour`
- `slow motion (key beats only)` — 一次一個瞬間

---

## 參數

- **時長**：Pro = 5s；Std = 6s / 10s
- **解析度**：Pro = 1080p；Std = 720p
- **CFG / 創意**：通常 0.5（預設）；若角色漂移升到 0.6–0.7
- **負面 prompt**：支援（見下方常用）
- **Ratio**：16:9 / 9:16 / 1:1

---

## 強項 / 弱項

### ✅ Hailuo 最強的場景

1. **人物微表情** — 眼神變化、淚水、嘴角弧度、呼吸可見，對標 Runway Act-Two 級
2. **角色情緒戲** — 獨白、凝視、內心獨白型動作（思考、掙扎、頓悟）
3. **單人或雙人對視** — 眼神接觸、對話、互動高保真
4. **動作層級** — 武打招式、舞蹈、精確身體動作（不及 Kling CoT 物理但穩定）
5. **短鏡頭密度** — 5s 內故事感濃、節奏緊

### ❌ Hailuo 弱項

1. **複雜多主體場景** — 3+ 人同時動作會變形或互相干擾
2. **複雜環境物理** — 液體、布料、碎屑動態不如 Veo 3.1 / Runway
3. **寬景深敘事** — 開始圖偏 close-up（i2v 輸入若是寬景 Hailuo 可能縮變遠）⚠️ **不要餵 wide shot 當首幀**
4. **密集英文詞彙** — 中文 prompt 出片率仍略高（多語模型不夠純化）
5. **10s 長鏡** — Std 雖支援但穩定度下降；Pro 上限 5s 本身就設計短精

---

## 常見失敗模式 + 修法

| 症狀 | 成因 | 修法 |
|---|---|---|
| 角色臉型 / 表情在動畫中變形、換臉 | prompt 內重複敘述臉部長相；i2v 時輸入圖無關鍵角度 | 用 i2v + 清晰 close-up 首幀；prompt **只**寫動作 + 情緒，絕不描述臉型 |
| 預期動作沒出現，生成靜態或隨便動 | prompt 太抽象（`confident movement` 而非具體動作）；或動作詞太多同句（模型自選變隨意） | 具體動作詞 + 現在式 + 因果順序：`reaches for X, hesitates, then grasps firmly` |
| 輸入 wide shot i2v → 角色縮小或畫面重組 | Hailuo i2v 偏好 medium/close-up 首幀 | 首幀改中景或特寫；若一定要 wide，提前告知 → t2v 走 close-up 別 i2v |
| 多人場景人物漂移或相互擁擠 | 一句裡 2+ 人獨立動作 | 拆成多鏡（Std 可 10s 接力）；或只寫主角動作，配角淡出背景 |
| 運鏡幌動、失控 | 一個 prompt 疊 3+ 個運鏡詞（pan + dolly + zoom）；或用亂飛的詞彙（`infinite movement` / `chaotic camera`） | 一鏡一運鏡；明確給「慢動作」modifier：`slow dolly-in` / `gentle handheld` |
| 看起來 AI 感強、不自然 | 堆 `masterpiece / 8k / photorealistic` 質量詞 + 關鍵字湯語法 | 改自然敘事：「一個女人凝視窗外，淚光在眼角，晨霧籠罩街道」（去掉修飾器，用動作/細節說話） |

---

## 高品質範例

### 1. 演員特寫 — 淚水與凝視

```
A woman in her 30s sits in soft golden afternoon light, gazing out a window. 
Her eyes slowly widen as realization dawns. A tear forms at the corner of her 
right eye, then slides down her cheek. Her hand rises to her face, fingers 
gently touching her jawline. Slow dolly-in on her face. Intimate, film grain, 
warm amber light, quiet moment of vulnerability.
```

**為何好：** 微表情（eyes widen / tear forms）+ 具體動作（hand rises，touching）+ 運鏡（slow dolly-in）+ 光線，五層都寫。角色清晰當主角。

### 2. 動作戲 — 格鬥對峙

```
A martial artist in black stands in a martial arts training hall. 
Her opponent charges forward. She pivots left, her long braid swinging sharply 
as her body rotates. She extends her palm in a controlled strike, held steady 
for a beat. The impact reverberates through her arm. Her breathing is visible. 
Static wide shot transitioning to slow tracking shot following her movement. 
Cinematic, dappled window light, dramatic contrast.
```

**為何好：** 動作分解（charges / pivots / extends / strike）+ 生理細節（braid swinging / breathing visible）+ 時序（held steady for a beat）。不說「fast attack」，改具體招式。

### 3. 內心獨白型 — 思考的瞬間

```
A man in a coffee shop holds an old handwritten letter. He unfolds it slowly, 
studying the words. His eyes scan line by line. Eyebrows furrow. He closes his 
eyes, and a subtle smile emerges—bittersweet. He folds the letter back carefully, 
holding it to his chest for a moment before placing it on the table. Warm cafe 
lighting, shallow depth of field. Slow push-in on his face. Contemplative, 
intimate, film noir.
```

**為何好：** 無對話純動作講故事；微表情（furrow / subtle smile）+ 物件互動（unfolds / studies / folds）+ 運鏡聚焦（push-in on face）。5s 內完整情緒弧線。

---

## 連結

- 官方：https://hailuo.ai/
- OiiOii 嵌入：https://oiioii.ai/（Hailuo 模型列表）
- 2.3 announcement (MiniMax blog)：https://www.minimaxi.com/（非英文站）
- 舞蹈/表演對標：vs Runway Act-Two · vs Kling Motion Control
- Hailuo 2.3 vs 2.2 變化追蹤（X / Reddit）

---

**使用建議**

- **角色表演優先 Hailuo**，不是萬用 workhorse。複雜場景 / 多鏡敘事改 Seedance / Veo / Kling。
- **Std 對 CP 值友善**，但 10s 長鏡穩定度未驗證 OiiOii —— 首次用確認。
- **i2v 入手若有形狀護需**，中景 close-up 首幀最穩；wide shot i2v 易縮變遠。
- **中文 prompt 可 optional —— 英文目前證據足，中文補集未充分測過。**

---

## 反向詞彙（Negative Prompt）常用

```
blurry, distorted face, warped features, unnatural expression, plastic skin,
oversaturated, flat lighting, jittery motion, unnatural body anatomy,
extra limbs, merged hands, low quality, watermark, text overlay, CGI look,
stiff movement, jerky animation, morphing features
```

中文版：
```
模糊、臉部扭曲、不自然表情、塑膠皮膚、過飽和、死板光線、
卡頓動作、不自然解剖、多肢、融合手、浮水印、生硬動作
```
