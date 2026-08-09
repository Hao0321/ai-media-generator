# Auto-Pilot Mode — 一句話到成品的超傻瓜自動化

**定位：** 使用者只說「幫我做一個 XXX」，Claude 自動跑完全流程 — 解析意圖、選平台、寫腳本、設計分鏡、組 prompt、操作網站、回報。**小朋友都會用**。

**核心哲學：**
- **不問就做** — 能推斷的全部推斷，預設值寫進本檔
- **只在必要處停** — 花錢、不可逆、撞付費牆、敏感內容才問
- **自動套語彙庫** — 所有 prompt 強制走 [cinematic-direction.md](../references/cinematic-direction.md) / [commercial-direction.md](../references/commercial-direction.md) / [vfx-effects.md](../references/vfx-effects.md) / [sound-design.md](../references/sound-design.md) / [preset-packs.md](preset-packs.md)
- **全程可視化** — 每階段 show 簡短 summary 讓使用者知道進度，不一直問

---

## 觸發條件 (Auto-Pilot 何時啟動)

使用者說的話包含 **任一** 這些模式：

| 模式 | 範例 |
|---|---|
| 「幫我做 [媒體] 關於 [主題]」 | 幫我做一張老街拍攝感的海報 |
| 「做一個 [風格] 的 [類型]」 | 做一個 Wes Anderson 風的 30 秒廣告 |
| 「我要 [媒體] [主題]」 | 我要一個動漫異世界短片 |
| 「生個 [風格/主題]」 | 生個賽博龐克雨夜 MV |
| 「幫我用 [工具] 做 X」 | 幫我用 Kling 做古風武俠 |

**不啟動 auto-pilot 的情境：**
- 使用者問概念/教學 (「什麼是 anamorphic lens？」)
- 使用者要你看/改/評論既有 prompt
- 使用者明確說「先討論再做」

---

## Pipeline 架構 (7 階段)

```
[使用者一句話]
    ↓
[1] Intent Parser 拆解 9 slot
    ↓
[2] Fill Defaults 推斷缺失
    ↓
[3] Platform Decider 自動選
    ↓
[4] Script + Storyboard Auto-gen
    ↓
[5] Prompt Crafting (強制語彙庫)
    ↓
[6] Preview + Go (最後確認一次，30 秒 override)
    ↓
[7] Execution + Report
```

---

## Stage 1 — Intent Parser (意圖解析)

把一句話拆成 9 個 slot。**什麼沒講就填預設**，不要問。

### 9 個 Slot

| # | Slot | 範例值 | 無資訊時的預設 |
|---|---|---|---|
| 1 | **媒體** | image / video / music / multi (MV) | video (最多人要) |
| 2 | **長度** | 單張 / 5s / 10s / 30s / 1min / 長片 | 若 video → 10s；image → 1 張；music → 1 分鐘 |
| 3 | **畫面比** | 16:9 / 9:16 / 1:1 / 2.39:1 | 16:9 |
| 4 | **主題/故事** | 具體情節 | 從關鍵字 brainstorm 一個 |
| 5 | **風格** | 動漫 / 寫實 / 電影感 / 廣告 / MV / 奢侈品 | 若無 → 電影感 (cinematic) |
| 6 | **角色/主體** | 具體人/物 | 從主題推斷 |
| 7 | **場景** | 具體地點 | 從主題推斷 |
| 8 | **音訊** (video only) | 有/無 對白 / SFX / BGM | 若 video → 預設有 SFX + BGM，無對白 |
| 9 | **語言** | 英 / 中 / 日 | prompt 英文 (最穩)；對白若需 → 看使用者母語 |

### 解析範例

**Input:** 「幫我做一個動漫風格的異世界短片」

**拆解：**
| Slot | 值 | 推斷邏輯 |
|---|---|---|
| 媒體 | video | 「短片」 |
| 長度 | 10s (預設短片) | 無明說 → 預設最穩的 10s 測試 |
| 畫面比 | 16:9 | 預設 |
| 主題 | 異世界 / 穿越 / 魔法 | 「異世界」= 日系輕小說 tropes |
| 風格 | 動漫 + Shinkai/Ghibli 混 | 「動漫風格」→ 最穩的日系動畫 |
| 角色 | 年輕主角 (少年/少女) + 奇幻元素 | 異世界常見 |
| 場景 | 奇幻森林 / 古城 / 天空島 / 龍 | 選最有視覺衝擊的 |
| 音訊 | 有 BGM + 環境音，無對白 | 10s 短片不需對白，BGM 情緒夠了 |
| 語言 | prompt 英文 | 預設 |

---

## Stage 2 — Fill Defaults (補預設)

Intent 解析後，若 slot 仍空，按下表填預設：

### 預設選擇邏輯

| 情境 | 預設 |
|---|---|
| 「動漫」但沒說子類 | Shinkai + Ghibli blend (安全) |
| 「電影感」但沒說導演 | cinematic + Roger Deakins DP |
| 「廣告」但沒說品牌類型 | Apple product hero 或 Nike heroic，看主體選 |
| 「MV」但沒說風格 | Hiro Murai surreal 或 Cole Bennett 看節奏 |
| 「寫實」但沒說什麼 | Flux 1.1 Pro + 攝影感 |
| 「海報」但沒說 | Seedream 4.5 或 Nano Banana Pro (文字強) |
| 無音訊指示 (video) | BGM + 環境音，無對白 |
| 無色調指示 | 看題材 — 奇幻 → 暖金 / 科幻 → 冷藍 / 愛情 → 柔粉 |

---

## Stage 3 — Platform Decider (自動選平台)

按 [../references/selector.md](../references/selector.md) 的決策樹自動選。**不問使用者**，直接選最佳，在 Stage 6 preview 讓他 override。

### Quick Pick 矩陣

| (媒體, 風格, 特殊要求) | 首選 | 備選 |
|---|---|---|
| video + 動漫 | **Kling 2.6 Pro** (物理+運鏡) | Seedance 1.0 Pro (多鏡) |
| video + 寫實電影 | **Kling 2.6 Pro** 或 **Seedance 1.0 Pro** | Runway Gen-4 |
| video + 需對白 | **Veo 3.1 Fast/Quality** | Sora 2 |
| video + 多鏡故事片 | **Seedance 1.0 Pro** (Cut to 多鏡) | OiiOii Story Video |
| video + 角色一致 (有照片) | **Kling I2V** 或 **Runway Gen-4 Refs** | Vidu Q2 Ref2V |
| video + 短片 (<15s) | Kling / Seedance | Veo 3.1 |
| video + 長片 (>15s) | Vidu Q3 (原生 16s) / Kling Extend | Seedance Extend |
| image + 文字海報 | **Seedream 4.5/5** (中英) 或 **Nano Banana Pro** (英) | Ideogram 3 |
| image + 寫實人像 | **Flux 1.1 Pro** | Nano Banana Pro |
| image + 藝術風格 | **Midjourney v7** | Seedream |
| image + 編輯既有圖 | **Nano Banana** 或 **Flux Kontext** | - |
| image + 角色一致 (storyboard) | **Nano Banana Pro** (5 人) | Midjourney --oref |
| music only | **Suno v5** | - |
| MV (一站式) | **OiiOii Story Video** (前期免費，影片付 $7) | Suno + Kling 組合 |

### Cost-aware override

**若使用者預算模糊：**
- 短影片 → 優先免費額度 (Kling / Vidu 有免費 tier)
- 海報 → Flux Schnell on fal.ai (約 $0.003/image)
- Quality 重於 cost → 找最頂旗艦 (Kling 2.6 Pro / Nano Banana Pro / Midjourney v7)

---

## Stage 4 — Script + Storyboard Auto-gen

### Short video (5-15s)

**Template：**
```
Logline (1 句): [故事精華]

Character:
- 主角: [3+ 視覺錨 — 髮色/服裝/年齡/特徵]

Scene (1 個):
- 地點: [具體環境]
- 時間: [光線時段]
- 氛圍: [情緒+色調]

Single shot:
- 景別: [wide/medium/close-up]
- 運鏡: [最多 2 個]
- 動作: [主角做什麼，beats]
- 光影: [具體光源]
```

### Medium video (30s-1min)

**Template：**
```
Logline (1 句)
Tone (情緒弧): A → B → C
3-5 shots:
  Shot 1 (Hook, 0-3s): ...
  Shot 2 (Build, 3-10s): ...
  Shot 3 (Peak, 10-20s): ...
  Shot 4 (Resolution, 20-30s): ...

Characters: 角色卡 (每個 3+ 視覺錨)
Music: 風格 + BPM + 動態
```

### Long / MV (1-3min)

用 [music-video.md](music-video.md) 或 [storyboard.md](storyboard.md) 的完整 template。

### 自動 brainstorm 風險管理

若使用者的 logline 太模糊 (例：「異世界短片」)：
- **不要問他要什麼故事** — 直接生一個合情理的
- 展示 logline 在 Stage 6 preview，讓他換句話就能重新跑
- 準備 **1 個主稿 + 2 個備選** 方向，若主稿被否決立刻切

---

## Stage 5 — Prompt Crafting (強制語彙庫)

**每個 shot 的 prompt 必須含：**

### 必填 (5-8 token 中挑至少 5)
- [ ] 導演/DP 名 (cinematic-direction.md Part 2-3)
- [ ] 鏡頭/焦段 (cinematic-direction.md Part 4)
- [ ] 底片 stock (cinematic-direction.md Part 5)
- [ ] 光比/燈光 (cinematic-direction.md Part 6)
- [ ] 色彩分級 (cinematic-direction.md Part 5)
- [ ] 構圖 (cinematic-direction.md Part 6)
- [ ] VFX/大氣 (vfx-effects.md)
- [ ] (video) 音訊三層 — 對白/SFX/BGM (sound-design.md)

### 禁用詞彙
```
cinematic / 8k / beautiful / masterpiece / detailed / high quality /
best quality / professional / ultra realistic
```
這些通用詞會稀釋訊號。**寧可 5 個具體 token，不要 20 個泛詞**。

### 快速路徑 — 直接套 preset
[preset-packs.md](preset-packs.md) 有 30 個現成食譜，對應常見需求。若使用者的 intent 落在 preset 範圍 (Ghibli / Blade Runner / Wes Anderson / Apple / Nike 等)，**直接改占位符即可**，不用從頭組。

### 自檢 3 題
產完 prompt 問自己：
1. 有沒有導演或 DP 名字？
2. 有沒有具體底片 stock？
3. 有沒有具體光學細節 (光比/燈光/鏡頭)？

**3 題都缺就回去補**。

---

## Stage 6 — Preview + Go (最後確認)

**這是唯一讓使用者有機會改的節點。**

展示內容：

```
我要幫你做：
📽 [媒體 + 長度 + 畫面比]
🎬 平台：[選定平台 + 預估成本]
📝 Logline: [故事一句]
🎭 角色: [主角視覺錨]
🎨 風格: [導演 + 底片 + 色調]

📋 分鏡 (X shots):
Shot 1 — [情節] | prompt preview: [前 1 行]
Shot 2 — [情節] | prompt preview: [前 1 行]
...

💰 預估成本: [點數 / $X]
⏱ 預估時間: [生成 + 後製]

要繼續？(若 30 秒無回覆或「go/確認/OK」→ 開始執行；
 若回「改」或具體修改 → 調整)
```

### 使用者若改

| 改什麼 | 怎麼處理 |
|---|---|
| 換風格 | 回 Stage 2 改風格 slot，重跑 3-5 |
| 換平台 | 回 Stage 3，重跑 4-5 |
| 換主題/場景 | 回 Stage 4 brainstorm 新 logline |
| 換成本 | 回 Stage 3 重選 (cost-aware 路徑) |

---

## Stage 7 — Execution + Report

### 執行

按 [../automation/click-protocol.md](../automation/click-protocol.md) + 對應 [../automation/site-profiles/](../automation/site-profiles/) 的協議自動操作：

1. Navigate → screenshot 確認登入
2. 按 site profile 的 Phase 序操作
3. 每個 click 前 screenshot 定位
4. 遇到 **paywall / 非預期 modal / 敏感內容 → 立刻停** 問使用者

### 必停的 Checkpoint (超傻瓜模式也要停)

- ⛔ **花錢 / 升級提示** — Paywall modal 出現一定停
- ⛔ **送出前最後一次** — 如 Flow 的 → 送出箭頭、OiiOii 的「確認並繼續」
- ⛔ **下載 / 分享 / 發布** — 所有會讓結果離開工作區的動作
- ⛔ **生成有敏感內容** — 涉及真人照片 + 裸露 / 暴力 / 政治人物

其他一律自動。

### 回報

完成後展示：
- 成品路徑/URL
- 使用工具/版本
- 實際花費 (點數/$)
- 實際耗時
- 品質自評 (有沒有對齊 brief，哪裡需要再跑)
- 下一步選項 (Extend / Remix / 再試一版 / 結束)

---

## 傻瓜 UX 設計原則

1. **短 summary 而非對話** — 每階段 2-4 行，不要像 AI 助理囉嗦
2. **預設值明確寫** — 使用者看到 summary 就知道「啊原來你幫我補了這個」
3. **進度可見** — `[1/7 intent ✓] [2/7 defaults ✓] [3/7 platform: Kling 2.6 Pro ✓]...`
4. **錯了就 rollback** — 如果產出不對，按使用者的一句話回退
5. **學習偏好** — 使用者多次選某平台/風格 → 寫進 memory，下次自動偏好

---

## 範例：從「做一個動漫風格的異世界短片」到成品

### Auto-Pilot 輸出

```
[1/7 Intent Parser]
媒體=video | 長度=10s | 畫面比=16:9 | 風格=動漫(Shinkai+Ghibli blend)
主題=異世界穿越 | 角色=少女 | 場景=奇幻森林入口 | 音訊=BGM+SFX，無對白
語言=prompt 英文

[2/7 Defaults 填入]
時間光=golden hour | 色調=暖金+深綠 | 導演 token=Shinkai+Ghibli
DP token=Hoyte van Hoytema (warm-cool) | 底片=Kodak Vision3 500T

[3/7 Platform Decider]
選定：Kling 2.6 Pro T2V, 16:9, 10s
理由：動漫 + 運鏡穩 + 物理 OK，市場頂尖
備案：Seedance 1.0 Pro (多鏡) 若要 3 個 shots
成本：Kling 免費額度內

[4/7 Script + Storyboard]
Logline: 一位紅斗篷少女推開魔法森林石門，第一次踏入異世界。

Character (主角):
- 14-16 歲少女
- 長黑髮，紅色斗篷連帽
- 淺色皮製旅行裝
- 背腰小皮袋

Scene:
- 地點: 古老魔法森林入口，巨大石雕門環繞著發光符文
- 時間: 黃金時刻 golden hour
- 氛圍: 初見奇幻的驚嘆+溫柔不安

Single Shot (10s):
- 景別: medium wide → 推進到 medium close-up
- 運鏡: slow dolly-in 跟隨少女往前走
- 動作: 少女站在石門前 (1-3s) → 推開門 (3-6s) → 光芒湧出，她表情驚奇 (6-10s)
- 光影: 暖金陽光穿過樹冠 + 石門綠光 + 飄浮粒子

[5/7 Prompt Crafting]
Prompt (Kling 2.6 Pro 五段式，單段破折號連接):
"""
A young girl with long black hair and a crimson-red hooded cloak
— stands before a massive ancient stone archway carved with softly
glowing runes at the entrance of an enchanted forest at golden hour
— slowly raises her hands and pushes the heavy door open, warm
emerald light pouring out from within as she steps forward, wonder
and quiet unease on her face — slow dolly-in tracking from behind,
medium-wide gradually tightening to medium close-up
— Makoto Shinkai cinematic anime blended with Studio Ghibli hand-
painted backgrounds, Hoyte van Hoytema warm-cool contrast, Kodak
Vision3 500T emulation, volumetric god rays piercing the canopy,
floating magical dust particles, crimson cloak billowing in a soft
wind, 16:9 widescreen
"""

Negative prompt (15-tag):
deformed face, extra fingers, distorted hands, plastic skin, blurry,
jittery camera, frame stutter, low quality, watermark, text overlay,
oversaturated, photorealistic, 3D render, modern clothing, urban city

參數:
- Model: Kling 2.6 Pro
- Mode: Text-to-Video
- Duration: 10s
- Aspect: 16:9
- CFG: 0.55
- Camera Control: 關 (prompt 已寫運鏡)

[6/7 Preview]
─────────────────────────────────────────────
我要幫你做：
📽 10 秒動漫短片，16:9
🎬 Kling 2.6 Pro (免費額度內)
📝 紅斗篷少女推開魔法森林石門，首次踏入異世界
🎭 少女：長黑髮 + 紅色斗篷 + 淺色皮裝
🎨 新海誠 + Ghibli blend，Hoytema warm-cool，Vision3 500T，黃金光
📋 1 個 shot，slow dolly-in 跟隨，10s 節奏：站 → 推門 → 驚奇
💰 約 0 點 (免費) 或小額
⏱ 生成 2-5 分鐘

要繼續？回 "go" 開始，或告訴我改什麼。
─────────────────────────────────────────────

[7/7 Execution]
使用者確認 go → 打開 Kling → screenshot 確認登入 → 貼 prompt → 送出 → 監控 → 下載 → 交付
```

這就是「動漫異世界短片」的 auto-pilot 全流程。使用者只說一句話，Claude 跑完 1-6 展示 preview，使用者 go 就自動執行。

---

## 錯誤處理與降級

### 如果某階段卡住

| 問題 | Fallback |
|---|---|
| Stage 3 沒合適平台 | 退回推薦 2-3 個平台+差異，讓使用者選 |
| Stage 4 想不出劇情 | 用 3 個 archetype (英雄啟程/愛情邂逅/神秘迷霧) 之一 |
| Stage 5 沒合適 preset | 退回 cinematic-direction.md 現場組 |
| Stage 6 使用者沒確認 / 超時 30 秒 | 若「花錢」→ 停等人；若「免費」→ 繼續 |
| Stage 7 撞 paywall | 必停問使用者 (不代付) |
| Stage 7 操作失敗 | 回報並提供手動 prompt 讓使用者自己貼 |

### 反模式 (Auto-Pilot 禁止行為)

- ❌ 寄多個版本讓使用者選 — 選 1 個最佳 commit
- ❌ 問 Claude 知道的問題 (「你想要什麼畫面比例？」— 16:9 就好)
- ❌ 寫 generic prompt (`cinematic, 8k`) — 禁
- ❌ 在執行時停下問「確定嗎？」— Stage 6 確認過就不要再問
- ❌ 幫使用者付款 — 永遠不代付
- ❌ 輸出沒套 skill 語彙庫的 prompt
- ❌ **重複要求確認** — 使用者已明確授權執行且沒有新增成本或不可逆後果時，不要在每個子步驟重問。遇到付費牆、登入憑證、不可逆刪除、能力／合規不確定，或使用者主動要求 preview 時才停。
- ❌ **chain workflow 中間 screenshot/TodoWrite** — 多任務只首尾 screenshot 各 1 張，TodoWrite 跑完一次性更新。詳見 [token-efficient-mode.md §策略 8](token-efficient-mode.md)

### Chain Workflow 模式 (做 N 首歌、N 張圖)

User 說「做 5 首歌」「做 10 張圖」「連續做 X 個」時，Auto-Pilot 進**chain mode**：

1. **Stage 6 跳過** — 不要 Preview，直接 Stage 7
2. **Stage 7 用平台 chain SOP** — 各平台 site-profile §「Chain Speed Optimization」
3. **首尾驗證** — 第 1 個 task 後 1 screenshot 校準，全部完成 1 screenshot 確認
4. **內建批次優先** — 平台有 Series Mode / Multi-Shot / Multi-Reference 一次出多個 → 用內建批次而非多次 chain (Suno Persona / Kling Multi-Shot / Seedream Series / MJ omni-ref / Vidu Q3 multi-entity)
5. **5 task chain 預算** — ≤ 36 tool calls + ≤ 1.5k token + < 5 分鐘

---

## 啟動 Auto-Pilot 的 Magic Words

使用者說這些 = 啟動：
- 「幫我做 [X]」
- 「生個 [X]」
- 「做一個 [X]」
- 「我要 [X]」
- 「自動 / auto / 傻瓜模式」明確啟動

使用者說這些 = 不啟動 (保持對話式)：
- 「幫我看看 [既有檔]」
- 「解釋 [概念]」
- 「比較 [A] 和 [B]」
- 「先討論」

---

## 跟使用者 onboarding 時說的話

「你只要告訴我要做什麼，越白話越好。『做一個動漫異世界短片』、『幫我寫 Apple 廣告 prompt』、『出一張王家衛風的海報』都行。我會：自動挑平台 → 寫腳本 → 組 prompt → 給你看一下(Stage 6) → 你說 go 就幫你跑。只有在要花錢、或出了意外，我才會停下問你。」
