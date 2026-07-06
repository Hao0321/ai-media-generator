# Preset Packs — 可直接套用的 Prompt 食譜

**定位：** 當使用者想要「Wes Anderson 風的 5 秒鏡頭」、「Nike 級運動廣告」、「Blade Runner 2049 雨夜」、「宮崎駿夢幻森林」— 不用每次重新從 [cinematic-direction.md](../references/cinematic-direction.md) 挑 token，直接來這裡抓現成的。

**使用方式：**
1. 按「任務類型」找最近的 preset
2. 把 `{BRACKETED}` 占位符換成你的主體/場景
3. 直接貼到目標平台 (若平台建議不同，見每個 preset 的 `Target`)

**每個 preset 的格式：**
- **Use case** — 什麼情境
- **Target** — 推薦平台 + 參數
- **Prompt** — 完整可貼
- **Negative** — 負面提示
- **Why** — 為什麼這些 token 管用
- **Swap points** — 可以改的地方

---

## 📽️ 電影感 Presets (Cinematic)

### 1. Blade Runner 2049 Rainy Cyberpunk (10s)

**Use case:** 賽博龐克雨夜、偵探/復古未來科幻、霓虹城市
**Target:** Kling 2.1 Master 或 2.6 Pro，10s，2.39:1

```
Shot by Roger Deakins, Panavision anamorphic 50mm at T2.0, Arri Alexa LF.
Medium close-up of {SUBJECT: 穿米色長風衣的男子} walking slowly through a
rain-soaked alley at night. Practical neon signs in magenta and cyan glow
from behind him, their reflections rippling in puddles. Cinestill 800T
emulation with heavy red halation around every light source. Volumetric
atmospheric haze. Teal shadows, sodium-amber highlights, 2.39:1 anamorphic
horizontal lens flares. Low-key 4:1 contrast. Slow dolly-in tracking from
slightly below. Breath visible in cold air. Rain streaks backlit as silver
needles.
```

**Negative:**
```
cartoon, anime, 3D render, bright daylight, cheerful, flat lighting,
low contrast, clean, pristine, oversaturated
```

**Why:** Deakins + Panavision anamorphic 是 Blade Runner 2049 招牌。Cinestill 800T halation 是霓虹夜拍的招牌效果。Teal/amber + 2.39:1 鎖住電影感。

**Swap points:** `{SUBJECT}` 換角色；`magenta and cyan` 可改 `orange and teal`；`alley` 可改 `highway underpass` / `subway platform`。

---

### 2. Wes Anderson Symmetrical Deadpan (5s)

**Use case:** 對稱可愛、書本感、粉彩、微幽默
**Target:** Midjourney v7 (`--niji 7 --s 300` 可選) 或 Kling / Sora

```
Wes Anderson style cinematography, perfect vertical symmetry, centered
composition. Flat dollhouse-like tableau of {SUBJECT: 一位身穿薄荷綠制服
的飯店門童} standing at the center of an ornate pastel pink hotel lobby.
Meticulously balanced framing, symmetric everything. Pastel palette: powder
blue, cream, salmon pink, buttercream yellow. Soft diffused daylight,
shadowless, 35mm Kodak film grain. Futura bold signage visible. Subject
faces camera deadpan, hands at sides. Slow whip pan entry or static medium
shot. 2.66:1 Grand Budapest Hotel aspect ratio.
```

**Negative:**
```
dark, moody, asymmetric, gritty, realistic, desaturated, horror, chaotic,
modern minimalist
```

**Why:** Anderson 最招牌的三件事：對稱 + 粉彩 + 中景定格。Futura 字體 token 把「Anderson 世界」感鎖死。

**Swap points:** 把 `hotel lobby` 換成 `submarine interior` / `ski lodge` / `train compartment`；色盤可換 `mustard / olive / burnt orange` (Royal Tenenbaums 版)。

---

### 3. Christopher Nolan IMAX Epic (10s)

**Use case:** 宏大、科幻/戰爭/驚悚、建築感、史詩
**Target:** Kling / Veo 3.1 / Sora 2, 10s, 16:9

```
Shot by Hoyte van Hoytema, IMAX 70mm Kodak Vision3 500T, wide field of
view with ultra-shallow depth of field isolating {SUBJECT: 一位穿著物理
學家大衣的男子}. Nolan-inspired architectural scale, low-angle hero
composition. Subject stands small against massive brutalist concrete
interior, vast space dwarfing him. Desaturated cool blue-grey palette,
minimal warmth, only one practical tungsten source. 1.43:1 IMAX aspect or
2.39:1. Practical explosions or light sources only, no CGI. Deep crushed
shadows, highlight roll-off on concrete texture. Slow push-in dolly.
```

**Negative:**
```
warm, cozy, domestic, small scale, saturated, cartoonish, flat composition,
low budget
```

**Why:** Hoytema IMAX + Nolan 建築尺度 + 冷藍色調 = Oppenheimer/Dunkirk/Tenet 配方。Practical explosions 防止 AI 走 CGI 感。

---

### 4. Denis Villeneuve Monumental (10s)

**Use case:** 沙漠科幻、太空、單人面對宇宙、Dune 感
**Target:** Kling 2.6 Pro / Seedance 1.0 Pro, 10s, 2.39:1

```
Greig Fraser cinematography, Arri Alexa 65 with Panavision Panaspeed.
Villeneuve monumental scale: {SUBJECT: a hooded figure in desert gear} as
a tiny silhouette against a massive alien landscape of ochre sand dunes
and brutalist geometric architecture looming in the haze. Desaturated
sepia and steel palette, muted warmth. Low contrast atmospheric dust
suspension, golden hour low sun with visible aerial perspective
compressing distance. Slow crane up revealing scale. 2.39:1 anamorphic,
no lens flare, restrained. Cinematic grain from Kodak Vision3 250D
emulation. Harsh wind-blown sand particles as visible texture.
```

**Negative:**
```
lush green, urban city, high saturation, close-up, intimate, warm cozy,
flat lighting, small scale
```

**Why:** Greig Fraser 大片幅 + Villeneuve 的「一個人對宇宙」視覺 = Dune/Arrival 招牌。沙塵粒子 + aerial perspective 做出尺度感。

---

### 5. Wong Kar-wai Neon Longing (10s)

**Use case:** 霓虹情調、慢動作步態、HK 90s、紅綠對撞
**Target:** Kling (運鏡最準)，10s，2.35:1

```
Wong Kar-wai style, Christopher Doyle handheld cinematography, stepped-
frame slow motion, Fuji 8mm film grain. {SUBJECT: 一位身穿紅色旗袍的女子}
walks slowly through a rain-slick Hong Kong alley at night, lit by buzzing
magenta and green neon signs. Saturated red and emerald color collision,
deep shadows, Christopher Doyle's signature hot saturated highlights. In
the Mood for Love palette. Medium close-up tracking behind her, stepped
slow motion at 48fps rendered at 24fps for print effect. Cinestill 800T
halation. Fan blowing hair. Cigarette smoke curling in light.
```

**Negative:**
```
bright daylight, modern, clean, minimalist, desaturated, wide landscape,
outdoors nature
```

**Why:** Doyle handheld + stepped frame 是 WKW 招牌。紅綠對撞 (旗袍紅 + 霓虹綠) 是他的色彩簽名。

---

### 6. Studio Ghibli / Makoto Shinkai Dreamy Anime (10s)

**Use case:** 日系動漫、溫柔懷舊、自然光奇幻、城市 + 星空
**Target:** Kling (風格強) 或 Midjourney `--niji 7 --s 250`, Sora 2

```
Makoto Shinkai cinematic anime combined with Studio Ghibli hand-painted
backgrounds. {SUBJECT: a teenage girl with a red school backpack} stands
on a rooftop at dusk, overlooking a hyper-detailed Tokyo cityscape with
volumetric god rays piercing through pink-orange cumulus clouds. Lens
flare across frame. Cel-shaded character with soft watercolor painted
background. Warm amber-to-cool blue gradient sky. Gentle wind lifting
hair. Bittersweet longing mood. 16:9, painterly aesthetic, subtle film
grain. One static wide then slow push-in to medium shot.
```

**Negative:**
```
photorealistic, 3D render, dark grim, cyberpunk, adult sophisticated,
muted colors, flat sky
```

**Why:** Shinkai 的 volumetric 光 + Ghibli 的手繪背景 = 目前 AI 模型對「動漫美感」最穩的雙錨。

---

### 7. Kurosawa Wuxia Duel (10s)

**Use case:** 武俠、武士、竹林、雨中決鬥
**Target:** Kling 2.6 Pro / Seedance 1.0 Pro, 10s

```
Kurosawa-inspired samurai epic, multi-camera telephoto compression feel.
{SUBJECT A: 一位穿深靛藍武服的武士} and {SUBJECT B: 一位穿褪紅袍的對手}
face each other in tense standoff in a rain-drenched bamboo grove at dawn.
Low-angle hero wide shot. Shot on Arri Alexa with 200mm compressed
background, bamboo stalks rhythmic. Practical wind machine blowing rain
sideways. Desaturated earth tones with blood-red accent. 2.39:1
anamorphic. 120fps slow motion on the draw of a blade, flash of silver.
Kodak Vision3 500T grain holding under overcast sky. No score, only wind
and rain.
```

**Negative:**
```
bright saturated colors, modern clothing, cartoon, clean environment,
cheerful, indoor, daylight sunny
```

**Why:** Kurosawa 的 telephoto + 雨 + 多角色 standoff 是招牌。200mm 壓縮把竹林變詩意節奏。

---

### 8. K-drama Romance Soft Warm (5s)

**Use case:** 韓劇、溫柔情感、柔焦、暖色
**Target:** Kling 2.1 Master / Seedance, 5s, 16:9 或 9:16

```
K-drama modern cinematography, Netflix original look. Medium close-up of
{SUBJECT: 一位穿米色毛衣的男子} turning slowly to look at camera, soft
longing smile. Warm diffused tungsten practical from window behind, subtle
rim light on hair. Muted peach and beige palette. Phase One IQ4 sensor
look, 85mm f/1.4 extreme shallow depth of field, creamy bokeh. Slight warm
color grade, clean skin tones, no grain. Steady tripod, almost imperceptible
drift. Breath visible, quiet intimate moment. Seoul apartment dusk.
```

**Negative:**
```
high contrast dramatic, harsh shadows, desaturated, urban grit, dark moody,
noir, cyberpunk, anime
```

**Why:** K-drama 的「暖 + 柔 + 85mm」是全球模型都認的美學。

---

### 9. A24 Indie Naturalism (8s)

**Use case:** 文青獨立、真實不美化、冷靜敘事
**Target:** Flux 1.1 Pro (靜) 或 Kling / Sora

```
A24 indie film aesthetic, naturalistic unglamorous cinematography. Medium
shot of {SUBJECT: a weary middle-aged woman in a thrift store cardigan}
sitting at a kitchen table in morning light. Handheld subtle shake, 35mm
film grain, Kodak Vision3 500T. Available natural light only, overcast
sky through window, no fill. Slight green color cast typical of A24. Real
skin texture with freckles visible, no beauty retouching. Asymmetric
composition, space above her head. Extended duration, stillness. Static
tripod or locked-off. 1.85:1.
```

**Negative:**
```
glossy, Hollywood glamor, saturated, dramatic lighting, stylized, epic,
cinematic score feel, commercial polish
```

**Why:** A24 是反 Hollywood 美學 — 綠色漂移 + 真實皮膚 + 自然光是密碼。

---

### 10. 1970s New Hollywood Grain (8s)

**Use case:** 復古美式、Taxi Driver / Dog Day / The Long Goodbye 感
**Target:** Kling / Midjourney `--s 200 --raw`

```
1970s New Hollywood grit cinematography, Kodak 5254 negative emulation
with heavy grain. {SUBJECT: a man in a brown corduroy suit} walks down a
litter-strewn New York street in late afternoon sun. Zoom lens from 70mm
to 35mm during the take. Hazy urban smog compressing light. Warm amber
sodium vapor street lamps just starting to buzz on. Saturated browns and
ochres with washed-out highlights. Film gate weave and light flicker.
1.85:1. Robert Mulligan / Gordon Willis cinematography.
```

**Negative:**
```
clean digital, modern, crisp, saturated, HDR, smooth, polished, daylight
bright
```

**Why:** Kodak 5254 + zoom + 1970s 城市髒亂 = Taxi Driver 配方。Gordon Willis (The Godfather DP) token 拉美學。

---

## 🎬 廣告 Presets (Commercial)

### 11. Apple Product Hero (15s)

**Use case:** 科技產品、極簡奢華、白底無雜
**Target:** Flux 1.1 Pro (靜), 4:5 或 16:9

```
Apple product commercial aesthetic. {PRODUCT: iPhone 17 Pro in titanium}
floating in pure white infinity cyclorama. Impossibly clean seamless
gradient background, no visible light source. 100mm macro at f/8 on
product, razor-sharp edge detail. Slow dolly-in + product rotation on
invisible axis. Perfect reflection on imaginary glass table surface. Cool
white 5600K lighting with rim light separating product from background.
Phase One IQ4 150MP rendering, commercial product photography precision,
zero grain, zero imperfection. 35% of frame is negative space top and
bottom for headline text.
```

**Negative:**
```
cluttered, dark, moody, grainy, vintage, warm, organic, handheld, rustic,
filmic
```

**Why:** Apple 廣告的 DNA 是 `infinity cyclorama + macro + 冷 5600K + 留白`。

---

### 12. Nike Heroic Sport (30s, tell as shot list)

**Use case:** 運動品牌、激勵、低角、慢動作
**Target:** Kling 2.6 Pro + Phantom slow-mo token

```
Nike commercial heroic aesthetic, 200fps Phantom Flex 4K slow motion.

Shot 1 (0-3s): Extreme close-up of {SUBJECT: a female sprinter}'s eye
reflecting the finish line, sweat bead rolling down brow, harsh stadium
arc lamp rim light. Alexa 65, 100mm macro.

Shot 2 (3-10s): Low-angle tracking following feet pounding the track,
200mm telephoto compressed stadium lights into bokeh orbs. Dust kicks up
in slow motion.

Shot 3 (10-20s): Hero silhouette mid-stride, arms raised, backlit against
stadium arc lamp. Sweat droplets frozen in air. Saturated brand red gel
light from behind. Bleach bypass crushed blacks.

Shot 4 (20-30s): Athlete collapses to knees on finish line, warm golden
hour sun bleeds through stadium structure. Tear mixes with sweat. Kodak
Vision3 500T, natural grain. Bold white sans-serif tagline space bottom.
```

**Negative:**
```
clean polished beauty, soft warm cozy, feminine delicate, urban casual,
quiet still, pastel
```

**Why:** Nike 的「低機位 + 慢動作 + 汗珠 + bleach bypass」是三十年不變的配方。

---

### 13. Chanel Luxury Fashion (30s)

**Use case:** 奢侈品、時尚電影感、黑白或金色
**Target:** Runway Gen-4 / Flux 1.1 Pro / Kling, 靜態美

```
Chanel-inspired luxury fashion film. Medium close-up of {SUBJECT: a woman
in pearls and black silk dress} with face partially obscured by shadow or
turned away. Phase One medium format, 80mm f/2.8, shot through a gauze
scrim. Natural window light from Parisian apartment, soft north-facing
daylight only. Muted ivory and gold palette with deep charcoal shadows.
Kodak Portra 400 in 120 medium format, Noritsu scan, subtle grain and
light leak. 35mm film aesthetic, restrained refinement. Slow 2-second
static beats with slight camera drift. 4:5 aspect for print campaign
use. No branding visible.
```

**Negative:**
```
bright colorful, saturated, commercial product shot, cluttered, casual,
modern minimal, tech, industrial, suburban
```

**Why:** Chanel/LV 等奢侈品的密碼 = 中片幅 + 自然窗光 + 巴黎公寓 + 不露臉。

---

### 14. Rolex Macro Watch (15s)

**Use case:** 精密機械、微距、奢侈錶
**Target:** Flux 1.1 Pro (靜態細節最強), 1:1

```
Haute horlogerie extreme macro. {PRODUCT: a vintage Rolex Submariner
watch} on seamless obsidian black velvet. 100mm macro at f/11 focus-
stacked, micron-level sharpness on dial engravings and crown serrations.
3-point lighting: softbox 45° key, backlit rim separating bezel from
black, fill card for shadow detail. Sub-pixel sharp, crystal perfectly
clear. Slight shallow DoF on movement jewels in a second focus-stack
frame. Cool 5000K daylight balance. Deep obsidian blacks, saturated
metallic highlights, 1:1 square. Phase One XF IQ4 150MP. No retouching
needed — it was shot right.
```

**Negative:**
```
handheld, soft, dreamy, warm, vintage film grain, flat, amateur, colorful
background, dirty, scratched
```

**Why:** 奢華錶的三件事：`100mm macro + focus stack + obsidian velvet`。沒有第四樣。

---

### 15. Coca-Cola Warm Family (30s)

**Use case:** 快消溫情、家庭、節慶、亮色
**Target:** Seedance 1.0 Pro (multi-shot), 30s, 16:9

```
Coca-Cola commercial warmth. Three-beat 30-second family scene.

Beat 1: Aerial drone dusk shot descending toward a suburban home with
warm glowing windows, decorated for holidays, Kodak Portra 800 emulation.

Cut to: Medium shot of a multi-generational family gathered at a dinner
table, warm tungsten practical chandelier overhead, soft amber glow on
faces. Shallow DoF 85mm, handheld subtle intimate. {SUBJECT: 一個小女孩}
reaches for a chilled Coke bottle, condensation on glass catching light.

Cut to: Extreme close-up of the bottle cap opening with a satisfying
"kss" sound, bubbles rising. 240fps slow motion. Warm saturated brand red
as the dominant accent against amber interior. Portra 400 grain.
```

**Negative:**
```
cold, blue, minimalist, dark, sad, alone, industrial, urban grit,
cyberpunk, moody
```

**Why:** 溫情廣告的密碼：`warm tungsten + Portra 顆粒 + 手持 + 低 DoF 家人特寫`。

---

### 16. Tesla Tech Futurism (10s)

**Use case:** 科技前衛、極簡、冷調、未來感
**Target:** Veo 3.1 / Kling, 10s

```
Tesla-inspired futuristic commercial. {PRODUCT: an all-white minimalist
car} parked in a seamless white environment with deep ocean blue gradient
floor. Cool 6500K lighting, zero warmth. Single key softbox from 45° top,
practical LED floor accent lighting brand blue. Aerospace precision
composition, 2.35:1 wide. Slow dolly-in + 360° orbit combined. Zero grain,
zero film feel, pure digital clean. Low contrast smooth highlights on
body paint. Panavision Primo 50mm at f/5.6. Subtle reflection of clean
geometric overhead light on polished chrome. High-tech, near-silent
machine aesthetic.
```

**Negative:**
```
warm, vintage, film grain, organic, suburban, messy, rustic, saturated,
hand-crafted
```

**Why:** Tesla/Apple 共享「零暖色 + 零顆粒 + 精確幾何」的未來感 DNA。

---

## 🎼 MV Presets (Music Video)

### 17. Hiro Murai Surreal Lowlight (20s)

**Use case:** 超現實 MV、Childish Gambino / FKA twigs 感
**Target:** Kling / Sora 2, 20s (extend to 10s + 10s)

```
Hiro Murai surreal dreamlike aesthetic. One continuous handheld take of
{SUBJECT: a figure in a white robe} walking through a dimly lit abandoned
suburban street at night. Practical streetlamp every 30 feet providing
pools of warm tungsten, darkness between. Thin layer of ground haze
catching light. Desaturated palette, warm amber pools against crushed blue-
green darkness. Slow deliberate pace, unsettling stillness. Silhouette
revealed and re-absorbed by shadow. Kodak Vision3 500T, visible grain.
35mm Alexa, subtle handheld float. One take, no cuts. Run-down decay
textures on houses behind. Subject occasionally turns to look off-frame,
no payoff reveal.
```

**Negative:**
```
bright, saturated, cheerful, urban glamorous, high-energy, fast cuts,
daylight, clean
```

**Why:** Murai 的「一個人 + 低光 + 郊區 + 一鏡到底 + 不解釋」= Atlanta / This Is America MV 美學。

---

### 18. Cole Bennett Lyrical Lemonade (15s)

**Use case:** Hip-hop MV、動漫 cut-out、高飽和特效
**Target:** Runway Gen-4 + Aleph (特效疊加) 或 Kling

```
Cole Bennett Lyrical Lemonade aesthetic. {SUBJECT: a rapper in chrome
chain and designer fit} performs directly to camera in a warped surreal
environment. Fisheye 14mm wide. Saturated primary colors. Cartoon-style
cutout VFX overlays: glowing halos, cash symbols, anime-style impact
stars reacting to beats. Camera zooms aggressively in/out matched to
bass drops. Smoke bomb color gas filling background. Practical strobes
flash on beat. Bleach-bypass contrast, punchy saturated reds and yellows
and cyans. 24fps standard speed with occasional step-frame on punch lines.
```

**Negative:**
```
realistic naturalistic, subtle, muted, cinematic slow, arthouse, luxury
static, minimalist
```

**Why:** Bennett 的 MV DNA = `fisheye + cut-out VFX + 飽和色煙 + 節拍剪輯`。

---

### 19. Jonathan Glazer Clinical Unease (15s)

**Use case:** 冷感 MV、高級 fashion film、Radiohead / Massive Attack 感
**Target:** Runway Gen-4 / Kling, 15s

```
Jonathan Glazer clinical precision. Static locked-off medium shot.
{SUBJECT: a figure in institutional white clothing} standing motionless
in a sterile empty white cube space, slight tilt of head. Cold 5600K
fluorescent overhead flat lighting, zero shadow character, surgical
clinical. Desaturated palette, dead whites and pale skin. Extended
duration, subject barely breathes. Unsettling symmetry. Uncomfortable
stillness. Sennheiser microphone-dry room tone only. Shot on Alexa 35
with clean digital rendering, slight film grain layer Vision3 200T. 1.85:1.
```

**Negative:**
```
dynamic, fast, warm, cozy, saturated colorful, handheld energetic,
performance, dancing, party, bright
```

**Why:** Glazer 的不安來自「看起來像醫院/監獄但沒事發生」。Zero shadow + static + 延長時長是密碼。

---

### 20. Michel Gondry Handmade Whimsy (15s)

**Use case:** 童趣實驗 MV、紙藝、Björk / Rosalía / White Stripes 感
**Target:** Kling (物理強), 15s

```
Michel Gondry handmade practical magic. {SUBJECT: a woman in a yellow
raincoat} walks through a visibly cardboard-constructed miniature city
with paper clouds and yarn rivers. In-camera practical effects only —
visible tape, visible cardboard, charming imperfection. Warm childlike
wonder aesthetic. Stop-motion jitter feel. Subtle handheld. Saturated
primary colors with clear hand-crafted texture. Super 8mm film emulation,
grain and light leaks. Available natural light, no fill. Whimsical
impossible scale contrasts (giant flower, tiny person). 1.66:1.
```

**Negative:**
```
digital CGI, photorealistic, polished commercial, minimalist clean, dark
moody, horror, serious dramatic
```

**Why:** Gondry 的魔法是「你看得見道具」。Prompt 寫明 `cardboard / tape / yarn` 反而保留手工感。

---

## 🎨 時尚 / 美妝 Presets

### 21. Vogue Italia Editorial (15s)

**Use case:** 高端編輯、時尚雜誌封面級
**Target:** Flux 1.1 Pro / Midjourney v7 `--s 400 --raw`

```
Vogue Italia editorial fashion photography. Medium 3/4 shot of {SUBJECT: 
a model in avant-garde couture} against a deep saturated emerald green
backdrop. Steven Meisel theatrical lighting, single hard key from camera-
right, deep shadow side. Face turned in 3/4 profile, intense gaze.
Fashion-forward strong brow and lip. Phase One XF IQ4 150MP, Schneider
80mm f/4. Crisp focus on eye. Film grain layer Kodak Ektachrome E100
emulation, crisp micro-contrast. Slight dress fabric movement. 4:5 print
magazine aspect. Saturated yet refined color, no filter. No retouching on
skin texture.
```

**Negative:**
```
casual, smiling, soft pastel, cute, beauty retouched, glossy, commercial,
shopping, suburban
```

**Why:** Vogue Italia 的密碼 = Meisel + 單硬光 + 深色背景 + 中片幅。

---

### 22. Beauty Campaign Clamshell (10s)

**Use case:** 美妝、香水、保養品
**Target:** Flux Kontext (可修) / Seedream 4.5, 10s

```
Luxury beauty campaign aesthetic. Extreme close-up of {SUBJECT: a model's
flawless skin and lips} filling frame. Clamshell beauty lighting: large
softbox above + reflector card below, shadowless luminous glow. 85mm
macro at f/2.8, extreme shallow DoF on lips. Skin texture with subtle
pore detail, not plastic but refined. Peach, cream, blush palette. Subtle
golden hour warm rim. Perfectly diffused highlights on lips. Slight
product reflection in the model's eye ({PRODUCT: a crystal perfume
bottle} visible as reflection only). 2048×2048, no retouching needed —
lighting does the work.
```

**Negative:**
```
harsh shadows, grunge, gritty, yellow unflattering, oily, dry cracked,
dark moody, orange tan fake, over-filtered
```

**Why:** 美妝廣告 = 無陰影 clamshell + 85mm macro + 自然皮膚細節。

---

## 🌌 VFX 情境 Presets

### 23. Wuxia Bamboo Duel Slow Motion (10s)

**Use case:** 武俠 + VFX + 竹林
**Target:** Kling 2.6 Pro / Seedance Pro, 10s

```
Two wuxia swordmasters mid-duel in a bamboo forest at dawn. Shot on Arri
Alexa with 2.39:1 Panavision anamorphic. {SUBJECT A: 青衣武者} and
{SUBJECT B: 白衣武者} mid-leap in opposing arcs, blades catching first
morning sun as horizontal lens flare streaks. 240fps Phantom slow motion.
Bamboo leaves drift around them in slow motion particles. Visible
volumetric god rays through the bamboo canopy, atmospheric mist. Practical
wind machine effect on robes, silk flowing. Zhang Yimou saturated emerald
green + blood accent color grade. Slight wire-work weightlessness.
Kodak Vision3 250D emulation.
```

**Negative:**
```
modern urban, clean daylight, stationary, fast-cut, cartoon, daylight
bright, saturated rainbow
```

**Why:** 武俠 + 240fps 慢動作 + 竹葉粒子 + god rays = Zhang Yimou 招牌。

---

### 24. Underwater Caustics Dreamscape (10s)

**Use case:** 水下世界、夢幻、游泳池、珊瑚礁
**Target:** Kling / Veo 3.1 (物理), 10s

```
Underwater dreamscape cinematography. {SUBJECT: a woman in a flowing white
dress} floating serenely below the surface, hair suspended in current.
Golden hour sunlight caustics dappling on sandy floor, visible suspended
particles in water. Bubbles rising in slow streams around her. Blue-green
color shift, aerial perspective thickening with depth. Hoyte van Hoytema
underwater housing aesthetic. Wide 24mm lens, slight fisheye. Backlit
from sun above, volumetric light columns penetrating the water. 120fps
slow motion for dress fabric simulation. Kodak Vision3 500T. 2.39:1.
```

**Negative:**
```
dry land, above water, fast action, gritty urban, dark horror, neon
cyberpunk, cartoon anime
```

**Why:** 水下三要素：caustics + 懸浮粒子 + 上方體積光。

---

### 25. Haunted Hospital Corridor (10s)

**Use case:** 恐怖、心理驚悚、工業驚駭
**Target:** Kling (低光強) / Runway, 10s

```
Abandoned hospital corridor at night. Slow forward dolly through the
endless perspective-vanishing hall. Flickering green fluorescent
practicals hanging askew, one out every three. Thin layer of atmospheric
haze drifting. Peeling institutional paint texture, rusted gurneys against
walls. Muted desaturated palette with sickly green fluorescent cast.
Wide 18mm lens creating deep deep perspective, subject (if any) distant
as tiny silhouette. Hiro Murai low-light lowfi unease. Locked tripod,
subtle drift only. Vision3 500T grain. No music cue, dry room tone only.
1.85:1.
```

**Negative:**
```
warm cheerful, daylight, modern clean, people smiling, colorful, bright
fluorescents even, domestic, suburban
```

**Why:** 恐怖 = 閃爍綠螢光 + 深透視 + atmospheric haze。Hiro Murai token 加深不安。

---

### 26. Magical Forest Fireflies (10s)

**Use case:** 奇幻、夢幻、Ghibli 感魔法
**Target:** Kling / Seedream (靜圖), 10s

```
Enchanted forest at twilight. {SUBJECT: a young traveler with a lantern}
walks a moss-covered path lit only by hundreds of floating bioluminescent
particles (fireflies mixed with magical glowing motes) rising upward.
Dense volumetric god rays piercing through giant ancient tree canopy.
Practical small LED accent mushrooms on the forest floor glowing soft
cyan and green. Cool blue-green base with warm lantern-glow accent on
subject. Terrence Malick Lubezki Steadicam drift, natural light feel.
Slight handheld organic movement. Ghibli hand-painted texture on
backgrounds. 2.39:1. Kodak Vision3 500T grain.
```

**Negative:**
```
modern urban, clean minimalist, harsh daylight, tech futuristic, dry
desert, stadium lights
```

**Why:** 魔幻森林的三層光：環境藍綠 + 粒子 + 主角暖光，缺一不可。

---

### 27. Volcanic Eruption Hero (10s)

**Use case:** 自然災害、壯觀、尺度
**Target:** Kling 2.6 Pro / Veo 3.1, 10s

```
Volcanic eruption at dusk, epic scale. {SUBJECT: a solitary silhouette
on a distant ridge} as tiny reference for the monumental lava fountain
erupting behind. Molten orange rock spewing skyward, black smoke plume
reaching stratosphere. Volcanic ash falling foreground, lightning within
the ash cloud (volcanic lightning). Hoyte van Hoytema IMAX 70mm
cinematography. Low-angle hero wide, crane descending. Warm magma orange
vs cold blue twilight sky high-contrast collision. Practical fire glow
on the silhouette's edges. Kodak Vision3 500T heavy grain under low
light. 1.43:1 IMAX or 2.39:1.
```

**Negative:**
```
intimate close-up, domestic interior, daylight calm, urban city, bright
happy, cartoon, stylized anime
```

**Why:** 尺度感 = 巨大自然 + 渺小人類 + Hoytema IMAX 鏡語。

---

### 27a. 純抽象 VFX — 時空裂縫 / 維度突破 (15s, OiiOii-safe) ⭐ v1.4.4

**Use case:** 想做 dramatic VFX 但避開所有版權 / IP / 高風險視覺類別 (機甲/超英/光劍/怪獸 都會被擋)。純抽象幾何 + 粒子 + 光線 = OiiOii copyright filter 完全友善 + 仍極致電影感。

**Target:** OiiOii 自由畫布 + Seedance 2.0 pro, 15s, 16:9 (140 STAR ≈ NT$33)

```
[00:00-00:05] 純黑虛空中央，一道{ENERGY_FORM: 金白色閃電裂痕}緩慢撕開空間，邊緣噴發白熾粒子與青藍電弧。相機極端微距 dolly-in 推進核心，呈現極致細節，粒子在四周漂浮翻轉。Smooth transition to: {EXPANSION: 裂痕劇烈擴張}。

[00:05-00:10] {EXPANSION_PAYOFF: 裂痕爆發，內部湧出另一維度視覺}：{GEOMETRY: 液態水銀構成的莫比烏斯幾何結構}、漂浮的水晶碎片、扭曲折射的光線、緩慢翻轉的{COSMIC: 紫金星雲}。相機高速 orbit 環繞 270 度，揭示結構複雜性。Match cut to: {ACCELERATION: 變形加速}。

[00:10-00:15] 鏡頭穿越進維度核心，揭示{FINAL_FORM: 無限循環的莫比烏斯能量結構}，由純光與粒子構成，緩慢呼吸般脈動。Final beat: 鏡頭緩慢拉遠 (dolly pull-back)，整個結構在黑色虛空中如{ANCHOR: 鑽石}般閃爍，定格畫面。

[視覺風格] AAA 級電影 VFX 製作，IMAX 8K 級畫質，多層次粒子模擬，超寫實光線追蹤，極致景深，電影感色彩分級。
[色彩] 純黑底 + {COLOR_1: 金白高光} + {COLOR_2: 青藍電弧} + {COLOR_3: 深紫星雲} + {COLOR_4: 水銀銀白}。
[音訊] 低頻 sub-bass 環境，{SFX_TEXTURE: 維度撕裂金屬聲}，能量爆發 whoosh，定格水晶共振音。
[Constraints] 平滑鏡頭轉場、ONE camera motion per shot、無變形漂移、無人類角色、無建築、無 IP、無機甲、純抽象視覺。
```

**Negative (內建在 Constraints 行)：**
```
人類角色、建築、IP 角色、機甲、辨識的影視特徵、變形、漂移
```

**Why:**
- 3-shot 結構 + `Smooth transition to` / `Match cut to` / `Final beat` = Seedance 2.0 pro multi-shot 信號
- 「純黑虛空 + 抽象幾何 + 粒子 + 光」= 零 IP 觸發風險
- 4 行 metadata (視覺/色彩/音訊/Constraints) = agent 完整解析所有指令
- 「無 IP、無機甲、純抽象視覺」明寫進 Constraints = 強化 copyright safety
- 「ONE camera motion per shot」防止 Seedance 在一個 shot 內亂切鏡

**Swap points (可換的 placeholder)：**

| Placeholder | 替代範例 |
|---|---|
| `ENERGY_FORM` | 液態水銀球 / 漂浮水晶 / 旋轉星雲 / 能量漩渦 / 光繭 |
| `EXPANSION` | 球體扭曲變形 / 水晶生長 / 星雲爆發 / 流體炸開 |
| `GEOMETRY` | 莫比烏斯 / 克萊因瓶 / 分形幾何 / 蜂巢結構 / 螺旋 DNA |
| `COSMIC` | 紫金星雲 / 銀河旋臂 / 太陽風暴 / 黑洞吸積盤 |
| `FINAL_FORM` | 莫比烏斯能量 / 完美鑽石 / 旋轉銀河 / 神聖幾何 |
| `ANCHOR` | 鑽石 / 水晶 / 黑洞 / 星座 / 光繭 |
| `COLOR_*` | 純黑/金白/青藍/紫/銀白/翡翠/血紅/海藍/橘紅 |
| `SFX_TEXTURE` | 維度撕裂金屬 / 液態流動 / 風暴呼嘯 / 水晶共振 |

**OiiOii-safe 替代主題（同公式換 placeholder 即可）：**

1. **液態金屬星雲幾何**：水銀球 → 克萊因瓶 → 鑽石
2. **時空裂縫**：閃電裂痕 → 莫比烏斯 → 黑洞
3. **水晶宇宙生成**：水晶種子 → 分形生長 → 銀河系
4. **能量花苞綻放**：光繭 → 蜂巢結構 → 太陽
5. **水墨宇宙**：墨滴入水 → 銀河擴散 → 黑洞收縮
6. **光譜萬花筒**：白光分裂 → 萬花筒幾何 → 鑽石折射

**禁忌（OiiOii copyright filter 會擋）：**
- ❌ 巨型機甲 / 鋼鐵巨人 → 撞 Pacific Rim / Transformers / Gundam
- ❌ 超英雄飛行戰鬥 → 撞 Marvel / DC
- ❌ 光劍 / 絕地武士 → 撞 Star Wars
- ❌ 巨型怪獸破壞城市 → 撞 Godzilla / Pacific Rim Kaiju
- ❌ 動漫熱血少年 → 撞 Naruto / DBZ / One Piece
- ❌ 賽博龐克偵探 + trench coat → 撞 Blade Runner

實測 2026-05-21 連續兩次 「巨型機甲」prompt 都觸發攔截（即使第 2 次完全沒寫 IP 名稱），所以**整個視覺類別**都要避開，不只 IP 名稱。

---

### 28. 80s Synthwave Neon Drive (10s)

**Use case:** 復古未來、合成波、Drive / Kavinsky MV 感
**Target:** Kling / Runway, 10s

```
1980s synthwave retro-future aesthetic. Low-angle tracking shot alongside
{SUBJECT: a matte black retro cafe racer motorcycle}, rider silhouette
backlit. Driving through a neon-saturated empty freeway tunnel at night.
Saturated magenta and cyan LED wash. Horizontal anamorphic lens flares
on every light source. Chrome reflections. VHS fuzz and light chromatic
aberration. 2.39:1. Kodak Vision3 500T grain plus subtle video scanline
overlay. Moebius / Tron Legacy inspired color palette. Cool synth-pink
sunset visible at tunnel end. 60fps slight motion blur on environment,
subject stabilized.
```

**Negative:**
```
warm earth tones, natural daylight, rural, vintage 1970s, grunge, natural
film look, documentary realism
```

**Why:** 80s 合成波 = `magenta + cyan + chrome + VHS fuzz + anamorphic flare`。

---

## 📱 9:16 短影音 Presets

### 29. TikTok UGC Authentic (9:16, 10s)

**Use case:** 真實 UGC 感、非商業、有機
**Target:** Kling 9:16 / Seedance, 10s

```
Authentic TikTok UGC aesthetic. Vertical 9:16 handheld iPhone front
camera selfie-style. {SUBJECT: 一位年輕人} talking to camera in natural
room lighting, slight motion blur from handheld. Messy background
(bedroom or kitchen detail visible, lived-in). No filter, no retouching,
subject's real skin texture and casual outfit. 4K/30fps iPhone 15 Pro
look, auto white balance slightly warm. Center framed with head top
third, face eye-contact. Ambient ambient room tone. Natural eyes, no
ring light glow in eyes (important: avoid the "influencer ring" look).
```

**Negative:**
```
polished commercial, studio lighting, professional camera, retouched
flawless, luxury brand aesthetic, perfect composition, graded
cinematic
```

**Why:** UGC 的關鍵是「不像廣告」。要明確寫 `no ring light glow in eyes` 擋掉 AI 的 influencer 預設值。

---

### 30. Food ASMR Overhead (9:16, 10s)

**Use case:** 美食、ASMR、料理直拍
**Target:** Kling I2V (首幀食物靜圖起跑) / Seedance, 9:16, 10s

```
Food ASMR overhead 90° top-down vertical 9:16. Extreme close-up of
{SUBJECT: a sizzling steak being sliced} on dark wooden board. Steam
rising, lit dramatically by side rim light at 45°. Slow 240fps Phantom
motion on knife cutting, juices glistening. Shallow DoF 100mm macro at
f/4. Saturated but natural food colors — deep caramel crust, pink medium
interior. Dark cinematic background, negative space for text overlay top.
Kodak Vision3 500T emulation but clean, minimal grain. SFX (for Veo 3):
knife crunch through crust, sizzle, ambient restaurant kitchen.
```

**Negative:**
```
wide landscape, full table cluttered, horizontal 16:9, cartoonish
saturated, dry not glistening, dusty
```

**Why:** 美食 ASMR = `overhead 90° + side rim light + 240fps + 蒸氣`。

---

## 🏆 產品廣告 / 物理材質 Presets（反瑕疵實戰）

> 搭配 [quality-control.md](../references/quality-control.md)。產品廣告兩大殺手：**主體變形** + **物理動得很假**。這兩個 preset 直接內建鎖形狀 + 分材質物理 token。

### 31. 產品 Hero i2v（鎖形狀，廣告最可靠路徑）⭐

**Use case:** 任何實體產品（瓶/罐/鞋/錶/3C/包）要動畫化但**絕不能變形**。
**Target:** 先用 Seedream 5.0 / Nano Pro / Ideogram（含字）定死 hero 圖 → i2v（OiiOii Seedance 2.0 pro / Kling 3.0 Start Frame）

**Step 1 — Hero 圖（鎖形狀的關鍵，t2v 產品永遠輸這步）：**
```
奢華精品產品攝影 hero shot：{PRODUCT 具體材質+輪廓，如「磨砂玻璃精華液瓶，金屬滴管蓋」}，
形狀完整剛硬無變形，立於 {SURFACE 如濕潤黑岩板} 中央。單一柔光左上斜射，淺景深 f/1.4，
背景純黑漸層柔焦。Phase One IQ4 中畫幅，物理級光線追蹤，AAA 廣告攝影，{COLOR 暖金高光}。
無文字 logo、無品牌名、無人物。8K 超精細。
```

**Step 2 — i2v 動畫化（黃金公式，只寫運鏡不重述視覺）：**
```
根據圖片中的物體、畫面、風格來生成影片，
[00:00-00:05] 極慢 ultra-macro dolly-in 推近產品，產品形狀完全不變，背景 caustics 光紋微動
[00:05-00:10] 緩慢 180° orbit 環繞，pull-back 定格 hero。Final beat: 金色 lens flare 綻放
[Constraints] 產品 rigid 形狀不變、ONE camera motion per shot、無變形漂移、無文字、平滑轉場
```

**Why 鎖形狀有效：** 圖把「產品類型 + 質感 + 構圖 + 打光」鎖得很牢，i2v 主要負責「動」→ 變形機率比 t2v 驟降。**t2v 產品三次還變形 → 必切此流程。**
**⚠️ 實測校準（2026-06-05）：** Seedance i2v 不是像素級凍結，瓶身比例仍可能中度漂移。要更硬的凍結 → hero 圖背景極簡 + 運鏡越小越好 + 或改 **Kling 3.0 Start Frame + Motion Brush**（產品本體不刷 = 幾乎不動）。

**Swap：** `{PRODUCT}` / `{SURFACE}` / `{COLOR}` / orbit 角度（產品鏡頭最多 180°，別 360°）

---

### 32. 物理材質速查 Inject（水/玻璃/金屬/布/煙/食物）

**Use case:** 結果「動得很假、像果凍」。按材質抽對應 token 補進 prompt。
**Target:** 影片通用；複雜流體優先 Runway Gen-4.5 / Veo 3.1（弱物理模型硬做必爛）

| 材質 | 補進 prompt 的 token | 配套 |
|---|---|---|
| 水/液體 | `crown splash, surface tension, caustics, ray-traced refraction` | + `extreme slow motion` |
| 玻璃 | `subsurface scattering, refraction, specular highlights` | 簡化背景 + 固定光源 |
| 金屬 | `brushed metal, anisotropic reflection, metallic specular` | + `stable reflections` 慢運鏡 |
| 布料/絲 | `flowing fabric, natural drape, cloth inertia` | + `gentle motion, natural weight` |
| 煙/霧 | `volumetric, turbulent flow, natural dissipation, god rays` | `wispy, fine particulate` |
| 食物 | `fresh, glistening, steam rising, subsurface scattering` | + `soft key light` 反塑膠感 |

**通用物理強化句（任何材質可加）：**
```
physically accurate, realistic weight and momentum, natural gravity,
extreme slow motion high-speed camera capture
```

**Why：** 慢動作讓物理更可信（模型有更多幀描繪細節）；「physically accurate」「realistic weight」是新模型理解的物理先驗 token。

---

## 如何使用這些 Preset

### Quick Start
1. **找最近的 preset** — 用 Ctrl+F 搜 "Nike"、"Wes Anderson"、"水下" 等關鍵字
2. **換占位符** — `{SUBJECT}` / `{PRODUCT}` / `{SCENE}` 填具體內容
3. **貼到目標平台** — 每個 preset 已標 `Target` 推薦平台
4. **加 negative** — 把對應的 negative 貼到平台的 negative 欄 (若有)

### 進階
- **混搭**：把 preset A 的 `Style & Lighting` + preset B 的 `Camera & Subject` 拼起來
- **調 intensity**：覺得太過 → 拿掉 1-2 個 token (例如拿掉 DP 名);覺得太弱 → 從 [cinematic-direction.md](../references/cinematic-direction.md) 補 1-2 個
- **修改版**：成功的組合存成自己的 preset，建立個人語彙庫

### 針對平台微調
- **Midjourney v7**：把 preset 主文放前面，參數 `--ar 16:9 --s 200 --raw --v 7` 放尾
- **Flux 1.1 Pro**：preset 已是自然語言句子，直接用，去掉 `shot on XXX` 可讓輸出更 neutral
- **Kling I2V**：把 preset 的「主體描述」拿掉，只留動作 + 風格 + 運鏡 (見 [kling.md](../references/kling.md) I2V 黃金法則)
- **Sora 2**：把 10s preset 拆成 3 beats，每 beat 一段
- **Seedance Pro**：用 `Cut to:` 串多個 preset 成多鏡
- **Runway Aleph**：改成動詞開頭 (`restyle...` / `add...`) 配既有影片

---

## 🎬 Seedance 2.0 Community Featured (2026-05-18)

**Source:** [YouMind-OpenLab/awesome-seedance-2-prompts](https://github.com/YouMind-OpenLab/awesome-seedance-2-prompts) (CC BY 4.0, attribution required)

6 hand-picked prompts from community + 4 best-pattern examples. Each has demonstrated outstanding Seedance 2.0 output.

> ⚠️ All prompts below are reproduced under CC BY 4.0. Original authors credited per-preset.

### S1. 日系青春純愛 15s 多鏡（中文）— Featured #1

**Use case:** Romance、純愛、表情張力、教室、暗戀情境
**Target:** Seedance 2.0 pro，15s，16:9，720p+
**Author:** AIGC｜阳家豪 ([@JiahaoYang_art](https://x.com/JiahaoYang_art))

```
15 秒電影感日系純愛曖昧短片，極致寫實質感，午後空教室裡溫暖金色陽光透過百葉窗灑在並排的書桌上，細小塵埃在光束中緩慢漂浮，老舊木質書桌，極其自然細微的動作、呼吸與眼神張力，人物全程保持一致的臉孔、衣著與髮型，不變形、不漂移、不出現偽影，真實微妙的胸口起伏與呼吸同步，淺景深，奶油散景，溫暖底片顆粒，8K 銳利，日系青春克制悸動窒息氛圍。
0-4 秒：從桌面中景極緩推鏡至兩人並肩側臉特寫。穿夏季校服的純真少女低頭專注寫筆記，烏黑長髮與耳邊碎髮被微風輕輕揚起，長睫毛在臉上投下淡淡陰影，肌膚自然粉嫩，因專注而嘴角不自覺微揚，呼吸輕勻。
4-9 秒：切到男孩特寫。校服衣領微鬆，他撐肘趴桌偷偷側頭凝視她，眼中盛滿溫柔克制的愛意與溫暖,瞳孔輕微擴大,喉結輕滾。突然察覺她筆停下,慌張快速轉頭裝作看自己的筆記,耳垂迅速微紅,指尖握筆微顫,偶爾從瀏海下偷瞄她,呼吸略亂,雙唇緊抿努力鎮定。
9-15 秒：兩人面孔同框極特寫,慢鏡頭眼神突然相對:少女緩緩抬頭,先是怔忡驚訝,隨即羞紅快速垂眸 0.3 秒,輕咬下唇,雙頰與耳垂瞬間綻放櫻花粉,濕潤睫毛怯怯抬眼再度迎上他目光,同時輕柔害羞地低聲呢喃:"...你在看什麼?";男孩瞳孔放大徹底僵住 0.4 秒,慌張低聲結巴回應:"沒...沒什麼...". 少女更小聲咬唇又偷看,繼續低聲:"...騙人。". 男孩頓住,輕嘆低語:"...就是想看你。",嘴角緩慢揚起羞澀溫柔歪笑,眼角細紋出現,呼吸明顯加深。兩張臉之間曖昧張力被無形電流拉扯,分享彼此呼吸溫度,背景完全融化成奶油夢幻光斑、溫暖光暈與細密空氣微粒層次。
唇形同步自然精準,情緒微顫與呼吸同步,對白為低能量氣音,語氣羞澀,自然短停頓 200-400 毫秒,口型僅在發聲時微動,不誇張不機械,呈現完美自然唇音與情緒真實感。
整體音效:遠處夏蟬隱約鳴叫,筆尖觸紙的柔軟刮擦聲,心跳低頻幾乎不可聞地脈動,最終淡入極輕柔氣音鋼琴。對白完全自然融入場景的氣音呢喃,女孩聲音柔軟害羞,男孩從慌張結巴轉為溫柔。
角色身份始終保持一致,真實細微頭部傾斜、眼神移動與呼吸同步,無文字、浮水印、字幕,純日系青春暗戀心動懸念。
```

**Why:** Bracketed-style header + 3-shot 5s blocks + 對白雙語 + 詳細 lip-sync directive + Sound design + Constraints tail。**這個 prompt 是 Seedance 2.0 「日系純愛」最完整 reference。**

**Swap points:**
- 場景：教室 → 圖書館 / 雨後巷弄 / 校園櫻花樹下
- 互動：偷看 → 同撐傘 / 一起整理書 / 籃球場邊
- 對白：保留 lip-sync directive 但改台詞

---

### S2. Hollywood 高級訂製奇幻 15s（中文）— Featured #2

**Use case:** 高奢時尚、超寫實奇幻、流體渲染、視覺特效
**Target:** Seedance 2.0 pro，15s，16:9
**Author:** John ([@johnAGI168](https://x.com/johnAGI168))

```
[風格] 好萊塢高級訂製奇幻巨作，8K 超高清，超寫實，時尚編輯風格，Unreal Engine 5 流體渲染，視覺錯覺。[時長] 15 秒。[場景] 無邊無際、真實的烏尤尼鹽沼（天空之鏡）鹽灘。天空佈滿壓抑的烏雲，地面完美地反射一切，如同鏡子一般，整體畫面呈現極簡的冷色調。

[00:00-00:05] 鏡頭 1：高級訂製入場與瓷肌。攝影機位置：極低角度仰拍，超長焦鏡頭推近。動作：一位具有高度辨識度、時尚臉孔的亞洲女模特兒，在水面上酷酷地行走。效果：她穿的不是布料，而是一件由流動的、真實的液態青花瓷製成的長裙。隨著她走動，裙擺發出真實陶瓷般清脆的碰撞聲，表面是流動的釉光，傳統青花紋路在白瓷質感的裙擺上彷彿活著般移動。

[00:05-00:10] 鏡頭 2：物理粉碎與水墨墜落。攝影機位置：臉部極特寫，焦點快速拉遠。動作：模特兒突然停下，冷冷地凝視鏡頭，清脆地打了個響指。效果：手指打響的瞬間，她的青花瓷長裙並未掉落，而是瞬間炸開成數千隻極致寫實的水墨燕子。這些燕子帶著真實的水滴與墨痕，拖著黑色流體殘影，瘋狂地繞著她旋轉。

[00:10-00:15] 鏡頭 3：維度溶解與深淵倒影。攝影機位置：高空俯視鏡頭，攝影機快速旋轉下降。動作：水墨燕群俯衝進入模特兒腳下的鏡面湖水。效果：原本堅實的鹽湖表面張力瞬間消失。整個極致寫實的世界開始像濃墨滴入清水般劇烈滲血、溶解。真實烏雲與模特兒身形整個轉化成一個極為宏偉的 3D 流體水墨漩渦，將鏡頭徹底吞沒進黑白交織的深淵。
```

**Why:** 完整 bracketed header + 3-shot 5s blocks + format anchor (`Unreal Engine 5 流體渲染`) + 攝影機位置 + 動作 + 效果三層分解。**水墨/中國美學 + 西方高奢 fusion 的範本。**

**Swap points:**
- 場景：鹽沼 → 沙漠 / 海面 / 雪原
- 材質變形：青花瓷 → 水晶 / 鎏金 / 流動火焰
- 終極變身：水墨燕子 → 玻璃蝴蝶 / 光點 / 花瓣風暴

---

### S3. 現代田園美學療癒短片 15s（中文）— Featured #3

**Use case:** 療癒、生活感、ASMR、Slow living、商業形象短片
**Target:** Seedance 2.0 pro，15s，16:9
**Author:** John ([@johnAGI168](https://x.com/johnAGI168))

```
[風格]
現代田園美學，電影級商業質感，Sony A7S3/cinema camera 拍攝，4K/8K 超清，極致 Macro，自然透光，治癒系 ASMR，無古裝劇感。

[場景]
精心打理的現代農舍開放式廚房，背景是茂密的蔬菜花園，明亮陽光。

[人物]
現代田園創作者，烏黑長髮用木質髮簪隨意盤起，穿深藍色舒適亞麻服飾，乾淨妝容，專注平和的眼神。

[分鏡細節]
[00:00-00:05] 鏡頭 1：晨間採摘（鮮活感）
畫面：高清特寫。晨光以側逆光打在植物上。
動作：創作者赤裸的雙手（修長乾淨的手指）從藤上摘下一顆閃著露珠的鮮紅番茄。
細節：焦點極銳利，清晰呈現番茄表皮的細毛與水珠滑落軌跡。背景是高品質虛化的綠色。

[00:05-00:10] 鏡頭 2：極致工藝（手藝感）
畫面：室內灶台區域，生活感卻一塵不染。
動作：創作者正在切菜，動作熟練精準（非表演性質）。
細節：Macro 鏡頭捕捉刀刃切入食材的瞬間，汁水飛濺。隨後切換到土灶中跳動的橘色火焰，光影溫暖真實。

[00:10-00:15] 鏡頭 3：寧靜時光（瞬間感）
畫面：全身鏡頭/中景。
動作：精緻的家常菜被擺在院子的木質長桌上。創作者靜靜坐下，輕輕整理一縷散髮，拿起一口食物。
氛圍：蒸氣在逆光中緩緩升起，場景安靜得彷彿能聽見風聲，呈現現代人嚮往的極致鬆弛感。
```

**Why:** Bracketed header + Character card + 3-shot 結構 + Visuals/動作/細節三層解析 + format anchor (`Sony A7S3`)。療癒系商業短片 gold standard。

**Swap points:**
- 場景：田園 → 海邊 / 山中 / 都市陽台
- 動作：採番茄 → 揉麵 / 沖咖啡 / 插花
- 風格：田園 → 北歐極簡 / 日式侘寂 / 法式鄉村

---

### S4. 真人版動漫呼吸法決鬥 15s（中文）— Featured #4

**Use case:** 動漫真人化、武打/呼吸法、特效對決、爆炸光效
**Target:** Seedance 2.0 pro，15s，16:9
**Author:** John ([@johnAGI168](https://x.com/johnAGI168))

```
真人版動漫改編・呼吸法巔峰決戰（15 秒・超燃特效版）
【核心焦點】：水之呼吸（藍色水龍） VS 雷之呼吸（金色閃電），真人極速對決。

【風格】：好萊塢真人版動漫改編電影質感，黑暗武士風，4K 超清，極速快剪，爆炸性粒子光效，不血腥。
【時長】：15 秒
【場景】：月光下的迷霧森林，泥濘地面，落葉。

[00:00-00:05] 鏡頭 1：水之韻律前奏・起手式（蓄勢感）
畫面：身穿綠黑棋盤紋羽織（夾克）的年輕武士，月光下重心壓低，雙手握劍。
動作：他深吸一口氣，周圍空氣瞬間凝結。拔劍出鞘的同時，由高壓水流凝聚而成的巨大藍色水龍憑空出現，在他身體與劍身周圍快速旋轉，發出流水的咆哮。
特效細節：水流具有真實的飛濺感，照亮黑暗森林。

[00:05-00:10] 鏡頭 2：雷之閃電・突進（極速感）
畫面：對手是身穿黃色三角紋羽織的金髮劍客，極低姿勢蹲下，採居合道（拔刀術）的架勢。
動作：地面突然爆裂，他瞬間化為耀眼金色閃電殘影，以肉眼無法捕捉的速度在森林中以「Z 字形」折射衝刺。
特效細節：他經過的地方留下金色電弧與焦黑落葉。

[00:10-00:15] 鏡頭 3：水雷碰撞・終焉之音（必殺技交鋒）
畫面：極速碰撞。年輕武士揮動巨大藍色水龍迎擊，化為閃電的金髮劍客正面撞上。
動作：兩把刀在畫面中央劇烈碰撞。
特效奇觀：藍色水龍與金色閃電瞬間爆炸，形成巨大水雷能量風暴向外擴散。周圍大樹被能量波震斷成兩截，泥土與光遮蔽鏡頭。場景在極度炫目的藍、黃、白光中結束。
```

**Why:** 動漫真人化 prompt 的範本 — 用了 `【核心焦點】` 標籤 + 3-shot blocks + 每 shot 分「畫面/動作/特效細節」。可以套到任何雙人對決場景。

**Swap points:**
- 對決元素：水 vs 雷 → 火 vs 冰 / 風 vs 土 / 光 vs 影
- 角色服飾：羽織 → 連帽斗篷 / 機甲 / 現代武術服
- 場景：森林 → 沙漠 / 都市天台 / 太空站

---

### S5. 80 歲老奶奶 Rap MV 15s（中文）— Featured #5

**Use case:** Music Video、街頭嘻哈、反差人物、Trap beat、quoted dialogue + lip-sync
**Target:** Seedance 2.0 pro，15s，16:9
**Author:** 松果先森 ([@songguoxiansen](https://x.com/songguoxiansen))

```
16:9 橫屏，街頭 rap MV 風格，霓虹紫藍冷色調，爆炸性的冷酷狂野氛圍。

0-3 秒：中景推鏡，城市街道夜景閃爍霓虹燈，一位 80 歲銀髮老奶奶站在塗鴉牆前，短銀白髮俐落後梳，方臉輪廓鮮明，劍眉斜飛入鬢，眼神銳利如電，眼角皺紋如時光勳章，嘴角自信笑意，身穿黑色皮夾克外搭白色印花 T 恤（胸前黑色大字「YOLO」）+ 黑色工裝褲 + 白色高筒運動鞋，脖子掛粗金鏈，手腕銀手鐲，雙手舉麥克風，BGM 強勁鼓點響起，老奶奶眼神一凜，雙唇張開開始 Rap。

3-7 秒：中景+特寫切換，老奶奶開始 rap，節奏感極強，銀髮隨點頭動作飛揚，一手持麥，另一手隨節奏比手勢——食指指鏡頭、手掌上下切節拍、做 hip-hop 手勢，動作流暢瀟灑，眼神銳利直視鏡頭，皺紋隨表情生動跳動，雙唇開合迅速吐出歌詞：

[Rap 歌詞]
"八十歲的腿腳，能跳得比你還溜！
銀髮飄揚，這是我的驕傲！
別說我老，我的 Flow 比你還潮，
你玩 rap 時，我在聽 disco！"

（語速快、節奏強、態度凶）

快剪：面部特寫、手部動作、全身搖擺、側面剪影，與 BGM 節拍同步。

7-11 秒：舞蹈段落，鏡頭拉遠展示全身，老奶奶開始跳舞——先是經典 hip-hop bounce，接著俐落街舞 freeze，緊接一個從肩膀傳到腳尖的 body wave，再來快速 footwork 練步，動作乾淨利落，銀髮在霓虹下飛揚，皮夾克在空中翻飛，邊跳邊繼續 Rap：

[Rap 歌詞]
"手腳麻利速度不慢，我的歌詞刻進時間！
你們玩手機，我玩節拍，八十年人生寫進這段！"

（節奏更快、語調更狠）

低角度仰拍 + 360 度環繞鏡頭，捕捉老奶奶酷帥的舞步。

11-15 秒：高潮收尾，老奶奶酷帥轉身，銀髮在空中劃出弧線，正對鏡頭做一個食指比「噓」的手勢，然後雙唇貼近麥克風，低聲磁性地唱出最後一句：

[現實歌詞]
"時光從未敗給美人，我只是換了種方式體驗青春..."

（節奏放慢、情感深沉、餘韻不絕）

鏡頭緩慢推近老奶奶眼眸特寫，眼角皺紋全是故事，眼神依然銳利卻多了一絲溫柔，BGM 在高潮處戛然而止，畫面定格在老奶奶酷帥又略帶溫柔的笑容上，暗角 + 霓虹紫光暈。

整體音樂：Trap 電子節奏 + 重 808 鼓 + 街頭嘻哈 sample
整體氛圍：反差感（年齡 vs 態度）+ 街頭嘻哈文化 + 復古酷感
```

**Why:** Music video 的範本 — 4-shot 結構（intro/verse/chorus/outro）+ [Rap 歌詞] / [現實歌詞] tag + lip-sync 對白 + 整體音樂層 + 360 度環繞鏡頭。**反差人設 + 雙語歌詞 + lip-sync** 三位一體。

**Swap points:**
- 角色：80 歲奶奶 → 5 歲小孩 / 修女 / 商務人士
- 音樂：Trap → Lo-fi / K-pop / 古風電音
- 服飾：嘻哈造型 → 古風漢服 / 童裝 / 西裝

---

### S6. 電影感賽車街頭夜飆 15s（英文）— Featured #6

**Use case:** 賽車、極速、Fast & Furious 風、霓虹都市、NOS 爆炸瞬間
**Target:** Seedance 2.0 pro，15s，16:9
**Author:** Pierrick Chevallier | IA ([@CharaspowerAI](https://x.com/CharaspowerAI))

```
cinematic street racing sequence at night, a focused driver inside a high-performance car grips the steering wheel, intense eye focus, city lights reflecting on windshield, tension building before sudden acceleration

camera: rapid multi-angle system with seamless transitions, interior close-up → over-the-shoulder → exterior tracking → low ground shots, ultra dynamic camera movement, whip pans + speed ramp transitions + motion blur masking cuts, continuous flow illusion

(0-2s) interior close-up on driver, hand tightens on gear shift, subtle breathing, dashboard lights glowing
(2-4s) over-the-shoulder shot, road ahead stretching into neon-lit city, engine vibration building
(4-6s) extreme close-up on finger pressing NOS button, instant ignition reaction
(6-8s) explosive acceleration, camera snaps to exterior side tracking shot, car launches forward with violent speed surge
(8-10s) ultra low ground shot near asphalt, wheels spinning at extreme velocity, environment streaking past
(10-12s) high-speed chase through tight streets, sharp turns, camera whip pans between angles, reflections and light trails enhancing speed

Dense urban night environment, wet asphalt reflecting neon lights, tunnel passages, street lights streaking, high-speed city atmosphere
Ultra realistic, fast and furious inspired energy, photorealistic lighting, intense motion blur, high contrast neon reflections, cinematic depth of field, extreme sense of speed, fluid transitions, no distortion, no stretching
```

**Why:** 英文版範本 — 比中文 prompt 短（~200 字），用 `(0-2s)` 簡潔時間區塊。`camera:` 分段集中描述。`Fast and Furious inspired` 是 brand-style format anchor。**Pure-English Seedance 範本。**

**Swap points:**
- 場景：街道 → 高速公路 / 海邊公路 / 山路
- 車輛：tuner car → 摩托車 / 卡丁車 / 重機
- 風格：Fast and Furious → Mad Max / Initial D / Cyberpunk 2077

---

### S7. 寫實詠春 Wing Chun 15s（英文，焦段示範）

**Use case:** 武術、寫實風格、specific 焦段 token、動作運鏡
**Target:** Seedance 2.0 pro，15s，16:9
**Author:** Jahan Zaib ([@jzaib4269](https://x.com/jzaib4269))

```
REALISTIC CINEMATIC 15s PROMPT — Japanese Wing Chun Martial Artist
Ultra-realistic cinematic martial arts sequence.
A 35-year-old Japanese martial artist trains intensely with a traditional wooden Wing Chun dummy inside a Japanese outdoor courtyard.
Lean athletic build, sharp focused eyes, defined jawline, short slightly messy black hair, subtle facial stubble, calm disciplined expression, realistic skin texture with sweat and natural imperfections. Wearing a fitted black training shirt and loose dark martial arts pants.
Environment: traditional Japanese courtyard with textured stone pavement, bamboo plants, wooden architecture, paper sliding doors, atmospheric dust particles, soft daylight, cinematic shadows, subtle wind movement, shallow depth of field, grounded realism.
Style: high-end live-action realism inspired by modern Japanese and Hong Kong martial arts cinema. Physically accurate movement, realistic muscle tension, cinematic contrast, subtle film grain, immersive atmosphere, dynamic camera movement, authentic choreography. No anime, no CGI look, no digital painting, no commercial aesthetic.
Sequence progression:
— Hard-cut opening, close 35mm follow shot. The martial artist is already moving rapidly through continuous Wing Chun trapping hands, chain punches, compact strikes, and quick front kicks against the wooden dummy. Fast but controlled footwork across stone pavement. Realistic sweat and fabric movement.
— Match-cut to 50mm lateral tracking shot. Smooth transitions between straight punches, slapping hands, elbow strikes, turning techniques, and compact leg attacks. Strong wooden recoil and realistic impact feedback.
— Close-up orbit shot on focused eyes and breathing. Hands flow continuously between offense and defense. Sunlight catches sweat on the face while strikes create subtle vibration through the wooden dummy.
— Full-body 28mm push-in shot. Efficient footwork, chain punches, elbows, turning movements, coordinated kicks. Bamboo leaves rustle softly while dust reacts naturally to movement.
— Extreme close-up handheld realism. Rapid arm transitions between blocks and strikes. One powerful straight punch lands heavily on the wooden dummy with realistic muscle tension and sleeve movement. Dust shakes loose from impact.
— Smooth mirror reflection transition back to frontal medium shot. Continuous flowing Wing Chun combinations with calm precision. Hair and clothing react naturally during movement.
— Final cinematic shot, slow push-in then static hold. The wooden dummy sways slightly after the final strike. Dust settles through sunlight. Bamboo movement slowly stops. The martial artist raises his eyes toward camera with calm confidence, holding a traditional Wing Chun stance in silence.
Sound design: fabric friction, realistic wooden impacts, air swishes, breathing, stone footwork, subtle ambient wind, bamboo rustling.
15 seconds, cinematic pacing, grounded realism, highly detailed
```

**Why:** **焦段 token 示範**（`close 35mm follow shot`, `50mm lateral tracking`, `28mm push-in shot`, `Close-up orbit shot`）— 證明 Seedance 2.0 吃焦段。同時 `Sound design:` 一行整理音訊。`No anime, no CGI look, no digital painting` 排除清單。

**Swap points:**
- 武術：Wing Chun → 太極 / 巴西柔術 / 拳擊 / 劍道
- 場景：日式庭院 → 香港天台 / 紐約地下室 / 沙漠
- 鏡頭：35mm follow → 85mm portrait / 24mm wide

---

### S8. NBA 賽事轉播 + 觀眾席美女（英文）

**Use case:** Sports broadcast、live event、引用 commentator dialogue
**Target:** Seedance 2.0 pro，15s，16:9
**Author:** UxUi Tega ([@Tegadesigns](https://x.com/Tegadesigns))

```
Cinematic live NBA broadcast style video, ESPN-style 16:9, ultra realistic courtside camera shot during Knicks vs 76ers Eastern Conference Semi-Finals Game 3, packed arena, live crowd atmosphere, broadcast color grading, subtle motion blur, slight TV compression artifacts, realistic sports commentary in background.

A beautiful blonde woman with loose wavy blonde hair, striking blue eyes, soft glamorous makeup, elegant white fitted top, sitting courtside naturally watching the game. She looks effortlessly beautiful, calm and classy, unaware the camera is focused on her.

The camera slowly cuts to her from the live game broadcast, subtle natural movements, soft breathing, blinking naturally, slight smile forming. Slow cinematic zoom toward her face, detailed zoom on her beautiful blue eyes. The man sitting beside her notices her beauty and keeps admiring her, looking at her with subtle fascination.

She gently turns her head, gives a soft elegant smile, then lightly adjusts her hair with a stylish feminine hair flick, smooth and natural, followed by a confident subtle smile. The crowd remains realistic and active in the background, players moving on court, fans cheering.

Live NBA commentary continues:
"Brunson pulls up from deep… and he knocks it down! The Knicks are absolutely on fire tonight. You can feel the energy inside the arena—New York leads the series 3-1, and they're looking to close this out in style here in Game 3."

Extremely realistic, luxury aesthetic, natural beauty, viral social media quality, premium cinematic sports broadcast look.
```

**Why:** Sports broadcast 範本 — `ESPN-style 16:9` format anchor + `broadcast color grading, subtle motion blur, slight TV compression artifacts` 三件套 + quoted commentary（會生成口播）。

**Swap points:**
- 賽事：NBA → FIFA / IPL cricket / NFL / F1
- 主角：courtside 美女 → 球星家屬 / 評論員 / 教練

---

### S9. 90 年代動漫魔王吃拉麵 15s（英文，dialogue + subtitle）

**Use case:** 風格化 anime、quoted dialogue 雙語 subtitle、Slice of life
**Target:** Seedance 2.0 pro，15s，9:16 vertical
**Author:** PromptlyAI ([@PromptlyAI_YT](https://x.com/PromptlyAI_YT))

```
Create a 15-second anime-style video clip in 9:16 vertical format.

Concept: The Demon Lord's Day Off
A terrifying but exhausted Demon Lord takes a break from world domination to quietly eat ramen in a small Japanese ramen shop. Tone: cinematic, slightly funny, and emotionally grounded.

Style:
90s anime film style, fantasy slice-of-life, subtle gritty realism, moody lighting, slightly desaturated colors, warm ramen-shop highlights, expressive character acting, polished hand-drawn anime look, soft film grain.

Character:
Male Demon Lord with black horns, long dark hair, red eyes, dark armor, and a cape. He looks powerful but tired, intimidating yet relatable.

Setting:
A cozy ramen shop at night in Japan. Warm lantern light, wooden counter, rising steam, soft reflections, quiet customers, and a subtle rainy-night atmosphere.

Audio:
Natural Japanese dialogue, English subtitles only, soft ambient ramen shop sounds, and gentle emotional background music.

Shots / Timing:

[00–05s]
Medium-wide shot. The Demon Lord enters or sits at the counter, exhausted. Steam drifts through the warm shop.
Japanese: 「今日は世界を滅ぼさない。」
Subtitle: "Today, I'm not destroying the world."

[05–10s]
Medium shot. A steaming bowl of ramen is placed in front of him. He visibly relaxes.
Japanese: 「今日は休みだ。ラーメンを食べる。」
Subtitle: "It's my day off. I'm eating ramen."

[10–15s]
Close-up. He takes a bite of ramen and finally looks at peace. End on a calm, composed shot with subtle humor.
Japanese: 「世界征服は、また明日だ。」
Subtitle: "Taking over the world can wait until tomorrow."

Camera / Constraints:
Use cinematic anime framing with medium-wide, medium, and close-up shots. Smooth motion, subtle push-ins, and strong atmosphere. Keep it simple, readable, and emotionally clear. No extra on-screen text besides English subtitles.
```

**Why:** **雙語對白範本** — 日文原文 + 英文 subtitle，Seedance 2.0 會 lip-sync 日文 + render 英文字幕。9:16 vertical 適合 TikTok / Reels。

**Swap points:**
- 角色：魔王 → 機器人 / 古代仙人 / 外星人
- 場景：拉麵店 → 居酒屋 / 便利店 / 咖啡廳
- 風格：90s anime → 京都 anim / Ghibli / 新海誠

---

### S10. 古代奇幻戰爭 IMAX 9:16（英文）

**Use case:** Epic battle、IMAX 質感、9:16 vertical、Format anchor 重度使用
**Target:** Seedance 2.0 pro，15s，9:16
**Author:** Elara Noor ([@ElaraNoorAl8](https://x.com/ElaraNoorAl8))

```
Duration: 15 Seconds
Style: Ancient Fantasy War • Dark Mythology • Cinematic Survival Epic
Format: Vertical 9:16 • UHD 8K • IMAX Fantasy Camera • Dolby Atmos • Hyper Realism

A hyper-realistic cinematic fantasy battle sequence inside a gigantic ancient temple hidden deep within a volcanic mountain during a violent eclipse storm. Lava rivers glow beneath cracked stone bridges. Massive chains hang from the ceiling. Ash and embers float through the air. Thunder shakes the mountain while giant fire torches flicker against black obsidian walls. Cinematic smoke, glowing particles, realistic destruction physics, intense motion blur, handheld IMAX camera movement, dramatic shadows, and ultra-detailed fantasy VFX create an epic dark mythology atmosphere.

MAIN CHARACTER:
A mysterious female warrior wearing black royal battle armor infused with glowing ancient runes, long flowing cape, emotionless warrior queen energy, master of sword combat and supernatural fire abilities.

0–2 SEC:
Camera flies through massive volcanic temple. Lava erupts below ancient stone bridges. Heavy footsteps echo. Close-up of glowing armored boots walking slowly across cracked obsidian floor.

2–4 SEC:
Battle erupts instantly. She unsheathes glowing ancient sword. Massive shockwave blasts enemies backward. IMAX whip-pan follows her spinning slash cutting through armored warriors.

4–6 SEC:
Close combat inside collapsing temple corridor. She performs brutal sword combinations mixed with supernatural fire magic.

6–8 SEC:
A giant armored executioner attacks with massive molten hammer. She slides beneath attack in ultra slow motion.

8–10 SEC:
Temple begins collapsing into lava abyss. Giant chains snap from ceiling. Fire storms swirl through chamber.

10–12 SEC:
Final enemy emerges — gigantic shadow king covered in molten armor. Epic duel sequence begins.

12–14 SEC:
The volcanic mountain explodes violently. She escapes by leaping from collapsing temple onto giant mythical black dragon emerging through smoke and fire.

14–15 SEC:
Dragon flies above erupting volcano while entire mountain collapses into flames below.
```

**Why:** **格式 anchor stacking** 範本 — `Format: Vertical 9:16 • UHD 8K • IMAX Fantasy Camera • Dolby Atmos` 一行塞 4 個 anchor，Seedance 2.0 都吃。7-shot 結構 ~2s each，極快剪。

**Swap points:**
- 場景：火山神殿 → 冰原城堡 / 海底遺跡 / 浮空島
- 主角：女戰士 → 男劍客 / 機甲駕駛 / 龍騎士
- 對手：影子王 → 古龍 / 機械神 / 不死法師

---

## Seedance 2.0 速查總結

| 場景類型 | 用哪個 preset | 語言 |
|---|---|---|
| 純愛 / Romance | S1 | 中文 |
| 高奢 / Fashion | S2 | 中文 |
| 療癒 / Slow living | S3 | 中文 |
| 武打 / 動漫對決 | S4 | 中文 |
| MV / 音樂 | S5 | 中文（quoted lyrics）|
| 賽車 / 動作 | S6 | 英文 |
| 武術 / 焦段示範 | S7 | 英文 |
| 體育轉播 | S8 | 英文 |
| 雙語 dialogue / Anime | S9 | 英文（quoted 日文）|
| 史詩戰爭 / IMAX | S10 | 英文 |

**選擇規則：** 中國/亞洲文化主題 → 中文；西方/賽車/體育 → 英文。

---

## § Gemini Omni 多輪編輯範本 (Multi-Turn Interactive Editing Presets)

**定位：** Gemini Omni 的招牌能力是「**對話式編輯**」—— 每個 turn 都在前幾 turn 的脈絡下理解指令（第 3 turn 改機位，模型記得第 1-2 turn 的角色/光線/場景）。五個 preset 示範如何利用這點做「基礎層 → 第一次修改 → 第二次修改」的無縫迭代，最後產出廣告級成品。

> **使用方式：** 
> 1. 把「基礎層」prompt 貼進 Gemini Omni（Flow PRO 或 Gemini app）
> 2. 等出第一版影片或截圖
> 3. 依「Edit Round 1」修改指令，Omni 會記得所有前文背景
> 4. 若滿意就可輸出；若要進一步改進，用「Edit Round 2」指令
> 5. 每個 turn 只改**一個變數**（例：改打光、改機位、改材質），別一次全改（無法歸因）

---

### 33. 產品廣告「精華液瓶」3 Turn 編輯（打光 → 角度 → 視角）

**Use case:** 護膚/精品瓶類產品，多輪從棚拍→改打光→改機位，保持瓶身形狀 / 液位 / 配色全程一致
**Target:** Gemini Omni Flash（Flow PRO），每 turn 4-6 秒推薦，16:9

#### 【Base Prompt】
```
Create a 5-second premium skincare product video. The scene shows a crystalline
glass serum bottle with a polished rose-gold dropper cap, standing upright on a
wet black slate surface. The liquid inside is a luminous amber color. Soft,
diffused studio lighting from the left side (45° key light), creating gentle
reflections on the glass and slate. The background fades to pure black. Camera:
static locked-off shot, slightly low angle (15° tilt up). Product takes up center
frame with breathing room. No text, no branding visible. Premium, minimal,
editorial aesthetic. No motion — just static hold on the product.
```

#### 【Edit Round 1 — 打光改進】
```
Keep everything identical (same product, same angle, same framing, same background).
Now change the lighting: replace the left 45° key light with a DRAMATIC single 
overhead spotlight, creating a strong rim light on the left edge of the bottle 
and the dropper cap. The lighting should feel more luxe, more theatrical — like 
a jewelry commercial. The amber liquid inside should glow with the top highlight.
```

**為什麼有效：** Omni 記得「rose-gold cap / amber liquid / black slate」，你只需改「light direction」這一變數。新 lighting 會自動套用在既有的產品構圖上，無需重寫整句。

#### 【Edit Round 2 — 機位改變】
```
Keep the same lighting setup from the previous version (overhead spotlight with 
dramatic rim light). Now change only the camera: perform a very slow, gentle 
dolly-in from the current wide view, zooming in 15% toward the bottle center 
over the full 5 seconds. The product shape and bottle position should remain 
identical, only the camera pushes in. End frame is a medium close-up of the 
dropper cap catching the highlight.
```

**Why 多層編輯有效：** Turn 2 明確寫 `Keep the same lighting setup from the previous version` → Omni 理解「上一版本的打光保留」。你只改運鏡（dolly-in），產品和光源不變。三 turn 下來 = 精華液瓶從「靜態棚拍」→「強烈打光」→「推進特寫」，全程瓶身完整一致。

**Swap points:** `rose-gold cap` 換其他金屬色（銀/鍍金）；`amber liquid` 換 `translucent clear` / `deep blue`；「overhead spotlight」可改 `chiaroscuro side-light` / `three-point Hollywood`。

---

### 34. 角色廣告「模特轉身」3 Turn 編輯（基礎姿勢 → 表情 → 動作細節）

**Use case:** 時尚品牌人像，多輪控制角色姿勢/表情/肢體動作
**Target:** Gemini Omni Flash，6 秒推薦，16:9

#### 【Base Prompt】
```
Create a 6-second fashion commercial. A young woman in her 20s, wearing an 
elegant ivory silk slip dress, stands facing the camera in a high-end minimalist 
apartment. Warm golden afternoon light spills from a large window on the left, 
creating soft shadows. She has a neutral, composed expression. The camera is 
static, medium shot framed at eye level. Her posture is upright, arms relaxed at 
her sides. Soft background bokeh (apartment interior blurred). Cinematic, 
editorial, luxury fashion aesthetic. No motion of the character yet.
```

#### 【Edit Round 1 — 表情改進】
```
Keep the entire scene identical (same woman, same dress, same lighting, same 
apartment, same camera angle). Change ONLY her expression and eye direction: 
she should now turn her gaze slightly to camera-left (her left), as if looking 
at something just off-frame. A subtle smile, confident and contemplative. Her 
face should show more presence and emotion while the body remains in the same 
pose. The moment should feel more intimate and intentional.
```

#### 【Edit Round 2 — 肢體動作】
```
Keep the expression and gaze from the previous version (looking off-frame with 
subtle smile). Now add subtle body movement: her right hand should slowly rise to 
rest gently on her collarbone, a graceful gesture over 3 seconds. The dress fabric 
should catch light as her arm moves. Her body remains otherwise still. The 
movement should feel natural and unhurried — like a fashion editorial moment. No 
abrupt transitions.
```

**Why 有效：** Turn 1 鎖「表情」。Turn 2 在「現有表情」基礎上加「肢體動作」。角色的臉部 identity 全程保持一致，只疊加不同層面的動作。

**Swap points:** `ivory silk dress` → `dark leather jacket` / `oversized sweater`；`afternoon light` → `cool evening blue` / `dramatic chiaroscuro`；手的動作 → `touching hair` / `crossing arms` / `adjusting necklace`。

---

### 35. 敘事短片「咖啡師手沖」3 Turn 編輯（場景 → 對白 → 配樂氛圍）

**Use case:** 生活品牌短片，咖啡廳/食飲場景，從無聲→加音效台詞→加環境音
**Target:** Gemini Omni Flash，8 秒推薦，16:9

#### 【Base Prompt】
```
Create an 8-second narrative video. A barista in a light linen shirt is 
concentrating on hand-pouring water over coffee grounds in a glass dripper. 
Close-up of hands, steam rising. The café is minimalist: white walls, natural 
wood counter, soft diffused north-facing window light. Shallow depth of field. 
No background music, no dialogue. The sound is ambient café atmosphere — just 
the quiet pour of water, slight steam sound. The barista's expression is focused, 
almost meditative. Warm, intimate, educational vibe.
```

#### 【Edit Round 1 — 加台詞層】
```
Keep the scene, barista, lighting, and visual composition identical. Add audio: 
the barista should speak a brief, quiet line (in a natural, conversational tone) 
while pouring, something like "The water temperature matters most." The voice 
should be male, calm, educational but not stiff. The line should sync with the 
pour action around second 4. Keep all the ambient café sounds (pour, steam).
```

#### 【Edit Round 2 — 配樂與情緒】
```
Keep the barista's spoken line and all visual elements from the previous version. 
Now add subtle background music: a gentle, minimalist piano or acoustic guitar 
piece that complements the meditative mood. The music should enter softly around 
second 2, rise very slightly during the pour (where the dialogue happens), then 
fade gently to silence by second 8. The overall feeling should shift from purely 
instructional to contemplative — still intimate, still coffee-focused, but with 
more emotional resonance.
```

**Why 有效：** 三層堆疊。Turn 1 視覺凍結。Turn 2 加聲音層（台詞）。Turn 3 加音樂層，但台詞保留。每層獨立可控，全程場景/角色未變。

**Swap points:** 台詞內容 → `"Three minutes, medium grind"` / `"The bloom comes first"`；角色性別 → 女性；背景樂風格 → `jazz ballad` / `ambient electronic`。

---

### 36. 美食廣告「蒸氣盛菜」3 Turn 編輯（菜品 → 蒸氣視覺 → 餐廳氛圍）

**Use case:** 食飲品牌，從靜物美食→加蒸氣動態→加環境故事感
**Target:** Gemini Omni Flash，7 秒推薦，16:9

#### 【Base Prompt】
```
Create a 7-second food video. A ceramic bowl filled with steaming hot dumpling 
soup — clear broth, delicate wontons, fresh green onion garnish, soft tofu. The 
bowl sits on a light wood table with wooden chopsticks beside it. Warm golden 
hour light from a window on the left. Camera: static, slightly elevated angle 
(30° down-looking). Shallow depth of field, soft bokeh background (out-of-focus 
restaurant interior). Warm, comforting, appetite-inducing. The soup is still 
steaming but no movement yet — pure product showcase.
```

#### 【Edit Round 1 — 蒸氣動態】
```
Keep the bowl, broth, garnish, table, and lighting identical. Add subtle motion: 
wisps of steam should rise continuously from the surface of the broth over the 
7 seconds. The steam should move naturally with gentle air currents — rising, 
curling, dispersing. It should look organic, not mechanical. Capture the 
sensory moment of hot soup reaching your senses. No other movement — the bowl, 
chopsticks, and background stay static.
```

#### 【Edit Round 2 — 餐廳故事感】
```
Keep the bowl, broth, steam motion from the previous version. Now expand the 
background: as the camera slowly pulls back (a gentle 8% zoom-out over 7 
seconds), reveal more context — the hands of someone reaching for the chopsticks 
(just hands, arms visible at frame edge, not distracting). A second bowl being 
placed by a server in soft motion at far left of frame (depth blur). Warm 
restaurant ambient light on wood tables. The feel should shift from "food 
product" to "dining experience story" while keeping the hero soup as focus.
```

**Why 有效：** 三層編輯。Turn 1 加「視覺動態」（蒸氣）。Turn 2 加「敘事層」（人物→服務→餐廳）。菜品完整保留，蒸氣保留，只新增周邊情節層。最終成品既有產品清楚度，又有生活氛圍故事感。

**Swap points:** `wontons soup` → `ramen` / `braised meat rice`；`golden hour` → `warm indoor restaurant tungsten`；背景人物 → `wife's hands joining` / `child reaching for spoon`。

---

### 37. 精品揭幕「香水瓶全景」3 Turn 編輯（產品→打光技法→旋轉視角）

**Use case:** 香水/香氛品牌，完整產品揭幕流程，多輪控制視角光感
**Target:** Gemini Omni Flash，10 秒推薦，16:9

#### 【Base Prompt】
```
Create a 10-second luxury fragrance product reveal. A cut-crystal glass 
perfume bottle (Art Deco geometric facets) sits centered on a black marble 
pedestal. The bottle contains deep amber liquid. Initial framing: wide 
establishment shot showing the full pedestal and background (dark charcoal 
cyclorama), the bottle in center with significant negative space. Camera: 
locked-off static. Lighting: cool 5500K practical overhead spotlight creating 
a single sharp highlight on the bottle's top facet. Minimal shadows, clean and 
architectural. No motion. Premium, sculptural, precious object reveal aesthetic.
```

#### 【Edit Round 1 — Chiaroscuro 打光改進】
```
Keep the bottle form, marble pedestal, dark background, and camera framing 
identical. Change ONLY the lighting approach: introduce a second light source 
(dramatic side light from camera-left, warm golden 2700K) creating strong 
Chiaroscuro contrast — half the bottle catches warm light with deep amber glow, 
the other half falls into cool shadow. This should feel more cinematic, more 
luxe, like a jewel commercial. The overhead cool light remains as secondary rim.
```

#### 【Edit Round 2 — 旋轉揭幕視角】
```
Keep the Chiaroscuro lighting from the previous version (warm side light + cool 
overhead rim). Now add camera motion: a slow 90° clockwise orbit around the 
pedestal over 10 seconds, beginning from the wide establishment view and 
tightening slightly as it rotates. During the rotation, let viewers see the 
bottle's facet geometry catch both the warm and cool lights differently from 
each angle. End frame: bottle in 3/4 profile, catching maximum Chiaroscuro 
contrast. Smooth, no jitter, one fluid motion.
```

**Why 有效：** Turn 1「打光」× Turn 2「運鏡」分離。瓶身形狀/液體顏色全程凍結，光與視角獨立疊加。最終看起來像專業 3D 渲染的豪華揭幕，但全是 multi-turn 對話控制。

**Swap points:** `cut-crystal / Art Deco` → `frosted matte glass` / `minimalist cylinder`；`amber liquid` → `clear` / `pale rose`；`orbit 90°` → `full 360°` 或 `slow dolly-in only`；`Chiaroscuro` → `three-point lighting` / `single dramatic spotlight`。

---

## 🔑 Gemini Omni 多輪編輯黃金法則

1. **每 turn 只改一個變數** — 打光 XOR 機位 XOR 動作 XOR 配樂，別同時改多個（無法歸因，Omni 容易混亂）
2. **明確寫「Keep」語句** — 例 `Keep the lighting from the previous version` 防止 Omni 誤重做
3. **用「從 X → Y」結構** — 例 `replace the left key light with overhead spotlight`，比「加強打光」更精準
4. **不需重述「已鎖定」的元素** — 產品形狀、顏色、場景背景、主要角色，turn 2-3 別重寫，只寫「改什麼」
5. **一鏡一運動**：如果是 multi-turn 加運鏡，務必 `ONE camera motion per shot`（avoid gimbal shake 或同鏡多次轉向）
6. **時間錨點明確**：audio turn 要寫 `around second 4` 這樣的秒數錨點，Omni 才能同步台詞與視覺
7. **降期待值**：Omni 對「極細節」(如「水珠悬浮角度」) 的控制不如 Kling Motion Brush，但對「光色 / 情緒層 / 廣域運鏡」的理解遠強於 i2v 模型

---

## 連動

- 概念先行 → [concept-first-prompting.md](../references/concept-first-prompting.md)
- 反瑕疵 → [quality-control.md](../references/quality-control.md)
- Omni 完整技巧 → [../references/model-picker.md](../references/model-picker.md) § Gemini Omni
- 多輪產品 i2v 基礎 → [§31 產品 Hero i2v](#31-產品-hero-i2v)
