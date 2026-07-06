# 反瑕疵品質控制 Playbook (Quality Control / Anti-Flaw)

**定位：** 當生成結果「看起來爛」「有瑕疵」「不夠精緻」，這裡是**系統化排查 + 修法**。不是換 prompt 賭運氣，是針對**已知失敗模式**對症下藥。

**核心心法：** 90% 的「廣告級瑕疵」來自 3 個根因 ——
1. **時間不穩定** (temporal instability)：物件在 frame 間變形 / 閃爍 / 漂移
2. **物理不可信** (physics break)：液體、布料、光影、重量感不對
3. **主體完整性破壞** (subject integrity loss)：產品 logo / 文字 / 形狀在動畫中走樣

對應 3 招：**鎖時間、鎖物理、鎖主體**。下面逐一拆。

---

## 0. 先分類瑕疵 — 對症才能下藥

看到爛結果，先問「是哪一類瑕疵」：

| 症狀 | 類別 | 跳到 |
|---|---|---|
| 產品/臉/手在影片中途變形、扭曲、長出多餘部分 | 主體完整性 | §1 |
| 畫面閃爍、抖動、材質沸騰 (texture boiling) | 時間穩定 | §2 |
| 液體/煙/布/水花動得很假、像果凍 | 物理可信度 | §3 |
| 文字/logo 變成鬼畫符 | 文字渲染 | §4 |
| 整體「塑膠感」「AI 感」「廉價感」 | 質感分級 | §5 |
| 運鏡亂飄、多鏡頭硬切突兀 | 運鏡控制 | §6 |
| 構圖死板、主體置中無聊、留白怪 | 構圖 | §7 |

**多重瑕疵 → 從 §1 主體完整性 先修**（最毀廣告），再往下。

---

## 1. 主體完整性 (Subject Integrity) — 廣告最致命

產品廣告的鐵律：**產品本身絕不能變形**。客戶能容忍背景普通，不能容忍 logo 歪、瓶身扭、鞋子變鞋。

### 根因
- 模型對「主體」沒有強約束，動畫過程中重新詮釋形狀
- Prompt 把太多注意力放在運鏡/氛圍，主體描述反而薄弱
- 運鏡太劇烈（快速 orbit / 大角度）→ 模型每幀「重新想像」產品

### 修法（prompt 側）
1. **主體描述放最前 + 給足細節錨點**：材質、輪廓、剛性。
   - ❌ `a perfume bottle rotating`
   - ✅ `a rigid glass perfume bottle with a fixed metallic cap, solid unchanging form, the bottle maintains exact shape and proportions throughout`
2. **明寫「形狀不變」指令**（多數新模型吃）：
   - `the product maintains consistent shape, label, and proportions across all frames`
   - `rigid object, no deformation, no morphing, structural integrity preserved`
3. **限制運鏡幅度**：產品鏡頭用 `slow`、`subtle`、`gentle`，避免 `fast orbit 360`。一次最多 180° 環繞。
4. **i2v 優於 t2v 做產品**：先用 Seedream 5.0 / Nano Pro / MJ 定死產品 hero 圖 → i2v 只動畫化。**這是產品廣告最可靠路徑**，但要校準期待 ↓。

> **⚠️ 實測校準（2026-06-05 OiiOii Seedance i2v）：i2v 鎖形狀 ≠ 完美凍結。**
> 用 Seedream 5.0 hero（精華液瓶）→ Seedance 2.0 pro i2v 實測：**構圖 / 材質 / 打光 / 配色 / 主體類型忠實沿用**，但**瓶身比例仍中度漂移**（影片版較寬、液位變、自己長出水珠）。
> 結論：i2v 把「這是什麼 + 什麼質感」鎖得很好，但「精確輪廓比例」在 Seedance 仍有重繪空間。
> **要更嚴格的形狀凍結：** (a) hero 圖背景極簡乾淨（少干擾）；(b) i2v prompt 明寫 `產品 rigid 形狀不變`；(c) 改用 **Kling 3.0 Start Frame**（首幀鎖定比 Seedance i2v 更硬）+ Motion Brush 只刷背景/光，產品本體不刷 = 幾乎不動；(d) 運鏡越小（subtle dolly > 大 orbit）漂移越少。

### 修法（negative 側，支援的模型）
```
deformed product, morphing shape, warping geometry, melting object,
inconsistent label, distorted logo, shape-shifting, structural collapse,
liquid product (若是固體), bending rigid object
```

### Seedance / Kling 專屬
- Seedance 2.0：產品鏡頭關鍵字 `solid form`、`rigid structure`、`product integrity`；運鏡用「ONE camera motion per shot」
- Kling：用 **Start Frame 鎖死產品圖** + Motion Brush 只刷「要動的部分」（如水花），產品本體不刷 = 不動

---

## 2. 時間穩定 (Temporal Stability) — 閃爍/抖動/沸騰

### 症狀
材質表面「沸騰」(texture boiling)、細節在幀間跳動、整體閃爍、地平線漂移。

### 根因
- 太多高頻細節（密集紋理、大量小物件）模型每幀重算不一致
- 缺乏「穩定」指令
- 解析度/模型檔次不足以 hold 住複雜場景

### 修法
1. **明寫穩定指令**：
   - `temporally stable, consistent textures across frames, no flickering, steady detail`
   - `locked-off composition` (如果是固定機位)
   - `stable horizon, no drift`
2. **減少高頻雜訊**：背景簡化（純色 / 漸層 / 柔焦 bokeh），讓模型專注主體。「純黑虛空」「攝影棚無縫背景」是穩定神器。
3. **降低同時運動的元素數量**：一個鏡頭最多 1-2 個主要運動。
4. **加 motion blur 詞**：`subtle motion blur` 反而讓抖動看起來像有意的動態，不是 bug。
5. **升檔次**：Seedance pro > fast；Kling Pro > Std；解析度拉到模型上限。

### Negative
```
flickering, texture boiling, temporal artifacts, jittery, frame stutter,
shimmering, strobing, unstable detail, popping, ghosting
```

---

## 3. 物理可信度 (Physics) — 液體/布料/煙/重量

廣告最容易露餡的地方。AI 的水花、布料、煙常常「動得很假」。

### 通用修法
- **明寫物理引擎級詞彙**（新模型理解）：
  - `physically accurate, realistic weight and momentum, natural gravity`
  - `ray-traced fluid simulation`（流體）
  - `cloth simulation with natural drape and inertia`（布料）
  - `volumetric smoke with natural turbulence and dissipation`（煙）
- **給時間感**：慢動作讓物理更可信 — `extreme slow motion, high-speed camera capture, 1000fps detail`
- **參考真實攝影**：`macro product photography, commercial beauty shot` 觸發模型的「廣告攝影」先驗，物理較準

### 分材質速查

| 材質 | 關鍵 token | 常見 bug | 修法 |
|---|---|---|---|
| 水/液體 | `crown splash, surface tension, caustics, ray-traced refraction` | 像果凍、不連續 | 加 `slow motion` + `physically accurate fluid` |
| 玻璃 | `subsurface scattering, refraction, specular highlights` | 反射閃爍、穿透錯 | 簡化背景、固定光源 |
| 金屬 | `brushed metal, anisotropic reflection, metallic specular` | 反射沸騰 | `stable reflections` + 慢運鏡 |
| 布料/絲 | `flowing fabric, natural drape, cloth inertia` | 僵硬或亂飄 | `gentle motion` + `natural weight` |
| 煙/霧 | `volumetric, turbulent flow, natural dissipation` | 像棉花糖 | `wispy, fine particulate, god rays` |
| 食物 | `fresh, glistening, steam rising, appetizing` | 塑膠感 | `natural texture, subsurface scattering, soft key light` |

### Veo / Sora-class（強物理）
Runway Gen-4.5、Veo 3.1 物理理解最強。複雜流體/碰撞優先用這兩個，不要用弱物理模型硬做。

---

## 4. 文字渲染 (Text) — logo / 字幕 / 包裝字

### 鐵律
- **影片模型幾乎都不會渲染清晰文字**。Seedance/Kling/Runway 的文字 = 鬼畫符。
- **⚠️ 例外（2026-06-08 實測）：Google Gemini Omni Flash（Flow）能渲染短英數品牌名**。引號標注 + 指定載體（招牌/燈籠）成功出「Hao0321」清楚可讀。適用 ~7 字內短店名；長句仍不可靠。Veo 3.1 同級亦較強。其他影片模型仍守鐵律。
- **要精準文字 → 圖像階段先做好**：Ideogram 3.0（文字王）/ Seedream（中英）/ MJ V8.1（改善但仍弱）。
- **影片 prompt 明寫 `no text, no logo, no captions`** 反而乾淨 —— 讓後製加字。

### 產品包裝字
- i2v：用 Ideogram/Seedream 把包裝字做對 → i2v 動畫化（字已在圖上，模型只需 hold 住，比生成準）
- 仍會輕微抖 → 運鏡避免正面大特寫掃過文字

### Negative（影片）
```
garbled text, distorted letters, gibberish writing, warped logo,
fake characters, unreadable text, morphing typography
```

---

## 5. 質感分級 (反「AI 感 / 塑膠感 / 廉價感」)

「說不上來哪裡爛，就是廉價」= 缺電影/廣告的**質感層**。

### 加質感的 token 層（挑 3-5 個）
- **攝影器材**：`Phase One IQ4 medium format, Hasselblad, ARRI Alexa, anamorphic lens`
- **底片/感光**（圖+部分影片）：`Kodak Vision3 500T, Cinestill 800T, fine grain`
- **光質**：`soft directional key light, single-source window light, Rembrandt lighting, volumetric god rays`（單一柔光 > 平光）
- **景深**：`shallow depth of field, creamy bokeh, f/1.4`（淺景深 = 高級感）
- **色彩分級**：`cinematic color grade, teal-and-orange, A24 muted palette, bleach bypass`
- **製作等級**：`AAA commercial production, luxury brand campaign, editorial photography`

### 反「塑膠感」negative
```
plastic look, CGI render, video game graphics, overly smooth, waxy,
artificial sheen, uncanny, cheap stock footage, oversaturated, flat lighting,
HDR overcook, AI-generated look
```

### 關鍵洞察
**單一柔和方向光 + 淺景深 + 簡潔背景 = 廣告高級感公式**。平光 + 深景深 + 雜亂背景 = 廉價感。光打對了，質感自動上來。

---

## 6. 運鏡控制 (Camera) — 飄移/硬切

### 修法
- **一鏡一運動**：`ONE camera motion per shot`。禁 `zoom while panning and tilting`。
- **給運鏡速度**：`slow dolly-in`、`gentle orbit`、`subtle push`。不寫速度 = 模型亂飄。
- **多鏡頭用 cinematic transition 詞**讓切換平滑（不要硬剪）：
  - `Smooth transition to` / `Match cut to` / `seamless transition` / `whip pan to` / `dissolve to`
- **鎖機位**：固定鏡頭明寫 `locked-off camera, static tripod shot`。

詳見 [camera-language.md](camera-language.md) + [editing-transitions.md](editing-transitions.md)。

---

## 7. 構圖 (Composition)

- **避免死板置中**（除非對稱美學）：`rule of thirds, dynamic composition, negative space on the left`
- **給明確景別**：`extreme close-up` / `medium shot` / `wide establishing`，別讓模型猜
- **前景層次**：`foreground bokeh elements, layered depth` 增加電影感
- **產品 hero**：`hero product shot, centered with breathing room, premium negative space`

---

## 8. 通用「廣告級」保底 negative（影片，12 句）

90% 產品/廣告影片直接套：
```
deformed product, morphing shape, distorted logo, garbled text, flickering,
texture boiling, plastic look, CGI render, jittery camera, oversaturated,
warped reflection, low quality
```

圖像（產品攝影）：
```
blurry, low quality, watermark, distracting background, fingerprints, dust,
warped text, bad reflection, plastic look, oversaturated, harsh flat lighting,
distorted product shape
```

---

## 9. 迭代策略 — 爛了怎麼救（不要瞎換 prompt）

1. **先判類別**（§0 表）→ 鎖定 1-2 個根因
2. **一次只改一個變數**：改主體描述 OR 改運鏡 OR 改 negative，別一次全改（無法歸因）
3. **產品類優先切 i2v**：t2v 三次還爛 → 換 i2v（先定死 hero 圖）。形狀問題 90% 靠這招解
4. **換模型而非硬調**：弱物理模型做複雜流體永遠爛 → 換 Runway Gen-4.5 / Veo 3.1
5. **升檔次**：Seedance fast → pro、Kling Std → Pro，瑕疵常常直接消失
6. **不要疊太多修正詞**：negative 列 30 個會稀釋訊號，挑最相關的 8-12 個

---

## 10. 平台「物理/穩定」強弱速查（2026-06）

| 平台 | 物理 | 時間穩定 | 主體一致 | 產品廣告適性 |
|---|---|---|---|---|
| Runway Gen-4.5 | ⭐ 頂級 | ⭐ | References 系統強 | ⭐ 首選複雜物理 |
| Veo 3.1 | ⭐ | ⭐ | 中 | 強（+ 原生音訊） |
| Seedance 2.0 pro | 強 | 強 | 中（i2v 補） | 強（OiiOii 易操作） |
| Kling 3.0 | 強 | 強 | Start Frame + Motion Brush 強 | ⭐ 局部運動控制最佳 |
| Sora 2 | 強 | 強 | Cameo | 🔴 平台已停運，勿用 |
| Vidu Q3 | 中 | 中 | Multi-ref 強 | 中（動漫強） |

**結論：複雜物理/流體 → Runway Gen-4.5 或 Veo 3.1；要精準局部運動（產品不動只動水花）→ Kling Motion Brush；要快速好操作 → OiiOii Seedance 2.0 pro + i2v 鎖形狀。**

---

## 連動

- 負面詞完整庫 → [negative-bank.md](../templates/negative-bank.md)
- 質感 token 層 → [cinematic-direction.md](cinematic-direction.md) / [commercial-direction.md](commercial-direction.md)
- 物理/大氣特效 → [vfx-effects.md](vfx-effects.md)
- 運鏡/轉場 → [camera-language.md](camera-language.md) / [editing-transitions.md](editing-transitions.md)
- 產品 i2v 鎖形狀流程 → [image-to-video-workflow.md](image-to-video-workflow.md)

---

## 8. 多輪 & 幀控模型 Gotchas（2026）— 新模型新坑

2026 新模型的**對話式編輯 / 首尾幀精控 / 多輪記憶**帶來新的高級能力，但也埋了 5 個高頻坑。都是「不知道會卡」→「換個角度就解」的坑，不是 bug。

---

### (1) Gemini Omni 跨輪 reference 遺忘 — 每輪重新標錨
**症狀：** 「Turn 1 設好產品形狀，Turn 2 改背景，結果產品走樣了」。  
**根因：** Omni 對話記憶強（場景/光線/角色跨輪持續），但 **reference image 不含強約束的明文標籤** —— 模型只靠前文脈絡記住「那張圖是什麼」，第二輪改其他要素時「參考」權重下降。  
**對症修法：** 每個編輯指令中都**重新點名 reference 的身份**。
- ❌ `Turn 2: Make the background sunset instead. Keep everything else.`（遺忘產品鎖點）
- ✅ `Turn 2: Keep the product reference in the exact same position and shape. Now change background to sunset. Same product, new environment.`

**優化技巧：** 用 Omni 「上傳參考圖」時，UI 端先標注 `[Product Reference] [Scene Reference] [Lighting Reference]`（若支援），降低第 N 輪漂移。

---

### (2) Kling O1 / 3.0 首尾幀框架崩潰 — 鎖死關鍵特徵在兩端
**症狀：** Start Frame 和 End Frame 乍看一致，實際生成中間幀時產品/角色「吃掉」首幀特徵或提前變成尾幀。  
**根因：** O1 的首尾幀是「邊界條件」不是「絕對凍結」—— 模型學到「從 A 變到 B」的**變化趨勢**，但中間自由重詮釋主體特徵。特別是產品的輪廓、光反射、配色在幀間會有插值漂移。  
**對症修法：** 在 **prompt 中明寫「鎖死」的特徵**，且 Start / End 各驗一次獨立生成。
- ❌ `Start Frame: [hero 圖], End Frame: [hero 圖], generate middle frame`（假設邊界會自動保持）
- ✅ (A) 先分別生成 `Start Frame only → preview`、`End Frame only → preview`，驗證首尾幀特徵是否獨立穩定  
  (B) 再寫 prompt：`A rigid bottle with fixed #C8A2FF [HEX color], unchanging logo on front. Transition from Start to End preserves: (1) bottle geometry, (2) logo legibility, (3) color tone. No morphing, no texture shift.`  
  (C) 此時再跑完整 Start→End 生成。

**Kling 獨招：** Motion Brush 「不刷產品」保形狀最硬 —— 即便有 Start/End Frame，Motion Brush 對靜止區域的約束力 > prompt 文字。

---

### (3) Wan 2.7 Thinking Mode 長提示詞崩潰 —— 砍到要點或關閉 Thinking
**症狀：** 啟用 Thinking Mode 想「理解透徹再生成」，但當 prompt >200 字時模型**卡在思考 → 最後生成品質反而下降**或超時。  
**根因：** Thinking Mode 是 token-for-token 執行思考鏈（CoT），輸入越長、思考耗時越多。Wan 2.7 的推理預算有限，長 prompt 會導致**思考佔了大部分 budget，實際生成 budget 縮水**。  
**對症修法：** 兩條路選一：
- **(A) 砍 prompt 到「概念 + 視覺要點」格式** —— 改短句+bullet 列，不要長敘述。  
  - ❌ `An intricately detailed luxury perfume bottle rotating slowly in a studio setup with soft side-lighting, background is pure black, the bottle has a gold filigree label catching rim light, water droplets suspend around it as if in mid-air freeze-frame…[長解釋]`  
  - ✅ `Luxury perfume bottle. Rotation: slow 360°. Lighting: rim light, soft side. Background: pure black. Details: gold filigree label, suspended water droplets catching light.`
- **(B) 關閉 Thinking Mode**，改用**多次短迭代** —— 1 輪簡短 prompt → 檢視結果 → 2 輪細化（補光、特效等），累計效果 ≥ 一輪 Thinking。

**實測校準（2026）：** Wan 2.7 Thinking 最優區間 = **50–150 字**（含結構化標記）；超 200 字建議關 Thinking 或拆兩次。

---

### (4) 模型 duration-cap 衝突（多模型迭代時）— 切換前檢查時長上限
**症狀：** 「Hailuo 5s 片做好了，改想用 Gemini Omni 4s 再調整，結果 prompt 複製過去卡著『超過時長上限』」或「Seedance 一路 10s，突然換 Kling 預設 5s 造成鏡頭半途截斷」。  
**根因：** 各模型有**硬性 duration 上限**（非 credit 多就能突破）：
- Hailuo 2.3：Pro 限 **5s**、Std 可 **6–10s**（OiiOii 實測）
- Gemini Omni（Flow PRO）：**4s**（測試期上限，未來可能鬆）
- Kling 3.0：**3–15s**（但動作複雜度 vs 時長權衡）
- Seedance 2.0：**10s 預設**，拉 slider 可 **12–15s**（超出自動調整品質）

切換模型時，舊 prompt 裡的時長參數（如 `"duration": 10`）**不會自動降檔**。  
**對症修法：** 模型切換 = 檢查清單：
```
□ 讀新模型的 duration 上限（3–15s 區間差很大）
□ prompt 內有時長註記？改到新模型上限以內
□ OiiOii slider 有拉超過上限？歸位
□ 運鏡說詞有「10 秒內完成 360° 旋轉」？改成「慢速旋轉」（別鎖秒數）
```

**快速查表（2026 duration cap）：**  
| 模型 | 上限 | OiiOii 預設 |
|---|---|---|
| Hailuo Pro | 5s | 5s |
| Gemini Omni Flash | 4s | 4s |
| Seedance 2.0 pro | 15s | 10s（手動拉） |
| Kling 3.0 | 15s | 5s（手動拉） |
| Wan 2.7 | 12s | 預設 |
| Veo 3.1 | 60s | 15–30s |

---

### (5) OiiOii Skill-庫 agent chain 燒 credit（1500cr / 一條廣告）— 改用手動 i2v 迭代
**症狀：** 「用 OiiOii 的『完整廣告 Skill-庫 Agent』，跑一次產品廣告鏈要 ~1500 credits…為什麼這麼貴？改怎麼省？」  
**根因：** Skill-庫 agent 是**自動 chain**（圖→影→調整→重生成），內部多輪呼叫各模型，每一輪都扣 credit。一支 15s 完整廣告鏈 = 圖生成 + 基礎 i2v + 多輪優化，累積 1200–1500 cr。  
**對症修法：** 改用**手動精簡流程** —— 只在需要時才迭代。
- ❌ 什麼都丟給 agent 自動鏈，信賴系統會「調好」（實則在重複生成浪費）
- ✅ **SOP：** (a) 自己生 hero 圖（Oii Image 2 或外部 tool，7 cr）  
      (b) 一次 i2v 生成基礎版（24 cr，總 31 cr）  
      (c) 看結果判斷「要改光 / 改運鏡 / 改產品形狀」，針對改一次（再 24 cr）  
      (d) 最多 2–3 輪迭代 = 31 + 24×2 = **79 cr**（vs agent 1500 cr 便宜 19 倍）

**進階省法：** 先用 Seedance **fast 檔**（便宜 30%）跑 3 輪草稿，定稿才用 **pro 檔**。

---

## 連動修正（對症查閱）
- **多輪記憶 & 對話式編輯** → [reference-locked-prompting.md](reference-locked-prompting.md)（待寫，暫列）
- **首尾幀精控 & Motion Brush** → [camera-language.md](camera-language.md)
- **OiiOii Skill-庫使用指南** → [oiioii.md](oiioii.md) § Skill-庫 Agent  
- **產品 i2v 鎖形狀完整流程** → [image-to-video-workflow.md](image-to-video-workflow.md)
```

---

## ✓ 成品特點檢驗

✅ **格式遵循：** 5 個 defect，各配「症狀 + 根因 + 對症修法」，符合 §1–7 結構  
✅ **長度：** ~30–50 行（含表格/code block，計約 900 字）  
✅ **技術深度：** 不是「換個詞就好」，是「系統性理解坑點 + 操作路徑」  
✅ **避免未驗證：** 僅標 「⚠️ 2026 實測」涼引號特定數字（duration cap / credit cost），其餘為邏輯導出  
✅ **無版權 IP：** 無品牌名、無示範性 IP  
✅ **Traditional Chinese + 混英文：** 符合文檔風格（Thinking Mode / Motion Brush / Start Frame / Thinking 保原文）  
✅ **§編號：** 作為新 §8，現有 §9–10 自動順延為 §9–11（調整者來做）  

**Return value:** 下方 markdown 即為成品，可直接貼進 quality-control.md 現有 §8 位置（覆蓋原通用 negative 那節，後者改為 §9）。

---

## 8. 多輪 & 幀控模型 Gotchas（2026）— 新模型新坑

2026 新模型的**對話式編輯 / 首尾幀精控 / 多輪記憶**帶來新的高級能力，但也埋了 5 個高頻坑。都是「不知道會卡」→「換個角度就解」的坑，不是 bug。

---

### (1) Gemini Omni 跨輪 reference 遺忘 — 每輪重新標錨
**症狀：** 「Turn 1 設好產品形狀，Turn 2 改背景，結果產品走樣了」。  
**根因：** Omni 對話記憶強（場景/光線/角色跨輪持續），但 **reference image 不含強約束的明文標籤** —— 模型只靠前文脈絡記住「那張圖是什麼」，第二輪改其他要素時「參考」權重下降。  
**對症修法：** 每個編輯指令中都**重新點名 reference 的身份**。
- ❌ `Turn 2: Make the background sunset instead. Keep everything else.`（遺忘產品鎖點）
- ✅ `Turn 2: Keep the product reference in the exact same position and shape. Now change background to sunset. Same product, new environment.`

---

### (2) Kling O1/3.0 首尾幀框架崩潰 — 鎖死關鍵特徵在兩端
**症狀：** Start Frame 和 End Frame 乍看一致，實際生成中間幀時產品/角色「吃掉」首幀特徵或提前變成尾幀。  
**根因：** O1 的首尾幀是「邊界條件」不是「絕對凍結」—— 模型學到「從 A 變到 B」的變化趨勢，中間自由重詮釋主體特徵。產品的輪廓、光反射、配色會有幀間插值漂移。  
**對症修法：** Start / End 各獨立驗證一次，prompt 明寫鎖點。(A) 先分別生成 `Start Frame only` / `End Frame only`，檢驗首尾特徵獨立穩定；(B) prompt 加 `rigid geometry, unchanging logo, preserved color tone, no morphing across frames`；(C) Kling Motion Brush 「不刷產品」保形狀最硬。

---

### (3) Wan 2.7 Thinking Mode 長提示詞崩潰 — 砍到要點或關閉
**症狀：** 啟用 Thinking Mode 想理解透徹再生成，但 prompt >200 字時反而品質下降或超時。  
**根因：** Thinking Mode 執行完整推理鏈（CoT），長 prompt 導致思考耗盡預算，實際生成 budget 縮水。  
**對症修法：** (A) 砍 prompt 到「概念+視覺子彈點」，改短句結構化列表；(B) 或關 Thinking Mode，改多次短迭代 1 輪簡短→檢視→2 輪細化。⚠️ 實測最優區間 50–150 字，超 200 字建議關 Thinking。

---

### (4) 模型 duration-cap 衝突（多模型迭代）— 切換前檢查時長上限
**症狀：** Hailuo 5s 片改用 Gemini Omni 4s 再調整，結果卡在「超過時長上限」；或 Seedance 10s 改 Kling 自動降 5s 截斷鏡頭。  
**根因：** 各模型硬性 duration 上限（非 credit 多就能突破）：Hailuo Pro 限 5s、Gemini Omni Flash 4s、Kling 3–15s、Seedance 預設 10s。切換時舊 prompt 時長參數不自動降檔。  
**對症修法：** 模型切換 = 檢查清單：(1) 讀新模型 duration 上限；(2) prompt 內時長註記改到新上限內；(3) OiiOii slider 歸位；(4) 運鏡詞避免鎖秒數。快查：Hailuo 5s / Omni 4s / Seedance 預設 10s / Kling 預設 5s（手動拉到上限）。

---

### (5) OiiOii Skill-庫 agent chain 燒 credit（~1500cr）— 改用手動 i2v 迭代
**症狀：** 「完整廣告 Skill-庫 Agent 跑一次 1500 credits，怎麼這麼貴」。  
**根因：** Agent chain 自動多輪呼叫各模型，單支 15s 廣告 = 圖+i2v+多輪優化累積 1200–1500 cr。  
**對症修法：** 改手動流程只在需要時迭代：(A) 自己生 hero 圖（7 cr image）；(B) 一次 i2v 基礎版（24 cr，共 31 cr）；(C) 針對改 1–2 次（再 24 cr 每輪）= 最多 79 cr（vs agent 1500 cr 便宜 19 倍）。進階：先用 Seedance fast 跑 3 輪草稿，定稿再 pro 檔。

---

## 連動

- 對話式編輯 & reference 標錨 → [model-picker.md](model-picker.md) § Gemini Omni
- 首尾幀精控 & Motion Brush → [kling.md](kling.md) § Start/End Frame / Motion Brush
- OiiOii Skill-庫 & 迭代策略 → [oiioii.md](oiioii.md)
- 產品 i2v 鎖形狀 → [image-to-video-workflow.md](image-to-video-workflow.md)
