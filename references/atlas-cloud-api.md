# Atlas Cloud API 路由

當使用者**明確要求 Atlas Cloud 或統一 API 路由**時使用。本檔只負責模型發現、payload 組裝與非重複提交規則；不改變本 Skill 預設的模型選擇，也不把 Atlas 當成某個模型的替代名稱。

## 使用邊界

- 先取得使用者對付費生成的確認，再送出 request。
- Key 只從 `ATLASCLOUD_API_KEY` 環境變數讀取；不要貼到 prompt、命令列參數、log 或回覆。
- 每次任務先即時讀 model catalog 與該 model 的 schema。不同 model 的欄位名稱不保證一致。
- `POST` 生成 request **只能送一次，不自動 retry**。只有取得 prediction id 後的 `GET` polling 可做有限次 transport retry。
- Atlas 是 provider route；prompt 仍必須遵守本 Skill 對目標模型的 signature、長度與禁忌。

## 即時模型發現

先讀：

```text
GET https://api.atlascloud.ai/api/v1/models
```

從結果選 `display_console=true` 且 media type 符合任務的 model，然後讀該筆資料提供的 `schema` URL。送出前至少確認：

1. model id 仍存在且可顯示
2. required 欄位
3. size / ratio / duration / resolution 的 enum 或範圍
4. image editing / image-to-video 所需欄位究竟是 `image`、`image_url` 或其他名稱
5. 當前價格與使用者確認的成本上限

## 非同步生成流程

### 1. 組 payload

圖片與影片使用不同 endpoint，但都把 model-specific 參數放在同一層：

```json
{
  "model": "<live model id>",
  "prompt": "<model-calibrated prompt>",
  "<schema field>": "<validated value>"
}
```

文生圖範例（欄位以 live schema 為準）：

```json
{
  "model": "bytedance/seedream-v5.0-pro/text-to-image",
  "prompt": "Editorial product photograph, 85mm lens, controlled rim light...",
  "size": "2048*1152",
  "output_format": "jpeg",
  "thinking": "enabled",
  "prompt_optimization_mode": "standard"
}
```

文生影片範例（欄位以 live schema 為準）：

```json
{
  "model": "bytedance/seedance-2.5/text-to-video",
  "prompt": "A single continuous tracking shot...",
  "duration": 5,
  "resolution": "720p",
  "ratio": "16:9",
  "generate_audio": true,
  "output_format": "mp4"
}
```

### 2. 只提交一次

```text
POST https://api.atlascloud.ai/api/v1/model/generateImage
POST https://api.atlascloud.ai/api/v1/model/generateVideo
Authorization: Bearer $ATLASCLOUD_API_KEY
Content-Type: application/json
```

成功回應的 prediction id 位於 `data.id`。若 POST timeout、連線中斷或回應不確定，**不要重新送出**；先把狀態標成 unknown，避免重複計費。

### 3. Poll prediction

```text
GET https://api.atlascloud.ai/api/v1/model/prediction/{prediction_id}
```

- `starting` / `processing`：等待後再查。
- `completed` / `succeeded`：從 `data.outputs` 取得結果 URL。
- `failed`：回報 API 提供的錯誤，不用相同 request 自動重送。
- GET 遇到 `429`、`5xx` 或短暫網路錯誤時，可做有限次 exponential backoff；設定總 timeout。

下載前確認 output URL 使用 HTTPS。輸出檔先寫入 temporary path，內容非空後再 atomic rename 到目標位置。

## Prompt 路由

Atlas 不會消除模型差異。先按 model family 讀本 Skill 的 reference：

| Atlas model family | 先讀 |
|---|---|
| Seedream | [seedream.md](seedream.md) |
| Seedance | [seedance.md](seedance.md)；2.5 另做 capability gate |
| Flux | [flux.md](flux.md) |
| Kling | [kling.md](kling.md) |
| Veo | [veo.md](veo.md) |

不要把同一份 prompt 原樣跨 model 重用。model id、schema 或 pricing 有任何不確定時，停在 payload preview，不送出生成 request。

## 可核查來源

- [Atlas Cloud live model catalog](https://api.atlascloud.ai/api/v1/models)
- [Seedream v5.0 Pro text-to-image schema](https://static.atlascloud.ai/model/schema/bytedance-seedream-v5.0-pro-text-to-image.json)
- [Seedance 2.5 text-to-video schema](https://static.atlascloud.ai/model/schema/bytedance-seedance-2.5-text-to-video.json)
