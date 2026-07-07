# Prompt Craft Engine — 更強 + 更快的組裝引擎

**這份檔案治一個病：知道要寫什麼（concept 有了），但「不知道怎麼把它組成一句強 prompt」，或每次從零想很慢。**

> 這是 **META 引擎**，不是 token 清單。它給你**組裝的骨架 + 送出前的評分閘**，每一層要填的實際 token（導演/DP/底片/鏡頭/光/VFX 詞）去對應 vocab 檔抓。
> **在流程中的位置：** `concept-first`（有沒有 idea）→ **本檔（把 idea 組成強 prompt + 評分）** → vocab 檔（每層填什麼字）→ `quality-control`（生出來爛了怎麼修）。

## 目錄
1. [世界級 Prompt 解剖 — 通用 6 層結構](#1-世界級-prompt-解剖--通用-6-層結構)
2. [快速填空骨架 — 6 種媒材 <60 秒組好](#2-快速填空骨架--6-種媒材-60-秒組好)
3. [10 分 Prompt 品質評分表（送出前必過）](#3-10-分-prompt-品質評分表送出前必過)
4. [Before → After 實例（套引擎全流程）](#4-before--after-實例套引擎全流程)
5. [連動](#連動)

---

## 1. 世界級 Prompt 解剖 — 通用 6 層結構

任何 media、任何模型，一句強 prompt 都由這 6 層組成。**弱 prompt 的每一層都是形容詞；強 prompt 的每一層都是可驗證的具體事實。** Subject 儘量放最前 10-15 token——多數模型（尤其 CLIP/diffusion 系）早段權重高；純自然語言的影片模型（Veo/Omni/Sora）位置沒那麼硬，但「主體先講清楚」永遠不吃虧。

| 層 | ❌ 弱（形容詞/空話） | ✅ 強（具體事實） | 一句訣 |
|---|---|---|---|
| **① 主體 Subject** | `a product` / `a woman` | `霧面墨黑陶瓷精華液瓶，圓肩窄頸，霧金屬壓頭` | **材質 + 形狀 + 一個辨識細節**；路人 3 秒認得出 |
| **② 動作 Action** | `moving dynamically` / `fast` | `一滴水珠墜落擊中瓶肩 → 向外漣漪` | **ONE 可視動詞**；寫「發生什麼事件」不寫狀態/情緒 |
| **③ 場景 Scene** | `in a nice background` | `濕潤深色板岩檯面、後方薄霧、單側窗光` | **2-4 個具體元素**（非 0 空背景，非 10 元素爆炸） |
| **④ 光影 Lighting** | `good lighting` / `cinematic lighting` | `單硬 key light 左斜 45°、低調 4:1 對比、右側微 rim` | **方向 + 質感 + 光比**（3 樣缺一都會平） |
| **⑤ 鏡頭光學 Camera/Optics** | `close up` | `100mm macro at f/4、緩 dolly-in、rack focus 到標籤` | **焦段 + 光圈 + 一個具名運鏡**（靜圖用角度，別寫運鏡） |
| **⑥ 風格色調 Style/Grade** | `beautiful, masterpiece, high quality` | `anamorphic 橫向耀光 + 橢圓散景、teal 陰影 + amber 高光` | **ONE 風格錨 + 一個 grade**；不是技術詞湯 |

**填每層 token 去哪抓（本檔不重列）：**
- ①②③ 主體/動作/場景寫法 → 若「不知道在拍什麼」先回 [concept-first-prompting.md](concept-first-prompting.md)
- ④⑤ 光 + 鏡頭 → [cinematic-direction.md](cinematic-direction.md) Part 3-4 + [camera-language.md](camera-language.md)
- ⑥ 風格/grade/底片 → [cinematic-direction.md](cinematic-direction.md) Part 5 + [commercial-direction.md](commercial-direction.md)（品牌調性）+ [vfx-effects.md](vfx-effects.md)（特效層）
- **每層填多少 token / 用哪個模型的簽名結構** → [community-prompt-patterns.md](community-prompt-patterns.md)（單一 source of truth）

> **鐵律：一層只挑 5-8 個高質量 token，不是每層塞滿。** 過度堆疊稀釋權重（見 cinematic-direction「使用哲學」）。

---

## 2. 快速填空骨架 — 6 種媒材 <60 秒組好

把 `[方括號]` 換成你的內容即可。順序照 6 層。**先填 3-4 層跑第一版，再補光/風格細修**（2026 共識：iterative > 一次填滿）。

**A. 產品靜圖（image）**
```
[材質+形狀 產品名] on [場景檯面]，[光: 方向+質感+光比]，
[鏡頭: 焦段+光圈+角度]，[構圖: 負空間/主體佔比]，[風格錨 + grade]。--ar [比例]
```

**B. 產品影片（i2v 優先，最抗變形）**
```
根據圖片中的物體、畫面、風格來生成影片。
[ONE 運鏡: slow dolly-in / lateral orbit] + [次要運動: 水珠/蒸氣/光變]。
Negative: [對症「怕出現的瑕疵」: 瓶身變形、標籤扭曲/模糊、繞圈漂移、flicker]。
```
（i2v 鎖形狀 SOP → [image-to-video-workflow.md](image-to-video-workflow.md) + [multimodel-video-cheatsheet.md](multimodel-video-cheatsheet.md)）

**C. 人像 Portrait**
```
[年齡+特徵 人物]，[一個微表情/微動作]，[場景]，
[肖像光: Rembrandt / clamshell / split]，[85mm f/2 淺景深]，[底片模擬 + grade]。
```

**D. 食物 Food**
```
[食物 + 關鍵質感詞: 拉絲/油光/蒸氣]，[動作: 叉子提起 / 傾倒 / 蒸氣升起]，
[90° 俯拍 或 側逆光]，[100mm macro 淺景深]，[飽和但自然色]，[9:16 或 16:9]。
```

**E. 敘事場景 Scene**
```
[Concept 一句總綱]。[主體+動作]，[場景 2-4 元素]，[光+時段]，[焦段+運鏡]，[grade]。
[原生音訊模型才加，如 Veo 3.1 / Gemini Omni] SFX: [...]／Ambient: [...]／Soundtrack: [...]
```

**F. VFX / 特效**
```
[事件寫法 not 狀態: 玻璃向外炸裂、火焰捲升]，[物理: 動量/碎屑/餘燼]，
[體積光/大氣]，[運鏡配合]，[grade]。**禁用版權 IP 名**（觸發平台攔截）。
Negative: [flicker / warp / extra limbs]。
```

---

## 3. 10 分 Prompt 品質評分表（送出前必過）

**逐項給 1 分，滿分 10。≥8 才送出；<8 回去補對應的 layer/骨架欄。** 每出現一個「跨平台垃圾詞」（beautiful / masterpiece / detailed / high quality）額外 **-1**（它們觸發 generic beauty，稀釋你的真 token）。

> ⚠️ **例外不是鐵律：** 少數模型實測會吃某些「看似垃圾」的詞（如 **Seedance 2.0** 對 `cinematic` / `8K` 有反應）。任何模型特例一律以 [community-prompt-patterns.md](community-prompt-patterns.md) 為準，別把本表當跨模型硬規則。

| # | 檢查項 | 通過標準 | 對症失敗 |
|---|---|---|---|
| 1 | **具體主體** | ①有材質+形狀+辨識細節 | `a product` / `a woman` |
| 2 | **可視動詞** | 每鏡 ONE 畫得出的動詞 | `fast` / `dynamic` / 多動詞同句 |
| 3 | **2-4 場景元素** | 場景元素數落在 2-4 | 空背景 / 元素爆炸 |
| 4 | **鏡頭精度** | 靜圖有焦段+光圈+角度；影片有具名運鏡 | 只寫 `close up` |
| 5 | **單一風格錨** | 只 1 個風格錨 | 3 種風格混（打架） |
| 6 | **長度甜蜜點** | 字數落在目標模型甜蜜區間 | 太短 under-deliver / 太長漂移 |
| 7 | **無自相矛盾** | 沒有衝突指令 | `raw` + `dreamy`；`靜態` + `whip pan`；`2.39:1` + `--ar 16:9` |
| 8 | **平台簽名對** | 用對目標模型的簽名 token+結構 | Flux 塞導演名（BFL 已 scrub）、Seedance 塞 `fast` |
| 9 | **負面對症** | negative 列「怕出現的瑕疵本身」（非「不要變形」的雙重否定） | 泛用 negative 清單貼上了事 |
| 10 | **存在性測試通過** | 刪掉任一鏡故事仍成立才砍 | 只是「多一個漂亮角度」的鏡頭 |

**查表去哪：** #6 #8 → [community-prompt-patterns.md](community-prompt-patterns.md)（各模型字數甜蜜點 + 簽名 + 禁忌，本檔不重列）；#9 → [quality-control.md §8](quality-control.md) + [negative-bank.md](../templates/negative-bank.md)；#10 → [concept-first-prompting.md](concept-first-prompting.md) Step 3。

---

## 4. Before → After 實例（套引擎全流程）

**❌ Before（generic，評分約 1/10）**
```
a luxury perfume bottle, beautiful, cinematic, high quality, 8k, dynamic lighting
```
逐項掛：①`a bottle` 無材質形狀（0）②`dynamic` 非可視動詞（0）③無場景（0）④`dynamic lighting` 空話（0）⑤無焦段（0）⑥`beautiful/high quality` 垃圾詞（-2）… 幾乎全紅。

**✅ After — 產品靜圖 hero（骨架 A，6 層全實）**
```
霧面墨黑玻璃香水瓶，方肩細長頸，霧金屬噴頭，正面刻印虛構標籤「AVENOIR」——
立於濕潤深色板岩檯面，後方薄霧、單側窗光從左斜入；
單硬 key light 左 45°、低調 4:1 對比、右側微 rim light 分離瓶身；
100mm macro at f/4、微俯 15° 的 3/4 hero 角、瓶身佔畫面約 40%；
anamorphic 橫向耀光 + 橢圓散景、teal 陰影 + amber 高光、霧黑棚背景。--ar 16:9
```
> 標籤用**自創虛構名**示範文字渲染——**送出前務必確認它不是真實品牌**（連神似真品牌都會被平台版權攔截）；一律換描述詞或確認過的虛構名。

**✅ After — 接力做影片（骨架 B，i2v 鎖形狀）**
```
根據圖片中的物體、畫面、風格來生成影片。
一滴水珠自上墜落、擊中瓶肩後向外漣漪，微噴霧升起再靜止；
鏡頭單一緩 push-in 推進至標籤高度，焦點隨推進落定於標籤。
Negative: 瓶身變形、標籤扭曲或模糊、繞圈漂移、flicker、多餘倒影。
```
逐項對照 rubric：① 材質+形狀+細節（沿用圖片鎖形狀）✅ ② 單一可視事件「水珠墜落擊瓶肩」✅ ③ 沿用圖片場景（板岩+薄霧+窗光）✅ ④ 光延續 key light、不重述靜態視覺 ✅ ⑤ 單一 push-in、焦點落定標籤=ONE 運鏡 ✅ ⑥ 延續 anamorphic + teal-amber 單錨 ✅ ⑦ 無衝突指令 ✅ ⑧ i2v 前綴=平台對法（鎖形狀）✅ ⑨ negative 列瑕疵本身、對症瓶身/標籤/漂移 ✅ ⑩ 水珠事件是唯一敘事主軸，抽掉就沒戲 ✅ → **10 項全過（10/10），送出。**

> **心法：先 concept，再用本引擎把 concept 逐層具體化，最後過評分閘。慢在組裝時省，快在不用重生。**

## 連動
- 有沒有 idea（空洞/沒主題）→ [concept-first-prompting.md](concept-first-prompting.md)
- 每層 token 庫 → [cinematic-direction.md](cinematic-direction.md) / [commercial-direction.md](commercial-direction.md) / [vfx-effects.md](vfx-effects.md) / [camera-language.md](camera-language.md)
- 字數甜蜜點 + 平台簽名 + 禁忌 → [community-prompt-patterns.md](community-prompt-patterns.md)
- 生出來爛了怎麼修 → [quality-control.md](quality-control.md)
- 現成逐字 prompt → [proven-prompts.md](proven-prompts.md)｜成套範本 → [../templates/preset-packs.md](../templates/preset-packs.md)
