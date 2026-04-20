# R2MS_Lite_ERTMaker 開發筆記
+ 作者: HsiupoYeh  
+ 更新日期: 2026-04-19
+ 從 **R2MS_Lite_ERTMaker_v1.0(v20260419a).md** 開始製作安裝包。

## 安裝 Inno Setup
+ 官方網站: https://jrsoftware.org/isinfo.php
+ 官方Github: https://github.com/jrsoftware/issrc
+ 版本: innosetup-6.7.1.exe
  + 安裝檔下載連結: https://github.com/jrsoftware/issrc/releases/tag/is-6_7_1
  + 安裝時語言選擇「English」。

### 步驟
+ 雙擊桌面的「Inno Setup Compiler」圖示，開啟「Inno Setup」主程式。
+ 會跳出歡迎訊息，選擇「Cancel」，將會關掉這個視窗。
+ 選「File>New」，將開啟「Inno Setup Script Wizard」，維持沒有打勾，按下「Next」。
+ 然後要填入應用程式名稱:
  + Application name: `R2MS_Lite_ERTMaker_v1.0`
  + Application version: `1.0`
  + Application publisher: `HsiupoYeh`
  + Application website: 空白
  + 以上填好之後按「Next」。
+ 接著要填「Application Folder」:
  + Application destination base folder: 選擇 「(Custom)」，然後下面一格填: `C:\`
  + Application folder name: `R2MS_Lite_ERTMaker_v1.0`
  + Allow user to change the application folder: 改成不要打勾
  + The application doesn't need a folder: 維持不要打勾
  + 以上填好之後按「Next」。
+ 接著處理:
  + 按「Browse...」，去選擇編譯好的「R2MS_Lite_ERTMaker.exe」。
  + Allow user to start the application after installation has finished: 改成不要打勾
  + The application doesn't have a main executable file: 維持不要打勾
  + Other application files:
