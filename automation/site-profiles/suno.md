# Site Profile: Suno v5

**URL:** `https://suno.com/`
**驗證狀態:** ✅ 2026-04-20 Phase 1-6 實測 (免費 hao0321 帳號 50 credits，跑《夜貓》10 credits/2 songs，v4.5-all model)
**平台類型:** AI 音樂生成 (2026-04 主力 v5)
**Stack:** React，Style / Lyrics 兩個獨立欄位

---

## 1. 預期 UI (根據 references/suno.md 知識)

### 登入
- 左上 Sign In (Google / email / Microsoft / Apple)
- 需訂閱 (Basic / Pro / Premier) 才能完整使用；免費每日 50 credits 限 5 首歌

### 主流程入口 (猜測)
```
https://suno.com/create  ← Custom Mode
https://suno.com/        ← 對話模式
```

**Custom Mode (推薦) 的關鍵欄位：**
- **Style** 欄 (貼 4-8 個英文曲風 / 情緒 / 樂器 / BPM tag)
- **Lyrics** 欄 (貼結構化歌詞 + `[Verse]` / `[Chorus]` / `[Bridge]` 方括號 metatags)
- **Title** (歌名)
- **Model** 選擇 (v4.5 / v5)
- **Make Instrumental** (無人聲 toggle)

---

## 2. Phase-by-phase (2026-04-20 實測)

### Phase 1 — 登入
- 左上 "SUNO" logo 下有 Sign In / 頭像
- 登入後顯示帳號名 + credits 數 (例：hao0321 / 50 credits)
- 左上方有 ♪ 50 credits 球 icon 也顯示

### Phase 2 — 進 Custom Mode
- URL `https://suno.com/create` 直達 (不用繞首頁)
- 頂部中央有 **Simple / Advanced** 切換 (Advanced 才是 Custom Mode)，預設會選 Advanced
- **右上 model 下拉** (`v4.5-all ▾`) 點開看全版本：
  - v5.5 Pro / v5 Pro / v4.5+ Pro / v4.5 Pro — **所有 Pro 版要訂閱**
  - **v4.5-all ✓ Best free model** — 免費可用
  - v4 Pro
- 若進首頁 `https://suno.com/` 有 Spring Sale modal (X 關 at ~898, 165)

### Phase 3 — Lyrics 欄位
**類型：** textbox (ref_93 或類似 ref)，**支援 form_input？ 尚未測；目前走 click + type 穩定**
**位置：** 中央編輯面板最上方
**SOP：** `find "Lyrics textbox"` → `left_click ref` → `type <整段>`

**寫法：** 見 [../../references/suno.md](../../references/suno.md)。關鍵：
- **全部結構用方括號** `[Verse 1]` `[Chorus]` 等
- Top-anchor：頂部放 `[C minor ballad, 68 BPM, dual-tracked breathy female lead, lo-fi bedroom pop intimate]`
- 每段 vocal 指示 `[Female Vocal, whispered]` `[Full band enters, harmony on "夜"]`
- Bridge 可加 **spoken word** `[Female Vocal, spoken half-whispered]`
- 中文每行 **6-8 字** 最穩
- 結尾 `[Outro]` + `[End]` 防無限 loop

### Phase 4 — Styles 欄位
**類型：** textbox (ref_102)，位於 Lyrics 下方
**預設內容：** 會顯示範例 `soft pop, 123 bpm, rap trap, slow acoustic, spanish flamenco` — **要先清空再貼**
**清空 SOP：** `left_click ref` → `key ctrl+a` → `key Delete` → `type <新 style>`

**專業音樂人級寫法** (實測有效)：
```
Intimate lo-fi bedroom pop ballad in C minor, 68 BPM, melancholic yet 
hopeful, dual-tracked breathy female lead with subtle male harmony on 
chorus, muted Rhodes MK II electric piano, fingerpicked nylon-string 
acoustic guitar, analog tape saturation with subtle vinyl crackle, 
brushed kit with ghost snares only on chorus, upright double bass with 
audible fret noise, ambient city night field recording, warm saturated 
tube compression, wide stereo field with mono bass, side-chained pad 
swells on chorus, midnight blue tonal mood, late-night radio aesthetic, 
no auto-tune, no 808 drums, no EDM drops
```

專業 token：Key/BPM 明確、dual-tracked vocal、樂器型號 (Rhodes MK II)、混音語彙 (tube compression, tape saturation, side-chained, mono bass, ghost snares)、Negative 放最後。

### Phase 5 — Song Title (可選)
- 位置：Styles 下方，**More Options** 折疊區之後
- 有 ♪ icon + placeholder "Song Title (Optional)"
- ref_79 和 ref_166 都可用 (ref_166 是底部那個)
- 可填中文 (實測「夜貓」✓)

### Phase 6 — More Options (未展開詳測)
- ⏳ 展開後預計可設 Instrumental / Persona / Weirdness / Exclude styles 等

### Phase 7 — Create 送出
**按鈕：** 底部大 Create 按鈕 (紫紅 gradient)，座標 ~(371, 585)
**下方文字：** "This will use **10 credits** to generate **2 songs**" (v4.5-all 免費 tier)
**送出後：** 按鈕變旋轉圖示 🔄，Suno 開始生成 2 版
**等待：** v4.5-all 約 30-90 秒

### Phase 8 — 成品 + 試聽 + 下載 (2026-04-20 實測中)

**🎁 免費 tier 行銷機制 (發現)：** 送出 `v4.5-all` 生 2 首後，Suno **額外附贈 2 首 `v5.5 Preview`** 同 prompt 的試聽片段 (1:08 秒)。這些 v5.5 Preview 旁有 `Upgrade for full song` 按鈕 — **只能試聽，要升級 Pro 才能下載完整版**。意義：免費帳號能**即刻感受** v5.5 品質，是 Suno 的 upsell 策略。

**Workspaces 列表：**
- 新作品插在列表最頂 (Newest 排序)
- 每項 row：
  - 左側封面 (loading 狀態是動畫圈)
  - 歌名 + model tag (v4.5-all / v5.5 Preview)
  - Style prompt 片段 (description 列)
  - 🖤 愛心 / 👎 / 📤 分享 icons
  - ⋯ 更多選項按鈕 (右側)
- 列表每 4-5 秒自動 refresh (生成完進度 icon 會變封面)

**試聽 (實測)：** 點封面圖 → 底部固定播放器出現：
- 左下：封面縮圖 + 歌名「夜貓」+ 作者「Hao0321」
- 中央：🔀 shuffle / ⏮ prev / **⏸ play/pause (粉色 active)** / ⏭ next / 🔁 repeat
- 進度條：當前時間 / 總長 (例 0:59 / 5:17)
- 右下：📝 lyrics / 👍 / 👎 / 💬 / 📤 share / ⋯ / 🔊 volume

**⋯ 選單 (row 右側 ~1513, 145) 展開完整選項：**
| 選項 | 免費可用？ |
|---|---|
| Remix/Edit (submenu) | ✓ |
| Create (submenu) | ✓ |
| **Get Stems** | ⛔ **Pro 專屬** (v4.5-all 免費無分軌) |
| Add to Queue | ✓ |
| Add to Playlist | ✓ |
| Move to Workspace | ✓ |
| Publish | ✓ |
| Song Details | ✓ |
| Generate Cover Art | ✓ |
| Visibility & Permissions (submenu) | ✓ |
| Share (submenu) | ✓ |
| **Download** (submenu) | 見下 |
| Report | ✓ |
| Move to Trash | ✓ |

**Download submenu：**
| 格式 | 可用性 |
|---|---|
| 🎵 **MP3 Audio** | ✅ 免費可下 |
| 🎵 **WAV Audio** | ⛔ **Pro 專屬** (無損音訊) |
| 🎬 **Video** | ⛔ **Pro 專屬** (Suno 自動生成配圖/動畫) |

**免費 tier 實際可拿到：** MP3 音檔 (完整歌曲長度 4-5 分鐘)。要 stems / WAV / video → 升級 Pro。

**v5.5 Preview 限制：**
- 預覽時長 ~1:08
- `Upgrade for full song` 按鈕位置：row 右側
- 下載不可 (除非升級)

### Phase 9 — 時長隨歌詞結構自動延長 (重要發現)

**實測：** 5-part 專業結構的《夜貓》(Intro / V1 / PC / C / V2 / PC / C / Bridge / Final C / Outro / End) 自動跑到 **5:17 + 4:28** (兩版不同長度)。Suno v4.5-all **不限定固定長度**，依歌詞 metatag 結構自動延長。

**意義：** 寫越完整的結構 (所有 metatags 齊全) → 得到越完整的歌。只寫 Verse + Chorus 可能只有 1-2 分鐘。

## 3. 座標速查 (1568×708 viewport, 2026-04-20 實測)

| 元素 | 座標 / ref |
|---|---|
| 頂部 Simple / Advanced 切換 | (304, 27) / (356, 27) |
| 右上 Model 下拉 (v4.5-all ▾) | (480, 27) |
| Lyrics textbox | ref_93 (動態) — 中央上方 |
| Lyrics 清空按鈕 (🗑 icon) | 標題列右側 |
| Styles textbox | ref_102 (動態) — Lyrics 下方 |
| Song Title textbox | ref_166 (底部) |
| More Options 折疊按鈕 | ~(342, 437) |
| **Create 按鈕** (紫紅) | (371, 585) |
| 左下 Credits 球 | (197, 27) |
| Workspaces 右側列表 | 作品點開播放 |
| Sale modal X (若出現) | (898, 165) |

## 4. 實測記錄

**2026-04-20 Phase 1-7 實測：**
- 帳號：hao0321，50 credits
- Model: v4.5-all (免費最佳)
- 模式：Advanced (Custom Mode)
- 作品：《夜貓》— 深夜鳥 IP 的主題曲
- Prompt：見 Memory `asset-library.md` 的 @深夜鳥-少女 配套
- 消耗：10 credits / 2 songs
- 結果：⏳ 待 Phase 8 驗證

---

## 3. 已知陷阱 (from references/suno.md)

- Style 欄塞歌詞 → 會當 tag 處理，出來一團糟
- Lyrics 欄用 "Verse:" 沒括號 → 被唱出來
- 忘記 `[End]` 或 `[Outro]` → 突然截斷或無限 loop
- 一首歌塞 10+ instruments → 混音擠爆。挑 4–6 個
- Negative 放在 Style 中間 → 要放最後
- 中文歌用純中文 Style 描述 → Style 欄對英文最準；要用英文描述曲風
- 同時指定衝突 mood (`happy and melancholic`) → 選一個 dominant

---

## 4. Auto-Pilot 進 Suno 時

**Intent Parser 特化：**
- 偵測使用者要「歌/音樂/BGM/配樂」→ 強制平台 Suno
- 解析「中文/英文/日文」→ 歌詞語言
- 解析「幾分鐘」→ v5 最長 4 分鐘
- 解析風格 (lo-fi / pop / rock / ballad / ambient / hip-hop)

**Prompt 自動拆成 Style + Lyrics 兩欄**，不要混。

---

## 5. 版本與更新紀錄

- **2026-04 (stub)：** 建立 profile 架構，等待實際操作補完

---

## 6. 🔥 SPEED OPTIMIZATION (2026-04-20 慘痛教訓)

**痛點：** 5 首歌跑 15 分鐘 + 3.9k token，user 罵「垃圾」。

**根因：**
1. 每步 screenshot 驗證 (~1.5k token/張，跑了 8+ 張 = 12k+ 浪費)
2. clear field 用 3 步 (triple_click + Ctrl+A + Delete) 而非 1 步點垃圾桶
3. TodoWrite 每首一次 (沒必要，5 首是線性 chain)
4. 歌詞寫太長 (50 行 vs 標準 25 行)，type 翻倍

### 最佳 Suno chain SOP (每首 7 calls，5 首 = 35 calls 總)

**前置 (1 次)：** navigate to suno.com/create + 1 screenshot 確認 UI

**每首 7 calls：**
```
1. left_click (437, 156)    → 點 Lyrics 區的 🗑 icon (清空)
2. left_click (437, 327)    → 點 Styles 區的 🗑 icon (清空)
3. left_click (340, 380)    → focus Style textarea
4. type <style prompt>       → 一次貼完 (含 BPM/Key/樂器/negative)
5. left_click (340, 220)    → focus Lyrics textarea
6. type <lyrics>             → 一次貼完 (~25 行最佳)
7. left_click (373, 585)    → click Create
```

**禁忌：**
- ❌ 中間不要 screenshot — Suno UI 座標穩定 (1568×708)，不用驗證
- ❌ 不要 TodoWrite 每首 — 5 首是 chain，跑完一次更新就好
- ❌ 不要寫 50 行歌詞 — 流行歌標準 = Intro + V1 + PC + C + V2 + C + Bridge + Final C + Outro = ~25 行
- ❌ 不要 triple_click + Ctrl+A + Delete — 直接點 🗑 icon 一鍵清空

**驗證點 (僅 2 次)：**
- 第 1 首送出後 1 張 screenshot 確認 credits 扣了 + 新 entry 出現
- 全部跑完 1 張 screenshot 確認 5 首都在 workspace

**預期效能：** 5 首歌 < 5 分鐘 + < 1.5k token (節省 70%)

**陷阱：** Style/Lyrics 欄左上有 ↩(undo) / 📌 (pin) / 🗑 (trash) / 🪄 (magic) icons
- Lyrics 🗑 icon 約 (437, 156)
- Styles 🗑 icon 約 (437, 327)
- 點下去會立刻清空整個欄位，不彈確認 (✓ 安全)
