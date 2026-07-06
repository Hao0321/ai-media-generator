# Site Profiles

每個站一份深度 profile：UI 地圖 + 座標 + 陷阱 + 付費結構 + 決策樹。

## 驗證狀態速查

| 網站 | 狀態 | 檔案 | 備註 |
|---|---|---|---|
| OiiOii.ai | ✅ **2026-06-08 大改版重測** | [oiioii.md §0](oiioii.md) | **§0 = 新版 SOURCE OF TRUTH**（左欄導航/新建直進canvas/Agent▾模型器/新模型/i2v/一致性系統）。§1-§12 舊 UI 保留參考 |
| Google Flow (Veo 3.1) | ✅ 2026-04-19 | [flow.md](flow.md) | **完整 Phase 1-6**，Fast/Quality/Lite 已測 |
| Kling 3.0 | ✅ 2026-04-20 | [kling.md](kling.md) | **完整 Phase 1-7**，3.0 + Fast-Track + Native Audio |
| Suno v5 | ✅ 2026-04-20 | [suno.md](suno.md) | **完整 Phase 1-9** + Speed Optimization (5 首歌 chain SOP) |
| Midjourney v7 | 📝 Stub | [midjourney.md](midjourney.md) | Web + Discord 兩種模式 |
| Seedream (即夢 / 豆包) | 📝 Stub | [seedream.md](seedream.md) | 三入口 + 中國區登入特殊 |
| Runway Gen-4 / Aleph / Act-Two | 📝 Stub | [runway.md](runway.md) | Gen-4 Refs (3 圖) + Act-Two mocap |
| Sora 2 | 🔴 **已停運** | [sora.md](sora.md) | app/web 2026-04-26 關、API 2026-09-24 關。**不支援自動化，新案改 Veo/Kling/Runway** |
| Vidu Q3 | 📝 Stub | [vidu.md](vidu.md) | Q3 1-7 ref + 16s 多鏡含音 |
| Ideogram 3 | 📝 Stub | [ideogram.md](ideogram.md) | 強文字渲染 (海報 / Logo) |
| Seedance 2.0 pro | ✅ 2026-04-21 | [oiioii.md §12.5](oiioii.md) | 透過 OiiOii 聚合操作 5 次，8 維公式 + Constraints tail 證據見 [../../references/seedance.md](../../references/seedance.md) |
| Seedance / Volcengine (直入) | ⏳ 未驗證 | 待操作時補 | 官方入口尚未操作，Multi-shot Cut to: 語法 |
| Flux (BFL / fal.ai / Replicate) | ⏳ 未驗證 | 待操作時補 | 見 references/flux.md |
| Stable Diffusion (A1111 / ComfyUI) | ⏳ 未驗證 | 待操作時補 | 本地工具 |
| Nano Banana (Gemini) | ⏳ 未驗證 | 可在 Flow/Gemini App 中 | 見 [../../references/nano-banana.md](../../references/nano-banana.md) |

## 寫新 profile 的方法

1. 複製 [`_template.md`](_template.md) 成 `<platform>.md`
2. 實際操作該平台時，**即時記錄每 phase 的**：
   - 關鍵元素座標 (截圖下 1568×? viewport)
   - 不在 accessibility tree 的元素 (用 find 找不到的要特別記)
   - Selected state 的視覺特徵
   - 付費牆的觸發位置
   - 等待時間與 agent 互動節奏
3. demo 完成後把檔案補完
4. 回來這個 README，把驗證狀態從 ⏳ 改成 ✅ 並標日期

## 通用 click 協議

所有 site 都 inherits [../click-protocol.md](../click-protocol.md)。profile 只寫**該站特有**的東西，通用的坑 (ref 失效、click 沒中診斷等) 不要在 profile 重複。

## ⚡ Chain Speed Optimization (強制)

**所有 profile 都必須有「Chain Speed Optimization」章節** (見 _template.md §9)。chain workflow (連跑多任務) 不照辦會浪費 70% token + 3 倍時間。

**鐵則：**
- Chain 中只首尾各 1 張 screenshot
- TodoWrite 在 chain 跑完一次性更新，不要每 task 一次
- Clear field 用 1-click (trash icon / ctrl+a) 不要 3 步
- 內容寫**標準長度**不要超寫 (Suno 歌詞 25 行、Veo prompt 80 字、MJ keyword 30-50 字、Seedream 80-120 字)
- 平台有 Series Mode / Multi-Shot / Multi-Reference → **優先用內建批次** 而非 N 次 chain

完整論述見 [../click-protocol.md §「Token + 時間最佳化」](../click-protocol.md)。
