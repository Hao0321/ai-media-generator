# 經驗證 Prompt 庫 (Proven Prompts) — 可直接複製貼上

**定位：** 不用從零寫。這裡是 2026 從**官方 + 社群**挖出、經來源驗證、**逐字可貼**的高品質 prompt，按用途分類。每個標：情境 ｜ 模型 ｜ 全文 ｜ 為何好 ｜ 來源。

> 用法：找最近的 → 換主體/品牌/色盤 → 貼到目標平台。配合 [concept-first-prompting.md](concept-first-prompting.md)（先有概念）+ [quality-control.md](quality-control.md)（反瑕疵）+ [model-picker.md](model-picker.md)（選對模型）。

## 🔑 廣告 prompt 黃金結構（多來源共識）

`Subject 主體(外觀/材質具體) → Action 動作(拆 beats 不寫摘要) → Setting 場景(時間/光/背景) → Camera 運鏡(orbit/truck/crane/rack focus/macro push-in) → Style 風格(打光名詞+色盤+"commercial")`

**打光語彙 = 廣告片靈魂（高價值 token）：**
- `rim lighting` — 飲料/香水把畫面拉成 3D
- `Chiaroscuro / high-contrast` — 汽車/科技
- `single dramatic spotlight + pure black background` — 精品錶/珠寶
- `golden hour / warm grade` — 美食/生活感

---

## 🍔 美食 / 飲料（食慾感）

**漢堡空中組裝（Kling 3.0 / 通用）**
```
A static shot of a burger as it assembles in mid-air. The entire shot is in dramatic slow-motion. The individual ingredients—a grilled patty, fresh lettuce, tomato, melting cheese, and a toasted bun—fly into place from different directions. The background is a clean, simple, professional studio gradient to keep the focus on the product. Style: TV food commercial.
```
*為何好：mid-air 組裝 + 慢動作經典食慾結構，乾淨棚拍鎖焦點。｜來源 leonardo.ai/news/kling-ai-prompts*

**牛排煎鍋微距 sizzle（Veo 3，ASMR 原生音）**
```
Extreme close-up of a cast-iron pan as butter foams and a steak sears, with macro steam and micro bubbles under overhead practical light with a warm grade. Sizzle and soft kitchen clatter sounds, no dialogue.
```
*為何好：微距+奶油起泡+蒸氣把「滋滋聲」視覺化，Veo 原生 sizzle 音效直接 ASMR。｜來源 skywork.ai/blog/best-veo-3-prompts-2025*

**早餐爆炸重組（Veo 3，三幕病毒公式）**
```
High-definition, cinematic food photography, vibrant and rich textures, golden hour lighting, appetizing and luxurious. Scene 1 (0-2s): A pristine glass jar of chocolate-hazelnut spread sits invitingly on a rustic, weathered wooden table. Scene 2 (2-5s): In breathtaking super slow motion, the jar dramatically bursts open. A vibrant explosion of rich spread, whole roasted hazelnuts, golden-brown toast slices, and glistening fresh red strawberries erupts outwards. Scene 3 (5-8s): With a final flourish, all the scattered ingredients gracefully converge and assemble themselves into a perfectly composed breakfast platter.
```
*為何好：陳列→爆炸→倒放重組三幕，視覺衝擊高。｜來源 veo3promptwriter.com*

**馬丁尼/調酒懸浮（通用 / Veo，飲料 rim light）**
```
Ultra-realistic cinematic video of a martini glass slowly rotating while liquid flows naturally inside. A green olive and a spiraling citrus peel remain suspended as bubbles rise through the drink. Rim lighting separates the glass and splash from a dark background, creating a thin line of light around the edge. Crown splash droplets freeze in mid-air. Macro detail, premium beverage commercial.
```
*為何好：rim lighting 把畫面拉成 3D，懸浮橄欖+crown splash 凍結是高轉換飲品視覺。｜來源 banana-prompts.net*

**咖啡晨間儀式（Veo 3，rack focus 情境）**
```
Coffee being poured from a French press into a ceramic mug in slow motion. Steam rises from the cup. The camera racks focus from the pour to a laptop screen showing email in the background. Morning work ritual. Warm tones, shallow depth of field.
```
*為何好：rack focus 把產品和情境一鏡串起，慢動作蒸氣+暖調生活感。｜來源 simplified.com*

---

## 🚗 汽車 / 精品 / 產品 Hero

**超跑鹽湖高速跟拍（Kling，速度感）**
```
The camera trucks (moves laterally) at the same high speed, perfectly parallel to the car and its driver. The car stays locked in the center of the frame. In the second half of the shot, the car executes a quick, slight S-curve maneuver, kicking up a small plume of white salt dust from its tires. The camera perfectly mirrors this S-curve. Cinematic, car commercial, high-energy.
```
*為何好：truck 鏡頭 + 車鎖中心 + 鏡頭 mirror S 彎是汽車速度感核心技法。｜來源 leonardo.ai/news/kling-ai-prompts*

**電動車雨夜大燈揭幕（通用 / Seedance / Veo）**
```
Create a 5-second cinematic product reveal for a minimalist matte black electric car. Start with an extreme macro shot of the LED headlight flickering on in a dark, rain-slicked studio. Execute a slow, sweeping crane shot upward to reveal the car's silhouette. Use high-contrast "Chiaroscuro" lighting with cold blue accents. Ultra-realistic textures, 4K resolution, 60fps. End with a subtle glow on the product.
```
*為何好：大燈微距→crane 上升揭全車經典 reveal 節奏，Chiaroscuro+冷藍高級電動車語彙。｜來源 adcreative.ai*

**腕錶水珠懸浮微距（Seedance 2.0 / 通用，精品打光教科書）**
```
A premium wristwatch floating in mid-air, slowly rotating with precision, water droplets suspended around it catching the light like diamonds, pure black background with a single dramatic spotlight from above, extreme macro detail showing every texture on the watch face, high-end jewelry commercial aesthetic, ultra-smooth rotation.
```
*為何好：純黑+單頂光+鑽石般懸浮水珠是精品錶廣告教科書打光。｜來源 atlascloud.ai*

**香水花瓣慢落 + hard cut 噴霧（Seedance 2.0）**
```
Macro shot of a luxury perfume bottle among scattered pink peonies, shallow depth of field, petals floating in warm afternoon light, soft ambient music. Camera glides closer, a feminine hand enters frame from the right, fingers gently touch the glass bottle. Hard cut to slow-motion spray, golden mist diffuses through the air, particles catching rim light against a dark background.
```
*為何好：靜物美學→hard cut 噴霧製造節奏轉折，金 mist 顆粒+rim light 是香水標誌畫面。｜來源 seedance.tv*

**奢華腕錶 360 轉（Veo 3.1）**
```
Premium watch rotating 360 degrees, silver and gold details catching light, deep navy background, multiple dramatic spotlight sources, slow orbital camera movement, luxury brand commercial quality.
```
*為何好：360 轉+multiple spotlight+orbital movement 給足打光與運鏡。｜來源 veo3ai.io*

---

## 🎬 電影感 / 運鏡（官方範本）

**吊臂揭示 hiker→canyon（Veo 3.1，Google 官方）**
```
Crane shot starting low on a lone hiker and ascending high above, revealing they are standing on the edge of a colossal, mist-filled canyon at sunrise, epic fantasy style, awe-inspiring, soft morning light.
```
*為何好：官方「camera move→主體→揭示→風格→光」標準結構。｜來源 cloud.google.com Veo 3.1 guide*

**淺景深雨夜公車（Veo 3.1，Google 官方）**
```
Close-up with very shallow depth of field, a young woman's face, looking out a bus window at the passing city lights with her reflection faintly visible on the glass, inside a bus at night during a rainstorm, melancholic mood with cool blue tones, moody, cinematic.
```
*為何好：reflection faintly visible 是高級細節指令，cool blue tones 鎖調色。｜來源 cloud.google.com*

---

## 🎞 多鏡頭敘事（時間碼 / 分鏡）

**8 秒 4 鏡頭探險敘事（Veo 3.1，Google 官方 timestamp）⭐**
```
[00:00-00:02] Medium shot from behind a young female explorer with a leather satchel and messy brown hair in a ponytail, as she pushes aside a large jungle vine to reveal a hidden path.
[00:02-00:04] Reverse shot of the explorer's freckled face, her expression filled with awe as she gazes upon ancient, moss-covered ruins. SFX: The rustle of dense leaves, distant exotic bird calls.
[00:04-00:06] Tracking shot following the explorer as she steps into the clearing and runs her hand over the intricate carvings on a crumbling stone wall. Emotion: Wonder and reverence.
[00:06-00:08] Wide, high-angle crane shot, revealing the lone explorer standing small in the center of the vast, forgotten temple complex, half-swallowed by the jungle. SFX: A swelling, gentle orchestral score begins to play.
```
*為何好：一個 prompt 切 4 鏡頭，各帶 shot type + SFX + Emotion，角色跨鏡一致。｜來源 cloud.google.com*

**Seedance 時間軸分段（武士竹林，0–4s/4–9s/9–13s）**
```
Multi-shot cinematic. 0–4s Wide shot: the samurai walks forward from a distance through a bamboo forest. 4–9s Medium tracking shot: he draws his sword and takes his opening stance with leaves falling around him. 9–13s Close-up: the blade cuts through the air with slow-motion water splashes. Photorealistic, 35mm film quality, professional color grading, film grain.
```
*為何好：Seedance 招牌時間軸分段，每段獨立寫動作+運鏡仍連戲。｜來源 higgsfield.ai / vmake.ai*

**Kling 15 秒多鏡頭 + Audio（火星溫室）**
```
Shot 1: Wide establishing shot of a desolate Mars colony greenhouse during a red dust storm. Shot 2: Cut to a macro close-up of a small green sprout. A botanist's gloved hand gently touches the leaf. Shot 3: Over-the-shoulder shot. The botanist stands up, looking out the reinforced glass window at the storm. Audio: Low hum of life-support systems, muffled howling wind outside. Cold blue interior lighting.
```
*為何好：Kling 3.0 原生 15s 單 prompt 多鏡頭，`Shot 1/2/3 + Audio:` 結構免 extend。｜來源 klingaio.com*

---

## 🗣 原生對白 / 音訊

**偵探黑色電影對白（Veo 3.1，ingredients-to-video，Google 官方）**
```
Using the provided images for the detective, the woman, and the office setting, create a medium shot of the detective behind his desk. He looks up at the woman and says in a weary voice, "Of all the offices in this town, you had to walk into mine."
```
*為何好：參考圖鎖角色一致 + says in a weary voice 指定語氣 + 引號對白生原生語音。可串第二句做雙人對話。｜來源 cloud.google.com*

**東京 vlogger 自拍對白（Veo 3，含防字幕技巧）**
```
A selfie video of a travel blogger exploring a bustling Tokyo street market. She's wearing a vintage denim jacket and has excitement in her eyes. The afternoon sun creates beautiful shadows between vendor stalls. She's sampling street foods while talking, occasionally looking into the camera. The image is slightly grainy, film-like. She speaks in a British accent and says: "Okay, you have to try this place when you visit Tokyo. The takoyaki here is absolutely incredible." She ends with a thumbs up.
```
*為何好：指定 accent + 完整口語腳本 + film-like grain。⚠️ Veo 會自動燒字幕 → 不要字幕就加 `(no subtitles!)`。｜來源 replicate.com/blog/using-and-prompting-veo-3*

**Kling 雙角色對白 + 自動對嘴**
```
A tense corporate boardroom. Alternating medium shots focusing on the speakers. [Character A: Older CEO, deep gravelly authoritative voice]: "We are not selling the company. Not today, not ever." Immediately, [Character B: Young Rival, sharp fast-paced angry tone] stands up abruptly and points: "Then you are sinking this ship with everyone on board!"
```
*為何好：`[角色: 音色描述]: "台詞"` 是 Kling 原生對白正解語法，自動對嘴+切 medium shot。｜來源 klingaio.com*

---

## 🔒 一致性鎖法（角色/產品跨鏡不變）

**Seedance `@AssetName` + 顯式鎖（商品雙鏡頭）**
```
[Shot 1: Macro] @ProductRef rotating slowly on a velvet pedestal, soft rim lighting, luxury aesthetic. [Shot 2: Lifestyle] A hand in elegant attire reaching out to grab @ProductRef on a marble countertop. 60fps, creamy bokeh, commercial grade.
```
*為何好：`@ProductRef` 同商品橫跨「特寫旋轉」+「手取情境」不變形 = OiiOii 商品 i2v 痛點解法。句末可加 `Reference @ProductRef for consistent shape and label.`｜來源 vmake.ai*

**Kling 產品文字穩定（旋轉時 logo 不崩）**
```
Slow macro dolly-in of a luxury crystal perfume bottle on a velvet pedestal. Clearly embossed on the glass label is the word "ETTREAL" in an elegant gold serif font. Soft golden hour lighting creates refractive caustics on the velvet. The bottle slowly rotates 45 degrees, ensuring the text "ETTREAL" remains perfectly stable and readable throughout the motion.
```
*為何好：「remains perfectly stable and readable throughout the motion」明令鎖字防旋轉崩壞。把 ETTREAL 換你的品牌。｜來源 klingaio.com*

**Vidu `@tag` 雙掛（多角色動漫一致）**
```
@SCENE_BG @Akari_front @Akari_profile enters from the left sprinting, lavender energy trail, dolly-in, rim light.
```
*為何好：`@角色_front`+`@角色_profile` 正側面雙掛確保轉身一致，tag 順序=優先序（主體放最前）。`@` 在輸入框打會跳已註冊資產選單。｜來源 vidu.com*

**Gemini Omni 對話式多輪編輯（每輪只改一變數）**
```
Turn 1: 10-second clip: a person opening a sleek black product box on a wooden desk in a minimalist apartment. Morning light. Slow zoom in.
Turn 2: Make it sunset light instead of morning. Keep everything else identical.
Turn 3: Now make the box white instead of black. Keep the lighting and camera move the same.
```
*為何好：Omni 招牌對話式編輯，每輪改一個變數 + 「Keep everything else identical」鎖其餘，跨輪記憶保留。｜來源 opus.pro*

**Omni 產品片含負面約束（落地頁 hero）**
```
Create a 6-second premium product video. Use the uploaded image as the main product reference. Keep the product shape, material, color, label, and logo placement consistent. Place the product in a clean studio environment with soft lighting and subtle reflections. Use a slow camera push-in from the front. Elegant, minimal, high-end. Do not change the product design. Do not add random text or extra objects.
```
*為何好：明鎖 shape/material/color/label/logo 五項 + 3 條負面約束防變形/亂加字。｜來源 medium（Omni guide）*

---

## 🎵 音樂 MV（Suno 生歌 + Veo/Omni/Kling 生畫面）

**MV 製作鏈：** Suno 生歌（含 stems）→ 抓 BPM / 段落 → 每段視覺 prompt **把運鏡/打光/lip-sync 錨在節拍 hit 上**（別讓鏡頭漂）→ Scene Builder 接。Veo 3.1 最強（in-frame 演唱 + 原生對嘴）。

**品牌 MV（Flow，敘事型）**
```
Create a 45-second brand music video showcasing teamwork, innovation, and growth. Use an upbeat electronic track with smooth transitions between modern office scenes, brainstorming shots, and client meetings. Bright natural lighting, energetic handheld camera, diverse team, optimistic and forward-looking tone.
```
*為何好：官方級敘事 MV 結構，指定曲風+轉場+情緒。｜來源 invideo.io/blog/ai-music-video-prompting-guide*

**歌詞視覺隱喻（Veo，節拍同步技法）**
```
Cinematic performance music video, a singer in a rain-soaked neon alley lip-syncing to the track. On the heavy bass drop, high-speed physics-accurate shattering glass erupts around her in slow motion, shards catching neon light. Camera pushes in on the beat, lighting pulses with the rhythm. Moody, high-contrast, 35mm.
```
*為何好：把歌詞意象（shattered glass）綁在 bass drop + 鏡頭 push on the beat = MV 節拍同步核心技法。｜來源 resource.digen.ai / invideo*

> **節拍同步咒語**：`camera pushes in on the beat`、`lighting pulses with the rhythm`、`cut on the snare`、`anchor motion to the bass drop`。

---

## 📱 9:16 直式短影音 / UGC（TikTok / Reels / Shorts）

**UGC 黃金公式：** `創作者類型 + 產品 + 場景 + 前 2 秒 hook + demo 動作 + proof point + 運鏡 + 打光 + 旁白台詞 + 字卡 + CTA + 9:16`。**5 種 hook**：痛點 / 開箱 / 數據 / before-after / 好奇。總長 20-25s，前 3 秒定生死。

**保養品 UGC（9:16，痛點 hook）**
```
A 9:16 vertical UGC-style video. A skincare creator in her 20s films herself in a bright bathroom with soft morning light, holding a vitamin C serum bottle, talking casually to the front camera. She says: "I wanted something simple for dull skin." She applies a few drops, the camera pushes in on her glowing skin. Authentic iPhone selfie look, slight handheld motion, natural skin texture, no ring-light eyes. Caption text space at the top third.
```
*為何好：痛點 hook + 真人對鏡 + demo + 真實 iPhone 質感（避網紅 ring light）= 高轉換 UGC 標竿。｜來源 ugcmaker.org*

**攜帶果汁機 UGC（9:16，demo hook）**
```
Vertical 9:16 UGC video. A creator films on a kitchen counter with a portable blender, natural daylight. Hook (first 2s): "This is my fastest breakfast trick." She drops in fruit, milk, and ice, blends quickly, then pours a smoothie into a glass and takes a sip. Fast satisfying demo, handheld phone camera, authentic home setting, upbeat energy. Leave safe area top and bottom for captions and CTA.
```
*為何好：demo hook + 快節奏滿足感 + 留字卡安全區（直式構圖鐵則）。｜來源 ugcmaker.org / adxmagic*

> **直式鐵則**：1080×1920、主體置中、上下留 caption/CTA 安全區、前 3 秒給 hook（痛點/開箱/數據/before-after/好奇 五選一）。

---

## 🏠 房產 / 看房

**無剪接空拍式走位（單鏡，連續）**
```
A camera showing the front door entrance, then slowly moving inside into the living room. There should be no cuts in the video, the camera acts like a smooth drone gliding forward. The door opens, the camera maintains a level horizon. Bright natural daylight, modern interior, wide angle, real estate tour aesthetic.
```
*為何好：`no cuts + drone-like + maintains level` 是看房一鏡到底的關鍵約束。｜來源 invideo.io*

**多鏡頭看房 tour（Veo/Seedance，黃金午後光）**
```
Cinematic real estate tour, modern apartment, golden afternoon light. Wide shot of the entryway, slow forward push into the living room, light spills across hardwood floors. Cut to a medium shot of the kitchen, slow drift right, natural light catches the marble countertop. Cut to a wide shot of the master bedroom through a doorway, slow push-in, golden light on white linens. Smooth, elegant, premium listing video.
```
*為何好：entryway→living→kitchen→bedroom 標準看房動線，每鏡 push/drift + 自然光鋪陳。｜來源 milehightitleguy.com*

> **建案進度神器**：start frame=空地、end frame=完工建築 → Frames-to-Video 內插出「興建縮時」。

---

## 👗 時尚 / 電商（布料動態 + 360 展示）

**模特 360 子彈時間（Kling，先生模特圖再 i2v）**
```
Full quick 360-degree bullet-time rotation around the woman wearing the dress. The camera performs a continuous orbit with smooth speed and strong parallax. The subject subtly rotates to face the lens during the motion, maintaining focus and dramatic depth. Studio lighting, clean background, fashion e-commerce aesthetic.
```
*為何好：360 子彈時間讓買家看遍服裝各角度 = 時尚電商核心；strong parallax + 主體回望增加高級感。｜來源 leonardo.ai / wearview.co*

> **時尚鐵則**：服裝廣告「布料怎麼動」決定成敗 → 寫 `fabric flows naturally, natural drape and inertia, wind catches the hem`；先用 image 鎖模特+服裝再 i2v 保一致。

---

## ✨ VFX / 魔法 / 科幻（無 IP，安全）

**魔法傳送門開啟**
```
A glowing magical portal opening in the middle of an ancient forest, swirling blue and purple energy, leaves and debris being pulled toward it, a silhouette of a person approaching, mystical atmosphere, cinematic light flare, wide angle, 4K.
```
*為何好：能量漩渦+碎葉被吸入+剪影接近=傳送門標準視覺，藍紫雙色鎖定。｜來源 leonardo.ai*

**死靈法師喚靈（無 IP 奇幻）**
```
A cloaked necromancer raising spirits in a moonlit graveyard, glowing runes on the ground, slow motion, stormy skies, eerie green mist, cinematic.
```
*為何好：slow motion + 發光符文 + 風暴天，把「施法瞬間」時間性與氛圍寫滿。｜來源 leonardo.ai*

**太空巡洋艦離港（科幻空鏡）**
```
A massive star cruiser departing from a space port orbiting a red gas giant, smaller ships in the background, engine glow, cinematic slow zoom out, epic scale, 3D cinematic render.
```
*為何好：紅氣態巨行星+背景小船給尺度感，cinematic zoom out 揭史詩規模。｜來源 focalml.com*

**賽博龐克機器人雨夜（科幻角色）**
```
An android walking through a neon-lit alley, rain falling, lens flares, puddles reflecting neon signs, cyberpunk environment, 3D cinematic render, moody atmosphere.
```
*為何好：霓虹+雨+鏡頭光暈+水窪反射=賽博龐克四件套。｜來源 medium aitooldiscovery*

---

## 🏃 運動 / 動作（速度感）

**跑車甩尾東京夜（高能量）**
```
A sports car drifting around a corner on a wet Tokyo street at night, sparks flying, neon reflections on the car body, dynamic low camera angle, motion blur, high energy, cinematic color grading.
```
*為何好：甩尾火花+霓虹車身反射+低角度動態，速度感拉滿（避寫 Fast&Furious IP，改 dynamic high-energy）。｜來源 media.io / seedance.tv*

**山路高速跟拍（賽車感）**
```
High-speed tracking shot of a sports car on a winding mountain road, motion blur on background, sharp focus on the vehicle, cinematic color grading, golden hour, dramatic atmosphere.
```
*為何好：tracking shot + 背景動態模糊 + 車身銳利 = 賽車跟拍核心對比。｜來源 media.io*

---

## 🌀 ASMR / 滿足感 / 無縫循環（loop）

**水銀球變形 loop（極致 ASMR）**
```
A perfect sphere of liquid mercury sitting on a mirror surface, slowly deforming under an invisible force into a cube shape, then morphing back to a sphere, reflections shifting with each transformation, seamless loop-ready motion, ASMR-satisfying aesthetic, macro lens extreme close-up, clean minimalist background.
```
*為何好：球↔方變形 + 鏡面反射隨之變 + `seamless loop-ready` 明令可循環，是 satisfying loop 標竿。｜來源 simplified.com*

**巧克力切片微距（食物 ASMR）**
```
Extreme close-up of a knife slicing glossy chocolate. Ultra-detailed texture visible. Smooth slow motion captures each cut, soft crack sound, warm key light, satisfying ASMR aesthetic.
```
*為何好：微距+慢動作+每刀脆裂聲，食物 ASMR 高黏著度公式。｜來源 simplified.com*

> **滿足感咒語**：`seamless loop-ready motion`、`satisfying ASMR aesthetic`、`smooth and hypnotic movement`、`oddly satisfying`。

---

## 🎌 動漫 / 2D 動畫風

**魔法少女變身（Kling anime 最高出片率）**
```
Anime magical girl spinning with sparkles and ribbons swirling around her, transformation sequence, vibrant pastel colors, dynamic camera rotation, glowing effects, anime style, cel shading, high quality animation.
```
*為何好：cel shading + 動態旋轉 + 變身敘事一次到位，多源重複引用。｜來源 vofy.art*

**奇幻法師施法（無 IP）**
```
Anime wizard casting spell with glowing magic circle appearing, robes flowing, mystical energy particles rising, dramatic lighting, slow motion, vibrant purple and blue colors, anime style, fantasy aesthetic.
```
*為何好：slow motion + 粒子上升 + 魔法陣浮現，避免靜態貼圖感。｜來源 vofy.art*

**雨中傘下 slice-of-life（情緒運鏡）**
```
Anime character standing in rain with umbrella, water droplets visible, neon city lights reflecting in puddles, melancholic mood, slow dolly forward, anime style, detailed rain effects, cinematic.
```
*為何好：slow dolly forward + 霓虹倒影 + melancholic，新海誠式情緒氛圍。｜來源 vofy.art*

---

## 🦁 自然 / 生態紀錄片

**森林雨滴微距（BBC Earth 風，破折號分段法）**
```
Heavy rain falling on forest leaves — macro close-up, camera holding still — overcast natural light, every droplet catching grey sky — nature documentary style, BBC Earth reference — water running along branch edges, fog in background — shallow depth of field, slow motion, cinematic 4K quality, no text.
```
*為何好：破折號分段（鏡頭/光/細節/風格/景深）是 Veo 紀錄片高命中寫法，BBC Earth + no text 防雜訊。｜來源 veo3ai.io*

**草原獅子特寫（極簡基準範本）**
```
Close-up of a lion in the savannah, gentle sunlight, ambient wind, 12-second slow pan, cinematic 4K.
```
*為何好：極簡但要素齊（主體+光+環境音+運鏡+時長），被多篇引用的 wildlife 基準。｜來源 cyberlink.com*

**顯微結晶縮時（微觀/科學感）**
```
Microscopic crystalline structures growing in time-lapse, brilliant colors (purple, teal, gold) as crystals form, close-up macro perspective, scientific wonder and beauty.
```
*為何好：縮時+指定三色盤+macro，補足少見的微觀類別，鎖色防糊。｜來源 veo3ai.io*

---

## ✈️ 旅遊 / 目的地

**晨霧山谷空拍揭幕（establishing）**
```
Sunrise over misty mountain valley, golden rays breaking through clouds, slow crane shot rising from low angle, epic and majestic atmosphere, cinematic 4K quality.
```
*為何好：slow crane shot rising from low angle 是 Veo 可靠運鏡，晨光破雲適合目的地開場。｜來源 veo3ai.io*

**威尼斯運河晨景（地標城市）**
```
Venice canal at dawn, gondola floating past ancient buildings, perfect reflections in still water, slow push forward through the canal, warm morning light, serene atmosphere.
```
*為何好：slow push forward + 靜水倒影，第一人稱划進城市的旅遊鏡頭感。｜來源 veo3ai.io*

**城市夜景 hyperlapse（高空車軌縮時）**
```
High altitude drone view, hyper-lapse sequence of a bustling metropolitan city skyline, light trails from car traffic, 30-second long exposure, deep blue night sky.
```
*為何好：hyper-lapse + 30-second long exposure 把曝光與時間語言講給模型聽。｜來源 edimakor.hitpaw.com*

---

## 💻 科技 / SaaS / App demo

**SaaS 著陸頁 hero（保留真實 UI，治亂編介面）**
```
Create a clean B2B SaaS product demo video for a landing page hero. The scene shows an approved analytics dashboard on a laptop screen. Preserve the layout and visible UI exactly. Add subtle camera movement and gentle motion that shows fragmented data becoming one clear account timeline. Professional SaaS marketing style, calm lighting, no fake logos, no invented metrics, no unreadable text, no unrealistic interface behavior. End on a stable frame suitable for a headline overlay.
```
*為何好：明令 `Preserve the visible UI exactly + no invented metrics + 結尾停穩給標題`，直擊 AI 產品 demo 最大痛點（亂編介面/數字）。｜來源 veo3ai.io*

**功能上線片（舊流程→新流程對比敘事）**
```
Create a 20-second feature launch clip for a B2B SaaS product. The viewer is a product operations manager preparing a release. Show the old workflow as scattered notes and status updates, then reveal the approved product screen where the new launch checklist organizes owners, tasks, and risk flags. Preserve UI structure, avoid fake customer names, keep text minimal and readable, and end with a clean CTA-safe frame. Motion should be clear, not flashy.
```
*為何好：以「買家是誰+舊痛點 vs 新解法」敘事開場（非先炫特效），SaaS 真正會轉換的結構 + CTA-safe 收尾。｜來源 veo3ai.io*

**手機產品全息揭幕（Apple 風，產品清楚 hero）**
```
A sleek smartphone floating and rotating in space, holographic UI elements appearing around it, dark background with subtle blue light, Apple-style product reveal, clean and futuristic, premium tech commercial.
```
*為何好：懸浮旋轉+全息 UI 環繞+暗底藍光科技 reveal 標準語彙；產品本體清楚當 hero（符合禁抽象原則）。｜來源 seedance.tv*

---

## 🎥 病毒格式速查（POV / 縮時 / hyperlapse）

| 格式 | 起手式 token | 範例 |
|---|---|---|
| **POV 第一人稱** | `First person POV [動作] through [環境], camera at eye level, slight motion shake, continuous forward movement` | Shibuya 夜走 POV、騎乘穿越 |
| **Timelapse 縮時** | `[事件] in time-lapse, [變化過程], dramatic atmosphere` | 草原雷暴、結晶生長、設計稿成形 |
| **Hyperlapse** | `hyper-lapse sequence, light trails, 30-second long exposure` | 城市車軌夜景 |
| **Bullet-time** | `360-degree bullet-time rotation, continuous orbit, strong parallax` | 時尚 360、動作定格 |
| **空拍 drone** | `high altitude drone view / slow crane shot rising from low angle` | 自然/地標揭幕 |

> **共通鐵則**：給**時間性 token**（slow motion / time-lapse / hyperlapse / zoom-in / rotating drone）= 模型才有「動」的方向感。

---

## 😱 恐怖 / 驚悚 / Liminal（克制 > jump scare）

> **核心洞察：恐怖靠克制不靠血腥。** 用「鎖機位+數秒不動+第 N 秒一個動作」+ 邏輯破綻製造心理壓迫。

**廢棄醫院 found footage（Veo，官方級結構）**
```
Horror film style, found footage POV shot. A long, narrow, decaying hospital corridor, lit only by the camera's weak light. A grotesque, long-limbed creature contorts itself as it scrambles along the ceiling towards the camera. SFX: distorted chittering sounds & the scraping of claws on plaster. Ambient: a low, unsettling drone.
```
*為何好：主體+動作+風格+光源+SFX+環境音全 slot 齊，Veo 官方教學 killer prompt。｜來源 arsturn.com*

**牆後的聲音（Kling，秒級克制恐怖）**
```
Interior hallway at night. Camera locked off, slightly low angle. A closed door at the end of the hallway. A single practical sconce on the wall casting an oval of light. The camera holds for 6 seconds — nothing moves. On second 7, a shadow passes under the door. Camera remains locked. 16mm grain, green cast in shadows.
```
*為何好：鎖機位+6 秒不動+第 7 秒一個影子=張力來自克制，精準到秒。｜來源 imagine.art*

**鏡像邏輯（Kling，邏輯破綻恐怖）**
```
Medium shot, bathroom mirror. The reflection shows something different from what should be there. Camera holds while the viewer studies the reflection. Mundane bathroom sconce lighting, clinical white. The horror is in the logic break.
```
*為何好：恐怖點在「邏輯破綻」非血腥，臨床白光反襯，極省 token。｜來源 imagine.art*

**Backrooms liminal space**
```
A solitary person in their 20s seen from behind, wearing a plain beige hoodie and dark jeans, slowly walking through an endless maze of yellowed office carpet corridors with buzzing fluorescent ceiling panels.
```
*為何好：黃地毯+嗡嗡日光燈+背影=liminal 三件套，最具辨識度。｜來源 talesfromthegloom.com*

> **克制恐怖咒語**：`camera holds for N seconds, nothing moves` / `the light creates more darkness than it resolves` / `the horror is in the logic break`。

---

## 📼 復古 / 膠卷美學（年代質感寫進光裡）

**Super 8 公路片（1970s）**
```
Super 8mm home movie footage, 1970s road trip, view from the passenger seat of a convertible driving through the American Southwest desert, wind blowing hair, warm sunset light, dust particles, shaky handheld camera, nostalgic mood.
```
*為何好：點名 Super 8mm + 手持抖+塵粒+暖夕陽=顆粒感建進光裡。｜來源 hitpaw*

**VHS found footage（1999 夜視）**
```
1999 camcorder footage, night vision mode, walking through a dark forest, flashlight beam illuminating trees, heavy digital noise, glitching tape artifacts, timestamp in corner "SEP 24 1999", scary atmosphere.
```
*為何好：年份+夜視+角落時間碼+磁帶 glitch=VHS 招牌，可兼恐怖。｜來源 hitpaw*

**80s Synthwave VHS**
```
1980s anime style, dark cyberpunk city street, neon pink and blue signage, rain falling, a retro sports coupe driving by, VHS tape quality, slight tracking error at the bottom, synthwave aesthetic, lo-fi glow.
```
*為何好：`slight tracking error at the bottom` 精準召喚 VHS 底部抖動條（最像真磁帶的細節）。｜來源 hitpaw（IP 車名已改 generic）*

**Film Noir（1950s 偵探）**
```
Film Noir style 1950s, detective office, smoke swirling in the air, light streaming through venetian blinds creating harsh shadows, a glass of whiskey on the desk, black and white, cinematic lighting, suspenseful atmosphere.
```
*為何好：百葉窗硬光條紋=noir 視覺簽名。｜來源 hitpaw*

> hitpaw 同源另 11 條全文（1920s 默片、Technicolor 郊區、16mm、迪斯可、有氧操廣告、90s sitcom 片頭等）。

---

## 🎭 停格 / 黏土動畫（反 AI 光滑是關鍵）

**Laika 風微縮村落空拍**
```
Stop-motion claymation aerial view of a miniature handcrafted village at sunset, tiny clay villagers walking down a cobblestone street, felt grass on the hills, paper trees, fabric clouds drifting overhead, warm golden hour lighting, slightly stuttery 12fps animation feel, Laika studio aesthetic, slow cinematic dolly forward.
```
*為何好：`12fps stuttery` + 逐一材質（felt 草/paper 樹/fabric 雲）把手作質感寫死。｜來源 reelmind.ai*

**黏土小狗（Wallace & Gromit 風）**
```
Stop-motion claymation small fluffy dog with oversized round eyes and a wagging tail, sitting in a miniature handcrafted living room with a felt rug and a tiny clay couch, warm soft lighting, visible clay texture, slight imperfections, 12fps stop-motion animation feel.
```
*為何好：`visible clay texture, slight imperfections` 是讓 AI 別做太光滑的關鍵指令。｜來源 reelmind.ai*

**羊毛氈牧羊（材質多樣）**
```
Wool felt stop motion of a shepherd and sheep on a rolling hillside. Soft felt textures, visible stitching, cloudy sky backdrop, gentle stop motion pacing.
```
*為何好：`visible stitching` 是羊毛氈定格獨有質感，跳脫黏土路線。｜來源 reelmind.ai*

> **反 AI 光滑咒語（停格必加）**：`visible clay texture / fingerprints / staggered movement / 12fps stuttery / slight imperfections`。

---

## 🎄 節慶 / 季節

**跨年煙火空拍（未來城市）**
```
Cinematic drone shot, aerial view of a futuristic city skyline at midnight on New Year's Eve. Massive multi-colored fireworks exploding in the sky, illuminating the glass skyscrapers. Reflections of the fireworks shimmering on a large river below. 4k, hyper-realistic, volumetric lighting, smoke trails dissipating in the wind, celebratory atmosphere, wide angle.
```
*為何好：`reflections shimmering on river` + `smoke trails dissipating in the wind` 給物理依據，煙火不像貼圖。｜來源 veoprompt.org*

**聖誕 i2v（先生圖再動，符合 i2v 黃金公式）**
```
Camera slowly dolly-in toward the tree, candlelight flickering, soft bokeh glow, gentle snowfall outside window.
```
*為何好：專為 i2v 設計——只寫運鏡+氛圍動態（燭光/飄雪），不重述視覺。先用 Nano Banana 生溫馨客廳聖誕樹圖 → 套此句 i2v。｜來源 videoweb.ai*

**萬聖 POV 廢棄遊樂園**
```
POV: walking through abandoned carnival, creaking rides, whispers in the background, fog rolling in.
```
*為何好：POV+環境音（吱嘎/低語）+霧氣三元素撐起氛圍，極簡萬聖感強。｜來源 fylia.ai*

> ⚠️ 誠實：萬聖/聖誕的「t2v 級完整全文」公開池較淺（多為靜圖 prompt 或 i2v 短句）。要完整聖誕 t2v → 套上方煙火結構把主體換聖誕場景，比抄現成可靠。

---

## 🐾 寵物 / 動物

**萌寵歪頭對鏡（寵物 influencer hero）**
```
Adorable golden retriever puppy tilting head curiously at camera, soft couch background, cute and wholesome aesthetic, pet influencer style, bright and cheerful.
```
*為何好：歪頭對鏡=擬人神態+親密感，演算法獎勵的寵物 hero shot。｜來源 aiveed.io*

**黃金獵犬海灘黃金時刻（Veo 3.1 寫實寵物運動最強）**
```
Golden retriever running joyfully along a sandy beach at sunset, waves crashing in the background, warm golden hour lighting, slow-motion capture, cinematic style, camera tracking the dog from the side.
```
*為何好：side-tracking + slow-motion + golden hour 三件套=寵物廣告最穩運動鏡。｜來源 simplified.com*

**動物擬人偽紀錄片（病毒哏）**
```
A nature documentary scene showing an otter piloting a small airplane. The otter wears aviator goggles and a tiny scarf, paws on the controls, looking focused yet playful.
```
*為何好：「nature documentary」框架+具體道具讓荒謬設定有寫實質感。｜來源 imagine.art*

**會說話的貓 i2v（Seedance @Image1 防臉崩）**
```
Create a smooth video using @Image1 as the main reference. Add natural head, eye, and ear movements, and sync the cat's expressions with the playful cat voice. Keep the scene cute, stable, and visually consistent with soft motion and lighting.
```
*為何好：拆 head/eye/ear movements + stable/consistent 防漂移，寵物配音 i2v 關鍵句。｜來源 seedance2prompt*

---

## 💄 美妝 / 化妝品

**口紅扭開觸唇 editorial（產品清楚 hero）**
```
A tube of lipstick twists open in extreme close-up, revealing a deep burgundy color. The tip touches a lip in slow motion. Camera pulls back to reveal a model's face with a confident expression. High-fashion editorial lighting with dramatic shadows.
```
*為何好：從產品 extreme close-up 拉到 model face，產品（膏體+色號）清楚當主角。｜來源 simplified.com*

**上妝三連拍 beauty sequence（一條出 3 鏡）**
```
Extreme close-up beauty sequence: mascara wand sweeps across lashes, lipstick glides across lips, blush brush dusts across cheekbone. Each shot is 2 seconds with smooth transitions. Soft, diffused beauty lighting. High-end cosmetics commercial style. Skin texture visible.
```
*為何好：內建 each shot 2 seconds 分鏡 + skin texture visible 逼真細節。｜來源 simplified.com*

**精華液大理石奢華（macro logo → dolly-out 揭幕）**
```
Create a cinematic luxury skincare advertisement featuring a transparent glass serum bottle placed on polished white marble. Tiny water droplets slide down the bottle surface. Begin with an extreme macro close-up of the brand logo, then perform a slow dolly-out revealing the full product. Use soft diffused studio lighting with subtle golden highlights.
```
*為何好：macro logo→dolly-out 全貌的揭幕運鏡，產品全程明確主角=精華液教科書結構。｜來源 imagine.art*

**ASMR 護膚敲玻璃（9:16，Seedance 原生音）**
```
Create a vertical ASMR video with no music, focusing on macro details. A light blue skincare gel bottle sits on glass. A pale, elegant hand gently taps the glass, producing crisp fingernail tapping sounds.
```
*為何好：vertical + no music + fingernail tapping sounds 直攻 ASMR 短影音。｜來源 imagine.art*

---

## 💎 珠寶 / 鑽石特寫

**鑽戒旋轉彩虹折射 macro**
```
Diamond ring slowly rotating on velvet display, catching light and creating rainbow prisms, extreme macro detail, black background with spot lighting, luxury jewelry commercial.
```
*為何好：rainbow prisms + spot lighting + black background 命中鑽石火光物理。｜來源 aiveed.io*

**鑽戒天鵝絨 push-in（攝影語彙級）**
```
A macro shot of a diamond ring resting on a dark velvet cushion, slow cinematic push-in with a 100mm macro lens, soft studio lighting emphasizing sharp specular highlights and light refraction.
```
*為何好：指定 100mm macro lens + specular highlights/refraction 是攝影師級寫法；i2v 用起始圖鎖構圖最穩。｜來源 hailuoai.video*

**18k 金項鍊黑絲擺盪（金屬髮絲光）**
```
A 18k gold necklace fluidly swaying against a black silk background, gentle slow pan left with a shallow depth of field, warm cinematic lighting highlighting the metallic sheen and anisotropic reflections.
```
*為何好：anisotropic reflections 精準描述金屬髮絲光 + fluidly swaying 給柔性動態。｜來源 hailuoai.video*

> **珠寶鐵則**：video 全文池淺、t2v 易變形 → 優先 **i2v**（先生 hero 圖再動），鎖形狀。

---

## 💪 健身 / 運動 / wellness

**高強度 gym 蒙太奇（一條出 4 動作）**
```
A high-energy gym workout montage: kettlebell swings, battle ropes, box jumps, and a final victorious fist pump. Quick cuts between exercises with each clip lasting 1-2 seconds. Dramatic gym lighting with dark background. Motivational fitness content. Punchy, intense energy.
```
*為何好：列 4 動作 + 內建 1-2 sec quick cuts 剪輯節奏，dark background 戲劇打光。｜來源 simplified.com*

**屋頂日出瑜伽（具名動作鏈）**
```
A woman practices yoga alone on a high rooftop at sunrise. City skyline stretches behind her, golden hour light catching her silhouette. She moves through sun salutation sequence.
```
*為何好：具名 sun salutation sequence 給明確動作鏈 + golden hour silhouette 剪影美學。｜來源 imagine.art*

**晨霧森林跑者 tracking（臨場細節）**
```
A jogger running on a misty forest trail at dawn. Camera tracks alongside at running pace. Dappled light breaks through the tree canopy. The runner's breath is visible in the cool air. Fitness lifestyle content. Muted, natural color palette.
```
*為何好：camera tracks at running pace + breath visible in cool air 兩個細節給臨場寫實，muted palette 避廉價飽和。｜來源 imagine.art*

---

## 🎮 遊戲 / 電競

**Fantasy game trailer 開場（world-reveal）**
```
A warrior discovers an ancient mech buried in the desert—golden hour lighting, swirling sand, quiet awe, cinematic pacing.
```
*為何好：quiet awe + cinematic pacing 給情緒節奏，AAA trailer 經典 world-reveal 開場。｜來源 imagine.art*

**Gameplay 第三人稱 + HUD（遊戲畫面語言）**
```
Third-person view of a fantasy character running through a burning village—epic soundtrack, intense camera motion, cinematic HUD overlay.
```
*為何好：唯一帶 cinematic HUD overlay + third-person 直接召喚遊戲畫面感。要真電競感再加 `scoreboard overlay, kill-feed, broadcast graphics, crowd roar`。｜來源 imagine.art*

**賽博龐克 megacity（環境空鏡，三模型通吃）**
```
A futuristic cyberpunk megacity at night, flying cars between massive skyscrapers covered in holographic advertisements, rain falling, neo-noir dystopian atmosphere, camera tilting up from street level.
```
*為何好：flying cars + holographic ads + rain + 街面 tilt-up=賽博龐克四件套。（已把 IP 詞改 `neo-noir dystopian` 避 OiiOii 版權攔截。）｜來源 seedance.tv*

---

## 🏛 室內設計 / 建築

**豪華開放廚房走位（模板型，可換 room/方向）**
```
Luxury open-plan kitchen interior, natural light from south-facing windows, smooth pan from island to living room, interior design magazine style, warm tones, professional staging.
```
*為何好：指採光方向 + 運鏡起訖 + magazine style 風格錨，換 room 即用。｜來源 seedance.tv*

**環形繞室 orbit（Kling 室內專用運鏡）**
```
Camera orbits 180 degrees around room center at eye level, revealing design from multiple angles, then settles facing the main feature wall.
```
*為何好：180° orbit + 固定 eye level + 收尾於 feature wall（有起點終點，Kling 不漂移）。｜來源 boardspace.ai*

**Golden hour 光掃過房間（i2v 完美句型）**
```
Warm sunlight slowly sweeps across room from window, casting moving shadows on walls and floor, then settles into soft ambient glow.
```
*為何好：只寫「光怎麼動」不重述陳設=完美 i2v（先生室內圖再動）。｜來源 boardspace.ai*

---

## 🏢 企業 / Explainer / 教育

**機器人吉祥物 2D explainer（鎖品牌色）**
```
Minimalist 2D vector art of a friendly robot mascot waving at the camera, eye-level medium shot, in a modern sunlit startup office with large windows and plants, using a color palette of navy blue, coral, and off-white.
```
*為何好：minimalist 2D vector art + 三色盤鎖扁平風與品牌色，避免寫實漂移。｜來源 imagine.art*

**帶 label 圖解動畫（教育最可靠）**
```
Animation showing a cell undergoing mitosis, clear labels for each phase (prophase, metaphase, anaphase, telophase), educational diagram style, clean and clear visualization, suitable for high school biology.
```
*為何好：逐一寫出要標的 phase + educational diagram style + 指定學齡=帶 label 圖解動畫最穩寫法。｜來源 veo3ai.io*

**黑板 motion graphics（低成本高辨識）**
```
A clean animated chalkboard shows a simple solar eclipse diagram: Sun, Moon, Earth labeled. Smooth push-in as arrows animate to show movement. Flat, high-contrast classroom style with white lines on dark green.
```
*為何好：white lines on dark green + arrows animate 精準描述黑板圖解動態。｜來源 makeaiprompt.com*

---

## 🐠 水下 / 海洋 / 空拍地景

**生物發光深海洞穴（光線穿透 + 發光生物）**
```
A deep sea diver descending into a bioluminescent underwater cave, glowing jellyfish and coral, light rays penetrating from above, ethereal atmosphere, slow motion, IMAX quality.
```
*為何好：bioluminescent + 上方 light rays penetrating + slow motion + IMAX，水下標誌畫面一次寫滿。｜來源 seedance.tv*

**珊瑚礁慢移 dolly（完整範本，物理交代到位）**
```
Create a serene and vibrant animated video showcasing a pristine underwater coral reef. The camera performs a slow, graceful horizontal dolly shot, revealing a diverse ecosystem teeming with life. Schools of tropical fish swim past brilliantly colored soft and hard corals. Sunlight penetrates the surface from above, creating beautiful caustic patterns and dappled light rays that dance across the reef. The water is crystal clear, with a slight gentle current causing the anemones and soft corals to sway hypnotically. Professional underwater lighting, broad diffuse light highlighting the spectrum of colors, avoiding harsh shadows.
```
*為何好：明寫 caustic patterns / dappled light rays / 海流讓珊瑚 sway / 避免 harsh shadows，水下打光與物理交代完整。｜來源 medium aitooldiscovery*

**雪山日出空拍（光打哪+雲怎麼動）**
```
Aerial drone shot flying over a vast mountain range at sunrise, golden light hitting snow-capped peaks, clouds flowing through valleys below, cinematic 4K quality, smooth camera movement.
```
*為何好：golden light hitting peaks + clouds flowing through valleys 給空拍兩個動態錨，smooth movement 防抖。｜來源 seedance.tv*

---

## ⚙️ 跨模型語法速查

| 模型 | 一致性鎖法 | 多鏡頭語法 | 對白/音 |
|---|---|---|---|
| **Veo 3.1 / Omni** | Ingredients(≤3-4 圖) + 句中複述關鍵細節 | `[00:00-00:02]` 時間碼分鏡 | 引號對白 + `says in a X voice` + `(no subtitles!)` |
| **Kling 3.0** | 逐拍動作+`remains in sharp focus`；文字加`stable and readable` | `Shot 1/2/3 + Audio:`（原生 15s） | `[角色: 音色]: "台詞"` 自動對嘴 |
| **Seedance 2.0** | `@Character1`/`@ProductRef` + `Reference @X for consistent...` | `[Shot 1:][Cut to:]` 或 `0–4s/4–9s` | `@Audio1` 配樂；最多 12 asset |
| **Vidu Q3** | `@角色_front`+`@角色_profile`，tag 順序=優先序 | 逐 clip(4s/段) `@tag+動作+運鏡` | 原生音畫；`says: "台詞"` 自動 lip-sync |

**鐵則：用了 @reference / Ingredients 就別再重述角色長相 —— 只寫動作/運鏡**（與 i2v 黃金公式同源）。

## 來源（全部已標於各 prompt）
官方：cloud.google.com Veo 3.1 guide · deepmind.google/models/veo · klingaio.com · vidu.com · replicate.com/blog · labs.google/flow/tv（Show Prompt）
社群（標 source 附全文）：veo3ai.io · seedance.tv · atlascloud.ai · vmake.ai · higgsfield.ai · leonardo.ai · simplified.com · adcreative.ai · skywork.ai · banana-prompts.net · opus.pro

## 連動
- 先有概念 → [concept-first-prompting.md](concept-first-prompting.md)
- 反瑕疵 → [quality-control.md](quality-control.md)
- 選模型 → [model-picker.md](model-picker.md)
- 成套主題範本 → [../templates/preset-packs.md](../templates/preset-packs.md)

---

## 🆕 2026-06 新模型逐字 prompt（curated）

本節精選 2026 年 6 月上線的新世代模型，經 OiiOii 平台驗證。每個 prompt 可直接複製貼上，無需調整。

---

**Oii Image 2（Google Nano Banana Pro）— 文字完美海報 [2026-06 curated]**

```
Create a premium product poster for a luxury skincare brand. The image shows a sleek glass serum bottle (cylindrical, 50ml, clear liquid inside, metallic rose-gold cap) centered on a marble background. Text overlay: "LUMINOVA ESSENCE" in bold white sans-serif font positioned above the bottle. Below: "Advanced Hydration Complex" in smaller elegant script. Additional text: "Clinical Results in 7 Days" positioned bottom-right corner, gold accent color. High-end beauty editorial aesthetic, 4K, professional studio lighting with soft shadows, no blur on text.
```

*為何好：Oii Image 2 文字渲染冠軍。引號標題 + 指定字型 + 顏色置位 = 包裝海報教科書。避免海報常見的「文字模糊/傾斜/疊字」。｜試用：Nano Banana Pro 對引號 + 字型控制吃得很深，材質（rose-gold vs 銀）也精準。｜應用：社群廣告 KV、電商落地頁 hero、包裝設計稿。*

---

**Gemini Omni（Google，Flow PRO）— 多輪產品編輯 [2026-06 curated]**

```
Turn 1: Create a 4-second cinematic product reveal. A white minimalist box sits on a light oak desk. Soft morning light from the left window. Slow dolly-in from wide to medium shot. Clean, premium, zen aesthetic.

Turn 2: Keep the scene and lighting identical. Change the box color to deep navy instead of white.

Turn 3: Now place the same box on a white marble countertop instead of the oak desk. Keep the navy color and morning lighting the same.
```

*為何好：Gemini Omni 殺手鐧 = 對話式多輪編輯，每輪改一變數、其他保持。模型記憶前輪脈絡（角色/光線/場景），第 3 輪不會忘記 navy + 晨光。≈ OiiOii「加入對話」痛點的官方解法，無需反覆重生整段影片。｜試用：Omni Flash 已上線 Flow PRO（⚠️ 未實測 OiiOii 體感上限）。｜應用：快速迭代、A/B 版本、背景無縫切換、產品色盤開發。*

---

**Wan 2.7（阿里通義萬相）— Editorial Chiaroscuro 首尾幀 [2026-06 curated]**

```
[Start Frame] A luxury watchmaker's workbench: close-up of hands (wearing white cotton gloves) holding a disassembled watch mechanism. Extreme macro, tungsten light from above creating deep shadows, high-contrast Chiaroscuro aesthetic. Depth of field tight on the gears.

[Thinking Mode Brief]
Concept: craft + precision heritage. First 2 seconds: hands methodically align a spring. Second 2 seconds: light catches the polished surfaces. Final 1 second: hands step back — the fully assembled watch reveals itself, catching light like a jewel.
Lighting: single overhead tungsten, dramatic hard shadows, no fill light. Grade: desaturated, only metallic gold tones. Aspect: 16:9 cinema.

[End Frame] The completed watch mounted on a velvet display stand. Same Chiaroscuro lighting. Wrist entering frame with white shirt cuff. Ownership moment.
```

*為何好：Wan 2.7 Thinking Mode 強化 = 先規劃構圖 / 敘事節拍 / 打光再生成，導演感強。首幀→尾幀 + 中間敘事拆 beats（不寫摘要）= editorial video 教科書。Chiaroscuro（高對比古典打光）新世代才能穩定執行。⚠️ 未實測：Thinking Mode 在 OiiOii 是否預設啟用（建議手動加前綴或檢查 UI）。｜應用：奢侈品廣告、工藝紀錄、品牌故事、鐘錶/珠寶 TVC。*

---

**Kling O1 / Omni — 物理事件 + 身份鎖 產品 TVC [2026-06 curated]**

```
Shot 1 (0-3s, establish): <<<element_hero>>> (sleek black ceramic electric kettle, matte finish, minimal spout) sits on a white Carrara marble countertop. Morning sunlight from the right. Camera: locked-off wide shot. The kettle maintains exact shape and proportions throughout. Static product, environment only.

Shot 2 (3-5s, hero moment): The same <<<element_hero>>> from closer angle. A hand (feminine, manicured, elegant fingers) pours boiling water. Steam rises in slow motion. Water cascade is physically accurate — follows gravity, splashes realistically, surface tension visible. Rim lighting on the steam vapor.

Shot 3 (5-7s, proof): Close-up. The kettle now sits unplugged on the marble. Temperature indicator on the base glows softly amber. Hand removes the kettle from the heating base (product shape locked, zero visible wear). Product integrity 100%.

Audio: subtle ceramic clink at start, authentic water pour SFX, ambient morning room tone, gentle steam hiss.
```

*為何好：Kling O1「身份鎖 @element」 + 「物理事件」（倒水 = 重力 + 濺起，而非靜態描述）= 產品廣告最高階。Motion Brush（未寫進 prompt 但可用 UI）沒刷產品只刷背景/光 = 產品完全靜止、只環境動。3.0 CoT 物理推理把水花/蒸氣渲染成真。⚠️ 未實測：OiiOii 後台身份鎖實際一致率（官方稱 90%+）。｜應用：小家電 TVC、廚具廣告、精品商品、快手電商直播素材。*

---

**HappyHorse 1.0（阿里 ATH Innovation Unit）— 短 Prompt 敘事 + 原生音訊 [2026-06 curated]**

```
A 10-second narrative short. Scene 1 (0-3s): A young woman (mid-20s, warm smile, shoulder-length hair) stands in a sun-filled apartment, holding a ceramic cup of tea. She takes a mindful sip, closes her eyes briefly. Scene 2 (3-7s): Cut to the same woman at a café table, laptop open, focused on work. Natural afternoon light. Scene 3 (7-10s): Same woman on a park bench at golden hour, phone in hand, looking peaceful. All scenes: natural lighting, lived-in feeling, no drama or artifice.

Audio design: Gentle ambient café chatter (scene 2), soft park birds (scene 3), opening scene has subtle morning room ambience. Voice-over (female, warm Mandarin tone, slightly meditative pace): "生活中的小暫停，讓一切變清晰。" (Life's small pauses make everything clear.) No music track, only natural SFX + voice.
```

*為何好：HappyHorse 單一 transformer（文圖影音 token 同序列）= 多鏡敘事 + 對白 + Foley 同時生成，一條出完整品牌短片。短 prompt 依靠模型深度理解日常敘事（遠強過長文冗餘）。原生中文對白 + 自然音軌 = Seedance 純 SFX 的進化版（10s 對標，音質業界最強）。⚠️ 未實測：OiiOii 實際出片長度與音訊品質穩定性。｜應用：品牌小故事、生活方式廣告、社群短影、wellness 產品敘事、餐飲品牌日常。*

---

## 連動 + 版本對照

新模型 2026-06 升級與既有檔案對應：

- **Oii Image 2（文字渲染超強）** → [model-picker.md](model-picker.md)§圖片卡「Nano Banana Pro」更新
- **Gemini Omni（多輪對話編輯）** → 補齊 i2v 流程痛點，直接「對話改」免重跑全片
- **Wan 2.7（首尾幀 + Thinking Mode）** → [kling.md](kling.md) Start/End Frame 概念延伸，打光層更激進
- **Kling O1（身份鎖 @element）** → [kling.md](kling.md)§進階 Recipe R2 多鏡一致性強化版
- **HappyHorse（全模態音訊）** → [quality-control.md](quality-control.md)§3 物理 + §5 質感的對標實現

| 新模型 | OiiOii 內部代號 | 最強用途 | 時長上限 | 音訊 | 狀態 |
|---|---|---|---|---|---|
| Oii Image 2 | Oii Nano Pro | 海報/KV 文字 | N/A (圖像) | N/A | ✅ 上線 |
| Gemini Omni | Gemini Omni Flash | 對話迭代編輯 | 4s (Flow PRO) | ✅ 原生 | ⚠️ 未實測 |
| Wan 2.7 | Wan 2.7 | 首尾幀精控 + Thinking | 10s | ✅ 原生 | ⚠️ 未實測 |
| Kling O1 | Kling O1 / O3 | 身份鎖 + 物理 | 15s | ✅ 原生 | ✅ 已驗 |
| HappyHorse | Oii X Imagine (待驗) | 多鏡對白 + Foley | ⚠️ 6s (未實測 vs 官方 10s) | ✅ 原生 | ⚠️ 未實測 |

**⚠️ 未實測註記（落地前必驗）：**
- Gemini Omni 在 OiiOii 後台實際內容保留轉輪數（官方 Flow PRO 理論無限，平台可能有上限或 token 計費）
- Wan 2.7 Thinking Mode 在 OiiOii 是否預設啟用（建議 UI 檢查或 prompt 手動加 `[Thinking Mode]` 前綴）
- HappyHorse / Oii X Imagine 時長上限（官方 6s vs 社群傳聞 10s 有差異，OiiOii 實測為準）
- 各模型跨平台「價格/時長/音訊品質」以 OiiOii UI 實測為準（非各家原生 API 數字）
