# R2MS_Lite_Smart_Scheduler 開發筆記
+ 作者: HsiupoYeh  
+ 更新日期: 2025-09-15

## 開發環境  
+ 使用「Win10_64_CHT_Lazarus4.2_v20250915a.ova」。
  + Windows 10 22H2 64bit 企業版 繁體中文(SW_DVD9_Win_Pro_10_22H2.19_64BIT_ChnTrad_Pro_Ent_EDU_N_MLF_X23-74684.ISO)  
  + Lazarus 4.2 (lazarus-4.2-fpc-3.2.2-win64.exe)
    + 安裝套件:
      + Indy10
      + UniqueInstance
      + LazSerial
      + PythonForLazarus

## 運行需求  
- Windosw作業系統。主要測試在Windows 10。  
- 可運行64位元應用程式。

### 備忘  
+ 預設運作平台: Default，應該就是win10 64bit。
+ 依照官網建議，為了避免DPI問題，各項操作建議(安裝IDE、設計窗體、編寫程式碼)在縮放大小100%的情況下進行。編譯出來的軟體可以拿去其他DPI設定下的電腦測試。
  + 使用解析度: 1920x1080
  + 縮放大小: 100%

### 開發紀錄(製作APP Icon)  
+ 使用工具1: https://icon.kitchen/  
  + IOS Icon  
  + Text: 「R2MS Lite」  
  + BackGround>Type=「Gradient」  
  + More>Badge = 「Scheduler」  
+ 再使用工具2: https://tfin.ch/iosicon.html  
  + 輸出有圓角的圖片。通常選 **android/play_store_512.png** 來做圓角。
+ 再使用工具3: PNG轉ICO線上工具 例如:https://www.aconvert.com/tw/icon/png-to-ico/
  + 需要的多解析度包含:  
    + 全部清單中有的
+ ICO下載位置: [右鍵另存](https://raw.githubusercontent.com/cgrgncu/lazarus_study/refs/heads/main/R2MS_Lite_Smart_Scheduler/ico/R2MS_Lite_Smart_Scheduler.ico) 

> ICO:  
>  ![image](https://raw.githubusercontent.com/cgrgncu/lazarus_study/refs/heads/main/R2MS_Lite_Smart_Scheduler/ico/R2MS_Lite_Smart_Scheduler.ico)  


## 開發紀錄(基礎框架部分)
1. 建立新專案。  
    + **1.1 按下「Project>New Project ...」。**  
    + **1.2 選擇「Project>Application 」之後按「OK」。**  
2. 獨立把專案存到指定資料夾中，以利日後管理。大部分自訂程式碼都會是相對路徑，未來只需要帶走這個資料夾即可在其他地方重新使用。  
    + **2.1 在桌面建立「R2MS_Lite_Smart_Scheduler」資料夾**  
    + **2.2 回到Lazarus，按下「Project>Save Project As」。**  
    + **2.3 將Project檔案(.lpi)存到桌面上的「R2MS_Lite_Smart_Scheduler｣資料夾中，檔案命名為「R2MS_Lite_Smart_Scheduler.lpi」。**  
3. 專案設定需要調整。  
    + **3.1 選擇「Project>Project Options ...」設定「Application」頁面下利用「Load Icon 」按鈕更換自訂圖示(選前面製作的ico檔案)。**  
      + 3.1.1 說明: 程式圖示。  
    + 3.2 檢查「Application」頁面下「Execution Level 」為「As Invoker」。  
      + 3.2.1 說明: 「As Invoker、Highest Available」都是普通權限就可以執行，「requireAdministrator」是執行前請求獲得管理者權限才能執行。
    + **3.3 選擇「Project>Project Options ...」設定「Compiler Options」頁面下，按下「Build modes」最右邊的「齒輪」按鈕。再按「Create Debug and Release modes」，並將「Release」勾選「Active」，再按「OK」，再按「OK」**
      + 3.3.1 說明: 用Debug有時候防毒會報錯。  
4. 預設狀況下就有一個Form元件存在，預設名稱叫做「Form1」，先做一些基礎設定以利後續排版。  
    + 4.1 設定「Form1」的「properties」。這裡主要列出常用的一些。  
      + 4.1.1 檢查「Form1」的「properties」頁面下「BorderIcons」為「[biSystemMenu,biMinimize,biMaximize]」。  
        + 4.1.1.1 說明:設定窗體右上角的按鈕如果存在的話要不要被啟用。「[biSystemMenu,biMinimize,biMaximize]」表示這三個被打勾。  
      + 4.1.2 檢查「Form1」的「properties」頁面下「BorderStyle」為「bsSizeable」。  
        + 4.1.2.1 說明:設定窗體的樣式。可依需求嘗試不同風格決定是否符合需求。  
      + **4.1.2 設定「Form1」的「properties」頁面下「Caption」為「R2MS_Lite_智慧排程工具(R2MS_Lite_Smart_Scheduler)」。**  
        + 4.1.2.1 說明:設定窗體的標題列文字。  
      + **4.1.3 設定「Form1」的「properties」頁面下「Color」為「clWindow」。**  
        + 4.1.3.1 說明:設定成白色使工具列元件底色是白色。  
      + **4.1.4 設定「Form1」的「properties」頁面下「Height」為「700」。**  
        + 4.1.4.1 說明:設定窗體的外框高度。Lazarus比較特別，窗體不含標題之類的區域，只有內部，所以會比Delphi還大一點。
      + **4.1.5 設定「Form1」的「properties」頁面下「Position」為「poScreenCenter」。**  
        + 4.1.5.1 說明:設定窗體的出現位置，建議是用「poScreenCenter」，在螢幕中央。  
      + **4.1.6 設定「Form1」的「properties」頁面下「Width」為「1100」。**  
        + 4.1.6.1 說明:設定窗體的外框寬度。  
    > 以上預設尺寸確定後就可以開始排版。  
5. 設計主選單  
    + **5.1 拖拉一個「Standard>TMainMenu」到「Form1」中。預設名稱會是「MainMenu1」。**  
    + **5.2 點兩下元件，進入編輯模式，建立第一層第一個選單「說明(&H)」，「Caption」設為「說明(&H)」，「Name」設為「MainMenu1_1」。**  
    + **5.3 繼續建立子選單「說明(&H)->關於我(&A)」，「Caption」設為「關於我(&A)」，「Name」設為「MainMenu1_1_1」。**  
      + 5.3.1 設定「MainMenu1_1_1」的「Event」。  
        + **5.3.1.1 原始碼最前面部分先宣告為如下程式碼。**  
        ```pascal  
        var
          Form1: TForm1;
          //--
          //Global Variable add by HsiupoYeh
          version_str: AnsiString = 'v20250915a';
          //--  
        ```  
        + **5.3.1.2 設定「MainMenu1_1_1」的「Event」頁面下「OnClick」為如下程式碼。**  
        ```pascal  
        procedure TForm1.MainMenu1_1_1Click(Sender: TObject);
        var
          temp_str: AnsiString;
        begin
          temp_str:='作者: HsiupoYeh.'+#13#10+'程式版本: '+version_str;
          Application.MessageBox(PChar(temp_str),'關於我',64);
        end;  
        ```
6. 設計狀態列。經典的Windows應用程式會在下方放置一個狀態列。如果不趕時間應該避免當作SimplePanel來使用。建議使用標準的(TStatusPanels)來規劃出多分隔的狀態列。可參考經典軟體「記事本」與「NotePad++」的狀態列規劃。
  + **6.1 拖拉一個「Common Controls>TStatusBar」到「Form1」中。預設名稱會是「StatusBar1」。**
  + 6.2 設定「TStatusBar」的「Properties」。這裡主要列出常用的一些。
    + 6.2.1 檢查「StatusBar1」的「Properties」頁面下「Align」為「alBottom」。
      + 6.2.1.1 說明:設定狀態列位置緊貼窗體下方。**
    + **6.2.2 設定「StatusBar1」的「Properties」頁面下「Panel」，按下「...」後會出現編輯介面。從編輯介面建立5個TStatusPanels。**
      + 6.2.2.1 說明:建立狀態列中多個小版面。
    + 6.2.3 設定各「TStatusPanels」的「Properties」。
      + 6.2.3.1 檢查「TStatusPanels」的「Properties」頁面下「Alignment」為「taLeftJustify」。
        + 6.2.3.1.1 說明:「taLeftJustify」是文字靠左對齊，「taCenter」是文字水平置中，「taRightJustify」是文字靠右對齊。注意，如果有人改掉「BiDiMode」可能會顛倒，所以建議不要動「BiDiMode」這個屬性。
      + 6.2.3.2 檢查「TStatusPanels」的「Properties」頁面下「Style」為「psText」。
        + 6.2.3.2.1 說明:「psText」是可寫文字，「psOwnerDraw」是用另外的函數處理非文字。
      + **6.2.3.3 設定「TStatusPanels」的「Properties」頁面下「Text」為「初始文字」。**
        + 6.2.3.3.1 說明:初始文字可以預先在排版階段測試顯示效果，其中最靠右會被吃掉字元，請依需求測試設定文字內容。
      + **6.2.3.4 設定「TStatusPanels」的「Properties」頁面下「Width」為「120」。**
        + 6.2.3.4.1 說明:可以搭配初始文字來調整Width，以利排版。預設只能靠右對齊來分配欄位，其他效果則需要利用程式碼做調整。
    + **6.2.4 設定「StatusBar1」的「Properties」頁面下「SimplePanel」為「false」。**
      + 6.2.4.1 說明:要使前面設定的TStatusPanels有效，須將「SimplePanel」設為「false」。
  + 6.3 設定「TStatusBar」的「Event」。
    + **6.3.1 設定「StatusBar1」的「Event」頁面下「OnResize」為如下程式碼。**
    ```pascal
    procedure TForm1.StatusBar1Resize(Sender: TObject);
    var
      temp_remain_width: Integer;
    begin
      // --
      // 計算剩餘空間
      temp_remain_width := StatusBar1.Width;
    
      // --
      // 先配置最後一格（索引4），希望是200
      //
      StatusBar1.Panels.Items[4].Width := 200;
      // 計算剩餘空間
      temp_remain_width := temp_remain_width - StatusBar1.Panels.Items[4].Width;
    
      // --
      // 配置隔壁一格（索引3），希望是150
      //
      StatusBar1.Panels.Items[3].Width := 150;
      if temp_remain_width < StatusBar1.Panels.Items[3].Width then
      begin
        StatusBar1.Panels.Items[3].Width := temp_remain_width;
      end;
      // 計算剩餘空間
      temp_remain_width := temp_remain_width - StatusBar1.Panels.Items[3].Width;
    
      // --
      // 配置隔壁一格（索引2），希望是150
      //
      StatusBar1.Panels.Items[2].Width := 150;
      if temp_remain_width < StatusBar1.Panels.Items[2].Width then
      begin
        StatusBar1.Panels.Items[2].Width := temp_remain_width;
      end;
      // 計算剩餘空間
      temp_remain_width := temp_remain_width - StatusBar1.Panels.Items[2].Width;
    
      // --
      // 配置隔壁一格（索引1），希望是150
      //
      StatusBar1.Panels.Items[1].Width := 150;
      if temp_remain_width < StatusBar1.Panels.Items[1].Width then
      begin
        StatusBar1.Panels.Items[1].Width := temp_remain_width;
      end;
      // 計算剩餘空間
      temp_remain_width := temp_remain_width - StatusBar1.Panels.Items[1].Width;
    
      // --
      // 最前方一格（索引0）享受最大空間
      //
      StatusBar1.Panels.Items[0].Width := temp_remain_width;
    
      // --
    end; 
    ```
7.設計視窗卷軸(TScrollBox)，搭配空白版面(TPanel)避免窗體縮小後排板被吃掉。
  + **7.1 拖拉一個「Additional>TScrollBox」到「Form1」中。預設名稱會是「ScrollBox1」。**
  + **7.2 拖拉一個「Standard>TPanel」到「ScrollBox1」中。預設名稱會是「Panel1」，修改「Name」為「EmptyPanel」**
    + 7.2.1 設定「EmptyPanel」的「Properties」。
      + **7.2.1.1 設定「EmptyPanel」的「Properties」頁面下「BevelOuter」為「bvNone」。**
      + **7.2.1.2 設定「EmptyPanel」的「Properties」頁面下「Caption」為空白。**
      + **7.2.1.3 設定「EmptyPanel」的「Properties」頁面下「Left」為「926」。**
      + **7.2.1.4 設定「EmptyPanel」的「Properties」頁面下「Top」為「624」。**

### 開發紀錄(主頁面)  
1. 四個分頁(TPageControl):  
    + **1.1 拖拉一個「Common Controls>TPageControl」到「ScrollBox1」中。預設名稱會是「PageControl1」。**  
      + **1.1.1 設定「Align」為「AlClient」。**
      + **1.1.2 設定「Font」，按下「...」，選「大小」為「14」。**
    + **1.2 在「PageControl1」元件上按「右鍵>Add Page」，會增加一個新頁面。預設名稱會是「TabSheet1」。**
      + **1.2.1 設定「Caption」為「智慧排程工具」。**
    + **1.3 在「PageControl1」元件上按「右鍵>Add Page」，會增加一個新頁面。預設名稱會是「TabSheet2」。**
      + **1.3.1 設定「Caption」為「參數檔案內容」。**
    + **1.4 在「PageControl1」元件上按「右鍵>Add Page」，會增加一個新頁面。預設名稱會是「TabSheet3」。** 
      + **1.4.1 設定「Caption」為「運行紀錄」。**
    + **1.5 在「PageControl1」元件上按「右鍵>Add Page」，會增加一個新頁面。預設名稱會是「TabSheet4」。**
      + **1.5.1 設定「Caption」為「更新紀錄」。**
      
2. 第一個分頁「智慧排程工具」(TabSheet1):  
    + **2.1 拖拉一個「Standard>TLabel」到「Form1」中。預設名稱會是「Label1」。**  
      + **2.1.1 設定「Anchors」。**  
        + **2.1.1.1 「Top anchoring>Slbling」選為「ExternalProgram_Edit:TEdit」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選下面的。**  
        + **2.1.1.2 「Left anchoring>Slbling」選為「Form1:TForm1」。維持「Left anchoring>Enable」為打勾。維持「Left anchoring」右側三個鈕選左邊的。**  
        + **2.1.1.3 「Border space」正中間那格填「20」。**  
        + **2.1.1.4 設定完後關閉Anchors設定視窗。**  
      + **2.1.2 設定「Caption」為「外部應用程式參數:」。**  
      + **2.1.3 設定「Name」為「ExternalProgramParameters_Label」。**  
    + **2.2 拖拉一個「Standard>TEdit」到「Form1」中。預設名稱會是「Edit1」。**  
      + **2.2.1 設定「Anchors」。**  
        + **2.2.1.1 「Top anchoring>Slbling」選為「ExternalProgram_Edit:TEdit」。維持「Top anchoring>Enable」為打勾。選擇「Top anchoring」右側三個鈕選下面的。**  
        + **2.2.1.2 「Left anchoring>Slbling」選為「ExternalProgramParameters_Label:TLabel」。維持「Left anchoring>Enable」為打勾。維持「Left anchoring」右側三個鈕選右邊的。**  
        + **2.2.1.3 「Right anchoring>Slbling」選為「Form1:TForm1」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
        + **2.2.1.4 「Border space」正中間那格填「10」，上面那格填「7」，右邊那格填「20」。**  
        + **2.2.1.5 設定完後關閉Anchors設定視窗。**  
      + **2.2.2 設定「Name」為「ExternalProgramParameters_Edit」。**  
      + **2.2.3 設定「Text」為「/c "dir"」。**
