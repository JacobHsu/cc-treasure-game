# Use Claude Code to Explore and Develop the project 
### Download the zip file of branch 'initial'
https://github.com/uopsdod/claude_code_treasure_game/tree/initial

### initialize the context
/clear
/init: generate the CLAUDE.md to understand how this project works 

npm install
npm run dev'

### specify file to the current context 
> use @src/audios/chest_open.mp3 in the @src/App.tsx to play the sound effect of the chest being opened. do not do anything else.

### add more to the context
> check the comments of existing changes

type '#' first 
"Always add comments on the top of every new function in one line to summarize the usage and Must document the inputs and output parameters" 
> 2. Project memory

> use @src/audios/chest_open_with_evil_laugh.mp3 in the @src/App.tsx to play the sound effect of the chest with skeleton inside being opened  

> check the comments of existing changes

### use screenshot to develop intuitively 
> screenshot and mark the area you want the change to be. 
> [!image] show the results to be either: win, tie, loss in the circled place according to the final score 

### Challenge: change the hover mouse point icon
use the src/assets/key.png icon when the mouse hovers over the closed treasure box

### manage context 
/context 
54k/200k tokens (27%)

> (optional) go through the url recursively to understand everything about SQLite and add all information in the context. 
https://nodejs.org/docs/latest/api/sqlite.html                                                      

/compact

/context 
> 26k/200k tokens (13%)

/clear 

/context
> 16k/200k tokens (8%)

> "Check my project with AngularJS to see if any syntax error."

esc + esc > select a conversation to go back 

### Plan mode: 
make a commit to store the current state

shift + tab 
> "What database options I have to implement sign up and sign in flow?"
> "how about SQLite as local storage?"
> "use SQLite to build a simple sign up and sign in flow and store the game score for each signed in user. In addition, allow to play the game as guest mode without storing any data."

> Ctrl + T: See the To-Do List 

### Ultrathink 
revert back to previous git commit 

> "Ultrathink to use SQLite to build a simple sign up and sign in flow and store the game score for each signed in user. In addition, allow to play the game as guest mode without storing any data."

> Ctrl + T: See the To-Do List 

### custom command - Vercel deployment
- create folder: .claude/commands
- create file: deploy_vercel.md
- after creation, re-open a new claude code session

### custom command - Github Page deployment
- create folder: .claude/commands
- create file: deploy_github_page.md 
- after creation, re-open a new claude code session

### prompt 
            
`當滑鼠移到寶箱時，滑鼠游標改為 @src/assets/key.png`    
滑鼠游標螢幕截圖  Windows 的 Win + Shift + S 螢幕錄影 → 撥放器mp4影像截圖  

### Favicon 製作教學

用 `src/assets/treasure_closed.png` 作為網站 favicon 來源，透過線上工具產生完整的 favicon 套件（含多種尺寸與 `.ico` 格式）。

**步驟：**

1. 開啟線上 favicon 產生器（擇一）：
   - https://favicon.io/favicon-converter/ （簡單版，快速產出）
   - https://realfavicongenerator.net/ （完整版，支援 PWA / Apple / Android 圖示）

2. 上傳來源圖：`src/assets/treasure_closed.png`

3. 下載產生的壓縮包，解壓後會包含：
   - `favicon.ico` — 主要 favicon（多尺寸合一）
   - `favicon-16x16.png` / `favicon-32x32.png` — 瀏覽器分頁圖示
   - `apple-touch-icon.png` — iOS 加入主畫面圖示
   - `android-chrome-192x192.png` / `android-chrome-512x512.png` — Android PWA 圖示

4. 將以上所有檔案放入專案根目錄的 `public/` 資料夾（Vite 會自動將 `public/` 內容複製到 build 輸出根目錄）。

5. 在 `index.html` 的 `<head>` 中加入引用：
   ```html
   <link rel="icon" type="image/x-icon" href="./favicon.ico" />
   <link rel="icon" type="image/png" sizes="32x32" href="./favicon-32x32.png" />
   <link rel="icon" type="image/png" sizes="16x16" href="./favicon-16x16.png" />
   <link rel="apple-touch-icon" sizes="180x180" href="./apple-touch-icon.png" />
   ```

6. 執行 `npm run build` 驗證輸出，瀏覽器即可看到新的 favicon。
   - 若 favicon 沒更新，可能是瀏覽器快取，按 `Ctrl + Shift + R` 強制重新整理。
