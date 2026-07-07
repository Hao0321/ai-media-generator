# 精緻度分級 — 從 AI slop 到 editorial / commercial-grade 的配方層

**定位：** 其他檔給的是「語彙選單」（單顆 token 的菜單）；這份檔給的是**組裝好、可整段貼入的完整配方**，帶真實 DP 級數值（角度 / 光比 / Kelvin / f-stop / 表面物理）。差別在：`Rembrandt lighting` 是選單，`key 45° 側 + 45° 高、4:1 光比、陰影側負補光` 是配方。要「更精美」靠的不是加更多形容詞，是把**方向 + 質地 + 比例 + 材質互動**寫成一句精確指令。

見 [cinematic-direction.md](cinematic-direction.md)（導演/DP/底片/構圖選單）、[commercial-direction.md](commercial-direction.md)（商業範式）、[vfx-effects.md](vfx-effects.md)（大氣/物理全庫）、[quality-control.md](quality-control.md)（反瑕疵排查）—— 本檔**不重覆選單，只給更深的配方與數值**。

> **⚠️ 數值校準（必讀）：** 下列 f-stop / Kelvin / 光比 / 角度是**真實攝影 ground truth**，非 AI 模型規格，不需 ⚠️ 標。但**模型是否精確解析「數字」= 逐平台不同、⚠️ 未實測** —— 把數值當「傾向錨」（bias 結果朝那個方向），真正 load-bearing 的是**具名 setup + 質地方向詞**（`hard side key / soft wraparound / crushed blacks`）。焦段詞、`teal-orange`、`shallow DoF` 跨模型都吃（見 community-prompt-patterns.md）；精確 `f/1.4`、`3200K`、`4:1` 則視模型而定，第一次跑先驗。

---

## 1. 打光配方 — 方向 + 質地 + 光比 三合一

格式：**具名 setup** — `可整段貼入的英文配方` — 用於什麼主體。

- **Rembrandt（戲劇肖像）** — `hard key 45° to camera side and 45° above eye level, small triangle of light on the shadow cheek, 4:1 key-to-fill, negative fill on shadow side` — 有輪廓的男性臉、工藝職人、烈酒瓶、editorial 人像。
- **Butterfly / glamour（美膚）** — `large soft frontal beauty light directly above the lens axis, symmetrical shadow under the nose, 2:1 low ratio, white bounce card under the chin` — 美妝、女性肖像、高端保養（凸顴骨、柔膚無瑕）。
- **Split（衝突/懸疑）** — `hard key at 90° to the side, exactly half the face lit half in shadow, 8:1 ratio, deep black shadow side` — 黑色電影、香水暗調、對立敘事、男性香氛。
- **Rim / Kicker（3D 分離）** — `thin backlight-kicker at 135° behind subject, one stop hotter than key, carves a bright edge separating subject from a dark background` — 飲料/香水/深色底產品拉成立體、逆光髮絲光。**深色底把主體拉成 3D 的關鍵配方**。
- **Chiaroscuro（奢侈 editorial）** — `single small hard source from upper side, no fill, 16:1 extreme ratio, luminous highlight falling into near-black, volumetric falloff` — 鐘錶機芯、珠寶、工藝、精品 TVC。
- **Softbox wraparound / Clamshell（乾淨棚拍）** — `two large soft sources, key above + fill below the lens, light wraps around the form, near-shadowless 1.5:1, feathered falloff` — 美妝、珠寶、食物、3C 乾淨產品。
- **Golden hour（生活感）** — `low 5° sun angle, warm ~3200K backlight rim plus cool ~7000K open-sky fill, long raking shadows, sun flaring through haze` — 戶外、汽車、寵物運動、旅遊、生活方式。
- **Practical-motivated（真實敘事）** — `lit only by in-frame practicals — neon, tungsten bulb, screen glow, candle — motivated pools of light with deep unlit falloff, mixed color temperature` — 夜景、酒吧、賽博、餐飲氛圍。

**光比對照**（key 比 fill 高幾檔）：`2:1`=1 檔=柔（美妝）｜`4:1`=2 檔=標準戲劇｜`8:1`=3 檔=noir｜`16:1`=4 檔=chiaroscuro。**塑膠感頭號元凶 = flat 平光（1:1）** → 一定給方向 + 光比。

---

## 2. 色彩科學 / 調色 — 精確 hue / 溫度寫法

**技巧：用 split-toning 語言（`shadows → X, highlights → Y`）比單寫色名精準**，並明令 `protect skin midtones` 避免整臉染色。

| 調色 | 精確貼入 phrase | 用於什麼主體 |
|---|---|---|
| **Teal & Orange** | `split-tone: shadows toward teal-cyan (~190° hue), highlights and skin toward warm amber (~30°), protect midtone skin, complementary separation` | 大片、汽車、動作、科技 |
| **Bleach bypass** | `silver-retention look, desaturated ~40%, crushed blacks, raised contrast, metallic hard highlights` | 戰爭、運動、工業、硬派 |
| **Warm amber editorial** | `warm 4800K bias, honey-amber highlights, gentle S-curve, rich-not-saturated, film-like density` | 時尚、精品、餐飲、生活感 |
| **Cool desaturated** | `cool 7500K blue bias, saturation −30%, green-grey shadows, low contrast, restrained` | 科技、北歐、懸疑、沉靜 |
| **High-key clean** | `lifted blacks, bright even exposure, minimal contrast, clean neutral whites` | 美妝、醫療、母嬰、電商白底 |
| **Low-key moody** | `dominant shadow, single accent, deep blacks held just above clipping (detail retained), high contrast` | 香水、烈酒、戲劇 |
| **Pastel** | `desaturated except gentle pinks and powder-blues, lifted blacks, soft matte low-contrast finish` | 甜點、少女、夢幻、生活風 |
| **Neon-noir** | `magenta-and-cyan neon dominance, crushed teal blacks, saturated practical sources, halation bloom around lights` | 夜景、賽博、MV、都會 |

---

## 3. 材質 / 表面渲染 — 反塑膠的關鍵

**核心心法：描述「光如何與表面互動」(specular / SSS / fresnel / refraction)，不是「材質很高級」。** 塑膠感 = 均勻死白反光 + 零紋理。

- **玻璃 / 水晶** — `sharp specular highlights, internal refraction and caustic light bending, fresnel edge brightening at grazing angles, faint green edge tint of real glass` — 香水瓶、酒瓶、杯器、錶鏡。
- **液體** — `surface tension meniscus, ray-traced refraction, crown splash with discrete droplets, condensation beading, light bending through the pour` — 飲料、酒、精華液、倒注鏡。
- **金屬** — `anisotropic brushed reflection along the grain, sharp specular streak, the real environment mirrored in the surface, faint micro-scratches` — 錶殼、珠寶、車漆、3C。**反光要「映出環境」不是白斑**。
- **皮膚** — `visible pores and fine vellus hair, subsurface scattering glow in thin areas (ears, nose bridge), micro-shadows in skin texture from side light, natural specular sheen on the T-zone, no airbrushing` — 肖像、美妝、時尚。**側光 / rim 光讓 SSS 現形**。
- **布料 / 絲** — `directional sheen following the weave, velvet light absorption, visible thread and fibre texture, natural drape and fold shadows` — 時尚、精品包裝、家居。絲=方向性反光、天鵝絨=吸光。
- **食物** — `glistening moist highlights, rising steam, fresh surface sheen, subsurface glow in translucent slices, crisp edge with soft interior, one natural imperfection` — 美食、飲品 ASMR。避免 over-glossy 假光澤。
- **木頭** — `visible grain and open pores, matte-to-satin sheen, warm subsurface glow in the grain, subtle wear and patina at edges` — 家具、工藝、餐桌、質樸品牌。

---

## 4. 鏡頭光學 — 焦段手感 + 光圈散景 + 克制的鏡頭性格

**光圈 → 景深 / 散景對照**（精緻感的隱形推手）：
- `f/1.4–f/2` = razor-thin focus plane, creamy circular bokeh, subject pops — 肖像 / 情緒 / 淺景高級感。
- `f/2.8–f/4` = subject sharp, background soft but still readable — 情境 + 產品並存。
- `f/8–f/11 focus-stacked` = full product tack-sharp front to back — **精品 / 珠寶 / 錶必要**（淺景會糊掉工藝）。

**焦段手感速記**（配光學質地寫）：`24mm environmental, keep subject off-edge to avoid distortion`｜`35mm documentary intimacy`｜`50mm natural eye-level neutrality`｜`85mm flattering compression, creamy bokeh`（肖像/美妝）｜`100mm macro 1:1, paper-thin DoF`（產品/珠寶/食物/水滴）。

**鏡頭性格（**克制是關鍵字**）：** `vintage uncoated lens, gentle low-contrast bloom`｜`anamorphic oval bokeh with a subtle horizontal flare`｜`slight chromatic aberration only at extreme edges`｜`soft halation bloom around highlights, controlled not blown out`。散景質地：`creamy circular / cat-eye at frame edge / soap-bubble / swirly vintage bokeh`。
> **反炫技鐵則：** 光學缺陷加 `subtle / controlled / gentle / restrained` 才高級；過曝滿屏光斑 = 廉價。

---

## 5. 氛圍 / 大氣 — 深度分離是「貴」的 tell

**高級畫面的隱形招：空氣透視（aerial perspective）製造前中後景分離。**

- **深度霾** — `light atmospheric haze separating depth planes: foreground crisp, background progressively hazed and desaturated` — **幾乎所有場景加它，前中後景立刻分層**。
- **體積光 / 耶穌光** — `backlit volumetric god rays through window / canopy / haze, visible light shafts, Tyndall effect` — 室內晨光、森林、教堂、揭示鏡。
- **背光微粒** — `floating dust motes / pollen / embers catching a single shaft of light, backlit particles` — 靜物、氛圍、魔幻。
- **地面霧** — `thin ground fog, knee-height mist for an atmospheric floor` — 戶外、恐怖、奇幻。
- **微氣候** — `condensation, steam, or breath vapor as micro-atmosphere` — 飲料、冷冽戶外、食物。

**密度控制：** `light haze`=優雅｜`thick fog`=壓迫。**鐵則：大氣必須配 backlight 才「發光」，否則只是灰霧。** 逆光 + 微粒 = 立體。

---

## 6. 反 AI 塑膠感速查 — 最可靠的 8 句

塞不下全部時，這 8 句最穩地把結果推向照片感（挑 3-5 句）：

1. `single-source directional light with visible falloff and soft shadow`（**打平光是塑膠感頭號元凶**）
2. `shallow depth of field with a clearly defined focus plane`
3. `visible surface micro-texture — pores / grain / weave / one natural imperfection`
4. `subsurface scattering and fresnel edge on translucent or edge-lit materials`
5. `subtle film grain and natural sensor noise`（**零噪點 = CGI 感**）
6. `light atmospheric haze for depth separation`
7. `specular highlights that reflect the actual environment, not white blobs`
8. `slightly imperfect, unposed, candid — natural asymmetry and wear`

**正向 > 負面：** 與其堆 negative，不如正面點名「光 / 材質 / 景深」三件事。需要 negative 時最精簡：`plastic, waxy, airbrushed, over-smooth, 3D render, flat uniform lighting, zero noise, oversaturated, uncanny`。

---

## 使用哲學

- **一個主體一套配方** — 別把美膚 clamshell 疊在戲劇 chiaroscuro 上。挑一個對位的打光 + 一個調色 + 對應材質，共 3-5 個 recipe 即可。
- **數值是傾向錨不是保證** — 精確 f-stop / Kelvin / 光比逐模型解析度不同（⚠️ 未實測），第一次跑先驗；具名 setup + 質地方向詞才是跨模型 load-bearing。
- **精緻 = 物理正確** — 光的方向、表面互動、深度分離對了，「高級感」自動出來；形容詞（漂亮/質感）永遠救不了平光 + 深景深 + 均勻反光。
- **配方服務主體、不美化到認不出** — 承接 [concept-first-prompting.md](concept-first-prompting.md)：概念是主詞、配方是形容詞。打光 / 材質是為了讓產品被**看清楚 + 被渴望**，不是把它抽象化成漂亮空殼。

## 連動 + 來源

- 導演 / DP / 底片 / 構圖選單 → [cinematic-direction.md](cinematic-direction.md)｜商業範式 → [commercial-direction.md](commercial-direction.md)
- 大氣 / 物理特效全庫 → [vfx-effects.md](vfx-effects.md)｜反瑕疵排查 → [quality-control.md](quality-control.md)
- 逐字可貼 prompt → [proven-prompts.md](proven-prompts.md)｜跨模型簽名 → [community-prompt-patterns.md](community-prompt-patterns.md)
- [Cinematography Lighting Ratios 101 (Wandering DP)](https://wanderingdp.com/cinematography/cinematography-school-lighting-ratios-101/) · [Rembrandt Lighting Guide (Honcho)](https://thehoncho.app/blog/rembrandt-lighting/) · [Contrast Ratios for Cinematography (Shutterstock)](https://www.shutterstock.com/blog/how-use-contrast-ratios-filmmaking)
- [Subsurface Scattering for Realistic Skin (Hailuo)](https://hailuoai.video/pages/knowledge/subsurface-scattering-realistic-skin-tones-guide) · [Why AI Portraits Look Plastic & How to Fix (Vicsee)](https://vicsee.com/blog/ai-portrait-plastic-fix) · [Why AI Images Look Fake (Vofy)](https://www.vofy.art/blog/why-ai-images-look-fake-photorealistic-solutions)
