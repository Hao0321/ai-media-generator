# Seedance 2.5 垂直領域提示詞配方庫

把商業、建商、廣告、產品、汽車、美食、電影與動畫需求，編譯成可驗收、可降級到 Seedance 2.0 的 production brief。研究校準：**2026-08-09**。

## 目錄

1. [版本與證據邊界](#1-版本與證據邊界)
2. [所有商業案共用的編譯規則](#2-所有商業案共用的編譯規則)
3. [商業／企業影片](#3-商業企業影片)
4. [建商／建築／房地產](#4-建商建築房地產)
5. [廣告／投放／UGC](#5-廣告投放ugc)
6. [產品／電商](#6-產品電商)
7. [汽車](#7-汽車)
8. [美食／餐飲](#8-美食餐飲)
9. [電影／敘事](#9-電影敘事)
10. [動畫](#10-動畫)
11. [領域選型矩陣](#11-領域選型矩陣)
12. [交付前 Linter](#12-交付前-linter)
13. [研究來源](#13-研究來源)

## 1. 版本與證據邊界

- BytePlus 公開 prompt guide 在 2026-07-31 更新後仍明列 **Dreamina Seedance 2.0 series**。
- Dreamina／CapCut 的 2.5 行銷頁宣布 30 秒、4K、多參考、R2V 與局部編輯，但同一官方生成器頁仍顯示 **coming soon**。把這些視為 announced capability，不是所有帳號／入口都可用的參數。
- AtlasCloud 的公開案例庫明寫：目前 executable default 是 Seedance 2.0，只有 provider 真正露出 2.5 才切換。150 prompts／50 previews 是案例覆蓋，不是 150 次跨平台成功率。
- 任何模板先填 provider、可見 model ID、時長、解析度、素材槽位、audio、edit／extend；未確認就用 2.0 的 15 秒可拆分版本。

證據用法：官方文件鎖能力與限制；附 prompt＋preview 的原始案例萃取結構；只有文案或轉貼的規格不得升格成硬規則。

## 2. 所有商業案共用的編譯規則

### 2.1 先寫 Communication Contract

```text
Audience: <誰會看>
Single message: <只傳達一件事>
Observable proof: <畫面能證明什麼>
Desired action: <看完要做什麼；CTA 字樣後期加>
Truth boundary: <不可虛構的數字、機能、景觀、效果或認證>
Delivery: <9:16 paid social / 16:9 website / 1:1 ecommerce>
```

模型只生成可見事件，不能替品牌證明療效、性能、投報率、建案交期或法規聲明。所有精準數字、價格、Logo、CTA、免責與法規文字後期合成。

### 2.2 Reference Job Cards

```text
@image1 = PRODUCT_OR_BUILDING_GEOMETRY_ONLY
@image2 = MATERIAL_AND_COLOR_ONLY
@image3 = TALENT_IDENTITY_ONLY
@image4 = LOCATION_LAYOUT_ONLY
@video1 = CAMERA_PATH_AND_PACING_ONLY
@audio1 = VOICE_OR_MUSIC_RHYTHM_ONLY

Conflict precedence:
truth/source geometry > identity > brand material/color > spatial continuity > proof event > camera/style
```

不要上傳沒有職責的 moodboard。商品、車、建築最常失敗的原因不是 prompt 太短，而是來源互相矛盾。

### 2.3 商業影片節奏

```text
15s: Hook 0–3 → Proof 3–9 → Hero 9–13 → Clean hold 13–15
30s: Hook 0–4 → Context 4–10 → Proof 10–20 → Payoff 20–26 → Clean hold 26–30
```

若 UI 只支援 15 秒，把 30 秒拆成兩段；每段都有獨立 end state 與 continuation state。不要硬塞八個賣點。

### 2.4 商業驗收四問

1. 靜音觀看時，三秒內看得懂主體與用途？
2. 畫面真的呈現 proof，還是只有氣氛？
3. 商品／車／建築／食物在每鏡是否仍是同一個？
4. 最後是否有 1–3 秒穩定 hero frame 可供後期加 Logo／CTA？

## 3. 商業／企業影片

適用 B2B、品牌形象、企業文化、製造流程、創辦人故事與服務概念。首選真實廠房／團隊／產品素材；AI 畫面不得偽裝成已發生的客戶成果。

### 核心控制

- 一支片只講 `problem → observable process → outcome category`，不編造客戶、營收、效率數字。
- UI、儀表板與數據畫面用真截圖 reference；難以準確生成時改為後期 motion graphic。
- 人員安全裝備、機械流程與場域動線必須符合真實 SOP。

### Template：企業能力證明片

```text
[MODEL GATE] <provider / visible model / duration / aspect>
[FORMAT] 16:9 corporate documentary, restrained, credible, hard cuts only.
[REFERENCE MAP]
@image1 = facility layout and machinery identity only.
@image2 = actual product geometry only.
@video1 = real process order and operator motion only.
[TRUTH LOCK]
Show only processes visible in the references. No invented client, metric, certification,
screen UI, uniform logo or production claim.
[TIMELINE]
00:00–00:03 — Start in motion: macro material enters the real process; clean mechanical SFX.
00:03–00:08 — Medium tracking shot follows one operator performing one verified step.
00:08–00:12 — Macro proof: the product reaches its completed physical state.
00:12–00:15 — Product and team in a stable clean frame; negative space right 25% for post text.
[LOOK] Natural available light plus practical industrial fixtures, accurate materials,
observational camera, no stock-footage smiles.
[ENDING] Hold the verified product for 2 seconds; no generated slogan, logo or statistics.
```

QA：流程順序、PPE、手部與機械接觸、產品形狀、畫面中不存在虛構文字。

## 4. 建商／建築／房地產

用途分成三類：`concept visualization`、`actual-property marketing`、`floor-plan walkthrough`。三者不可混稱；概念畫面必在發布階段標示示意。

### 最佳輸入與硬鎖

- `@image1` 2D 平面圖：牆、門、窗、走道與房間相對位置。
- `@image2` 白模／BIM massing：樓高、立面開口與體量。
- `@image3–N` 實際材質、室內 reference、基地照片與核准景觀。
- `@video1` camera route only：動線速度與轉向，不負責改空間。
- 禁止自行增加房間、挑高、陽台、泳池、山海景、交通或公共設施。

### Template：平面圖到樣品屋動線

```text
[FORMAT] Architectural visualization walkthrough, 16:9, ONE CONTINUOUS TAKE.
[REFERENCE MAP]
@image1 = exact floor-plan topology only: walls, doors, windows, room adjacency and scale.
@image2 = exact white-model massing and ceiling height only.
@image3 = materials and furniture mood only; it may not alter topology.
[SPATIAL LOCK]
Follow this route only: entrance → living room → dining area → kitchen → primary bedroom.
Never pass through a wall; every doorway remains in its floor-plan position.
[CAMERA]
Human eye height 1.55 m, slow stabilized gimbal, natural walking pace, 24–28mm equivalent,
no impossible drone move indoors, no speed ramp.
[LIGHT]
Keep one sun direction and time of day throughout; exposure changes naturally between rooms.
[TRUTH CONTRACT]
Do not add rooms, windows, views, amenities, landscaping, people, signage or fixtures absent
from approved references. This is a concept visualization, not documentary footage.
[ENDING]
Stop inside the primary bedroom facing the approved window; hold 1 second; no text or logo.
```

### Template：建案情緒廣告

把 `空間 proof` 放在情緒前：先看得懂基地／建築／室內，再放生活感。若是未完工建案，人物與活動不得暗示已交屋。

QA：平面拓撲、門窗數量、材質、採光方向、景觀真實性、concept disclaimer safe zone。

## 5. 廣告／投放／UGC

先選 `brand film` 或 `performance ad`。前者賣定位與感受；後者賣一個痛點、一個 proof、一個行動。不要把高級 TVC 與假手機自拍混在同一支。

### Template：9:16 UGC 投放素材

```text
[FORMAT] 9:16 creator-style paid-social video, <15/30s>, hard cuts only.
[SUBJECT / PRODUCT LOCK]
@image1 controls the creator identity and outfit only.
@image2 controls exact product shape, cap, dispenser, material and color only.
[CAPTURE BEHAVIOR]
Consumer phone camera, slight handheld drift, imperfect reframing, brief autofocus settling,
subtle exposure breathing. The camera reacts a fraction late; no beauty-commercial polish.
[TIMELINE]
00:00–00:03 — Already using the product; show the surprising physical result first.
00:03–00:08 — Creator points to one visible feature and demonstrates one action.
00:08–00:12 — Macro proof shot; product remains readable and unchanged.
00:12–00:15 — Creator holds product beside the result; direct eye contact; clean lower-third safe zone.
[AUDIO]
Exact owner and line if supplied; production sound and one product SFX; no random BGM.
[TRUTH]
No invented testimonial, before/after result, health claim, discount, review count or authority badge.
[ENDING]
Hold 1 second for editor-added CTA. No generated captions, Logo, URL or price.
```

### 投放變體規則

- 先鎖一支 master 的產品、人物、proof 與 ending，再只改 hook 或場景。
- 局部 edit／SKU 替換只有 UI 明確支援時使用；每版重新驗收標籤、顏色與物理互動。
- 對照組一次只改一個變量：hook、creator、proof、camera 或 offer framing。

## 6. 產品／電商

首選 I2V：先把 packshot 做對，再動畫。Prompt 主要寫 motion，不重述產品外觀。

### Product Integrity Ledger

```text
silhouette / dimensions / component count / label placement / logo geometry
material finish / color / transparency / reflections / moving parts / legal marks
```

### Template：精品產品 Hero

```text
Use @image1 as the exact source composition and product master.
Preserve product silhouette, proportions, component count, label placement, readable source text,
logo geometry, material finish, color, background layout and key-light direction exactly.
Animate only: a 35-degree slow clockwise turntable rotation and one narrow highlight sweep from
camera-left to camera-right. Camera performs one 10% slow push-in; no orbit.
The base remains in full contact with the surface; reflections respond physically to the movement.
By 00:08 the product faces three-quarter front and stops. Hold completely still for the final 2 seconds.
No redesign, flexing, liquid-like surface, warped label, duplicate item, hand, scene replacement,
generated copy, subtitle, Logo overlay, fade or outro.
```

### Template：功能 Demo

每段只示範一個機構：`未操作狀態 → 手部對準 → 完成操作 → 可見結果`。手不能穿透產品；零件數量守恆；UI／數值使用真素材或後製。

QA：輪廓像素級比對、標籤位置、接縫／按鍵／孔位、材質反射、手指、最終 packshot。

## 7. 汽車

車是高風險剛體：車身比例、軸距、輪圈、燈組、格柵、徽標、車牌與玻璃線條要分別鎖。不要用文字讓模型重新設計 reference car。

### Template：電動車 Launch Film

```text
[FORMAT] Premium automotive commercial, 16:9, <15/30s>, hard cuts only.
[VEHICLE MASTER]
@image1 = exact vehicle identity and exterior geometry only.
@image2 = exact wheel design, paint color and lighting signature only.
Preserve body proportions, wheelbase, four-wheel count, wheel arches, glasshouse, lamps,
grille, mirrors, badges and plate area. No redesign or model-year drift.
[ROAD / PHYSICS]
All four tires maintain believable road contact; wheel rotation matches travel speed;
suspension compresses subtly under acceleration and cornering; reflections move with the body panels.
[TIMELINE]
00:00–00:04 — Low static three-quarter front: lamps wake sequentially; wet street reflections.
00:04–00:10 — Low lateral tracking beside the moving car; one smooth acceleration; locked horizon.
00:10–00:13 — Macro insert of wheel and body highlight; no impossible wheel deformation.
00:13–00:15 — Car stops at the approved hero angle; stable negative space for post copy.
[AUDIO] Tire texture, restrained motor/engine note, one indicator or door sound; no random music.
[EXCLUDE]
Extra wheel, changing badge, melted body panel, sliding without wheel rotation, floating tire,
driver teleport, impossible road reflection, generated plate text, Logo overlay or fade.
```

汽車內外景分開生成較穩；若要同支連續，先鎖駕駛位置、方向盤側、座椅／儀表與窗外路線。高速度用可見行為 `rapid lateral tracking, strong road spray, compressed background`，不要只寫 `fast`。

## 8. 美食／餐飲

美食不是「saturated food porn」就會好看；要鎖原料狀態、烹調狀態、黏度、溫度、表面與操作順序。

### Food Physics Ledger

- 水／茶：透明、低黏度、細小飛滴、折射。
- 醬汁／糖漿：高黏度、連續拉絲、較慢回落。
- 起司／麵糊：有彈性但不變橡膠；拉伸有斷點。
- 肉／麵包：重量、切面纖維、焦化邊緣、油脂反光不可塑膠化。
- 蒸氣：從熱源上升、受氣流影響，不從冷物件亂冒。

### Template：15 秒餐廳招牌菜

```text
[FORMAT] 9:16 premium food commercial, 15s, macro inserts with hard cuts only.
[DISH LOCK]
@image1 controls exact plating, ingredient count, plate geometry and color only.
No ingredient substitution, multiplication or garnish redesign.
[TIMELINE]
00:00–00:03 — Macro hook: knife breaks the crisp surface once; crumbs fall with real weight.
00:03–00:07 — Side-lit close-up: interior steam rises naturally; camera makes one slow push-in.
00:07–00:11 — One sauce pour from above; <viscosity> ribbon lands on the intended area only.
00:11–00:13 — Fork lifts one bite; fibers separate naturally; plate remains unchanged.
00:13–00:15 — Finished hero plate, stable overhead three-quarter frame, clean top safe zone.
[LIGHT / COLOR]
Soft side key, gentle negative fill, accurate appetizing color without neon oversaturation;
ceramic, sauce, crust and steam retain distinct materials.
[AUDIO]
Real knife crack, soft sizzle, sauce contact and room tone; no dialogue, no exaggerated crunch.
[EXCLUDE]
Plastic food, endless cheese pull, impossible steam, duplicated garnish, utensil penetration,
extra fingers, dirty preparation, generated menu text, logo, subtitle or fade.
```

QA：食材數量、熟度／溫度、液體黏度、刀叉與手、食安、盤面最後是否可對照 reference。

## 9. 電影／敘事

一支生成片段只承擔一個 dramatic turn。先寫角色與事件狀態，再寫電影感；不要用導演名取代可拍攝的攝影、表演與美術。

### Template：電影概念場景

```text
[FORMAT] Live-action dramatic scene, 2.39:1, <duration>, one continuous event with hard cuts only.
[REFERENCE MAP]
@image1 = protagonist identity and wardrobe only.
@image2 = location topology and prop placement only.
@audio1 = protagonist voice timbre only.
[DRAMATIC CONTRACT]
Before: <what the character believes / wants>.
Event: <one observable discovery, confrontation or choice>.
After: <what visibly changes by the end>.
[PERFORMANCE]
Restrained posture, named eyeline, breathing and one micro-expression change; no theatrical posing.
[SHOT PLAN]
00:00–... establish geography and pressure.
... medium action shows the event without dialogue overload.
... close-up records the turn; only the named speaker says "<exact line>".
... final wide/close holds the changed relationship or unresolved threat.
[LOOK]
Define lens/framing, motivated key source, palette, texture and camera behavior directly.
[AUDIO]
Room tone, event-bound Foley, exact dialogue owner and pauses; music enters only at a named beat.
[ENDING]
Hold the new state for 0.7s; no trailer title, subtitle, watermark or unrequested black fade.
```

若超過實際模型時長，按 scene state 拆段並帶 Continuation State。IP／真人：不用受保護角色、明星臉、原台詞或「完全仿某位在世創作者」；改寫成鏡頭、光線、色彩、節奏與表演語法。

## 10. 動畫

先選媒材：2D cel、cutout、stop-motion、clay、felt、stylized 3D 或 motion graphics。不要同時要求 2D 平塗與寫實 3D 皮膚。

### Animation Identity Ledger

```text
silhouette / scale / proportion / palette / line weight / material
face design / outfit / accessory / rig limits / movement signature / emotional baseline
```

### Template：原創 3D Physical Comedy

```text
[FORMAT] Original stylized 3D feature-animation comedy, 9:16, 15s, four hard-cut shots.
[CHARACTER LOCK]
@image1 = Character A design only: short wide silhouette, large square glasses,
three-finger cartoon hands, quick staccato steps, overconfident baseline.
@image2 = Character B design only: tall narrow silhouette, long sleeves,
slow elastic movement, deadpan baseline.
Maintain scale contrast, face design, palette, outfit and material in every shot.
[TIMELINE]
00:00–00:03 — Setup: A pulls one lever; B watches without moving.
00:03–00:07 — Escalation: the machine inflates one balloon behind A; A remains unaware.
00:07–00:11 — Collision: balloon pops once; A jumps vertically; B only blinks.
00:11–00:15 — Payoff: A pretends nothing happened; B slowly hands over ear protection; hold reaction.
[ANIMATION]
Readable anticipation → action → settle; clear arcs, contact, weight and recovery;
facial motion follows the named beat, never random lip movement.
[AUDIO]
Mechanical click, rubber stretch, one pop, short room reverb; no dialogue or music.
[EXCLUDE]
Existing copyrighted character resemblance, face drift, changing scale, extra limbs,
rubbery environment, dissolve, morph between shots, caption, Logo or freeze-frame opening.
```

### Template：2D Cel 動畫

固定 line weight、shadow shape、frame cadence、palette 與背景繪法；動態用 pose-to-pose、smear frame、impact frame、held cel 等動畫語言。禁止每鏡更換 rendering pipeline。

QA：角色 silhouette、比例、色票、線寬、材質、動作 anticipation／contact／recovery、剪接與口型 owner。

## 11. 領域選型矩陣

| 領域 | 首選模式 | 最重要 reference | 最高風險 | 必驗收 |
|---|---|---|---|---|
| 商業／企業 | 真素材 R2V／紀錄式 | 真流程、場域、產品 | 虛構成果／數據 | SOP、PPE、真實聲明 |
| 建商 | multi-reference walkthrough | 平面圖＋白模 | 空間與景觀造假 | topology、門窗、示意標示 |
| 廣告／UGC | reference I2V | 產品＋creator | 假見證／錯 CTA | hook、proof、safe zone |
| 產品 | I2V motion-only | packshot master | 形狀／文字漂移 | silhouette、label、hold |
| 汽車 | I2V＋motion ref | 車身 master＋wheel | 多輪／熔車／滑行 | 幾何、接地、反射 |
| 美食 | I2V／macro T2V | plating master | 塑膠食物／錯物理 | 熟度、黏度、食材數量 |
| 電影 | controlled multi-shot | 角色＋場景＋聲音 | 變臉／事件過載 | dramatic turn、state |
| 動畫 | model sheet＋storyboard | 角色表＋timing ref | style／比例漂移 | silhouette、timing、reaction |

## 12. 交付前 Linter

1. 已記錄 provider 與可見 model ID，而不是只寫 Seedance 2.5？
2. 30 秒、4K、多參考、audio、edit 等規格都由 UI／API 實際確認？
3. 已定義 audience、single message、observable proof 與 truth boundary？
4. 每個 reference 有唯一職責與衝突優先級？
5. 產品／車／建築／食物有具體 invariant ledger？
6. 每個 time block 只有一個主狀態變化與可驗收 end state？
7. camera move 可實際拍攝，且每個 beat 不堆疊互斥運鏡？
8. 所有精準文字、Logo、價格、CTA、數據與免責都安排後期？
9. 企業案沒有虛構客戶、成果、流程或認證？
10. 建案沒有新增空間、設施、景觀或暗示已完工？
11. 廣告沒有假見證、療效、折扣或 before/after？
12. 商品的輪廓、零件、標籤、材質與最終角度可逐項核對？
13. 汽車的輪數、接地、轉速、軸距、燈組與反射合理？
14. 食物不同材質有不同黏度、重量、溫度與表面行為？
15. 電影角色有 state before／event／state after 與對白 owner？
16. 動畫角色的 silhouette、比例、palette、line/material 與 timing 鎖定？
17. Audio 事件有時間或 owner；不需要時明寫 none？
18. Ending 留出可後製且不漂移的 clean hold？
19. 沒有未授權人物、受保護角色、商標誤用或可辨識仿製？
20. 失敗重跑時只改一個 failure category？

## 13. 研究來源

### 官方／官方生態

- ByteDance Seedance 2.0 launch: <https://seed.bytedance.com/blog/seedance-2-0-official-launch>
- BytePlus Seedance 2.0 prompt guide（2026-07-31 update）: <https://docs.byteplus.com/en/docs/ModelArk/2222480>
- BytePlus 2.5 launch contact page: <https://www.byteplus.com/en/contact-us/ai-seedance2-5-official>
- Dreamina 2.5 generator（頁面同時標示 coming soon；能力需 gate）: <https://dreamina.capcut.com/seedance/seedance-2-5>
- Dreamina prompt guide: <https://dreamina.capcut.com/seedance/seedance-2-5-prompt>
- Dreamina floor-plan workflow: <https://dreamina.capcut.com/seedance/seedance-2-5-floor-plan-to-video>
- Dreamina cinematic ad examples: <https://dreamina.capcut.com/seedance/seedance-2-5-cinematic-ad-generator>
- Dreamina UGC workflow: <https://dreamina.capcut.com/seedance/seedance-2-5-ai-ugc-ads>

### Prompt＋preview／可追溯社群

- AtlasCloud 150 prompts／50 previews；明列 2.0 executable default 與 2.5 capability gate: <https://github.com/AtlasCloudAI/awesome-seedance-2.5-prompts-skills>
- YouMind 2.0 prompt library: <https://github.com/YouMind-OpenLab/awesome-seedance-2-prompts>
- ZeroLu X／微信來源庫: <https://github.com/ZeroLu/awesome-seedance>
- Reddit 2.0 vs 2.5 animation comparison: <https://www.reddit.com/r/Seedance_AI/comments/1vi3miq/seedance_2_vs_seedance_25_same_prompt_same/>
- Reddit product-ad workflow discussion: <https://www.reddit.com/r/Seedance_AI/comments/1vf3ot8/making_30second_product_ads_with_seedance_25/>

長 prompt 不從來源逐字搬運；本檔只保存跨案例可重現的結構、風險與原創模板。
