# ai-media-generator

> **零門檻做出專業 AI 影片 / 圖片 / 音樂 — 讓 Codex 或 Claude Code 幫你套上「資深導演級」提示詞。**
>
> Zero-skill cinema. Senior-director prompts on autopilot.

說「做個古代將軍騎馬衝鋒的電影感短片」就好。Agent 會：

1. **挑對平台** —— Seedance 寫實武打？Veo 3.1 要原生音效？Kling 3.0 / Runway Gen-4.5 要寫實運鏡？這個 skill 知道（Sora 2 🔴 已停運，見下）
2. **寫對提示詞** —— 不是 `cinematic, 8k, beautiful` 這種沒用的詞，而是 Deakins、Lubezki、Kodak Vision3 500T、teal-orange grade、Constraints tail（不抖動、不變形…）這類「平台真正吃」的 token
3. **直接操作網站** —— 透過瀏覽器 MCP 把提示詞送上 OiiOii / Flow / Kling / Suno，按完所有按鈕，把成品 fetch 回來

涵蓋 14+ 平台：Midjourney V8.1、Flux 1.1 Pro、Sora 2（🔴 已停運）、Veo 3.1、Kling 3.0、Seedance 2.0／2.5（依 provider 能力閘）、Suno v5.5、Runway Gen-4.5、Ideogram 3、Seedream 5.0、Nano Banana Pro、Vidu Q3、Stable Diffusion、OiiOii…

---

**A Codex and Claude Code Skill for zero-skill, senior-quality AI media generation across 14+ platforms.**

You say "make a cinematic shot of an ancient general charging on horseback." The agent handles the rest — picking the right platform, writing the platform-specific signature prompt (no more `beautiful, masterpiece, detailed` filler — actual director / DP / film-stock / lens / Constraints-tail vocabulary, calibrated to what the target model actually parses), and driving the browser to submit it.

## 為什麼需要這個 skill？

**因為「提示詞會寫」是門高門檻 — 而且每個平台寫法都不同。**

一般人寫的 prompt：
```
cinematic shot of an ancient general on a horse, 8k, beautiful, masterpiece, detailed
```
→ 平台吃不到方向，出來的成品永遠像「AI 圖庫」。

這個 skill 寫的 prompt（Seedance 中文戰鬥範例）：
```
古代中國將軍 Aria 主角，身披金色鱗甲、紅披風在風中翻飛，手持青銅寶劍前舉，
騎黑色戰馬在黎明戰場上緩慢衝鋒。戰場遠處千軍列陣、紅色戰旗獵獵、塵霧漂浮。
金色逆光晨霧、熒光輪廓。低角度手持 tracking 鏡頭沿馬側跟拍，輕微晃動、穩定地平線。
墨色留白東方美學、史詩寫實電影感。慢動作、流暢、連貫、不僵硬、720p 高清。
不抖動、不變形、不多肢、穩定地平線、穩定時間一致性。
```
→ 平台懂的每個細節都到位：8 維公式（Subject+Action+Scene+Light+Camera+Style+Quality+Constraints），符合 Seedance 中文訓練、單動詞、Constraints tail。

**差別不在你聰不聰明 — 在你知不知道每個平台吃什麼 token。** 這個 skill 就是把那份「該怎麼寫」的內部知識，從跨平台研究（X / Threads / Reddit / 小紅書 / Bilibili + 官方 cookbook）+ 兩輪 head-to-head benchmark 抓出來，塞給 Claude 用。

---

## Why this exists (English)

Generative-AI prompts are **not portable**. The same idea sent to Flux vs Midjourney vs Seedance produces wildly different quality, because each model was trained on different captioning conventions:

- **Midjourney V8.1** loves comma-chunked detail + `--style raw --stylize 750`
- **Flux** strips out director names but rewards 80-200 word natural paragraphs with technical photography vocabulary
- **Seedance** (中文訓練) treats English director names as weak signal — it wants concrete visual nouns in Chinese, plus a Constraints tail (`不抖動、不變形…`)
- **Sora 2** wants "format anchors" (`bodycam footage`, `surveillance`) and quoted dialogue — 🔴 app/web 已於 2026-04-26 停運（API 撐到 2026-09-24），新任務改用 Runway Gen-4.5 / Veo 3.1 / Kling 3.0
- **Veo 3.1** is the only model where SFX / Ambient tags actually generate audio

This skill captures those signatures — both from official cookbooks (OpenAI, Google Cloud, fal.ai, BFL) and from real-world community posts on X / Threads / Reddit / 小紅書 / Bilibili — into references an agent can load on demand.

## v2.2 highlights

- Seedance 2.5 community prompt patterns with provider capability gates.
- 30-second continuity, first-shot locking, multi-reference, time-freeze／reverse-time and complex-physics workflows.
- Domain playbooks for commercial, real estate, advertising, products, automotive, food, film and animation.
- Codex UI metadata and reusable evaluation cases.

## What's inside

```
ai-media-generator/
├── SKILL.md                      # Top-level skill (auto-pilot + hard rules)
├── references/                   # Platform-specific prompt guides (14 platforms)
│   ├── community-prompt-patterns.md   # ⭐ cross-platform meta + per-model signatures
│   ├── seedance-2-5-community-playbook.md # community patterns + 30s / multi-ref / UGC
│   ├── seedance-domain-playbook.md         # commercial / real estate / auto / food / film / animation
│   ├── seedance-longform-continuity.md     # identity locks / reverse time / complex physics
│   ├── cinematic-direction.md         # advanced director / DP / film-stock vocabulary
│   ├── commercial-direction.md
│   ├── vfx-effects.md
│   ├── sound-design.md
│   ├── editing-transitions.md
│   ├── camera-language.md
│   ├── selector.md                    # which platform for which use case
│   ├── kling.md / seedance.md / sora.md / veo.md / vidu.md / runway.md
│   ├── midjourney.md / flux.md / ideogram.md / seedream.md / stable-diffusion.md
│   ├── nano-banana.md / suno.md / oiioii.md
├── templates/
│   ├── auto-pilot.md             # one-line-to-output pipeline
│   ├── preset-packs.md           # 30+ ready-made preset prompts
│   ├── storyboard.md
│   ├── music-video.md
│   ├── negative-bank.md
│   ├── user-flags.md             # natural-language flag translator
│   └── token-efficient-mode.md   # lazy-load / grep / subagent strategy
└── automation/                   # browser automation protocols
    ├── browser-guide.md
    ├── click-protocol.md         # reliable-click SOP + token optimization
    └── site-profiles/            # deep UI maps for verified platforms
        ├── oiioii.md                  # ✅ Phase 1-3E + Seedance §12.9 deep playbook
        ├── flow.md                    # ✅ Veo 3.1 complete
        ├── kling.md                   # ✅ Kling 3.0 complete
        ├── suno.md                    # ✅ Suno v5.5 complete
        └── (stubs for midjourney/seedream/runway/sora/vidu/ideogram)
```

## Key concepts

### 🔴 The Meta Rule (priority order)

> **Writing the prompt correctly once ≫ submitting it fast 10 times.**

A wrong prompt costs ~10 minutes of regeneration + token waste. A slow submit costs ~5 minutes. So the speed-optimization priority is:

1. **Look up the platform signature** → `references/community-prompt-patterns.md`
2. **Optimize single submission** → `automation/site-profiles/<platform>.md`
3. **Wait without polling** → `Bash run_in_background:true + sleep N`

### 🔴 The Hard Rule

Every prompt **must** embed 5-8 high-signal tokens from the appropriate vocabulary layer (director / DP / lens / film-stock / lighting / grading / composition / VFX). Generic words (`cinematic`, `8k`, `beautiful`, `masterpiece`) are banned — they dilute signal.

**Platform-aware exception:** Director names work on Midjourney / Sora 2 / Veo but **degrade** Flux / Nano Banana Pro / Seedance / Wan output. The skill bakes this into the selection logic.

### 🤖 Auto-Pilot mode

When a user says "make me a 15-second cinematic ad of X" the skill:

1. Parses the request into 9 slots (medium / duration / aspect / subject / style / character / scene / audio / language)
2. Fills defaults
3. Selects the right platform via `selector.md`
4. Auto-generates the storyboard + prompt
5. Drives the browser via `site-profiles/<platform>.md`
6. Reports back

## Installation

Clone the folder into the skills directory used by your agent:

```bash
# Codex, per-user
git clone https://github.com/Hao0321/ai-media-generator.git ~/.codex/skills/ai-media-generator

# Claude Code, per-user
git clone https://github.com/Hao0321/ai-media-generator.git ~/.claude/skills/ai-media-generator

# Claude Code, project-level
git clone https://github.com/Hao0321/ai-media-generator.git ./.claude/skills/ai-media-generator
```

Codex and Claude Code discover `SKILL.md` files inside their configured skills directories.

## Requirements

- **Codex**, **Claude Code**, or another client that supports the Skills format.
- For browser automation: a compatible browser-control tool or MCP server connected.
- For specific platforms you want to drive: a logged-in account on that platform in the same browser session.

## Verified platforms (browser automation)

| Platform | Site profile status | Notes |
|---|---|---|
| OiiOii.ai | ✅ Verified | Phase 1-3E + Seedance 2.0 pro deep playbook |
| Google Flow (Veo 3.1) | ✅ Verified | Fast / Quality / Lite modes mapped |
| Kling 3.0 | ✅ Verified | + Fast-Track + Native Audio |
| Suno v5.5 | ✅ Verified | 5-song chain SOP |
| Midjourney V8.1 | 📝 Stub | Reference only |
| Seedream 5.0 / Runway Gen-4.5 / Vidu Q3 / Ideogram 3 | 📝 Stub | Prompt guides complete; site profiles pending |
| ~~Sora 2~~ | 🔴 停運 | app/web 2026-04-26 已關，API 撐到 2026-09-24；新任務改用 Runway Gen-4.5 / Veo 3.1 / Kling 3.0 |

Prompt-craft reference files exist for all 14+ platforms regardless of automation status.

## Provenance

This skill was built iteratively against real-world tasks (storyboarding, music-video production, cinematic shorts) with two rounds of head-to-head benchmarking. Iteration-1 measured "with-skill vs no-skill" baseline (95% vs 47%). Iteration-2 specifically measured "senior-director thinking" (vocabulary depth) — with-skill won 3/3 head-to-head subjective evaluations.

The community-prompt-patterns.md file consolidates research across:

- Official cookbooks: OpenAI Sora 2 (🔴 已停運，歷史參考), Google Cloud Veo 3.1, Runway Gen-4.5 / Aleph, fal.ai (Kling, Wan, Seedream), BFL (Flux Kontext), Ideogram, Midjourney V8.1 docs, Google Nano Banana
- Community posts: X / Threads / Reddit / 小紅書 / Bilibili
- Practitioner write-ups: Atlas Cloud Seedance Library, awesome-seedance-2-prompts (GitHub), 知乎 / 腾讯新聞 / CSDN 即夢 Seedance 手册

## License

MIT — see [LICENSE](LICENSE).

## Contributing

This is an opinionated, evidence-driven skill. Contributions welcome — especially:

- New `site-profiles/<platform>.md` for platforms that don't have one yet
- Updates when platforms ship new versions (model signatures change quickly)
- Better presets in `templates/preset-packs.md`

See [CHANGELOG.md](CHANGELOG.md) for version history.

## Acknowledgments

Inspired by users who got tired of "cinematic, 8k, masterpiece" prompts and wanted Claude to write like a senior director instead.
