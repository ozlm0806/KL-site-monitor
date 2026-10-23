# 開楠營造有限公司｜工地環境安全即時監控系統

**網址：** <https://ozlm0806.github.io/KL-site-monitor/>　｜　後台：<https://ozlm0806.github.io/KL-site-monitor/admin.html>

純前端網頁（PWA），可放在 GitHub Pages 免費架設。手機可「加入主畫面」，電腦（Chrome / Edge）可「安裝 APP 到桌面」。

## 功能
- 降雨監控、氣溫 / 熱危害、風速 / 陣風、地震監控四大數據卡
- 熱指數表（自動標示目前溫濕度所在格子，依勞動部熱危害分級上色）
- 中央氣象署雷達回波（點圖放大，近 2 小時動畫）＋ **Windy 圖資**（風、陣風、雨、雷達、衛星、氣溫…可切換）
- 作業安全研判：當日累積、熱危害、紫外線、吊車 / 塔吊、高處作業、混凝土澆置 / 開挖、鋼構塗裝 / 防水、氣象署特報、颱風、地震
- 逐時趨勢（過去 12 ＋ 未來 12 小時，點面板看逐時明細）
- 底部警示跑馬燈（紅色警示出現時發出提示音，可靜音）
- 工地氣象日報表（列印 / 另存 PDF、另開視窗、下載 HTML，含三方簽名欄）
- 多工地切換按鈕（按鈕上的燈號顯示各工地即時狀態）、地名搜尋、GPS 目前位置
- **管理後台**（admin.html）：管理者登入、新增 / 編輯 / 停用 / 排序工地（地圖點選 GPS 座標）、系統設定與門檻、管理者帳號、發佈到 GitHub

## 資料來源
| 資料 | 來源 | 金鑰 |
|---|---|---|
| 逐時預報、模式推估值 | Open-Meteo | 免 |
| 雷達回波圖 | 中央氣象署網站 | 免 |
| 測站實測（溫度、風、雨量站）、有感地震、天氣特報、颱風 | 中央氣象署開放資料 | **需要**（免費申請） |
| 地震（未設金鑰時） | USGS（本地震度為估算） | 免 |
| Windy 圖資 | embed.windy.com | 免 |

> 建議到 <https://opendata.cwa.gov.tw/user/authkey> 申請氣象署金鑰，登入後台「系統設定」填入，才有測站實測、特報與颱風研判。

## 部署到 GitHub Pages（第一次）
1. 登入 GitHub → 右上「＋」→ **New repository**，名稱例如 `site-monitor`，選 **Public**，建立。
2. 進入儲存庫 → **Add file → Upload files**，把本資料夾內下列檔案 / 資料夾全部拖進去後按 **Commit changes**：
   `index.html`、`admin.html`、`manifest.webmanifest`、`sw.js`、`.nojekyll`、`css/`、`js/`、`icons/`、`data/`
   （影片 mp4 與 KL_LOGO.jpg 不需上傳）
3. 儲存庫 **Settings → Pages** → Source 選 **Deploy from a branch**，Branch 選 `main`、資料夾 `/ (root)` → Save。
4. 約 1～2 分鐘後網址會出現在同一頁，例如 `https://你的帳號.github.io/site-monitor/`，把這個網址分享給同仁即可。

## 管理者第一次使用
1. 開啟 <https://ozlm0806.github.io/KL-site-monitor/admin.html>
2. 預設帳號 **admin**、密碼 **KL@admin2026**，登入後系統會要求立即變更密碼。
3. 在「工地專案」新增工地：輸入名稱、縣市，用「查座標」或直接在地圖點選設定 GPS。
4. 在「系統設定」填入氣象署金鑰、調整研判門檻。
5. 到「發佈 / 備份」發佈（見下方權杖說明），同仁重新整理就會看到新工地。

### 發佈用的 GitHub 權杖（只需設定一次）
1. GitHub 右上頭像 → **Settings → Developer settings → Personal access tokens → Fine-grained tokens → Generate new token**
2. Repository access 選 **Only select repositories** → 選 `site-monitor`
3. Permissions → Repository permissions → **Contents：Read and write**
4. 產生後複製 `github_pat_…`，貼到後台「發佈 / 備份」，可勾選「在此裝置記住權杖」。

> 不想用權杖也可以：後台按「下載 config.json」，再到 GitHub 儲存庫 `data/` 資料夾上傳覆蓋同名檔案。

## 安全性說明
- 網站是靜態網頁，後台登入驗證在瀏覽器端進行（密碼以 SHA-256 加鹽雜湊存在 config.json）。**真正能修改網站內容的是 GitHub 權杖**，請勿外流；管理者密碼請設 10 碼以上。
- 氣象署金鑰會公開在 config.json 中（免費、唯讀資料）；若遭濫用可隨時在氣象署網站重新產生。

## 安裝成 APP
- **Android / Chrome**：開啟網址 → 右上選單「安裝應用程式 / 加到主畫面」，或點畫面上的「⬇ 安裝 APP」。
- **iPhone / iPad**：用 Safari 開啟 → 分享 ⬆︎ →「加入主畫面」。
- **Windows / Mac 電腦**：Chrome 或 Edge 開啟 → 網址列右側「安裝」圖示，或點「⬇ 安裝 APP」，會在桌面建立捷徑。

## 本機測試
```bash
python -m http.server 8765
```
然後開啟 <http://localhost:8765/>。

## 檔案結構
```
index.html          監控主畫面
admin.html          管理後台
manifest.webmanifest / sw.js   PWA 安裝與快取
css/style.css, css/admin.css
js/core.js          共用計算（熱指數、風級、震度…）
js/data.js          資料擷取（Open-Meteo / 氣象署 / USGS）
js/app.js           主畫面邏輯與作業安全研判
js/report.js        工地氣象日報表
js/admin.js         後台邏輯與 GitHub 發佈
data/config.json    工地清單、管理者、系統設定（後台發佈即更新此檔）
icons/              Logo 與 APP 圖示（由 KL_LOGO.jpg 轉檔）
```

門檻為一般參考值，停工與復工仍依現場實測、契約規定與監造指示。
