# lazarus_study
+ 好消息是很熟悉的操作，壞消息是要學一下pascal。
+ 安裝雖然簡單，但是要裝得有點多...慢慢改吧

### 安裝
+ 2024-10-22，安裝最新版主程式(lazarus-3.6-fpc-3.2.2-win64.exe)到Win10中。
  + 優先建議安裝英文版，預設安裝路徑及選項。
  + 介面語言有簡體中文，語言都是安裝後可以再進去改的。
    
### 重要其他安裝

+ Indy10:
  + https://www.youtube.com/watch?v=JyjCw8WG05E
  + https://drive.google.com/file/d/15aHVXIQZ_ahNILHefhdUVMz_AK4laX01/view
  + 將檔案解壓縮後，找到Indy10的資料夾(下一層是有一堆檔案的):
    ```
    .\Indy10\  
    .\Indy10\Core\  
    ...  
    ```
  + 把剛剛這個資料夾複製到「C:\lazarus\components」資料夾內:
    ```
    C:\lazarus\components\Indy10\
    C:\lazarus\components\Indy10\Core\
    ...  
    ```
  + 開啟主程式，「Package」>「Open Package File (.lpk) ...」，選擇「C:\lazarus\components\Indy10\indylaz.lpk」，按下「Compile」，讓他跑一下直到出現編譯成功。
  + 按下「Use」>「Install」，問要不要rebuild就按「Yes」。
    
+ 固定IDE的小視窗們:
  + 「Package」>「Install/Uninstall Packages ...」
  + 從右側「Available for installation」下方清單找到並多選「anchordockingdsgn 1.0」、「dockedformediteor 0.0」，再按下下方的「Install Selection」。
  + 最後按一次最下方「Save and rebuild IDE」，再按「Continue」，接下來要稍等一下子，完成後會自動重新開啟主程式。
### 更改圖示
