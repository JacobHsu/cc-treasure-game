---
description: 將 Vite 專案建置並部署到 GitHub Pages（gh-pages 分支），最終回報部署網址
---

# Deploy to GitHub Pages

執行完整的 GitHub Pages 部署流程。本專案使用 Vite，產出位於 `build/` 目錄，部署到 `gh-pages` 分支。

## Step 1 — 部署前檢查

1. 執行 `git status`，確認工作區乾淨。若有未提交檔案，停止並提示使用者先 commit 或 stash。
2. 執行 `git branch --show-current`，記錄目前分支（通常為 `main`）。
3. 執行 `git remote get-url origin`，擷取 repo 資訊：
   - 從 URL 解析出 `<owner>` 與 `<repo>` 名稱
   - 例如 `git@github.com:JacobHsu/cc-treasure-game.git` → owner=`JacobHsu`, repo=`cc-treasure-game`
4. 預期部署網址為：`https://<owner>.github.io/<repo>/`
5. 檢查 `vite.config.*` 中的 `base` 設定：
   - GitHub Pages 子路徑部署需要 `base: '/<repo>/'`
   - 若未設定或設為 `/`，警告使用者並提示需在 `vite.config.ts` 加上 `base: '/cc-treasure-game/'`，否則資源路徑會壞掉
   - 詢問使用者是否要自動修改，或先停止讓使用者確認

## Step 2 — 安裝依賴與建置

1. 執行 `npm install`，確保依賴齊全。
2. 執行 `npm run build`，產出至 `build/`。建置失敗則停止並回報錯誤。
3. 確認 `build/index.html` 存在。
4. 在 `build/` 內建立 `.nojekyll` 空檔案（避免 GitHub Pages 把底線開頭的檔案忽略）：
   ```bash
   touch build/.nojekyll
   ```

## Step 3 — 部署到 gh-pages 分支

優先使用 `gh-pages` npm 套件（若未安裝則臨時用 `npx`）：

```bash
npx -y gh-pages -d build -b gh-pages -m "deploy: $(git rev-parse --short HEAD)"
```

- `-d build`：部署 `build/` 目錄內容
- `-b gh-pages`：推送到 `gh-pages` 分支
- `-m`：commit 訊息附上來源 commit hash

若指令失敗：
- 檢查是否有 push 權限
- 檢查 SSH key 是否設定
- 回報完整錯誤訊息並停止

## Step 4 — 部署後健康驗證

1. GitHub Pages 部署後通常需要 30 秒到數分鐘才會生效。先等候 30 秒：
   ```bash
   sleep 30
   ```
2. 對部署 URL 執行 `curl -I -s -o /dev/null -w "%{http_code}" https://<owner>.github.io/<repo>/`
3. 預期回傳 200。若回傳 404：
   - 可能 GitHub Pages 尚未啟用，提示使用者到 repo Settings → Pages 確認 Source 設為 `Deploy from a branch` → `gh-pages` / `/ (root)`
   - 或是部署仍在進行中，建議稍後再次驗證

## Step 5 — 回報部署結果

成功時以下列格式回報：

```
✅ GitHub Pages 部署完成

🌐 URL: https://<owner>.github.io/<repo>/
📦 來源分支: <分支名稱>
🔖 來源 Commit: <commit hash 短碼> - <commit 訊息>
🌿 部署分支: gh-pages
⏱️  部署耗時: <秒數>
🩺 健康檢查: <HTTP 狀態碼>（注意：剛部署可能需 1-3 分鐘才生效）
```

失敗時：

```
❌ 部署失敗於 Step <N>: <步驟名稱>

錯誤訊息:
<error output>

建議下一步:
<根據錯誤類型給出建議，例如：檢查 vite.config base、檢查 GitHub Pages 設定、檢查 SSH 權限等>
```

## 注意事項

- 全程使用 Bash tool 執行，必須讀取實際輸出，不要假設成功。
- 不要強推（`--force`）gh-pages 分支，`gh-pages` npm 套件已正確處理歷史。
- 若使用者第一次部署，提醒到 GitHub repo Settings → Pages 確認 Source 設定。
- `vite.config` 的 `base` 設定錯誤是最常見的部署後白畫面原因，務必在 Step 1 確實檢查。
