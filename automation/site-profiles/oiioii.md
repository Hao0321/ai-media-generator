# Site Profile: OiiOii.ai

**URL:** https://www.oiioii.ai/
**驗證狀態:** ✅ 2026-04-18/19 demo + 多次實測；🆕 **2026-06-08 大改版實測**（見 §0）
**平台類型:** Multi-agent 動畫/影片創作平台
**Stack:** React + tldraw canvas + **Slate prompt 框**（ref 極不穩定，座標優先；注入用 beforeinput）

---

## 0. 🆕🆕 2026-06-08 大改版完整重測（三大新功能）

**改版公告：「OiiOii 新升級，三大重磅功能全新上線」= 智能畫布 / 拉片復刻 / Skill 庫。** 整個首頁與創作介面重設計。以下實機重測。

### 新首頁佈局
- **左欄圖示導航（取代舊頂部 chip）**：發現(home) / **新建** / 專案 / 資產 / **技能(Skill庫)**
- **頂右**：活動 / 通知 / **credits（實測 20,612）PRO** / avatar
- 「晚安，導演！」+ 最近專案橫列 + **亮點功能模板庫** + 活動 banner
- **亮點功能模板**（點「開始」直接進對應 workflow）：故事劇情 / **拉片復刻(New)** / 場景設計 / 角色設計 / **商品展示廣告** / 我在世界盃現場 / 世界盃大亂鬥 / 無人機空拍 / 卡牌·放置遊戲買量 / 萌寵·懸疑·搞笑故事 / 貼紙設計

### 🔑 新創作流程（取代舊「新建專案 → 自由畫布 chip」）
```
1. 左欄點「新建」→ 直接進 /space/{uuid} canvas（即「智能畫布」，不再需要切自由畫布 chip）
2. 底部 prompt 列工具改版：+ / 劇本 / 「技能 skill ▾」/「Agent ▾」/ 風格 / 資產 / 語言 / send(粉箭頭)
3. 選模型 = 點底部「Agent ▾」按鈕（取代舊「智能模型」按鈕）→ 彈出「模型」面板：
   - 右上「Agent」toggle（= 舊「智能模型」toggle，ON=AI 自動挑模型；要指定模型先關掉）
   - 圖片 / 影片 tabs
4. inject prompt 到 Slate 框（beforeinput insertFromPaste **照舊可用**，class `_slate-area-editable_`）
5. 點 send 粉箭頭
```

**🔧 可靠的 JS 選模型法（2026-06-10 三模型連測驗證，比純座標穩）：**
座標會因 fresh space 載入時機/y 偏移落空。穩做法 = **座標點開 Agent 按鈕（~723,665）** 後，其餘全 JS（用 textContent 比對，不靠座標）：
```js
// (先座標點 Agent 開面板，因為 button 載入時 textContent 可能空，座標較穩)
// 然後 JS：
const toggle=[...document.querySelectorAll('button,[role="switch"],input')].find(el=>{const r=el.getBoundingClientRect();return r.top>360&&r.top<420&&r.left>820;}); toggle?.click(); // 關 Agent toggle
const vTab=[...document.querySelectorAll('*')].find(el=>el.children.length<=1&&el.textContent.trim()==='影片'); (vTab.closest('button,[role="tab"],div')||vTab).click();
const opt=[...document.querySelectorAll('*')].find(el=>el.children.length<=2&&el.textContent.trim()==='Kling 3.0 Pro'); opt.scrollIntoView({block:'center'}); (opt.closest('[role="button"],button,li,div')||opt).click();
// 驗證：currentModel = bottom button 含 model 名
```
⚠️ fresh space 要等 ~2.5s canvas 載入。

**🔧🔧 最可靠選模型法（2026-06-10 Wan/Kling Omni/HappyHorse 三連測最終定案）= 全 JS 迴圈開面板：**
```js
// JS 迴圈找+點 Agent 按鈕開面板（避開兩大坑，比座標穩）
for(let i=0;i<3;i++){
  const ab=[...document.querySelectorAll('button')].find(x=>{const t=(x.textContent||'').trim();const r=x.getBoundingClientRect();return (t==='Agent'||/^Agent/.test(t))&&r.top>600;});
  if(ab){ ab.click(); await sleep(800); }
  if(document.body.innerText.includes('Oii Image 2')) break;  // 驗證面板開了
  await sleep(500);
}
// 然後 toggle off / 影片 tab / 點 model（textContent 比對）/ 注入 / send
```
**兩大坑（卡很久才發現）：**
- **🐞 「Claude is active in this tab group」toast 蓋住 Agent 按鈕**（toast 在底部置中 x~655-915，Agent 在 x~723）→ 座標點 Agent 會點到 toast！每次導航 toast 重現。**JS 找 button 不靠座標就免疫**。
- **🐞 renderer 累積變重**：開過 ~10 個 space 後同 tab 的 tldraw canvas 變重 → 截圖 timeout、座標點擊建出雜框、面板開不了。**解法：開新分頁**（fresh renderer）navigate 到 oiioii.ai/home 重來，舊重 tab 可關。
- ⚠️ 別連點 Agent 兩次（座標 + JS）= 開了又關（toggle）。一種方法就好。

### 🆕 模型實測成本+品質（2026-06-10 手動模式 4 連測，credits）
| 模型 | 規格 | credits | 實測結果 |
|---|---|---|---|
| **Oii Image 2 [Best]** | 圖片海報 | **7** | ✅ **文字渲染旗艦確認**：「Hao0321 COFFEE」品牌名乾淨正確渲染（多數圖模做不到）。要圖內精準文字（海報/包裝/logo）首選 |
| **Gemini Omni** | 4s 16:9 720p | **20** | ✅ 球鞋 clean concrete hero，多參輸入 |
| **Oii X Imagine** | 6s 16:9 | **24** | ✅「低成本超寫實」確認，咖啡乾淨寫實 CP 值高 |
| Seedance 2.0 pro | (見 §12.5 盒飯 formula) | ~ | 影片旗艦 |
| 商品展示廣告 skill | 15s 16:9 **1080p** | **~1,500** | ✅ 玫瑰金腕錶精緻，但多 gate agent + 1080p，貴 75× |
> **手動模式（Agent off + 選模型）每支 7-30cr 超便宜**（圖 7cr / 短影片 20-24cr）；skill 模式貴很多（~1500cr）。要大量試/迭代 → 手動。要圖內文字 → Oii Image 2。要低成本影片 → Oii X Imagine / Gemini Omni。

### 🆕 三模型香水瓶對比（2026-06-10 同 prompt 實測 Kling/Vidu/Hailuo）
| 模型 | 結果 | note |
|---|---|---|
| **Kling 3.0 Pro** | 六角玻璃台座、乾淨棚拍、金噴頭+琥珀液、暗反光 | 乾淨產品棚拍風 |
| **Vidu Q3 Pro** | 修長瓶、紅琥珀漸層、洋紅氛圍光暈、戲劇打光 | editorial/戲劇感更強 |
| **Hailuo 2.3 Pro** | （5s）方形瓶、金蓋、圓黑台座、戲劇頂聚光、暗色極簡 | ⚠️ 見下方時長限制；agent 自動修正衝突後成功 |
- 三者都讓香水瓶清楚當 hero（禁抽象 ✓），但**打光性格不同**：Kling 偏乾淨棚拍、Vidu 偏戲劇 editorial。選模型可依想要的廣告調性。
- **⚠️⚠️ 模型時長限制不同，切模型舊時長殘留會衝突！**：**Hailuo 2.3 Pro 僅支持 5s**（殘留 6s → 「生成失敗：hailuo23pro 僅支持 5秒」）。**好消息：OiiOii agent 會抓到衝突**並跳「調整為 5 秒繼續 / 換模型」→ 點調整即可。各模型上限：Hailuo 5s / Gemini Omni 預設 4s / X Imagine 6s / Seedance 10s。切模型後檢查時長。

### 🆕 模型清單變動（2026-06-08 實測）
**圖片：** **Oii Image 2 [Best]**（超強文字控制+寫實，新旗艦，取代 GPT-Image2）/ Oii Nano Pro / Oii Nano 2 / **Oii 4o**（GPT-4o 改名）/ Midjourney niji7 / Seedream 5.0
**影片：** Seedance 2.0 pro/fast / Seedance 1.5 Pro / **🆕 Gemini Omni**（Google 模型上架了！）/ **🆕 Oii X Imagine**（新）/ Sora2（仍列，API 代理）/ Vidu Q3 Mix/Ref/Pro·Q2 / Kling 3.0 Pro/std·V3 Omni·O1·2.6 / Hailuo 2.3 Pro/Std / Wan 2.7 / HappyHorse / **Oii Agent**（智能路由）

### 自動化延續性（✅ 2026-06-10 新 SOP 實機驗證通過）

**完整驗證路徑（跑 Gemini Omni 球鞋廣告實測成功）：**
```
1. 左欄「新建」→ /space/{uuid} canvas
2. 底部「Agent ▾」按鈕 → 模型面板：
   - 點右上「Agent」toggle 關掉（粉→灰）才能指定模型（ON=AI自動挑）
   - 影片 tab → 點「Gemini Omni」（或其他）→ 底部變「⚡自由創作 ▾ + Gemini Omni ▾ + 16:9·4s·720p + 20cr」
3. Slate beforeinput insertFromPaste 注入 → domLen 增加 ✅ 照舊可用
4. send button class `_send-section_tx61o_*`（仍 _send-section_ 系列）→ 點擊
5. ⚠️ **prompt 框不會清空**（before==after），但**左側 Oii Agent 面板出現工作流**（藝術總監規劃完成→激活工作流→工作中…）= 生成已啟動。**別只看框沒清空就以為失敗，要看 agent 面板「工作中…」**
```

- ✅ **prompt 框仍是 Slate** → beforeinput insertFromPaste 注入法續用
- ⚠️ **入口變了**：舊「新建專案→自由畫布」JS 點擊鏈失效 → 改「左欄新建 → 直接 canvas」
- ⚠️ **模型選擇器位置變了**：舊「智能模型」按鈕 → 新「Agent ▾」按鈕；toggle 名稱 智能模型→**Agent**
- ⚠️ **Gemini Omni 在 OiiOii 預設 4s / 16:9 / 720p = 20 credits**（比 Flow 的 10s 短，要長要改設定）
- 🆕 新增 **拉片復刻**（上傳參考影片逐鏡復刻）、**Skill 庫**（左欄技能，可複用 workflow）、**商品展示廣告模板**（產品廣告專用，符合「產品清楚 hero」原則，待深測）
- 下方舊 §1-§12 SOP 多數仍適用（canvas/Slate/盒飯機制未變），但**入口與模型選擇器步驟以本 §0 為準**

### 🆕 Skill 庫 / 模板 = skillId 系統（2026-06-10 完整實測）
- 首頁「亮點功能」模板點開 = 帶 `?skillId=xxx` 的 space。例：**商品展示廣告 = `ecommerce_ads_skill`**（底部技能按鈕顯示「通用商品展示廣告」）。
- **🔑 架構發現：OiiOii 的 skill 跟 Claude skill 一樣有 SKILL.md！** agent 實際會「**讀取『ecommerce_ads_skill』的完整 SKILL.md 入口說明**」來知道步驟/規範/約束。= OiiOii skill 庫 = 一堆 SKILL.md 驅動的引導式 agent workflow。
- **商品展示廣告 skill = 引導式 5 步表單**（不是一鍵生成，注入 prompt 後 agent 抽參數→跳表單）：
  1. **商品名稱**（從 prompt 自動抽）
  2. **商品白底圖（可選）** — 「上傳乾淨白底圖讓 AI 精準還原；不傳則自動生成」= **i2v 鎖形狀的產品化**（呼應 quality-control §1 + i2v 流程）
  3. **商品賣點（至少兩個）**
  4. **影片時長**：15秒（直接生成精簡片，**推薦**）/ 30秒·60秒（**先生劇本再分段生成** = 多鏡頭結構化長片）
  5. **旁白/對白語言**：繁中/英/日
  → 「確認提交」→ **第二組自適應 gate**（agent 依需求智能精簡方案）：製作流程方案（同意直接生 / 修改加模特場景）→ 寬高比（16:9 TVC / 9:16 抖音小紅書 / 1:1 詳情頁）→ 光影動態細節（可選）→ **確認並開始生成** → 自動生 hero 白底圖 → i2v 動畫化 15s
- **產品廣告最佳路徑（改版後）：** 有真實產品圖 → 商品展示廣告 skill 上傳白底圖（鎖形狀）→ 5 步表單 → 生成。比手動自由畫布更結構化、更符合「產品清楚 hero」。
- **⚠️⚠️ 成本警告（實測 2026-06-10 完整跑完）：商品展示廣告 skill 一支 15s 1080p 腕錶廣告 = ~1,500 credits**（20,612→19,087）。對比**手動自由創作 Gemini Omni 4s 720p = 20 credits** → **skill 貴約 75×**！貴在：多 gate agent 推理 + 1080p Seedance 2.0 Pro 15s。成品品質：玫瑰金腕錶清楚 hero、1080p、奢華質感到位（符合禁抽象）。
- **決策：要快/省/迭代 → 手動自由創作（Agent▾→Slate注入→send）；要 1080p 精緻成品+有真實產品圖+不在乎成本 → skill。** 自動化跑 skill 要點 ~10 個「下一步/確認」button（textContent 比對找），且 1080p 生成 ~5-7 分鐘。
- 其他 skill 同理（各有自己的 SKILL.md + 引導表單）：拉片復刻 / 角色設計 / 場景設計 / 無人機空拍。底部「技能 skill ▾」可切換/疊加。
- **自動化：** 套 skill = 首頁點模板（或底部技能 skill ▾ 選）→ 進帶 skillId 的 space → Slate 注入 prompt → agent 跳表單 → JS 填表單欄位 + 點「下一步」×N → 「確認提交」。表單欄位是一般 input/contenteditable，下一步/確認提交是一般 button（用 textContent 比對找）。

### ✅ i2v 觸發改版後仍有效（2026-06-10 實機驗證）
- 右鍵 canvas 圖 → context menu **「生成影片」仍在**（+ 新增「存為角色/存為場景/存為風格」餵一致性/資產系統）。§12.10.10 SOP 不變。
- 點生成影片 → canvas-side i2v 框：**圖自動附**（多參 slot）+ **Seedance 2.0 pro 自動選** + 預設 16:9·10s·720p·**140cr**。
- 🆕 i2v 框 tabs：**文生 / 多參（多參考圖，可加圖片+影片）/ 首尾幀（first-last frame 控制）** —— 比舊版增強。
- 注入運鏡 prompt 進**右側 canvas-side 框**（contenteditable left>600，不是左 agent panel）→ beforeinput 注入 → send（i2v 框右側粉箭頭 left>980）。i2v prompt 照規則 prefix「根據圖片中的物體、畫面、風格來生成影片」+ 只寫運鏡。

### ✅ 一致性系統：存為角色/場景/風格（2026-06-10 實測，免費）
- 選中 canvas 圖右鍵 → **存為角色 / 存為場景 / 存為風格** → toast「已存為風格」→ 存進**資產庫**。
- 之後新生成用底部「**資產**」按鈕引用（= Seedance `@AssetName` 分派職責技巧的 OiiOii UI；角色鎖造型、場景鎖背景、風格鎖質感）→ 跨鏡頭一致性。
- ⚠️ **gotcha：右鍵前要先左鍵「選中」圖**，否則右鍵出的是 canvas 選單（貼上/多選/一鍵整理/添加畫板/上傳）不是圖片選單。圖片選單才有生成影片/存為角色等。

### 🆕 拉片復刻（三大新功能之一，2026-06-10 測繪入口）
- 首頁「拉片復刻」模板 → 進 canvas，5 步引導 tour（提示 1/5「上傳影片」），中央大框 +上傳，tooltip「**在這裡上傳一段影片開始複刻**」。
- 用途：**上傳一段參考影片 → 逐鏡復刻**（分析參考片運鏡/節奏/結構 → 重新生成同結構的新片）。適合「看到喜歡的影片想複刻同款運鏡/結構」。
- ⚠️ **需本地參考影片檔上傳才能跑**（用 file_upload）。本次無參考片，僅測繪入口/用途；完整逐鏡流程待有參考片時跑。

### ✅ Skill 庫 /skill 頁（2026-06-10 實測）
- 左欄「技能」→ `/skill` 頁 = **OiiOii 官方策展 skill 庫**，分類 tab：全部 / 自媒體 / 廣告行銷 / 遊戲 / 周邊設計。
- 每個 skill 卡：名稱 + 描述 + 「查看 N 個案例」+ **bookmark 收藏** + **試試看**（= 進帶 skillId 的 space）。例：無人機航拍「需上傳標註路徑的遠景圖（箭頭標起點終點）」。
- **⚠️ 無 UI 開放用戶自建/上傳自訂 SKILL.md**（這頁沒有 create/新增按鈕）。**複用方式 = bookmark 收藏 + 從創作頁底部「技能 skill ▾」選用官方 skill**。自訂 workflow 目前靠官方 skill 組合，不是自寫 SKILL.md。

### 待深測（下次）
- 拉片復刻完整逐鏡流程（需用戶提供參考影片檔上傳；入口+用途已測繪，recreation 技巧見 multimodel-video-cheatsheet.md）

---

## 1. 整體 UI 地圖

```
┌─────────────────────────────────────────────────────────┐
│ Top bar: 繁體中文選單 | 問題 | 加入會員 | 我的資產 | [代幣] │
├──────────────┬──────────────────────────────────────────┤
│ Left Sidebar │ Center Canvas (tldraw)                    │
│ - 🏠 首頁    │                                           │
│ - 📁 項目    │  [agent 產出顯示區：劇本卡 / 角色圖 /      │
│ - 👤 人像    │   場景圖 / 分鏡 / 影片]                    │
│ - 🗑 回收站  │                                           │
│ - (+) New    │                                           │
├──────────────┴──────────────────────────────────────────┤
│ Left Panel (chat-thread): agent 訊息 + 互動 UI          │
│ [prompt input / 滿意按鈕 / 選項 grid]                    │
└─────────────────────────────────────────────────────────┘
```

**關鍵：** 左側 panel 是 **chat-thread** (不是 form)，agent 訊息由上往下累加，**新內容往往在底部**，但 agent 的 narrative 回覆在上方。需要時 `scroll up` 看 agent 說什麼，`scroll down` 看互動按鈕。

---

## 2. Phase-by-phase 完整流程 + 座標

### Phase 0 — 進入首頁 (`/home`)

首頁中央 prompt 框 + 底部 4 個模式按鈕。

**互動元素 (ref 會變，座標僅供參考)：**
| 元素 | 座標 (1568×708) | 備註 |
|---|---|---|
| 中央 prompt 框 | ~(775, 275) | **contentEditable DIV**，不吃 form_input |
| Seedance 2.0 故事動畫 (預設) | ~(625, 378) | 粉框表已選 |
| 自由畫布 (多模型) | ~(755, 378) | |
| 劇情故事短片 | ~(860, 378) | |
| 角色設計 | ~(963, 378) | |
| + 按鈕 (上傳) | 左下 prompt 框內 | |
| 劇本 按鈕 | 左下 prompt 框內 | 開獨立劇本編輯 |
| 152 種風格 | 左下 prompt 框內 | 風格庫 |
| 送出箭頭 | ~(486, 325) 右下 prompt 框 | type 含 \n 也會觸發 |

### Phase 1 — 輸入 Brief

**步驟：**
1. `computer.left_click` 中央 prompt 框
2. `computer.type` 完整 brief (含 `\n` 會自動送出)
3. URL 立刻跳 `https://www.oiioii.ai/space/{uuid}`

**陷阱：**
- `form_input` 不吃 contentEditable DIV → 回錯 "Element type DIV is not supported"
- Type 帶換行會直接送出，要當心內容完整性

### Phase 2 — 參數頁 (送出 brief 後自動出現)

左側 panel 顯示 checklist + 三組必選：

**Checklist (順序)：**
- ✓ 激活工作流 (自動)
- ✓ 更新短片參數 (自動)

**三組必選 (選一個)：**

| 組別 | 選項 | 座標 (1568×708) |
|---|---|---|
| **影片長度** | 短影片 `<1min` | **(190, 200)** ★ 驗證過 |
| | 長影片 `≥1min` | **(395, 200)** |
| **影片比例** | 橫版 16:9 | **(190, 310)** ★ 驗證過 |
| | 豎版 9:16 | **(395, 310)** |
| **對白語言** | 英文 | **(157, 410)** |
| | 中文 | **(290, 410)** ★ 驗證過 |
| | 日文 | **(428, 410)** |
| **最終確認** | 確認並繼續 | **(290, 477)** (灰色 → 紫紅 active) |

**陷阱：**
- 語言按鈕 **不在 accessibility tree**，`find` 找不到，**必須用座標**
- ref_N 在 2 次 tool call 後會失效，不要先 find 再拖延
- 按鈕的 selected state = **深灰填底**，**不是粉色 ring** (粉色 ring 是 hover)
- 點 `確認並繼續` 後，UI **不會明顯變化** (參數頁凍結)，只會出現粉色「信息已確認」泡泡
- 若再點一次 `確認並繼續` → 紅色 toast「**選項暫不支援重選哦**」= **之前已送出 OK，別再點**

### Phase 3 — 多智能體互動階段 (依序)

每個 agent 2-5 分鐘出一次互動 UI。**60s polling 最好，別等超過 3 分鐘。**

#### 3A — 🔥 藝術總監 (第 1 個 agent)

**agent 訊息** (上方 chat-thread，要 scroll up 看)：「規劃完成 → 導演我收到你的創意提案...」

**互動 UI (情緒關鍵詞 6 選 1)：**
6 格 grid + 自訂輸入 + 隨機，配合 brief 內容 adaptive。

| 選項 | 座標 (1568×751) | 範例 |
|---|---|---|
| 左上 | (155, 290) | 夜晚 |
| 中上 | (290, 290) | 孤獨 |
| 右上 | (425, 290) | 治癒 |
| 左下 | (155, 425) | 溫柔 ★ demo 實測 |
| 中下 | (290, 425) | 青春 |
| 右下 (🎲 隨機) | (425, 425) | |

選中後右下出粉色泡泡 = 送出確認。

#### 3B — 🔥 編劇 (第 2 個 agent)

**agent 訊息：** 「劇本完成 → 場景 1：XXX」
**中央畫布：** 「我的劇本」卡片 + 完整劇本 (含內心 OS / 動作 / 對白)

**互動 UI：**
| 按鈕 | 座標範圍 (y 會漂) | 作用 |
|---|---|---|
| 滿意，請繼續角色設計 | x=290, y=385~430 | 通過進下階段 |
| 我要修改 | x=290, y=455~480 | 回調 |

**⚠️ 確認按鈕 y 座標會漂** — 實測 385 → 573 → 428 都出現過。**click 前必先 screenshot 定位 y**。

#### 3C — 🔥 角色設計師 (第 3 個 agent，3 層確認)

黃色王冠圖標。**有 3 個子階段：**

**3C-1 風格選擇** (8 選 1 grid + 自訂)

| 選項 | 座標 (1568×751) | 風格 |
|---|---|---|
| (135, 100) | Curved Flat |
| (235, 100) | Gentle |
| (335, 100) | Soft-Line |
| (435, 100) | Luminous Li... |
| (135, 255) | Ethereal |
| (235, 255) | Vibrant |
| (335, 255) | Soft Pastel |
| **(435, 255)** | Nostalgic Dreamline ★ demo 實測 |

下方 `+ Surprise Me` (隨機) + `風格庫 152 Style` (更多) + 自訂輸入。

選中後右下泡泡顯示完整風格名 (如 `Nostalgic Dreamline`)。

**3C-2 角色主圖確認** (生成後 2-4 分鐘出按鈕)

| 按鈕 | 座標 | 作用 |
|---|---|---|
| 滿意，請繼續生成角色概念圖 | x=290, y 漂 | ★ |
| 我要修改 | 下方 | |

**⚠️ 這階段 agent 可能自主改 brief 裡的視覺細節** — 實測寫「黑底白爪白胸斑貓」，agent 生成變「橘貓」。demo 可接受或走修改。

**3C-3 角色概念圖確認** (character sheet 生成)

中央畫布顯示每個角色的完整 character sheet (三視角 + 色票 + 特寫分解)。

| 按鈕 | 座標 | 作用 |
|---|---|---|
| 滿意，請繼續場景設計 | x=290, y 漂 | ★ |
| 我要修改 | 下方 | |

#### 3D — 🔥 場景設計師 (第 4 個 agent，3 層確認)

紅色火焰圖標。**結構跟角色設計師一樣的 3 層：**

**3D-1 場景風格**
- 推薦卡片：**「林恩，橘貓 的風格」** (沿用角色一致性) 座標 ~(290, 380)
- 風格庫 152 Style 下方
- 自訂輸入框

**最佳做法：點「林恩，橘貓 的風格」** → 場景跟角色同風格，視覺一致。

**3D-2 場景主圖確認** (生成後出圖 + 按鈕)

| 按鈕 | 作用 |
|---|---|
| 滿意，請繼續生成場景多視圖 | ★ |
| 我要修改 | |

**3D-3 場景多視圖確認** (從主圖衍生多角度縮圖格子)

| 按鈕 | 作用 |
|---|---|
| 滿意，請繼續分鏡設計 | ★ |
| 我要修改 | |

#### 3E — 🔵 分鏡師 ⚠️ **付費牆階段**

藍色圖標 (`@場景設計師 邀請 @分鏡師 加入了群聊`)。

**要選兩組：**

**影片模型 (2 選 1)：**
| 選項 | 座標 | 成本 |
|---|---|---|
| Sora 2 試用 | x=193, y=279 (舊) / y=194 (scroll 後) | **$7 解鎖** ⚠️ |
| Seedance 2.0 試用 | x=390, y=279 / y=194 | **$7 解鎖** ⚠️ |

**分鏡方案 (2 選 1)：**
| 選項 | 座標 | 用途 |
|---|---|---|
| 多圖參考 | x=290, y=378 / y=333 | 直接生影片、快便宜 |
| 宮格圖 | x=290, y=452 / y=410 | 從分鏡畫面開始、可控 |

**確認並繼續：** x=290, y 漂 (477 / 497 / 520 都出現過)

**⚠️ 付費牆機制：**
- 點 Sora 2 或 Seedance 2.0 → 跳 modal：「模型稍貴，為您奉上 **$7 (1,000 便當) 試用套餐**」
- 兩個按鈕：`$7 小試一下` / `升級會員（不差錢）`
- **不付費就無法進到 confirmation send 成功**
- OiiOii 前期免費 (角色 / 場景 / 劇本)，**影片生成關卡一定收費**

**行動原則：** 碰到這個 modal 立刻停下，**不要代付**，問使用者：(a) 付 $7 / (b) 改走 Kling/Vidu 免費模型 / (c) 暫停

#### 3F — 付費後：剪輯 / 配樂 / 最終 MP4

未驗證 (demo 暫停於此)。預計：
- 剪輯師組裝多鏡為連續影片
- 配樂生成
- 最終下載 MP4

---

## 3. 付費結構速查

| 階段 | 成本 | 驗證 |
|---|---|---|
| Brief + 藝術總監 + 情緒 + 劇本 | 免費 (2,000 FREE 代幣內) | ✅ |
| 角色設計 (風格 + 主圖 + 概念圖) | 免費 | ✅ (代幣仍 2,000 未扣) |
| 場景設計 (風格 + 主圖 + 多視圖) | 免費 | ✅ |
| **分鏡 → 影片生成** | **$7 / 1,000 便當 一次性** | ✅ 實測彈窗 |
| 會員訂閱升級 | 價格未查 | - |

---

## 4. 已驗證的 ref 不穩定證據

- `read_page filter=interactive` 回傳 ref_515 (短影片) / ref_520 (橫版 16:9) → 數秒後 click 已失效
- `find "短影片 button"` 回 ref_550 → 下次 click 也失敗
- **對策：改用座標，每 click 前 screenshot 驗證位置**

---

## 5. 常見 toast 速查

| Toast | 意義 | 對策 |
|---|---|---|
| 「信息已確認」粉色泡泡 | action 送出成功 | 繼續下步 |
| 「選項暫不支援重選哦」紅色 | 已送出過、不能再點 | 停點 |
| 「模型稍貴，為您奉上 $7...」modal | paywall | 停下問使用者 |
| 「工作中...」左下 spinner | agent 背後處理中 | 等 60s-3min |

---

## 6. 快速決定樹

```
打開 OiiOii 要什麼？
├── 快速 demo → Phase 0 預設 Seedance 2.0 故事動畫 + 短影片/16:9/中文
├── 多場景故事 → 切「劇情故事短片」模式
├── 角色 IP → 切「角色設計」模式
└── 圖像實驗 → 切「自由畫布」

到分鏡師付費牆前：
├── 使用者想看成品 → 問他付不付 $7
├── 使用者只要角色/場景設計 → 下載資產後結束
└── 使用者想完整影片但不付費 → 把資產匯出，改走 Kling I2V / Runway Gen-4 Refs 免費/低成本生影片
```

---

## 7. Workspace 永久保存

每個 brief 送出後會拿到 URL `https://www.oiioii.ai/space/{uuid}`。這個 URL 永久保存所有產出 (劇本、角色圖、場景圖)，使用者登入後隨時回來繼續。demo 中的 space: `e9ae7550-9ea1-42a8-8ee9-072057ee2a02`。

---

## 8. 代幣監控

右上「2,000 FREE」顯示剩餘免費代幣。demo 完整跑到分鏡師 (含劇本 + 2 角色 + 3 場景圖) 後代幣 **完全未扣** — 確認免費階段真的免費，付費在分鏡師 gate。

---

## 9. 未驗證但值得注意的行為

以下是 demo 沒遇到但別的使用者可能碰到的：

- 上傳參考圖到 prompt 框的 drag-drop 區
- 「劇本」按鈕打開獨立劇本編輯器 (可能有自己的 UI)
- 分享按鈕 (Phase 1+ 右上) 的權限設定
- 若 agent 某步卡住 5 分鐘+，reload 頁面能否從斷點續
- IP Creation (角色設計) 模式的完整流程

發現時補進本檔。

---

## 10. 版本與更新紀錄

- **2026-04-18/19 demo：** 完整跑 Phase 1-3E，撞上分鏡師付費牆停。座標以當時 1568×708/751 viewport 為準。
- **2026-04-20 深夜鳥 project：** Story Video 完整流程跑完 (3 場景 + BGM)，確認 STAR 費 (3 scene 480p Fast = 232 STAR)，解鎖預覽與匯出。匯出 1080p = 1 盒飯/2s (獨立 credit system)。
- **2026-04-20 自由畫布探索：** 測完全部 24 模型、settings panel、盒飯 cost formula — 見 §12。
- **2026-04-20 Seedance 2.0 pro 實戰驗證：** 賽博武士 15s 720p 戰鬥生成成功 — 210 盒飯。發現 `智能模型` toggle **必須關掉** 才能強制使用指定模型，否則 agent 自動 override 到 Sora 2（見 §12.4）。generation 時間 ~3 min，output 品質驚人（chrome 武士 + 電漿刀光 + 火花爆散 + 霓虹街景）。

---

## 11. ⚡ Chain Speed Optimization (OiiOii 連跑多劇本)

**OiiOii 比較特殊 — 是 agent workflow 不是純 prompt 工具。chain 機會少 (通常 1 個劇本 = 1 個專案)，但**選項點選**階段可以省。

### 選項 chain SOP

**重要陷阱：** OiiOii 是 React/tldraw，**ref 易失效**。每個選項點之間：
- ✅ screenshot → 找新 ref → 立刻 left_click (不插任何中間 tool call)
- ❌ find → screenshot → click — **ref 已失效**

**選項 click 流程 (比 Suno 慢但可靠)：**
```
1. screenshot                  → 看當前狀態
2. find <選項描述>             → 拿 ref
3. left_click ref              → 立刻點，無中間步
4. screenshot 驗證 (此網站必須驗 — selected = 填底色非 hover)
5. 點下一步 / 確認按鈕
```

**禁忌：**
- ❌ 跳過 screenshot 驗 selected 狀態 — pink ring = hover 不是 selected
- ❌ 「選項暫不支援重選」toast 出現 → **已提交，不要再點**，不是失敗
- ❌ TodoWrite 每選項一次

**何時可以 chain：** 多個劇本獨立創建時，1 個劇本 1 次完整流程；劇本間沒共享 state，可以連跑但不要多 tab 並行。

**內容長度：** OiiOii 劇本可長可短，1000 字內最佳。

**預期效能：** 單劇本到分鏡師 (撞付費牆) ~3 分鐘 + 800 token
- **下次使用前必做：** navigate → screenshot → 對照本檔 UI 地圖，若配置差很多 (UI 改版)，更新此檔。

---

## 12. 自由畫布 (Free Canvas) 完整模式 ★ 2026-04-20 新驗證

**定位：** OiiOii 的多模型聚合入口 — 24 個頂級 AI 模型一站式切換，是 OiiOii 區別於 Flow/Kling 單平台的最大賣點。不走 agent workflow，是**純 prompt + 模型 + 參數**形式 (像 Midjourney / Seedream / Flow 混合版)。

### 12.1 進入方式

從首頁底部 4 模式按鈕選 **「自由畫布 (多模型)」** (~755, 378) → URL 變 `/space/{uuid}` 但空間類型是自由畫布 (不是 agent workflow)。

### 12.2 UI 佈局

```
┌─ Left (chat panel, ~520px wide) ─┬─ Right (tldraw canvas) ────┐
│ 🎨 藝術總監                       │                              │
│ 「自由選擇模型，快速生成圖片或視訊素材」│                              │
│                                   │   [Empty Frame / generated  │
│ [prompt textbox]                  │    results stacked here]    │
│ [+] [📊 model] [👤] [😊] [⋯= settings]│                          │
│                   [🍱 X 盒飯] [➤]   │                              │
└───────────────────────────────────┴──────────────────────────────┘
```

### 12.3 底部工具列 5 icons (核心座標)

| Icon | 座標 (1568×708) | 功能 | 備註 |
|---|---|---|---|
| `+` | (107, 673) | 附加 / 上傳參考圖 | 未深驗 |
| `📊` bars | (137, 673) | **模型選擇** | 開 dropdown |
| `👤` | (166, 673) | 角色 / 人像 | 未深驗 |
| `😊` | (196, 673) | 風格 | 未深驗 |
| `⋯=` sliders | (225, 673) | **比例/時長/解析度設定** ★ | 影片模型才顯示時長/解析度 |
| 🍱 cost | (~450, 673) | 當前生成成本 (盒飯) | 動態更新 |
| `➤` send | (486, 673) | 送出生成 | |

### 12.4 模型 dropdown — 24 個全列表

**結構：** 上方 `圖片` / `影片` 切換 tab + 右上 `智能模型` toggle (ON = 由 AI 挑模型)。toggle OFF 才顯示全模型並自選。

**⚠️ 關鍵陷阱 — 智能模型 toggle (2026-04-20 實測)：**

`智能模型` toggle 位置：模型 panel 右上角 ~(313, 400)，預設 **ON**（粉色滑桿）。

**ON 行為：** 即使你手動點選某模型（如 Seedance 2.0 pro），`自由畫布 藝術總監` agent 在 planning 階段 **會無視你的選擇** 自己挑一個（常挑 Sora 2）。你看到的是 "使用Sora2生成影片" 而不是你選的。

**OFF 行為：** toggle 關掉後（灰色），agent 嚴格使用你在模型列表點選的那個（會顯示 "使用Seedance 2.0生成影片"），且 Sora 2 的 ✓ 會取消，由你點的模型接手。

**SOP：要指定特定模型 → 先 click toggle OFF → 再點模型 → 再 submit。** 這是強制步驟，跳過 100% 被 override。

**驗證 log：** 2026-04-20 跑賽博武士 15s 戰鬥 prompt 3 次都被 override 到 Sora 2，直到關 toggle + 手點 Seedance 2.0 pro 才成功用對模型（規劃步驟顯示 "使用Seedance 2.0生成影片" ✓ 綠勾）。

#### 🖼 圖片 (8 個)

| 模型 | Badge | 定位 |
|---|---|---|
| Nano 2 | | 速度快，綜合生圖最好 |
| Midjourney niji7 | | 動漫美學最佳，無角色參考 |
| **Nano Pro** | Best | 綜合生圖能力最好 |
| Seedream 5.0 | | 最懂亞洲人美學，理解能力更強 |
| Midjourney niji6 | | 動漫美學效果最佳 |
| NovelAI | | 擅長動漫和插畫風格 |
| Seedream 4.5 | | 默認 2K，最懂亞洲人寫真 |
| Gpt 4o | | 輸入理解最好，生成速度慢 |

#### 🎬 影片 (16 個)

| 模型 | Badge | 定位 |
|---|---|---|
| **Seedance 2.0 pro** | Best | 當下最強影片生成模型 |
| Seedance 2.0 fast | | 趨近 Pro，性價比優選 |
| Sora 2 | | 綜合最佳效果，精準控制 |
| Vidu Q3 Mix | New | 多參增強版，音畫同出 |
| Wan 2.7 | New | 畫面升級，智慧切削鏡 |
| Vidu Q3 Ref | | 參考一致性，音畫同出 |
| Kling 3.0 Pro | | 超清畫質，3D/寫實內容生成較好 |
| Kling V3 Omni | | 多元素參考 |
| Hailuo 2.3 Pro | | 角色動態效果流暢、生動 |
| Wan 2.6 | | 多鏡頭敘事，音畫同出 |
| Kling 3.0 std | | 3D/寫實內容生成較好 |
| Kling O1 | | 多元素參考，激發想像力 |
| Seedance 1.5 Pro | | 高質量視頻生成，運鏡控制力強 |
| Hailuo 2.3 Std | | 角色動態效果流暢、生動，高性價比 |
| Kling 2.6 | | 高質量生成，音畫同出 |
| Vidu Q2 | | 多參啟動 |

**OiiOii 最大賣點：** 不用切 Flow / Kling / Vidu 三個網站就能切換它們的模型 — **一個帳號一個 prompt 直接比較**。圖片類 Nano Pro + Seedream + MJ niji7 一站到位省切站時間。

### 12.5 設定 Panel (⋯= icon 開啟) ★ 影片模型必看

**觸發：** 選好影片模型後 → click sliders icon (225, 673) → 彈出設定 panel (~220-525 x, ~430-665 y)

**三組設定：**

| 類別 | 選項 | 備註 |
|---|---|---|
| **比例** | 16:9 / 9:16 / 1:1 / 4:3 / 3:4 / **21:9** | 21:9 **只 Seedance 2.0 fast 有**，pro 沒有！ |
| **時長** | 4-15 秒 (可選)，- / + 按鈕，1s 一階 | 預設 10s |
| **解析度** | 480p / 720p slider | **1080p 要另外從 Library 匯出** |

**盒飯 cost formula (Seedance 2.0 實測)：**

| 模型 | 480p | 720p |
|---|---|---|
| Seedance 2.0 fast | **5 盒飯/秒** | 11 盒飯/秒 |
| Seedance 2.0 pro | 7 盒飯/秒 | 14 盒飯/秒 |

- 例：fast 10s 480p = 50 盒飯 (實測 55 at 11s)
- 例：pro 11s 720p = 154 盒飯
- **480p vs 720p ≈ 2.2x**
- **pro vs fast ≈ 1.4x** (pro 40% 貴)

**省錢鐵則：** 測試用 **fast + 480p + 10s = 50 盒飯** 最便宜。滿意再用 pro + 720p。

**其他模型的 cost formula 待驗** — Kling 3.0 Pro / Vidu Q3 Ref 預計更貴（🔴 Sora 2 已停運，不再可用）。

### 12.6 盒飯 vs STAR 雙貨幣系統

OiiOii 有 **兩套 credit system**：

| 貨幣 | 用途 | 取得 | 驗證 |
|---|---|---|---|
| **STAR** | Story Video agent workflow (Phase 0-3E) | 免費 + 訂閱加購 | ✅ 4,968 remaining after 深夜鳥 (232 用) |
| **盒飯** | 自由畫布生成 + 1080p 匯出 | 訂閱 / 日常贈送 | ✅ 100 盒飯 (3 天到期)，有限非 credit 池 |

**注意：**
- STAR ≠ 盒飯 — 不互通
- 盒飯**有到期日** (3 天)，用不完浪費
- STAR 免費 tier 充足 (2000-5000)，盒飯往往是瓶頸
- 1080p 匯出**獨立收 1 盒飯/2s** (Story Video 免費跑但匯出付費)

### 12.7 Chain Speed Optimization — 自由畫布專用

**適用情境：** 5-10 個獨立 prompt，同一模型，探索主題。

**最佳 SOP：**
```
前置 (1 次)：
1. navigate /home → click 自由畫布
2. screenshot 確認 UI
3. click sliders icon → 設定 fast/480p/10s (最便宜)
4. click bars icon → 選 Seedance 2.0 fast (或目標模型)

每 prompt (chain, 理想 ≤ 4 calls)：
1. (若需清空) triple_click prompt textbox
2. type <new prompt>
3. click ➤ send (486, 673)
4. wait 60-90s + screenshot (每 2-3 prompts 一次，非每次)
```

**禁忌：**
- ❌ 每生成後 screenshot — 浪費 1.5k token
- ❌ TodoWrite 每 prompt
- ❌ 重新開設定 panel 每次 — 設定會記住
- ❌ 混切模型 — 每切模型設定會 reset

**並行替代：** 單 prompt 不能 batch 多張 (不像 Ideogram 一次 4 張)。想多樣 **寫多個 prompt 連發** 最快。

**預期效能：** 5 prompts × 10s 480p = 250 盒飯，~8 分鐘，< 2k token。

### 12.8 未驗證 (下次探索補)

- ⏳ `+` icon 上傳參考圖的流程
- ⏳ `👤` 角色 icon — 是否有 character library
- ⏳ `😊` 風格 icon — 是否跟 152 種風格庫連動
- ⏳ 152 種風格入口在自由畫布位置 (初畫面有，選模型後消失)
- ⏳ 圖片模型 (Nano 2 / Seedream 5.0 / MJ niji7) 的盒飯 cost — 可能更便宜 (沒時長維度)
- ⏳ Sora 2 / Kling 3.0 Pro / Vidu Q3 Ref 的 cost formula
- ⏳ 1 次 prompt 能否同時產多張 (變體)
- ⏳ tldraw canvas 上的 Frames 是否能拖拉編輯、加 text
- ⏳ 「多選」「添加畫板」按鈕功能

---

### 12.9 單次生成「指定模型」黃金 SOP ★★★ 2026-04-20 實戰優化

**情境：** 用戶指定特定模型 + 特定規格（如「Seedance 2.0 pro 720p 15秒」）— 必須**確保 agent 不 override**、一次到位。

#### 🥇 7 步流程 (< 10 calls，純點擊 < 30 秒 + 等待 ~3 min)

```
1. (若未在 space) navigate 到 /home → click 自由畫布入口
   └─ 若已在 /space/<id> 且在自由畫布模式 → 跳過
2. click 📊 模型 icon (137, 673)        # 打開模型 panel
3. click 智能模型 toggle OFF (313, 400) # ⚠️ 灰色 = OFF，絕對不能省
4. click 目標模型 (Best 標籤的在列表最上)
   └─ Seedance 2.0 pro = 第 1 項 ~(225, 485)
   └─ Seedance 2.0 fast = 第 2 項 ~(225, 530)
   └─ panel 會自動關閉，bottom toolbar 顯示該模型的設定 icon
5. click ⋯= sliders icon (225, 673)    # 打開設定 panel
6. 按 + button (440, 620) 直到時長 = 用戶要求或模型上限
   └─ 每次 click = +1s (非跳 4/8/12)
   └─ 最大值到時 button 變灰
   └─ 720p button ~(415, 620) 若要高解析度 click
7. click 空白處 (1000, 200) 關設定 panel # ⚠️ 設定會覆蓋 textbox，必須先關
   → click textbox (290, 587)
   → type <prompt>
   → click ➤ send (486, 673)
```

#### 📊 模型上限速查（避免「要 20s 被 cap 到 15s」之坑）

| 模型 | 時長 | 解析度 | 比例 | 720p 成本 | 適用 |
|---|---|---|---|---|---|
| **Seedance 2.0 pro** | 4-15s 連續 | 480p/720p | 5 種 (無 21:9) | 14 盒飯/s | 品質優先 |
| **Seedance 2.0 fast** | 4-15s 連續 | 480p/720p | 6 種 (含 21:9) | 11 盒飯/s | 性價比 |
| **Seedance 1.5 Pro** | 4-12s 連續 | 固定 (單 preset) | 3 種 | 5 盒飯/s | 省錢 |
| **Sora 2** | 固定（待驗） | 2 種 | STAR/盒飯 待驗 | 高 | 精準控制 |
| **Vidu Q3 Mix/Ref** | up to 16s | 含音訊 | 待驗 | 待驗 | 長片 + 音畫同出 |
| **Kling 3.0 Pro** | 待驗 | 超清 | 待驗 | 待驗 | 寫實 |

**用戶要超過 15s？** Seedance 系列都不行 → 改 Vidu Q3 (16s) 或 Seedance fast × 2 段接續。

#### ⚠️ 三大陷阱速避（本次踩過才寫出來）

1. **智能模型 ON = 手選被 override**
   - 症狀：你選 Seedance 2.0 pro，agent "思考完成" 卻顯示 "使用Sora2生成影片"
   - 對策：**每次進模型 panel 第一件事就是確認 toggle 是灰色**
   - 代價：一次 override 浪費 70-210 盒飯 + 3 分鐘

2. **設定 panel 覆蓋 textbox**
   - 症狀：type 文字沒進 textbox，畫面看不出反應
   - 對策：**開設定 → 調完 → 必先 click 空白處關 panel → 才 click textbox**
   - 替代：只調設定不 type 時可保持 panel 開

3. **成本顯示延遲**
   - 症狀：選模型後 cost badge 看起來是舊值
   - 對策：開一次 sliders panel（+ / - 按一下）→ 右下 cost 才跳到正確值
   - 不要在沒打開 sliders 就相信當前 cost

#### ⏱ 時間預算

| 階段 | 時長 | 備註 |
|---|---|---|
| 點擊階段（7 步） | ~30s | 不含等待 |
| Seedance 2.0 pro 15s 720p | ~2.5-3 min | 同 prompt + setup |
| Seedance 2.0 fast 15s 480p | ~1-1.5 min | 約 pro 一半 |
| Sora 2 同規格 | ~2-4 min | 待驗 |
| **total per generation** | **~3-4 min** | 含全部 |

#### 🔁 Chain 多次同模型（2+ 次）優化

模型 + 設定會**記住**，chain 中每次只需：
```
1. click textbox (290, 587)
2. triple_click 清空
3. type <new prompt>
4. click ➤ (486, 673)
[wait ~3 min，全部 chain 結束後 1 次 screenshot]
```
省掉 toggle / 模型 / sliders 的 4 次 click = 每 prompt 省 ~6 秒 + 省 token。

#### 💸 預算建議

- **測試階段：** Seedance 2.0 **fast** + **480p** + 10s = **50 盒飯** 最便宜
- **品質階段：** Seedance 2.0 **pro** + **720p** + 15s = **210 盒飯** 上限
- **免費 tier：** 每日 60-100 盒飯配額 → 品質一次 = 一天的配額，測試一次 = 半天
- **1080p 匯出**：Library → 額外 1 盒飯/2s（**不是**生成時就含 1080p）

---

### 12.9.5 ⚡ Speed v2：單次生成 tool-call 最小化 (2026-04-20 實戰優化)

前版 §12.9 SOP 驗證 ≈ 11 min + 1.5k token。分析發現 **~40% 浪費在等待迴圈 + 多餘驗證 screenshot**。這是 v2 削刀版。

#### 🔪 五刀優化

| 刀 | 舊作法 | 新作法 | 省 |
|---|---|---|---|
| **① 等待** | `wait 10s` × 18-27 次 = 20+ tool calls | `Bash "sleep 180"` 單次（max 600s 可用） | ~25 calls |
| **② 驗證 screenshot** | 每步確認（開 panel / 調滑桿 / 打完字 / 送出後）= 7 張 | 只首尾 2 張 | ~5 calls + 0.4k token |
| **③ Zoom 驗證 toggle** | zoom 看 switch 顏色 | **信號法**：cost badge 不變 = 模型/設定未變 | -1 call |
| **④ Prompt 長度** | 400+ 字 English 美圖段 | **180 字 shot-list 腳本**（§12.9.6） | -0.3k token + 10s type |
| **⑤ 關 panel** | `click (1000, 200)` → `click textbox` | **✅ 實戰驗證：panel 不會自動關**，必須先 click 畫布空白處（如 `800, 300`）→ 再 click textbox。原預想的「textbox click 自動關 panel」是錯的。 | 0（rule stays 2-step） |

#### ⏱ 時間 / token 預算比較

| 情境 | Tool calls | 時間 | Token |
|---|---|---|---|
| §12.9 v1（未優化） | ~40 | ~11 min | ~1.5k |
| **§12.9.5 v2（單次優化）** | **~12** | **~6 min** | **~0.7k** |
| **§12.9.5 v2 Chain（第 2+ 生成）** | **~5** | **~4 min** | **~0.3k** |

#### ✂️ Chain-ready minimal flow（第 2+ 生成同模型同設定）

**注意 — 生成後 `時長` 可能被重置回預設 10s**（2026-04-20 驗證：140 → 210 生成後 → 回 140）。ratio/res 會保留，**只有時長需要重調**。

```
[若時長被重置回 10s]
1. click sliders (225, 673)
2. + × N 到目標時長 (462, 560)
[關 panel — 必要]
3. click 畫布空白處 (800, 300)  # ⚠️ panel 不會因 textbox click 自動關
[type + send]
4. click textbox (290, 605)     # y 用 605（590 以上可能仍在 panel 範圍）
5. ctrl+a → Delete              # 或 triple_click，清空舊內容
6. type <new prompt>
7. click send (486, 673)
8. Bash sleep 180 &             # 背景等 3 min（1 call 取代 18× wait）
9. screenshot                    # 驗收
```

= **~9 tool calls** 含重調時長；若時長保留則 **~6 calls**。

**座標精修（2026-04-20 驗證）：**
- textbox 安全座標 = `(290, 605)` 不是 `(290, 587)` — 587 在時長 panel 範圍內
- 畫布空白處關 panel = `(800, 300)` 最穩（不點到工具列、不點到視頻縮圖）

#### 🧨 禁忌再強調

- ❌ 不要用 10s wait 迴圈 — 每個 wait 是獨立 tool call，開銷巨大
- ❌ 不要每次操作後 screenshot — cost badge 已是狀態信號
- ❌ 不要 zoom 看 toggle — 相信 §12.9 前置狀態
- ❌ 不要寫 400+ 字 prompt — 藝術總監會重寫，長寫 = 白工 + 浪費 type time

---

### 12.9.6 🎬 Elite Prompt Engineering：Shot-List 腳本法 (2026-04-20 質感革新)

**病根：** 寫「美圖羅列段」→ 模型把 15s 當單鏡處理 → 鏡位單一 / 戰鬥鬆散 / 廣告沒節奏。

**解法：** 寫成 **shot-list 腳本** + 時間戳 + 強攝影語 + 類型專用動詞庫。

#### 🧱 通用結構模板

```
[HEADER]  genre + aspect + duration + palette + lens/film stock
SHOT 1 [0-3s]   type | subject | action | camera | VFX
SHOT 2 [3-7s]   ...
SHOT 3 [7-11s]  ...
HERO  [11-15s]  money shot + brand/beat moment
[LIGHTING]  key/fill/rim/practicals
[TEXTURE]   macro noise + surface specifics
[MOTION]    speed ramps, match cuts, whip pans
```

**關鍵：** 模型吃到 `SHOT 1/2/3/HERO` 標籤會**自動切鏡** — 這是 unlock 多鏡位的核心。

#### 📹 通用攝影 / 鏡頭 / 底片詞庫

`dolly push-in` `crane reveal` `whip pan` `orbit` `speed ramp` `time remap` `match cut` `rack focus` `split diopter` `over-the-shoulder` `low-angle hero` `Dutch tilt` `handheld breathe` `anamorphic 2.39:1` `ARRI ALEXA` `Kodak 500T grain` `35mm T1.4 prime` `85mm macro` `vintage cooke anamorphic bloom` `practical backlight` `Villeneuve amber` `Fincher teal-orange` `Deakins golden`

#### ⚔️ 戰鬥類強 token 庫

**編舞動詞**（取代弱的 "clash"）：
`parry → riposte` `bind + disengage` `reversal` `hilt smash` `footwork pivot` `kill-shot freeze`

**impact VFX：**
`bullet-time freeze at impact` `blade arc trail` `shockwave ripple` `kinetic spark burst` `ember cascade` `armor plate deflect` `motion blur tracers`

**攝影語：**
`whip-pan follow blade` `handheld shake on impact` `speed ramp into final blow` `72fps high-speed on strike` `low-angle hero silhouette`

**聽覺 cue（幫模型 time beat）：**
`sub-bass thud on impact` `steel ring` `breath fog` `cloth tear`

#### 🥤 廣告類強 token 庫（飲料 / 產品）

**4 幕結構：** `cold open tease → pour crescendo → splash freeze → product moneyshot + end-card`

**商業語：**
`cold open product tease` `rim-lit bottle silhouette` `condensation beads rolling` `pour crescendo` `crystalline ice crackle` `splash crown freeze at peak` `honey-amber ribbon swirl` `citrus twist cascade` `mint leaf drift` `droplet ping on wet marble` `push-in macro on label` `rack focus from bokeh to logo` `dew glint catch-light` `end-card negative space for logo`

**聽覺 cue：**
`ASMR pour sound` `satisfying pour-splash-reveal beat` `ice crackle foley`

#### ✍️ 模板示範：冰紅茶 15s（180 字 shot-list）

```
15s luxury iced black tea commercial, 16:9 anamorphic 2.39,
warm honey-amber palette with cool marble contrast,
35mm T1.4, Kodak 500T grain, ARRI ALEXA clean highlights.

SHOT 1 [0-3s] extreme macro close-up, frosted amber bottle silhouette
rim-lit by window haze, condensation beads rolling, slow dolly-in.

SHOT 2 [3-7s] overhead medium, hero pour — deep ruby tea cascading
over crystalline ice into chilled crystal glass, splash crown freeze
at peak, honey-amber ribbons swirl, 72fps speed ramp, ice crackle.

SHOT 3 [7-11s] side macro, sunlit lemon twist tumbling, mint cascade,
droplet ping on wet black marble, whip-pan transition.

HERO [11-15s] push-in rack focus from bokeh to label sharpening,
single dew droplet glints on backlight, beat drops on reveal,
end-card negative space for logo.

Rim-lit sunbeam key, marble bounce fill, amber kitchen bokeh practical.
```

#### ✍️ 模板示範：戰鬥 15s（180 字 shot-list）

```
15s cyberpunk samurai duel, 16:9 anamorphic 2.39,
neon magenta-cyan palette over rain-slick asphalt,
35mm T1.4, Kodak 500T grain, handheld breathe.

SHOT 1 [0-3s] low-angle hero silhouette, chrome-clad samurai steps forward,
breath fog, plasma katana hum ignites, rim-lit by alley neon.

SHOT 2 [3-7s] whip-pan follow blade — first parry and riposte against
mech-ninja tungsten sword, kinetic spark burst, bullet-time freeze at
impact, blade arc trail, 72fps on strike.

SHOT 3 [7-11s] handheld shake, bind + disengage, armor plate deflect,
shockwave ripple, ember cascade from clashing edges.

HERO [11-15s] speed ramp into final blow, hilt smash reversal, kill-shot
freeze on blade-light flare, sub-bass thud, motion blur tracers.

Neon magenta key, cyan fill from wet asphalt bounce, practical sign haze.
```

#### 🔑 三不寫（訊號汙染）

- ❌ `cinematic` / `ultra-photoreal` / `4K` — 早成雜訊，換成**具體鏡頭 + 底片名**
- ❌ 單段落羅列形容詞 — 模型會當單鏡處理
- ❌ 沒時間戳 — 模型無法切鏡節奏

#### 🧪 驗證成本

Elite prompt 不會增加生成成本（同 210 盒飯），**質感提升在模型側**。首次 token 多寫 ~50，但省下「結果爛再重跑」的 210 盒飯 + 4 min。

---

### 12.9.7 🎭 角色參考（資產庫）特殊流程 (2026-04-21 實戰發現)

**發現情境：** 用 😊 icon (195, 673) → 我的資產 → Characters tab → 點角色附到 prompt → submit。
**與純文字最大差別：** **不是**直接呼叫 Seedance，而是觸發 **3-agent serialized pipeline**。

#### 🔬 Pipeline 解剖

```
[你 submit prompt + char ref]
  ↓
① 圖片理解 agent     — 讀角色主圖 → 產出文字描述（canvas 上顯示 "圖片理解中"）
  ↓
② 👑 角色設計師 agent — 生 3-view turnaround sheet（正/側/背 + 服裝/髮型細節）
  ↓
③ Seedance 2.0 pro   — 用 char + turnaround 做 character-consistent video
  ↓
[結果：canvas 多出 turnaround image + 影片，左側 "已成功產生影片"]
```

**visual cue：** canvas 會長出 "👑 角色設計師" 節點 + turnaround sheet image（有「同步資產庫」按鈕可存起來重用）

#### ⏱ 時間 / 成本對照

| 模式 | 時間 | STAR 消耗（15s pro） | 備註 |
|---|---|---|---|
| 純文字 → Seedance | 4-6 min | 210 | §12.9 baseline |
| **首次用某角色 ref** | **~15 min** | **110**（實測 4,438→4,328） | pipeline serialized，pro 單價反而低？待復核 |
| 同角色重用（理論） | ~6 min | ~210 | turnaround 已存資產庫 → 跳過設計師 step（⏳ 待驗證） |

**為什麼實測 110 STAR 而不是 210？** 待查。可能是：
- (a) 角色 ref 流程走不同定價（底價 + 設計師贈送？）
- (b) 某次生成時長被系統降級
- (c) 觀察誤差（前一次 balance 記錯）

#### 🧭 等待策略（不要 timeout，不要中間亂戳）

```
submit → Bash sleep 300 (5 min) × 2-3 輪
→ 每輪 1 screenshot 確認：
   ① STAR 變化？（Seedance 已執行）
   ② 左側 panel "已成功產生影片"？
```

**⚠️ 第一次用某角色必慢** — 別 sleep 180 就 panic 以為卡住，**給它 15 min**。

#### ✍️ 角色 ref prompt 寫法

- ✅ `featuring <Name>` / `<Name> does X` — 引用 ref
- ✅ 直接寫動作 + 場景 — ref 會處理外觀
- ❌ 別重寫外觀形容 — `a man with short black hair in black t-shirt` 會與 ref 衝突 → 生成混亂

**範例（實戰驗證過）：**
```
15s gritty realistic urban combat featuring Hao, 16:9 anamorphic 2.39,
cold desaturated steel-blue palette with amber streetlight warm contrast,
35mm T1.4, Kodak 500T grain, ARRI ALEXA clean highlights, handheld breathe.

SHOT 1 [0-3s] low-angle hero silhouette of Hao on rain-slick asphalt alley,
breath fog in cold night air, cracked knuckles clench, over-the-shoulder
reveal of attacker across the alley, neon signage bleed.
SHOT 2 [3-7s] whip-pan into engagement — Hao throws tight jab-cross combo,
attacker parries and counters with elbow, handheld shake on impact...
...
Photoreal, no stylization.
```
→ 結果：Hao 臉部與 ref 一致，光影電影感，蹲姿 hero frame 成功。

#### 🎯 決策樹

```
要做角色一致影片？
├─ 首次 → 自由畫布 + 資產庫選角色 → 等 15 min，產出 video + turnaround
├─ 同角色再做 → 同畫布連跑（turnaround 已有）→ 理論 6 min
└─ 不在乎特定長相 → 別用 ref，純文字 4-6 min 即可
```

---

### 12.9.8 🛠 Prompt 工具列完整拆解 (2026-04-21 實戰探勘)

**自由畫布底部 prompt 工具列，左到右 5 個 icon：**

| 座標 | Icon | 功能 | 說明 |
|---|---|---|---|
| (107, 673) | **+** | **選擇圖片** | 上傳 ref 墊圖，支援 `.jpeg` `.png` `.jpg`，開系統 file picker |
| (137, 673) | **ll (直條)** | **模型選擇器** | 開 dropdown，有 `圖片` `影片` tabs + 智能模型 toggle + Best 標 |
| (166, 673) | **🎨 (圓圈相扣)** | **風格庫** | 34 風格 filtered by model（**影片模型全部不支援**）|
| (195, 673) | **😊** | **我的資產** | 角色 / 場景 ref（§12.10 詳述）|
| (225, 673) | **≡ (sliders)** | **參數設定** | duration / aspect / resolution |

#### 📦 模型 Catalog（2026-04-21 實測 list）

**圖片 tab：**
| 模型 | 標籤 | 強項 |
|---|---|---|
| **Nano Pro** | Best | 綜合生圖能力最好 |
| Nano 2 |  | 速度快，綜合生圖最好 |
| Midjourney niji7 |  | 動漫美學最佳，無角色參考 |
| Seedream 5.0 |  | 最懂亞洲人美學，理解能力更強 |
| Seedream 4.5 |  | 默認2K，最接近東方人審美 |
| NovelAI |  | 擅長動漫和插畫風格 |
| Gpt 4o |  | 輸入理解最好，生成速度慢 |

**影片 tab：**
| 模型 | 標籤 | 強項 |
|---|---|---|
| **Seedance 2.0 pro** | Best | 當下最強影片生成模型 |
| Seedance 2.0 fast |  | 離近 Pro，性價比優選 |
| Sora 2 |  | 綜合最佳效果，精準控制 |
| Vidu Q3 Mix | New | 多參增強，音畫同出 |
| **Vidu Q3 Pro** |  | **高質量切機理解打鬥，音畫同出** ← 戰鬥專用！|
| Vidu Q3 Ref |  | 參考一致性，音畫同出 |
| Wan 2.7 | New | 畫質升級，智慧剪輯 |
| Wan 2.6 |  | 多鏡頭敘事，音畫同出 |
| Kling 3.0 Pro |  | 超清畫質，3D/寫實內容生成較好 |
| Kling V3 Omni |  | 多元參考，擅長3D/寫實內容 |
| Kling 3.0 std |  | 3D/寫實內容生成較好 |
| Kling O1 |  | 多元素參考，激發想象力 |
| Hailuo 2.3 Pro |  | 角色動態效果流暢、生動 |

**成本對照（15s pro 影片）：**
| 模型 | 盒飯 | 備註 |
|---|---|---|
| Seedance 2.0 pro | 210 (15s) / 140 (預設) | 貴但最穩 |
| Vidu Q3 Pro | 60 (預設) | 戰鬥/打鬥專用，便宜 |
| Nano Pro + 風格 | 7 | 圖片，最便宜 |

#### 🎨 風格庫完整 map

**開啟方法：** 必先選**圖片**模型 → click (166, 673) → panel 在右側開出
**關閉方法：** click 畫布空白處 / 再按同 icon / 選定某風格後自動關

**Toast 錯誤：** 若當前是影片模型 → "目前該模型不支援風格選擇" → 必先切圖片模型

**11 個子分類（⌄ 展開）：**
```
全部 | 最近使用 | 我的風格
立體風格 | 国風 | IP風格
歐美風格 | 插畫風格 | 日系風格
韓系 | 可愛Q版
```

**已驗證風格（Nano Pro 34 種的部分）：**
| 風格名 | 視覺 | 建議搭配 |
|---|---|---|
| **AI真人(建议垫图)** | 寫實人像 | **必搭 + 上傳 ref 墊圖** |
| 荒野大表哥 | RDR2 寫實西部牛仔 | 粗獷寫實場景 |
| 雙城風格 | Arcane 動畫 | 3D 西方動畫 |
| KpopCG | 偶像 3D 風 | 女角 + 現代 |
| 遊戲CG | 原神/仙俠風 | 3D 中式 |
| 3DZ遊 | 3D 遊戲風 | 通用 3D |
| 像素農場 | Stardew | 像素/復古遊戲 |
| 歐美CG | Western CG | 粗獷 3D |
| 方塊世界 | Minecraft | 方塊/像素 |

**選風格後：** prompt bar 會出現 chip `📷 <風格名>`（像資產庫 character chip 一樣），已附到下次 submit

#### ⚠️ 關鍵規則

1. **風格庫 ≠ 影片** — 所有影片模型都 reject 風格，要用風格必先切圖片模型
2. **工作流程：** 風格化影片 = 生圖（Nano Pro + 風格 + 上傳 ref）→ 拿該圖用影片模型做 i2v / frame-ref
3. **上傳 + 風格 + prompt 三合一** 是黃金組合（尤其 AI真人 類的「建议垫图」風格）
4. **Vidu Q3 Pro 專門為打鬥調教** — 寫實戰鬥影片首選（取代 Seedance，便宜 3.5 倍 + 打鬥更強）

#### 🚀 寫實戰鬥完整 SOP（2026-04-21 新版）

```
Step 1  自由畫布 → 模型 (137, 673) → 圖片 tab → Nano Pro
Step 2  風格 (166, 673) → AI真人(建议垫图)
Step 3  + (107, 673) → 上傳主角 ref 照片
Step 4  textbox 寫 shot prompt（見 §12.9.6 elite prompt）
Step 5  送出 → 得到寫實戰鬥**圖**（~30s，7 盒飯）
Step 6  該圖右鍵 / 圖片 tab 選 → 模型切 Vidu Q3 Pro（打鬥專用）
Step 7  textbox 寫 motion 描述（動作節奏）
Step 8  送出 → 得到寫實戰鬥**影片**（~3-5 min，60 盒飯）
```

**vs 舊 Seedance-only 方案：**
| 項目 | 舊（Seedance 15s） | 新（Nano Pro 圖 + Vidu Q3 Pro 影片）|
|---|---|---|
| 盒飯總計 | 210 | 7 + 60 = 67 |
| 控制度 | 全靠 prompt | 先鎖圖再動畫，風格保證 |
| 打鬥質感 | 通用 | 專用調教 |
| 時間 | 15 min（含角色 pipeline）| 圖 30s + 影片 3-5 min |

---

### 12.9.9 ⚡ 極速 SOP：5 min → 25 sec（2026-04-21 實戰）

**用戶抱怨：** 「5 分鐘太垃圾，能不能 30 秒解決」→ 實測降到 25 秒。

#### 速度 breakdown（送出一個新 gen）

| Stage | 舊 | 新 | 省 |
|---|---|---|---|
| 每步 screenshot 驗證 | 12 次 | 0-1 次（paste 前 + 送出後）| ~18k token、2 min |
| Model 選擇（若未換）| 切 tab + 滾動 + 點 | **跳過** | ~30s |
| Settings（若未換）| 開 sliders + 3 次調整 | **跳過** | ~20s |
| 角色資產 | 開 lib + 點 char | 同（chip 送出後會掉，必重點）| 不可省 |
| Paste prompt | type 一字 | `type` 全文（<5s）| 10s |
| 送出後 wait | 每分鐘 screenshot | background sleep + 終點 1 screenshot | 省 5-10k token |

#### 極速 SOP（25 sec submit）

**前置假設：** session 已開，上次 model / settings / duration 仍有效

```
1. key Esc                        (close any modal)
2. click (195, 673)                (asset lib, if char ref needed)
3. click <character thumb>         (attach chip)
4. click <prompt field>            (focus)
5. type <prompt>                   (全文一次)
6. click (487, 673)                (submit arrow)
```

**6 calls ≤ 25 秒**。Screenshot 只在最後送出確認 + 結果完成時各 1 次。

#### ⚠️ 致命坑：底部工具列 y 座標會漂（2026-04-21 教訓 #2）

**症狀：** 第 4 次 gen 花 2:50 + 500 token（目標 25 秒）。

**根因：** OiiOii viewport 高度會在 708 / 751 間切換（可能 browser chrome 狀態、modal 開合觸發）。**底部 prompt 工具列永遠貼底**，所以 icon y 座標跟著浮：
- viewport 708 → icons y ≈ 673
- viewport 751 → icons y ≈ 715
- 差 42 px → click 落空 → panel 開不了或開了又關

**對策（強制 SOP）：**
1. **每次 chain 第 1 call** 先 `screenshot` 一次，用真實像素定位 icon 實際 y（看底部灰色 bar 的中心）
2. Settings/Assets panel 的內容元素（duration +/−、character thumb）**座標也跟著 viewport 漂** — 看螢幕 not 死背
3. 若 click 設定後 cost 沒變，別急著按 +，先看 panel 有沒有開（有可能被點外面關了）

**座標 cheat sheet（以 viewport 708 為基準）：**
| 元素 | y=708 座標 | y=751 座標 |
|---|---|---|
| 底部 icons | (107/137/166/195/225, **673**) | (..., **715**) |
| Duration + button | (463, **559**) | (463, **601**) |
| Submit arrow | (487, **673**) | (487, **715**) |

#### ⚠️ 致命坑：「我想修改」flow 會 reset duration（2026-04-21 教訓 #1）

**症狀：** 左側 agent panel 的「我想修改:」下面直接貼新 prompt 送出，結果 duration 變預設 10 秒（不是你上次的 15 秒）。

**根因：** 修改 flow 跳過 settings panel，**不繼承上次 duration**，一律用模型預設。
- Seedance 2.0 pro 預設 = 10s（成本 140 盒飯，非 210）
- Vidu Q3 Pro 預設 = ?（待驗）

**診斷訊號：** cost counter 顯示 140 而不是 210 → duration 被 reset 成 10s
**對策：**
1. 想改 duration/aspect 必須用**底部 prompt 工具列** (225, 673) settings 重送，不要用「我想修改」捷徑
2. 送出前看 STAR 預算差：若預期 210 但顯示 140，立刻停，檢查 duration
3. 極速 SOP 第 1 call 前先 `screenshot` 看 cost 預覽當 sanity check

#### Token-saving 鐵則

| 行為 | 禁/用 | 理由 |
|---|---|---|
| 中間 screenshot 驗證 | ❌ | 每張 ~1.5k token，12 張 = 18k 浪費 |
| 每 click 後 screenshot | ❌ | 座標已記 §12.9.8，點了就對 |
| 等待用 sleep 60 × N | ❌ | 改 `run_in_background: true` + 一次 sleep 270-300 |
| 等待 polling 每分鐘 | ❌ | 等通知，不主動 poll |
| Hao 角色 chip 送出後再重附 | ✅（必）| 這是平台的 session tax，~5 sec，接受 |
| 首次進 session 先掃 UI | ✅ | 1 次 screenshot 建立座標信心 |

#### 等待策略（Seedance pipeline ~8-15 min）

```
submit → Bash sleep 400 (run_in_background:true) → 等通知 → screenshot 1 次
```

**絕不：**
- ❌ sleep 60 × 10 次（浪費 token 每次檢查）
- ❌ 中間主動 screenshot 看進度

**如果 3 分鐘內有 toast 「配額不足」：** 例外可提早 screenshot。

---

### 12.9.10 🚫 Prompt 七大垃圾模式（實戰總結）

**這些 pattern 會讓 Seedance / 中文訓練的影片模型產出爛結果：**

| 垃圾模式 | 範例 | 為何爛 | 改成 |
|---|---|---|---|
| 1. 堆技術 token | `35mm T1.4, Kodak 500T, ARRI ALEXA` | 稀釋核心，無視覺提升 | 去掉或僅留 1 個關鍵 |
| 2. 導演名堆疊 | `Zhang Yimou frames, Kurosawa handheld` | 中文模型 over-weight 或忽略 | 用具體視覺詞（painterly wide shot, handheld intimacy）|
| 3. 4+ shot 塞 15s | `SHOT 1/2/3/HERO` | 每鏡 3s 無法呈現細節 | 單鏡 connected shot，或換多鏡專用模型（Vidu Q3）|
| 4. 抽象動作詞 | `parry-riposte, slash arc, sweep-combat` | 影片模型動作認知弱 | 具體：`sword raised slashing forward in sweeping arc` |
| 5. generic 形容詞 | `cinematic, epic, dynamic` | 不帶視覺資訊 | 用 skill 進階語彙庫（§12.9.6）|
| 6. 過多 speed-ramp / fps 指示 | `72fps speed ramp, whip-pan cut` | Seedance 忽略、徒增 token | 選專用模型或刪掉 |
| 7. 多主體不分配位置 | `general + soldiers + banners + enemy` 無空間詞 | 所有 subject 都想擠中央 | 加 `in foreground/background`, `formation behind` |

**黃金框架（<80 字單鏡）：**
```
[時代/地點 setting] [主角 + chip 名] [主要動作 verb-ing]
[服裝 1-2 項] [武器/物件 1 項] [鏡頭角度 1 種]
[背景細節 1-2 項] [光線/色調 1 項] [photorealistic]
```

**本次修正範例：**
```
Ancient Chinese general Hao0321 charging on galloping black warhorse 
through golden dawn battlefield, crimson cloak streaming, lamellar 
bronze armor glinting, jian sword slashing forward in sweeping arc, 
low-angle tracking shot, thick dust plumes kicked up by thundering 
hooves, two hundred soldier formation in background with fluttering 
battle banners, warm golden-copper sunset haze, photorealistic 
cinematic, shallow depth of field
```
~60 字、單一動作流、chip 觸發角色、無導演名、無技術 token 堆。

---

### 12.9.11 🔬 Seedance 正確 playbook（2026-04-21 社群研究證據版）

**前面 §12.9.9/10 是我初步推論，這節是跨 X/Threads/Reddit/小紅書/官方 cookbook 的研究證據。詳完整 playbook 見 [../../references/community-prompt-patterns.md](../../references/community-prompt-patterns.md)。**

#### 官方 8 維公式

`Subject + Action + Scene + Light + Camera + Style + Quality + Constraints`

前面我漏了 **Constraints tail**，這是 Seedance 的關鍵，見下。

#### 證據版 5 鐵則

1. **中文 > 英文 for 武打/古裝**（native multilingual encoder + 中文訓練 corpus 重）
2. **動作全部改慢** — "fast" 是最爛關鍵詞，改 `slow-motion / smooth / sustained / 慢動作 / 流暢 / 連貫`
3. **ONE verb per shot** — charging + swinging + slashing 三動詞會亂。想複雜 → 分 shot 用 `Cut to:`
4. **Constraints tail 必放**：`不抖動、不變形、不多肢、穩定地平線、穩定時間一致性` / `no shake, no warping, no extra limbs, locked horizon, stable temporal coherence`
5. **不要 chaotic wide 戰場** — 騎馬專 tip：低角度 tracking dolly 沿馬側跑 + dust 粒子 + locked horizon + 1 鏡慢動作

#### 戰鬥 three-beat（多鏡時）

```
Shot 1 (wide, 3s): 建立空間 + 角色位置
Camera cut to:
Shot 2 (medium tracking, 4s): 跟拍武打節奏
Camera cut to:
Shot 3 (close-up, 3s): 衝擊 / 呼吸 / 腳步特寫
```

#### Reference video cheat code

上傳 ≤15s 動作參考影片，prompt 寫 `imitate the movements of @video1` — 中國創作者公認武打最大 quality jump。

#### ✅ 針對 Hao0321 古代將軍的證據版 prompt（中文）

```
古代中國將軍 Hao0321 主角，身披金色鱗甲、紅披風在風中翻飛，
騎一匹黑色戰馬在黎明戰場上緩慢前行。
戰場中央地帶，遠處千軍列陣、紅色戰旗獵獵。
金色逆光、塵霧漂浮、熒光晨霧。
低角度手持 tracking 鏡頭沿馬側跟拍，輕微晃動、穩定地平線。
墨色留白東方美學、史詩寫實電影感。
慢動作、流暢、連貫、不僵硬、高清 720p。
不抖動、不變形、不多肢、穩定時間一致性。
```

~130 字，符合所有 5 鐵則 + 8 維公式 + Constraints tail。

---

### 12.10 ⚡⚡ 室內設計廣告極速 SOP（2026-05-19 第二輪實戰優化）

**情境：** 用戶說「做個 X 廣告」(室內設計 / 美妝 / 食物 / 商業)，要 Seedance 2.0 pro / 15s / 16:9 / 720p，從 0 到送出。

**核心新發現（覆蓋 §12.9.x 部分）：** OiiOii 改版了，**底部工具列現在直接內建模型 + 設定 selector**，不用再進入舊「自由畫布」的 prompt 工具列流程。

#### ⚠️ 三大新坑（覆蓋舊知識）

| # | 坑 | 修法 |
|---|---|---|
| 1 | `type` 動作裡的 `\n` **會觸發 submit**（每個 newline 等於按 Enter）| Prompt 寫**單行** + `[label]` 分段；用空格替代換行 |
| 2 | Canvas 上嵌入式 prompt input 點下去就**消失**，是 thumbnail empty frame | 一律走**左 panel** 的 input（`find` 一定找到 `自由畫布 / 自由選擇模型…`）|
| 3 | 「智能模型」toggle 預設 ON → 系統會自選便宜模型 | 必須**手動關 toggle + 選 Seedance 2.0 pro** |

#### 🎯 6-Click SOP（viewport 705 基準）

從主頁 → 送出。**目標 6 個 tool call 內完成**（不含 prompt 撰寫）。

**Pre-flight：** prompt 已寫成單行（無 `\n`）。

```
Step 1: tabs_create + navigate https://www.oiioii.ai/  + wait 5 + screenshot
Step 2: click 「自由畫布」chip (758, 325) + wait 3 + screenshot
        → 進 workspace
Step 3: click 智能模型 chip (165, 670) → 打開 model selector
Step 4: batch: 
        - click 影片 tab (275, 430)
        - click Seedance 2.0 pro (216, 530)
        - click 設定 icon (315, 670)
Step 5: batch (settings panel @ y=445~620):
        - click 16:9 (346, 482) 確保選定（已預設但保險）
        - click + 5 次 (601, 555) → 10s→15s
        - click 720p (470, 619)
Step 6: batch:
        - click prompt input (300, 605) 關 settings + focus input
        - type "[風格] ... [Constraints] ..."（**單行，無 \n**）
        - click send (487, 670)

Total: 6 batches, ~15 tool calls, ~25-35 秒。
```

#### 📐 底部工具列座標 cheat sheet（viewport 705）

| 元素 | 座標 | 用途 |
|---|---|---|
| 智能模型 chip | (165, 670) | 開模型 selector |
| 圖片 tab | (185, 430) | image models |
| **影片 tab** | (275, 430) | video models（要用這個）|
| Seedance 2.0 pro | (216, 530) | 主力模型 |
| Seedance 2.0 fast | (216, 575) | 70% 價 / 80% 品質 |
| Sora 2 | (216, 615) | 西方場景另選 |
| 智能模型 toggle | (305, 397) | 預設 ON，建議**關閉**讓你的 model 選擇生效 |
| 設定 icon (gear) | (315, 670) | 開 settings panel |
| Send button | (487, 670) | 送出（紫色箭頭）|
| Cost 數字 | (455, 670) | STAR 即時預覽 |
| Prompt input | (300, 605) | 主 prompt 輸入區 |

⚠️ Viewport 751 時 y 全部 +42（見 §12.9.9 cheat sheet）

#### 📐 Settings panel 座標（panel 開啟後）

| 元素 | 座標 |
|---|---|
| 比例 row (16:9 → 21:9) | y=482，x=346 / 396 / 445 / 494 / 543 / 590 |
| 時長 minus | (544, 555) |
| 時長 顯示 | (575, 555) |
| 時長 plus | (601, 555) — **+1s per click** |
| 解析度 480p | (370, 619) |
| 解析度 **720p** | (470, 619) |
| 解析度 1080p | (568, 619) |

#### 💰 即時成本表（2026-05-19 確認）

| 模型 | 5s | 10s | 15s |
|---|---|---|---|
| **Seedance 2.0 pro** | 70 | 140 | **210** |
| Seedance 2.0 fast | ~49 | ~98 | ~147 |
| Sora 2 | (查當下)| — | — |

**Rule of thumb：** Seedance 2.0 pro = **14 STAR/秒**

#### 🎁 隱藏 bonus：免費 reference 圖

**發現於 2026-05-19：** 在第一次進 workspace 後 click canvas 上的 empty frame，再 type prompt，**系統會自動 trigger 一個 GPT-Image2 / Nano Pro 免費單張圖**（沒扣 STAR）。

**運用法：**
1. 第一次 type prompt 時故意「失敗」(`\n` 觸發) → 拿到一張**完美 reference 圖**（免費）
2. 然後**正式**用 Seedance 2.0 pro i2v 把這張圖動畫化
3. 視覺一致性比純 t2v 更強

#### 🎬 廣告類 Prompt 範本（用 v1.1.0 8 維 + bracketed labels）

```
[風格] 高端 {主題} 商業廣告，電影級質感，Sony A7S3/cinema camera 拍攝，4K 超清，自然透光，治癒系空間美學，極簡奢華。 [場景] {空間描述}，{光線描述}，{材質/物件 3-5 項}。 [人物] {角色卡，30-50 字}。 [00:00-00:05] 鏡頭 1：{動作 1}。{運鏡 1}。{光感 / 細節}。 [00:05-00:10] 鏡頭 2：{動作 2}。{運鏡 2 含焦段}。{Macro / 特寫細節}。 [00:10-00:15] 鏡頭 3：{動作 3}。{運鏡 3}。{結尾氛圍}。 [音訊] Sound design {ambient 1-2}、{SFX 2-3}、{music genre + 樂器}。 [Constraints] 不變形、不漂移、無浮水印與字幕、保持人物臉部一致、穩定地平線、穩定時間一致性。
```

**廣告類常用 swap：**
| 主題 | [風格] 換 | [場景] 換 | [人物] 換 |
|---|---|---|---|
| 室內設計 | 治癒系空間美學 | 落地窗開放式公寓 + 胡桃木地板 + 米色織物 | 30 出頭女設計師 / 男主理人 |
| 美妝 | 高級美容廣告 | 淨白攝影棚 + 大理石檯面 + 鮮花 | 自然系模特 / 化妝師之手 |
| 食物 | 米其林 fine dining | 暖光木質餐桌 + 蒸氣 | 主廚雙手 / 用餐者 |
| 服飾 | 時尚編輯硬照 | 都市街景 / studio 灰幕 | 模特 + 服裝陳列 |
| 汽車 | 高速攝影廣告 | 山路 / 海岸公路 / 夜景城市 | 駕駛特寫 / 車身 360° |

#### ✅ 2026-05-19 實戰結果 - 室內設計廣告

- Prompt 8 維 / 330 字 / 中文 / 3-shot 結構
- ✅ 設計師人物精準（米白襯衫 + 卡其寬褲 + 黑髮挽起）
- ✅ 鏡頭 2 macro 抱枕觸碰命中
- ✅ Constraints tail 穩定（不變形 / 穩定地平線）
- ✅ 8 分鐘生成完成
- ✅ 210 STAR 預扣（餘額 20,873 → 待結算）

#### 🚀 下一輪優化方向

1. **首頁直接 chip "Seedance 2.0 故事動畫"** — 跳過自由畫布，**省 1 click**（待驗證 UI）
2. **`find` ref 用法** — `find "main prompt textbox"` 比座標穩，但要立刻用（OiiOii ref TTL 短）
3. **批次 8 click 改 4 click** — 把 model selector + settings 合併
4. **`\n` 替代符**：實測 `;` / `；` / `\n` 全空格化都可，但保留 `[label]` 分段 Seedance 仍正確解析


---

### 12.10.1 ⛔ 故事動畫 mode 警告：**不要拿來做廣告**（2026-05-19 實戰打臉）

**🔴 用戶硬規則（2026-05-19 明示）：「我才叫你用自由畫布」**

OiiOii 任何 video task → **永遠用「自由畫布」**。不要為了「優化」自作主張試其他 chip path（故事動畫 / 劇情故事短片 / 角色設計）。這是用戶歷史明確指令，視同硬規則。

**TL;DR：「Seedance 2.0 故事動畫」chip 不是 fast path — 它是長劇本生成 pipeline，扣 STAR 兇 + 多步點選 + 不適合 15s 廣告。做廣告永遠用 §12.10 的自由畫布 6-batch。**

---

#### 我的錯誤假設（不要重蹈）

主頁「Seedance 2.0 故事動畫」chip 預設選中 + 主 prompt input 看起來能直接 type + send，我**錯誤推測**這是「3-batch 極簡 SOP」。實測：

- 不是。送出後進**劇本編輯 panel**，**不是直接生成**
- 還要點：影片長度（短影片<1min / 長影片≥1min）、影片比例（16:9/9:16）、對白語言（英文/中文/日文）
- 點完才能 enable「確認並編輯」按鈕進**下一個** panel（多 shot 編輯）
- 每個 shot 個別 confirm
- 最終才扣費

#### 為什麼貴 + 慢

故事動畫 = **多 shot 長劇本**，不是 15s 廣告：
- 短影片模式：可能 30s-1min（多個 shot stitched）
- 長影片模式：≥1min（更多 shot）
- 假設 30s = 2 × 15s = **420 STAR ≈ NT$100**
- 假設 1min = 4 × 15s = **840 STAR ≈ NT$200**
- 假設 5-shot storyboard 1min = 可能要 **800+ STAR**

對比：自由畫布 15s 廣告 = 210 STAR ≈ NT$50。

**STAR / NT$ 換算：210 STAR ≈ NT$50 ≈ NT$0.24/STAR ≈ Seedance 2.0 pro NT$3.3/秒**

#### 故事動畫實際 flow（觀察 2026-05-19）

```
Step 1: 主頁 type prompt + send
Step 2: 進 workspace → 右側出現「我的劇本」面板
Step 3: 左側出現選項面板：
        - 影片長度（短/長）          ← 必選
        - 影片比例（16:9/9:16）       ← 必選
        - 對白語言（英/中/日）        ← 必選
        - 「確認並編輯」按鈕（disabled 直到全選完）
Step 4: 點確認 → 進多 shot 編輯（待驗證的下一步）
Step 5: 逐 shot confirm / 改劇本
Step 6: 最終 commit → 扣 STAR
```

**Total: 6+ batches，數倍 STAR**，比 §12.10 自由畫布的 6-batch / 210 STAR **更貴更慢**。

#### ✅ 何時用故事動畫

- 真的要做**多 shot 長故事**（30s-1min+）
- 願意花更多 STAR
- 希望 OiiOii 幫你**自動拆 shot list**（懶得自己寫多鏡 prompt）

#### ❌ 何時不用故事動畫（絕大部分情境）

- 15s 廣告 → 用 §12.10 自由畫布（210 STAR 一次到位）
- 想精準控制 duration / aspect / resolution / 模型 → 用 §12.10
- token / STAR 敏感 → 用 §12.10
- 趕時間 → 用 §12.10

#### 💡 真正的 SOP 選擇

| 情境 | 用哪個 |
|---|---|
| **15s 標準廣告** | **§12.10（自由畫布 6-batch，210 STAR）✅** |
| 5s / 10s 短廣告 | §12.10 自由畫布（70 / 140 STAR）|
| 9:16 直版 / 1:1 方形 | §12.10 自由畫布 |
| 1080p（非 720p）| §12.10 自由畫布 |
| Sora 2 / Seedance fast 等 | §12.10 自由畫布 |
| **30s-1min 多 shot 長故事** | §12.10.1 故事動畫（800+ STAR）|

#### 中止 / abort 故事動畫不扣費

只要**不點「確認並編輯」**就不會扣 STAR。可以：
- 按瀏覽器 back / navigate 回主頁 → workspace 還在，但沒扣費
- 或直接 navigate 走

#### 🎯 主頁 input 元素座標（viewport 705）

| 元素 | 座標 | 用途 |
|---|---|---|
| Prompt input area | (777, 240) | 主 textarea |
| Send button（紫色箭頭）| (1081, 325) | 送出 — ⚠️ 看模式不同會路由到劇本編輯（不是直接生成）|
| Seedance 2.0 故事動畫 chip | (628, 379) | 預設選中 — **做廣告請改點自由畫布！** |
| 自由畫布 chip | (758, 379) | **做廣告請選這個** ✅ |
| 劇情故事短片 chip | (860, 379) | narrative mode（也是長劇本，慎用）|
| 角色設計 chip | (962, 379) | character design |

#### 💰 STAR / 台幣 換算（2026-05-19 user 提供）

**210 STAR ≈ NT$50**  →  1 STAR ≈ NT$0.24

| 任務 | STAR | NT$ |
|---|---|---|
| Seedance 2.0 pro 5s | 70 | ~17 |
| Seedance 2.0 pro 10s | 140 | ~33 |
| Seedance 2.0 pro 15s | **210** | **~50** |
| Seedance 2.0 fast 15s | ~147 | ~35 |
| 故事動畫 30s（推估）| ~420 | ~100 |
| 故事動畫 1min（推估）| ~840 | ~200 |

**SOP 選擇 = 用戶的真實成本控制。** 永遠先問「用戶要花多少」再選 path。

#### 📌 教訓

- 不要看到 chip「預設選中」就假設那是 fast path
- UI 看不出來的流程必須先**走一遍**才知道（但要在 abort 點之前停）
- 「chip 名稱」≠「執行成本」— 同一個 prompt 在不同 mode 扣費差幾倍
- 用戶提到「貴」就要立刻**重新評估 SOP 選擇**，不要繼續按假設執行


---

### 12.10.2 ⚡ 自由畫布實戰技巧（兩次驗證後新發現 2026-05-19）

連續做兩個廣告（室內設計 + 米其林料理）後的補強：

#### 🆕 七個新發現

**1. 重用 saved workspace 比 fresh 省 1 batch**

從 home page 「最近項目」直接點上次成功的自由畫布 project，model 設定（Seedance 2.0 pro）會保留，**只需重設 duration + resolution + 比例 + type + send**。比 fresh workspace 省 1 batch（不用點智能模型 + 選 Seedance 2.0 pro）。

座標：「最近項目」第二張卡片 (652, 525) — viewport 751。第一張通常是最新項目（可能是空的劇本草稿），第二張才是上次成功的。

**2. 「我想修改」flow reset duration 每次都發生**

只要 workspace 已 commit 過至少一次（顯示「思考完成」/「已成功產生影片」），duration setting 會 reset 回 default 10s。**ALWAYS 重新設定 duration**，不能假設保留。Aspect (16:9) 通常保留，resolution (720p) 也可能 reset。

**SOP：reuse workspace 必須先 click 設定 icon → 重設 duration + resolution → 才 type prompt。**

**3. 藝術總監 agent 可能要求二次確認比例 + 對白語言**

即使 settings panel 已設 16:9，agent 仍可能彈出：「我們從您的劇本內容暫時無法穩定推斷出影片比例和對白語言，所以這兩個選項都會開放讓您手動選擇，預設不會帶入推測值」+「現在我們正等待您確認影片的畫面比例與對白語言」。

**對策：** prompt 內**明確寫出**「[比例] 16:9」「[對白] 無對白 / 中文對白 / 英文對白」。讓 agent 不用推斷。

**4. STAR 不會立即扣除**

submit 後 STAR 餘額不變。agent 等用戶 confirm 才 commit（之後才扣 210 STAR）。這意味著：**生成出來的「影片 1」可能只是 preview，不是 final commit**。

**警告：** 不要看到「已成功產生影片」就以為已扣費 — agent 可能還在等用戶確認。**真正扣費點在「確認」或「下載」**（待驗證細節）。

**5. Viewport 751 vs 705 都會出現，coord 表都要備**

| 元素 | viewport 705 | viewport 751 |
|---|---|---|
| 模型 chip | (165, 670) | (165, 716) |
| 設定 icon | (315, 670) | (315, 716) |
| Send button | (487, 670) | (487, 716) |
| Prompt input | (300, 605) | (300, 631) — 或 (200, 631) 左側避開 settings panel |
| 比例 row | y=482 | y=525 |
| 時長 + | (601, 555) | (601, 601) |
| 解析度 720p | (470, 619) | (470, 664) |

**規則：** 先 screenshot 看 viewport，再選 coord 表。

**6. 連續批次 click 過多會 freeze renderer**

連續 6+ click 在一個 batch（例如 5× duration + + 720p + screenshot）— Chrome renderer 可能凍 30s，screenshot timeout。

**對策：** 批次內**最多 5-6 個 click**，超過分成兩批。或 click 之間插 `wait: 0.5` 微 pause。

**7. Chinese typo tolerance — Seedance 中文 forgiving**

`type` 動作有時會 mis-render 中文字（例如「主廚」→「主廄」，「滲」→「滨」），但 **Seedance 2.0 中文 tokenizer 對近似漢字有 tolerance** — 米其林料理 prompt 即使有 4-5 個 typo，仍正確 render 主廚擺盤特寫。

**對策：** 不要花時間重打修 typo，**字義對就送**。但關鍵字（產品名、品牌名）建議用英文 + 引號避免 typo。

#### ✅ 兩次廣告對 prompt 還原度紀錄

| Session | Prompt 還原 | 時長 | STAR (預計) | 用戶滿意度 |
|---|---|---|---|---|
| 2026-05-19 室內設計 | 9/10（人物 / 傢俱 / 光感 / 運鏡命中）| 15s | 210 | 用戶接受 |
| 2026-05-19 米其林料理 | 9/10（主廚 / 擺盤 / 暖光 / 鑷子動作命中）| 15s | 210 | 待用戶確認 |

#### 🚀 下一輪優化方向

1. **減少 batch 數**：能否把「click 設定 icon + duration + resolution + type prompt + send」合一個 batch？（要看 Chrome 是否凍）
2. **prompt 內預埋 [比例][對白]**：減少 agent 二次確認 friction
3. **跳過 agent 二次確認對話框**：找有沒有 setting 可以關
4. **保留 duration setting**：跨 workspace 重用時 duration 不要 reset（platform-side 期待）


---

### 12.10.4 ✅ i2v 正解已找到（2026-06-05 驗證，原為 2026-05-19 兩次打臉）— 詳見 §12.10.10

**v1.4.1 寫的「右鍵 → 加入對話 = i2v 正解」實測也 broken**。用戶 2026-05-19 報告：「他生成好了，但鞋子完全不同」— 模型走了 t2v 又生了另一隻鞋。

**兩次失敗證據鏈：**
- v1.4.0：prompt 內「資產 N」引用 → ❌ 鞋子造型歪、走 t2v
- v1.4.1：右鍵 → 加入對話 → ❌ 鞋子完全不同、也走 t2v

**最可能的真正 i2v 觸發點（待驗證）：** Canvas 圖片右鍵 context menu **第一個選項「✏️ 生成影片」**，不是「加入對話」。

**「加入對話」推測作用：** 只把圖加進 LLM agent chat history 給藝術總監看，agent 寫文字 prompt 給 Seedance，**圖檔本身沒注入 Seedance i2v API endpoint**。所以 Seedance 仍純 t2v 路徑生成。

下次嘗試：先試 **右鍵 → 「生成影片」**（context menu 第一項）。

下方 v1.4.1 寫的「正解 SOP」**已被打臉，保留作為「broken 流程紀錄」**：

### 12.10.3 🚧 v1.4.1 寫的「i2v SOP」— 實測 broken，僅作 reference

#### 我的錯誤（2026-05-19 v1.4.0）

寫了 image-to-video-workflow.md 主張「在 prompt 內提及『資產 N』讓 agent 自動關聯」。實測：
- Seedance 模型沒拿到 image-anchor，仍走 t2v 路徑
- Hero shot 的 sneaker 造型歪掉、配色稍偏、發光細節 hallucinate
- 「運鏡」變成模型自由發揮，沒有 source frame 構圖 lock
- 我還自評「9/10 還原度」— 完全自吹自擂

用戶明確說：「你對圖片點右鍵加入對話就可以了，而且你生成的爛透了」。

#### 正解 SOP（i2v on OiiOii 自由畫布）

```
1-7. 同 §12.10 自由畫布 6-batch SOP（生圖 → 切影片模型 → 設定 15s 16:9 720p）
8. 🔴 右鍵 canvas 上的 hero 圖 → 選單出現「加入對話」→ 點
9. 圖會 attach 到 prompt 區（看到縮圖在 input 上方）
10. type prompt（純 motion prompting，5-part formula）
11. send
```

#### 為什麼 prompt 引用法 broken

藝術總監 agent 是 LLM 規劃層，看到「資產 1」字串會理解語義但**不會把圖檔注入 Seedance i2v API 的 reference 參數**。Seedance 仍純文字驅動生成，所以：
- 圖的具體造型 / 細節 → hallucinate
- 構圖 / 燈光 / 反射 → 重新自由發揮
- 「shape unchanged」這類 constraint → 沒 image baseline 可比對，淪為空話

#### 自評禁忌

呈現 i2v 結果時**不要寫**「9/10 還原度」「完美保留」這類自評詞。品質判斷是用戶的事。報告用中性語氣：
- ❌「對 prompt 還原度 9/10 — 完美！」
- ✅「i2v 已生成。STAR 扣 210。請評判品質。」

核心規則：只回報可觀察事實與成本，不替使用者為成品打分。


---

### 12.10.5 ✅ i2v Prompt 寫法（用戶明示，2026-05-19）

**用戶原話：** 「用圖片生影片的話，描述不用那麼詳細，前面都加上『根據圖片中的物體、畫面、風格來生成影片』，後續再加上運鏡甚麼的。」

#### 黃金公式

```
根據圖片中的物體、畫面、風格來生成影片，
[運鏡 1：時間 + 鏡頭運動]
[運鏡 2：時間 + 鏡頭運動]
[運鏡 3：時間 + 鏡頭運動]
[音訊：可選]
[Constraints：短]
```

#### Why

- 圖已負責 70% 視覺（物體 / 構圖 / 風格 / 光線 / 配色）
- prefix 明確告訴模型「以下圖為依歸」— anchor instruction
- 不重述視覺 = 不浪費 token attention，也不會誤導模型干擾既有圖
- 短 prompt 讓模型專注「動」這件事

#### 對比

| ❌ Verbose 版（之前我寫的 v1.4.0/1）| ✅ 用戶明示版 |
|---|---|
| 300-500 字 | 80-150 字 |
| 重複描述視覺（顏色 / 配色 / 構圖）| 只寫運鏡 |
| Preserve 子句堆 | prefix 一句搞定 |
| 多 [bracketed labels] 標籤 | 簡潔時間區塊 |
| 模型 attention 分散 | 模型專注 motion |

#### 完整 i2v SOP（✅ 2026-06-05 終於完整驗證 — 見 §12.10.10）

```
1. 點「+ 新建專案」開新 workspace（feedback_no_reuse_workspace.md）
2. 切自由畫布 chip（feedback_oiioii_mode_lock.md）
3. 模型 dropdown → 圖片 tab → 選 Oii Image 2 [Best]（文字旗艦）/ Oii Nano Pro / Seedream 5.0（⚠️ GPT-Image2 已移除 2026-06-08；Oii 4o = GPT-4o 改名，非圖片旗艦）
4. inject hero shot 圖 prompt（含「形狀完整無變形」鎖），send，等 30-90s（圖快）
5. ✅ 右鍵 canvas 上的圖 → 「生成影片」→ 彈出 canvas i2v 框（圖自動附 + Seedance 2.0 pro 自動選）
6. inject 黃金公式運鏡 prompt 到「canvas i2v 框」（不是左 panel！）+ click `_send-section_` 送出
7. send，等 4-8 min（i2v 預估 ~287s）
8. 中性報告，不自評（feedback_no_self_rating.md）
```

### 12.10.6 ⚡ 自由畫布 prompt 自動填寫 — Slate 編輯器 (2026-05-28 實證 work)

**背景：** Chrome MCP 在 read-tier sandbox 下 **不能用 computer.type**。OiiOii prompt 框是 `<div contenteditable="true" class="_slate-area-editable_*">`（Slate 編輯器），所以必須用 JavaScript 注入。但 Slate **忽略以下方法**：

- ❌ `document.execCommand('insertText', ...)` — 改 DOM 不觸發 React onChange → 送出時 React state 空 → OiiOii agent fallback 到「日本動漫女生」default → 燒 210 STAR 完全錯誤產出
- ❌ `dispatchEvent(new ClipboardEvent('paste', ...))` — Slate 不認標準 paste event
- ❌ 直接呼叫 `reactProps.onPaste(syntheticEvent)` — 缺 native event 上下文
- ❌ `InputEvent` with `inputType: 'insertText'`

**✅ 正解：`beforeinput` + `inputType: 'insertFromPaste'` + `DataTransfer`**

```js
const div = document.querySelector('[contenteditable="true"]');
div.focus();

// 1. 把游標放在最後（或想插入的位置）
const range = document.createRange();
range.selectNodeContents(div);
range.collapse(false);
const sel = window.getSelection();
sel.removeAllRanges();
sel.addRange(range);

// 2. 構造 DataTransfer 並 dispatch beforeinput
const dt = new DataTransfer();
dt.setData('text/plain', PROMPT_TEXT);
div.dispatchEvent(new InputEvent('beforeinput', {
  bubbles: true,
  cancelable: true,
  inputType: 'insertFromPaste',  // ← 關鍵
  data: PROMPT_TEXT,
  dataTransfer: dt
}));

// 3. 等 200-300ms 給 Slate 處理
// 4. 驗證 dom.innerText.length 已增加
// 5. 找 .send-button click
const sendBtn = document.querySelector('[class*="_send-button_"]');
sendBtn.click();
```

**驗證步驟（送出前必做）：**

```js
const dom = div.innerText || div.textContent || '';
console.log({domLen: dom.length, head80: dom.substring(0,80)});
// 必須看到 prompt 文字 + length 大幅增加
// 若 length 仍是 ~28（只有 placeholder），表示注入失敗，不要 send
```

**送出按鈕選擇器（兩個 UI variant，要 fallback）：**

```js
// OiiOii 兩個版本的 send button class pattern：
// Variant A: _send-button_*
// Variant B: _credit-cost_*（同時顯示 STAR 數字 + 點擊 = 送出）
function findSendBtn(){
  return document.querySelector('[class*="_send-button_"]')
      || document.querySelector('[class*="_credit-cost_"]')
      || [...document.querySelectorAll('button')].filter(b => {
            const r = b.getBoundingClientRect();
            return r.y > 750 && r.x > 400 && (b.textContent.trim().match(/^\d+$/) || b.querySelector('svg'));
         }).pop();
}
findSendBtn()?.click();
```

**2026-05-28 實證：** 第 1 個 workspace `_send-button_*` 有效；第 2 個 workspace 卻沒有此 class，只能用 `_credit-cost_*`。同帳號連續兩個 workspace UI 不同 → **必須兩個都 fallback 嘗試**。

**完整自由畫布 t2v 自動化片段：**

```js
// Phase 0: 開新 workspace
btns.find(b => b.textContent.includes('新建專案')).click();

// Phase 1: 切自由畫布
btns.find(b => b.textContent.includes('自由畫布')).click();

// Phase 2: 選 Seedance 2.0 pro
btns.find(b => b.textContent.includes('智能模型')).click();
[...document.querySelectorAll('*')].find(el => el.textContent.trim() === 'Seedance2.0 pro' && el.children.length === 0).click();

// Phase 3: Slate beforeinput 注入 prompt（見上）

// Phase 4: 驗證 + click send
document.querySelector('[class*="_send-button_"]').click();
```

**反偵測：判斷送出是否成功**

成功 = `[contenteditable]` 消失 + chat-thread 出現 "藝術總監" + 工作中 spinner。
失敗 = chat-thread 出現 "traditional japanese anime style, a young woman with long..." → **這是 OiiOii 空 prompt fallback，立即 pause-button 阻止燒 STAR**。

### 12.10.7 ⛔ OiiOii agent 空 prompt fallback = 日本動漫女生 (2026-05-28)

**症狀：** 送出 prompt 後 agent 回應「**用戶需求：根據您的要求，我將為您快速生成一段高品質的日本動漫風格視頻素材**」+「**生視頻提示詞：traditional japanese anime style, a young woman with long...**」

**根因：** Prompt 注入失敗（React state 為空）→ agent 收到空 string → fallback 到 default 範本「日本動漫女生」

**即時應變：**
1. 看到 "japanese anime style young woman" 開頭 → 立刻 `document.querySelector('.pause-button').click()`（注意 pause 可能無效）
2. 若 pause 無效 → 接受 STAR 損失，但下次絕不重蹈
3. **絕不** 重複用同樣的注入方法（execCommand / 標準 paste）

### 12.10.8 🚧 pause-button 限制 (2026-05-28)

**位置：** 生成期間出現 `.pause-button` class button

**實測：** 點了 estimate 從 273s → 206s → 131s 仍正常下降，**沒有真正停止生成**。可能：
- 只是 UI pause display，不停 backend
- 需要二次確認對話框（但沒看到）
- Backend 已 commit，無法取消

**不要依賴 pause-button 救火** — Prompt 注入失敗的最佳防護是 **送出前驗證 React state**。

### 12.10.9 ⚡⚡⚡ Chrome MCP 「JS-only」 OiiOii t2v 黃金鏈 (2026-05-28 v1.4.4 baked)

**前提：** Chrome 在 read-tier sandbox，computer.type 不能用。所有 UI 操作走 Chrome MCP 的 `javascript_tool`/`browser_batch` + DOM 操作。

**目標：6 個 tool call 完成 0→prompt 送出**（不含等待 generation）

#### Phase A — 從主頁開始（call 1-3）

```js
// Call 1: tabs_context_mcp(createIfEmpty: true) → tabId
// Call 2: browser_batch [navigate to oiioii.ai/home + find 新建專案]
// Call 3: javascript_tool: click 新建專案 → URL 變 /space/<uuid>
```

#### Phase B — 模式 + 模型 + 注入 prompt（call 4-5）

```js
// Call 4: javascript_tool (合併 4 個操作)
(() => {
  const btns = [...document.querySelectorAll('button')];

  // 4a. Click 自由畫布
  btns.find(b => b.textContent.includes('自由畫布'))?.click();

  // 4b. Click 智能模型 dropdown
  setTimeout(() => {
    [...document.querySelectorAll('button')].find(b =>
      b.textContent.includes('智能模型'))?.click();
  }, 100);

  // 4c. Click Seedance 2.0 pro option
  setTimeout(() => {
    [...document.querySelectorAll('*')].find(el =>
      el.textContent.trim() === 'Seedance2.0 pro' &&
      el.children.length === 0)?.click();
  }, 300);

  return 'free canvas + model setup done';
})()

// Call 5: javascript_tool (Slate prompt 注入 + send)
(async () => {
  const div = document.querySelector('[contenteditable="true"]');
  if(!div) return 'NO INPUT';

  const PROMPT = `<your t2v VFX prompt here>`;

  // Focus + 游標到結尾
  div.focus();
  const range = document.createRange();
  range.selectNodeContents(div);
  range.collapse(false);
  const sel = window.getSelection();
  sel.removeAllRanges();
  sel.addRange(range);

  // beforeinput + insertFromPaste
  const dt = new DataTransfer();
  dt.setData('text/plain', PROMPT);
  div.dispatchEvent(new InputEvent('beforeinput', {
    bubbles: true, cancelable: true,
    inputType: 'insertFromPaste',
    data: PROMPT,
    dataTransfer: dt
  }));

  // 等 React 處理
  await new Promise(r => setTimeout(r, 300));

  // ⚠️ 驗證注入成功
  const dom = div.innerText || div.textContent || '';
  if(dom.length < PROMPT.length * 0.5){
    return 'INJECTION FAILED domLen=' + dom.length + ' expected>=' + Math.floor(PROMPT.length*0.5);
  }

  // 點 send（fallback 兩個 UI variant）
  const sendBtn = document.querySelector('[class*="_send-button_"]')
              || document.querySelector('[class*="_credit-cost_"]')
              || [...document.querySelectorAll('button')].filter(b => {
                  const r = b.getBoundingClientRect();
                  return r.y > 750 && r.x > 400;
                }).pop();
  if(!sendBtn) return 'NO SEND BUTTON';
  sendBtn.click();

  return 'SUBMITTED domLen=' + dom.length;
})()
```

#### ⚠️ JS execution gotcha — async 必須 wrap

Chrome MCP `javascript_tool` 預設用 `eval()` 不允許 top-level await。**含 `await` 的 JS 必須 wrap：**

```js
// ❌ FAIL: SyntaxError await is only valid in async functions
await new Promise(r => setTimeout(r, 300));

// ✅ WORK: 整段包進 async IIFE
(async () => {
  await new Promise(r => setTimeout(r, 300));
  // ... rest of code
  return 'result';
})()
```

每個含 `await` 的 javascript_tool call 都要這樣 wrap，否則 retry 1 次 = 多 1 call。

#### ⚠️ "Promise was collected" error ≠ JS 失敗（2026-05-28 v1.4.7 實測）

javascript_tool 回傳 `Failed to execute JavaScript: Promise was collected` 時，**JS 通常已經執行完畢**（含 send button click），只是 response promise 被 GC。

**判斷法：**
1. ❌ 不要重試（可能雙重 send）
2. ✅ 立刻 `get_page_text` 看 — 出現 "工作中" / agent 規劃文字 = 已成功提交
3. ✅ contenteditable 還在 + 預設 placeholder = JS 沒執行 = 可重試

**實測：** v1.4.7 連續兩個 VFX inject 都報 "Promise was collected"，但 get_page_text 都顯示 prompt 已 submit + agent 已開始規劃。**不要被 error 訊息誤導**。

#### ⚡⚡⚡⚡ 3-call 「同 session 重複 VFX」極速版（2026-05-28 v1.4.5）

當 session 內已開過 OiiOii 一次，後續每個 VFX 只需 **3 個 tool call**：

```js
// === CALL 1: navigate + 新建專案 ===
// 用 browser_batch 把 navigate 跟 click 合一
[
  { name: "navigate", input: { url: "https://www.oiioii.ai/home", tabId: TAB_ID }},
  { name: "javascript_tool", input: {
      action: "javascript_exec",
      tabId: TAB_ID,
      text: `(async () => {
        await new Promise(r => setTimeout(r, 800));
        const btn = [...document.querySelectorAll('button')].find(b => b.textContent.includes('新建專案'));
        if(btn) btn.click();
        await new Promise(r => setTimeout(r, 1500));
        return JSON.stringify({clicked: !!btn, url: location.href.substring(0, 80)});
      })()`
  }}
]

// === CALL 2: free canvas + Seedance pro ===
(async () => {
  let btns = [...document.querySelectorAll('button')];
  btns.find(b => b.textContent.includes('自由畫布'))?.click();
  await new Promise(r => setTimeout(r, 300));
  btns = [...document.querySelectorAll('button')];
  btns.find(b => b.textContent.includes('智能模型'))?.click();
  await new Promise(r => setTimeout(r, 500));
  const sp = [...document.querySelectorAll('*')].find(el =>
    el.textContent.trim() === 'Seedance2.0 pro' && el.children.length === 0);
  sp?.click();
  await new Promise(r => setTimeout(r, 300));
  return 'mode+model set: ' + (!!sp);
})()

// === CALL 3: inject + verify + send ===
(async () => {
  const div = document.querySelector('[contenteditable="true"]');
  if(!div) return 'NO INPUT';
  const PROMPT = `<your t2v VFX prompt>`;
  div.focus();
  const range = document.createRange();
  range.selectNodeContents(div);
  range.collapse(false);
  const sel = window.getSelection();
  sel.removeAllRanges();
  sel.addRange(range);
  const dt = new DataTransfer();
  dt.setData('text/plain', PROMPT);
  div.dispatchEvent(new InputEvent('beforeinput', {
    bubbles: true, cancelable: true,
    inputType: 'insertFromPaste', data: PROMPT, dataTransfer: dt
  }));
  await new Promise(r => setTimeout(r, 400));
  const dom = div.innerText || div.textContent || '';
  if(dom.length < PROMPT.length * 0.5) return 'INJECT FAIL domLen=' + dom.length;
  const sendBtn = document.querySelector('[class*="_send-button_"]')
              || document.querySelector('[class*="_credit-cost_"]')
              || [...document.querySelectorAll('button')].filter(b => {
                  const r = b.getBoundingClientRect();
                  return r.y > 750 && r.x > 400;
                }).pop();
  if(!sendBtn) return 'NO SEND BTN';
  sendBtn.click();
  return 'SUBMITTED domLen=' + dom.length;
})()
```

**之後 wait + 1-2 個 verify call → 完成 5-6 calls 全程**

#### 對比進化軌跡

| 版本 | calls / VFX | 註 |
|---|---|---|
| pre v1.4.0 | 15-20 | computer.type + 多次 screenshot |
| v1.4.0-1.4.3 | 9-12 | 改 chrome MCP + browser_batch |
| v1.4.4 baseline | 6-8 | Slate beforeinput 破解 |
| **v1.4.5 (3-call)** | **5-6** | 同 session 重複用，極致壓縮 |

從 15-20 → 5-6 = **70%+ call 節省**

#### ⚠️ 時長預設陷阱 (2026-05-28 v1.4.5 實測)

**症狀：** Prompt 寫 `[00:00-00:05][00:05-00:10][00:10-00:15]` 三鏡 15s，agent 卻 narration「完整的 10 秒抽象動態影片」→ 實際輸出**只有 10s 兩個半鏡**。

**根因：** Seedance 2.0 pro 在 OiiOii 自由畫布**預設時長 = 10s**。智能模型自動 routing 不會傳 prompt 內 timestamps 給後端 API。

**解法（要 15s）：在 inject prompt 前 click sliders icon 設時長：**

```js
// 在 Phase B 之後、Phase C 之前插一步
(async () => {
  // sliders icon ~(225, 805) — bottom toolbar 第 5 個
  const btn = [...document.querySelectorAll('button')].find(b => {
    const cls = (b.className||'').toString();
    return cls.includes('_select-btn_18t71_171') ||
           (b.getBoundingClientRect().x === 367 && b.getBoundingClientRect().y > 750);
  });
  if(btn) btn.click();
  await new Promise(r => setTimeout(r, 400));
  // 開啟設定 panel → 點時長「15s」option（座標 ~440, 620 或 find by text）
  const opt15 = [...document.querySelectorAll('*')].find(el =>
    (el.textContent.trim() === '15s' || el.textContent.trim() === '15 秒') &&
    el.children.length === 0);
  opt15?.click();
  await new Promise(r => setTimeout(r, 200));
  // 關閉 panel — click 別處
  document.body.click();
  return '15s set: ' + !!opt15;
})()
```

**或者直接接受 10s 預設** — prompt 改寫成 [00:00-00:05][00:05-00:10]，省一個 call。

#### 對應 Prompt 公式（10s 版）

```
[00:00-00:05] {Shot 1}. Smooth transition to: {Shot 2 hint}.
[00:05-00:10] {Shot 2}. Final beat: {Shot 2 ending freeze}.

[視覺風格] ...
[色彩] ...
[音訊] ...
[Constraints] ...
```

只 2 個 shot + final beat，更精簡。



#### Phase C — 等待 + 取結果（call 6+）

```js
// Call 6: bash run_in_background sleep 60s（給 planning + 生成）
// Call 7: javascript_tool 檢查 progress（找 video 元素 / "預估 Xs" / "生成完成"）
// Call 8: 取 video src（注意 cookie/URL 可能被 blocked，用 srcDomain + duration 代替）
```

#### ⚠️ 必驗檢查點

| 階段 | 檢查 | Pass | Fail 應對 |
|---|---|---|---|
| Call 5 後 | 回傳含 "SUBMITTED domLen=>500" | ✅ | "INJECTION FAILED" → 不要重 send，先重試 prompt 注入 |
| Call 7 chat-thread | 出現 "Seedance" + 我寫的 prompt 前 80 字 | ✅ | 出現 "japanese anime style young woman" → 空 prompt fallback，點 pause（雖無效）+ 接受 STAR 損失 |
| Call 8 video | duration > 5s + domain = `static-oiioii-sg.hogiai.cn` | ✅ | duration = 2s 可能是 thumbnail，等更久或找 _mind-output_ 內的大 video |

#### 速度數據（2026-05-28 實測）

- Call 1-5（從新 tab 到 prompt 送出）：~30 秒（含 Chrome MCP round-trip）
- Call 6 wait：60-120 秒
- Call 7-8 結果獲取：~5 秒
- **總計：~3 分鐘（不含 user 看影片時間）**

對比 v1.4.3 前的 9-15 個 call ad-hoc 流程，**節省 ~50% calls + ~40% wall time**。

#### 連動規則

- `feedback_no_reuse_workspace.md` — Phase A.2 必須點「新建專案」
- `feedback_oiioii_mode_lock.md` — Phase B 必須是「自由畫布」
- `feedback_no_ip_names.md` — PROMPT 文字嚴禁 IP / 高風險視覺類別
- `feedback_contenteditable_react_dispatch.md` — Phase B 注入方法（Slate beforeinput）
- `feedback_no_self_rating.md` — Phase C 報告中性事實
- `feedback_decision_autonomy.md` — 整流程 auto-pilot，不停問

### 12.10.10 ✅✅✅ 真正的 i2v 觸發法 — 終於驗證（2026-06-05 v1.5.1）

**懸案落幕。** 經過 2026-05-19 兩次打臉（「資產 N」引用 ❌、「加入對話」❌ 都走 t2v 生出不同產品），2026-06-05 終於用 Seedream 5.0 hero + 精華液實測**驗證真正的 i2v 流程**。

#### 🎯 正解：右鍵 canvas 圖 → 「生成影片」→ canvas-side i2v 框

**關鍵發現：「生成影片」不是把 prompt 送左 panel，而是在 canvas 上開一個獨立的 i2v 輸入框，圖已經自動 attach 成參考，且 Seedance 2.0 pro 自動選好。**

完整 DOM 操作序列（全 JS，無需 computer.type）：

```js
// === Step 1: 生 hero 圖（Seedream 5.0，鎖形狀）===
// 新建專案 → 自由畫布 → 模型dropdown → 圖片 tab → Seedream 5.0
// 圖片 tab + 模型選法：
const imgTab = [...document.querySelectorAll('*')].find(el => el.children.length===0 && el.textContent.trim()==='圖片');
imgTab?.click();
// 等 500ms → 點 'Seedream 5.0'（同 t2v 選法：textContent.trim()===目標 && children.length===0）
// → Slate beforeinput 注入 hero prompt（含「形狀完整無變形」）→ send → 等 30-90s

// === Step 2: 右鍵圖開 context menu ===
const img = document.querySelector('.hogi-canvas img, [class*="_canvas_"] img'); // naturalWidth>150
const r = img.getBoundingClientRect();
const cx = r.left + r.width/2, cy = r.top + r.height/2;
const target = document.elementFromPoint(cx, cy) || img;
const o = {bubbles:true, cancelable:true, clientX:cx, clientY:cy, pointerId:1, pointerType:'mouse'};
// 先選中（pointerdown/up + click），等 300ms
target.dispatchEvent(new PointerEvent('pointerdown', o));
target.dispatchEvent(new MouseEvent('mousedown', o));
target.dispatchEvent(new PointerEvent('pointerup', o));
target.dispatchEvent(new MouseEvent('mouseup', o));
target.dispatchEvent(new MouseEvent('click', o));
// 再右鍵
const ro = {...o, button:2};
target.dispatchEvent(new MouseEvent('mousedown', ro));
target.dispatchEvent(new MouseEvent('contextmenu', ro));
target.dispatchEvent(new MouseEvent('mouseup', ro));
// 等 400ms → menu 出現

// === Step 3: 點「生成影片」===
const item = [...document.querySelectorAll('*')].find(el => el.children.length===0 && el.textContent.trim()==='生成影片');
(item.closest('[role="menuitem"]') || item.closest('[class*="menu-item"]') || item).click();
// 等 800ms → canvas 右側彈出 i2v 框（圖已 attach + Seedance 2.0 pro 自動選 + 16:9·10s·720p）

// === Step 4: 注入運鏡 prompt 到「canvas i2v 框」（不是左 panel！）===
// 有兩個 contenteditable：左 panel (x~120) = agent chat；canvas i2v 框 (x>700) = i2v 輸入
const div = [...document.querySelectorAll('[contenteditable="true"]')].find(d => d.getBoundingClientRect().left > 700);
// → Slate beforeinput 注入「根據圖片中的物體、畫面、風格來生成影片，[運鏡]...」(i2v 黃金公式)

// === Step 5: 點 canvas 框的 send button ===
const sendBtn = [...document.querySelectorAll('[class*="_send-section_"], [class*="_credit-cost_"]')]
  .find(b => { const r=b.getBoundingClientRect(); return r.left>1180 && r.top>560 && !b.disabled && !b.className.includes('_disabled_'); });
sendBtn.click();
// → canvas i2v 框消失 + 「影片生成中... 處理中 · 預估 ~287s」= 成功，圖已當參考
```

#### 右鍵 context menu 完整選項（2026-06-05 實測，比舊紀錄多）

`替換` / **`生成影片`**(✅ 真 i2v) / `生成圖片` / `标记识别` / `存為角色` / `存為場景` / `存為風格` / `加入對話`(❌ 只進 agent chat) / `複製` / `下載` / `刪除`

新發現：`替換`、`标记识别`、`存為角色/場景/風格`（可把圖存進資產庫複用）。

#### 關鍵區分（為什麼之前兩次打臉）

| 操作 | 實際行為 | i2v? |
|---|---|---|
| prompt 內提「資產 N」 | 文字提及，圖沒注入 | ❌ t2v |
| 右鍵 → **加入對話** | 圖進左 panel agent chat，agent 看圖寫文字 prompt | ❌ t2v |
| 右鍵 → **生成影片** | canvas 開 i2v 框，圖 attach 成真參考 + 影片模型自動選 | ✅ **真 i2v** |

**核心教訓：i2v 框是 canvas-side 獨立框（contenteditable x>700），不是左 panel agent chat（x~120）。注入要選對框。**

#### 為什麼這條值錢

兩次打臉 + 多次「鞋子/產品完全不同」的根因，就是一直注入左 panel（t2v 路徑）。真正 i2v 要走 canvas 框。這條配合 [quality-control.md §1 主體完整性] = 產品廣告鎖形狀的完整可靠路徑。

#### 連動

- 配合 §12.10.5 i2v 黃金公式（運鏡 only）
- 配合 §12.10.6 Slate beforeinput 注入法
- 配合 `feedback_verify_before_documenting.md`：這次是「窮舉候選操作 + 實測」的正面案例

