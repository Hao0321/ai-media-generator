# OiiOii.ai — 多智能體 AI 動畫/影片平台

> ⚠️⚠️ **本檔 §「創作模式」以下（2026-04 舊 UI）已被 2026-06-08 大改版推翻。**
> **最新 UI / 流程 / 模型清單 / 成本 / i2v / 一致性系統 / 選模型 SOP 一律以 [`automation/site-profiles/oiioii.md §0`](../automation/site-profiles/oiioii.md) 為準（SOURCE OF TRUTH）。**
> 本檔的 **prompt 寫法與創作哲學仍適用**；但 **UI 座標 / 點擊順序 / 模型名稱 / 付費結構勿照本檔**（會失敗）。改版重點：左欄圖示導航、新建直進 canvas、模型選擇器移到底部「Agent ▾」、新模型 Oii Image 2/Gemini Omni/Oii X Imagine、三大新功能（智能畫布/拉片復刻/Skill庫）。

## 它是什麼

OiiOii (官網 `https://www.oiioii.ai/`) 是一個 **多智能體協作** 的 AI 動畫/影片創作平台。你丟一張圖 (或一個想法)，它內部會派出多個 AI 角色 (藝術總監、編劇、角色設計師、場景師、剪輯師、音效師) 分工完成整部動畫。核心概念：**「Upload just one image → full music MV」**。

角色一致性靠 **DreamActor-M1** 系統（存為角色/場景/風格 → 資產庫複用，見 site-profile §0）；影片模型現支援 Seedance 2.0 pro / Kling / Vidu / Gemini Omni 等（🔴 Sora 2 已停運 2026-04-26，不再可用）。音樂與配樂則是 AI 匹配生成。

## 創作模式 (UI 頂部切換) — 2026-04 實測

**注意：OiiOii 定期改版，下方是 2026-04 實際介面。reference 任何「名稱」前先 screenshot 對照。**

實際 UI 在首頁中央 prompt 框下方有 4 個模式按鈕：

| 實際 UI 名稱 | 舊名/概念對應 | 何時用 |
|---|---|---|
| **Seedance 2.0 故事動畫** (預設選中) | ≈ Quick Video | 快速單一動畫，brief 進去直接跑，最簡單 |
| **自由畫布** (多模型 badge) | — | 多模型混用的自由工作區 |
| **劇情故事短片** | ≈ Story Video | 多場景敘事片 (3-5 shots) |
| **角色設計** | ≈ IP Creation | 建立可重用角色 |

**UI 一些觀察：**
- 首頁有大 prompt 框 (placeholder: 「拖拽/貼上 🖼️ 圖片到這裡，來試試【角色】、【風格】參考」) — 這不是純 textarea，是 **contentEditable DIV**
- 下方進階選項：`+` (新增)、`劇本` (開獨立劇本編輯)、`152 種風格` (下拉風格庫)、emoji 按鈕
- 右上「2,000 FREE」代幣顯示剩餘免費點數
- 上方常駐 banner 宣傳/活動 (如「訂閱最高享 46 折」)

## 提示詞寫法

OiiOii 不是「把 prompt 交給一個模型」，而是交給一組代理人。所以 prompt 的最佳寫法比較偏 **劇情大綱 / 創意簡報 (creative brief)** 而非傳統的「主體 + 場景 + 風格」。

### 建議結構

```
[故事概念 1-2 句]
— 發生了什麼，有誰，在哪

[角色清單]
- 角色 A：外型、個性、關鍵特徵
- 角色 B：…

[場景與時序]
- 開場：地點、時間、情境
- 發展：轉折點
- 結尾：結局或情緒定格

[風格與調性]
- 視覺風格：動漫 / 3D / 寫實 / 水墨
- 情感：溫馨 / 緊張 / 奇幻 / 懷舊
- 音樂偏好 (Anime MV 模式)：爵士 / 電子 / 鋼琴敘事 / J-pop

[特殊指定 (可選)]
- 保持角色一致性 (on by default)
- 鏡頭偏好 (如多用近景、慢節奏)
- 禁止元素
```

### 範例 1 — Anime Music Video

```
Story: A lonely teenage girl rides her bike through autumn streets at dusk,
finds a stray cat, and decides to take it home.

Characters:
- Girl: 16 years old, long black hair tied with a red ribbon, oversized beige
  cardigan, shy smile
- Cat: small, orange tabby, one torn ear

Scenes:
- Opening: empty tree-lined street, fallen red leaves, golden sunset
- Middle: girl crouching by a vending machine where the cat hides
- Ending: girl riding home with cat in her basket, first stars appearing

Style: Makoto Shinkai cinematic anime, warm autumn palette, volumetric light,
shallow depth of field.

Music: gentle lo-fi piano with light strings, ~80 BPM, nostalgic and hopeful.

Keep character consistent across all shots.
```

### 範例 2 — Story Video (IP 可重用)

```
Story: Episode 3 of our series "Neon Dumpling Shop". Tonight, a mysterious
customer orders dumplings that don't exist on the menu.

Characters:
- Chef Lin (established IP): middle-aged man, white chef coat, scar on left
  brow, always smoking a digital cigarette
- Mysterious Customer: tall figure in a black trench coat, face hidden under a
  wide-brim hat, glowing amber eyes

Scenes:
- Opening: cyberpunk Taipei night market, steam rising, neon signs flickering
- Middle: customer sits at the counter, places a folded paper napkin with a
  hand-drawn dumpling on it
- Ending: Chef Lin pauses, slowly smiles, reaches under the counter

Style: cinematic cyberpunk anime, Blade Runner color palette (teal + orange),
handheld camera, grain, practical neon.

Music: dark synthwave with Taiwanese erhu samples, ~95 BPM.
```

## 參數與選項

- **輸出長度**：短影片 (15–30s)、MV (1–3 分鐘)、Story (可多集)
- **畫面比**：9:16 (手機豎屏)、16:9、1:1，依模式不同
- **語言**：介面與 prompt 皆支援中英
- **角色一致性**：DreamActor-M1 自動處理；IP Creation 模式可把角色存入專案庫

## 實際操作流程 (2026-04 實測步驟)

### Phase 1 — 輸入 brief

1. 登入 `https://www.oiioii.ai/` — 登入後會導向 `/home`
2. **直接把 brief 貼進中央大 prompt 框** — 不需要先選模式，預設就是 Seedance 2.0 故事動畫
3. 中央框是 **contentEditable DIV** (不是 `<textarea>`)，所以 MCP 的 `form_input` 會回「Element type DIV is not a supported form input」。要用 `computer.left_click + type`
4. **輸入帶換行的文字會直接觸發送出** (Enter 鍵綁定)，URL 會立刻跳到 `/space/{uuid}`。如果想先調設定再送，要改用單行或避免 Enter
5. 要切到別的模式 (劇情故事短片 / 角色設計)，**在貼 brief 前** 點對應按鈕
6. 要加風格參考或角色圖，**在貼 brief 前** 拖進 prompt 框

### Phase 2 — 參數頁 (送出 brief 後自動出現)

左側側欄會出現 checklist + 三組必填選項：

- ✓ **激活工作流** (自動)
- ✓ **更新短片參數** (自動)
- **影片長度** (必選)：`短影片 <1min` / `長影片 ≥1min`
- **影片比例** (必選)：`橫版 16:9` / `豎版 9:16`
- **對白語言** (必選)：`英文` / `中文` / `日文`
- **確認並繼續** 按鈕 — 灰色直到三組都選完，選完變紫紅色 active

全部選好 **按一次** 確認並繼續；送出後出現粉色「信息已確認」泡泡，checklist 往下加新項目。

### Phase 3 — 多智能體管線

**左側 Panel 是 chat-like thread，不是單純的 checklist**。每個 agent 的回應會以訊息泡泡形式加在 brief 下方，而 checklist 項目 (✓ 激活工作流 / ✓ 更新短片參數 / …) 是穿插其中的狀態列。**Panel 會自動捲到最下**，所以 agent 的文字回覆往往要往上捲才看得到。

已觀察到的 agent 回應樣本 (深夜鳥 demo, 2026-04)：

> 🔥 **藝術總監** — 規劃完成
> 「導演，我收到了你最新的創意提案... 我的藝術總監團隊已經開始針對這個提案進行規劃，首先會先梳理場景的視覺基調... 稍後會讓你確認初始的場景與角色設定方向。」

OiiOii 的 agents 依序工作 (依 observed 順序)：

1. **藝術總監**：解讀 brief，給出整體視覺基調規劃 (**約 5-8 分鐘**)
2. **情緒關鍵詞 6 選 1** (實測)：6 個 grid 按鈕 + 自訂輸入框 + 隨機。觀察到的選項 (依 brief 內容 adaptive)：`夜晚 / 孤獨 / 治癒 / 溫柔 / 青春 / 🎲 隨機`。上方附各標籤的「推薦理由」短段落。選中顯示粉紅邊框，右下跳粉色確認泡泡。**座標 (1568×751)**：左上 (155,290) 中上 (290,290) 右上 (425,290) 左下 (155,425) 中下 (290,425) 隨機 (425,425)
3. **編劇**：寫完整劇本並渲染到中央畫布 (**約 3-5 分鐘**)
   - 中央畫布出現卡片「我的劇本」含場景標題 (e.g. `場景1：午夜街道 / 自動販賣機旁`) + 完整劇本文字 (旁白、動作、對白、內心 OS 一應俱全)
   - 左側問「劇本信息已完成！您滿意嗎？」+ 兩按鈕：
     - `滿意，請繼續角色設計` (主要) — 座標 ≈ (290, 405-430)
     - `我要修改` — 座標 ≈ (290, 455-478)
   - 下方說明「如果有具體的修改意見，可以直接告訴我」— 可在 prompt 框輸入修改意見
4. **角色設計師** (黃色王冠圖標) — 有 **3 層確認** 流程，每層 2-4 分鐘：
   **Step 4A — 風格選擇：**
   - 中央畫布出現每個角色卡片 (名字 + 個性描述 + 空白圖片區 + `加到資產庫` 按鈕)
   - **注意 agent 可能自主解讀 brief 裡的視覺細節**：例如 brief 寫「黑底白爪白胸斑貓」agent 實測變成「橘貓」。demo 可接受或點「我要修改」回調
   - 左側出現 **風格 8 選 1 grid + Surprise Me + 152 Style 風格庫**
   - 8 個 preset 觀察到的名稱：`Curved Flat / Gentle / Soft-Line / Luminous Li... / Ethereal / Vibrant / Soft Pastel / Nostalgic Dreamline`
   - **座標 (1568×751)** (2-row × 4-col grid)：
     - Row 1 (y≈100): Curved Flat (135,100) Gentle (235,100) Soft-Line (335,100) Luminous (435,100)
     - Row 2 (y≈255): Ethereal (135,255) Vibrant (235,255) Soft Pastel (335,255) Nostalgic (435,255)
   - 選中：粉紅邊框 + 右下跳粉色泡泡 (完整風格名，e.g. `Nostalgic Dreamline`)

   **Step 4B — 角色主圖確認：** (風格選後 2-4 分鐘)
   - 每個角色的空白圖區填入實際 artwork (站立全身圖、動漫風)
   - 左側 scroll 到底出現「「林恩、橘貓」的角色主圖已生成，請確認是否滿意？」
   - 按鈕：`滿意，請繼續生成角色概念圖` ≈ (290, 385) | `我要修改` ≈ (290, 434)

   **Step 4C — 角色概念圖：** (主圖確認後進入)
   - agent 會再跑一輪生成 (不同姿勢/表情/服裝細節概念)
   - 左側回報：規劃中 → 思考中 → 生成 → 確認
   - 再一次 `滿意` / `我要修改`
5. **場景師**：場景視覺稿 — 有 **3 層確認** 流程：
   **Step 5A — 風格選擇：** 推薦「`林恩，橘貓 的風格`」卡片 (直接沿用角色的風格，保證視覺一致) + 風格庫 152 Style + 自訂
   **Step 5B — 場景主圖確認：** 單張大圖 (如「深夜街角自動販賣機」夜景)，左側「滿意，請繼續生成場景多視圖」按鈕
   **Step 5C — 場景多視圖確認：** 從主圖衍生多個角度 (縮圖格子)，「滿意，請繼續分鏡設計」按鈕
6. **分鏡師** (藍色圖標) — **這是付費牆** (⚠️ 見下方)：
   - 「@場景設計師 邀請 @分鏡師 加入了群聊」訊息
   - 要選 **影片模型**：`Seedance 2.0 試用` / Kling / Vidu（🔴 Sora 2 已停運，舊版曾列此已移除）
   - 要選 **分鏡方案**：`多圖參考` (直接生影片，快便宜) / `宮格圖` (分鏡畫面，可控)
   - **⚠️ 兩個影片模型都會跳付費彈窗**：「模型稍貴，為您奉上 $7 (1,000 便當) 試用套餐」
     - 按鈕：`$7 小試一下` / `升級會員（不差錢）`
     - 「試用」**不是** 模型免費，而是指 **$7 套餐可試用**
   - 不付費 → 無法進到下一步 (確認並繼續按鈕點不動)
7. **剪輯師 / 音效師**：付費後才會進到這階段，組裝與配樂，最終輸出 MP4

**Click 座標漂移警告：** Phase 3 各 agent 的確認按鈕**在 scroll 狀態差異下座標會變**。實測例：角色概念圖確認按鈕時 x=290 固定但 y 從 385 → 573 → 428 橫跨 200 px。策略：**click 前永遠 screenshot 定位按鈕當前 y，別用記憶座標**。準確度比速度重要。

**節奏與監控：** 每個 agent 之間間隔 **2-5 分鐘**，但出現互動 UI 後要 **即時處理** (用 60 秒 schedule，不要 4-5 分鐘才回來，否則會錯過好幾輪)。ScheduleWakeup runtime 最小 60 秒。

每個 agent 完成後可能要使用者確認。**UI 靜止並不代表沒進展** — 藝術總監的文字回覆寫出來時左側 panel 不會自動滾到那段；要主動 scroll up 看。

### Phase 4 — 輸出

MV 通常 5–15 分鐘；Seedance 2.0 故事動畫更快 (實測 3–10 分鐘)。完成後：
- 中央畫布顯示生成的影片 (tldraw-based canvas)
- 可下載 MP4 / 用內建編輯器微調分鏡

## Prompt 工作流建議

**步驟 A — 先寫劇本大綱 (中文最省心)**
給使用者確認：發生什麼、誰在場、結尾是什麼。

**步驟 B — 轉成結構化 brief (英文較穩)**
按上方範本重寫，確保角色/場景/風格/音樂各段齊全。

**步驟 C — 送出前檢查**
- 每個角色有沒有視覺錨 (髮色、穿著、年紀、記號)？
- 場景數是不是 2–4 個 (太多會失去一致性)？
- 風格錨是不是具體 (給導演/動畫工作室名會比「anime」精準)？

## ⚠️ 付費結構 (2026-04 實測，非常重要)

**OiiOii 是 freemium：前期免費、後期付費**。這不是「贈送 2000 代幣就能做完整動畫」。

| 階段 | 成本 |
|---|---|
| Brief + 藝術總監 + 情緒 + 劇本 | 免費 (2,000 FREE 代幣範圍) |
| 角色設計 (主圖 + 概念圖 + character sheet) | 免費 |
| 場景設計 (風格 + 主圖 + 多視圖) | 免費 |
| **分鏡師 → 影片生成** | **必須付 $7 (1,000 便當) 試用套餐** 或升級會員 |
| 剪輯 / 配樂 / 最終 MP4 | 跟在付費模型之後 |

**代幣計費觀察：** 跑完前面所有階段 (角色 + 場景) 後，右上「2,000 FREE」代幣顯示**完全沒扣**。這讓人誤以為免費，但到影片這關才跳付費牆。判斷這些「FREE」代幣只在 **某些特定功能** 才扣，影片生成不在其中。

**對 demo / 評估的意義：**
- 若只要看 skill 能產出高品質「劇本 + 角色視覺 + 場景視覺」→ 免費 OK
- 若要看到「真實可播放的影片 MP4」→ **必須預算 $7 (一次性) 或訂閱會員**
- 對使用者要透明：在建議 OiiOii 之前先說明付費結構，別讓他們跑到最後才撞牆

## 創作陷阱

- **角色描述太模糊** → 多集之間臉會跑。務必給每個角色 3+ 個固定視覺錨。
- **劇情太多場景** → 短影片塞 10 個 scene 會爛。Seedance 2.0 故事動畫 1–2 個 scene，劇情故事短片 3–5 個最穩。
- **音樂指令打架** → 不要同時寫 "upbeat + melancholy"。選一個。
- **依賴英文運鏡術語** → OiiOii 的多智能體以劇情理解為主，不像 Kling/Runway 會精確執行 "dolly in"。運鏡最好用描述式 ("camera slowly follows her as she walks")。

## 瀏覽器自動化陷阱 (2026-04 實測)

- **ref 極度不穩定** — OiiOii 用 React + tldraw canvas，每次 `read_page` 或 `find` 拿到的 `ref_N` 可能在下一次動作前就失效 ("Element may have been removed")。**對策**：
  - 一 find 到 ref **立刻** click，中間不要插 screenshot 或 zoom
  - 多數情況改用 **viewport 座標** 直接 click (座標比 ref 穩)
- **accessibility tree 缺很多元素** — 語言按鈕 (英文/中文/日文) 不在 a11y tree 裡，`find` 找不到 ref；只能靠座標
- **選中狀態視覺** — 選中是 **深灰填底** (比未選深一點)，**不是** 粉色 ring。粉色 ring 是 hover focus，不要誤判
- **Screenshot 回傳 1568×708，viewport 是 1920×867** — 但 `computer` action 座標 ≈ screenshot 像素座標 (實測 click (290, 410) 能中中文按鈕)，不需要手動縮放
- **Enter 鍵觸發送出** — 在 prompt 框 type 多行文字時 `\n` 直接送出，不會停在輸入狀態
- **參數頁按鈕座標 (1568×708 screenshot)** 僅供參考，UI 會變動：
  - 短影片 ≈ (190, 200)
  - 長影片 ≈ (395, 200)
  - 橫版 16:9 ≈ (190, 310)
  - 豎版 9:16 ≈ (395, 310)
  - 英文 ≈ (157, 410)
  - 中文 ≈ (290, 410)
  - 日文 ≈ (428, 410)
  - 確認並繼續 ≈ (290, 477) — 三項選完後才 active
- **form_input 不吃中央 prompt 框** (contentEditable DIV)，要用 `computer.left_click + type`
- **Phase 2 確認並繼續 送出後 UI 不會變** — 只會出現「信息已確認」粉色泡泡，參數頁保持原狀 (凍結狀態，不可再改)。此時再 click 會跳紅色 toast「**選項暫不支援重選哦**」→ **出現這個 toast 就代表確認已送出**，別誤以為卡住再點
- **多智能體背後工作期間 UI 靜止** — 即使 agents 跑 10-15 分鐘，左側 checklist 不會加新項目、也不會有進度條。`工作中...` 左下角是唯一訊號。不要因為 UI 不動就 panic → 真卡住的徵兆是 checklist 倒退或出現錯誤 toast，而非單純不動

## 連結

- 官網：https://www.oiioii.ai/
- 深度評測 (Kingy AI)：https://kingy.ai/ai/oiioii-oiioii-ai-a-deep-dive-review-the-ai-animation-studio-that-wants-to-replace-your-entire-production-team/
- Pollo AI 評測 + 免費試用入口：https://pollo.ai/m/oiioii-ai, https://pollo.ai/hub/oiioii-review
- NavTools 簡介：https://navtools.ai/tool/oiioii-ai
