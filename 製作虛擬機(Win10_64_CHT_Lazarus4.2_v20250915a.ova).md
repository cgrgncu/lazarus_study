# 製作虛擬機(Win10_64_CHT_Lazarus4.2_v20250915a.ova)
+ 作者: HsiupoYeh
+ 更新日期: 2025-09-15
+ 使用「Win10_64_CHT_START_v20241022a.ova」。  
+ 輸出為「Win10_64_CHT_Lazarus4.2_v20250915a.ova」。  

### 安裝主程式  
+ 依照官網建議，為了避免DPI問題，各項操作建議(安裝IDE、設計窗體、編寫程式碼)在縮放大小100%的情況下進行。編譯出來的軟體可以拿去其他DPI設定下的電腦測試。  
  + 使用解析度: 1920x1080  
  + 縮放大小: 100%  
+ 2025-09-15，安裝最新版主程式(lazarus-4.2-fpc-3.2.2-win64.exe)到Win10中。  
  + 安裝英文版，預設安裝路徑及選項。  
    + 介面語言有簡體中文，安裝後可以再進去改語言設定。
  + 第一次開啟要選擇IDE Layout:
    + 選 **Modern IDE(Single-Window)**
    + 選 **Modern Form Editor(docked/tabbed)**
    + 按下 **Start IDE**
    
### 線上安裝Package。  
+ 在連接網路的情況下:  
  + 選擇「Package>Online Package Manager ...」，開啟後靜待一下，等清單載入。  
  + 從清單中勾選「Indy10」與「UniqueInstance」，再按下位於視窗下方的「Install」。
  + 出現提示「Do you want to rebuild Lazarus with profiles: Normal IDE?」，按下「Yes」。
  + 等待編譯完成會重新啟動IDE。
+ 固定IDE的小視窗們:
  + 「Package」>「Install/Uninstall Packages ...」
  + 從右側「Available for installation」下方清單找到並多選「anchordockingdsgn 1.0」、「dockedformediteor 0.0」，再按下位於視窗下方的「Install selection」。
  + 最後按一次最下方「Save and rebuild IDE」，再按「Continue」，接下來要稍等一下子，完成後會自動重新開啟主程式。
        
### 離線安裝方法(前面成功就不需要用這個)
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

# 其他有用的資源
+ https://github.com/t-edson/UnTerminal
  + UnTerminal-trunk.zip
+ TProcessEx:
  + biglines_output.zip
