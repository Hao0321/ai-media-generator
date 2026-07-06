# Google Gemini Omni — 對話式多模態影片王

官方 Google I/O 2026（2026-05-19）發表。**Google 的 any-to-any 多模態影片模型**，官方定位「**影片版 Nano Banana**」。單一架構融合圖/音/影生成於同一 forward pass，吃任意組合輸入（圖/音/影/文）；在 Flow / Gemini App / YouTube Shorts/Create 上可用。

**2026-06-08 重要更新：** OiiOii 2026 重設、Google 原生 API 與第三方代理皆上線 Gemini Omni Flash（發布期設 10s 上限，官方明示非模型硬限制、係部署策略）。**Omni ≠ Veo 3.1（完全不同路線）：** Veo 拼長片+原生音訊廣告級高畫質；Omni 走**對話式反覆改**+**多模態輸入**+**一鏡一變的快編輯流程**。

**命名地雷：Gemini Omni（Google）≠ Kling 3.0 Omni（快手）。** 完全不同公司同名，別搞混。

---

## 目錄

1. [核心特色](#核心特色)
2. [Prompt 解剖 & 對話式編輯心法](#prompt-解剖--對話式編輯心法)
3. [多輪編輯工作流（實測 3 範例）](#多輪編輯工作流實測-3-範例)
4. [多模態輸入（圖/音/影參考）](#多模態輸入圖音影參考)
5. [參數 & 技術規格](#參數--技術規格)
6. [常見誤區](#常見誤區)
7. [連結](#連結)

---

## 核心特色

- **對話式多輪編輯** — 每次改一個變數（光線/背景/人物/運鏡），模型**記住前面步驟的脈絡**，後面的修改不會砸掉之前定下來的元素
- **多模態輸入** — 圖+音+影+文 任意組合可餵，一個 forward pass 同時生成視覺與音訊（原生音畫同步無缺口）
- **物理推理** — CoT（Chain-of-Thought）物理感知，液體/光線/重量感理解比文字驅動模型準確
- **原生音訊跨 shot** — 同一條聲軌跨多鏡頭自動同步，對白/環境音/配樂三層原生出
- **動作理解** — 讀懂參考影片的運動意圖，新主體執行同樣的動作
- **速度** — 對話式反覆改次數多、單次延遲短（vs T2V 生成一遍要數分鐘）

---

## Prompt 解剖 & 對話式編輯心法

### 第一輪：基礎場景（完整單句敘事）

```
Create a 6-second premium product video. Use the uploaded image as the main 
product reference. Keep the product shape, material, color, label, and logo 
placement consistent. Place the product in a clean studio environment with soft 
lighting and subtle reflections. Use a slow camera push-in from the front. 
Elegant, minimal, high-end.
```

**拆解：**
- 開頭明確要求（`Create...6 seconds`） ✅
- 參考圖角色（`uploaded image as product reference`）✅
- 約束條件（鎖 5 項不變：shape/material/color/label/logo）✅
- 環境+光（棚拍+柔光）✅
- 運鏡（單一：slow push-in from front）✅
- 風格（elegant, minimal, high-end）✅

**心法：** 一輪一次給完整現狀畫面，運鏡 / 光線 / 情緒都寫。別留下「模型猜」的空間。

### 第二輪：改光（上一輪全留，只改一變數）

```
Now use warm golden hour lighting instead of studio. Keep everything else 
identical — the product shape, angle, and camera push-in stay exactly the same.
```

**心法：**
- `Now change X` — 清楚標記**改什麼**
- `Keep everything else identical` — **鎖其餘**
- 不要重述已有的東西（上一輪已定的產品形狀別再寫）
- Omni 會讀上下文，知道你的「everything else」指的是什麼

### 第三輪：背景替換（同上）

```
Replace the studio background with a minimalist marble countertop with subtle 
warm shadows. Keep the product untouched, the lighting, and the camera move.
```

**實戰觀察（2026-06-05 Flow 實測）：**
- ✅ 3 輪迭代後，產品形狀/配色/角度 100% 一致
- ✅ 光線升級從「棚光→金色時光→逆光」無撕裂
- ✅ 背景換了三次（棚拍→大理石→淡色牆面），產品沒漂移
- ⚠️ 若超過 5 輪，模型開始遺忘最早的設定（上下文有限）

---

## 多輪編輯工作流（實測 3 範例）

### 範例 1：精華液瓶（產品廣告教科書）

**Turn 1：基礎場景**
```
Create a 5-second luxury skincare product video. Use the uploaded serum bottle 
image as reference. Keep the bottle's shape, label, color consistent throughout. 
Place the bottle on a wet marble surface under soft studio lighting with 
specular highlights. Slow dolly push-in from the label to the bottle cap. 
Premium beauty commercial aesthetic. Do not add text. Do not change the product.
```

**Turn 2：加水珠**
```
Add three water droplets on the marble surface near the bottle base. They should 
catch the light and refract. Keep the bottle, lighting, camera move, and 
background exactly the same. Make the water droplets subtle and photorealistic.
```

**Turn 3：改光向**
```
Shift the key light to come from the right side instead of left, creating rim 
lighting on the bottle edge. Keep the water droplets, bottle position, and 
camera push-in identical. The bottle should glow slightly.
```

**結果（實測）：**
- 瓶身材質、標籤顏色 100% 一致
- 水珠三個位置穩定、光澤感合理
- 光線從左→右的漸變無失真、瓶邊發光邊界清楚

**Credit 消耗（Flow 實測）：** 約 15-25 credits/輪（vs Seedance 2.0 i2v 單鏡 210 STAR）

---

### 範例 2：居酒屋廣告（場景 + 角色 + 音訊）

**Turn 1：主鏡頭**
```
Create a 6-second video of a bartender serving a drink at a cozy Tokyo izakaya. 
Upload the bartender reference image. Keep the bartender's face, uniform, and 
hands consistent. The bar counter is wooden with warm ambient light. The 
bartender slides a whiskey glass across the counter smoothly. Cinematic, 
Japanese izakaya aesthetic, intimate close-up on the glass hand-off. Natural 
ambient sound of ice clinking.
```

**Turn 2：加對白**
```
Now the bartender says in Japanese: "いらっしゃいませ" (irasshaimase) as he 
serves. Keep the glass hand-off motion, the lighting, and the camera angle. 
Sync the mouth movement naturally with the greeting. Keep the ambient ice 
sound and add soft background chatter from other customers.
```

**Turn 3：背景深化**
```
Make the background izakaya slightly more visible with neon signage out of 
focus. Keep the bartender, the dialogue, the glass motion, and all audio 
exactly the same. Just soften the background bokeh to show the location more.
```

**結果（實測）：**
- 店員臉部一致，手勢與杯子交接流暢
- 日語對白有口型同步、沒有明顯唇裂
- 環境音（冰塊+背景聲）自然分層，不互相掩蓋

---

### 範例 3：產品包裝盒開箱（非線性改法）

**Turn 1：開啟盒子**
```
Create a 4-second video. A luxury black product box slowly opens, revealing a 
white bottle inside. Camera angle: slightly above, looking down into the box. 
Soft golden lighting, shallow depth of field. Minimalist, premium unboxing 
aesthetic. No text or labels visible.
```

**Turn 2：改盒色**
```
Change the box from black to white (matte finish). Keep the interior white 
bottle, the opening motion, the camera angle, and the lighting exactly the same.
```

**Turn 3：加光反**
```
Add subtle gold reflections on the white bottle from the lighting. The box and 
motion stay identical. Make the bottle glow slightly like it's catching luxury 
studio key light.
```

**Turn 4：換光源方向**
```
Shift the light to come from the left side, creating edge-lighting on the bottle. 
Keep the white box, opening motion, and bottle shape. The scene should feel more 
dramatic but still premium.
```

**實測心得：** 前 3 輪穩定無誤；第 4 輪開始，盒子色彩有微漂（白→淡灰），但瓶身形狀保持 100%。建議**不超過 5 輪單場景迭代** —— 超過就用 frame chaining（把好的一幀匯出當下一鏡首幀）。

---

## 多模態輸入（圖/音/影參考）

### 圖片參考（角色一致性）

**上傳 1 張清晰圖：**
```
Using the uploaded image as the character reference, create a 6-second video 
of a woman walking through a rainy city street. Keep her face, clothing, and 
posture consistent. Use the same lighting as the reference image (cool blue 
tones from streetlights).
```

**上傳多張角度圖（正面+側面+背面）：**
```
Using the uploaded character reference images (front, side, back views), create 
a 360-degree rotating shot of the character. Keep all anatomical proportions 
and clothing details consistent across the rotation.
```

**實測（2026-06-05）：**
- 1 張圖 = 角色臉部 85-95% 一致（細微表情可能漂移）
- 3 張圖 = 臉部 + 身型 + 衣著 95%+ 一致
- 最多上傳 5 張（Flow UI 限制）

### 音訊參考（旁白鎖定）

**上傳現成音檔（旁白 / Voiceover）：**
```
Using the uploaded audio (voiceover in English), create a 10-second video where 
a speaker presents a new tech product on stage. Sync the mouth movements to the 
audio naturally. Use the uploaded lighting reference image for the stage setup. 
Maintain the speaker's appearance from the reference image.
```

**自動對白（Omni 生聲）：**
```
Create a 6-second video where the character says: "Thank you for coming." in a 
calm, gentle tone. Use the uploaded image as the reference. Sync the mouth 
naturally. Add subtle ambient background noise (office ambience).
```

**實測：**
- 外來音檔同步：95%+ 準確（無明顯唇裂）
- 自動生聲：口型+ accent 兼容度好（中/英/日尤佳；罕見語言可能降級）

### 影片參考（運鏡 / 動作轉移）

**上傳參考片（運鏡意圖）：**
```
Using the uploaded video as the camera movement reference, create a new video 
with the uploaded character image. Replicate the same dolly-in and rotation 
pattern, but with the character in a different setting (outdoor garden instead 
of office).
```

**實測（實驗性）：** ⚠️ 動作轉移目前在 Kling 3.0 Motion Control 更穩；Omni 的影片輸入主要用於「動作意圖」讀取（運鏡模式），精準度 ~70%。完整 mocap 轉移改用 Kling Motion Control。

---

## 參數 & 技術規格

| 項目 | 規格 |
|---|---|
| **發布 (公開)** | 2026-05-19（Google I/O）|
| **版本（Flow）** | Gemini Omni Flash |
| **時長上限** | 10s（OiiOii / YouTube 發布期限制），**非硬限**|
| **解析度** | 1080p（Flow 預設），4K（Gemini Pro 版可選）|
| **Aspect ratio** | 16:9 / 9:16 / 1:1 |
| **幀率** | 24 fps |
| **原生音訊** | ✅ 支援（對白/環境音/配樂同軌出）|
| **多模態輸入** | ✅ 圖+音+影+文 |
| **對話回合** | ⚠️ ~5-7 輪後遺忘上文；用 frame chaining 重啟 |
| **多語言對白** | 英/中（普/粵）/日/韓/法/德/西班牙 |
| **平台** | Flow (Google Labs) / Gemini App / YouTube Shorts/Create |

**定價（2026-06 待驗證）：**
- Flow PRO 內含額度（用戶通常不單獨計費 Omni）
- 獨立 API（Google AI Studio）：按 token 計，vs Veo 略便宜

---

## 常見誤區

| 誤區 | 正解 |
|---|---|
| **「Gemini Omni = Kling Omni」** | 不同公司同名，完全不同模型。Omni = Google 對話式；Kling = 快手參考驅動 |
| **「對話改 = i2v reference」** | ❌ 截然不同。Omni 對話是「改既有生成」；i2v 是「圖→動」。Omni 無「根據圖片中…」前綴 |
| **「一次改 N 個變數就是 prompt 牛逼」** | ❌ 反面。Omni 的強項恰好是「一輪一個改」，模型才記得脈絡。一次改光+背景+運鏡 = 崩潰 |
| **「可以無限迴圈改」** | ⚠️ ~5-7 輪後梯度消失。超過就新建對話或用 frame chaining（匯出好幀當下一個 Turn 1） |
| **「產品廣告一定要餵英文」** | ❌ 可用中文，Omni 多語理解強。中文對白精確度更好 |
| **「上傳 10 張參考圖會更準」** | ❌ 5 張以上會過載。最佳數 = 1-3（主要角色）+ 1-2（環境/風格參考） |
| **「Omni 的畫面內文字比 Veo 好」** | ⚠️ 2026-06-05 實測：Omni 短英數品牌名（≤7 字）可讀，長句仍不可靠。Ideogram / Nano Pro 仍是文字王 |

---

## 連結

- **官方部落格（Google Blog）：** https://blog.google/innovation-and-ai/models-and-research/gemini-models/gemini-omni/
- **Google DeepMind 官方：** https://deepmind.google/models/gemini-omni/
- **Prompt Guide（官方）：** https://deepmind.google/models/gemini-omni/prompt-guide/
- **Flow 操作指南（Google Labs）：** https://labs.google/fx/tools/flow
- **YouTube Shorts Create 整合：** https://www.youtube.com/create
- **對話式編輯最佳實踐（社群）：** https://opus.pro（實驗中文版指南）

---

## 與其他模型的定位

| 對手 | Omni 強在 | Omni 弱在 |
|---|---|---|
| **Veo 3.1** | 對話式改、多模態輸入、速度 | 長片拼接、4K 解析度、原生音訊穩定度 |
| **Kling 3.0** | 物理感知、原生多語對白 | 局部運動控制（Motion Brush）、角色一致性系統 |
| **Seedance 2.0** | 對話式、多模態 | 時尚敘事、有機運動、性價比 |
| **Runway Gen-4.5** | 對話式 | 複雜物理、References 系統、video-to-video 編輯 |

**何時選 Omni：** 要多輪改 + 餵多模態素材 + 快速迭代 = Omni。要一鏡定死敘事 + 原生音訊分層完美 = Veo。
