# 平台選擇決策樹

用這個檔案決定「使用者的需求最適合哪個平台」。如果使用者已指定，直接跳過。

## 決策順序

### Q1: 要產出什麼？

| 媒體 | 走哪一節 |
|---|---|
| 靜態圖 (海報、插畫、產品、人像) | Q2-IMAGE |
| 影片 (動畫、短片、電影感) | Q2-VIDEO |
| 音樂 / 歌 | Suno v5 (唯一主流選擇) → [suno.md](suno.md) |
| MV / 音樂+影像一條龍 | OiiOii (低操作成本，**但影片階段要付 $7**) 或 Suno + Kling/Seedance 手動串 → [oiioii.md](oiioii.md), [../templates/music-video.md](../templates/music-video.md) |
| 多鏡頭故事 / 分鏡動畫 | Seedance (multi-shot 強) 或 OiiOii (**影片付費**) → [seedance.md](seedance.md), [oiioii.md](oiioii.md) |

**OiiOii 付費提醒：** OiiOii 前期的劇本/角色/場景生成在 2000 FREE 代幣範圍內免費，但**進到影片生成階段 (分鏡師) 會跳 $7 付費牆**。若使用者預算有限或只要劇本/視覺設計，OiiOii 到場景師為止就該停。看到「Sora 2 試用 / Seedance 2.0 試用」兩個模型選項時，兩個都是 $7 解鎖。

---

### Q2-IMAGE: 靜態圖

**主要決策：要不要文字在圖裡？**

| 需求 | 首選 | 次選 | 原因 |
|---|---|---|---|
| 圖裡有中英文字 (海報、梗圖、標題) | **Seedream 4.5/5** (中英) 或 **Nano Banana Pro** (英文海報) | Ideogram 3 | Seedream 中英都強；Nano Banana Pro 文字業界第一；Ideogram 英文準但中文弱 |
| 寫實人像、攝影感 | **Flux 1.1 Pro** | Nano Banana Pro / Midjourney v7 (--raw) | Flux 皮膚質感最真；Nano Banana Pro 近接 Flux 水準且可編輯 |
| 藝術風格、插畫、氛圍 | **Midjourney v7** | Seedream / Nano Banana | Midjourney 美感預設值高 |
| 編輯既有圖片 (局部改、換衣、換背景) | **Nano Banana (Gemini)** 或 **Flux Kontext** | Seedream 4.0 editing | Nano Banana 對話式編輯最直覺；Kontext 技術最精；兩者皆優 |
| 角色一致性多張圖 (storyboard) | **Nano Banana Pro** (最多 5 人不用 fine-tune) | Midjourney v7 --oref / Seedream reference | Nano Banana Pro 目前業界角色一致性最強 |
| 多圖融合 (blend 角色+物件+場景) | **Nano Banana Pro** (原生多圖) | Midjourney --oref 多張 | Nano Banana Pro 原生 blend 最靈活 |
| 本地 / 開源 / ComfyUI | **Stable Diffusion 3.5** | Flux dev (open weights) | 唯二有完整開源生態 |
| 快速大量批次 | **Flux Schnell** | SDXL Turbo | 皆為蒸餾快速版 |
| 中式美學 (水墨、國風、漢服) | **Seedream** | Midjourney --niji | Seedream 訓練集有大量中文文化資料 |
| 動漫 / 二次元 | **Midjourney v7 --niji** | Seedream (動畫 token) | niji 專門分支 |

---

### Q2-VIDEO: 影片

**主要決策：要電影感質量，還是快速控制？**

| 需求 | 首選 | 次選 | 原因 |
|---|---|---|---|
| 原生同步音訊 (對話、環境音) | **Veo 3.1** | Sora 2, Vidu Q3 | Veo 是第一個原生 audio；Sora/Vidu Q3 後期也有 |
| 運鏡最準、物理感最真 | **Kling 2.6 Pro** 或 **3.0** | Seedance 1.0 Pro | Kling 最新版運鏡、人體動態是市場頂尖 |
| 多鏡頭一氣呵成 (MV / 短故事) | **Seedance 1.0 Pro** | Vidu Q2 | Seedance 有 "Cut to" 多鏡頭語法 |
| 最多參考圖 (角色 + 物件 + 場景) | **Vidu Q2** | Runway Gen-4 | Vidu 支援 2 videos + 4 images 共 7 refs |
| 真人演員風格 / 電影製作 | **Runway Gen-4 + Aleph** | Veo 3.1 | Gen-4 Refs + Act-Two 是好萊塢路徑 |
| 影片編輯 (局部改、加刪元素、換風格) | **Runway Aleph** | — | Aleph 是唯一專做 video editing 的大模型 |
| 社群短片 / 免費測試 | **Kling 免費額度** 或 Vidu 免費 | Sora 2 (ChatGPT Plus) | 都有免費 tier |
| 16 秒以上長片 | **Vidu Q3** (原生 16s) 或 Kling extend | Seedance extend | Q3 是少數原生 16s |
| Image-to-Video (首幀變影片) | **Kling 2.1+** 或 **Seedance I2V** | Runway Gen-4, Vidu I2V | 都有，Kling 的動態保真最好 |
| 首尾幀 (Start-End Frame) | **Vidu** | Kling | Vidu 做得特別穩 |
| 對口型 / Lip Sync | **Runway Act-Two** | Kling Lip Sync | Act-Two 是 performance capture 級 |
| 中文文化影片 (漢服、國風武俠) | **Kling** 或 **Seedance** | Vidu | 中國模型對中文敘事與美學最準 |

---

### Q3: 有沒有 API 預算？

| 情境 | 建議 |
|---|---|
| 完全免費 | Kling 免費額度、Vidu 免費、Sora (ChatGPT Plus 已內含) |
| 訂閱制 | Midjourney ($10/mo+)、Runway、Kling Pro、Suno |
| 按用量付費 / API | fal.ai、Replicate、Volcengine (Seedream/Seedance)、BytePlus ModelArk、Vertex AI (Veo)；使用者指定統一 media API 時可走 [Atlas Cloud](atlas-cloud-api.md) |
| 企業 / 大量 | 直接接官方 API (Google Vertex AI for Veo / Imagen, OpenAI for Sora 2, BFL 企業方案) |

---

## 典型選擇範例

**「我想做一個 30 秒古風武俠短片，有配樂」**
→ Suno v5 做配樂 + Seedance 1.0 Pro 做 3–5 個多鏡頭片段 + 最後剪接。或一站式用 OiiOii Story Video 模式。

**「我要幫公司做一張雙語產品海報，文字要精確」**
→ Seedream 4.5。Prompt 把 slogan 用雙引號包起來。

**「我上傳我兒子的照片，想做一段他跑過花田的影片」**
→ Kling 2.1 I2V (首幀+prompt) 或 Runway Gen-4 References (更穩的人臉一致)。

**「我想要一個跟 Discord bot 自動接的 AI 生圖」**
→ Flux Schnell via fal.ai (最便宜最快) 或 SD 3.5 self-host。

**「我完全不會寫 prompt，想上傳一張風景圖自動變 MV」**
→ OiiOii 的 "Anime Music Video" 模式。

---

## 什麼時候該推翻首選

- 使用者已經熟某工具 → 照他熟的。工具切換成本 > 模型差距。
- 任務要做 3+ 次同類 → 走 API 自動化；不要用 Web UI 操作。
- 使用者明確指定 Atlas Cloud → 先讀 [atlas-cloud-api.md](atlas-cloud-api.md)，即時核對 model schema；生成 POST 不自動重送。
- 有嚴格品牌 / 法律需求 (商用、版權乾淨) → 優先選官方有明確商用條款的 (Google Veo、Adobe Firefly、Black Forest Labs 商用授權)。
