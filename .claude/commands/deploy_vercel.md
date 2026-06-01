---
description: 部署專案到 Vercel Production 環境，包含部署前檢查、建置測試、部署、健康驗證與網址回報
---

# Deploy to Vercel (Production)

執行完整的 Vercel 正式環境部署流程。依序完成以下步驟，任何一步失敗就停止並回報原因。

## Step 1 — 部署前檢查

1. 執行 `git status`，確認工作區乾淨（無未提交變更）。若有未提交檔案，停止並提示使用者先 commit 或 stash。
2. 執行 `git branch --show-current`，確認目前在 `main` 分支。若不在，停止並詢問是否確定要從此分支部署 production。
3. 確認 `vercel` CLI 已安裝：`vercel --version`。若未安裝，提示使用者執行 `npm i -g vercel`。
4. 確認專案已連結 Vercel：檢查 `.vercel/project.json` 是否存在。若不存在，提示先執行 `vercel link`。

## Step 2 — 本地建置與測試

1. 安裝依賴：`npm install`（或依專案使用的套件管理器：pnpm / yarn / bun）。
2. 執行建置：`npm run build`。建置失敗則停止並回報錯誤訊息。
3. 若 `package.json` 有定義 `test` script，執行 `npm test`。測試失敗則停止。
4. 若有 lint script，執行 `npm run lint`（可選，失敗時警告但不中斷）。

## Step 3 — 執行 Production 部署

1. 執行 `vercel --prod --yes`，將輸出記錄下來。
2. 從輸出中擷取部署 URL（通常為 `https://<project>.vercel.app` 或自訂 domain）。
3. 若部署指令回傳非 0 結束碼，停止並回報完整錯誤輸出。

## Step 4 — 部署後健康驗證

1. 對部署 URL 執行 `curl -I -s -o /dev/null -w "%{http_code}" <URL>`。
2. 確認回傳 HTTP 200（或其他預期狀態碼）。
3. 若狀態碼非 2xx/3xx，警告使用者並仍回報部署網址供手動檢查。

## Step 5 — 回報部署結果

最終以以下格式回報給使用者：

```
✅ 部署完成

🌐 Production URL: <部署網址>
📦 Branch: <分支名稱>
🔖 Commit: <最新 commit hash 短碼> - <commit 訊息>
⏱️  部署時間: <花費秒數>
🩺 健康檢查: <HTTP 狀態碼>
```

若任何步驟失敗，改以下列格式回報：

```
❌ 部署失敗於 Step <N>: <步驟名稱>

錯誤訊息:
<error output>

建議下一步:
<根據錯誤類型給出建議>
```

## 注意事項

- 全程使用 Bash tool 執行命令，不要假設輸出結果，必須讀取實際輸出。
- 不要使用 `--force` 或 `--no-verify` 等繞過檢查的參數。
- 若使用者在 Step 1 的檢查中被擋下，等待使用者確認後再繼續，不要自動跳過。
