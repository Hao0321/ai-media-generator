# Seedance 2.5 社群案例蒸餾手冊

本檔把官方資料、公開提示詞庫與社群「prompt + 成品／預覽」案例，編譯成可重複使用的 Seedance 提示詞規則。研究日期：**2026-08-09**。

> 這不是把社群文案原封不動複製進 Skill。真正要學的是：什麼結構在什麼任務有效、哪些規格只存在於供應商宣傳、哪些案例其實是同一篇轉貼。
>
> 商業／企業、建商／建築、廣告、產品、汽車、美食、電影與動畫的領域配方，另讀 [seedance-domain-playbook.md](seedance-domain-playbook.md)。

## 目錄

1. [證據分級與版本閘](#1-證據分級與版本閘)
2. [平台覆蓋與去重結果](#2-平台覆蓋與去重結果)
3. [社群案例萃取出的十二個模式](#3-社群案例萃取出的十二個模式)
4. [任務路由](#4-任務路由)
5. [Production Brief 編譯格式](#5-production-brief-編譯格式)
6. [可直接套用模板](#6-可直接套用模板)
7. [失敗模式與修法](#7-失敗模式與修法)
8. [交付前 Linter](#8-交付前-linter)
9. [來源登錄表](#9-來源登錄表)

## 1. 證據分級與版本閘

### 證據分級

| 等級 | 定義 | 可訓練內容 |
|---|---|---|
| A | ByteDance／BytePlus／TikTok 官方產品頁、文件、API 範例 | 能力、限制、官方語法、平台特定規則 |
| B | 公開完整 prompt，且附成品、預覽或同 prompt 對比 | 結構、節奏、鏡頭、表演、聲音與 constraints 模式 |
| C | 有完整 prompt 但沒有可驗證成品，或只有創作者解讀 | 候選技巧；不得寫成硬規則 |
| Q | 發布前預測、聯盟行銷、SEO 文章、轉貼規格、無法追到原始案例 | 只進隔離區，不進預設模板 |

### 版本事實

- **Seedance 2.0：** 官方公開文件可確認四模態參考、最多 9 圖／3 影片／3 音訊、15 秒高品質多鏡頭音影片，以及生成、編輯、延長能力。
- **Seedance 2.5：** BytePlus 已有官方 launch landing page；但截至研究日，公開 ModelArk prompt guide 與 API model list 仍主要列 2.0。官方 landing page 的存在能證明名稱／上線行銷，不能單獨證明每個入口都支援相同的 30 秒、4K、50 references 或局部編輯參數。
- **Dreamina 狀態矛盾：** 官方生態的 2.5 內容頁宣傳 30 秒／4K／多參考，但 generator 頁同時顯示 `coming soon`；AtlasCloud 案例庫也明寫 executable default 仍是 2.0、provider 真正露出 2.5 才切換。因此行銷頁只證明 announced capability，不證明目前帳號可執行。
- **供應商標籤不是模型證明：** Atlas Cloud、CapCut、Starpop 或其他平台顯示「2.5」時，仍需看實際 model ID、duration、reference slots、resolution、editing controls 與真人臉政策。

### Capability Gate（寫 prompt 前必做）

```text
Provider / entry:
Visible model ID:
Max duration shown in UI:
Accepted references by type:
Output resolution options:
Native audio / dialogue:
Video edit / extend controls:
Real-face policy:
```

路由：

1. UI 明確提供 30 秒與對應槽位 → 可用 30 秒模板。
2. 官方 2.0／最多 15 秒 → 拆成 2–3 段，使用 Continuation State 接續。
3. TikTok Symphony 的 Seedance 2.0 wrapper → 官方建議簡潔、單一場景；多場景分開生成後在 editor 組合。
4. 版本不明 → 先寫可拆分 beat sheet，不宣稱任何未顯示規格。

## 2. 平台覆蓋與去重結果

| 平台 | 找到的有效材料 | 採用方式 |
|---|---|---|
| ByteDance / BytePlus / TikTok | 2.0 官方 launch、prompt guide、API 範例、Symphony 提示規則、2.5 launch landing | A 級基準與平台轉接器 |
| Reddit | 寫實臥室 slice-of-life、MiniDV 咖啡 vlog、3D physical comedy、雙角色對話、POV 動作長鏡 | B/C 級案例；學結構，不學未證實規格 |
| GitHub | YouMind 3,552 篇 2.0 庫、AtlasCloud 150 篇含 50 預覽、ZeroLu X／微信彙整 | 去重後萃取結構、類型與 provenance |
| X | 公開頁受登入牆限制；透過標示原作者與原連結的 GitHub 庫追溯 | 只採可追來源案例，不把鏡像算第二票 |
| Bilibili | 17 集提示詞系列、時間碼／運鏡／多人／情緒與動作邏輯案例 | 用來補中文社群題型；過載角速度案例列為反例 |
| 抖音 | 詳細導演 prompt vs 極簡 prompt 對比、即夢教學與 2.5 工作流展示 | 萃取「探索模式 vs 控制模式」 |
| 知乎 | 多參考角色分工、@ 素材角色、衝突優先級、時間碼公式 | C 級交叉驗證；規格仍回查官方 |
| TikTok | TikTok for Business 的 Symphony 官方 Seedance 2.0 說明 | 加入 wrapper-specific 單場景 adapter |
| YouTube | 公開索引多為 review／tutorial，少有可抓取的完整 prompt + 同頁成品 | 不製造假規則；只留平台交付比例資訊 |
| Instagram / Reels / Threads | 多為成果轉貼、付費平台導流或 Reddit 二手討論 | 不把成果 caption 當 prompt 證據 |
| 小紅書 | 公開搜尋索引不足，無法可靠取得 prompt + output 對 | 不納入硬規則，等待使用者提供原帖或可訪問連結 |

### 去重規則

- 同一 prompt 從 X → GitHub → Reddit → 部落格鏡像，只算**一個案例**。
- 同一組「30 秒／50 references／4K」規格文字出現在多個聯盟網站，不算多方證實。
- 「官方」標籤若只出現在第三方 curated repo，記為 B；必須追到 ByteDance／BytePlus 原頁才升 A。
- prompt library 的篇數是**覆蓋量**，不是成功率。只有預覽能證明某次設定曾成功，不能保證跨 provider 重現。
- 不逐字保存受著作權保護的長 prompt；保存短摘記、結構與原始連結。

## 3. 社群案例萃取出的十二個模式

### 3.1 先選 Exploration 或 Control

- **Exploration：** 使用者只求「武俠打鬥好看」「復古冰果室有奇蹟感」，且不需精準事件順序。用 1–3 句，交代角色、結果、整體攝影與聲音，留空間給模型 priors。
- **Control：** 多角色、客戶商品、對白、教學步驟、時間凍結、參考圖一致性、精準結尾。改用 Production Brief、reference map、timeline、locks 與 acceptance contract。
- 不要把「越短越好」或「越詳細越好」設成全域規則。控制需求決定 prompt 密度。

### 3.2 Reference Job Card：每個素材只做明確工作

```text
@image1 = HERO_IDENTITY_ONLY
@image2 = WARDROBE_AND_ACCESSORY
@image3 = LOCATION_LAYOUT
@video1 = CAMERA_PATH_AND_PACING_ONLY
@audio1 = VOICE_TIMBRE_AND_CADENCE_ONLY

Conflict precedence:
identity > product geometry/text > continuity locks > action > camera/style
```

- 不要寫「參考全部素材」；要寫哪個素材控制外貌、服裝、空間、動作、運鏡、聲音或首／尾幀。
- I2V 不重述已在圖中的外貌與產品細節；只寫「什麼動、怎麼動、什麼不能動」。
- 素材越多，衝突面越大。只上傳有工作的 references。

### 3.3 Character Lock 不只鎖臉，也鎖 Performance Signature

每個重要角色建立：

```text
visual identity / scale / silhouette / outfit / accessory
movement signature / default posture / emotional baseline
voice binding / dialogue ownership
```

3D 喜劇案例顯示：角色若有不同身形、移動節奏與反應方式，比只寫「same character」更容易維持辨識。真人多鏡則再鎖髮型、飾品、年齡感、手持物與服裝。

### 3.4 Start in Motion

- Vlog／UGC：第一幀就已在調相機、拿杯子、走進畫面或做任務；避免靜止 reference sheet 感。
- 動作片：威脅已接近或角色已奔跑，除非故事刻意需要建立鏡。
- 若首幀必須精準對齊 reference，寫「首幀保持 0.3–0.5 秒後開始」，不要同時要求 instant motion。

### 3.5 一個 time block 一個主事件，不是一個字面動詞

舊版「ONE verb per shot」過度絕對。更準確的規則：

- 每個時間區塊有一個**可驗收的主狀態變化**。
- 支撐主事件的微動作可存在，例如「倒入牛奶並形成拉花」仍是一個主事件。
- 角色 A、B、C 各自執行無關任務，或同一區塊同時塞追逐、爆炸、對白、變裝，才是過載。
- 2–3 秒 montage 適合單一步驟；3–5 秒適合敘事 beat；複雜物理／對白應給更長。

### 3.6 Camera Operator Behavior 比單寫 handheld 更真

社群寫實案例反覆出現：

- 相機對動作慢半拍，之後才重新構圖
- 微小手震，而非持續劇烈抖動
- autofocus 晚半拍後鎖定
- auto-exposure 輕微 breathing
- 不完美 framing、短暫遮擋、拍攝者調整握法
- MiniDV／手機感要寫真實裝置行為，不只寫濾鏡名稱

這些只用在 UGC／紀錄感；精品廣告、產品 packshot 與精密 VFX 不套用。

### 3.7 Transition Contract 必須唯一

在 prompt 頂部選一種主邏輯：

- `ONE CONTINUOUS TAKE — no cuts`
- `HARD CUTS ONLY at 00:03, 00:06... — no dissolve, no fade`
- `MATCH CUT on the shared object / gesture / shape`
- `PASS-THROUGH transitions while the camera crosses doors / windows / foreground occluders`

不可同時要求一鏡到底、硬切與 montage。Match cut 需要跨場景不變的 shared visual anchor；pass-through 需要門、窗、霧、衣物或前景遮擋作橋。

### 3.8 Lighting Arc 是長片一致性的第二時間軸

30 秒案例會明確寫：

- 光源方向與色溫基準
- 哪個事件改變光線
- 變化是突變、擴散或漸進
- 改變後保持到結尾

不要讓每個 shot 重新發明打光。若房間情緒不同，保留共同建築結構，讓窗外景色與窗光成為受控變量。

### 3.9 Audio Event Ledger

```text
00:00–00:03 room tone + cloth rustle
00:03 exact tray impact
00:04–00:07 liquid droplets, no music
Dialogue owner: HERO only
Dialogue delivery: low voice, 250 ms pause, restrained mouth motion
Music: none / or exact cue and beat transitions
```

- 寫實 UGC 常以 production sound only 勝過背景樂。
- 對白要同時寫 owner、exact line、delivery、pause 與 lip-sync；不需要對白就明寫所有人不開口。
- 音效與畫面事件共用時間碼，比「cinematic sound design」有效。

### 3.10 End State Contract

每支片定義最後 0.5–1 秒：

- 人物、產品、鏡頭與光線最後在哪裡
- 動作是否完整結束
- hold／freeze／loop／hard cut to black
- 是否允許字幕、logo、fade

沒有 end state，長片最容易在最後幾秒漂移、重做動作或生成多餘片尾。

### 3.11 Constraints 要寫成 Acceptance Contract

優先寫「保留什麼」再寫精簡排除：

```text
Preserve: identity, outfit, product geometry, spatial layout, camera path.
Allow change only in: steam, hand motion, highlight sweep.
Exclude: duplicate subject, reference sheet, unreadable text, extra limbs,
scene reset, unintended cut, fade, logo, subtitles.
```

官方公開文件沒有證實某種「negative embedding」機制；不要教使用者相信所有 no-X 一定按負向權重處理。用正向 preservation、allow-change scope、end state 與少量 exclusions 組合。

### 3.12 Revise One Variable at a Time

每次失敗先分類：identity、event order、camera、physics、audio、text、ending。只改一層並重跑；若 provider 有 draft／seed reuse，再保留 seed 與其他參數。不要整篇重寫，否則無法知道是哪個變量有效。

## 4. 任務路由

| 任務 | 首選結構 | 核心控制 |
|---|---|---|
| 5–10 秒單鏡氛圍 | 1–3 句 compact prompt | 主體 + 主事件 + camera + light + audio |
| 15–30 秒多鏡敘事 | Production Brief + timed beats | locks + timeline + transition + end state |
| UGC／Vlog | observational profile | start-in-motion + operator lag + optical imperfections + raw audio |
| 3D／動畫喜劇 | shot list + character performance signatures | silhouette + timing + hard cuts + reaction beats |
| 雙人／多人對話 | reference map + dialogue ownership | reverse shots + eyelines + voice binding + reaction shots |
| I2V 商品／角色 | motion-only prompt | preserve + allow-change scope + motion range + hold point |
| 教學／安裝步驟 | numbered timed steps | 每步參考圖 + 手部動作 +完成狀態 + VO |
| 一鏡到底穿越 | continuous camera path | spatial topology + portals/occlusion + constant motion |
| 影片局部編輯 | delta edit（能力需通過 gate） | source locked + only additions/removals + side effects scope |
| 時間凍結／倒轉 | 另讀 longform compiler | event states + exception set + physics ledger |

## 5. Production Brief 編譯格式

```text
[MODEL GATE]
Provider: <actual provider>
Model: <visible model ID>
Duration / aspect: <UI-supported value>

[FORMAT]
<live action / 3D / UGC / tutorial>, <aspect>, <duration>,
<ONE TAKE / HARD CUTS / MATCH CUT / PASS-THROUGH>

[REFERENCE MAP]
@image1 = ... only
@video1 = ... only
Conflict precedence: ...

[LOCKS]
Identity: ...
Wardrobe/accessory: ...
Product/prop geometry: ...
Location topology: ...
Performance signature: ...

[CAMERA / LOOK]
Capture behavior, lens/framing, movement, light baseline, texture.

[TIMELINE]
00:00–00:04 — shot / observable event / camera / audio / required end state
00:04–00:08 — ...

[LIGHTING ARC]
Baseline → trigger → transition → held final state.

[AUDIO LEDGER]
Dialogue owner and delivery; ambience; timed SFX; music rule.

[ACCEPTANCE CONTRACT]
Preserve ...
Allow change only in ...
Exclude ...

[ENDING]
Final pose / framing / sound / hold / cut. No unrequested outro.
```

## 6. 可直接套用模板

### A. 寫實 UGC／Vlog

```text
[FORMAT] 9:16 observational UGC, <duration>. Multi-shot montage with hard cuts only.
[SUBJECT LOCK] @image1 controls identity only. Same person, outfit and handheld prop throughout.
[CAPTURE BEHAVIOR] Consumer <phone/MiniDV> footage: slight handheld drift, imperfect reframing,
brief autofocus settling and subtle exposure breathing. The camera reacts a fraction late to movement.
Natural blinking, breathing and pauses; restrained performance; never poses for camera.
[TIMELINE]
00:00–... Already in motion: <one observable beat>.
... Each beat has one main state change and a completed end state.
[AUDIO] Raw production sound only: <timed room tone, object sounds, breath>. No music.
[CONSTRAINTS] No beauty-commercial polish, duplicated subject, reference sheet, captions, logo or watermark.
[ENDING] Subject covers the lens / completes the task; hold 0.5s; hard cut.
```

### B. 角色鎖定的 3D Physical Comedy

```text
[FORMAT] Stylized feature-animation physical comedy, 9:16, <duration>, <N> hard-cut shots.
[CHARACTER LOCK]
Character A: @image1 identity, <short/wide silhouette>, <quick staccato movement>, <overconfident baseline>.
Character B: @image2 identity, <tall/narrow silhouette>, <slow elastic movement>, <deadpan baseline>.
Maintain scale contrast, outfits and facial design in every shot.
[LIGHTING ARC] <baseline> → <event trigger> → <final held state>.
[TIMELINE] <2–4s beats: setup → escalation → collision → reaction → payoff>.
[TRANSITIONS] HARD CUTS ONLY at <timestamps>; no dissolves, morphs or freeze-frame opening.
[AUDIO] <ambient>, impact accents, cloth/footstep sounds; no dialogue.
[ENDING] Clear reaction pose and completed gag; hold 0.7s.
```

### C. I2V 商品鎖形狀

```text
Use @image1 as the exact source composition.
Preserve the product silhouette, proportions, label placement, readable text, materials,
background layout and lighting direction exactly as shown.
Animate only: <one product/hand motion>, <one highlight or environmental motion>.
Motion range: <degrees / centimeters / start-to-end position>.
Camera: <single move>. Background movement: <none/subtle named elements only>.
The product reaches <final orientation> by <time> and holds for the final <seconds>.
No redesign, warped label, extra product, scene replacement, cut, fade or overlay text.
```

### D. 雙角色對話

```text
[REFERENCE MAP]
@image1 = Character A identity and outfit only.
@image2 = Character B identity and outfit only.
@audio1 = A voice only; @audio2 = B voice only.
[SPATIAL LOCK] A remains screen-left, B screen-right; consistent eyeline and room geography.
[SHOT PLAN]
00:00–... two-shot establishes both.
... over-shoulder on A: A says "<exact line>" in <delivery>, <pause>; B listens silently.
... reverse on B: B replies "<exact line>"; A reacts without speaking.
... final two-shot resolves the exchange.
[AUDIO] Precise voice ownership and lip-sync; room tone; no music unless specified.
[CONSTRAINTS] Stable faces, voices and outfits; no speaker swap, duplicate person, captions or extra dialogue.
```

### E. 影片 Delta Edit（僅在 UI 明確支援 edit 時）

```text
Source video locked: preserve @video1's composition, timing, camera movement, lighting,
color, subjects and natural imperfections exactly.
Edit objective: <single removal/addition/replacement>.
Allow secondary changes only in: <shadows / reflections / physical reaction caused by the edit>.
Everything else remains unchanged. No new cut, reframing, restaging, style conversion or duration change.
```

### F. 30 秒參考式教學

```text
[FORMAT] Clear reference-guided tutorial, <aspect>, 30s only if UI supports it.
[PRODUCT LOCK] @image1 controls product geometry, buttons, ports and markings.
[STEPS]
00:00–00:03 result preview; no unreadable generated title.
00:03–00:07 Step 1: <one manipulation>; show alignment point; end fully seated.
00:07–00:12 Step 2: ...
...
[VOICEOVER] One short sentence per step; terminology matches visible parts; no overlapping dialogue.
[CAMERA] Medium orientation shot → close-up only for the manipulated part; preserve left/right geography.
[CONSTRAINTS] Correct step order, no missing/extra component, no impossible hand penetration,
no logo mutation, no generated UI text. Add titles later in editor when text accuracy matters.
```

## 7. 失敗模式與修法

| 症狀 | 常見根因 | 修法 |
|---|---|---|
| 角色像不同演員 | reference 未分工、鎖只寫一次、不同稱呼漂移 | job card + invariant ledger + exact anchor nouns |
| 片頭像角色設定圖 | prompt 從站立介紹開始 | start-in-motion；排除 reference sheet／posing |
| 事件順序錯 | 一個 block 多主事件、無完成狀態 | 一 beat 一主狀態變化；寫 end state |
| 一鏡到底突然切 | camera path 沒有空間拓撲／過門 | 定義房間順序、門窗位置、pass-through bridge |
| Montage 變 dissolve | 只寫 montage，未定 cut contract | 精確 cut timestamps + hard cuts only |
| UGC 太像廣告 | 只寫 handheld + cinematic | 加 operator lag、focus settling、exposure breathing、raw audio |
| 商品重畫 | I2V 重述／改寫外觀，動作幅度過大 | preserve + allow-change only + motion range |
| 對白換人說 | 沒有 owner、reverse shot 太密 | speaker/voice binding + listening reaction blocks |
| 音效對不上 | 只列音效，未綁時間與事件 | Audio Event Ledger |
| 最後幾秒亂生 | 內容不夠填滿、無結尾 | 縮短 duration 或加 hold/end-state contract |
| 規格能寫但 UI 不接受 | 抄了 2.5 宣傳數字 | capability gate；按 provider 降級 |
| prompt 越修越差 | 每次全篇重寫 | 只改一個 failure category，保存其他條件 |

### 不納入預設的社群寫法

- 每 1–2 秒塞高速旋轉與精確角速度：可作實驗，不是通用增益；常讓物理與主體可讀性崩潰。
- `cinematic, 8K, masterpiece, professional` 單獨堆疊：缺乏可驗收事件與攝影行為。
- 把品牌／工作室風格名當唯一描述：改寫成輪廓、材質、光色、動畫 timing 與表演特徵。
- 12 個 hard cuts／30 秒套到所有片型：只適合音樂、喜劇或明確 montage。
- 同時寫 one take、hard cuts、montage、no cuts：互相矛盾。
- 因為某 repo 寫「2.5」就宣稱 provider 原生 4K／50 references／180 秒：禁止。

## 8. 交付前 Linter

逐項回答 yes/no；任何核心項為 no 就先修 prompt。

1. 已記錄 provider、可見 model ID、duration 與 reference slots？
2. 已選 Exploration 或 Control，沒有無理由地過長／過短？
3. 每個 reference 都有唯一或明確工作？
4. 角色／商品／場景的 invariants 能逐字找到？
5. 每個 time block 只有一個主狀態變化，且有完成狀態？
6. shot/cut/transition 規則只有一套且不矛盾？
7. camera 是可拍攝的路徑／行為，不是運鏡詞堆？
8. audio 已綁角色或時間事件？不需要音訊時是否明確寫 none？
9. 最後 0.5–1 秒已定義？
10. constraints 是否同時包含 preserve、allow-change scope、精簡 exclusions？
11. I2V 是否避免重述外觀，主要描述 motion？
12. 是否刪除未經 UI／官方文件確認的規格宣稱？

## 9. 來源登錄表

### A 級官方

- ByteDance Seedance 2.0 official launch: <https://seed.bytedance.com/blog/seedance-2-0-official-launch>
- BytePlus Dreamina Seedance 2.0 prompt guide: <https://docs.byteplus.com/en/docs/ModelArk/2222480>
- BytePlus video generation API: <https://docs.byteplus.com/en/docs/modelark/1520757>
- BytePlus Seedance 2.5 launch landing: <https://www.byteplus.com/en/contact-us/ai-seedance2-5-official>
- TikTok Symphony + Dreamina Seedance 2.0: <https://ads.us.tiktok.com/help/article/about-tiktok-symphony-and-dreamina-seedance-2-0>
- TikTok Symphony generation and prompt tips: <https://ads.us.tiktok.com/help/article/how-to-use-ingredients-to-video-in-symphony-creative-studio?lang=en>

### B 級 prompt + output／preview

- AtlasCloudAI, 150 prompts / 50 previews / provenance fields: <https://github.com/AtlasCloudAI/awesome-seedance-2.5-prompts-skills>
- Reddit observational bedroom slice-of-life: <https://www.reddit.com/r/seedance2pro/comments/1vj5vd8/how_to_create_a_candid_bedroom_sliceoflife_videos/>
- Reddit MiniDV coffee vlog: <https://www.reddit.com/r/Seedance_AI/comments/1vhv8g8/built_a_full_minidv_camcorder_coffee_vlog_grinder/>
- Reddit 3D animation physical comedy: <https://www.reddit.com/user/AssociationHead6964/comments/1vj43zv/seedance_25_prompt_3d_animation/>
- Reddit two-reference dialogue comparison: <https://www.reddit.com/r/Seedance_AI/comments/1vi3miq/seedance_2_vs_seedance_25_same_prompt_same/>
- Reddit POV action one-take prompt: <https://www.reddit.com/r/HiggsfieldAI/comments/1vi2zce/i_generated_this_video_using_seedance_25_and/>
- ZeroLu X/WeChat-sourced prompt collection: <https://github.com/ZeroLu/awesome-seedance>
- YouMind 2.0 prompt library: <https://github.com/YouMind-OpenLab/awesome-seedance-2-prompts>

### C 級中文社群／教學交叉驗證

- 抖音：詳細導演 prompt vs 極簡結果 prompt：<https://www.douyin.com/shipin/7431031956181288994>
- Bilibili Seedance 2 提示詞系列：<https://www.bilibili.com/video/BV1PjEd6yEc4/>
- Bilibili 高密度時間碼／運鏡案例：<https://www.bilibili.com/video/BV1pSZWBJEup/>
- 知乎多模態 prompt 方法（搜尋入口，規格需回查官方）：<https://www.zhihu.com/search?q=Seedance%202.0%20%E6%8F%90%E7%A4%BA%E8%AF%8D>

### 隔離來源

- 未追到 ByteDance／BytePlus 原始規格的 30 秒／50 references／180 秒／真人臉「滿血版」宣傳。
- 同文改寫的 API affiliate repos 與 SEO prompt farms。
- 無 prompt、只有生成結果 caption 的 Reels／Threads／YouTube 成果頁。
