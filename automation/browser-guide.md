# 瀏覽器自動操作指南 — High-level 流程速查

當使用者要求「幫我直接操作 OiiOii/Kling/Suno 等網站」時使用。這份檔是 **各站的高層流程摘要**。

**Click 怎麼點？見 [click-protocol.md](click-protocol.md)** (reliable click SOP、fallback chain、驗證循環)。
**某站的 UI 地圖？見 [site-profiles/](site-profiles/)** (每站一份深度 profile)。

## 工具選擇

| 工具 | 用途 |
|---|---|
| **Chrome control skill** (`chrome:control-chrome`) | 操作使用者真實的瀏覽器（保留已登入狀態），最自然 |
| **Claude Preview** (`mcp__Claude_Preview__*`) | 開一個新的沙箱瀏覽器，有自己的憑證時用 |

多數情境選 Codex 的 `chrome:control-chrome`，因為 AI 生成平台都需要登入 + 付費會員狀態。

## 5 大不可越界的安全規則

1. **絕不代輸入密碼** — 網站要求登入時，停下來請使用者自己登入
2. **送出前一律預覽** — 要貼進去的 prompt 完整顯示給使用者，**等他確認才送**
3. **每個 irreversible action 前後都 screenshot** — 送出、付款、下載前 (before) 與後 (after)，比對驗證
4. **撞到 paywall / modal 立刻停** — 不要嘗試繞過、不代付款、問使用者選項
5. **不要亂重試** — click 沒中先 screenshot 診斷，不要傻傻連點三次 (可能連點會觸發非預期行為)

## 等待節奏速查 (詳見 [click-protocol.md](click-protocol.md) Part 8)

| 情境 | 節奏 |
|---|---|
| 等 agent 互動 UI 出現 | 60 秒 (`ScheduleWakeup` 最小值) |
| agent 背後處理無 UI 變動 | 3-5 分鐘 |
| 影片生成長任務 | 3 分鐘 |
| 上傳/下載 | 30 秒 |

## 平台別操作腳本

### OiiOii (https://www.oiioii.ai/) — 2026-04 實測腳本

**前置：** 使用者已登入，右上「FREE 代幣」有餘額。

**實測反常規 gotcha (非常重要)：**

1. **ref 超級不穩** — `read_page`/`find` 回的 `ref_N` 在下次 action 前常已失效。對策：**能用座標就用座標**，不要靠 ref。ref 就拿來確認元素在那裡，然後立刻 click，不要中間插 screenshot/zoom
2. **a11y tree 缺件** — 某些按鈕 (尤其是小型選項按鈕如語言 3 選 1) 完全不在 a11y 裡，find 找不到，只能靠座標
3. **中央 prompt 框 = contentEditable DIV**，不是 `<textarea>` → `form_input` 回錯，要用 `computer.left_click` 然後 `computer.type`
4. **type 含 `\n` 會觸發送出** → 貼多行 brief 後 URL 立刻跳 `/space/{uuid}`，不需要再找送出鍵。若想不送出先存，得手動用 `\\n` 或去掉換行
5. **選中視覺 = 填底深灰** (非粉色 ring — ring 是 hover focus) → 驗證 click 是否成功時要看填底不是看邊框

**UI 模式 (2026-04 實際)**
- Seedance 2.0 故事動畫 (預設，≈ Quick Video)
- 自由畫布 (多模型)
- 劇情故事短片 (多鏡)
- 角色設計 (IP Creation)

**標準腳本：**

1. `navigate` `https://www.oiioii.ai/`
2. `screenshot` 確認 `/home` 已載入、代幣餘額、模式按鈕就位
3. **選模式** (若不用預設 Seedance 2.0)：`find` 對應按鈕 → 立刻 `left_click`
4. **貼 brief**：`find` 中央 prompt 框 (ref_N) → `left_click ref=ref_N` → `type <brief>` (含 `\n` 會自動送出)
5. URL 跳 `/space/{uuid}`，左側欄出現 checklist：`激活工作流` (auto) → `更新短片參數` (auto) → 要使用者選三項
6. **參數頁座標 click** (viewport ≈ screenshot 座標，實測 OK)：
   - 短影片: `(190, 200)`  ｜ 長影片: `(395, 200)`
   - 橫版 16:9: `(190, 310)` ｜ 豎版 9:16: `(395, 310)`
   - 英文: `(157, 410)` ｜ 中文: `(290, 410)` ｜ 日文: `(428, 410)`
   - 確認並繼續: `(290, 477)` — 三組都選完後才變紫紅色 active
7. Click 確認並繼續 → 出現「信息已確認」泡泡 → checklist 進下一項
8. **監控迴圈** — 多智能體每階段可能要再確認；但 Phase 2 送出後會進入 10-15 分鐘純背後工作 (UI 完全靜止)。策略：
   - Phase 2 送出後第一次 screenshot 確認「信息已確認」泡泡出現
   - **出現紅 toast「選項暫不支援重選哦」也是 OK 的** (代表確認已送出，不要再點)
   - 之後改為 **每 3-5 分鐘** screenshot 一次 (不是 30-60s)，直到 checklist 出現新項目或畫布出現影片 / 圖像
   - 若 15 分鐘後仍靜止 + `工作中...` 還在 → 考慮 reload 或檢查代幣餘額
9. 完成後中央 tldraw canvas 顯示影片，在右側工具列找下載

**代幣花費實測：** Seedance 2.0 故事動畫 短影片 (<1min) 消耗 TBD (等這次跑完補)

**每次新 session 前一律重跑 screenshot + find**，OiiOii 會改版，本節僅保證 2026-04 準確。

### Kling (https://klingai.com/)

**前置：** 已登入 (國際版需要 Google / email)。

**操作步驟：**
1. 導航到 `https://app.klingai.com/global/` 或 `https://klingai.com/`
2. 左側選 "Video" tab
3. 選 mode：Text to Video / Image to Video
4. 選模型：Kling 1.6 / 2.0 / 2.1 / 2.1 Master / 2.6 Pro
5. 貼 prompt (textarea)
6. 填 negative prompt (advanced 區塊)
7. 設 aspect、duration (5s/10s)、generation count
8. 如要 Motion Brush / Camera Control / Elements → UI 對應按鈕
9. 送出 → 等待 (通常 2–5 分鐘 per shot)
10. History 區找結果下載

### Suno (https://suno.com/)

**前置：** 已登入，**必須已訂閱** (免費額度也可測試，但較慢)。

**操作步驟：**
1. 導航到 `https://suno.com/create`
2. 切換到 **Custom Mode** (上方切換)，才可分別填 Style 與 Lyrics
3. Style 欄貼 4–8 個 tag 組合
4. Lyrics 欄貼完整結構 (含 `[Verse]`, `[Chorus]` metatags)
5. 設 model (v5)
6. Title 填歌名
7. 點 "Create" → 等 30–60 秒
8. 產生 2 版 → 試聽 → 若滿意點下載 MP3 / WAV (v5 Studio 可下載 stems)

### Midjourney (https://www.midjourney.com/)

**前置：** 已登入，訂閱中。

**操作步驟 (Web UI)：**
1. 導航到 `https://www.midjourney.com/imagine`
2. 頂部 imagine 欄貼 prompt (含參數)
3. Enter 送出
4. 下方網格等生成完成
5. 點圖放大 → 選 Upscale 或 Vary

**Discord 模式：** 進 `#general-N` 頻道，`/imagine prompt: ...`。Discord 操作需要 MCP 對 Discord 的支援或 switch 到瀏覽器版本的 Discord。

### Runway (https://app.runwayml.com/)

1. 導航 → 選 Gen-4 Video / Aleph / Gen-4 Images
2. Text 欄 / 上傳參考圖
3. 設定 aspect / duration / seed
4. Generate → 等待 → 下載

### 即夢 Jimeng / 豆包 Doubao (Seedream / Seedance)

中文 UI。
- `https://jimeng.jianying.com/ai-tool/home`
- `https://www.doubao.com/chat/`

主要流程同上，介面為中文。登入需手機號。

### Vidu (https://www.vidu.com/)

1. 導航 → 選 Text/Image/Reference-to-Video
2. Reference 模式：上傳 1-7 張 refs
3. Prompt 欄
4. 設 model (Q2 / Q3) / aspect / duration
5. Generate → 等待 → 下載

## 與 Codex Chrome control 整合的範本

```
使用者：幫我在 Kling 上產生這個 prompt 的影片：[prompt]

Codex：
1. 先讀 references/kling.md 確認 prompt 格式正確
2. 向使用者確認最終 prompt + 參數
3. 依 `chrome:control-chrome` skill 導航到 Kling
4. 讀取頁面文字或 screenshot 確認已登入
5. 若未登入 → 告訴使用者去登入 → 等待
6. 找到 prompt textarea → 用 Chrome 控制工具輸入
7. 設參數 → 送出
8. 定期 screenshot 追蹤狀態
9. 生成完 → 找下載按鈕 → 點擊
10. 告訴使用者檔案位置
```

## 除錯

### 找不到元素
- 先 `screenshot` 看 UI 變了沒
- 用 `read_page` 或 `get_page_text` 拿可見文字
- 用 `find` 或 `inspect` 定位 CSS 選擇器
- 網站 rebrand 後選擇器會變，**不要記死**

### 送出後卡住
- Screenshot 看是不是跳 captcha / 付費牆
- 帳號餘額不足 → 告訴使用者
- 過伺服器繁忙 → 等 1 分鐘再試

### 下載失敗
- 檢查瀏覽器下載設定 (預設路徑)
- 有些站要右鍵另存
- 用 `preview_eval` 跑 JS 拿 direct URL，再 `Download` 工具

## 安全與倫理

- **不要幫使用者突破 paywall / 免費額度限制** (例如多開帳號)
- **不要生成違反平台條款的內容** (Deepfake 無同意對象、兒童不當、仿冒商標、名人無授權)
- **不要 scraping 他人作品** 訓練或商業用
- 生成結果 **提醒使用者商用前確認版權條款** (各平台不同)

## 常見平台的版權 / 商用狀態摘要 (2026-04, 以各平台條款為準)

| 平台 | 商用 | 備註 |
|---|---|---|
| Midjourney Basic+ | ✓ | 年費 $10k+ 公司需要 Pro |
| Runway | ✓ | 付費方案 |
| Kling | ✓ (付費) | 國際版較明確 |
| Suno | ✓ Pro 以上 | 免費方案無商用 |
| Seedream / Seedance | 依 Volcengine/BytePlus 協議 | 企業需另簽 |
| Vidu | ✓ 付費 | |
| Veo 3 (Vertex) | ✓ | Google 企業協議 |
| Sora (ChatGPT Plus) | 看最新條款 | 過去模糊，現已放寬 |
| Flux Pro API | ✓ BFL 商用授權 | |
| Flux Dev | ✗ 非商用 | Schnell 可商用 |
| Stable Diffusion 3.5 | 依 Stability license | 企業需 Enterprise |
| OiiOii | 依會員等級 | 看官方條款 |

**最終依據永遠是各平台最新服務條款**，操作前請使用者自行確認。
