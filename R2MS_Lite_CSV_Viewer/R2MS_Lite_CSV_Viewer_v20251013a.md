# R2MS_Lite_CSV_Viewer 開發筆記
+ 作者: HsiupoYeh  
+ 更新日期: 2025-10-13
+ 這部分只有UI，功能另外寫。

## 開發環境  
+ 使用「Win10_64_CHT_Lazarus4.2_v20250915a.ova」。**匯入為「Win10_Lazarus4.2_R2MS_Lite_CSV_Viewer_v20251013a.ova」。**
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
  + More>Badge = 「CSV Viewer」  
+ 再使用工具2: https://tfin.ch/iosicon.html  
  + 輸出有圓角的圖片。通常選 **android/play_store_512.png** 來做圓角。
+ 再使用工具3: PNG轉ICO線上工具 例如:https://www.aconvert.com/tw/icon/png-to-ico/
  + 需要的多解析度包含:  
    + 全部清單中有的
+ ICO下載位置: [右鍵另存](https://raw.githubusercontent.com/cgrgncu/lazarus_study/refs/heads/main/R2MS_Lite_CSV_Viewer/ico/R2MS_Lite_CSV_Viewer.ico) 

> ICO:  
>  ![image](https://raw.githubusercontent.com/cgrgncu/lazarus_study/refs/heads/main/R2MS_Lite_CSV_Viewer/ico/R2MS_Lite_CSV_Viewer.ico)  


## 開發紀錄(基礎框架部分)
1. 建立新專案。  
    + **1.1 按下「Project>New Project ...」。**  
    + **1.2 選擇「Project>Application 」之後按「OK」。**  
2. 獨立把專案存到指定資料夾中，以利日後管理。大部分自訂程式碼都會是相對路徑，未來只需要帶走這個資料夾即可在其他地方重新使用。  
    + **2.1 在桌面建立「R2MS_Lite_CSV_Viewer」資料夾**  
    + **2.2 回到Lazarus，按下「Project>Save Project As」。**  
    + **2.3 將Project檔案(.lpi)存到桌面上的「R2MS_Lite_CSV_Viewer｣資料夾中，檔案命名為「R2MS_Lite_CSV_Viewer.lpi」。**  
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
      + **4.1.2 設定「Form1」的「properties」頁面下「Caption」為「R2MS_Lite_CSV_Viewer」。**  
        + 4.1.2.1 說明:設定窗體的標題列文字。  
      + **4.1.3 設定「Form1」的「properties」頁面下「Color」為「clWindow」。**  
        + 4.1.3.1 說明:設定成白色使工具列元件底色是白色。  
      + **4.1.4 設定「Form1」的「properties」頁面下「Height」為「700」。**  
        + 4.1.4.1 說明:設定窗體的外框高度。Lazarus比較特別，窗體不含標題之類的區域，只有內部，所以會比Delphi還大一點。
      + **4.1.5 設定「Form1」的「properties」頁面下「Position」為「poScreenCenter」。**  
        + 4.1.5.1 說明:設定窗體的出現位置，建議是用「poScreenCenter」，在螢幕中央。  
      + **4.1.6 設定「Form1」的「properties」頁面下「Width」為「1100」。**  
        + 4.1.6.1 說明:設定窗體的外框寬度。
      + **4.1.7 設定「Form1」的「Event」頁面下的「OnCreate」為如下程式碼。**
        ```pascal
        procedure TForm1.FormCreate(Sender: TObject);
        begin
          //--------------------------------------------------------------------------
          // 在這裡對全域變數進行初始化
          version_str := 'v20251013a';
          Current_Folder_Path := ExtractFilePath(Application.ExeName);
          //--------------------------------------------------------------------------
          //--------------------------------------------------------------------------
          // 表單標題追加版本
          Form1.Caption := Form1.Caption + ' ' + version_str;
          //--------------------------------------------------------------------------
        end;
        ```
      + **4.1.8 原始碼最前面部分先宣告為如下程式碼。**  
        ```pascal  
        implementation        
        //--------------------------------------------------------------------------
        //宣告全域變數 add by HsiupoYeh
        var
          version_str: AnsiString;
          Current_Folder_Path: AnsiString;
        //--------------------------------------------------------------------------
        
        {$R *.lfm}    
        ```  
    > 以上預設尺寸確定後就可以開始排版。  
5. 設計主選單  
    + **5.1 拖拉一個「Standard>TMainMenu」到「Form1」中。預設名稱會是「MainMenu1」。**
    + **5.2 點兩下元件，進入編輯模式，建立第一層第一個選單「開啟檔案(&O)」，「Caption」設為「開啟檔案(&O)」，「Name」設為「MainMenu1_1」。** 
    + **5.3 點兩下元件，進入編輯模式，建立第一層第二個選單「說明(&H)」，「Caption」設為「說明(&H)」，「Name」設為「MainMenu1_2」。**  
    + **5.4 繼續建立第一層第二個選單的子選單「說明(&H)->關於我(&A)」，「Caption」設為「關於我(&A)」，「Name」設為「MainMenu1_2_1」。**  
      + 5.4.1 設定「MainMenu1_2_1」的「Event」。
        + **5.4.1.1 設定「MainMenu1_2_1」的「Event」頁面下「OnClick」為如下程式碼。**  
        ```pascal  
        procedure TForm1.MainMenu1_2_1Click(Sender: TObject);
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
    + **7.1.1 設定「Align」為「AlClient」。**
  + **7.2 拖拉一個「Standard>TPanel」到「ScrollBox1」中。預設名稱會是「Panel1」，修改「Name」為「EmptyPanel」**
    + 7.2.1 設定「EmptyPanel」的「Properties」。
      + **7.2.1.1 設定「EmptyPanel」的「Properties」頁面下「BevelOuter」為「bvNone」。**
      + **7.2.1.2 設定「EmptyPanel」的「Properties」頁面下「Caption」為空白。**
      + **7.2.1.3 設定「EmptyPanel」的「Properties」頁面下「Left」為「926」。**
      + **7.2.1.4 設定「EmptyPanel」的「Properties」頁面下「Top」為「624」。**

### 開發紀錄(主頁面)  
1. 五個分頁(TPageControl):  
    + **1.1 拖拉一個「Common Controls>TPageControl」到「ScrollBox1」中。預設名稱會是「PageControl1」。**  
      + **1.1.1 設定「Align」為「AlClient」。**
      + **1.1.2 設定「Font」，按下「...」，選「大小」為「14」。**
    + **1.2 在「PageControl1」元件上按「右鍵>Add Page」，會增加一個新頁面。預設名稱會是「TabSheet1」。**
      + **1.2.1 設定「Caption」為「資料波形」。**
    + **1.3 在「PageControl1」元件上按「右鍵>Add Page」，會增加一個新頁面。預設名稱會是「TabSheet2」。**
      + **1.3.1 設定「Caption」為「資料內容」。**
    + **1.4 在「PageControl1」元件上按「右鍵>Add Page」，會增加一個新頁面。預設名稱會是「TabSheet3」。**
      + **1.4.1 設定「Caption」為「運行紀錄」。**
    + **1.5 在「PageControl1」元件上按「右鍵>Add Page」，會增加一個新頁面。預設名稱會是「TabSheet4」。**
      + **1.5.1 設定「Caption」為「更新紀錄」。**
            
2. 第一個分頁「資料波形」(TabSheet1):  
    + **2.1 拖拉一個「Standard>TToolBar」到「TabSheet1」中。預設名稱會是「ToolBar1」。**
      + **2.1.1 設定「ShowCaptions」為「True」。**
      + **2.1.2 在「ToolBar1」元件上按「右鍵>New CheckButton」，會增加一個新按鈕。預設名稱會是「ToolButton1」，修改「Name」為「VerticalChartLayout_ToolButton」**
        + **2.1.2.1 設定「Caption」為「垂直排列」。**
        + **2.1.2.2 設定「Down」為「True」。**
        + **2.1.2.3 設定「Grouped」為「True」。**
      + **2.1.2 在「ToolBar1」元件上按「右鍵>New CheckButton」，會增加一個新按鈕。預設名稱會是「ToolButton1」，修改「Name」為「HorizontalChartLayout_ToolButton」**
        + **2.1.2.1 設定「Caption」為「水平排列」。**
        + **2.1.2.2 設定「Grouped」為「True」。**
      + **2.1.3 在「ToolBar1」元件上按「右鍵>New CheckButton」，會增加一個新按鈕。預設名稱會是「ToolButton1」，修改「Name」為「VoltageChartOnlyLayout_ToolButton」**
        + **2.1.3.1 設定「Caption」為「電壓波形」。**
        + **2.1.3.2 設定「Grouped」為「True」。**
      + **2.1.4 在「ToolBar1」元件上按「右鍵>New CheckButton」，會增加一個新按鈕。預設名稱會是「ToolButton1」，修改「Name」為「CurrentChartOnlyLayout_ToolButton」**
        + **2.1.4.1 設定「Caption」為「電流波形」。**
        + **2.1.4.2 設定「Grouped」為「True」。**
    + **2.2 拖拉一個「Standard>TPanel」到「TabSheet1」中。預設名稱會是「Panel1」，修改「Name」為「VoltageChart_Panel」**
      + **2.2.1 設定「Align」為「alLeft」。**
      + **2.2.2 設定「BevelOuter」為「bvNone」。**
      + **2.2.3 設定「Caption」為空白。**
      + **2.2.4 設定「Width」為「544」。**
    + **2.3 拖拉一個「Standard>TPanel」到「TabSheet1」中。預設名稱會是「Panel1」，修改「Name」為「CurrentChart_Panel」**
      + **2.3.1 設定「Align」為「AlClient」。**
      + **2.3.2 設定「BevelOuter」為「bvNone」。**
      + **2.3.3 設定「Caption」為空白。**
    + **2.4 拖拉一個「Chart>TChart」到「VoltageChart_Panel」中。預設名稱會是「Chart1」，修改「Name」為「Voltage_Chart」**
      + **2.4.1 設定「Align」為「AlClient」。**
      + **2.4.2 設定「AxisList>Left>Title>Caption」為「Voltage[mV]」。**
      + **2.4.3 設定「AxisList>Left>Title>Visible」為「True」。**
      + **2.4.4 設定「AxisList>Bottom>Title>Caption」為「Index[count]」。**
      + **2.4.5 設定「AxisList>Bottom>Title>Visible」為「True」。**
      + **2.4.6 設定「Color」為「clWindow」。**
      + **2.4.7 設定「Series」按下「...」。**
        + **2.4.7.1 建立65個「Line Series」，預設名稱會是「Voltage_ChartLineSeries1」，修改「Name」為分別為「Voltage_PSU、Voltage_CH01、Voltage_CH02、...、Voltage_CH64」。**
          + Caption:
          ```
          PSU
          CH01
          CH02
          ...
          CH65
          ```
          + 顏色:
          ```
          #0:  RGB(204, 0, 0)      // 柔和紅 (Muted Red)
          #1:  RGB(0, 179, 0)      // 森林綠 (Forest Green)
          #2:  RGB(0, 0, 204)      // 柔和藍 (Muted Blue)
          #3:  RGB(240, 179, 0)    // 柔和黃 (Mustard Yellow)
          #4:  RGB(0, 204, 204)    // 柔和青 (Turquoise)
          #5:  RGB(204, 0, 204)    // 柔和洋紅 (Plum)
          #6:  RGB(102, 0, 204)    // 深靛青
          #7:  RGB(220, 100, 0)    // 較深橘色
          #8:  RGB(0, 190, 90)     // 較深春綠
          #9:  RGB(128, 0, 0)      // 深紅
          #10: RGB(0, 128, 0)      // 深綠
          #11: RGB(0, 0, 128)      // 深藍
          #12: RGB(128, 128, 0)    // 橄欖
          #13: RGB(0, 128, 128)    // 海綠
          #14: RGB(128, 0, 128)    // 紫色
          #15: RGB(255, 102, 102)  // 鮭魚粉
          #16: RGB(102, 204, 102)  // 柔和淺草綠
          #17: RGB(102, 102, 220)  // 柔和淺靛藍
          #18: RGB(220, 220, 102)  // 柔和淺檸檬黃
          #19: RGB(102, 220, 220)  // 柔和淺青色
          #20: RGB(220, 102, 220)  // 柔和淺紫紅
          #21: RGB(179, 0, 0)      // 磚紅
          #22: RGB(0, 179, 0)      // 濃森林綠
          #23: RGB(0, 0, 179)      // 普魯士藍
          #24: RGB(179, 179, 0)    // 深黃綠
          #25: RGB(0, 179, 179)    // 深青色
          #26: RGB(179, 0, 179)    // 深洋紅
          #27: RGB(220, 150, 0)    // 柔和琥珀
          #28: RGB(0, 200, 150)    // 柔和薄荷綠
          #29: RGB(150, 200, 0)    // 柔和萊姆綠
          #30: RGB(200, 0, 150)    // 柔和樹莓紅
          #31: RGB(150, 0, 200)    // 柔和較淺靛青
          #32: RGB(255, 204, 204)  // 極淺紅 (保留對比度)
          #33: RGB(220, 60, 0)     // 柔和熾熱橘
          #34: RGB(0, 60, 220)     // 柔和鮮豔藍
          #35: RGB(60, 200, 0)     // 柔和酸性綠
          #36: RGB(200, 0, 60)     // 柔和猩紅色
          #37: RGB(0, 150, 150)    // 深青色 (Deep Cyan)
          #38: RGB(60, 0, 200)     // 柔和閃電紫
          #39: RGB(255, 153, 153)  // 粉紅
          #40: RGB(153, 220, 153)  // 柔和淡薄荷
          #41: RGB(153, 153, 220)  // 柔和天藍
          #42: RGB(220, 220, 153)  // 柔和奶油黃
          #43: RGB(153, 220, 220)  // 柔和淡水藍
          #44: RGB(220, 153, 220)  // 柔和淡紫
          #45: RGB(179, 179, 179)  // 中灰 (Medium Gray)
          #46: RGB(128, 128, 128)  // 經典灰 (Classic Gray)
          #47: RGB(64, 64, 64)     // 深灰 (Dark Gray)
          #48: RGB(192, 192, 192)  // 淺銀灰
          #49: RGB(204, 150, 150)  // 加深極淺紅
          #50: RGB(150, 204, 150)  // 加深極淺綠
          #51: RGB(150, 150, 204)  // 加深極淺藍
          #52: RGB(200, 200, 150)  // 加深極淺黃
          #53: RGB(150, 204, 204)  // 加深極淺青
          #54: RGB(204, 150, 204)  // 加深極淺洋紅
          #55: RGB(100, 149, 237)  // 矢車菊藍
          #56: RGB(218, 165, 32)   // 金色
          #57: RGB(139, 69, 19)    // 咖啡棕
          #58: RGB(240, 128, 128)  // 淺珊瑚紅
          #59: RGB(70, 130, 180)   // 鋼藍
          #60: RGB(184, 134, 11)   // 金黃色
          #61: RGB(34, 139, 34)    // 濃森林綠
          #62: RGB(200, 20, 120)   // 柔和深粉紅
          #63: RGB(120, 40, 200)   // 柔和藍紫色
          #64: RGB(120, 120, 90)   // 柔和卡其色
          ```
      + **2.4.8 設定「Title>Text」為「Voltage Chart」。**
      + **2.4.9 設定「Title>Visible」為「True」。**
    + **2.5 拖拉一個「Standerd>TPopupMenu」到「Form1」中。預設名稱會是「PopupMenu1」，修改「Name」為「Voltage_PopupMenu」**
      + **2.5.1 點兩下元件，進入編輯模式，建立第一層第一個選單「全選」，「Caption」設為「全選」，「Name」設為「Voltage_PopupMenu_1_1」。**
        + **2.5.1.1 設定「Voltage_PopupMenu_1_1」的「Event」頁面下「OnClick」為如下程式碼。**  
        ```pascal
        procedure TForm1.Voltage_PopupMenu_1_1Click(Sender: TObject);
        begin
          Voltage_PSU.Active := True;
          Voltage_CH01.Active := True;
          Voltage_CH02.Active := True;
          Voltage_CH03.Active := True;
          Voltage_CH04.Active := True;
          Voltage_CH05.Active := True;
          Voltage_CH06.Active := True;
          Voltage_CH07.Active := True;
          Voltage_CH08.Active := True;
          Voltage_CH09.Active := True;
          Voltage_CH10.Active := True;
          Voltage_CH11.Active := True;
          Voltage_CH12.Active := True;
          Voltage_CH13.Active := True;
          Voltage_CH14.Active := True;
          Voltage_CH15.Active := True;
          Voltage_CH16.Active := True;
          Voltage_CH17.Active := True;
          Voltage_CH18.Active := True;
          Voltage_CH19.Active := True;
          Voltage_CH20.Active := True;
          Voltage_CH21.Active := True;
          Voltage_CH22.Active := True;
          Voltage_CH23.Active := True;
          Voltage_CH24.Active := True;
          Voltage_CH25.Active := True;
          Voltage_CH26.Active := True;
          Voltage_CH27.Active := True;
          Voltage_CH28.Active := True;
          Voltage_CH29.Active := True;
          Voltage_CH30.Active := True;
          Voltage_CH31.Active := True;
          Voltage_CH32.Active := True;
          Voltage_CH33.Active := True;
          Voltage_CH34.Active := True;
          Voltage_CH35.Active := True;
          Voltage_CH36.Active := True;
          Voltage_CH37.Active := True;
          Voltage_CH38.Active := True;
          Voltage_CH39.Active := True;
          Voltage_CH40.Active := True;
          Voltage_CH41.Active := True;
          Voltage_CH42.Active := True;
          Voltage_CH43.Active := True;
          Voltage_CH44.Active := True;
          Voltage_CH45.Active := True;
          Voltage_CH46.Active := True;
          Voltage_CH47.Active := True;
          Voltage_CH48.Active := True;
          Voltage_CH49.Active := True;
          Voltage_CH50.Active := True;
          Voltage_CH51.Active := True;
          Voltage_CH52.Active := True;
          Voltage_CH53.Active := True;
          Voltage_CH54.Active := True;
          Voltage_CH55.Active := True;
          Voltage_CH56.Active := True;
          Voltage_CH57.Active := True;
          Voltage_CH58.Active := True;
          Voltage_CH59.Active := True;
          Voltage_CH60.Active := True;
          Voltage_CH61.Active := True;
          Voltage_CH62.Active := True;
          Voltage_CH63.Active := True;
          Voltage_CH64.Active := True;
        end; 
        ```
      + **2.5.2 點兩下元件，進入編輯模式，建立第一層第二個選單「全不選」，「Caption」設為「全不選」，「Name」設為「Voltage_PopupMenu_1_2」。**
        + **2.5.2.1 設定「Voltage_PopupMenu_1_1」的「Event」頁面下「OnClick」為如下程式碼。**  
        ```pascal
        procedure TForm1.Voltage_PopupMenu_1_1Click(Sender: TObject);
        begin
          Voltage_PSU.Active := False;
          Voltage_CH01.Active := False;
          Voltage_CH02.Active := False;
          Voltage_CH03.Active := False;
          Voltage_CH04.Active := False;
          Voltage_CH05.Active := False;
          Voltage_CH06.Active := False;
          Voltage_CH07.Active := False;
          Voltage_CH08.Active := False;
          Voltage_CH09.Active := False;
          Voltage_CH10.Active := False;
          Voltage_CH11.Active := False;
          Voltage_CH12.Active := False;
          Voltage_CH13.Active := False;
          Voltage_CH14.Active := False;
          Voltage_CH15.Active := False;
          Voltage_CH16.Active := False;
          Voltage_CH17.Active := False;
          Voltage_CH18.Active := False;
          Voltage_CH19.Active := False;
          Voltage_CH20.Active := False;
          Voltage_CH21.Active := False;
          Voltage_CH22.Active := False;
          Voltage_CH23.Active := False;
          Voltage_CH24.Active := False;
          Voltage_CH25.Active := False;
          Voltage_CH26.Active := False;
          Voltage_CH27.Active := False;
          Voltage_CH28.Active := False;
          Voltage_CH29.Active := False;
          Voltage_CH30.Active := False;
          Voltage_CH31.Active := False;
          Voltage_CH32.Active := False;
          Voltage_CH33.Active := False;
          Voltage_CH34.Active := False;
          Voltage_CH35.Active := False;
          Voltage_CH36.Active := False;
          Voltage_CH37.Active := False;
          Voltage_CH38.Active := False;
          Voltage_CH39.Active := False;
          Voltage_CH40.Active := False;
          Voltage_CH41.Active := False;
          Voltage_CH42.Active := False;
          Voltage_CH43.Active := False;
          Voltage_CH44.Active := False;
          Voltage_CH45.Active := False;
          Voltage_CH46.Active := False;
          Voltage_CH47.Active := False;
          Voltage_CH48.Active := False;
          Voltage_CH49.Active := False;
          Voltage_CH50.Active := False;
          Voltage_CH51.Active := False;
          Voltage_CH52.Active := False;
          Voltage_CH53.Active := False;
          Voltage_CH54.Active := False;
          Voltage_CH55.Active := False;
          Voltage_CH56.Active := False;
          Voltage_CH57.Active := False;
          Voltage_CH58.Active := False;
          Voltage_CH59.Active := False;
          Voltage_CH60.Active := False;
          Voltage_CH61.Active := False;
          Voltage_CH62.Active := False;
          Voltage_CH63.Active := False;
          Voltage_CH64.Active := False;
        end; 
        ```
    + **2.6 拖拉一個「Chart>TChartListbox」到「VoltageChart_Panel」中。預設名稱會是「ChartListbox1」，修改「Name」為「Voltage_ChartListbox」**
      + **2.6.1 設定「Align」為「alRight」。**
      + **2.6.2 設定「BorderSpacing>Bottom」為「40」。**
      + **2.6.3 設定「BorderSpacing>Top」為「30」。**
      + **2.6.4 設定「Chart」為「Voltage_Chart」。**
      + **2.6.5 設定「Color」為「clWindow」。**
      + **2.6.6 設定「FontSize」為「10」。**
      + **2.6.7 設定「PopupMenu」為「Voltage_PopupMenu」。**
      + **2.6.8 設定「Width」為「120」。**

    + **2.6 拖拉一個「Chart>TChart」到「CurrentChart_Panel」中。預設名稱會是「Chart1」，修改「Name」為「Current_Chart」**
      + **2.6.1 設定「Align」為「AlClient」。**
      + **2.6.2 設定「AxisList>Left>Title>Caption」為「Current[mA]」。**
      + **2.6.3 設定「AxisList>Left>Title>Visible」為「True」。**
      + **2.6.4 設定「AxisList>Bottom>Title>Caption」為「Index[count]」。**
      + **2.6.5 設定「AxisList>Bottom>Title>Visible」為「True」。**
      + **2.6.6 設定「Color」為「clWindow」。**
      + **2.6.7 設定「Series」按下「...」。**
        + **2.6.7.1 建立65個「Line Series」，預設名稱會是「Voltage_ChartLineSeries1」，修改「Name」為分別為「Current_PSU、Current_CH01、Current_CH02...」...。**
        + Caption:
        ```
        PSU
        CH01
        CH02
        ...
        CH65
        ```
        + 顏色:
        ```
        #0:  RGB(204, 0, 0)      // 柔和紅 (Muted Red)
        #1:  RGB(0, 179, 0)      // 森林綠 (Forest Green)
        #2:  RGB(0, 0, 204)      // 柔和藍 (Muted Blue)
        #3:  RGB(240, 179, 0)    // 柔和黃 (Mustard Yellow)
        #4:  RGB(0, 204, 204)    // 柔和青 (Turquoise)
        #5:  RGB(204, 0, 204)    // 柔和洋紅 (Plum)
        #6:  RGB(102, 0, 204)    // 深靛青
        #7:  RGB(220, 100, 0)    // 較深橘色
        #8:  RGB(0, 190, 90)     // 較深春綠
        #9:  RGB(128, 0, 0)      // 深紅
        #10: RGB(0, 128, 0)      // 深綠
        #11: RGB(0, 0, 128)      // 深藍
        #12: RGB(128, 128, 0)    // 橄欖
        #13: RGB(0, 128, 128)    // 海綠
        #14: RGB(128, 0, 128)    // 紫色
        #15: RGB(255, 102, 102)  // 鮭魚粉
        #16: RGB(102, 204, 102)  // 柔和淺草綠
        #17: RGB(102, 102, 220)  // 柔和淺靛藍
        #18: RGB(220, 220, 102)  // 柔和淺檸檬黃
        #19: RGB(102, 220, 220)  // 柔和淺青色
        #20: RGB(220, 102, 220)  // 柔和淺紫紅
        #21: RGB(179, 0, 0)      // 磚紅
        #22: RGB(0, 179, 0)      // 濃森林綠
        #23: RGB(0, 0, 179)      // 普魯士藍
        #24: RGB(179, 179, 0)    // 深黃綠
        #25: RGB(0, 179, 179)    // 深青色
        #26: RGB(179, 0, 179)    // 深洋紅
        #27: RGB(220, 150, 0)    // 柔和琥珀
        #28: RGB(0, 200, 150)    // 柔和薄荷綠
        #29: RGB(150, 200, 0)    // 柔和萊姆綠
        #30: RGB(200, 0, 150)    // 柔和樹莓紅
        #31: RGB(150, 0, 200)    // 柔和較淺靛青
        #32: RGB(255, 204, 204)  // 極淺紅 (保留對比度)
        #33: RGB(220, 60, 0)     // 柔和熾熱橘
        #34: RGB(0, 60, 220)     // 柔和鮮豔藍
        #35: RGB(60, 200, 0)     // 柔和酸性綠
        #36: RGB(200, 0, 60)     // 柔和猩紅色
        #37: RGB(0, 150, 150)    // 深青色 (Deep Cyan)
        #38: RGB(60, 0, 200)     // 柔和閃電紫
        #39: RGB(255, 153, 153)  // 粉紅
        #40: RGB(153, 220, 153)  // 柔和淡薄荷
        #41: RGB(153, 153, 220)  // 柔和天藍
        #42: RGB(220, 220, 153)  // 柔和奶油黃
        #43: RGB(153, 220, 220)  // 柔和淡水藍
        #44: RGB(220, 153, 220)  // 柔和淡紫
        #45: RGB(179, 179, 179)  // 中灰 (Medium Gray)
        #46: RGB(128, 128, 128)  // 經典灰 (Classic Gray)
        #47: RGB(64, 64, 64)     // 深灰 (Dark Gray)
        #48: RGB(192, 192, 192)  // 淺銀灰
        #49: RGB(204, 150, 150)  // 加深極淺紅
        #50: RGB(150, 204, 150)  // 加深極淺綠
        #51: RGB(150, 150, 204)  // 加深極淺藍
        #52: RGB(200, 200, 150)  // 加深極淺黃
        #53: RGB(150, 204, 204)  // 加深極淺青
        #54: RGB(204, 150, 204)  // 加深極淺洋紅
        #55: RGB(100, 149, 237)  // 矢車菊藍
        #56: RGB(218, 165, 32)   // 金色
        #57: RGB(139, 69, 19)    // 咖啡棕
        #58: RGB(240, 128, 128)  // 淺珊瑚紅
        #59: RGB(70, 130, 180)   // 鋼藍
        #60: RGB(184, 134, 11)   // 金黃色
        #61: RGB(34, 139, 34)    // 濃森林綠
        #62: RGB(200, 20, 120)   // 柔和深粉紅
        #63: RGB(120, 40, 200)   // 柔和藍紫色
        #64: RGB(120, 120, 90)   // 柔和卡其色
        ```
      + **2.6.8 設定「Title>Text」為「Current Chart」。**
      + **2.6.9 設定「Title>Visible」為「True」。**
    + **2.7 拖拉一個「Chart>TChartListbox」到「VoltageChart_Panel」中。預設名稱會是「ChartListbox1」，修改「Name」為「Current_ChartListbox」**
      + **2.7.1 設定「Align」為「alRight」。**
      + **2.7.2 設定「BorderSpacing>Bottom」為「40」。**
      + **2.7.3 設定「BorderSpacing>Top」為「30」。**
      + **2.7.4 設定「Chart」為「Current_Chart」。**
      + **2.7.5 設定「Color」為「clWindow」。**
      + **2.7.6 設定「FontSize」為「10」。**
      + **2.7.7 設定「Width」為「120」。**
     
      + 
    + **2.3 拖拉一個「Standard>TButton」到「ERT_ver2_9_9_Client_GroupBox」中。預設名稱會是「Button1」，修改「Name」為「ERT_ver2_9_9_Client_OpenSavePath_Button」**
      + **2.3.1 設定「Anchors」。**  
        + **2.3.1.1 「Top anchoring>Slbling」維持「(nil)」。取消「Top anchoring>Enable」為不打勾。維持「Top anchoring」右側三個鈕選上面的。**  
        + **2.3.1.2 「Left anchoring>Slbling」維持「(nil)」。取消「Left anchoring>Enable」為不打勾。維持「Left anchoring」右側三個鈕選左邊的。**  
        + **2.3.1.3 「Right anchoring>Slbling」選為「ERT_ver2_9_9_Client_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
        + **2.3.1.4 「Border space」右邊那格填「16」。**  
        + **2.3.1.5 設定完後關閉Anchors設定視窗。**  
      + **2.3.2 設定「Caption」為「開啟資料夾」。**
      + **2.3.2 設定「Height」為「41」。**
      + **2.3.2 設定「Top」為「16」。**
      + **2.3.2 設定「Width」為「105」。**
    + **2.4 拖拉一個「Standard>TEdit」到「ERT_ver2_9_9_Client_GroupBox」中。預設名稱會是「Edit1」，修改「Name」為「ERT_ver2_9_9_Client_SavePath_Edit」**
      + **2.4.1 設定「Anchors」。**  
        + **2.4.1.1 「Top anchoring>Slbling」維持「(nil)」。取消「Top anchoring>Enable」為不打勾。維持「Top anchoring」右側三個鈕選上面的。**  
        + **2.4.1.2 「Left anchoring>Slbling」選為「ERT_ver2_9_9_Client_SavePath_Label:TLabel」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選右邊的。**  
        + **2.4.1.3 「Right anchoring>Slbling」選為「ERT_ver2_9_9_Client_OpenSavePath_Button:TButton」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選左邊的。**  
        + **2.4.1.4 「Border space」正中間那格填「0」，左邊那格填「5」，右邊那格填「15」。**  
        + **2.4.1.5 設定完後關閉Anchors設定視窗。**  
      + **2.4.2 設定「ReadOnly」為「True」。**
      + **2.4.3 設定「Text」為空白。**  
      + **2.4.4 設定「Top」為「20」。**
    + **2.5 拖拉一個「Standard>TButton」到「ERT_ver2_9_9_Client_GroupBox」中。預設名稱會是「Button1」，修改「Name」為「CheckExternalDevices_Button」**
      + **2.5.1 設定「Caption」為「偵測外部裝置」。**
      + **2.5.2 設定「Height」為「49」。**
      + **2.5.3 設定「Left」為「8」。**
      + **2.5.4 設定「Top」為「77」。**
      + **2.5.5 設定「Width」為「163」。** 
    + **2.6 拖拉一個「Standard>TButton」到「ERT_ver2_9_9_Client_GroupBox」中。預設名稱會是「Button1」，修改「Name」為「NowRun_Button」**
      + **2.6.1 設定「Caption」為「立刻單次運行」。**
      + **2.6.2 設定「Height」為「49」。**
      + **2.6.3 設定「Left」為「8」。**
      + **2.6.4 設定「Top」為「144」。**
      + **2.6.5 設定「Width」為「163」。**   
    + **2.7 拖拉一個「Standard>TButton」到「ERT_ver2_9_9_Client_GroupBox」中。預設名稱會是「Button1」，修改「Name」為「NowSystemTest_Button」**
      + **2.7.1 設定「Caption」為「立刻單次檢測」。**
      + **2.7.2 設定「Height」為「49」。**
      + **2.7.3 設定「Left」為「8」。**
      + **2.7.4 設定「Top」為「211」。**
      + **2.7.5 設定「Width」為「163」。**
    + **2.8 拖拉一個「Standard>TButton」到「ERT_ver2_9_9_Client_GroupBox」中。預設名稱會是「Button1」，修改「Name」為「ScheduleRun_Button」**
      + **2.8.1 設定「Caption」為「啟動智慧排程」。**
      + **2.8.2 設定「Height」為「49」。**
      + **2.8.3 設定「Left」為「8」。**
      + **2.8.4 設定「Top」為「278」。**
      + **2.8.5 設定「Width」為「163」。**
    + **2.9 拖拉一個「Standard>TGroupBox」到「TabSheet1」中。預設名稱會是「GroupBox1」，修改「Name」為「ERT_Profile_Name_GroupBox」**
      + **2.9.1 設定「Caption」為「測線名稱:」。**
      + **2.9.2 設定「Height」為「66」。**
      + **2.9.3 設定「Left」為「180」。**
      + **2.9.4 設定「Top」為「63」。**
      + **2.9.5 設定「Width」為「400」。**
    + **2.10 拖拉一個「Standard>TEdit」到「ERT_Profile_Name_GroupBox」中。預設名稱會是「Edit1」，修改「Name」為「ERT_Profile_Name_Edit」**
      + **2.10.1 設定「Anchors」。**  
        + **2.10.1.1 「Top anchoring>Slbling」選為「ERT_Profile_Name_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**  
        + **2.10.1.2 「Left anchoring>Slbling」選為「ERT_Profile_Name_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
        + **2.10.1.3 「Right anchoring>Slbling」選為「ERT_Profile_Name_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
        + **2.10.1.4 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
        + **2.10.1.5 設定完後關閉Anchors設定視窗。**  
      + **2.10.2 設定「ReadOnly」為「True」。**
      + **2.10.2 設定「Text」為空白。**  
    + **2.11 拖拉一個「Standard>TGroupBox」到「TabSheet1」中。預設名稱會是「GroupBox1」，修改「Name」為「ERT_Project_Name_GroupBox」**
      + **2.11.1 設定「Anchors」。**  
        + **2.11.1.1 「Top anchoring>Slbling」選為「ERT_Profile_Name_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**  
        + **2.11.1.2 「Left anchoring>Slbling」選為「ERT_Profile_Name_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選右邊的。**  
        + **2.11.1.3 「Right anchoring>Slbling」選為「ERT_ver2_9_9_Client_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
        + **2.11.1.4 「Border space」正中間那格填「0」，左邊那格填「6」，右邊那格填「15」。**  
        + **2.11.1.5 設定完後關閉Anchors設定視窗。**  
      + **2.11.2 設定「Caption」為「計畫名稱:」。**
      + **2.11.3 設定「Height」為「66」。**
    + **2.12 拖拉一個「Standard>TEdit」到「ERT_Project_Name_GroupBox」中。預設名稱會是「Edit1」，修改「Name」為「ERT_Project_Name_Edit」**
      + **2.12.1 設定「Anchors」。**  
        + **2.12.1.1 「Top anchoring>Slbling」選為「ERT_Project_Name_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**  
        + **2.12.1.2 「Left anchoring>Slbling」選為「ERT_Project_Name_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
        + **2.12.1.3 「Right anchoring>Slbling」選為「ERT_Project_Name_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
        + **2.12.1.4 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
        + **2.12.1.5 設定完後關閉Anchors設定視窗。**  
      + **2.12.2 設定「ReadOnly」為「True」。**
      + **2.12.3 設定「Text」為空白。**  
    + **2.13 拖拉一個「Standard>TGroupBox」到「TabSheet1」中。預設名稱會是「GroupBox1」，修改「Name」為「DMM_SN_GroupBox」**
      + **2.13.1 設定「Caption」為「DMM序號:」。**
      + **2.13.2 設定「Height」為「66」。**
      + **2.13.3 設定「Left」為「180」。**
      + **2.13.4 設定「Top」為「135」。**
      + **2.13.5 設定「Width」為「400」。**
    + **2.14 拖拉一個「Standard>TEdit」到「DMM_SN_GroupBox」中。預設名稱會是「Edit1」，修改「Name」為「DMM_SN_Edit」**
      + **2.14.1 設定「Anchors」。**  
        + **2.14.1.1 「Top anchoring>Slbling」選為「ERT_Profile_Name_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**  
        + **2.14.1.2 「Left anchoring>Slbling」選為「ERT_Profile_Name_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
        + **2.14.1.3 「Right anchoring>Slbling」選為「ERT_Profile_Name_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
        + **2.14.1.4 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
        + **2.14.1.5 設定完後關閉Anchors設定視窗。**  
      + **2.14.2 設定「ReadOnly」為「True」。**
      + **2.14.3 設定「Text」為空白。**  
    + **2.15 拖拉一個「Standard>TGroupBox」到「TabSheet1」中。預設名稱會是「GroupBox1」，修改「Name」為「DMM_GroupBox」**
      + **2.15.1 設定「Anchors」。**  
        + **2.15.1.1 「Top anchoring>Slbling」選為「DMM_SN_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**  
        + **2.15.1.2 「Left anchoring>Slbling」選為「ERT_Profile_Name_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選右邊的。**  
        + **2.15.1.3 「Right anchoring>Slbling」選為「ERT_ver2_9_9_Client_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
        + **2.15.1.4 「Border space」正中間那格填「0」，左邊那格填「6」，右邊那格填「15」。**  
        + **2.15.1.5 設定完後關閉Anchors設定視窗。**  
      + **2.15.2 設定「Caption」為「DMM(34465A,34470A)序列埠:」。**
      + **2.15.3 設定「Height」為「66」。**
    + **2.16 拖拉一個「Standard>TEdit」到「DMM_GroupBox」中。預設名稱會是「Edit1」，修改「Name」為「DMM_Edit」**
      + **2.16.1 設定「Anchors」。**  
        + **2.16.1.1 「Top anchoring>Slbling」選為「ERT_Project_Name_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**  
        + **2.16.1.2 「Left anchoring>Slbling」選為「ERT_Project_Name_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
        + **2.16.1.3 「Right anchoring>Slbling」選為「ERT_Project_Name_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
        + **2.16.1.4 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
        + **2.16.1.5 設定完後關閉Anchors設定視窗。**  
      + **2.16.2 設定「ReadOnly」為「True」。**
      + **2.16.3 設定「Text」為空白。**  
    + **2.17 拖拉一個「Standard>TGroupBox」到「TabSheet1」中。預設名稱會是「GroupBox1」，修改「Name」為「ESP32_SN_GroupBox」**
      + **2.17.1 設定「Caption」為「SwitchArray序號:」。**
      + **2.17.2 設定「Height」為「66」。**
      + **2.17.3 設定「Left」為「180」。**
      + **2.17.4 設定「Top」為「207」。**
      + **2.17.5 設定「Width」為「400」。**
    + **2.18 拖拉一個「Standard>TEdit」到「ESP32_SN_GroupBox」中。預設名稱會是「Edit1」，修改「Name」為「ESP32_SN_Edit」**
      + **2.18.1 設定「Anchors」。**  
        + **2.18.1.1 「Top anchoring>Slbling」選為「ERT_Profile_Name_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**  
        + **2.18.1.2 「Left anchoring>Slbling」選為「ERT_Profile_Name_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
        + **2.18.1.3 「Right anchoring>Slbling」選為「ERT_Profile_Name_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
        + **2.18.1.4 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
        + **2.18.1.5 設定完後關閉Anchors設定視窗。**  
      + **2.18.2 設定「ReadOnly」為「True」。**
      + **2.18.2 設定「Text」為空白。**  
    + **2.19 拖拉一個「Standard>TGroupBox」到「TabSheet1」中。預設名稱會是「GroupBox1」，修改「Name」為「ESP32_GroupBox」**
      + **2.19.1 設定「Anchors」。**  
        + **2.19.1.1 「Top anchoring>Slbling」選為「ESP32_SN_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**  
        + **2.19.1.2 「Left anchoring>Slbling」選為「ERT_Profile_Name_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選右邊的。**  
        + **2.19.1.3 「Right anchoring>Slbling」選為「ERT_ver2_9_9_Client_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
        + **2.19.1.4 「Border space」正中間那格填「0」，左邊那格填「6」，右邊那格填「15」。**  
        + **2.19.1.5 設定完後關閉Anchors設定視窗。**  
      + **2.19.2 設定「Caption」為「SwitchArray序列埠:」。**
      + **2.19.3 設定「Height」為「66」。**
    + **2.20 拖拉一個「Standard>TEdit」到「ESP32_GroupBox」中。預設名稱會是「Edit1」，修改「Name」為「ESP32_Edit」**
      + **2.20.1 設定「Anchors」。**  
        + **2.20.1.1 「Top anchoring>Slbling」選為「ERT_Project_Name_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**  
        + **2.20.1.2 「Left anchoring>Slbling」選為「ERT_Project_Name_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
        + **2.20.1.3 「Right anchoring>Slbling」選為「ERT_Project_Name_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
        + **2.20.1.4 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
        + **2.20.1.5 設定完後關閉Anchors設定視窗。**  
      + **2.20.2 設定「ReadOnly」為「True」。**
      + **2.20.2 設定「Text」為空白。**
    + **2.21 拖拉一個「Standard>TGroupBox」到「TabSheet1」中。預設名稱會是「GroupBox1」，修改「Name」為「PSU_SN_GroupBox」**
      + **2.21.1 設定「Caption」為「PSU序號:」。**
      + **2.21.2 設定「Height」為「66」。**
      + **2.21.3 設定「Left」為「180」。**
      + **2.21.4 設定「Top」為「279」。**
      + **2.21.5 設定「Width」為「400」。**
    + **2.22 拖拉一個「Standard>TEdit」到「PSU_SN_GroupBox」中。預設名稱會是「Edit1」，修改「Name」為「PSU_SN_Edit」**
      + **2.22.1 設定「Anchors」。**  
        + **2.22.1.1 「Top anchoring>Slbling」選為「ERT_Profile_Name_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**  
        + **2.22.1.2 「Left anchoring>Slbling」選為「ERT_Profile_Name_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
        + **2.22.1.3 「Right anchoring>Slbling」選為「ERT_Profile_Name_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
        + **2.22.1.4 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
        + **2.22.1.5 設定完後關閉Anchors設定視窗。**  
      + **2.22.2 設定「ReadOnly」為「True」。**
      + **2.22.2 設定「Text」為空白。**  
    + **2.23 拖拉一個「Standard>TGroupBox」到「TabSheet1」中。預設名稱會是「GroupBox1」，修改「Name」為「PSU_GroupBox」**
      + **2.23.1 設定「Anchors」。**  
        + **2.23.1.1 「Top anchoring>Slbling」選為「PSU_SN_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
        + **2.23.1.2 「Left anchoring>Slbling」選為「ERT_Profile_Name_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選右邊的。**  
        + **2.23.1.3 「Right anchoring>Slbling」選為「ERT_ver2_9_9_Client_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
        + **2.23.1.4 「Border space」正中間那格填「0」，左邊那格填「6」，右邊那格填「15」。**  
        + **2.23.1.5 設定完後關閉Anchors設定視窗。**  
      + **2.23.2 設定「Caption」為「PSU序列埠:」。**
      + **2.23.3 設定「Height」為「66」。**
    + **2.24 拖拉一個「Standard>TEdit」到「PSU_GroupBox」中。預設名稱會是「Edit1」，修改「Name」為「PSU_Edit」**
      + **2.24.1 設定「Anchors」。**  
        + **2.24.1.1 「Top anchoring>Slbling」選為「ERT_Project_Name_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**  
        + **2.24.1.2 「Left anchoring>Slbling」選為「ERT_Project_Name_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
        + **2.24.1.3 「Right anchoring>Slbling」選為「ERT_Project_Name_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
        + **2.24.1.4 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
        + **2.24.1.5 設定完後關閉Anchors設定視窗。**  
      + **2.24.2 設定「ReadOnly」為「True」。**
      + **2.24.2 設定「Text」為空白。**  
    + **2.25 拖拉一個「Standard>TGroupBox」到「TabSheet1」中。預設名稱會是「GroupBox1」，修改「Name」為「ScheduleSetting_GroupBox」**
      + **2.25.1 設定「Anchors」。** 
        + **2.25.1.1 「Top anchoring>Slbling」選為「PSU_SN_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。選擇「Top anchoring」右側三個鈕選下面的。** 
        + **2.25.1.2 「Left anchoring>Slbling」選為「ERT_ver2_9_9_Client_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。** 
        + **2.25.1.3 「Right anchoring>Slbling」選為「ERT_ver2_9_9_Client_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**
        + **2.25.1.4 「Bottom anchoring>Slbling」選為「ERT_ver2_9_9_Client_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選下面的。** 
        + **2.25.1.5 「Border space」正中間那格填「0」，上面那格填「3」，左邊那格填「15」，右邊那格填「15」，下面那格填「3」。**  
        + **2.25.1.6 設定完後關閉Anchors設定視窗。**
      + **2.25.2 設定「Caption」為「啟動時間設定:」。**
    + **2.26 拖拉一個「Standard>TRadioButton」到「ScheduleSetting_GroupBox」中。預設名稱會是「RadioButton1」，修改「Name」為「EveryDay_RadioButton」**
      + **2.26.1 設定「Caption」為「每天」。**
      + **2.26.2 設定「Left」為「48」。**
      + **2.26.3 設定「Top」為「0」。**
    + **2.27 拖拉一個「Standard>TRadioButton」到「ScheduleSetting_GroupBox」中。預設名稱會是「RadioButton1」，修改「Name」為「OnlyToday_RadioButton」**
      + **2.27.1 設定「Caption」為「只有今天」。**
      + **2.27.2 設定「Left」為「134」。**
      + **2.27.3 設定「Top」為「0」。**
    + **2.28 拖拉一個「Standard>TListView」到「ScheduleSetting_GroupBox」中。預設名稱會是「ListView1」，修改「Name」為「ScheduleSetting_ListView」**
      + **2.28.1 設定「Anchors」。** 
        + **2.28.1.1 「Top anchoring>Slbling」選為「EveryDay_RadioButton:TRadioButton」。維持「Top anchoring>Enable」為打勾。選擇「Top anchoring」右側三個鈕選下面的。** 
        + **2.28.1.2 「Left anchoring>Slbling」選為「ScheduleSetting_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。** 
        + **2.28.1.3 「Right anchoring>Slbling」選為「ScheduleSetting_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**
        + **2.28.1.4 「Bottom anchoring>Slbling」選為「ScheduleSetting_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選下面的。** 
        + **2.28.1.5 「Border space」正中間那格填「0」，上面那格填「3」，左邊那格填「3」，右邊那格填「3」，下面那格填「3」。**  
        + **2.28.1.6 設定完後關閉Anchors設定視窗。**
      + **2.28.2 設定「Checkboxes」，打勾為「True」。**
      + **2.28.3 設定「Font」，按下「...」，選「大小」為「12」。**
      + **2.28.4 設定「ViewStyle」為「vsReport」。**
      + **2.28.5 設定「Columns」，按下「...」，利用「Add」增加9個項目。**
        + 2.28.5.1 第1個項目:
          + **2.28.5.1.1 設定「Caption」為「索引」。**
          + **2.28.5.1.2 設定「Width」為「80」。**
        + 2.28.5.2 第2個項目:
          + **2.28.5.2.1 設定「Alignment」為「taCenter」。**
          + **2.28.5.2.2 設定「Caption」為「排程時間(HH:NN)」。**
          + **2.28.5.2.3 設定「Width」為「150」。**
        + 2.28.5.3 第3個項目:
          + **2.28.5.3.1 設定「Alignment」為「taCenter」。**
          + **2.28.5.3.2 設定「Caption」為「放電模式」。**
          + **2.28.5.3.3 設定「Width」為「100」。**
        + 2.28.5.4 第4個項目:
          + **2.28.5.4.1 設定「Alignment」為「taCenter」。**
          + **2.28.5.4.2 設定「Caption」為「限電壓」。**
          + **2.28.5.4.3 設定「Width」為「80」。**
        + 2.28.5.5 第5個項目:
          + **2.28.5.5.1 設定「Alignment」為「taCenter」。**
          + **2.28.5.5.2 設定「Caption」為「限電流」。**
          + **2.28.5.5.3 設定「Width」為「80」。**
        + 2.28.5.6 第6個項目:
          + **2.28.5.6.1 設定「Alignment」為「taCenter」。**
          + **2.28.5.6.2 設定「Caption」為「狀態」。**
          + **2.28.5.6.3 設定「Width」為「100」。**
        + 2.28.5.7 第7個項目:
          + **2.28.5.7.1 設定「Alignment」為「taCenter」。**
          + **2.28.5.7.2 設定「Caption」為「上次時間」。**
          + **2.28.5.7.3 設定「Width」為「205」。**
        + 2.28.5.8 第8個項目:
          + **2.28.5.8.1 設定「Alignment」為「taCenter」。**
          + **2.28.5.8.2 設定「Caption」為「下次時間」。**
          + **2.28.5.8.3 設定「Width」為「205」。**
        + 2.28.5.9 第9個項目:
          + **2.28.5.9.1 設定「Alignment」為「taCenter」。**
          + **2.28.5.9.2 設定「Caption」為「放電時間預估(分鐘)」。**
          + **2.28.5.9.3 設定「Width」為「205」。**    
      + **2.28.6 設定「Items」，按下「...」，利用「Add」增加26個項目。**
        + 2.28.6.1 第1個項目:
          + **2.28.6.1.1 設定「Caption」為「000」。**
          + **2.28.6.1.2 利用「New SubItem」增加8個子項目。並設定第一個子項目「Caption」為「00:00」，其餘子項目「Caption」為空白。**
        + 2.28.6.2 第2個項目:
          + **2.28.6.2.1 設定「Caption」為「001」。**
          + **2.28.6.2.2 利用「New SubItem」增加8個子項目。並設定第一個子項目「Caption」為「00:01」，其餘子項目「Caption」為空白。**
        + 2.28.6.3 第3個項目:
          + **2.28.6.3.1 設定「Caption」為「002」。**
          + **2.28.6.3.2 利用「New SubItem」增加8個子項目。並設定第一個子項目「Caption」為「01:01」，其餘子項目「Caption」為空白。**
        + 2.28.6.4 第4個項目:
          + **2.28.6.4.1 設定「Caption」為「003」。**
          + **2.28.6.4.2 利用「New SubItem」增加8個子項目。並設定第一個子項目「Caption」為「02:01」，其餘子項目「Caption」為空白。**
        + 2.28.6.5 第5個項目:
          + **2.28.6.5.1 設定「Caption」為「004」。**
          + **2.28.6.5.2 利用「New SubItem」增加8個子項目。並設定第一個子項目「Caption」為「03:01」，其餘子項目「Caption」為空白。**
        + 2.28.6.6 第6個項目:
          + **2.28.6.6.1 設定「Caption」為「005」。**
          + **2.28.6.6.2 利用「New SubItem」增加8個子項目。並設定第一個子項目「Caption」為「04:01」，其餘子項目「Caption」為空白。**
        + 2.28.6.7 第7個項目:
          + **2.28.6.7.1 設定「Caption」為「006」。**
          + **2.28.6.7.2 利用「New SubItem」增加8個子項目。並設定第一個子項目「Caption」為「05:01」，其餘子項目「Caption」為空白。**
        + 2.28.6.8 第8個項目:
          + **2.28.6.8.1 設定「Caption」為「007」。**
          + **2.28.6.8.2 利用「New SubItem」增加8個子項目。並設定第一個子項目「Caption」為「06:01」，其餘子項目「Caption」為空白。**
        + 2.28.6.9 第9個項目:
          + **2.28.6.9.1 設定「Caption」為「008」。**
          + **2.28.6.9.2 利用「New SubItem」增加8個子項目。並設定第一個子項目「Caption」為「07:01」，其餘子項目「Caption」為空白。**
        + 2.28.6.10 第10個項目:
          + **2.28.6.10.1 設定「Caption」為「009」。**
          + **2.28.6.10.2 利用「New SubItem」增加8個子項目。並設定第一個子項目「Caption」為「08:01」，其餘子項目「Caption」為空白。**
        + 2.28.6.11 第11個項目:
          + **2.28.6.11.1 設定「Caption」為「010」。**
          + **2.28.6.11.2 利用「New SubItem」增加8個子項目。並設定第一個子項目「Caption」為「09:01」，其餘子項目「Caption」為空白。**
        + 2.28.6.12 第12個項目:
          + **2.28.6.12.1 設定「Caption」為「011」。**
          + **2.28.6.12.2 利用「New SubItem」增加8個子項目。並設定第一個子項目「Caption」為「10:01」，其餘子項目「Caption」為空白。**
        + 2.28.6.13 第13個項目:
          + **2.28.6.13.1 設定「Caption」為「012」。**
          + **2.28.6.13.2 利用「New SubItem」增加8個子項目。並設定第一個子項目「Caption」為「11:01」，其餘子項目「Caption」為空白。**
        + 2.28.6.14 第14個項目:
          + **2.28.6.14.1 設定「Caption」為「013」。**
          + **2.28.6.14.2 利用「New SubItem」增加8個子項目。並設定第一個子項目「Caption」為「12:01」，其餘子項目「Caption」為空白。**
        + 2.28.6.15 第15個項目:
          + **2.28.6.15.1 設定「Caption」為「014」。**
          + **2.28.6.15.2 利用「New SubItem」增加8個子項目。並設定第一個子項目「Caption」為「13:01」，其餘子項目「Caption」為空白。**
        + 2.28.6.16 第16個項目:
          + **2.28.6.16.1 設定「Caption」為「015」。**
          + **2.28.6.16.2 利用「New SubItem」增加8個子項目。並設定第一個子項目「Caption」為「14:01」，其餘子項目「Caption」為空白。**
        + 2.28.6.17 第17個項目:
          + **2.28.6.17.1 設定「Caption」為「016」。**
          + **2.28.6.17.2 利用「New SubItem」增加8個子項目。並設定第一個子項目「Caption」為「15:01」，其餘子項目「Caption」為空白。**
        + 2.28.6.18 第18個項目:
          + **2.28.6.18.1 設定「Caption」為「017」。**
          + **2.28.6.18.2 利用「New SubItem」增加8個子項目。並設定第一個子項目「Caption」為「16:01」，其餘子項目「Caption」為空白。**
        + 2.28.6.19 第19個項目:
          + **2.28.6.19.1 設定「Caption」為「018」。**
          + **2.28.6.19.2 利用「New SubItem」增加8個子項目。並設定第一個子項目「Caption」為「17:01」，其餘子項目「Caption」為空白。**
        + 2.28.6.20 第20個項目:
          + **2.28.6.20.1 設定「Caption」為「019」。**
          + **2.28.6.20.2 利用「New SubItem」增加8個子項目。並設定第一個子項目「Caption」為「18:01」，其餘子項目「Caption」為空白。**
        + 2.28.6.21 第21個項目:
          + **2.28.6.21.1 設定「Caption」為「020」。**
          + **2.28.6.21.2 利用「New SubItem」增加8個子項目。並設定第一個子項目「Caption」為「19:01」，其餘子項目「Caption」為空白。**
        + 2.28.6.22 第22個項目:
          + **2.28.6.22.1 設定「Caption」為「021」。**
          + **2.28.6.22.2 利用「New SubItem」增加8個子項目。並設定第一個子項目「Caption」為「20:01」，其餘子項目「Caption」為空白。**
        + 2.28.6.23 第23個項目:
          + **2.28.6.23.1 設定「Caption」為「022」。**
          + **2.28.6.23.2 利用「New SubItem」增加8個子項目。並設定第一個子項目「Caption」為「21:01」，其餘子項目「Caption」為空白。**
        + 2.28.6.24 第24個項目:
          + **2.28.6.24.1 設定「Caption」為「023」。**
          + **2.28.6.24.2 利用「New SubItem」增加8個子項目。並設定第一個子項目「Caption」為「22:01」，其餘子項目「Caption」為空白。**
        + 2.28.6.25 第25個項目:
          + **2.28.6.25.1 設定「Caption」為「024」。**
          + **2.28.6.25.2 利用「New SubItem」增加8個子項目。並設定第一個子項目「Caption」為「23:01」，其餘子項目「Caption」為空白。**
        + 2.28.6.26 第26個項目:
          + **2.28.6.26.1 設定「Caption」為「999」。**
          + **2.28.6.26.2 利用「New SubItem」增加8個子項目。並設定第一個子項目「Caption」為「23:59」，其餘子項目「Caption」為空白。**
      + **2.28.7 設定「GridLines」為「True」。**
    + **2.29 拖拉一個「Standard>TCheckBox」到「ScheduleSetting_GroupBox」中。預設名稱會是「CheckBox1」，修改「Name」為「PSU_RangeHIGH_CheckBox」**
      + **2.29.1 設定「Caption」為「HIGH(600V/0.5A)」。**
      + **2.29.2 設定「Left」為「288」。**
      + **2.29.3 設定「Top」為「0」。**
    + **2.30 拖拉一個「Standard>TCheckBox」到「ScheduleSetting_GroupBox」中。預設名稱會是「CheckBox1」，修改「Name」為「PSU_RangeLOW_CheckBox」**
      + **2.30.1 設定「Caption」為「LOW(400V/0.35A)」。**
      + **2.30.2 設定「Left」為「496」。**
      + **2.30.3 設定「Top」為「0」。**
    + **2.31 拖拉一個「Standard>TMemo」到「TabSheet1」中。預設名稱會是「Memo1」，修改「Name」為「ReadMe_Memo」**
      + **2.31.1 設定「Align」為「AlClient」。**
      + **2.31.2 設定「Lines」為以下文字。**
        ```
        ==============================================
        使用說明:
        (1)請準備好正確內容的INI參數檔案(本工具軟體同名的INI檔案，放在執行檔所在目錄)，再運行本程式。
         *若內容有誤，請重新編輯INI檔案後重新運行本工具軟體，直到內容正確。
         *若INI檔案損毀，請手動刪除該檔案後重新運行本工具軟體，將會產生一個新的預設INI檔案。
        (2)確認內容正確後，按下「啟動檢測排程」按鈕，軟體將自動依照排程進行工作。
         *每次按下「啟動檢測排程」按鈕，都會清空「R2MS_Lite_WorkSpace」資料夾。
         *每次排程工作都會把所需的資料存到「local」資料夾中，因此「R2MS_Lite_WorkSpace」資料夾中的內容不太重要。
         *因外部程式「ERT_ver2_9_9_Client.exe」輸出資料無檔頭，不利後續資料分享與使用。本軟體將另存為有檔頭的CSV檔案。
        (3)不想要繼續進行工作時，直接關閉本工具軟體。
         *若正在運行外部程式，建議等待工作完成。或手動操作該外部程式，使其正常關閉。
         *待外部軟體工作結束後，趁還在倒數的時候關閉軟體。請注意是否會觸發下一個工作排程。
        ==============================================
        
        ==============================================
        狀態說明:
        未知:
        軟體開啟後，尚未確認工作內容時，狀態為「未知」。 表示軟體剛開啟。
        
        停用:
        按下「啟動檢測排程」按鈕後，未勾選的排程，狀態為「停用」。表示此排程將不會被使用。
        
        待機:
        按下「啟動檢測排程」按鈕後，經查詢清單後，可用的工作排程之狀態填入「待機」。其「下次時間」欄位則填入預期進行工作的確切時間。
        
        略過:
        按下「啟動檢測排程」按鈕後，經查詢清單後，若在「只有今天」的設定下，已經超過當下時間的工作排程，其狀態表示為「略過」。被略過不運行的工作，其「上次時間」欄位將保持空白。
        
        完成:
        正常運作完成的排程工作狀態將顯示為「完成」。其「上次時間」欄位將填入其工作完成時間。在「只有今天」的設定下，狀態維持為「完成」；在「每天」設定下，狀態會再次改為「待機」。
        
        即將:
        按下「啟動檢測排程」按鈕後，經查詢清單後，將下一個可運作的工作狀態表示為「即將」。同時按鈕會倒數計時，當秒數小於0時進行呼叫「ERT_ver2_9_9_Client.exe」軟體的工作。
        
        初始化:
        「即將」的工作時間到後，將呼叫外部程式，該程式進行初始化檢驗的時候，狀態為「初始化」。進入下個階段的時間將顯示在「下次時間」欄位中。注意，倒數時間會比外部程式所需實際時間更久一些。
        
        等待運作:
        「初始化」的工作時間到後，外部程式自動進行運作，狀態填入「等待運作」。目前的檢測工作內容耗時要4分鐘。時間到會自動關閉外部程式。當工作正常運作後，狀態將顯示為「完成」。
        
        運作失敗:
        各項工作若遇到不正常的狀況，將在狀態顯示「運作失敗」。錯誤原因可查詢「運行記錄」頁面。多數情況下，異常發生與儀器設備異常有關，將停止排程以避免發生進一步損害。使用者應於異常排除後重新運行本軟體。
        
        ==============================================
        
        
        ```
      + **2.31.3 設定「ReadOnly」為「True」。**
      + **2.31.4 設定「ScrollBars」為「ssBoth」。**
3. 第2個分頁「參數檔案內容」(TabSheet2):  
    + **3.1 拖拉一個「Standard>TGroupBox」到「TabSheet2」中。預設名稱會是「GroupBox1」，修改「Name」為「v299_DefaultSetting_GroupBox」**
      + **3.1.1 設定「Align」為「AlTop」。**
      + **3.1.2 設定「Caption」為「v299_DefaultSetting」。**
      + **3.1.3 設定「Height」為「250」。**
    + **3.2 拖拉一個「Standard>TMemo」到「v299_DefaultSetting_GroupBox」中。預設名稱會是「Memo1」，修改「Name」為「v299_DefaultSetting_Memo」**
      + **3.2.1 設定「Anchors」。**  
        + **3.2.1.1 「Top anchoring>Slbling」選為「v299_DefaultSetting_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。選擇「Top anchoring」右側三個鈕選上面的。** 
        + **3.2.1.2 「Left anchoring>Slbling」選為「v299_DefaultSetting_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
        + **3.2.1.3 「Right anchoring>Slbling」選為「v299_DefaultSetting_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
        + **3.2.1.4 「Bottom anchoring>Slbling」選為「v299_DefaultSetting_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選下面的。** 
        + **3.2.1.5 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」，下面那格填「3」。**  
        + **3.2.1.6 設定完後關閉Anchors設定視窗。**  
      + **3.2.2 設定「Lines」清空。**
      + **3.2.3 設定「ReadOnly」為「True」。**
      + **3.2.4 設定「ScrollBars」為「ssBoth」。**
    + **3.3 拖拉一個「Standard>TGroupBox」到「TabSheet2」中。預設名稱會是「GroupBox1」，修改「Name」為「v299_NowSetting_GroupBox」**
      + **3.3.1 設定「Align」為「AlTop」。**
      + **3.3.2 設定「Caption」為「v299_NowSetting」。**
      + **3.3.3 設定「Height」為「250」。**
    + **3.4 拖拉一個「Standard>TMemo」到「v299_NowSetting_GroupBox」中。預設名稱會是「Memo1」，修改「Name」為「v299_NowSetting_Memo」**
      + **3.4.1 設定「Anchors」。**  
        + **3.4.1.1 「Top anchoring>Slbling」選為「v299_NowSetting_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。選擇「Top anchoring」右側三個鈕選上面的。** 
        + **3.4.1.2 「Left anchoring>Slbling」選為「v299_NowSetting_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
        + **3.4.1.3 「Right anchoring>Slbling」選為「v299_NowSetting_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
        + **3.4.1.4 「Bottom anchoring>Slbling」選為「v299_NowSetting_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選下面的。** 
        + **3.4.1.5 「Border space」正中間那格填正中間那格填「0」，左邊那格填「3」，右邊那格填「3」，下面那格填「3」。**  
        + **3.4.1.6 設定完後關閉Anchors設定視窗。**  
      + **3.4.2 設定「Lines」清空。**
      + **3.4.3 設定「ReadOnly」為「True」。**
      + **3.4.4 設定「ScrollBars」為「ssBoth」。**
    + **3.5 拖拉一個「Standard>TGroupBox」到「TabSheet2」中。預設名稱會是「GroupBox1」，修改「Name」為「TXT_GroupBox」**
      + **3.5.1 設定「Align」為「AlClient」。**
      + **3.5.2 設定「Caption」為「TXT」。**
    + **3.6 拖拉一個「Standard>TMemo」到「TXT_GroupBox」中。預設名稱會是「Memo1」，修改「Name」為「TXT_Memo」**
      + **3.6.1 設定「Anchors」。**  
        + **3.6.1.1 「Top anchoring>Slbling」選為「TXT_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。選擇「Top anchoring」右側三個鈕選上面的。** 
        + **3.6.1.2 「Left anchoring>Slbling」選為「TXT_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
        + **3.6.1.3 「Right anchoring>Slbling」維持「nil」。維持「Right anchoring>Enable」不打勾。維持「Right anchoring」右側三個鈕選右邊的。**  
        + **3.6.1.4 「Bottom anchoring>Slbling」選為「TXT_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選下面的。** 
        + **3.6.1.5 「Border space」正中間那格填正中間那格填「0」，左邊那格填「3」，右邊那格填「3」，下面那格填「3」。**  
        + **3.6.1.6 設定完後關閉Anchors設定視窗。**  
      + **3.6.2 設定「Lines」清空。**
      + **3.6.3 設定「ReadOnly」為「True」。**
      + **3.6.4 設定「ScrollBars」為「ssBoth」。**
      + **3.6.5 設定「Width」為「800」。**
    + **3.7 拖拉一個「Standard>TMemo」到「TXT_GroupBox」中。預設名稱會是「Memo1」，修改「Name」為「Curl_Memo」**
      + **3.7.1 設定「Anchors」。**  
        + **3.7.1.1 「Top anchoring>Slbling」選為「TXT_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。選擇「Top anchoring」右側三個鈕選上面的。** 
        + **3.7.1.2 「Left anchoring>Slbling」選為「TXT_Memo:TMemo」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選右邊的。**  
        + **3.7.1.3 「Right anchoring>Slbling」選為「TXT_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
        + **3.7.1.4 「Bottom anchoring>Slbling」選為「TXT_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選下面的。** 
        + **3.7.1.5 「Border space」正中間那格填正中間那格填「0」，左邊那格填「3」，右邊那格填「3」，下面那格填「3」。**  
        + **3.7.1.6 設定完後關閉Anchors設定視窗。**  
      + **3.7.2 設定「Lines」清空。**
      + **3.7.3 設定「ReadOnly」為「True」。**
      + **3.7.4 設定「ScrollBars」為「ssBoth」。**
4. 第3個分頁「支援外部裝置」(TabSheet3):  
    + **4.1 拖拉一個「Standard>TGroupBox」到「TabSheet3」中。預設名稱會是「GroupBox1」，修改「Name」為「DMM_SupportedSerialNumbers_GroupBox」**
      + **4.1.1 設定「Align」為「AlTop」。**
      + **4.1.2 設定「Caption」為「DMM支援裝置序號清單:」。**
      + **4.1.3 設定「Height」為「200」。**
    + **4.2 拖拉一個「Standard>TMemo」到「DMM_SupportedSerialNumbers_GroupBox」中。預設名稱會是「Memo1」，修改「Name」為「DMM_SupportedSerialNumbers_Memo」**
      + **4.2.1 設定「Anchors」。**  
        + **4.2.1.1 「Top anchoring>Slbling」選為「DMM_SupportedSerialNumbers_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。選擇「Top anchoring」右側三個鈕選上面的。** 
        + **4.2.1.2 「Left anchoring>Slbling」選為「DMM_SupportedSerialNumbers_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
        + **4.2.1.3 「Right anchoring>Slbling」選為「DMM_SupportedSerialNumbers_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
        + **4.2.1.4 「Bottom anchoring>Slbling」選為「DMM_SupportedSerialNumbers_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選下面的。** 
        + **4.2.1.5 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」，下面那格填「3」。**  
        + **4.2.1.6 設定完後關閉Anchors設定視窗。**  
      + **4.2.2 設定「Lines」清空。**
      + **4.2.3 設定「ReadOnly」為「True」。**
      + **4.2.4 設定「ScrollBars」為「ssBoth」。**
    + **4.3 拖拉一個「Standard>TGroupBox」到「TabSheet3」中。預設名稱會是「GroupBox1」，修改「Name」為「ESP32_SupportedSerialNumbers_GroupBox」**
      + **4.3.1 設定「Align」為「AlTop」。**
      + **4.3.2 設定「Caption」為「SwitchArray支援裝置序號清單:」。**
      + **4.3.3 設定「Height」為「200」。**
    + **4.4 拖拉一個「Standard>TMemo」到「ESP32_SupportedSerialNumbers_GroupBox」中。預設名稱會是「Memo1」，修改「Name」為「ESP32_SupportedSerialNumbers_Memo」**
      + **4.4.1 設定「Anchors」。**  
        + **4.4.1.1 「Top anchoring>Slbling」選為「ESP32_SupportedSerialNumbers_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。選擇「Top anchoring」右側三個鈕選上面的。** 
        + **4.4.1.2 「Left anchoring>Slbling」選為「ESP32_SupportedSerialNumbers_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
        + **4.4.1.3 「Right anchoring>Slbling」選為「ESP32_SupportedSerialNumbers_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
        + **4.4.1.4 「Bottom anchoring>Slbling」選為「ESP32_SupportedSerialNumbers_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選下面的。** 
        + **4.4.1.5 「Border space」正中間那格填正中間那格填「0」，左邊那格填「3」，右邊那格填「3」，下面那格填「3」。**  
        + **4.4.1.6 設定完後關閉Anchors設定視窗。**  
      + **4.4.2 設定「Lines」清空。**
      + **4.4.3 設定「ReadOnly」為「True」。**
      + **4.4.4 設定「ScrollBars」為「ssBoth」。**
    + **4.5 拖拉一個「Standard>TGroupBox」到「TabSheet3」中。預設名稱會是「GroupBox1」，修改「Name」為「PSU_SupportedSerialNumbers_GroupBox」**
      + **4.5.1 設定「Align」為「AlTop」。**
      + **4.5.2 設定「Caption」為「PSU支援裝置序號清單:」。**
      + **4.5.3 設定「Height」為「200」。**
    + **4.6 拖拉一個「Standard>TMemo」到「PSU_SupportedSerialNumbers_GroupBox」中。預設名稱會是「Memo1」，修改「Name」為「PSU_SupportedSerialNumbers_Memo」**
      + **4.6.1 設定「Anchors」。**  
        + **4.6.1.1 「Top anchoring>Slbling」選為「PSU_SupportedSerialNumbers_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。選擇「Top anchoring」右側三個鈕選上面的。** 
        + **4.6.1.2 「Left anchoring>Slbling」選為「PSU_SupportedSerialNumbers_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
        + **4.6.1.3 「Right anchoring>Slbling」選為「PSU_SupportedSerialNumbers_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
        + **4.6.1.4 「Bottom anchoring>Slbling」選為「PSU_SupportedSerialNumbers_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選下面的。** 
        + **4.6.1.5 「Border space」正中間那格填正中間那格填「0」，左邊那格填「3」，右邊那格填「3」，下面那格填「3」。**  
        + **4.6.1.6 設定完後關閉Anchors設定視窗。**  
      + **4.6.2 設定「Lines」清空。**
      + **4.6.3 設定「ReadOnly」為「True」。**
      + **4.6.4 設定「ScrollBars」為「ssBoth」。**    
5. 第4個分頁「運行紀錄」(TabSheet4):  
    + **5.1 拖拉一個「Standard>TMemo」到「TabSheet4」中。預設名稱會是「Memo1」，修改「Name」為「RunningLog_Memo」**
      + **5.1.1 設定「Align」為「alClient」。**
      + **5.1.2 設定「Lines」清空。**
      + **5.1.3 設定「ReadOnly」為「True」。**
      + **5.1.4 設定「ScrollBars」為「ssBoth」。**
6. 第5個分頁「更新紀錄」(TabSheet5):  
    + **6.1 拖拉一個「Standard>TMemo」到「TabSheet5」中。預設名稱會是「Memo1」，修改「Name」為「Update_Memo」**
      + **6.1.1 設定「Align」為「alClient」。**
      + **6.1.2 設定「Lines」為以下文字。**
        ```
        軟體名稱: R2MS_Lite_智慧排程工具(R2MS_Lite__Smart_Scheduler)
        作者: HsiupoYeh
        --
        v20250918b
        重新調整UI。
        --
        v20240510a:
        初版
        
        ```
      + **6.1.3 設定「ReadOnly」為「True」。**
      + **6.1.4 設定「ScrollBars」為「ssBoth」。**
