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
        + **2.1.2.1 設定「Caption」為「垂直分割」。**
        + **2.1.2.2 設定「Down」為「True」。**
        + **2.1.2.3 設定「Grouped」為「True」。**
      + **2.1.2 在「ToolBar1」元件上按「右鍵>New CheckButton」，會增加一個新按鈕。預設名稱會是「ToolButton1」，修改「Name」為「HorizontalChartLayout_ToolButton」**
        + **2.1.2.1 設定「Caption」為「水平分割」。**
        + **2.1.2.2 設定「Grouped」為「True」。**
      + **2.1.3 在「ToolBar1」元件上按「右鍵>New CheckButton」，會增加一個新按鈕。預設名稱會是「ToolButton1」，修改「Name」為「VoltageChartOnlyLayout_ToolButton」**
        + **2.1.3.1 設定「Caption」為「電壓波形」。**
        + **2.1.3.2 設定「Grouped」為「True」。**
      + **2.1.4 在「ToolBar1」元件上按「右鍵>New CheckButton」，會增加一個新按鈕。預設名稱會是「ToolButton1」，修改「Name」為「CurrentChartOnlyLayout_ToolButton」**
        + **2.1.4.1 設定「Caption」為「電流波形」。**
        + **2.1.4.2 設定「Grouped」為「True」。**
      + **2.1.5 在「ToolBar1」元件上按「右鍵>New Separator」，會增加一個新按鈕。預設名稱會是「ToolButton1」，修改「Name」為「ToolButtonSeparator1」**
      + **2.1.6 在「ToolBar1」元件上按「右鍵>New Button」，會增加一個新按鈕。預設名稱會是「ToolButton1」，修改「Name」為「FirstPart_ToolButton」**
        + **2.1.6.1 設定「Caption」為「最前次」。**
      + **2.1.7 在「ToolBar1」元件上按「右鍵>New Button」，會增加一個新按鈕。預設名稱會是「ToolButton1」，修改「Name」為「PreviousPart_ToolButton」**
        + **2.1.7.1 設定「Caption」為「前次」。**
      + **2.1.8 在「ToolBar1」元件上按「右鍵>New Button」，會增加一個新按鈕。預設名稱會是「ToolButton1」，修改「Name」為「SelectPart_ToolButton」**
        + **2.1.8.1 設定「Caption」為「選擇目標」。**
      + **2.1.9 在「ToolBar1」元件上按「右鍵>New Button」，會增加一個新按鈕。預設名稱會是「ToolButton1」，修改「Name」為「NextPart_ToolButton」**
        + **2.1.9.1 設定「Caption」為「後次」。**
      + **2.1.10 在「ToolBar1」元件上按「右鍵>New Button」，會增加一個新按鈕。預設名稱會是「ToolButton1」，修改「Name」為「LastPart_ToolButton」**
        + **2.1.10.1 設定「Caption」為「最後次」。**
      + **2.1.11 在「ToolBar1」元件上按「右鍵>New Button」，會增加一個新按鈕。預設名稱會是「ToolButton1」，修改「Name」為「AllPart_ToolButton」**
        + **2.1.11.1 設定「Caption」為「全資料」。**        
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
      + **2.4.2 設定「AxisList>Left>Title>Caption」為「Voltage[V]」。**
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
        + **2.5.2.1 設定「Voltage_PopupMenu_1_2」的「Event」頁面下「OnClick」為如下程式碼。**  
        ```pascal
        procedure TForm1.Voltage_PopupMenu_1_2Click(Sender: TObject);
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
    + **2.7 拖拉一個「Chart>TChart」到「CurrentChart_Panel」中。預設名稱會是「Chart1」，修改「Name」為「Current_Chart」**
      + **2.7.1 設定「Align」為「AlClient」。**
      + **2.7.2 設定「AxisList>Left>Title>Caption」為「Current[A]」。**
      + **2.7.3 設定「AxisList>Left>Title>Visible」為「True」。**
      + **2.7.4 設定「AxisList>Bottom>Title>Caption」為「Index[count]」。**
      + **2.7.5 設定「AxisList>Bottom>Title>Visible」為「True」。**
      + **2.7.6 設定「Color」為「clWindow」。**
      + **2.7.7 設定「Series」按下「...」。**
        + **2.7.7.1 建立65個「Line Series」，預設名稱會是「Voltage_ChartLineSeries1」，修改「Name」為分別為「Current_PSU、Current_CH01、Current_CH02、...、Current_CH64」。**
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
      + **2.7.8 設定「Title>Text」為「Current Chart」。**
      + **2.7.9 設定「Title>Visible」為「True」。**
    + **2.8 拖拉一個「Standerd>TPopupMenu」到「Form1」中。預設名稱會是「PopupMenu1」，修改「Name」為「Current_PopupMenu」**
      + **2.8.1 點兩下元件，進入編輯模式，建立第一層第一個選單「全選」，「Caption」設為「全選」，「Name」設為「Current_PopupMenu_1_1」。**
        + **2.8.1.1 設定「Current_PopupMenu_1_1」的「Event」頁面下「OnClick」為如下程式碼。**  
        ```pascal
        procedure TForm1.Current_PopupMenu_1_1Click(Sender: TObject);
        begin
          Current_PSU.Active := True;
          Current_CH01.Active := True;
          Current_CH02.Active := True;
          Current_CH03.Active := True;
          Current_CH04.Active := True;
          Current_CH05.Active := True;
          Current_CH06.Active := True;
          Current_CH07.Active := True;
          Current_CH08.Active := True;
          Current_CH09.Active := True;
          Current_CH10.Active := True;
          Current_CH11.Active := True;
          Current_CH12.Active := True;
          Current_CH13.Active := True;
          Current_CH14.Active := True;
          Current_CH15.Active := True;
          Current_CH16.Active := True;
          Current_CH17.Active := True;
          Current_CH18.Active := True;
          Current_CH19.Active := True;
          Current_CH20.Active := True;
          Current_CH21.Active := True;
          Current_CH22.Active := True;
          Current_CH23.Active := True;
          Current_CH24.Active := True;
          Current_CH25.Active := True;
          Current_CH26.Active := True;
          Current_CH27.Active := True;
          Current_CH28.Active := True;
          Current_CH29.Active := True;
          Current_CH30.Active := True;
          Current_CH31.Active := True;
          Current_CH32.Active := True;
          Current_CH33.Active := True;
          Current_CH34.Active := True;
          Current_CH35.Active := True;
          Current_CH36.Active := True;
          Current_CH37.Active := True;
          Current_CH38.Active := True;
          Current_CH39.Active := True;
          Current_CH40.Active := True;
          Current_CH41.Active := True;
          Current_CH42.Active := True;
          Current_CH43.Active := True;
          Current_CH44.Active := True;
          Current_CH45.Active := True;
          Current_CH46.Active := True;
          Current_CH47.Active := True;
          Current_CH48.Active := True;
          Current_CH49.Active := True;
          Current_CH50.Active := True;
          Current_CH51.Active := True;
          Current_CH52.Active := True;
          Current_CH53.Active := True;
          Current_CH54.Active := True;
          Current_CH55.Active := True;
          Current_CH56.Active := True;
          Current_CH57.Active := True;
          Current_CH58.Active := True;
          Current_CH59.Active := True;
          Current_CH60.Active := True;
          Current_CH61.Active := True;
          Current_CH62.Active := True;
          Current_CH63.Active := True;
          Current_CH64.Active := True;
        end; 
        ```
      + **2.8.2 點兩下元件，進入編輯模式，建立第一層第二個選單「全不選」，「Caption」設為「全不選」，「Name」設為「Current_PopupMenu_1_2」。**
        + **2.8.2.1 設定「Current_PopupMenu_1_2」的「Event」頁面下「OnClick」為如下程式碼。**  
        ```pascal
        procedure TForm1.Current_PopupMenu_1_2Click(Sender: TObject);
        begin
          Current_PSU.Active := False;
          Current_CH01.Active := False;
          Current_CH02.Active := False;
          Current_CH03.Active := False;
          Current_CH04.Active := False;
          Current_CH05.Active := False;
          Current_CH06.Active := False;
          Current_CH07.Active := False;
          Current_CH08.Active := False;
          Current_CH09.Active := False;
          Current_CH10.Active := False;
          Current_CH11.Active := False;
          Current_CH12.Active := False;
          Current_CH13.Active := False;
          Current_CH14.Active := False;
          Current_CH15.Active := False;
          Current_CH16.Active := False;
          Current_CH17.Active := False;
          Current_CH18.Active := False;
          Current_CH19.Active := False;
          Current_CH20.Active := False;
          Current_CH21.Active := False;
          Current_CH22.Active := False;
          Current_CH23.Active := False;
          Current_CH24.Active := False;
          Current_CH25.Active := False;
          Current_CH26.Active := False;
          Current_CH27.Active := False;
          Current_CH28.Active := False;
          Current_CH29.Active := False;
          Current_CH30.Active := False;
          Current_CH31.Active := False;
          Current_CH32.Active := False;
          Current_CH33.Active := False;
          Current_CH34.Active := False;
          Current_CH35.Active := False;
          Current_CH36.Active := False;
          Current_CH37.Active := False;
          Current_CH38.Active := False;
          Current_CH39.Active := False;
          Current_CH40.Active := False;
          Current_CH41.Active := False;
          Current_CH42.Active := False;
          Current_CH43.Active := False;
          Current_CH44.Active := False;
          Current_CH45.Active := False;
          Current_CH46.Active := False;
          Current_CH47.Active := False;
          Current_CH48.Active := False;
          Current_CH49.Active := False;
          Current_CH50.Active := False;
          Current_CH51.Active := False;
          Current_CH52.Active := False;
          Current_CH53.Active := False;
          Current_CH54.Active := False;
          Current_CH55.Active := False;
          Current_CH56.Active := False;
          Current_CH57.Active := False;
          Current_CH58.Active := False;
          Current_CH59.Active := False;
          Current_CH60.Active := False;
          Current_CH61.Active := False;
          Current_CH62.Active := False;
          Current_CH63.Active := False;
          Current_CH64.Active := False;
        end; 
        ```
    + **2.9 拖拉一個「Chart>TChartListbox」到「CurrentChart_Panel」中。預設名稱會是「ChartListbox1」，修改「Name」為「Current_ChartListbox」**
      + **2.9.1 設定「Align」為「alRight」。**
      + **2.9.2 設定「BorderSpacing>Bottom」為「40」。**
      + **2.9.3 設定「BorderSpacing>Top」為「30」。**
      + **2.9.4 設定「Chart」為「Current_Chart」。**
      + **2.9.5 設定「Color」為「clWindow」。**
      + **2.9.6 設定「FontSize」為「10」。**
      + **2.9.7 設定「PopupMenu」為「Current_PopupMenu」。**
      + **2.9.8 設定「Width」為「120」。**
    + **2.10 相關元件都建立好預設排版後回頭寫之前元件的功能，**
      + **2.9.1 設定「VerticalChartLayout_ToolButton」的「Event」頁面下「OnClick」為如下程式碼。**
      ```pascal
      procedure TForm1.VerticalChartLayout_ToolButtonClick(Sender: TObject);
      begin
        // 1. 確保 Panel 可見並設定為水平分割 (一左一右)
        VoltageChart_Panel.Visible := True;
        CurrentChart_Panel.Visible := True;
        VoltageChart_Panel.Align := alLeft;
        CurrentChart_Panel.Align := alClient;
        // 2. 隨便亂改一個高寬來觸發Resize
        VoltageChart_Panel.Height:=10;
        VoltageChart_Panel.Width:=10;
      end; 
      ```
      + **2.9.2 設定「HorizontalChartLayout_ToolButton」的「Event」頁面下「OnClick」為如下程式碼。**
      ```pascal
      procedure TForm1.HorizontalChartLayout_ToolButtonClick(Sender: TObject);
      begin
        // 1. 確保 Panel 可見並設定為垂直分割 (一上一下)
        VoltageChart_Panel.Visible := True;
        CurrentChart_Panel.Visible := True;
        VoltageChart_Panel.Align := alTop;
        CurrentChart_Panel.Align := alClient;
        // 2. 隨便亂改一個高寬來觸發Resize
        VoltageChart_Panel.Height:=10;
        VoltageChart_Panel.Width:=10;
      end;
      ```
      + **2.9.3 設定「VoltageChartOnlyLayout_ToolButton」的「Event」頁面下「OnClick」為如下程式碼。**
      ```pascal
      procedure TForm1.VoltageChartOnlyLayout_ToolButtonClick(Sender: TObject);
      begin
        CurrentChart_Panel.Align := alNone;
        CurrentChart_Panel.Visible := False;
        VoltageChart_Panel.Align := alClient;
        VoltageChart_Panel.Visible := True;
      end; 
      ```
      + **2.9.4 設定「CurrentChartOnlyLayout_ToolButton」的「Event」頁面下「OnClick」為如下程式碼。**
      ```pascal
      procedure TForm1.CurrentChartOnlyLayout_ToolButtonClick(Sender: TObject);
      begin
        VoltageChart_Panel.Align := alNone;
        VoltageChart_Panel.Visible := False;
        CurrentChart_Panel.Align := alClient;
        CurrentChart_Panel.Visible := True;
      end; 
      ```
      + **2.9.5 設定「VoltageChart_Panel」的「Event」頁面下「OnResize」為如下程式碼。**
      ```pascal
      procedure TForm1.VoltageChart_PanelResize(Sender: TObject);
      begin
        // 檢查是否處於垂直分割模式 (ToolButton.Down 狀態)
        if VerticalChartLayout_ToolButton.Down then
        begin
          VoltageChart_Panel.Width := (VoltageChart_Panel.Width + CurrentChart_Panel.Width) div 2;
        end
        // 檢查是否處於水平分割模式
        else if HorizontalChartLayout_ToolButton.Down then
        begin
          VoltageChart_Panel.Height := (VoltageChart_Panel.Height + CurrentChart_Panel.Height) div 2;
        end;
      end; 
      ```
       + **2.9.6 設定「CurrentChart_Panel」的「Event」頁面下「OnResize」為如下程式碼。**
      ```pascal
      procedure TForm1.CurrentChart_PanelResize(Sender: TObject);
      begin
        // 檢查是否處於垂直分割模式 (ToolButton.Down 狀態)
        if VerticalChartLayout_ToolButton.Down then
        begin
          VoltageChart_Panel.Width := (VoltageChart_Panel.Width + CurrentChart_Panel.Width) div 2;
        end
        // 檢查是否處於水平分割模式
        else if HorizontalChartLayout_ToolButton.Down then
        begin
          VoltageChart_Panel.Height := (VoltageChart_Panel.Height + CurrentChart_Panel.Height) div 2;
        end;
      end; 
      ```     
     
3. 第2個分頁「資料內容」(TabSheet2):  
    + **3.1 拖拉一個「Additional>TStringGrid」到「TabSheet2」中。預設名稱會是「StringGrid1」，修改「Name」為「v299_csv_StringGrid」**
      + **3.1.1 設定「Align」為「alClient」。**
      + **3.1.2 設定「ColCount」為「100」。**
      + **3.1.3 設定「MouseWheelOption」為「mwGrid」。**
      + **3.1.4 設定「Options>goColSizing」為「True」。**
      + **3.1.5 設定「Options>goDblClickAutoSize」為「True」。**
      + **3.1.6 設定「RowCount」為「100」。**
  
4. 第3個分頁「運行紀錄」(TabSheet3):  
    + **4.1 拖拉一個「Standard>TMemo」到「TabSheet3」中。預設名稱會是「Memo1」，修改「Name」為「RunningLog_Memo」**
      + **4.1.1 設定「Align」為「alClient」。**
      + **4.1.2 設定「Lines」清空。**
      + **4.1.3 設定「ReadOnly」為「True」。**
      + **4.1.4 設定「ScrollBars」為「ssBoth」。**
5. 第4個分頁「更新紀錄」(TabSheet4):  
    + **5.1 拖拉一個「Standard>TMemo」到「TabSheet4」中。預設名稱會是「Memo1」，修改「Name」為「Update_Memo」**
      + **5.1.1 設定「Align」為「alClient」。**
      + **5.1.2 設定「Lines」為以下文字。**
        ```
        軟體名稱: R2MS_Lite_CSV_Viewer
        作者: HsiupoYeh
        --
        v20251013a
        初版
        
        ```
      + **5.1.3 設定「ReadOnly」為「True」。**
      + **5.1.4 設定「ScrollBars」為「ssBoth」。**

### 寫功能
1. **去implementation區塊追加宣告全域變數**
```pascal
implementation
//--------------------------------------------------------------------------
//uses add by HsiupoYeh
uses
  Math;
//--------------------------------------------------------------------------
//--------------------------------------------------------------------------
//宣告全域變數 add by HsiupoYeh
var
  version_str: AnsiString;
  Current_Folder_Path: AnsiString;
//--------------------------------------------------------------------------

{$R *.lfm}
```
2. **設定「MainMenu1_1」的「Event」頁面下「OnClick」為如下程式碼。**
```pascal
procedure TForm1.MainMenu1_1Click(Sender: TObject);
var
  temp_i: Integer;
begin
  // 調整啟用禁用狀態
  MainMenu1_1.Enabled := False;
  //PageControl1.Enabled := False;
  // OpenDialog1 的初始資料夾路徑
  OpenDialog1.InitialDir := Current_Folder_Path;
  // OpenDialog1 下拉選單過濾檔名
  OpenDialog1.Filter := '逗號分隔檔案 (*.csv)|*.csv';
  // 呼叫 OpenDialog1
  if OpenDialog1.Execute then
  begin
    // 狀態列資訊
    StatusBar1.Panels[0].Text := '載入中，請稍候...';
    // 展示檔案名稱
    RunningLog_Memo.Lines.Add('選擇檔案名稱 = ' + OpenDialog1.FileName);
    // 載入到 v299_csv_StringGrid
    v299_csv_StringGrid.LoadFromCSVFile(OpenDialog1.FileName, ',', False, 0, True);
    // 插入最左方col並且固定灰色欄位數量為1
    v299_csv_StringGrid.InsertColRow(True,0);
    v299_csv_StringGrid.FixedCols := 1;
    // 補上 Row 編號 (在 Col[0] 灰色欄位中)
    for temp_i := v299_csv_StringGrid.FixedRows to v299_csv_StringGrid.RowCount - 1 do
    begin
        v299_csv_StringGrid.Cells[0, temp_i] := IntToStr(temp_i - v299_csv_StringGrid.FixedRows + 1);
    end;
    // 補上 Col 編號 (在 Row[0] 灰色欄位中)
    for temp_i := v299_csv_StringGrid.FixedCols to v299_csv_StringGrid.ColCount - 1 do
    begin
        v299_csv_StringGrid.Cells[temp_i, 0] := IntToStr(temp_i - v299_csv_StringGrid.FixedCols + 1);
    end;
    // 逐行調整尺寸，避免用戶覺得卡住。
    for temp_i := v299_csv_StringGrid.FixedCols to v299_csv_StringGrid.ColCount - 1 do
    begin
        v299_csv_StringGrid.AutoSizeColumn(temp_i);
        Application.ProcessMessages;
    end;
    // 檢查關鍵欄位
    if v299_csv_StringGrid.Cells[1, 1] <> 'Time[ms](R2MS_Lite_v299csv_v20240510a)' then
    begin
      // 錯誤
      RunningLog_Memo.Lines.Add('錯誤!不支援的CSV檔案!請檢查是否為R2MS_Lite_v299csv_v20240510a格式!' + v299_csv_StringGrid.Cells[0, 1]);
      Application.MessageBox('不支援的CSV檔案!','錯誤',16);
      // 調整啟用禁用狀態
      MainMenu1_1.Enabled := True;
      PageControl1.Enabled := True;
      // 狀態列資訊
      StatusBar1.Panels[0].Text := '不支援的CSV檔案!';
      Exit;
    end;
    // 檢查資料數量
    if ((v299_csv_StringGrid.RowCount-2) mod 30) = 0 then
    begin
      RunningLog_Memo.Lines.Add('放電次數 = ' + IntToStr((v299_csv_StringGrid.RowCount-2) div 30));
      // 狀態列資訊
      StatusBar1.Panels[2].Text := '放電次數 = ' + IntToStr((v299_csv_StringGrid.RowCount-2) div 30);
      StatusBar1.Panels[3].Text := '0';
      StatusBar1.Panels[4].Text := IntToStr((v299_csv_StringGrid.RowCount-2) div 30);
    end
    else
    begin
      // 錯誤
      RunningLog_Memo.Lines.Add('錯誤!放電秒數無法被30整除!');
      Application.MessageBox('放電秒數無法被30整除!','錯誤',16);
      // 調整啟用禁用狀態
      MainMenu1_1.Enabled := True;
      PageControl1.Enabled := True;
      // 狀態列資訊
      StatusBar1.Panels[0].Text := '錯誤!放電秒數無法被30整除!';
      Exit;
    end;
    //
    StatusBar1.Panels[1].Text := '全資料';
    //
    //--
    Voltage_PSU.Clear;
    Current_PSU.Clear;
    Voltage_CH01.Clear;
    Current_CH01.Clear;
    Voltage_CH02.Clear;
    Current_CH02.Clear;
    Voltage_CH03.Clear;
    Current_CH03.Clear;
    Voltage_CH04.Clear;
    Current_CH04.Clear;
    Voltage_CH05.Clear;
    Current_CH05.Clear;
    Voltage_CH06.Clear;
    Current_CH06.Clear;
    Voltage_CH07.Clear;
    Current_CH07.Clear;
    Voltage_CH08.Clear;
    Current_CH08.Clear;
    Voltage_CH09.Clear;
    Current_CH09.Clear;
    Voltage_CH10.Clear;
    Current_CH10.Clear;
    Voltage_CH11.Clear;
    Current_CH11.Clear;
    Voltage_CH12.Clear;
    Current_CH12.Clear;
    Voltage_CH13.Clear;
    Current_CH13.Clear;
    Voltage_CH14.Clear;
    Current_CH14.Clear;
    Voltage_CH15.Clear;
    Current_CH15.Clear;
    Voltage_CH16.Clear;
    Current_CH16.Clear;
    Voltage_CH17.Clear;
    Current_CH17.Clear;
    Voltage_CH18.Clear;
    Current_CH18.Clear;
    Voltage_CH19.Clear;
    Current_CH19.Clear;
    Voltage_CH20.Clear;
    Current_CH20.Clear;
    Voltage_CH21.Clear;
    Current_CH21.Clear;
    Voltage_CH22.Clear;
    Current_CH22.Clear;
    Voltage_CH23.Clear;
    Current_CH23.Clear;
    Voltage_CH24.Clear;
    Current_CH24.Clear;
    Voltage_CH25.Clear;
    Current_CH25.Clear;
    Voltage_CH26.Clear;
    Current_CH26.Clear;
    Voltage_CH27.Clear;
    Current_CH27.Clear;
    Voltage_CH28.Clear;
    Current_CH28.Clear;
    Voltage_CH29.Clear;
    Current_CH29.Clear;
    Voltage_CH30.Clear;
    Current_CH30.Clear;
    Voltage_CH31.Clear;
    Current_CH31.Clear;
    Voltage_CH32.Clear;
    Current_CH32.Clear;
    Voltage_CH33.Clear;
    Current_CH33.Clear;
    Voltage_CH34.Clear;
    Current_CH34.Clear;
    Voltage_CH35.Clear;
    Current_CH35.Clear;
    Voltage_CH36.Clear;
    Current_CH36.Clear;
    Voltage_CH37.Clear;
    Current_CH37.Clear;
    Voltage_CH38.Clear;
    Current_CH38.Clear;
    Voltage_CH39.Clear;
    Current_CH39.Clear;
    Voltage_CH40.Clear;
    Current_CH40.Clear;
    Voltage_CH41.Clear;
    Current_CH41.Clear;
    Voltage_CH42.Clear;
    Current_CH42.Clear;
    Voltage_CH43.Clear;
    Current_CH43.Clear;
    Voltage_CH44.Clear;
    Current_CH44.Clear;
    Voltage_CH45.Clear;
    Current_CH45.Clear;
    Voltage_CH46.Clear;
    Current_CH46.Clear;
    Voltage_CH47.Clear;
    Current_CH47.Clear;
    Voltage_CH48.Clear;
    Current_CH48.Clear;
    Voltage_CH49.Clear;
    Current_CH49.Clear;
    Voltage_CH50.Clear;
    Current_CH50.Clear;
    Voltage_CH51.Clear;
    Current_CH51.Clear;
    Voltage_CH52.Clear;
    Current_CH52.Clear;
    Voltage_CH53.Clear;
    Current_CH53.Clear;
    Voltage_CH54.Clear;
    Current_CH54.Clear;
    Voltage_CH55.Clear;
    Current_CH55.Clear;
    Voltage_CH56.Clear;
    Current_CH56.Clear;
    Voltage_CH57.Clear;
    Current_CH57.Clear;
    Voltage_CH58.Clear;
    Current_CH58.Clear;
    Voltage_CH59.Clear;
    Current_CH59.Clear;
    Voltage_CH60.Clear;
    Current_CH60.Clear;
    Voltage_CH61.Clear;
    Current_CH61.Clear;
    Voltage_CH62.Clear;
    Current_CH62.Clear;
    Voltage_CH63.Clear;
    Current_CH63.Clear;
    Voltage_CH64.Clear;
    Current_CH64.Clear;
    //--
    Voltage_PSU.BeginUpdate;
    Current_PSU.BeginUpdate;
    Voltage_CH01.BeginUpdate;
    Current_CH01.BeginUpdate;
    Voltage_CH02.BeginUpdate;
    Current_CH02.BeginUpdate;
    Voltage_CH03.BeginUpdate;
    Current_CH03.BeginUpdate;
    Voltage_CH04.BeginUpdate;
    Current_CH04.BeginUpdate;
    Voltage_CH05.BeginUpdate;
    Current_CH05.BeginUpdate;
    Voltage_CH06.BeginUpdate;
    Current_CH06.BeginUpdate;
    Voltage_CH07.BeginUpdate;
    Current_CH07.BeginUpdate;
    Voltage_CH08.BeginUpdate;
    Current_CH08.BeginUpdate;
    Voltage_CH09.BeginUpdate;
    Current_CH09.BeginUpdate;
    Voltage_CH10.BeginUpdate;
    Current_CH10.BeginUpdate;
    Voltage_CH11.BeginUpdate;
    Current_CH11.BeginUpdate;
    Voltage_CH12.BeginUpdate;
    Current_CH12.BeginUpdate;
    Voltage_CH13.BeginUpdate;
    Current_CH13.BeginUpdate;
    Voltage_CH14.BeginUpdate;
    Current_CH14.BeginUpdate;
    Voltage_CH15.BeginUpdate;
    Current_CH15.BeginUpdate;
    Voltage_CH16.BeginUpdate;
    Current_CH16.BeginUpdate;
    Voltage_CH17.BeginUpdate;
    Current_CH17.BeginUpdate;
    Voltage_CH18.BeginUpdate;
    Current_CH18.BeginUpdate;
    Voltage_CH19.BeginUpdate;
    Current_CH19.BeginUpdate;
    Voltage_CH20.BeginUpdate;
    Current_CH20.BeginUpdate;
    Voltage_CH21.BeginUpdate;
    Current_CH21.BeginUpdate;
    Voltage_CH22.BeginUpdate;
    Current_CH22.BeginUpdate;
    Voltage_CH23.BeginUpdate;
    Current_CH23.BeginUpdate;
    Voltage_CH24.BeginUpdate;
    Current_CH24.BeginUpdate;
    Voltage_CH25.BeginUpdate;
    Current_CH25.BeginUpdate;
    Voltage_CH26.BeginUpdate;
    Current_CH26.BeginUpdate;
    Voltage_CH27.BeginUpdate;
    Current_CH27.BeginUpdate;
    Voltage_CH28.BeginUpdate;
    Current_CH28.BeginUpdate;
    Voltage_CH29.BeginUpdate;
    Current_CH29.BeginUpdate;
    Voltage_CH30.BeginUpdate;
    Current_CH30.BeginUpdate;
    Voltage_CH31.BeginUpdate;
    Current_CH31.BeginUpdate;
    Voltage_CH32.BeginUpdate;
    Current_CH32.BeginUpdate;
    Voltage_CH33.BeginUpdate;
    Current_CH33.BeginUpdate;
    Voltage_CH34.BeginUpdate;
    Current_CH34.BeginUpdate;
    Voltage_CH35.BeginUpdate;
    Current_CH35.BeginUpdate;
    Voltage_CH36.BeginUpdate;
    Current_CH36.BeginUpdate;
    Voltage_CH37.BeginUpdate;
    Current_CH37.BeginUpdate;
    Voltage_CH38.BeginUpdate;
    Current_CH38.BeginUpdate;
    Voltage_CH39.BeginUpdate;
    Current_CH39.BeginUpdate;
    Voltage_CH40.BeginUpdate;
    Current_CH40.BeginUpdate;
    Voltage_CH41.BeginUpdate;
    Current_CH41.BeginUpdate;
    Voltage_CH42.BeginUpdate;
    Current_CH42.BeginUpdate;
    Voltage_CH43.BeginUpdate;
    Current_CH43.BeginUpdate;
    Voltage_CH44.BeginUpdate;
    Current_CH44.BeginUpdate;
    Voltage_CH45.BeginUpdate;
    Current_CH45.BeginUpdate;
    Voltage_CH46.BeginUpdate;
    Current_CH46.BeginUpdate;
    Voltage_CH47.BeginUpdate;
    Current_CH47.BeginUpdate;
    Voltage_CH48.BeginUpdate;
    Current_CH48.BeginUpdate;
    Voltage_CH49.BeginUpdate;
    Current_CH49.BeginUpdate;
    Voltage_CH50.BeginUpdate;
    Current_CH50.BeginUpdate;
    Voltage_CH51.BeginUpdate;
    Current_CH51.BeginUpdate;
    Voltage_CH52.BeginUpdate;
    Current_CH52.BeginUpdate;
    Voltage_CH53.BeginUpdate;
    Current_CH53.BeginUpdate;
    Voltage_CH54.BeginUpdate;
    Current_CH54.BeginUpdate;
    Voltage_CH55.BeginUpdate;
    Current_CH55.BeginUpdate;
    Voltage_CH56.BeginUpdate;
    Current_CH56.BeginUpdate;
    Voltage_CH57.BeginUpdate;
    Current_CH57.BeginUpdate;
    Voltage_CH58.BeginUpdate;
    Current_CH58.BeginUpdate;
    Voltage_CH59.BeginUpdate;
    Current_CH59.BeginUpdate;
    Voltage_CH60.BeginUpdate;
    Current_CH60.BeginUpdate;
    Voltage_CH61.BeginUpdate;
    Current_CH61.BeginUpdate;
    Voltage_CH62.BeginUpdate;
    Current_CH62.BeginUpdate;
    Voltage_CH63.BeginUpdate;
    Current_CH63.BeginUpdate;
    Voltage_CH64.BeginUpdate;
    Current_CH64.BeginUpdate;
    //--
    for temp_i := 2 to v299_csv_StringGrid.RowCount - 1 do
    begin
        Voltage_PSU.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[2, temp_i], NaN));
        Current_PSU.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[3, temp_i], NaN));
        Voltage_CH01.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[6, temp_i], NaN));
        Current_CH01.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[7, temp_i], NaN));
        Voltage_CH02.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[8, temp_i], NaN));
        Current_CH02.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[9, temp_i], NaN));
        Voltage_CH03.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[10, temp_i], NaN));
        Current_CH03.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[11, temp_i], NaN));
        Voltage_CH04.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[12, temp_i], NaN));
        Current_CH04.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[13, temp_i], NaN));
        Voltage_CH05.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[14, temp_i], NaN));
        Current_CH05.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[15, temp_i], NaN));
        Voltage_CH06.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[16, temp_i], NaN));
        Current_CH06.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[17, temp_i], NaN));
        Voltage_CH07.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[18, temp_i], NaN));
        Current_CH07.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[19, temp_i], NaN));
        Voltage_CH08.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[20, temp_i], NaN));
        Current_CH08.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[21, temp_i], NaN));
        Voltage_CH09.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[22, temp_i], NaN));
        Current_CH09.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[23, temp_i], NaN));
        Voltage_CH10.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[24, temp_i], NaN));
        Current_CH10.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[25, temp_i], NaN));
        Voltage_CH11.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[26, temp_i], NaN));
        Current_CH11.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[27, temp_i], NaN));
        Voltage_CH12.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[28, temp_i], NaN));
        Current_CH12.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[29, temp_i], NaN));
        Voltage_CH13.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[30, temp_i], NaN));
        Current_CH13.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[31, temp_i], NaN));
        Voltage_CH14.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[32, temp_i], NaN));
        Current_CH14.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[33, temp_i], NaN));
        Voltage_CH15.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[34, temp_i], NaN));
        Current_CH15.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[35, temp_i], NaN));
        Voltage_CH16.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[36, temp_i], NaN));
        Current_CH16.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[37, temp_i], NaN));
        Voltage_CH17.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[38, temp_i], NaN));
        Current_CH17.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[39, temp_i], NaN));
        Voltage_CH18.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[40, temp_i], NaN));
        Current_CH18.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[41, temp_i], NaN));
        Voltage_CH19.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[42, temp_i], NaN));
        Current_CH19.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[43, temp_i], NaN));
        Voltage_CH20.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[44, temp_i], NaN));
        Current_CH20.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[45, temp_i], NaN));
        Voltage_CH21.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[46, temp_i], NaN));
        Current_CH21.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[47, temp_i], NaN));
        Voltage_CH22.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[48, temp_i], NaN));
        Current_CH22.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[49, temp_i], NaN));
        Voltage_CH23.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[50, temp_i], NaN));
        Current_CH23.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[51, temp_i], NaN));
        Voltage_CH24.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[52, temp_i], NaN));
        Current_CH24.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[53, temp_i], NaN));
        Voltage_CH25.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[54, temp_i], NaN));
        Current_CH25.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[55, temp_i], NaN));
        Voltage_CH26.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[56, temp_i], NaN));
        Current_CH26.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[57, temp_i], NaN));
        Voltage_CH27.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[58, temp_i], NaN));
        Current_CH27.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[59, temp_i], NaN));
        Voltage_CH28.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[60, temp_i], NaN));
        Current_CH28.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[61, temp_i], NaN));
        Voltage_CH29.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[62, temp_i], NaN));
        Current_CH29.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[63, temp_i], NaN));
        Voltage_CH30.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[64, temp_i], NaN));
        Current_CH30.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[65, temp_i], NaN));
        Voltage_CH31.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[66, temp_i], NaN));
        Current_CH31.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[67, temp_i], NaN));
        Voltage_CH32.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[68, temp_i], NaN));
        Current_CH32.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[69, temp_i], NaN));
        Voltage_CH33.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[70, temp_i], NaN));
        Current_CH33.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[71, temp_i], NaN));
        Voltage_CH34.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[72, temp_i], NaN));
        Current_CH34.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[73, temp_i], NaN));
        Voltage_CH35.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[74, temp_i], NaN));
        Current_CH35.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[75, temp_i], NaN));
        Voltage_CH36.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[76, temp_i], NaN));
        Current_CH36.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[77, temp_i], NaN));
        Voltage_CH37.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[78, temp_i], NaN));
        Current_CH37.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[79, temp_i], NaN));
        Voltage_CH38.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[80, temp_i], NaN));
        Current_CH38.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[81, temp_i], NaN));
        Voltage_CH39.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[82, temp_i], NaN));
        Current_CH39.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[83, temp_i], NaN));
        Voltage_CH40.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[84, temp_i], NaN));
        Current_CH40.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[85, temp_i], NaN));
        Voltage_CH41.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[86, temp_i], NaN));
        Current_CH41.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[87, temp_i], NaN));
        Voltage_CH42.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[88, temp_i], NaN));
        Current_CH42.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[89, temp_i], NaN));
        Voltage_CH43.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[90, temp_i], NaN));
        Current_CH43.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[91, temp_i], NaN));
        Voltage_CH44.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[92, temp_i], NaN));
        Current_CH44.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[93, temp_i], NaN));
        Voltage_CH45.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[94, temp_i], NaN));
        Current_CH45.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[95, temp_i], NaN));
        Voltage_CH46.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[96, temp_i], NaN));
        Current_CH46.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[97, temp_i], NaN));
        Voltage_CH47.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[98, temp_i], NaN));
        Current_CH47.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[99, temp_i], NaN));
        Voltage_CH48.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[100, temp_i], NaN));
        Current_CH48.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[101, temp_i], NaN));
        Voltage_CH49.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[102, temp_i], NaN));
        Current_CH49.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[103, temp_i], NaN));
        Voltage_CH50.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[104, temp_i], NaN));
        Current_CH50.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[105, temp_i], NaN));
        Voltage_CH51.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[106, temp_i], NaN));
        Current_CH51.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[107, temp_i], NaN));
        Voltage_CH52.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[108, temp_i], NaN));
        Current_CH52.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[109, temp_i], NaN));
        Voltage_CH53.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[110, temp_i], NaN));
        Current_CH53.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[111, temp_i], NaN));
        Voltage_CH54.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[112, temp_i], NaN));
        Current_CH54.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[113, temp_i], NaN));
        Voltage_CH55.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[114, temp_i], NaN));
        Current_CH55.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[115, temp_i], NaN));
        Voltage_CH56.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[116, temp_i], NaN));
        Current_CH56.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[117, temp_i], NaN));
        Voltage_CH57.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[118, temp_i], NaN));
        Current_CH57.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[119, temp_i], NaN));
        Voltage_CH58.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[120, temp_i], NaN));
        Current_CH58.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[121, temp_i], NaN));
        Voltage_CH59.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[122, temp_i], NaN));
        Current_CH59.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[123, temp_i], NaN));
        Voltage_CH60.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[124, temp_i], NaN));
        Current_CH60.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[125, temp_i], NaN));
        Voltage_CH61.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[126, temp_i], NaN));
        Current_CH61.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[127, temp_i], NaN));
        Voltage_CH62.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[128, temp_i], NaN));
        Current_CH62.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[129, temp_i], NaN));
        Voltage_CH63.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[130, temp_i], NaN));
        Current_CH63.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[131, temp_i], NaN));
        Voltage_CH64.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[132, temp_i], NaN));
        Current_CH64.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[133, temp_i], NaN));
    end;
    //--
    Voltage_PSU.EndUpdate;
    Current_PSU.EndUpdate;
    Voltage_CH01.EndUpdate;
    Current_CH01.EndUpdate;
    Voltage_CH02.EndUpdate;
    Current_CH02.EndUpdate;
    Voltage_CH03.EndUpdate;
    Current_CH03.EndUpdate;
    Voltage_CH04.EndUpdate;
    Current_CH04.EndUpdate;
    Voltage_CH05.EndUpdate;
    Current_CH05.EndUpdate;
    Voltage_CH06.EndUpdate;
    Current_CH06.EndUpdate;
    Voltage_CH07.EndUpdate;
    Current_CH07.EndUpdate;
    Voltage_CH08.EndUpdate;
    Current_CH08.EndUpdate;
    Voltage_CH09.EndUpdate;
    Current_CH09.EndUpdate;
    Voltage_CH10.EndUpdate;
    Current_CH10.EndUpdate;
    Voltage_CH11.EndUpdate;
    Current_CH11.EndUpdate;
    Voltage_CH12.EndUpdate;
    Current_CH12.EndUpdate;
    Voltage_CH13.EndUpdate;
    Current_CH13.EndUpdate;
    Voltage_CH14.EndUpdate;
    Current_CH14.EndUpdate;
    Voltage_CH15.EndUpdate;
    Current_CH15.EndUpdate;
    Voltage_CH16.EndUpdate;
    Current_CH16.EndUpdate;
    Voltage_CH17.EndUpdate;
    Current_CH17.EndUpdate;
    Voltage_CH18.EndUpdate;
    Current_CH18.EndUpdate;
    Voltage_CH19.EndUpdate;
    Current_CH19.EndUpdate;
    Voltage_CH20.EndUpdate;
    Current_CH20.EndUpdate;
    Voltage_CH21.EndUpdate;
    Current_CH21.EndUpdate;
    Voltage_CH22.EndUpdate;
    Current_CH22.EndUpdate;
    Voltage_CH23.EndUpdate;
    Current_CH23.EndUpdate;
    Voltage_CH24.EndUpdate;
    Current_CH24.EndUpdate;
    Voltage_CH25.EndUpdate;
    Current_CH25.EndUpdate;
    Voltage_CH26.EndUpdate;
    Current_CH26.EndUpdate;
    Voltage_CH27.EndUpdate;
    Current_CH27.EndUpdate;
    Voltage_CH28.EndUpdate;
    Current_CH28.EndUpdate;
    Voltage_CH29.EndUpdate;
    Current_CH29.EndUpdate;
    Voltage_CH30.EndUpdate;
    Current_CH30.EndUpdate;
    Voltage_CH31.EndUpdate;
    Current_CH31.EndUpdate;
    Voltage_CH32.EndUpdate;
    Current_CH32.EndUpdate;
    Voltage_CH33.EndUpdate;
    Current_CH33.EndUpdate;
    Voltage_CH34.EndUpdate;
    Current_CH34.EndUpdate;
    Voltage_CH35.EndUpdate;
    Current_CH35.EndUpdate;
    Voltage_CH36.EndUpdate;
    Current_CH36.EndUpdate;
    Voltage_CH37.EndUpdate;
    Current_CH37.EndUpdate;
    Voltage_CH38.EndUpdate;
    Current_CH38.EndUpdate;
    Voltage_CH39.EndUpdate;
    Current_CH39.EndUpdate;
    Voltage_CH40.EndUpdate;
    Current_CH40.EndUpdate;
    Voltage_CH41.EndUpdate;
    Current_CH41.EndUpdate;
    Voltage_CH42.EndUpdate;
    Current_CH42.EndUpdate;
    Voltage_CH43.EndUpdate;
    Current_CH43.EndUpdate;
    Voltage_CH44.EndUpdate;
    Current_CH44.EndUpdate;
    Voltage_CH45.EndUpdate;
    Current_CH45.EndUpdate;
    Voltage_CH46.EndUpdate;
    Current_CH46.EndUpdate;
    Voltage_CH47.EndUpdate;
    Current_CH47.EndUpdate;
    Voltage_CH48.EndUpdate;
    Current_CH48.EndUpdate;
    Voltage_CH49.EndUpdate;
    Current_CH49.EndUpdate;
    Voltage_CH50.EndUpdate;
    Current_CH50.EndUpdate;
    Voltage_CH51.EndUpdate;
    Current_CH51.EndUpdate;
    Voltage_CH52.EndUpdate;
    Current_CH52.EndUpdate;
    Voltage_CH53.EndUpdate;
    Current_CH53.EndUpdate;
    Voltage_CH54.EndUpdate;
    Current_CH54.EndUpdate;
    Voltage_CH55.EndUpdate;
    Current_CH55.EndUpdate;
    Voltage_CH56.EndUpdate;
    Current_CH56.EndUpdate;
    Voltage_CH57.EndUpdate;
    Current_CH57.EndUpdate;
    Voltage_CH58.EndUpdate;
    Current_CH58.EndUpdate;
    Voltage_CH59.EndUpdate;
    Current_CH59.EndUpdate;
    Voltage_CH60.EndUpdate;
    Current_CH60.EndUpdate;
    Voltage_CH61.EndUpdate;
    Current_CH61.EndUpdate;
    Voltage_CH62.EndUpdate;
    Current_CH62.EndUpdate;
    Voltage_CH63.EndUpdate;
    Current_CH63.EndUpdate;
    Voltage_CH64.EndUpdate;
    Current_CH64.EndUpdate;
    //--
    // 調整啟用禁用狀態
    MainMenu1_1.Enabled := True;
    PageControl1.Enabled := True;
    // 狀態列資訊
    StatusBar1.Panels[0].Text := '載入完成!';
  end
  else
  begin
    // 使用者取消
    RunningLog_Memo.Lines.Add('使用者取消!');
    Application.MessageBox('使用者取消!','錯誤',16);
    // 調整啟用禁用狀態
    MainMenu1_1.Enabled := True;
    PageControl1.Enabled := True;
    // 狀態列資訊
    StatusBar1.Panels[0].Text := '';
    Exit;
  end;
end;        
```
3. **設定「FirstPart_ToolButton」的「Event」頁面下「OnClick」為如下程式碼。**
```pascal
procedure TForm1.FirstPart_ToolButtonClick(Sender: TObject);
var
  temp_i: Integer;
  temp_str: String;
begin
  // 沒有資料時不動作
  if v299_csv_StringGrid.Cells[1, 1] <> 'Time[ms](R2MS_Lite_v299csv_v20240510a)' then
  begin
    Exit;
  end;
  // 狀態列資訊
  StatusBar1.Panels[3].Text := IntToStr(1);
  StatusBar1.Panels[1].Text := '第' + StatusBar1.Panels[3].Text + '次放電';
  StatusBar1.Panels[0].Text := '#' + v299_csv_StringGrid.Cells[0, ( (StrToInt(StatusBar1.Panels[3].Text)-1)*30+1 )+1] + ' ~ #' + v299_csv_StringGrid.Cells[0, ( StrToInt(StatusBar1.Panels[3].Text)*30 )+1];
  // 取出放電狀態
  temp_str := Trim( v299_csv_StringGrid.Cells[5, ( (StrToInt(StatusBar1.Panels[3].Text)-1)*30+2 )] );
  RunningLog_Memo.Lines.Add( temp_str );
  // 依照放電狀態調整元件可見
  if Length(temp_str) = 64 Then
  begin
    //--
    Voltage_PSU.Active := False;
    Current_PSU.Active := False;
    Voltage_CH01.Active := (temp_str[1] = 'v');
    Voltage_CH02.Active := (temp_str[2] = 'v');
    Voltage_CH03.Active := (temp_str[3] = 'v');
    Voltage_CH04.Active := (temp_str[4] = 'v');
    Voltage_CH05.Active := (temp_str[5] = 'v');
    Voltage_CH06.Active := (temp_str[6] = 'v');
    Voltage_CH07.Active := (temp_str[7] = 'v');
    Voltage_CH08.Active := (temp_str[8] = 'v');
    Voltage_CH09.Active := (temp_str[9] = 'v');
    Voltage_CH10.Active := (temp_str[10] = 'v');
    Voltage_CH11.Active := (temp_str[11] = 'v');
    Voltage_CH12.Active := (temp_str[12] = 'v');
    Voltage_CH13.Active := (temp_str[13] = 'v');
    Voltage_CH14.Active := (temp_str[14] = 'v');
    Voltage_CH15.Active := (temp_str[15] = 'v');
    Voltage_CH16.Active := (temp_str[16] = 'v');
    Voltage_CH17.Active := (temp_str[17] = 'v');
    Voltage_CH18.Active := (temp_str[18] = 'v');
    Voltage_CH19.Active := (temp_str[19] = 'v');
    Voltage_CH20.Active := (temp_str[20] = 'v');
    Voltage_CH21.Active := (temp_str[21] = 'v');
    Voltage_CH22.Active := (temp_str[22] = 'v');
    Voltage_CH23.Active := (temp_str[23] = 'v');
    Voltage_CH24.Active := (temp_str[24] = 'v');
    Voltage_CH25.Active := (temp_str[25] = 'v');
    Voltage_CH26.Active := (temp_str[26] = 'v');
    Voltage_CH27.Active := (temp_str[27] = 'v');
    Voltage_CH28.Active := (temp_str[28] = 'v');
    Voltage_CH29.Active := (temp_str[29] = 'v');
    Voltage_CH30.Active := (temp_str[30] = 'v');
    Voltage_CH31.Active := (temp_str[31] = 'v');
    Voltage_CH32.Active := (temp_str[32] = 'v');
    Voltage_CH33.Active := (temp_str[33] = 'v');
    Voltage_CH34.Active := (temp_str[34] = 'v');
    Voltage_CH35.Active := (temp_str[35] = 'v');
    Voltage_CH36.Active := (temp_str[36] = 'v');
    Voltage_CH37.Active := (temp_str[37] = 'v');
    Voltage_CH38.Active := (temp_str[38] = 'v');
    Voltage_CH39.Active := (temp_str[39] = 'v');
    Voltage_CH40.Active := (temp_str[40] = 'v');
    Voltage_CH41.Active := (temp_str[41] = 'v');
    Voltage_CH42.Active := (temp_str[42] = 'v');
    Voltage_CH43.Active := (temp_str[43] = 'v');
    Voltage_CH44.Active := (temp_str[44] = 'v');
    Voltage_CH45.Active := (temp_str[45] = 'v');
    Voltage_CH46.Active := (temp_str[46] = 'v');
    Voltage_CH47.Active := (temp_str[47] = 'v');
    Voltage_CH48.Active := (temp_str[48] = 'v');
    Voltage_CH49.Active := (temp_str[49] = 'v');
    Voltage_CH50.Active := (temp_str[50] = 'v');
    Voltage_CH51.Active := (temp_str[51] = 'v');
    Voltage_CH52.Active := (temp_str[52] = 'v');
    Voltage_CH53.Active := (temp_str[53] = 'v');
    Voltage_CH54.Active := (temp_str[54] = 'v');
    Voltage_CH55.Active := (temp_str[55] = 'v');
    Voltage_CH56.Active := (temp_str[56] = 'v');
    Voltage_CH57.Active := (temp_str[57] = 'v');
    Voltage_CH58.Active := (temp_str[58] = 'v');
    Voltage_CH59.Active := (temp_str[59] = 'v');
    Voltage_CH60.Active := (temp_str[60] = 'v');
    Voltage_CH61.Active := (temp_str[61] = 'v');
    Voltage_CH62.Active := (temp_str[62] = 'v');
    Voltage_CH63.Active := (temp_str[63] = 'v');
    Voltage_CH64.Active := (temp_str[64] = 'v');
    //--
    Current_CH01.Active := not Voltage_CH01.Active;
    Current_CH02.Active := not Voltage_CH02.Active;
    Current_CH03.Active := not Voltage_CH03.Active;
    Current_CH04.Active := not Voltage_CH04.Active;
    Current_CH05.Active := not Voltage_CH05.Active;
    Current_CH06.Active := not Voltage_CH06.Active;
    Current_CH07.Active := not Voltage_CH07.Active;
    Current_CH08.Active := not Voltage_CH08.Active;
    Current_CH09.Active := not Voltage_CH09.Active;
    Current_CH10.Active := not Voltage_CH10.Active;
    Current_CH11.Active := not Voltage_CH11.Active;
    Current_CH12.Active := not Voltage_CH12.Active;
    Current_CH13.Active := not Voltage_CH13.Active;
    Current_CH14.Active := not Voltage_CH14.Active;
    Current_CH15.Active := not Voltage_CH15.Active;
    Current_CH16.Active := not Voltage_CH16.Active;
    Current_CH17.Active := not Voltage_CH17.Active;
    Current_CH18.Active := not Voltage_CH18.Active;
    Current_CH19.Active := not Voltage_CH19.Active;
    Current_CH20.Active := not Voltage_CH20.Active;
    Current_CH21.Active := not Voltage_CH21.Active;
    Current_CH22.Active := not Voltage_CH22.Active;
    Current_CH23.Active := not Voltage_CH23.Active;
    Current_CH24.Active := not Voltage_CH24.Active;
    Current_CH25.Active := not Voltage_CH25.Active;
    Current_CH26.Active := not Voltage_CH26.Active;
    Current_CH27.Active := not Voltage_CH27.Active;
    Current_CH28.Active := not Voltage_CH28.Active;
    Current_CH29.Active := not Voltage_CH29.Active;
    Current_CH30.Active := not Voltage_CH30.Active;
    Current_CH31.Active := not Voltage_CH31.Active;
    Current_CH32.Active := not Voltage_CH32.Active;
    Current_CH33.Active := not Voltage_CH33.Active;
    Current_CH34.Active := not Voltage_CH34.Active;
    Current_CH35.Active := not Voltage_CH35.Active;
    Current_CH36.Active := not Voltage_CH36.Active;
    Current_CH37.Active := not Voltage_CH37.Active;
    Current_CH38.Active := not Voltage_CH38.Active;
    Current_CH39.Active := not Voltage_CH39.Active;
    Current_CH40.Active := not Voltage_CH40.Active;
    Current_CH41.Active := not Voltage_CH41.Active;
    Current_CH42.Active := not Voltage_CH42.Active;
    Current_CH43.Active := not Voltage_CH43.Active;
    Current_CH44.Active := not Voltage_CH44.Active;
    Current_CH45.Active := not Voltage_CH45.Active;
    Current_CH46.Active := not Voltage_CH46.Active;
    Current_CH47.Active := not Voltage_CH47.Active;
    Current_CH48.Active := not Voltage_CH48.Active;
    Current_CH49.Active := not Voltage_CH49.Active;
    Current_CH50.Active := not Voltage_CH50.Active;
    Current_CH51.Active := not Voltage_CH51.Active;
    Current_CH52.Active := not Voltage_CH52.Active;
    Current_CH53.Active := not Voltage_CH53.Active;
    Current_CH54.Active := not Voltage_CH54.Active;
    Current_CH55.Active := not Voltage_CH55.Active;
    Current_CH56.Active := not Voltage_CH56.Active;
    Current_CH57.Active := not Voltage_CH57.Active;
    Current_CH58.Active := not Voltage_CH58.Active;
    Current_CH59.Active := not Voltage_CH59.Active;
    Current_CH60.Active := not Voltage_CH60.Active;
    Current_CH61.Active := not Voltage_CH61.Active;
    Current_CH62.Active := not Voltage_CH62.Active;
    Current_CH63.Active := not Voltage_CH63.Active;
    Current_CH64.Active := not Voltage_CH64.Active;
    //--
  end;
  //--
  // 清空數據
  Voltage_PSU.Clear;
  Current_PSU.Clear;
  Voltage_CH01.Clear;
  Current_CH01.Clear;
  Voltage_CH02.Clear;
  Current_CH02.Clear;
  Voltage_CH03.Clear;
  Current_CH03.Clear;
  Voltage_CH04.Clear;
  Current_CH04.Clear;
  Voltage_CH05.Clear;
  Current_CH05.Clear;
  Voltage_CH06.Clear;
  Current_CH06.Clear;
  Voltage_CH07.Clear;
  Current_CH07.Clear;
  Voltage_CH08.Clear;
  Current_CH08.Clear;
  Voltage_CH09.Clear;
  Current_CH09.Clear;
  Voltage_CH10.Clear;
  Current_CH10.Clear;
  Voltage_CH11.Clear;
  Current_CH11.Clear;
  Voltage_CH12.Clear;
  Current_CH12.Clear;
  Voltage_CH13.Clear;
  Current_CH13.Clear;
  Voltage_CH14.Clear;
  Current_CH14.Clear;
  Voltage_CH15.Clear;
  Current_CH15.Clear;
  Voltage_CH16.Clear;
  Current_CH16.Clear;
  Voltage_CH17.Clear;
  Current_CH17.Clear;
  Voltage_CH18.Clear;
  Current_CH18.Clear;
  Voltage_CH19.Clear;
  Current_CH19.Clear;
  Voltage_CH20.Clear;
  Current_CH20.Clear;
  Voltage_CH21.Clear;
  Current_CH21.Clear;
  Voltage_CH22.Clear;
  Current_CH22.Clear;
  Voltage_CH23.Clear;
  Current_CH23.Clear;
  Voltage_CH24.Clear;
  Current_CH24.Clear;
  Voltage_CH25.Clear;
  Current_CH25.Clear;
  Voltage_CH26.Clear;
  Current_CH26.Clear;
  Voltage_CH27.Clear;
  Current_CH27.Clear;
  Voltage_CH28.Clear;
  Current_CH28.Clear;
  Voltage_CH29.Clear;
  Current_CH29.Clear;
  Voltage_CH30.Clear;
  Current_CH30.Clear;
  Voltage_CH31.Clear;
  Current_CH31.Clear;
  Voltage_CH32.Clear;
  Current_CH32.Clear;
  Voltage_CH33.Clear;
  Current_CH33.Clear;
  Voltage_CH34.Clear;
  Current_CH34.Clear;
  Voltage_CH35.Clear;
  Current_CH35.Clear;
  Voltage_CH36.Clear;
  Current_CH36.Clear;
  Voltage_CH37.Clear;
  Current_CH37.Clear;
  Voltage_CH38.Clear;
  Current_CH38.Clear;
  Voltage_CH39.Clear;
  Current_CH39.Clear;
  Voltage_CH40.Clear;
  Current_CH40.Clear;
  Voltage_CH41.Clear;
  Current_CH41.Clear;
  Voltage_CH42.Clear;
  Current_CH42.Clear;
  Voltage_CH43.Clear;
  Current_CH43.Clear;
  Voltage_CH44.Clear;
  Current_CH44.Clear;
  Voltage_CH45.Clear;
  Current_CH45.Clear;
  Voltage_CH46.Clear;
  Current_CH46.Clear;
  Voltage_CH47.Clear;
  Current_CH47.Clear;
  Voltage_CH48.Clear;
  Current_CH48.Clear;
  Voltage_CH49.Clear;
  Current_CH49.Clear;
  Voltage_CH50.Clear;
  Current_CH50.Clear;
  Voltage_CH51.Clear;
  Current_CH51.Clear;
  Voltage_CH52.Clear;
  Current_CH52.Clear;
  Voltage_CH53.Clear;
  Current_CH53.Clear;
  Voltage_CH54.Clear;
  Current_CH54.Clear;
  Voltage_CH55.Clear;
  Current_CH55.Clear;
  Voltage_CH56.Clear;
  Current_CH56.Clear;
  Voltage_CH57.Clear;
  Current_CH57.Clear;
  Voltage_CH58.Clear;
  Current_CH58.Clear;
  Voltage_CH59.Clear;
  Current_CH59.Clear;
  Voltage_CH60.Clear;
  Current_CH60.Clear;
  Voltage_CH61.Clear;
  Current_CH61.Clear;
  Voltage_CH62.Clear;
  Current_CH62.Clear;
  Voltage_CH63.Clear;
  Current_CH63.Clear;
  Voltage_CH64.Clear;
  Current_CH64.Clear;
  //--
  // 停止更新圖形
  Voltage_PSU.BeginUpdate;
  Current_PSU.BeginUpdate;
  Voltage_CH01.BeginUpdate;
  Current_CH01.BeginUpdate;
  Voltage_CH02.BeginUpdate;
  Current_CH02.BeginUpdate;
  Voltage_CH03.BeginUpdate;
  Current_CH03.BeginUpdate;
  Voltage_CH04.BeginUpdate;
  Current_CH04.BeginUpdate;
  Voltage_CH05.BeginUpdate;
  Current_CH05.BeginUpdate;
  Voltage_CH06.BeginUpdate;
  Current_CH06.BeginUpdate;
  Voltage_CH07.BeginUpdate;
  Current_CH07.BeginUpdate;
  Voltage_CH08.BeginUpdate;
  Current_CH08.BeginUpdate;
  Voltage_CH09.BeginUpdate;
  Current_CH09.BeginUpdate;
  Voltage_CH10.BeginUpdate;
  Current_CH10.BeginUpdate;
  Voltage_CH11.BeginUpdate;
  Current_CH11.BeginUpdate;
  Voltage_CH12.BeginUpdate;
  Current_CH12.BeginUpdate;
  Voltage_CH13.BeginUpdate;
  Current_CH13.BeginUpdate;
  Voltage_CH14.BeginUpdate;
  Current_CH14.BeginUpdate;
  Voltage_CH15.BeginUpdate;
  Current_CH15.BeginUpdate;
  Voltage_CH16.BeginUpdate;
  Current_CH16.BeginUpdate;
  Voltage_CH17.BeginUpdate;
  Current_CH17.BeginUpdate;
  Voltage_CH18.BeginUpdate;
  Current_CH18.BeginUpdate;
  Voltage_CH19.BeginUpdate;
  Current_CH19.BeginUpdate;
  Voltage_CH20.BeginUpdate;
  Current_CH20.BeginUpdate;
  Voltage_CH21.BeginUpdate;
  Current_CH21.BeginUpdate;
  Voltage_CH22.BeginUpdate;
  Current_CH22.BeginUpdate;
  Voltage_CH23.BeginUpdate;
  Current_CH23.BeginUpdate;
  Voltage_CH24.BeginUpdate;
  Current_CH24.BeginUpdate;
  Voltage_CH25.BeginUpdate;
  Current_CH25.BeginUpdate;
  Voltage_CH26.BeginUpdate;
  Current_CH26.BeginUpdate;
  Voltage_CH27.BeginUpdate;
  Current_CH27.BeginUpdate;
  Voltage_CH28.BeginUpdate;
  Current_CH28.BeginUpdate;
  Voltage_CH29.BeginUpdate;
  Current_CH29.BeginUpdate;
  Voltage_CH30.BeginUpdate;
  Current_CH30.BeginUpdate;
  Voltage_CH31.BeginUpdate;
  Current_CH31.BeginUpdate;
  Voltage_CH32.BeginUpdate;
  Current_CH32.BeginUpdate;
  Voltage_CH33.BeginUpdate;
  Current_CH33.BeginUpdate;
  Voltage_CH34.BeginUpdate;
  Current_CH34.BeginUpdate;
  Voltage_CH35.BeginUpdate;
  Current_CH35.BeginUpdate;
  Voltage_CH36.BeginUpdate;
  Current_CH36.BeginUpdate;
  Voltage_CH37.BeginUpdate;
  Current_CH37.BeginUpdate;
  Voltage_CH38.BeginUpdate;
  Current_CH38.BeginUpdate;
  Voltage_CH39.BeginUpdate;
  Current_CH39.BeginUpdate;
  Voltage_CH40.BeginUpdate;
  Current_CH40.BeginUpdate;
  Voltage_CH41.BeginUpdate;
  Current_CH41.BeginUpdate;
  Voltage_CH42.BeginUpdate;
  Current_CH42.BeginUpdate;
  Voltage_CH43.BeginUpdate;
  Current_CH43.BeginUpdate;
  Voltage_CH44.BeginUpdate;
  Current_CH44.BeginUpdate;
  Voltage_CH45.BeginUpdate;
  Current_CH45.BeginUpdate;
  Voltage_CH46.BeginUpdate;
  Current_CH46.BeginUpdate;
  Voltage_CH47.BeginUpdate;
  Current_CH47.BeginUpdate;
  Voltage_CH48.BeginUpdate;
  Current_CH48.BeginUpdate;
  Voltage_CH49.BeginUpdate;
  Current_CH49.BeginUpdate;
  Voltage_CH50.BeginUpdate;
  Current_CH50.BeginUpdate;
  Voltage_CH51.BeginUpdate;
  Current_CH51.BeginUpdate;
  Voltage_CH52.BeginUpdate;
  Current_CH52.BeginUpdate;
  Voltage_CH53.BeginUpdate;
  Current_CH53.BeginUpdate;
  Voltage_CH54.BeginUpdate;
  Current_CH54.BeginUpdate;
  Voltage_CH55.BeginUpdate;
  Current_CH55.BeginUpdate;
  Voltage_CH56.BeginUpdate;
  Current_CH56.BeginUpdate;
  Voltage_CH57.BeginUpdate;
  Current_CH57.BeginUpdate;
  Voltage_CH58.BeginUpdate;
  Current_CH58.BeginUpdate;
  Voltage_CH59.BeginUpdate;
  Current_CH59.BeginUpdate;
  Voltage_CH60.BeginUpdate;
  Current_CH60.BeginUpdate;
  Voltage_CH61.BeginUpdate;
  Current_CH61.BeginUpdate;
  Voltage_CH62.BeginUpdate;
  Current_CH62.BeginUpdate;
  Voltage_CH63.BeginUpdate;
  Current_CH63.BeginUpdate;
  Voltage_CH64.BeginUpdate;
  Current_CH64.BeginUpdate;
  //--
  // 更新圖形數據
  for temp_i := ( (StrToInt(StatusBar1.Panels[3].Text)-1)*30+1 ) to ( StrToInt(StatusBar1.Panels[3].Text)*30 ) do
  begin
    Voltage_PSU.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[2, temp_i+1], NaN));
    Current_PSU.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[3, temp_i+1], NaN));
    Voltage_CH01.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[6, temp_i+1], NaN));
    Current_CH01.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[7, temp_i+1], NaN));
    Voltage_CH02.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[8, temp_i+1], NaN));
    Current_CH02.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[9, temp_i+1], NaN));
    Voltage_CH03.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[10, temp_i+1], NaN));
    Current_CH03.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[11, temp_i+1], NaN));
    Voltage_CH04.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[12, temp_i+1], NaN));
    Current_CH04.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[13, temp_i+1], NaN));
    Voltage_CH05.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[14, temp_i+1], NaN));
    Current_CH05.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[15, temp_i+1], NaN));
    Voltage_CH06.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[16, temp_i+1], NaN));
    Current_CH06.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[17, temp_i+1], NaN));
    Voltage_CH07.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[18, temp_i+1], NaN));
    Current_CH07.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[19, temp_i+1], NaN));
    Voltage_CH08.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[20, temp_i+1], NaN));
    Current_CH08.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[21, temp_i+1], NaN));
    Voltage_CH09.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[22, temp_i+1], NaN));
    Current_CH09.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[23, temp_i+1], NaN));
    Voltage_CH10.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[24, temp_i+1], NaN));
    Current_CH10.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[25, temp_i+1], NaN));
    Voltage_CH11.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[26, temp_i+1], NaN));
    Current_CH11.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[27, temp_i+1], NaN));
    Voltage_CH12.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[28, temp_i+1], NaN));
    Current_CH12.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[29, temp_i+1], NaN));
    Voltage_CH13.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[30, temp_i+1], NaN));
    Current_CH13.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[31, temp_i+1], NaN));
    Voltage_CH14.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[32, temp_i+1], NaN));
    Current_CH14.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[33, temp_i+1], NaN));
    Voltage_CH15.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[34, temp_i+1], NaN));
    Current_CH15.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[35, temp_i+1], NaN));
    Voltage_CH16.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[36, temp_i+1], NaN));
    Current_CH16.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[37, temp_i+1], NaN));
    Voltage_CH17.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[38, temp_i+1], NaN));
    Current_CH17.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[39, temp_i+1], NaN));
    Voltage_CH18.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[40, temp_i+1], NaN));
    Current_CH18.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[41, temp_i+1], NaN));
    Voltage_CH19.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[42, temp_i+1], NaN));
    Current_CH19.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[43, temp_i+1], NaN));
    Voltage_CH20.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[44, temp_i+1], NaN));
    Current_CH20.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[45, temp_i+1], NaN));
    Voltage_CH21.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[46, temp_i+1], NaN));
    Current_CH21.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[47, temp_i+1], NaN));
    Voltage_CH22.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[48, temp_i+1], NaN));
    Current_CH22.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[49, temp_i+1], NaN));
    Voltage_CH23.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[50, temp_i+1], NaN));
    Current_CH23.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[51, temp_i+1], NaN));
    Voltage_CH24.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[52, temp_i+1], NaN));
    Current_CH24.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[53, temp_i+1], NaN));
    Voltage_CH25.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[54, temp_i+1], NaN));
    Current_CH25.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[55, temp_i+1], NaN));
    Voltage_CH26.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[56, temp_i+1], NaN));
    Current_CH26.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[57, temp_i+1], NaN));
    Voltage_CH27.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[58, temp_i+1], NaN));
    Current_CH27.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[59, temp_i+1], NaN));
    Voltage_CH28.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[60, temp_i+1], NaN));
    Current_CH28.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[61, temp_i+1], NaN));
    Voltage_CH29.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[62, temp_i+1], NaN));
    Current_CH29.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[63, temp_i+1], NaN));
    Voltage_CH30.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[64, temp_i+1], NaN));
    Current_CH30.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[65, temp_i+1], NaN));
    Voltage_CH31.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[66, temp_i+1], NaN));
    Current_CH31.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[67, temp_i+1], NaN));
    Voltage_CH32.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[68, temp_i+1], NaN));
    Current_CH32.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[69, temp_i+1], NaN));
    Voltage_CH33.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[70, temp_i+1], NaN));
    Current_CH33.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[71, temp_i+1], NaN));
    Voltage_CH34.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[72, temp_i+1], NaN));
    Current_CH34.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[73, temp_i+1], NaN));
    Voltage_CH35.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[74, temp_i+1], NaN));
    Current_CH35.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[75, temp_i+1], NaN));
    Voltage_CH36.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[76, temp_i+1], NaN));
    Current_CH36.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[77, temp_i+1], NaN));
    Voltage_CH37.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[78, temp_i+1], NaN));
    Current_CH37.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[79, temp_i+1], NaN));
    Voltage_CH38.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[80, temp_i+1], NaN));
    Current_CH38.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[81, temp_i+1], NaN));
    Voltage_CH39.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[82, temp_i+1], NaN));
    Current_CH39.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[83, temp_i+1], NaN));
    Voltage_CH40.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[84, temp_i+1], NaN));
    Current_CH40.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[85, temp_i+1], NaN));
    Voltage_CH41.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[86, temp_i+1], NaN));
    Current_CH41.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[87, temp_i+1], NaN));
    Voltage_CH42.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[88, temp_i+1], NaN));
    Current_CH42.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[89, temp_i+1], NaN));
    Voltage_CH43.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[90, temp_i+1], NaN));
    Current_CH43.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[91, temp_i+1], NaN));
    Voltage_CH44.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[92, temp_i+1], NaN));
    Current_CH44.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[93, temp_i+1], NaN));
    Voltage_CH45.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[94, temp_i+1], NaN));
    Current_CH45.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[95, temp_i+1], NaN));
    Voltage_CH46.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[96, temp_i+1], NaN));
    Current_CH46.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[97, temp_i+1], NaN));
    Voltage_CH47.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[98, temp_i+1], NaN));
    Current_CH47.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[99, temp_i+1], NaN));
    Voltage_CH48.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[100, temp_i+1], NaN));
    Current_CH48.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[101, temp_i+1], NaN));
    Voltage_CH49.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[102, temp_i+1], NaN));
    Current_CH49.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[103, temp_i+1], NaN));
    Voltage_CH50.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[104, temp_i+1], NaN));
    Current_CH50.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[105, temp_i+1], NaN));
    Voltage_CH51.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[106, temp_i+1], NaN));
    Current_CH51.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[107, temp_i+1], NaN));
    Voltage_CH52.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[108, temp_i+1], NaN));
    Current_CH52.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[109, temp_i+1], NaN));
    Voltage_CH53.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[110, temp_i+1], NaN));
    Current_CH53.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[111, temp_i+1], NaN));
    Voltage_CH54.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[112, temp_i+1], NaN));
    Current_CH54.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[113, temp_i+1], NaN));
    Voltage_CH55.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[114, temp_i+1], NaN));
    Current_CH55.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[115, temp_i+1], NaN));
    Voltage_CH56.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[116, temp_i+1], NaN));
    Current_CH56.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[117, temp_i+1], NaN));
    Voltage_CH57.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[118, temp_i+1], NaN));
    Current_CH57.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[119, temp_i+1], NaN));
    Voltage_CH58.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[120, temp_i+1], NaN));
    Current_CH58.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[121, temp_i+1], NaN));
    Voltage_CH59.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[122, temp_i+1], NaN));
    Current_CH59.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[123, temp_i+1], NaN));
    Voltage_CH60.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[124, temp_i+1], NaN));
    Current_CH60.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[125, temp_i+1], NaN));
    Voltage_CH61.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[126, temp_i+1], NaN));
    Current_CH61.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[127, temp_i+1], NaN));
    Voltage_CH62.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[128, temp_i+1], NaN));
    Current_CH62.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[129, temp_i+1], NaN));
    Voltage_CH63.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[130, temp_i+1], NaN));
    Current_CH63.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[131, temp_i+1], NaN));
    Voltage_CH64.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[132, temp_i+1], NaN));
    Current_CH64.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[133, temp_i+1], NaN));
  end;
  //--
  // 允許更新圖形
  Voltage_PSU.EndUpdate;
  Current_PSU.EndUpdate;
  Voltage_CH01.EndUpdate;
  Current_CH01.EndUpdate;
  Voltage_CH02.EndUpdate;
  Current_CH02.EndUpdate;
  Voltage_CH03.EndUpdate;
  Current_CH03.EndUpdate;
  Voltage_CH04.EndUpdate;
  Current_CH04.EndUpdate;
  Voltage_CH05.EndUpdate;
  Current_CH05.EndUpdate;
  Voltage_CH06.EndUpdate;
  Current_CH06.EndUpdate;
  Voltage_CH07.EndUpdate;
  Current_CH07.EndUpdate;
  Voltage_CH08.EndUpdate;
  Current_CH08.EndUpdate;
  Voltage_CH09.EndUpdate;
  Current_CH09.EndUpdate;
  Voltage_CH10.EndUpdate;
  Current_CH10.EndUpdate;
  Voltage_CH11.EndUpdate;
  Current_CH11.EndUpdate;
  Voltage_CH12.EndUpdate;
  Current_CH12.EndUpdate;
  Voltage_CH13.EndUpdate;
  Current_CH13.EndUpdate;
  Voltage_CH14.EndUpdate;
  Current_CH14.EndUpdate;
  Voltage_CH15.EndUpdate;
  Current_CH15.EndUpdate;
  Voltage_CH16.EndUpdate;
  Current_CH16.EndUpdate;
  Voltage_CH17.EndUpdate;
  Current_CH17.EndUpdate;
  Voltage_CH18.EndUpdate;
  Current_CH18.EndUpdate;
  Voltage_CH19.EndUpdate;
  Current_CH19.EndUpdate;
  Voltage_CH20.EndUpdate;
  Current_CH20.EndUpdate;
  Voltage_CH21.EndUpdate;
  Current_CH21.EndUpdate;
  Voltage_CH22.EndUpdate;
  Current_CH22.EndUpdate;
  Voltage_CH23.EndUpdate;
  Current_CH23.EndUpdate;
  Voltage_CH24.EndUpdate;
  Current_CH24.EndUpdate;
  Voltage_CH25.EndUpdate;
  Current_CH25.EndUpdate;
  Voltage_CH26.EndUpdate;
  Current_CH26.EndUpdate;
  Voltage_CH27.EndUpdate;
  Current_CH27.EndUpdate;
  Voltage_CH28.EndUpdate;
  Current_CH28.EndUpdate;
  Voltage_CH29.EndUpdate;
  Current_CH29.EndUpdate;
  Voltage_CH30.EndUpdate;
  Current_CH30.EndUpdate;
  Voltage_CH31.EndUpdate;
  Current_CH31.EndUpdate;
  Voltage_CH32.EndUpdate;
  Current_CH32.EndUpdate;
  Voltage_CH33.EndUpdate;
  Current_CH33.EndUpdate;
  Voltage_CH34.EndUpdate;
  Current_CH34.EndUpdate;
  Voltage_CH35.EndUpdate;
  Current_CH35.EndUpdate;
  Voltage_CH36.EndUpdate;
  Current_CH36.EndUpdate;
  Voltage_CH37.EndUpdate;
  Current_CH37.EndUpdate;
  Voltage_CH38.EndUpdate;
  Current_CH38.EndUpdate;
  Voltage_CH39.EndUpdate;
  Current_CH39.EndUpdate;
  Voltage_CH40.EndUpdate;
  Current_CH40.EndUpdate;
  Voltage_CH41.EndUpdate;
  Current_CH41.EndUpdate;
  Voltage_CH42.EndUpdate;
  Current_CH42.EndUpdate;
  Voltage_CH43.EndUpdate;
  Current_CH43.EndUpdate;
  Voltage_CH44.EndUpdate;
  Current_CH44.EndUpdate;
  Voltage_CH45.EndUpdate;
  Current_CH45.EndUpdate;
  Voltage_CH46.EndUpdate;
  Current_CH46.EndUpdate;
  Voltage_CH47.EndUpdate;
  Current_CH47.EndUpdate;
  Voltage_CH48.EndUpdate;
  Current_CH48.EndUpdate;
  Voltage_CH49.EndUpdate;
  Current_CH49.EndUpdate;
  Voltage_CH50.EndUpdate;
  Current_CH50.EndUpdate;
  Voltage_CH51.EndUpdate;
  Current_CH51.EndUpdate;
  Voltage_CH52.EndUpdate;
  Current_CH52.EndUpdate;
  Voltage_CH53.EndUpdate;
  Current_CH53.EndUpdate;
  Voltage_CH54.EndUpdate;
  Current_CH54.EndUpdate;
  Voltage_CH55.EndUpdate;
  Current_CH55.EndUpdate;
  Voltage_CH56.EndUpdate;
  Current_CH56.EndUpdate;
  Voltage_CH57.EndUpdate;
  Current_CH57.EndUpdate;
  Voltage_CH58.EndUpdate;
  Current_CH58.EndUpdate;
  Voltage_CH59.EndUpdate;
  Current_CH59.EndUpdate;
  Voltage_CH60.EndUpdate;
  Current_CH60.EndUpdate;
  Voltage_CH61.EndUpdate;
  Current_CH61.EndUpdate;
  Voltage_CH62.EndUpdate;
  Current_CH62.EndUpdate;
  Voltage_CH63.EndUpdate;
  Current_CH63.EndUpdate;
  Voltage_CH64.EndUpdate;
  Current_CH64.EndUpdate;
  //--
end; 
```
4. **設定「PreviousPart_ToolButton」的「Event」頁面下「OnClick」為如下程式碼。**
```pascal
procedure TForm1.PreviousPart_ToolButtonClick(Sender: TObject);
var
  temp_i: Integer;
  temp_str: String;
begin
  // 沒有資料時不動作
  if v299_csv_StringGrid.Cells[1, 1] <> 'Time[ms](R2MS_Lite_v299csv_v20240510a)' then
  begin
    Exit;
  end;
  // 狀態列資訊
  if (StrToIntDef(StatusBar1.Panels[3].Text, 1) -1) < 1 Then
  begin
    StatusBar1.Panels[3].Text := IntToStr(1);
    StatusBar1.Panels[1].Text := '第' + StatusBar1.Panels[3].Text + '次放電';
  end
  else
  begin
    StatusBar1.Panels[3].Text := IntToStr(StrToIntDef(StatusBar1.Panels[3].Text, 1)-1);
    StatusBar1.Panels[1].Text := '第' + StatusBar1.Panels[3].Text + '次放電';
  end;
  StatusBar1.Panels[0].Text := '#' + v299_csv_StringGrid.Cells[0, ( (StrToInt(StatusBar1.Panels[3].Text)-1)*30+1 )+1] + ' ~ #' + v299_csv_StringGrid.Cells[0, ( StrToInt(StatusBar1.Panels[3].Text)*30 )+1];
  // 取出放電狀態
  temp_str := Trim( v299_csv_StringGrid.Cells[5, ( (StrToInt(StatusBar1.Panels[3].Text)-1)*30+2 )] );
  RunningLog_Memo.Lines.Add( temp_str );
  // 依照放電狀態調整元件可見
  if Length(temp_str) = 64 Then
  begin
    //--
    Voltage_PSU.Active := False;
    Current_PSU.Active := False;
    Voltage_CH01.Active := (temp_str[1] = 'v');
    Voltage_CH02.Active := (temp_str[2] = 'v');
    Voltage_CH03.Active := (temp_str[3] = 'v');
    Voltage_CH04.Active := (temp_str[4] = 'v');
    Voltage_CH05.Active := (temp_str[5] = 'v');
    Voltage_CH06.Active := (temp_str[6] = 'v');
    Voltage_CH07.Active := (temp_str[7] = 'v');
    Voltage_CH08.Active := (temp_str[8] = 'v');
    Voltage_CH09.Active := (temp_str[9] = 'v');
    Voltage_CH10.Active := (temp_str[10] = 'v');
    Voltage_CH11.Active := (temp_str[11] = 'v');
    Voltage_CH12.Active := (temp_str[12] = 'v');
    Voltage_CH13.Active := (temp_str[13] = 'v');
    Voltage_CH14.Active := (temp_str[14] = 'v');
    Voltage_CH15.Active := (temp_str[15] = 'v');
    Voltage_CH16.Active := (temp_str[16] = 'v');
    Voltage_CH17.Active := (temp_str[17] = 'v');
    Voltage_CH18.Active := (temp_str[18] = 'v');
    Voltage_CH19.Active := (temp_str[19] = 'v');
    Voltage_CH20.Active := (temp_str[20] = 'v');
    Voltage_CH21.Active := (temp_str[21] = 'v');
    Voltage_CH22.Active := (temp_str[22] = 'v');
    Voltage_CH23.Active := (temp_str[23] = 'v');
    Voltage_CH24.Active := (temp_str[24] = 'v');
    Voltage_CH25.Active := (temp_str[25] = 'v');
    Voltage_CH26.Active := (temp_str[26] = 'v');
    Voltage_CH27.Active := (temp_str[27] = 'v');
    Voltage_CH28.Active := (temp_str[28] = 'v');
    Voltage_CH29.Active := (temp_str[29] = 'v');
    Voltage_CH30.Active := (temp_str[30] = 'v');
    Voltage_CH31.Active := (temp_str[31] = 'v');
    Voltage_CH32.Active := (temp_str[32] = 'v');
    Voltage_CH33.Active := (temp_str[33] = 'v');
    Voltage_CH34.Active := (temp_str[34] = 'v');
    Voltage_CH35.Active := (temp_str[35] = 'v');
    Voltage_CH36.Active := (temp_str[36] = 'v');
    Voltage_CH37.Active := (temp_str[37] = 'v');
    Voltage_CH38.Active := (temp_str[38] = 'v');
    Voltage_CH39.Active := (temp_str[39] = 'v');
    Voltage_CH40.Active := (temp_str[40] = 'v');
    Voltage_CH41.Active := (temp_str[41] = 'v');
    Voltage_CH42.Active := (temp_str[42] = 'v');
    Voltage_CH43.Active := (temp_str[43] = 'v');
    Voltage_CH44.Active := (temp_str[44] = 'v');
    Voltage_CH45.Active := (temp_str[45] = 'v');
    Voltage_CH46.Active := (temp_str[46] = 'v');
    Voltage_CH47.Active := (temp_str[47] = 'v');
    Voltage_CH48.Active := (temp_str[48] = 'v');
    Voltage_CH49.Active := (temp_str[49] = 'v');
    Voltage_CH50.Active := (temp_str[50] = 'v');
    Voltage_CH51.Active := (temp_str[51] = 'v');
    Voltage_CH52.Active := (temp_str[52] = 'v');
    Voltage_CH53.Active := (temp_str[53] = 'v');
    Voltage_CH54.Active := (temp_str[54] = 'v');
    Voltage_CH55.Active := (temp_str[55] = 'v');
    Voltage_CH56.Active := (temp_str[56] = 'v');
    Voltage_CH57.Active := (temp_str[57] = 'v');
    Voltage_CH58.Active := (temp_str[58] = 'v');
    Voltage_CH59.Active := (temp_str[59] = 'v');
    Voltage_CH60.Active := (temp_str[60] = 'v');
    Voltage_CH61.Active := (temp_str[61] = 'v');
    Voltage_CH62.Active := (temp_str[62] = 'v');
    Voltage_CH63.Active := (temp_str[63] = 'v');
    Voltage_CH64.Active := (temp_str[64] = 'v');
    //--
    Current_CH01.Active := not Voltage_CH01.Active;
    Current_CH02.Active := not Voltage_CH02.Active;
    Current_CH03.Active := not Voltage_CH03.Active;
    Current_CH04.Active := not Voltage_CH04.Active;
    Current_CH05.Active := not Voltage_CH05.Active;
    Current_CH06.Active := not Voltage_CH06.Active;
    Current_CH07.Active := not Voltage_CH07.Active;
    Current_CH08.Active := not Voltage_CH08.Active;
    Current_CH09.Active := not Voltage_CH09.Active;
    Current_CH10.Active := not Voltage_CH10.Active;
    Current_CH11.Active := not Voltage_CH11.Active;
    Current_CH12.Active := not Voltage_CH12.Active;
    Current_CH13.Active := not Voltage_CH13.Active;
    Current_CH14.Active := not Voltage_CH14.Active;
    Current_CH15.Active := not Voltage_CH15.Active;
    Current_CH16.Active := not Voltage_CH16.Active;
    Current_CH17.Active := not Voltage_CH17.Active;
    Current_CH18.Active := not Voltage_CH18.Active;
    Current_CH19.Active := not Voltage_CH19.Active;
    Current_CH20.Active := not Voltage_CH20.Active;
    Current_CH21.Active := not Voltage_CH21.Active;
    Current_CH22.Active := not Voltage_CH22.Active;
    Current_CH23.Active := not Voltage_CH23.Active;
    Current_CH24.Active := not Voltage_CH24.Active;
    Current_CH25.Active := not Voltage_CH25.Active;
    Current_CH26.Active := not Voltage_CH26.Active;
    Current_CH27.Active := not Voltage_CH27.Active;
    Current_CH28.Active := not Voltage_CH28.Active;
    Current_CH29.Active := not Voltage_CH29.Active;
    Current_CH30.Active := not Voltage_CH30.Active;
    Current_CH31.Active := not Voltage_CH31.Active;
    Current_CH32.Active := not Voltage_CH32.Active;
    Current_CH33.Active := not Voltage_CH33.Active;
    Current_CH34.Active := not Voltage_CH34.Active;
    Current_CH35.Active := not Voltage_CH35.Active;
    Current_CH36.Active := not Voltage_CH36.Active;
    Current_CH37.Active := not Voltage_CH37.Active;
    Current_CH38.Active := not Voltage_CH38.Active;
    Current_CH39.Active := not Voltage_CH39.Active;
    Current_CH40.Active := not Voltage_CH40.Active;
    Current_CH41.Active := not Voltage_CH41.Active;
    Current_CH42.Active := not Voltage_CH42.Active;
    Current_CH43.Active := not Voltage_CH43.Active;
    Current_CH44.Active := not Voltage_CH44.Active;
    Current_CH45.Active := not Voltage_CH45.Active;
    Current_CH46.Active := not Voltage_CH46.Active;
    Current_CH47.Active := not Voltage_CH47.Active;
    Current_CH48.Active := not Voltage_CH48.Active;
    Current_CH49.Active := not Voltage_CH49.Active;
    Current_CH50.Active := not Voltage_CH50.Active;
    Current_CH51.Active := not Voltage_CH51.Active;
    Current_CH52.Active := not Voltage_CH52.Active;
    Current_CH53.Active := not Voltage_CH53.Active;
    Current_CH54.Active := not Voltage_CH54.Active;
    Current_CH55.Active := not Voltage_CH55.Active;
    Current_CH56.Active := not Voltage_CH56.Active;
    Current_CH57.Active := not Voltage_CH57.Active;
    Current_CH58.Active := not Voltage_CH58.Active;
    Current_CH59.Active := not Voltage_CH59.Active;
    Current_CH60.Active := not Voltage_CH60.Active;
    Current_CH61.Active := not Voltage_CH61.Active;
    Current_CH62.Active := not Voltage_CH62.Active;
    Current_CH63.Active := not Voltage_CH63.Active;
    Current_CH64.Active := not Voltage_CH64.Active;
    //--
  end;
  //--
  // 清空數據
  Voltage_PSU.Clear;
  Current_PSU.Clear;
  Voltage_CH01.Clear;
  Current_CH01.Clear;
  Voltage_CH02.Clear;
  Current_CH02.Clear;
  Voltage_CH03.Clear;
  Current_CH03.Clear;
  Voltage_CH04.Clear;
  Current_CH04.Clear;
  Voltage_CH05.Clear;
  Current_CH05.Clear;
  Voltage_CH06.Clear;
  Current_CH06.Clear;
  Voltage_CH07.Clear;
  Current_CH07.Clear;
  Voltage_CH08.Clear;
  Current_CH08.Clear;
  Voltage_CH09.Clear;
  Current_CH09.Clear;
  Voltage_CH10.Clear;
  Current_CH10.Clear;
  Voltage_CH11.Clear;
  Current_CH11.Clear;
  Voltage_CH12.Clear;
  Current_CH12.Clear;
  Voltage_CH13.Clear;
  Current_CH13.Clear;
  Voltage_CH14.Clear;
  Current_CH14.Clear;
  Voltage_CH15.Clear;
  Current_CH15.Clear;
  Voltage_CH16.Clear;
  Current_CH16.Clear;
  Voltage_CH17.Clear;
  Current_CH17.Clear;
  Voltage_CH18.Clear;
  Current_CH18.Clear;
  Voltage_CH19.Clear;
  Current_CH19.Clear;
  Voltage_CH20.Clear;
  Current_CH20.Clear;
  Voltage_CH21.Clear;
  Current_CH21.Clear;
  Voltage_CH22.Clear;
  Current_CH22.Clear;
  Voltage_CH23.Clear;
  Current_CH23.Clear;
  Voltage_CH24.Clear;
  Current_CH24.Clear;
  Voltage_CH25.Clear;
  Current_CH25.Clear;
  Voltage_CH26.Clear;
  Current_CH26.Clear;
  Voltage_CH27.Clear;
  Current_CH27.Clear;
  Voltage_CH28.Clear;
  Current_CH28.Clear;
  Voltage_CH29.Clear;
  Current_CH29.Clear;
  Voltage_CH30.Clear;
  Current_CH30.Clear;
  Voltage_CH31.Clear;
  Current_CH31.Clear;
  Voltage_CH32.Clear;
  Current_CH32.Clear;
  Voltage_CH33.Clear;
  Current_CH33.Clear;
  Voltage_CH34.Clear;
  Current_CH34.Clear;
  Voltage_CH35.Clear;
  Current_CH35.Clear;
  Voltage_CH36.Clear;
  Current_CH36.Clear;
  Voltage_CH37.Clear;
  Current_CH37.Clear;
  Voltage_CH38.Clear;
  Current_CH38.Clear;
  Voltage_CH39.Clear;
  Current_CH39.Clear;
  Voltage_CH40.Clear;
  Current_CH40.Clear;
  Voltage_CH41.Clear;
  Current_CH41.Clear;
  Voltage_CH42.Clear;
  Current_CH42.Clear;
  Voltage_CH43.Clear;
  Current_CH43.Clear;
  Voltage_CH44.Clear;
  Current_CH44.Clear;
  Voltage_CH45.Clear;
  Current_CH45.Clear;
  Voltage_CH46.Clear;
  Current_CH46.Clear;
  Voltage_CH47.Clear;
  Current_CH47.Clear;
  Voltage_CH48.Clear;
  Current_CH48.Clear;
  Voltage_CH49.Clear;
  Current_CH49.Clear;
  Voltage_CH50.Clear;
  Current_CH50.Clear;
  Voltage_CH51.Clear;
  Current_CH51.Clear;
  Voltage_CH52.Clear;
  Current_CH52.Clear;
  Voltage_CH53.Clear;
  Current_CH53.Clear;
  Voltage_CH54.Clear;
  Current_CH54.Clear;
  Voltage_CH55.Clear;
  Current_CH55.Clear;
  Voltage_CH56.Clear;
  Current_CH56.Clear;
  Voltage_CH57.Clear;
  Current_CH57.Clear;
  Voltage_CH58.Clear;
  Current_CH58.Clear;
  Voltage_CH59.Clear;
  Current_CH59.Clear;
  Voltage_CH60.Clear;
  Current_CH60.Clear;
  Voltage_CH61.Clear;
  Current_CH61.Clear;
  Voltage_CH62.Clear;
  Current_CH62.Clear;
  Voltage_CH63.Clear;
  Current_CH63.Clear;
  Voltage_CH64.Clear;
  Current_CH64.Clear;
  //--
  // 停止更新圖形
  Voltage_PSU.BeginUpdate;
  Current_PSU.BeginUpdate;
  Voltage_CH01.BeginUpdate;
  Current_CH01.BeginUpdate;
  Voltage_CH02.BeginUpdate;
  Current_CH02.BeginUpdate;
  Voltage_CH03.BeginUpdate;
  Current_CH03.BeginUpdate;
  Voltage_CH04.BeginUpdate;
  Current_CH04.BeginUpdate;
  Voltage_CH05.BeginUpdate;
  Current_CH05.BeginUpdate;
  Voltage_CH06.BeginUpdate;
  Current_CH06.BeginUpdate;
  Voltage_CH07.BeginUpdate;
  Current_CH07.BeginUpdate;
  Voltage_CH08.BeginUpdate;
  Current_CH08.BeginUpdate;
  Voltage_CH09.BeginUpdate;
  Current_CH09.BeginUpdate;
  Voltage_CH10.BeginUpdate;
  Current_CH10.BeginUpdate;
  Voltage_CH11.BeginUpdate;
  Current_CH11.BeginUpdate;
  Voltage_CH12.BeginUpdate;
  Current_CH12.BeginUpdate;
  Voltage_CH13.BeginUpdate;
  Current_CH13.BeginUpdate;
  Voltage_CH14.BeginUpdate;
  Current_CH14.BeginUpdate;
  Voltage_CH15.BeginUpdate;
  Current_CH15.BeginUpdate;
  Voltage_CH16.BeginUpdate;
  Current_CH16.BeginUpdate;
  Voltage_CH17.BeginUpdate;
  Current_CH17.BeginUpdate;
  Voltage_CH18.BeginUpdate;
  Current_CH18.BeginUpdate;
  Voltage_CH19.BeginUpdate;
  Current_CH19.BeginUpdate;
  Voltage_CH20.BeginUpdate;
  Current_CH20.BeginUpdate;
  Voltage_CH21.BeginUpdate;
  Current_CH21.BeginUpdate;
  Voltage_CH22.BeginUpdate;
  Current_CH22.BeginUpdate;
  Voltage_CH23.BeginUpdate;
  Current_CH23.BeginUpdate;
  Voltage_CH24.BeginUpdate;
  Current_CH24.BeginUpdate;
  Voltage_CH25.BeginUpdate;
  Current_CH25.BeginUpdate;
  Voltage_CH26.BeginUpdate;
  Current_CH26.BeginUpdate;
  Voltage_CH27.BeginUpdate;
  Current_CH27.BeginUpdate;
  Voltage_CH28.BeginUpdate;
  Current_CH28.BeginUpdate;
  Voltage_CH29.BeginUpdate;
  Current_CH29.BeginUpdate;
  Voltage_CH30.BeginUpdate;
  Current_CH30.BeginUpdate;
  Voltage_CH31.BeginUpdate;
  Current_CH31.BeginUpdate;
  Voltage_CH32.BeginUpdate;
  Current_CH32.BeginUpdate;
  Voltage_CH33.BeginUpdate;
  Current_CH33.BeginUpdate;
  Voltage_CH34.BeginUpdate;
  Current_CH34.BeginUpdate;
  Voltage_CH35.BeginUpdate;
  Current_CH35.BeginUpdate;
  Voltage_CH36.BeginUpdate;
  Current_CH36.BeginUpdate;
  Voltage_CH37.BeginUpdate;
  Current_CH37.BeginUpdate;
  Voltage_CH38.BeginUpdate;
  Current_CH38.BeginUpdate;
  Voltage_CH39.BeginUpdate;
  Current_CH39.BeginUpdate;
  Voltage_CH40.BeginUpdate;
  Current_CH40.BeginUpdate;
  Voltage_CH41.BeginUpdate;
  Current_CH41.BeginUpdate;
  Voltage_CH42.BeginUpdate;
  Current_CH42.BeginUpdate;
  Voltage_CH43.BeginUpdate;
  Current_CH43.BeginUpdate;
  Voltage_CH44.BeginUpdate;
  Current_CH44.BeginUpdate;
  Voltage_CH45.BeginUpdate;
  Current_CH45.BeginUpdate;
  Voltage_CH46.BeginUpdate;
  Current_CH46.BeginUpdate;
  Voltage_CH47.BeginUpdate;
  Current_CH47.BeginUpdate;
  Voltage_CH48.BeginUpdate;
  Current_CH48.BeginUpdate;
  Voltage_CH49.BeginUpdate;
  Current_CH49.BeginUpdate;
  Voltage_CH50.BeginUpdate;
  Current_CH50.BeginUpdate;
  Voltage_CH51.BeginUpdate;
  Current_CH51.BeginUpdate;
  Voltage_CH52.BeginUpdate;
  Current_CH52.BeginUpdate;
  Voltage_CH53.BeginUpdate;
  Current_CH53.BeginUpdate;
  Voltage_CH54.BeginUpdate;
  Current_CH54.BeginUpdate;
  Voltage_CH55.BeginUpdate;
  Current_CH55.BeginUpdate;
  Voltage_CH56.BeginUpdate;
  Current_CH56.BeginUpdate;
  Voltage_CH57.BeginUpdate;
  Current_CH57.BeginUpdate;
  Voltage_CH58.BeginUpdate;
  Current_CH58.BeginUpdate;
  Voltage_CH59.BeginUpdate;
  Current_CH59.BeginUpdate;
  Voltage_CH60.BeginUpdate;
  Current_CH60.BeginUpdate;
  Voltage_CH61.BeginUpdate;
  Current_CH61.BeginUpdate;
  Voltage_CH62.BeginUpdate;
  Current_CH62.BeginUpdate;
  Voltage_CH63.BeginUpdate;
  Current_CH63.BeginUpdate;
  Voltage_CH64.BeginUpdate;
  Current_CH64.BeginUpdate;
  //--
  // 更新圖形數據
  for temp_i := ( (StrToInt(StatusBar1.Panels[3].Text)-1)*30+1 ) to ( StrToInt(StatusBar1.Panels[3].Text)*30 ) do
  begin
    Voltage_PSU.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[2, temp_i+1], NaN));
    Current_PSU.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[3, temp_i+1], NaN));
    Voltage_CH01.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[6, temp_i+1], NaN));
    Current_CH01.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[7, temp_i+1], NaN));
    Voltage_CH02.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[8, temp_i+1], NaN));
    Current_CH02.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[9, temp_i+1], NaN));
    Voltage_CH03.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[10, temp_i+1], NaN));
    Current_CH03.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[11, temp_i+1], NaN));
    Voltage_CH04.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[12, temp_i+1], NaN));
    Current_CH04.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[13, temp_i+1], NaN));
    Voltage_CH05.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[14, temp_i+1], NaN));
    Current_CH05.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[15, temp_i+1], NaN));
    Voltage_CH06.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[16, temp_i+1], NaN));
    Current_CH06.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[17, temp_i+1], NaN));
    Voltage_CH07.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[18, temp_i+1], NaN));
    Current_CH07.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[19, temp_i+1], NaN));
    Voltage_CH08.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[20, temp_i+1], NaN));
    Current_CH08.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[21, temp_i+1], NaN));
    Voltage_CH09.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[22, temp_i+1], NaN));
    Current_CH09.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[23, temp_i+1], NaN));
    Voltage_CH10.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[24, temp_i+1], NaN));
    Current_CH10.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[25, temp_i+1], NaN));
    Voltage_CH11.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[26, temp_i+1], NaN));
    Current_CH11.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[27, temp_i+1], NaN));
    Voltage_CH12.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[28, temp_i+1], NaN));
    Current_CH12.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[29, temp_i+1], NaN));
    Voltage_CH13.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[30, temp_i+1], NaN));
    Current_CH13.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[31, temp_i+1], NaN));
    Voltage_CH14.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[32, temp_i+1], NaN));
    Current_CH14.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[33, temp_i+1], NaN));
    Voltage_CH15.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[34, temp_i+1], NaN));
    Current_CH15.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[35, temp_i+1], NaN));
    Voltage_CH16.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[36, temp_i+1], NaN));
    Current_CH16.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[37, temp_i+1], NaN));
    Voltage_CH17.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[38, temp_i+1], NaN));
    Current_CH17.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[39, temp_i+1], NaN));
    Voltage_CH18.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[40, temp_i+1], NaN));
    Current_CH18.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[41, temp_i+1], NaN));
    Voltage_CH19.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[42, temp_i+1], NaN));
    Current_CH19.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[43, temp_i+1], NaN));
    Voltage_CH20.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[44, temp_i+1], NaN));
    Current_CH20.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[45, temp_i+1], NaN));
    Voltage_CH21.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[46, temp_i+1], NaN));
    Current_CH21.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[47, temp_i+1], NaN));
    Voltage_CH22.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[48, temp_i+1], NaN));
    Current_CH22.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[49, temp_i+1], NaN));
    Voltage_CH23.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[50, temp_i+1], NaN));
    Current_CH23.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[51, temp_i+1], NaN));
    Voltage_CH24.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[52, temp_i+1], NaN));
    Current_CH24.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[53, temp_i+1], NaN));
    Voltage_CH25.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[54, temp_i+1], NaN));
    Current_CH25.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[55, temp_i+1], NaN));
    Voltage_CH26.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[56, temp_i+1], NaN));
    Current_CH26.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[57, temp_i+1], NaN));
    Voltage_CH27.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[58, temp_i+1], NaN));
    Current_CH27.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[59, temp_i+1], NaN));
    Voltage_CH28.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[60, temp_i+1], NaN));
    Current_CH28.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[61, temp_i+1], NaN));
    Voltage_CH29.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[62, temp_i+1], NaN));
    Current_CH29.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[63, temp_i+1], NaN));
    Voltage_CH30.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[64, temp_i+1], NaN));
    Current_CH30.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[65, temp_i+1], NaN));
    Voltage_CH31.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[66, temp_i+1], NaN));
    Current_CH31.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[67, temp_i+1], NaN));
    Voltage_CH32.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[68, temp_i+1], NaN));
    Current_CH32.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[69, temp_i+1], NaN));
    Voltage_CH33.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[70, temp_i+1], NaN));
    Current_CH33.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[71, temp_i+1], NaN));
    Voltage_CH34.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[72, temp_i+1], NaN));
    Current_CH34.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[73, temp_i+1], NaN));
    Voltage_CH35.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[74, temp_i+1], NaN));
    Current_CH35.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[75, temp_i+1], NaN));
    Voltage_CH36.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[76, temp_i+1], NaN));
    Current_CH36.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[77, temp_i+1], NaN));
    Voltage_CH37.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[78, temp_i+1], NaN));
    Current_CH37.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[79, temp_i+1], NaN));
    Voltage_CH38.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[80, temp_i+1], NaN));
    Current_CH38.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[81, temp_i+1], NaN));
    Voltage_CH39.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[82, temp_i+1], NaN));
    Current_CH39.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[83, temp_i+1], NaN));
    Voltage_CH40.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[84, temp_i+1], NaN));
    Current_CH40.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[85, temp_i+1], NaN));
    Voltage_CH41.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[86, temp_i+1], NaN));
    Current_CH41.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[87, temp_i+1], NaN));
    Voltage_CH42.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[88, temp_i+1], NaN));
    Current_CH42.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[89, temp_i+1], NaN));
    Voltage_CH43.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[90, temp_i+1], NaN));
    Current_CH43.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[91, temp_i+1], NaN));
    Voltage_CH44.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[92, temp_i+1], NaN));
    Current_CH44.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[93, temp_i+1], NaN));
    Voltage_CH45.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[94, temp_i+1], NaN));
    Current_CH45.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[95, temp_i+1], NaN));
    Voltage_CH46.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[96, temp_i+1], NaN));
    Current_CH46.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[97, temp_i+1], NaN));
    Voltage_CH47.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[98, temp_i+1], NaN));
    Current_CH47.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[99, temp_i+1], NaN));
    Voltage_CH48.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[100, temp_i+1], NaN));
    Current_CH48.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[101, temp_i+1], NaN));
    Voltage_CH49.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[102, temp_i+1], NaN));
    Current_CH49.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[103, temp_i+1], NaN));
    Voltage_CH50.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[104, temp_i+1], NaN));
    Current_CH50.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[105, temp_i+1], NaN));
    Voltage_CH51.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[106, temp_i+1], NaN));
    Current_CH51.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[107, temp_i+1], NaN));
    Voltage_CH52.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[108, temp_i+1], NaN));
    Current_CH52.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[109, temp_i+1], NaN));
    Voltage_CH53.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[110, temp_i+1], NaN));
    Current_CH53.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[111, temp_i+1], NaN));
    Voltage_CH54.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[112, temp_i+1], NaN));
    Current_CH54.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[113, temp_i+1], NaN));
    Voltage_CH55.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[114, temp_i+1], NaN));
    Current_CH55.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[115, temp_i+1], NaN));
    Voltage_CH56.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[116, temp_i+1], NaN));
    Current_CH56.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[117, temp_i+1], NaN));
    Voltage_CH57.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[118, temp_i+1], NaN));
    Current_CH57.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[119, temp_i+1], NaN));
    Voltage_CH58.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[120, temp_i+1], NaN));
    Current_CH58.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[121, temp_i+1], NaN));
    Voltage_CH59.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[122, temp_i+1], NaN));
    Current_CH59.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[123, temp_i+1], NaN));
    Voltage_CH60.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[124, temp_i+1], NaN));
    Current_CH60.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[125, temp_i+1], NaN));
    Voltage_CH61.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[126, temp_i+1], NaN));
    Current_CH61.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[127, temp_i+1], NaN));
    Voltage_CH62.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[128, temp_i+1], NaN));
    Current_CH62.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[129, temp_i+1], NaN));
    Voltage_CH63.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[130, temp_i+1], NaN));
    Current_CH63.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[131, temp_i+1], NaN));
    Voltage_CH64.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[132, temp_i+1], NaN));
    Current_CH64.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[133, temp_i+1], NaN));
  end;
  //--
  // 允許更新圖形
  Voltage_PSU.EndUpdate;
  Current_PSU.EndUpdate;
  Voltage_CH01.EndUpdate;
  Current_CH01.EndUpdate;
  Voltage_CH02.EndUpdate;
  Current_CH02.EndUpdate;
  Voltage_CH03.EndUpdate;
  Current_CH03.EndUpdate;
  Voltage_CH04.EndUpdate;
  Current_CH04.EndUpdate;
  Voltage_CH05.EndUpdate;
  Current_CH05.EndUpdate;
  Voltage_CH06.EndUpdate;
  Current_CH06.EndUpdate;
  Voltage_CH07.EndUpdate;
  Current_CH07.EndUpdate;
  Voltage_CH08.EndUpdate;
  Current_CH08.EndUpdate;
  Voltage_CH09.EndUpdate;
  Current_CH09.EndUpdate;
  Voltage_CH10.EndUpdate;
  Current_CH10.EndUpdate;
  Voltage_CH11.EndUpdate;
  Current_CH11.EndUpdate;
  Voltage_CH12.EndUpdate;
  Current_CH12.EndUpdate;
  Voltage_CH13.EndUpdate;
  Current_CH13.EndUpdate;
  Voltage_CH14.EndUpdate;
  Current_CH14.EndUpdate;
  Voltage_CH15.EndUpdate;
  Current_CH15.EndUpdate;
  Voltage_CH16.EndUpdate;
  Current_CH16.EndUpdate;
  Voltage_CH17.EndUpdate;
  Current_CH17.EndUpdate;
  Voltage_CH18.EndUpdate;
  Current_CH18.EndUpdate;
  Voltage_CH19.EndUpdate;
  Current_CH19.EndUpdate;
  Voltage_CH20.EndUpdate;
  Current_CH20.EndUpdate;
  Voltage_CH21.EndUpdate;
  Current_CH21.EndUpdate;
  Voltage_CH22.EndUpdate;
  Current_CH22.EndUpdate;
  Voltage_CH23.EndUpdate;
  Current_CH23.EndUpdate;
  Voltage_CH24.EndUpdate;
  Current_CH24.EndUpdate;
  Voltage_CH25.EndUpdate;
  Current_CH25.EndUpdate;
  Voltage_CH26.EndUpdate;
  Current_CH26.EndUpdate;
  Voltage_CH27.EndUpdate;
  Current_CH27.EndUpdate;
  Voltage_CH28.EndUpdate;
  Current_CH28.EndUpdate;
  Voltage_CH29.EndUpdate;
  Current_CH29.EndUpdate;
  Voltage_CH30.EndUpdate;
  Current_CH30.EndUpdate;
  Voltage_CH31.EndUpdate;
  Current_CH31.EndUpdate;
  Voltage_CH32.EndUpdate;
  Current_CH32.EndUpdate;
  Voltage_CH33.EndUpdate;
  Current_CH33.EndUpdate;
  Voltage_CH34.EndUpdate;
  Current_CH34.EndUpdate;
  Voltage_CH35.EndUpdate;
  Current_CH35.EndUpdate;
  Voltage_CH36.EndUpdate;
  Current_CH36.EndUpdate;
  Voltage_CH37.EndUpdate;
  Current_CH37.EndUpdate;
  Voltage_CH38.EndUpdate;
  Current_CH38.EndUpdate;
  Voltage_CH39.EndUpdate;
  Current_CH39.EndUpdate;
  Voltage_CH40.EndUpdate;
  Current_CH40.EndUpdate;
  Voltage_CH41.EndUpdate;
  Current_CH41.EndUpdate;
  Voltage_CH42.EndUpdate;
  Current_CH42.EndUpdate;
  Voltage_CH43.EndUpdate;
  Current_CH43.EndUpdate;
  Voltage_CH44.EndUpdate;
  Current_CH44.EndUpdate;
  Voltage_CH45.EndUpdate;
  Current_CH45.EndUpdate;
  Voltage_CH46.EndUpdate;
  Current_CH46.EndUpdate;
  Voltage_CH47.EndUpdate;
  Current_CH47.EndUpdate;
  Voltage_CH48.EndUpdate;
  Current_CH48.EndUpdate;
  Voltage_CH49.EndUpdate;
  Current_CH49.EndUpdate;
  Voltage_CH50.EndUpdate;
  Current_CH50.EndUpdate;
  Voltage_CH51.EndUpdate;
  Current_CH51.EndUpdate;
  Voltage_CH52.EndUpdate;
  Current_CH52.EndUpdate;
  Voltage_CH53.EndUpdate;
  Current_CH53.EndUpdate;
  Voltage_CH54.EndUpdate;
  Current_CH54.EndUpdate;
  Voltage_CH55.EndUpdate;
  Current_CH55.EndUpdate;
  Voltage_CH56.EndUpdate;
  Current_CH56.EndUpdate;
  Voltage_CH57.EndUpdate;
  Current_CH57.EndUpdate;
  Voltage_CH58.EndUpdate;
  Current_CH58.EndUpdate;
  Voltage_CH59.EndUpdate;
  Current_CH59.EndUpdate;
  Voltage_CH60.EndUpdate;
  Current_CH60.EndUpdate;
  Voltage_CH61.EndUpdate;
  Current_CH61.EndUpdate;
  Voltage_CH62.EndUpdate;
  Current_CH62.EndUpdate;
  Voltage_CH63.EndUpdate;
  Current_CH63.EndUpdate;
  Voltage_CH64.EndUpdate;
  Current_CH64.EndUpdate;
  //--
end;  
```
5. **設定「SelectPart_ToolButton」的「Event」頁面下「OnClick」為如下程式碼。**
```pascal
procedure TForm1.SelectPart_ToolButtonClick(Sender: TObject);
var
  temp_i: Integer;
  temp_str: String;
begin
  // 沒有資料時不動作
  if v299_csv_StringGrid.Cells[1, 1] <> 'Time[ms](R2MS_Lite_v299csv_v20240510a)' then
  begin
    Exit;
  end;
  // 狀態列資訊
  StatusBar1.Panels[3].Text := InputBox('選擇目標', '請輸入要選擇的次數：',  StatusBar1.Panels[3].Text);
  if (StrToIntDef(StatusBar1.Panels[3].Text, 0)) < 1 Then
  begin
    StatusBar1.Panels[3].Text := IntToStr(1);
    StatusBar1.Panels[1].Text := '第' + StatusBar1.Panels[3].Text + '次放電';
  end
  else if (StrToIntDef(StatusBar1.Panels[3].Text, 1) +1) > StrToInt(StatusBar1.Panels[4].Text) Then
  begin
    StatusBar1.Panels[3].Text := StatusBar1.Panels[4].Text;
    StatusBar1.Panels[1].Text := '第' + StatusBar1.Panels[3].Text + '次放電';
  end
  else
  begin
    StatusBar1.Panels[3].Text := IntToStr(StrToIntDef(StatusBar1.Panels[3].Text, 1));
    StatusBar1.Panels[1].Text := '第' + StatusBar1.Panels[3].Text + '次放電';
  end;
  StatusBar1.Panels[0].Text := '#' + v299_csv_StringGrid.Cells[0, ( (StrToInt(StatusBar1.Panels[3].Text)-1)*30+1 )+1] + ' ~ #' + v299_csv_StringGrid.Cells[0, ( StrToInt(StatusBar1.Panels[3].Text)*30 )+1];
  // 取出放電狀態
  temp_str := Trim( v299_csv_StringGrid.Cells[5, ( (StrToInt(StatusBar1.Panels[3].Text)-1)*30+2 )] );
  RunningLog_Memo.Lines.Add( temp_str );
  // 依照放電狀態調整元件可見
  if Length(temp_str) = 64 Then
  begin
    //--
    Voltage_PSU.Active := False;
    Current_PSU.Active := False;
    Voltage_CH01.Active := (temp_str[1] = 'v');
    Voltage_CH02.Active := (temp_str[2] = 'v');
    Voltage_CH03.Active := (temp_str[3] = 'v');
    Voltage_CH04.Active := (temp_str[4] = 'v');
    Voltage_CH05.Active := (temp_str[5] = 'v');
    Voltage_CH06.Active := (temp_str[6] = 'v');
    Voltage_CH07.Active := (temp_str[7] = 'v');
    Voltage_CH08.Active := (temp_str[8] = 'v');
    Voltage_CH09.Active := (temp_str[9] = 'v');
    Voltage_CH10.Active := (temp_str[10] = 'v');
    Voltage_CH11.Active := (temp_str[11] = 'v');
    Voltage_CH12.Active := (temp_str[12] = 'v');
    Voltage_CH13.Active := (temp_str[13] = 'v');
    Voltage_CH14.Active := (temp_str[14] = 'v');
    Voltage_CH15.Active := (temp_str[15] = 'v');
    Voltage_CH16.Active := (temp_str[16] = 'v');
    Voltage_CH17.Active := (temp_str[17] = 'v');
    Voltage_CH18.Active := (temp_str[18] = 'v');
    Voltage_CH19.Active := (temp_str[19] = 'v');
    Voltage_CH20.Active := (temp_str[20] = 'v');
    Voltage_CH21.Active := (temp_str[21] = 'v');
    Voltage_CH22.Active := (temp_str[22] = 'v');
    Voltage_CH23.Active := (temp_str[23] = 'v');
    Voltage_CH24.Active := (temp_str[24] = 'v');
    Voltage_CH25.Active := (temp_str[25] = 'v');
    Voltage_CH26.Active := (temp_str[26] = 'v');
    Voltage_CH27.Active := (temp_str[27] = 'v');
    Voltage_CH28.Active := (temp_str[28] = 'v');
    Voltage_CH29.Active := (temp_str[29] = 'v');
    Voltage_CH30.Active := (temp_str[30] = 'v');
    Voltage_CH31.Active := (temp_str[31] = 'v');
    Voltage_CH32.Active := (temp_str[32] = 'v');
    Voltage_CH33.Active := (temp_str[33] = 'v');
    Voltage_CH34.Active := (temp_str[34] = 'v');
    Voltage_CH35.Active := (temp_str[35] = 'v');
    Voltage_CH36.Active := (temp_str[36] = 'v');
    Voltage_CH37.Active := (temp_str[37] = 'v');
    Voltage_CH38.Active := (temp_str[38] = 'v');
    Voltage_CH39.Active := (temp_str[39] = 'v');
    Voltage_CH40.Active := (temp_str[40] = 'v');
    Voltage_CH41.Active := (temp_str[41] = 'v');
    Voltage_CH42.Active := (temp_str[42] = 'v');
    Voltage_CH43.Active := (temp_str[43] = 'v');
    Voltage_CH44.Active := (temp_str[44] = 'v');
    Voltage_CH45.Active := (temp_str[45] = 'v');
    Voltage_CH46.Active := (temp_str[46] = 'v');
    Voltage_CH47.Active := (temp_str[47] = 'v');
    Voltage_CH48.Active := (temp_str[48] = 'v');
    Voltage_CH49.Active := (temp_str[49] = 'v');
    Voltage_CH50.Active := (temp_str[50] = 'v');
    Voltage_CH51.Active := (temp_str[51] = 'v');
    Voltage_CH52.Active := (temp_str[52] = 'v');
    Voltage_CH53.Active := (temp_str[53] = 'v');
    Voltage_CH54.Active := (temp_str[54] = 'v');
    Voltage_CH55.Active := (temp_str[55] = 'v');
    Voltage_CH56.Active := (temp_str[56] = 'v');
    Voltage_CH57.Active := (temp_str[57] = 'v');
    Voltage_CH58.Active := (temp_str[58] = 'v');
    Voltage_CH59.Active := (temp_str[59] = 'v');
    Voltage_CH60.Active := (temp_str[60] = 'v');
    Voltage_CH61.Active := (temp_str[61] = 'v');
    Voltage_CH62.Active := (temp_str[62] = 'v');
    Voltage_CH63.Active := (temp_str[63] = 'v');
    Voltage_CH64.Active := (temp_str[64] = 'v');
    //--
    Current_CH01.Active := not Voltage_CH01.Active;
    Current_CH02.Active := not Voltage_CH02.Active;
    Current_CH03.Active := not Voltage_CH03.Active;
    Current_CH04.Active := not Voltage_CH04.Active;
    Current_CH05.Active := not Voltage_CH05.Active;
    Current_CH06.Active := not Voltage_CH06.Active;
    Current_CH07.Active := not Voltage_CH07.Active;
    Current_CH08.Active := not Voltage_CH08.Active;
    Current_CH09.Active := not Voltage_CH09.Active;
    Current_CH10.Active := not Voltage_CH10.Active;
    Current_CH11.Active := not Voltage_CH11.Active;
    Current_CH12.Active := not Voltage_CH12.Active;
    Current_CH13.Active := not Voltage_CH13.Active;
    Current_CH14.Active := not Voltage_CH14.Active;
    Current_CH15.Active := not Voltage_CH15.Active;
    Current_CH16.Active := not Voltage_CH16.Active;
    Current_CH17.Active := not Voltage_CH17.Active;
    Current_CH18.Active := not Voltage_CH18.Active;
    Current_CH19.Active := not Voltage_CH19.Active;
    Current_CH20.Active := not Voltage_CH20.Active;
    Current_CH21.Active := not Voltage_CH21.Active;
    Current_CH22.Active := not Voltage_CH22.Active;
    Current_CH23.Active := not Voltage_CH23.Active;
    Current_CH24.Active := not Voltage_CH24.Active;
    Current_CH25.Active := not Voltage_CH25.Active;
    Current_CH26.Active := not Voltage_CH26.Active;
    Current_CH27.Active := not Voltage_CH27.Active;
    Current_CH28.Active := not Voltage_CH28.Active;
    Current_CH29.Active := not Voltage_CH29.Active;
    Current_CH30.Active := not Voltage_CH30.Active;
    Current_CH31.Active := not Voltage_CH31.Active;
    Current_CH32.Active := not Voltage_CH32.Active;
    Current_CH33.Active := not Voltage_CH33.Active;
    Current_CH34.Active := not Voltage_CH34.Active;
    Current_CH35.Active := not Voltage_CH35.Active;
    Current_CH36.Active := not Voltage_CH36.Active;
    Current_CH37.Active := not Voltage_CH37.Active;
    Current_CH38.Active := not Voltage_CH38.Active;
    Current_CH39.Active := not Voltage_CH39.Active;
    Current_CH40.Active := not Voltage_CH40.Active;
    Current_CH41.Active := not Voltage_CH41.Active;
    Current_CH42.Active := not Voltage_CH42.Active;
    Current_CH43.Active := not Voltage_CH43.Active;
    Current_CH44.Active := not Voltage_CH44.Active;
    Current_CH45.Active := not Voltage_CH45.Active;
    Current_CH46.Active := not Voltage_CH46.Active;
    Current_CH47.Active := not Voltage_CH47.Active;
    Current_CH48.Active := not Voltage_CH48.Active;
    Current_CH49.Active := not Voltage_CH49.Active;
    Current_CH50.Active := not Voltage_CH50.Active;
    Current_CH51.Active := not Voltage_CH51.Active;
    Current_CH52.Active := not Voltage_CH52.Active;
    Current_CH53.Active := not Voltage_CH53.Active;
    Current_CH54.Active := not Voltage_CH54.Active;
    Current_CH55.Active := not Voltage_CH55.Active;
    Current_CH56.Active := not Voltage_CH56.Active;
    Current_CH57.Active := not Voltage_CH57.Active;
    Current_CH58.Active := not Voltage_CH58.Active;
    Current_CH59.Active := not Voltage_CH59.Active;
    Current_CH60.Active := not Voltage_CH60.Active;
    Current_CH61.Active := not Voltage_CH61.Active;
    Current_CH62.Active := not Voltage_CH62.Active;
    Current_CH63.Active := not Voltage_CH63.Active;
    Current_CH64.Active := not Voltage_CH64.Active;
    //--
  end;
  //--
  // 清空數據
  Voltage_PSU.Clear;
  Current_PSU.Clear;
  Voltage_CH01.Clear;
  Current_CH01.Clear;
  Voltage_CH02.Clear;
  Current_CH02.Clear;
  Voltage_CH03.Clear;
  Current_CH03.Clear;
  Voltage_CH04.Clear;
  Current_CH04.Clear;
  Voltage_CH05.Clear;
  Current_CH05.Clear;
  Voltage_CH06.Clear;
  Current_CH06.Clear;
  Voltage_CH07.Clear;
  Current_CH07.Clear;
  Voltage_CH08.Clear;
  Current_CH08.Clear;
  Voltage_CH09.Clear;
  Current_CH09.Clear;
  Voltage_CH10.Clear;
  Current_CH10.Clear;
  Voltage_CH11.Clear;
  Current_CH11.Clear;
  Voltage_CH12.Clear;
  Current_CH12.Clear;
  Voltage_CH13.Clear;
  Current_CH13.Clear;
  Voltage_CH14.Clear;
  Current_CH14.Clear;
  Voltage_CH15.Clear;
  Current_CH15.Clear;
  Voltage_CH16.Clear;
  Current_CH16.Clear;
  Voltage_CH17.Clear;
  Current_CH17.Clear;
  Voltage_CH18.Clear;
  Current_CH18.Clear;
  Voltage_CH19.Clear;
  Current_CH19.Clear;
  Voltage_CH20.Clear;
  Current_CH20.Clear;
  Voltage_CH21.Clear;
  Current_CH21.Clear;
  Voltage_CH22.Clear;
  Current_CH22.Clear;
  Voltage_CH23.Clear;
  Current_CH23.Clear;
  Voltage_CH24.Clear;
  Current_CH24.Clear;
  Voltage_CH25.Clear;
  Current_CH25.Clear;
  Voltage_CH26.Clear;
  Current_CH26.Clear;
  Voltage_CH27.Clear;
  Current_CH27.Clear;
  Voltage_CH28.Clear;
  Current_CH28.Clear;
  Voltage_CH29.Clear;
  Current_CH29.Clear;
  Voltage_CH30.Clear;
  Current_CH30.Clear;
  Voltage_CH31.Clear;
  Current_CH31.Clear;
  Voltage_CH32.Clear;
  Current_CH32.Clear;
  Voltage_CH33.Clear;
  Current_CH33.Clear;
  Voltage_CH34.Clear;
  Current_CH34.Clear;
  Voltage_CH35.Clear;
  Current_CH35.Clear;
  Voltage_CH36.Clear;
  Current_CH36.Clear;
  Voltage_CH37.Clear;
  Current_CH37.Clear;
  Voltage_CH38.Clear;
  Current_CH38.Clear;
  Voltage_CH39.Clear;
  Current_CH39.Clear;
  Voltage_CH40.Clear;
  Current_CH40.Clear;
  Voltage_CH41.Clear;
  Current_CH41.Clear;
  Voltage_CH42.Clear;
  Current_CH42.Clear;
  Voltage_CH43.Clear;
  Current_CH43.Clear;
  Voltage_CH44.Clear;
  Current_CH44.Clear;
  Voltage_CH45.Clear;
  Current_CH45.Clear;
  Voltage_CH46.Clear;
  Current_CH46.Clear;
  Voltage_CH47.Clear;
  Current_CH47.Clear;
  Voltage_CH48.Clear;
  Current_CH48.Clear;
  Voltage_CH49.Clear;
  Current_CH49.Clear;
  Voltage_CH50.Clear;
  Current_CH50.Clear;
  Voltage_CH51.Clear;
  Current_CH51.Clear;
  Voltage_CH52.Clear;
  Current_CH52.Clear;
  Voltage_CH53.Clear;
  Current_CH53.Clear;
  Voltage_CH54.Clear;
  Current_CH54.Clear;
  Voltage_CH55.Clear;
  Current_CH55.Clear;
  Voltage_CH56.Clear;
  Current_CH56.Clear;
  Voltage_CH57.Clear;
  Current_CH57.Clear;
  Voltage_CH58.Clear;
  Current_CH58.Clear;
  Voltage_CH59.Clear;
  Current_CH59.Clear;
  Voltage_CH60.Clear;
  Current_CH60.Clear;
  Voltage_CH61.Clear;
  Current_CH61.Clear;
  Voltage_CH62.Clear;
  Current_CH62.Clear;
  Voltage_CH63.Clear;
  Current_CH63.Clear;
  Voltage_CH64.Clear;
  Current_CH64.Clear;
  //--
  // 停止更新圖形
  Voltage_PSU.BeginUpdate;
  Current_PSU.BeginUpdate;
  Voltage_CH01.BeginUpdate;
  Current_CH01.BeginUpdate;
  Voltage_CH02.BeginUpdate;
  Current_CH02.BeginUpdate;
  Voltage_CH03.BeginUpdate;
  Current_CH03.BeginUpdate;
  Voltage_CH04.BeginUpdate;
  Current_CH04.BeginUpdate;
  Voltage_CH05.BeginUpdate;
  Current_CH05.BeginUpdate;
  Voltage_CH06.BeginUpdate;
  Current_CH06.BeginUpdate;
  Voltage_CH07.BeginUpdate;
  Current_CH07.BeginUpdate;
  Voltage_CH08.BeginUpdate;
  Current_CH08.BeginUpdate;
  Voltage_CH09.BeginUpdate;
  Current_CH09.BeginUpdate;
  Voltage_CH10.BeginUpdate;
  Current_CH10.BeginUpdate;
  Voltage_CH11.BeginUpdate;
  Current_CH11.BeginUpdate;
  Voltage_CH12.BeginUpdate;
  Current_CH12.BeginUpdate;
  Voltage_CH13.BeginUpdate;
  Current_CH13.BeginUpdate;
  Voltage_CH14.BeginUpdate;
  Current_CH14.BeginUpdate;
  Voltage_CH15.BeginUpdate;
  Current_CH15.BeginUpdate;
  Voltage_CH16.BeginUpdate;
  Current_CH16.BeginUpdate;
  Voltage_CH17.BeginUpdate;
  Current_CH17.BeginUpdate;
  Voltage_CH18.BeginUpdate;
  Current_CH18.BeginUpdate;
  Voltage_CH19.BeginUpdate;
  Current_CH19.BeginUpdate;
  Voltage_CH20.BeginUpdate;
  Current_CH20.BeginUpdate;
  Voltage_CH21.BeginUpdate;
  Current_CH21.BeginUpdate;
  Voltage_CH22.BeginUpdate;
  Current_CH22.BeginUpdate;
  Voltage_CH23.BeginUpdate;
  Current_CH23.BeginUpdate;
  Voltage_CH24.BeginUpdate;
  Current_CH24.BeginUpdate;
  Voltage_CH25.BeginUpdate;
  Current_CH25.BeginUpdate;
  Voltage_CH26.BeginUpdate;
  Current_CH26.BeginUpdate;
  Voltage_CH27.BeginUpdate;
  Current_CH27.BeginUpdate;
  Voltage_CH28.BeginUpdate;
  Current_CH28.BeginUpdate;
  Voltage_CH29.BeginUpdate;
  Current_CH29.BeginUpdate;
  Voltage_CH30.BeginUpdate;
  Current_CH30.BeginUpdate;
  Voltage_CH31.BeginUpdate;
  Current_CH31.BeginUpdate;
  Voltage_CH32.BeginUpdate;
  Current_CH32.BeginUpdate;
  Voltage_CH33.BeginUpdate;
  Current_CH33.BeginUpdate;
  Voltage_CH34.BeginUpdate;
  Current_CH34.BeginUpdate;
  Voltage_CH35.BeginUpdate;
  Current_CH35.BeginUpdate;
  Voltage_CH36.BeginUpdate;
  Current_CH36.BeginUpdate;
  Voltage_CH37.BeginUpdate;
  Current_CH37.BeginUpdate;
  Voltage_CH38.BeginUpdate;
  Current_CH38.BeginUpdate;
  Voltage_CH39.BeginUpdate;
  Current_CH39.BeginUpdate;
  Voltage_CH40.BeginUpdate;
  Current_CH40.BeginUpdate;
  Voltage_CH41.BeginUpdate;
  Current_CH41.BeginUpdate;
  Voltage_CH42.BeginUpdate;
  Current_CH42.BeginUpdate;
  Voltage_CH43.BeginUpdate;
  Current_CH43.BeginUpdate;
  Voltage_CH44.BeginUpdate;
  Current_CH44.BeginUpdate;
  Voltage_CH45.BeginUpdate;
  Current_CH45.BeginUpdate;
  Voltage_CH46.BeginUpdate;
  Current_CH46.BeginUpdate;
  Voltage_CH47.BeginUpdate;
  Current_CH47.BeginUpdate;
  Voltage_CH48.BeginUpdate;
  Current_CH48.BeginUpdate;
  Voltage_CH49.BeginUpdate;
  Current_CH49.BeginUpdate;
  Voltage_CH50.BeginUpdate;
  Current_CH50.BeginUpdate;
  Voltage_CH51.BeginUpdate;
  Current_CH51.BeginUpdate;
  Voltage_CH52.BeginUpdate;
  Current_CH52.BeginUpdate;
  Voltage_CH53.BeginUpdate;
  Current_CH53.BeginUpdate;
  Voltage_CH54.BeginUpdate;
  Current_CH54.BeginUpdate;
  Voltage_CH55.BeginUpdate;
  Current_CH55.BeginUpdate;
  Voltage_CH56.BeginUpdate;
  Current_CH56.BeginUpdate;
  Voltage_CH57.BeginUpdate;
  Current_CH57.BeginUpdate;
  Voltage_CH58.BeginUpdate;
  Current_CH58.BeginUpdate;
  Voltage_CH59.BeginUpdate;
  Current_CH59.BeginUpdate;
  Voltage_CH60.BeginUpdate;
  Current_CH60.BeginUpdate;
  Voltage_CH61.BeginUpdate;
  Current_CH61.BeginUpdate;
  Voltage_CH62.BeginUpdate;
  Current_CH62.BeginUpdate;
  Voltage_CH63.BeginUpdate;
  Current_CH63.BeginUpdate;
  Voltage_CH64.BeginUpdate;
  Current_CH64.BeginUpdate;
  //--
  // 更新圖形數據
  for temp_i := ( (StrToInt(StatusBar1.Panels[3].Text)-1)*30+1 ) to ( StrToInt(StatusBar1.Panels[3].Text)*30 ) do
  begin
    Voltage_PSU.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[2, temp_i+1], NaN));
    Current_PSU.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[3, temp_i+1], NaN));
    Voltage_CH01.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[6, temp_i+1], NaN));
    Current_CH01.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[7, temp_i+1], NaN));
    Voltage_CH02.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[8, temp_i+1], NaN));
    Current_CH02.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[9, temp_i+1], NaN));
    Voltage_CH03.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[10, temp_i+1], NaN));
    Current_CH03.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[11, temp_i+1], NaN));
    Voltage_CH04.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[12, temp_i+1], NaN));
    Current_CH04.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[13, temp_i+1], NaN));
    Voltage_CH05.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[14, temp_i+1], NaN));
    Current_CH05.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[15, temp_i+1], NaN));
    Voltage_CH06.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[16, temp_i+1], NaN));
    Current_CH06.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[17, temp_i+1], NaN));
    Voltage_CH07.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[18, temp_i+1], NaN));
    Current_CH07.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[19, temp_i+1], NaN));
    Voltage_CH08.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[20, temp_i+1], NaN));
    Current_CH08.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[21, temp_i+1], NaN));
    Voltage_CH09.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[22, temp_i+1], NaN));
    Current_CH09.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[23, temp_i+1], NaN));
    Voltage_CH10.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[24, temp_i+1], NaN));
    Current_CH10.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[25, temp_i+1], NaN));
    Voltage_CH11.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[26, temp_i+1], NaN));
    Current_CH11.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[27, temp_i+1], NaN));
    Voltage_CH12.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[28, temp_i+1], NaN));
    Current_CH12.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[29, temp_i+1], NaN));
    Voltage_CH13.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[30, temp_i+1], NaN));
    Current_CH13.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[31, temp_i+1], NaN));
    Voltage_CH14.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[32, temp_i+1], NaN));
    Current_CH14.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[33, temp_i+1], NaN));
    Voltage_CH15.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[34, temp_i+1], NaN));
    Current_CH15.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[35, temp_i+1], NaN));
    Voltage_CH16.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[36, temp_i+1], NaN));
    Current_CH16.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[37, temp_i+1], NaN));
    Voltage_CH17.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[38, temp_i+1], NaN));
    Current_CH17.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[39, temp_i+1], NaN));
    Voltage_CH18.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[40, temp_i+1], NaN));
    Current_CH18.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[41, temp_i+1], NaN));
    Voltage_CH19.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[42, temp_i+1], NaN));
    Current_CH19.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[43, temp_i+1], NaN));
    Voltage_CH20.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[44, temp_i+1], NaN));
    Current_CH20.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[45, temp_i+1], NaN));
    Voltage_CH21.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[46, temp_i+1], NaN));
    Current_CH21.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[47, temp_i+1], NaN));
    Voltage_CH22.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[48, temp_i+1], NaN));
    Current_CH22.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[49, temp_i+1], NaN));
    Voltage_CH23.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[50, temp_i+1], NaN));
    Current_CH23.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[51, temp_i+1], NaN));
    Voltage_CH24.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[52, temp_i+1], NaN));
    Current_CH24.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[53, temp_i+1], NaN));
    Voltage_CH25.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[54, temp_i+1], NaN));
    Current_CH25.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[55, temp_i+1], NaN));
    Voltage_CH26.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[56, temp_i+1], NaN));
    Current_CH26.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[57, temp_i+1], NaN));
    Voltage_CH27.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[58, temp_i+1], NaN));
    Current_CH27.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[59, temp_i+1], NaN));
    Voltage_CH28.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[60, temp_i+1], NaN));
    Current_CH28.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[61, temp_i+1], NaN));
    Voltage_CH29.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[62, temp_i+1], NaN));
    Current_CH29.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[63, temp_i+1], NaN));
    Voltage_CH30.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[64, temp_i+1], NaN));
    Current_CH30.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[65, temp_i+1], NaN));
    Voltage_CH31.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[66, temp_i+1], NaN));
    Current_CH31.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[67, temp_i+1], NaN));
    Voltage_CH32.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[68, temp_i+1], NaN));
    Current_CH32.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[69, temp_i+1], NaN));
    Voltage_CH33.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[70, temp_i+1], NaN));
    Current_CH33.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[71, temp_i+1], NaN));
    Voltage_CH34.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[72, temp_i+1], NaN));
    Current_CH34.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[73, temp_i+1], NaN));
    Voltage_CH35.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[74, temp_i+1], NaN));
    Current_CH35.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[75, temp_i+1], NaN));
    Voltage_CH36.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[76, temp_i+1], NaN));
    Current_CH36.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[77, temp_i+1], NaN));
    Voltage_CH37.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[78, temp_i+1], NaN));
    Current_CH37.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[79, temp_i+1], NaN));
    Voltage_CH38.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[80, temp_i+1], NaN));
    Current_CH38.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[81, temp_i+1], NaN));
    Voltage_CH39.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[82, temp_i+1], NaN));
    Current_CH39.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[83, temp_i+1], NaN));
    Voltage_CH40.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[84, temp_i+1], NaN));
    Current_CH40.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[85, temp_i+1], NaN));
    Voltage_CH41.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[86, temp_i+1], NaN));
    Current_CH41.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[87, temp_i+1], NaN));
    Voltage_CH42.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[88, temp_i+1], NaN));
    Current_CH42.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[89, temp_i+1], NaN));
    Voltage_CH43.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[90, temp_i+1], NaN));
    Current_CH43.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[91, temp_i+1], NaN));
    Voltage_CH44.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[92, temp_i+1], NaN));
    Current_CH44.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[93, temp_i+1], NaN));
    Voltage_CH45.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[94, temp_i+1], NaN));
    Current_CH45.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[95, temp_i+1], NaN));
    Voltage_CH46.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[96, temp_i+1], NaN));
    Current_CH46.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[97, temp_i+1], NaN));
    Voltage_CH47.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[98, temp_i+1], NaN));
    Current_CH47.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[99, temp_i+1], NaN));
    Voltage_CH48.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[100, temp_i+1], NaN));
    Current_CH48.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[101, temp_i+1], NaN));
    Voltage_CH49.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[102, temp_i+1], NaN));
    Current_CH49.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[103, temp_i+1], NaN));
    Voltage_CH50.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[104, temp_i+1], NaN));
    Current_CH50.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[105, temp_i+1], NaN));
    Voltage_CH51.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[106, temp_i+1], NaN));
    Current_CH51.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[107, temp_i+1], NaN));
    Voltage_CH52.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[108, temp_i+1], NaN));
    Current_CH52.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[109, temp_i+1], NaN));
    Voltage_CH53.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[110, temp_i+1], NaN));
    Current_CH53.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[111, temp_i+1], NaN));
    Voltage_CH54.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[112, temp_i+1], NaN));
    Current_CH54.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[113, temp_i+1], NaN));
    Voltage_CH55.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[114, temp_i+1], NaN));
    Current_CH55.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[115, temp_i+1], NaN));
    Voltage_CH56.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[116, temp_i+1], NaN));
    Current_CH56.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[117, temp_i+1], NaN));
    Voltage_CH57.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[118, temp_i+1], NaN));
    Current_CH57.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[119, temp_i+1], NaN));
    Voltage_CH58.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[120, temp_i+1], NaN));
    Current_CH58.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[121, temp_i+1], NaN));
    Voltage_CH59.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[122, temp_i+1], NaN));
    Current_CH59.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[123, temp_i+1], NaN));
    Voltage_CH60.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[124, temp_i+1], NaN));
    Current_CH60.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[125, temp_i+1], NaN));
    Voltage_CH61.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[126, temp_i+1], NaN));
    Current_CH61.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[127, temp_i+1], NaN));
    Voltage_CH62.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[128, temp_i+1], NaN));
    Current_CH62.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[129, temp_i+1], NaN));
    Voltage_CH63.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[130, temp_i+1], NaN));
    Current_CH63.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[131, temp_i+1], NaN));
    Voltage_CH64.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[132, temp_i+1], NaN));
    Current_CH64.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[133, temp_i+1], NaN));
  end;
  //--
  // 允許更新圖形
  Voltage_PSU.EndUpdate;
  Current_PSU.EndUpdate;
  Voltage_CH01.EndUpdate;
  Current_CH01.EndUpdate;
  Voltage_CH02.EndUpdate;
  Current_CH02.EndUpdate;
  Voltage_CH03.EndUpdate;
  Current_CH03.EndUpdate;
  Voltage_CH04.EndUpdate;
  Current_CH04.EndUpdate;
  Voltage_CH05.EndUpdate;
  Current_CH05.EndUpdate;
  Voltage_CH06.EndUpdate;
  Current_CH06.EndUpdate;
  Voltage_CH07.EndUpdate;
  Current_CH07.EndUpdate;
  Voltage_CH08.EndUpdate;
  Current_CH08.EndUpdate;
  Voltage_CH09.EndUpdate;
  Current_CH09.EndUpdate;
  Voltage_CH10.EndUpdate;
  Current_CH10.EndUpdate;
  Voltage_CH11.EndUpdate;
  Current_CH11.EndUpdate;
  Voltage_CH12.EndUpdate;
  Current_CH12.EndUpdate;
  Voltage_CH13.EndUpdate;
  Current_CH13.EndUpdate;
  Voltage_CH14.EndUpdate;
  Current_CH14.EndUpdate;
  Voltage_CH15.EndUpdate;
  Current_CH15.EndUpdate;
  Voltage_CH16.EndUpdate;
  Current_CH16.EndUpdate;
  Voltage_CH17.EndUpdate;
  Current_CH17.EndUpdate;
  Voltage_CH18.EndUpdate;
  Current_CH18.EndUpdate;
  Voltage_CH19.EndUpdate;
  Current_CH19.EndUpdate;
  Voltage_CH20.EndUpdate;
  Current_CH20.EndUpdate;
  Voltage_CH21.EndUpdate;
  Current_CH21.EndUpdate;
  Voltage_CH22.EndUpdate;
  Current_CH22.EndUpdate;
  Voltage_CH23.EndUpdate;
  Current_CH23.EndUpdate;
  Voltage_CH24.EndUpdate;
  Current_CH24.EndUpdate;
  Voltage_CH25.EndUpdate;
  Current_CH25.EndUpdate;
  Voltage_CH26.EndUpdate;
  Current_CH26.EndUpdate;
  Voltage_CH27.EndUpdate;
  Current_CH27.EndUpdate;
  Voltage_CH28.EndUpdate;
  Current_CH28.EndUpdate;
  Voltage_CH29.EndUpdate;
  Current_CH29.EndUpdate;
  Voltage_CH30.EndUpdate;
  Current_CH30.EndUpdate;
  Voltage_CH31.EndUpdate;
  Current_CH31.EndUpdate;
  Voltage_CH32.EndUpdate;
  Current_CH32.EndUpdate;
  Voltage_CH33.EndUpdate;
  Current_CH33.EndUpdate;
  Voltage_CH34.EndUpdate;
  Current_CH34.EndUpdate;
  Voltage_CH35.EndUpdate;
  Current_CH35.EndUpdate;
  Voltage_CH36.EndUpdate;
  Current_CH36.EndUpdate;
  Voltage_CH37.EndUpdate;
  Current_CH37.EndUpdate;
  Voltage_CH38.EndUpdate;
  Current_CH38.EndUpdate;
  Voltage_CH39.EndUpdate;
  Current_CH39.EndUpdate;
  Voltage_CH40.EndUpdate;
  Current_CH40.EndUpdate;
  Voltage_CH41.EndUpdate;
  Current_CH41.EndUpdate;
  Voltage_CH42.EndUpdate;
  Current_CH42.EndUpdate;
  Voltage_CH43.EndUpdate;
  Current_CH43.EndUpdate;
  Voltage_CH44.EndUpdate;
  Current_CH44.EndUpdate;
  Voltage_CH45.EndUpdate;
  Current_CH45.EndUpdate;
  Voltage_CH46.EndUpdate;
  Current_CH46.EndUpdate;
  Voltage_CH47.EndUpdate;
  Current_CH47.EndUpdate;
  Voltage_CH48.EndUpdate;
  Current_CH48.EndUpdate;
  Voltage_CH49.EndUpdate;
  Current_CH49.EndUpdate;
  Voltage_CH50.EndUpdate;
  Current_CH50.EndUpdate;
  Voltage_CH51.EndUpdate;
  Current_CH51.EndUpdate;
  Voltage_CH52.EndUpdate;
  Current_CH52.EndUpdate;
  Voltage_CH53.EndUpdate;
  Current_CH53.EndUpdate;
  Voltage_CH54.EndUpdate;
  Current_CH54.EndUpdate;
  Voltage_CH55.EndUpdate;
  Current_CH55.EndUpdate;
  Voltage_CH56.EndUpdate;
  Current_CH56.EndUpdate;
  Voltage_CH57.EndUpdate;
  Current_CH57.EndUpdate;
  Voltage_CH58.EndUpdate;
  Current_CH58.EndUpdate;
  Voltage_CH59.EndUpdate;
  Current_CH59.EndUpdate;
  Voltage_CH60.EndUpdate;
  Current_CH60.EndUpdate;
  Voltage_CH61.EndUpdate;
  Current_CH61.EndUpdate;
  Voltage_CH62.EndUpdate;
  Current_CH62.EndUpdate;
  Voltage_CH63.EndUpdate;
  Current_CH63.EndUpdate;
  Voltage_CH64.EndUpdate;
  Current_CH64.EndUpdate;
  //--
end;  
```
6. **設定「NextPart_ToolButton」的「Event」頁面下「OnClick」為如下程式碼。**
```pascal
procedure TForm1.NextPart_ToolButtonClick(Sender: TObject);
var
  temp_i: Integer;
  temp_str: String;
begin
  // 沒有資料時不動作
  if v299_csv_StringGrid.Cells[1, 1] <> 'Time[ms](R2MS_Lite_v299csv_v20240510a)' then
  begin
    Exit;
  end;
  // 狀態列資訊
  if (StrToIntDef(StatusBar1.Panels[3].Text, 1) +1) > StrToInt(StatusBar1.Panels[4].Text) Then
  begin
    StatusBar1.Panels[3].Text := StatusBar1.Panels[4].Text;
    StatusBar1.Panels[1].Text := '第' + StatusBar1.Panels[3].Text + '次放電';
  end
  else
  begin
    StatusBar1.Panels[3].Text := IntToStr(StrToIntDef(StatusBar1.Panels[3].Text, 1)+1);
    StatusBar1.Panels[1].Text := '第' + StatusBar1.Panels[3].Text + '次放電';
  end;
  StatusBar1.Panels[0].Text := '#' + v299_csv_StringGrid.Cells[0, ( (StrToInt(StatusBar1.Panels[3].Text)-1)*30+1 )+1] + ' ~ #' + v299_csv_StringGrid.Cells[0, ( StrToInt(StatusBar1.Panels[3].Text)*30 )+1];
  // 取出放電狀態
  temp_str := Trim( v299_csv_StringGrid.Cells[5, ( (StrToInt(StatusBar1.Panels[3].Text)-1)*30+2 )] );
  RunningLog_Memo.Lines.Add( temp_str );
  // 依照放電狀態調整元件可見
  if Length(temp_str) = 64 Then
  begin
    //--
    Voltage_PSU.Active := False;
    Current_PSU.Active := False;
    Voltage_CH01.Active := (temp_str[1] = 'v');
    Voltage_CH02.Active := (temp_str[2] = 'v');
    Voltage_CH03.Active := (temp_str[3] = 'v');
    Voltage_CH04.Active := (temp_str[4] = 'v');
    Voltage_CH05.Active := (temp_str[5] = 'v');
    Voltage_CH06.Active := (temp_str[6] = 'v');
    Voltage_CH07.Active := (temp_str[7] = 'v');
    Voltage_CH08.Active := (temp_str[8] = 'v');
    Voltage_CH09.Active := (temp_str[9] = 'v');
    Voltage_CH10.Active := (temp_str[10] = 'v');
    Voltage_CH11.Active := (temp_str[11] = 'v');
    Voltage_CH12.Active := (temp_str[12] = 'v');
    Voltage_CH13.Active := (temp_str[13] = 'v');
    Voltage_CH14.Active := (temp_str[14] = 'v');
    Voltage_CH15.Active := (temp_str[15] = 'v');
    Voltage_CH16.Active := (temp_str[16] = 'v');
    Voltage_CH17.Active := (temp_str[17] = 'v');
    Voltage_CH18.Active := (temp_str[18] = 'v');
    Voltage_CH19.Active := (temp_str[19] = 'v');
    Voltage_CH20.Active := (temp_str[20] = 'v');
    Voltage_CH21.Active := (temp_str[21] = 'v');
    Voltage_CH22.Active := (temp_str[22] = 'v');
    Voltage_CH23.Active := (temp_str[23] = 'v');
    Voltage_CH24.Active := (temp_str[24] = 'v');
    Voltage_CH25.Active := (temp_str[25] = 'v');
    Voltage_CH26.Active := (temp_str[26] = 'v');
    Voltage_CH27.Active := (temp_str[27] = 'v');
    Voltage_CH28.Active := (temp_str[28] = 'v');
    Voltage_CH29.Active := (temp_str[29] = 'v');
    Voltage_CH30.Active := (temp_str[30] = 'v');
    Voltage_CH31.Active := (temp_str[31] = 'v');
    Voltage_CH32.Active := (temp_str[32] = 'v');
    Voltage_CH33.Active := (temp_str[33] = 'v');
    Voltage_CH34.Active := (temp_str[34] = 'v');
    Voltage_CH35.Active := (temp_str[35] = 'v');
    Voltage_CH36.Active := (temp_str[36] = 'v');
    Voltage_CH37.Active := (temp_str[37] = 'v');
    Voltage_CH38.Active := (temp_str[38] = 'v');
    Voltage_CH39.Active := (temp_str[39] = 'v');
    Voltage_CH40.Active := (temp_str[40] = 'v');
    Voltage_CH41.Active := (temp_str[41] = 'v');
    Voltage_CH42.Active := (temp_str[42] = 'v');
    Voltage_CH43.Active := (temp_str[43] = 'v');
    Voltage_CH44.Active := (temp_str[44] = 'v');
    Voltage_CH45.Active := (temp_str[45] = 'v');
    Voltage_CH46.Active := (temp_str[46] = 'v');
    Voltage_CH47.Active := (temp_str[47] = 'v');
    Voltage_CH48.Active := (temp_str[48] = 'v');
    Voltage_CH49.Active := (temp_str[49] = 'v');
    Voltage_CH50.Active := (temp_str[50] = 'v');
    Voltage_CH51.Active := (temp_str[51] = 'v');
    Voltage_CH52.Active := (temp_str[52] = 'v');
    Voltage_CH53.Active := (temp_str[53] = 'v');
    Voltage_CH54.Active := (temp_str[54] = 'v');
    Voltage_CH55.Active := (temp_str[55] = 'v');
    Voltage_CH56.Active := (temp_str[56] = 'v');
    Voltage_CH57.Active := (temp_str[57] = 'v');
    Voltage_CH58.Active := (temp_str[58] = 'v');
    Voltage_CH59.Active := (temp_str[59] = 'v');
    Voltage_CH60.Active := (temp_str[60] = 'v');
    Voltage_CH61.Active := (temp_str[61] = 'v');
    Voltage_CH62.Active := (temp_str[62] = 'v');
    Voltage_CH63.Active := (temp_str[63] = 'v');
    Voltage_CH64.Active := (temp_str[64] = 'v');
    //--
    Current_CH01.Active := not Voltage_CH01.Active;
    Current_CH02.Active := not Voltage_CH02.Active;
    Current_CH03.Active := not Voltage_CH03.Active;
    Current_CH04.Active := not Voltage_CH04.Active;
    Current_CH05.Active := not Voltage_CH05.Active;
    Current_CH06.Active := not Voltage_CH06.Active;
    Current_CH07.Active := not Voltage_CH07.Active;
    Current_CH08.Active := not Voltage_CH08.Active;
    Current_CH09.Active := not Voltage_CH09.Active;
    Current_CH10.Active := not Voltage_CH10.Active;
    Current_CH11.Active := not Voltage_CH11.Active;
    Current_CH12.Active := not Voltage_CH12.Active;
    Current_CH13.Active := not Voltage_CH13.Active;
    Current_CH14.Active := not Voltage_CH14.Active;
    Current_CH15.Active := not Voltage_CH15.Active;
    Current_CH16.Active := not Voltage_CH16.Active;
    Current_CH17.Active := not Voltage_CH17.Active;
    Current_CH18.Active := not Voltage_CH18.Active;
    Current_CH19.Active := not Voltage_CH19.Active;
    Current_CH20.Active := not Voltage_CH20.Active;
    Current_CH21.Active := not Voltage_CH21.Active;
    Current_CH22.Active := not Voltage_CH22.Active;
    Current_CH23.Active := not Voltage_CH23.Active;
    Current_CH24.Active := not Voltage_CH24.Active;
    Current_CH25.Active := not Voltage_CH25.Active;
    Current_CH26.Active := not Voltage_CH26.Active;
    Current_CH27.Active := not Voltage_CH27.Active;
    Current_CH28.Active := not Voltage_CH28.Active;
    Current_CH29.Active := not Voltage_CH29.Active;
    Current_CH30.Active := not Voltage_CH30.Active;
    Current_CH31.Active := not Voltage_CH31.Active;
    Current_CH32.Active := not Voltage_CH32.Active;
    Current_CH33.Active := not Voltage_CH33.Active;
    Current_CH34.Active := not Voltage_CH34.Active;
    Current_CH35.Active := not Voltage_CH35.Active;
    Current_CH36.Active := not Voltage_CH36.Active;
    Current_CH37.Active := not Voltage_CH37.Active;
    Current_CH38.Active := not Voltage_CH38.Active;
    Current_CH39.Active := not Voltage_CH39.Active;
    Current_CH40.Active := not Voltage_CH40.Active;
    Current_CH41.Active := not Voltage_CH41.Active;
    Current_CH42.Active := not Voltage_CH42.Active;
    Current_CH43.Active := not Voltage_CH43.Active;
    Current_CH44.Active := not Voltage_CH44.Active;
    Current_CH45.Active := not Voltage_CH45.Active;
    Current_CH46.Active := not Voltage_CH46.Active;
    Current_CH47.Active := not Voltage_CH47.Active;
    Current_CH48.Active := not Voltage_CH48.Active;
    Current_CH49.Active := not Voltage_CH49.Active;
    Current_CH50.Active := not Voltage_CH50.Active;
    Current_CH51.Active := not Voltage_CH51.Active;
    Current_CH52.Active := not Voltage_CH52.Active;
    Current_CH53.Active := not Voltage_CH53.Active;
    Current_CH54.Active := not Voltage_CH54.Active;
    Current_CH55.Active := not Voltage_CH55.Active;
    Current_CH56.Active := not Voltage_CH56.Active;
    Current_CH57.Active := not Voltage_CH57.Active;
    Current_CH58.Active := not Voltage_CH58.Active;
    Current_CH59.Active := not Voltage_CH59.Active;
    Current_CH60.Active := not Voltage_CH60.Active;
    Current_CH61.Active := not Voltage_CH61.Active;
    Current_CH62.Active := not Voltage_CH62.Active;
    Current_CH63.Active := not Voltage_CH63.Active;
    Current_CH64.Active := not Voltage_CH64.Active;
    //--
  end;
  //--
  // 清空數據
  Voltage_PSU.Clear;
  Current_PSU.Clear;
  Voltage_CH01.Clear;
  Current_CH01.Clear;
  Voltage_CH02.Clear;
  Current_CH02.Clear;
  Voltage_CH03.Clear;
  Current_CH03.Clear;
  Voltage_CH04.Clear;
  Current_CH04.Clear;
  Voltage_CH05.Clear;
  Current_CH05.Clear;
  Voltage_CH06.Clear;
  Current_CH06.Clear;
  Voltage_CH07.Clear;
  Current_CH07.Clear;
  Voltage_CH08.Clear;
  Current_CH08.Clear;
  Voltage_CH09.Clear;
  Current_CH09.Clear;
  Voltage_CH10.Clear;
  Current_CH10.Clear;
  Voltage_CH11.Clear;
  Current_CH11.Clear;
  Voltage_CH12.Clear;
  Current_CH12.Clear;
  Voltage_CH13.Clear;
  Current_CH13.Clear;
  Voltage_CH14.Clear;
  Current_CH14.Clear;
  Voltage_CH15.Clear;
  Current_CH15.Clear;
  Voltage_CH16.Clear;
  Current_CH16.Clear;
  Voltage_CH17.Clear;
  Current_CH17.Clear;
  Voltage_CH18.Clear;
  Current_CH18.Clear;
  Voltage_CH19.Clear;
  Current_CH19.Clear;
  Voltage_CH20.Clear;
  Current_CH20.Clear;
  Voltage_CH21.Clear;
  Current_CH21.Clear;
  Voltage_CH22.Clear;
  Current_CH22.Clear;
  Voltage_CH23.Clear;
  Current_CH23.Clear;
  Voltage_CH24.Clear;
  Current_CH24.Clear;
  Voltage_CH25.Clear;
  Current_CH25.Clear;
  Voltage_CH26.Clear;
  Current_CH26.Clear;
  Voltage_CH27.Clear;
  Current_CH27.Clear;
  Voltage_CH28.Clear;
  Current_CH28.Clear;
  Voltage_CH29.Clear;
  Current_CH29.Clear;
  Voltage_CH30.Clear;
  Current_CH30.Clear;
  Voltage_CH31.Clear;
  Current_CH31.Clear;
  Voltage_CH32.Clear;
  Current_CH32.Clear;
  Voltage_CH33.Clear;
  Current_CH33.Clear;
  Voltage_CH34.Clear;
  Current_CH34.Clear;
  Voltage_CH35.Clear;
  Current_CH35.Clear;
  Voltage_CH36.Clear;
  Current_CH36.Clear;
  Voltage_CH37.Clear;
  Current_CH37.Clear;
  Voltage_CH38.Clear;
  Current_CH38.Clear;
  Voltage_CH39.Clear;
  Current_CH39.Clear;
  Voltage_CH40.Clear;
  Current_CH40.Clear;
  Voltage_CH41.Clear;
  Current_CH41.Clear;
  Voltage_CH42.Clear;
  Current_CH42.Clear;
  Voltage_CH43.Clear;
  Current_CH43.Clear;
  Voltage_CH44.Clear;
  Current_CH44.Clear;
  Voltage_CH45.Clear;
  Current_CH45.Clear;
  Voltage_CH46.Clear;
  Current_CH46.Clear;
  Voltage_CH47.Clear;
  Current_CH47.Clear;
  Voltage_CH48.Clear;
  Current_CH48.Clear;
  Voltage_CH49.Clear;
  Current_CH49.Clear;
  Voltage_CH50.Clear;
  Current_CH50.Clear;
  Voltage_CH51.Clear;
  Current_CH51.Clear;
  Voltage_CH52.Clear;
  Current_CH52.Clear;
  Voltage_CH53.Clear;
  Current_CH53.Clear;
  Voltage_CH54.Clear;
  Current_CH54.Clear;
  Voltage_CH55.Clear;
  Current_CH55.Clear;
  Voltage_CH56.Clear;
  Current_CH56.Clear;
  Voltage_CH57.Clear;
  Current_CH57.Clear;
  Voltage_CH58.Clear;
  Current_CH58.Clear;
  Voltage_CH59.Clear;
  Current_CH59.Clear;
  Voltage_CH60.Clear;
  Current_CH60.Clear;
  Voltage_CH61.Clear;
  Current_CH61.Clear;
  Voltage_CH62.Clear;
  Current_CH62.Clear;
  Voltage_CH63.Clear;
  Current_CH63.Clear;
  Voltage_CH64.Clear;
  Current_CH64.Clear;
  //--
  // 停止更新圖形
  Voltage_PSU.BeginUpdate;
  Current_PSU.BeginUpdate;
  Voltage_CH01.BeginUpdate;
  Current_CH01.BeginUpdate;
  Voltage_CH02.BeginUpdate;
  Current_CH02.BeginUpdate;
  Voltage_CH03.BeginUpdate;
  Current_CH03.BeginUpdate;
  Voltage_CH04.BeginUpdate;
  Current_CH04.BeginUpdate;
  Voltage_CH05.BeginUpdate;
  Current_CH05.BeginUpdate;
  Voltage_CH06.BeginUpdate;
  Current_CH06.BeginUpdate;
  Voltage_CH07.BeginUpdate;
  Current_CH07.BeginUpdate;
  Voltage_CH08.BeginUpdate;
  Current_CH08.BeginUpdate;
  Voltage_CH09.BeginUpdate;
  Current_CH09.BeginUpdate;
  Voltage_CH10.BeginUpdate;
  Current_CH10.BeginUpdate;
  Voltage_CH11.BeginUpdate;
  Current_CH11.BeginUpdate;
  Voltage_CH12.BeginUpdate;
  Current_CH12.BeginUpdate;
  Voltage_CH13.BeginUpdate;
  Current_CH13.BeginUpdate;
  Voltage_CH14.BeginUpdate;
  Current_CH14.BeginUpdate;
  Voltage_CH15.BeginUpdate;
  Current_CH15.BeginUpdate;
  Voltage_CH16.BeginUpdate;
  Current_CH16.BeginUpdate;
  Voltage_CH17.BeginUpdate;
  Current_CH17.BeginUpdate;
  Voltage_CH18.BeginUpdate;
  Current_CH18.BeginUpdate;
  Voltage_CH19.BeginUpdate;
  Current_CH19.BeginUpdate;
  Voltage_CH20.BeginUpdate;
  Current_CH20.BeginUpdate;
  Voltage_CH21.BeginUpdate;
  Current_CH21.BeginUpdate;
  Voltage_CH22.BeginUpdate;
  Current_CH22.BeginUpdate;
  Voltage_CH23.BeginUpdate;
  Current_CH23.BeginUpdate;
  Voltage_CH24.BeginUpdate;
  Current_CH24.BeginUpdate;
  Voltage_CH25.BeginUpdate;
  Current_CH25.BeginUpdate;
  Voltage_CH26.BeginUpdate;
  Current_CH26.BeginUpdate;
  Voltage_CH27.BeginUpdate;
  Current_CH27.BeginUpdate;
  Voltage_CH28.BeginUpdate;
  Current_CH28.BeginUpdate;
  Voltage_CH29.BeginUpdate;
  Current_CH29.BeginUpdate;
  Voltage_CH30.BeginUpdate;
  Current_CH30.BeginUpdate;
  Voltage_CH31.BeginUpdate;
  Current_CH31.BeginUpdate;
  Voltage_CH32.BeginUpdate;
  Current_CH32.BeginUpdate;
  Voltage_CH33.BeginUpdate;
  Current_CH33.BeginUpdate;
  Voltage_CH34.BeginUpdate;
  Current_CH34.BeginUpdate;
  Voltage_CH35.BeginUpdate;
  Current_CH35.BeginUpdate;
  Voltage_CH36.BeginUpdate;
  Current_CH36.BeginUpdate;
  Voltage_CH37.BeginUpdate;
  Current_CH37.BeginUpdate;
  Voltage_CH38.BeginUpdate;
  Current_CH38.BeginUpdate;
  Voltage_CH39.BeginUpdate;
  Current_CH39.BeginUpdate;
  Voltage_CH40.BeginUpdate;
  Current_CH40.BeginUpdate;
  Voltage_CH41.BeginUpdate;
  Current_CH41.BeginUpdate;
  Voltage_CH42.BeginUpdate;
  Current_CH42.BeginUpdate;
  Voltage_CH43.BeginUpdate;
  Current_CH43.BeginUpdate;
  Voltage_CH44.BeginUpdate;
  Current_CH44.BeginUpdate;
  Voltage_CH45.BeginUpdate;
  Current_CH45.BeginUpdate;
  Voltage_CH46.BeginUpdate;
  Current_CH46.BeginUpdate;
  Voltage_CH47.BeginUpdate;
  Current_CH47.BeginUpdate;
  Voltage_CH48.BeginUpdate;
  Current_CH48.BeginUpdate;
  Voltage_CH49.BeginUpdate;
  Current_CH49.BeginUpdate;
  Voltage_CH50.BeginUpdate;
  Current_CH50.BeginUpdate;
  Voltage_CH51.BeginUpdate;
  Current_CH51.BeginUpdate;
  Voltage_CH52.BeginUpdate;
  Current_CH52.BeginUpdate;
  Voltage_CH53.BeginUpdate;
  Current_CH53.BeginUpdate;
  Voltage_CH54.BeginUpdate;
  Current_CH54.BeginUpdate;
  Voltage_CH55.BeginUpdate;
  Current_CH55.BeginUpdate;
  Voltage_CH56.BeginUpdate;
  Current_CH56.BeginUpdate;
  Voltage_CH57.BeginUpdate;
  Current_CH57.BeginUpdate;
  Voltage_CH58.BeginUpdate;
  Current_CH58.BeginUpdate;
  Voltage_CH59.BeginUpdate;
  Current_CH59.BeginUpdate;
  Voltage_CH60.BeginUpdate;
  Current_CH60.BeginUpdate;
  Voltage_CH61.BeginUpdate;
  Current_CH61.BeginUpdate;
  Voltage_CH62.BeginUpdate;
  Current_CH62.BeginUpdate;
  Voltage_CH63.BeginUpdate;
  Current_CH63.BeginUpdate;
  Voltage_CH64.BeginUpdate;
  Current_CH64.BeginUpdate;
  //--
  // 更新圖形數據
  for temp_i := ( (StrToInt(StatusBar1.Panels[3].Text)-1)*30+1 ) to ( StrToInt(StatusBar1.Panels[3].Text)*30 ) do
  begin
    Voltage_PSU.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[2, temp_i+1], NaN));
    Current_PSU.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[3, temp_i+1], NaN));
    Voltage_CH01.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[6, temp_i+1], NaN));
    Current_CH01.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[7, temp_i+1], NaN));
    Voltage_CH02.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[8, temp_i+1], NaN));
    Current_CH02.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[9, temp_i+1], NaN));
    Voltage_CH03.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[10, temp_i+1], NaN));
    Current_CH03.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[11, temp_i+1], NaN));
    Voltage_CH04.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[12, temp_i+1], NaN));
    Current_CH04.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[13, temp_i+1], NaN));
    Voltage_CH05.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[14, temp_i+1], NaN));
    Current_CH05.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[15, temp_i+1], NaN));
    Voltage_CH06.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[16, temp_i+1], NaN));
    Current_CH06.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[17, temp_i+1], NaN));
    Voltage_CH07.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[18, temp_i+1], NaN));
    Current_CH07.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[19, temp_i+1], NaN));
    Voltage_CH08.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[20, temp_i+1], NaN));
    Current_CH08.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[21, temp_i+1], NaN));
    Voltage_CH09.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[22, temp_i+1], NaN));
    Current_CH09.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[23, temp_i+1], NaN));
    Voltage_CH10.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[24, temp_i+1], NaN));
    Current_CH10.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[25, temp_i+1], NaN));
    Voltage_CH11.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[26, temp_i+1], NaN));
    Current_CH11.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[27, temp_i+1], NaN));
    Voltage_CH12.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[28, temp_i+1], NaN));
    Current_CH12.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[29, temp_i+1], NaN));
    Voltage_CH13.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[30, temp_i+1], NaN));
    Current_CH13.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[31, temp_i+1], NaN));
    Voltage_CH14.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[32, temp_i+1], NaN));
    Current_CH14.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[33, temp_i+1], NaN));
    Voltage_CH15.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[34, temp_i+1], NaN));
    Current_CH15.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[35, temp_i+1], NaN));
    Voltage_CH16.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[36, temp_i+1], NaN));
    Current_CH16.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[37, temp_i+1], NaN));
    Voltage_CH17.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[38, temp_i+1], NaN));
    Current_CH17.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[39, temp_i+1], NaN));
    Voltage_CH18.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[40, temp_i+1], NaN));
    Current_CH18.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[41, temp_i+1], NaN));
    Voltage_CH19.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[42, temp_i+1], NaN));
    Current_CH19.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[43, temp_i+1], NaN));
    Voltage_CH20.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[44, temp_i+1], NaN));
    Current_CH20.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[45, temp_i+1], NaN));
    Voltage_CH21.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[46, temp_i+1], NaN));
    Current_CH21.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[47, temp_i+1], NaN));
    Voltage_CH22.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[48, temp_i+1], NaN));
    Current_CH22.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[49, temp_i+1], NaN));
    Voltage_CH23.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[50, temp_i+1], NaN));
    Current_CH23.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[51, temp_i+1], NaN));
    Voltage_CH24.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[52, temp_i+1], NaN));
    Current_CH24.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[53, temp_i+1], NaN));
    Voltage_CH25.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[54, temp_i+1], NaN));
    Current_CH25.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[55, temp_i+1], NaN));
    Voltage_CH26.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[56, temp_i+1], NaN));
    Current_CH26.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[57, temp_i+1], NaN));
    Voltage_CH27.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[58, temp_i+1], NaN));
    Current_CH27.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[59, temp_i+1], NaN));
    Voltage_CH28.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[60, temp_i+1], NaN));
    Current_CH28.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[61, temp_i+1], NaN));
    Voltage_CH29.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[62, temp_i+1], NaN));
    Current_CH29.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[63, temp_i+1], NaN));
    Voltage_CH30.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[64, temp_i+1], NaN));
    Current_CH30.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[65, temp_i+1], NaN));
    Voltage_CH31.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[66, temp_i+1], NaN));
    Current_CH31.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[67, temp_i+1], NaN));
    Voltage_CH32.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[68, temp_i+1], NaN));
    Current_CH32.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[69, temp_i+1], NaN));
    Voltage_CH33.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[70, temp_i+1], NaN));
    Current_CH33.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[71, temp_i+1], NaN));
    Voltage_CH34.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[72, temp_i+1], NaN));
    Current_CH34.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[73, temp_i+1], NaN));
    Voltage_CH35.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[74, temp_i+1], NaN));
    Current_CH35.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[75, temp_i+1], NaN));
    Voltage_CH36.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[76, temp_i+1], NaN));
    Current_CH36.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[77, temp_i+1], NaN));
    Voltage_CH37.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[78, temp_i+1], NaN));
    Current_CH37.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[79, temp_i+1], NaN));
    Voltage_CH38.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[80, temp_i+1], NaN));
    Current_CH38.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[81, temp_i+1], NaN));
    Voltage_CH39.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[82, temp_i+1], NaN));
    Current_CH39.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[83, temp_i+1], NaN));
    Voltage_CH40.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[84, temp_i+1], NaN));
    Current_CH40.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[85, temp_i+1], NaN));
    Voltage_CH41.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[86, temp_i+1], NaN));
    Current_CH41.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[87, temp_i+1], NaN));
    Voltage_CH42.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[88, temp_i+1], NaN));
    Current_CH42.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[89, temp_i+1], NaN));
    Voltage_CH43.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[90, temp_i+1], NaN));
    Current_CH43.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[91, temp_i+1], NaN));
    Voltage_CH44.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[92, temp_i+1], NaN));
    Current_CH44.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[93, temp_i+1], NaN));
    Voltage_CH45.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[94, temp_i+1], NaN));
    Current_CH45.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[95, temp_i+1], NaN));
    Voltage_CH46.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[96, temp_i+1], NaN));
    Current_CH46.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[97, temp_i+1], NaN));
    Voltage_CH47.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[98, temp_i+1], NaN));
    Current_CH47.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[99, temp_i+1], NaN));
    Voltage_CH48.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[100, temp_i+1], NaN));
    Current_CH48.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[101, temp_i+1], NaN));
    Voltage_CH49.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[102, temp_i+1], NaN));
    Current_CH49.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[103, temp_i+1], NaN));
    Voltage_CH50.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[104, temp_i+1], NaN));
    Current_CH50.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[105, temp_i+1], NaN));
    Voltage_CH51.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[106, temp_i+1], NaN));
    Current_CH51.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[107, temp_i+1], NaN));
    Voltage_CH52.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[108, temp_i+1], NaN));
    Current_CH52.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[109, temp_i+1], NaN));
    Voltage_CH53.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[110, temp_i+1], NaN));
    Current_CH53.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[111, temp_i+1], NaN));
    Voltage_CH54.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[112, temp_i+1], NaN));
    Current_CH54.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[113, temp_i+1], NaN));
    Voltage_CH55.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[114, temp_i+1], NaN));
    Current_CH55.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[115, temp_i+1], NaN));
    Voltage_CH56.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[116, temp_i+1], NaN));
    Current_CH56.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[117, temp_i+1], NaN));
    Voltage_CH57.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[118, temp_i+1], NaN));
    Current_CH57.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[119, temp_i+1], NaN));
    Voltage_CH58.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[120, temp_i+1], NaN));
    Current_CH58.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[121, temp_i+1], NaN));
    Voltage_CH59.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[122, temp_i+1], NaN));
    Current_CH59.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[123, temp_i+1], NaN));
    Voltage_CH60.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[124, temp_i+1], NaN));
    Current_CH60.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[125, temp_i+1], NaN));
    Voltage_CH61.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[126, temp_i+1], NaN));
    Current_CH61.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[127, temp_i+1], NaN));
    Voltage_CH62.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[128, temp_i+1], NaN));
    Current_CH62.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[129, temp_i+1], NaN));
    Voltage_CH63.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[130, temp_i+1], NaN));
    Current_CH63.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[131, temp_i+1], NaN));
    Voltage_CH64.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[132, temp_i+1], NaN));
    Current_CH64.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[133, temp_i+1], NaN));
  end;
  //--
  // 允許更新圖形
  Voltage_PSU.EndUpdate;
  Current_PSU.EndUpdate;
  Voltage_CH01.EndUpdate;
  Current_CH01.EndUpdate;
  Voltage_CH02.EndUpdate;
  Current_CH02.EndUpdate;
  Voltage_CH03.EndUpdate;
  Current_CH03.EndUpdate;
  Voltage_CH04.EndUpdate;
  Current_CH04.EndUpdate;
  Voltage_CH05.EndUpdate;
  Current_CH05.EndUpdate;
  Voltage_CH06.EndUpdate;
  Current_CH06.EndUpdate;
  Voltage_CH07.EndUpdate;
  Current_CH07.EndUpdate;
  Voltage_CH08.EndUpdate;
  Current_CH08.EndUpdate;
  Voltage_CH09.EndUpdate;
  Current_CH09.EndUpdate;
  Voltage_CH10.EndUpdate;
  Current_CH10.EndUpdate;
  Voltage_CH11.EndUpdate;
  Current_CH11.EndUpdate;
  Voltage_CH12.EndUpdate;
  Current_CH12.EndUpdate;
  Voltage_CH13.EndUpdate;
  Current_CH13.EndUpdate;
  Voltage_CH14.EndUpdate;
  Current_CH14.EndUpdate;
  Voltage_CH15.EndUpdate;
  Current_CH15.EndUpdate;
  Voltage_CH16.EndUpdate;
  Current_CH16.EndUpdate;
  Voltage_CH17.EndUpdate;
  Current_CH17.EndUpdate;
  Voltage_CH18.EndUpdate;
  Current_CH18.EndUpdate;
  Voltage_CH19.EndUpdate;
  Current_CH19.EndUpdate;
  Voltage_CH20.EndUpdate;
  Current_CH20.EndUpdate;
  Voltage_CH21.EndUpdate;
  Current_CH21.EndUpdate;
  Voltage_CH22.EndUpdate;
  Current_CH22.EndUpdate;
  Voltage_CH23.EndUpdate;
  Current_CH23.EndUpdate;
  Voltage_CH24.EndUpdate;
  Current_CH24.EndUpdate;
  Voltage_CH25.EndUpdate;
  Current_CH25.EndUpdate;
  Voltage_CH26.EndUpdate;
  Current_CH26.EndUpdate;
  Voltage_CH27.EndUpdate;
  Current_CH27.EndUpdate;
  Voltage_CH28.EndUpdate;
  Current_CH28.EndUpdate;
  Voltage_CH29.EndUpdate;
  Current_CH29.EndUpdate;
  Voltage_CH30.EndUpdate;
  Current_CH30.EndUpdate;
  Voltage_CH31.EndUpdate;
  Current_CH31.EndUpdate;
  Voltage_CH32.EndUpdate;
  Current_CH32.EndUpdate;
  Voltage_CH33.EndUpdate;
  Current_CH33.EndUpdate;
  Voltage_CH34.EndUpdate;
  Current_CH34.EndUpdate;
  Voltage_CH35.EndUpdate;
  Current_CH35.EndUpdate;
  Voltage_CH36.EndUpdate;
  Current_CH36.EndUpdate;
  Voltage_CH37.EndUpdate;
  Current_CH37.EndUpdate;
  Voltage_CH38.EndUpdate;
  Current_CH38.EndUpdate;
  Voltage_CH39.EndUpdate;
  Current_CH39.EndUpdate;
  Voltage_CH40.EndUpdate;
  Current_CH40.EndUpdate;
  Voltage_CH41.EndUpdate;
  Current_CH41.EndUpdate;
  Voltage_CH42.EndUpdate;
  Current_CH42.EndUpdate;
  Voltage_CH43.EndUpdate;
  Current_CH43.EndUpdate;
  Voltage_CH44.EndUpdate;
  Current_CH44.EndUpdate;
  Voltage_CH45.EndUpdate;
  Current_CH45.EndUpdate;
  Voltage_CH46.EndUpdate;
  Current_CH46.EndUpdate;
  Voltage_CH47.EndUpdate;
  Current_CH47.EndUpdate;
  Voltage_CH48.EndUpdate;
  Current_CH48.EndUpdate;
  Voltage_CH49.EndUpdate;
  Current_CH49.EndUpdate;
  Voltage_CH50.EndUpdate;
  Current_CH50.EndUpdate;
  Voltage_CH51.EndUpdate;
  Current_CH51.EndUpdate;
  Voltage_CH52.EndUpdate;
  Current_CH52.EndUpdate;
  Voltage_CH53.EndUpdate;
  Current_CH53.EndUpdate;
  Voltage_CH54.EndUpdate;
  Current_CH54.EndUpdate;
  Voltage_CH55.EndUpdate;
  Current_CH55.EndUpdate;
  Voltage_CH56.EndUpdate;
  Current_CH56.EndUpdate;
  Voltage_CH57.EndUpdate;
  Current_CH57.EndUpdate;
  Voltage_CH58.EndUpdate;
  Current_CH58.EndUpdate;
  Voltage_CH59.EndUpdate;
  Current_CH59.EndUpdate;
  Voltage_CH60.EndUpdate;
  Current_CH60.EndUpdate;
  Voltage_CH61.EndUpdate;
  Current_CH61.EndUpdate;
  Voltage_CH62.EndUpdate;
  Current_CH62.EndUpdate;
  Voltage_CH63.EndUpdate;
  Current_CH63.EndUpdate;
  Voltage_CH64.EndUpdate;
  Current_CH64.EndUpdate;
  //--
end; 
```
7. **設定「LastPart_ToolButton」的「Event」頁面下「OnClick」為如下程式碼。**
```pascal
procedure TForm1.LastPart_ToolButtonClick(Sender: TObject);
var
  temp_i: Integer;
  temp_str: String;
begin
  // 沒有資料時不動作
  if v299_csv_StringGrid.Cells[1, 1] <> 'Time[ms](R2MS_Lite_v299csv_v20240510a)' then
  begin
    Exit;
  end;
  // 狀態列資訊
  StatusBar1.Panels[3].Text := StatusBar1.Panels[4].Text;
  StatusBar1.Panels[1].Text := '第' + StatusBar1.Panels[3].Text + '次放電';
  StatusBar1.Panels[0].Text := '#' + v299_csv_StringGrid.Cells[0, ( (StrToInt(StatusBar1.Panels[3].Text)-1)*30+1 )+1] + ' ~ #' + v299_csv_StringGrid.Cells[0, ( StrToInt(StatusBar1.Panels[3].Text)*30 )+1];
  // 取出放電狀態
  temp_str := Trim( v299_csv_StringGrid.Cells[5, ( (StrToInt(StatusBar1.Panels[3].Text)-1)*30+2 )] );
  RunningLog_Memo.Lines.Add( temp_str );
  // 依照放電狀態調整元件可見
  if Length(temp_str) = 64 Then
  begin
    //--
    Voltage_PSU.Active := False;
    Current_PSU.Active := False;
    Voltage_CH01.Active := (temp_str[1] = 'v');
    Voltage_CH02.Active := (temp_str[2] = 'v');
    Voltage_CH03.Active := (temp_str[3] = 'v');
    Voltage_CH04.Active := (temp_str[4] = 'v');
    Voltage_CH05.Active := (temp_str[5] = 'v');
    Voltage_CH06.Active := (temp_str[6] = 'v');
    Voltage_CH07.Active := (temp_str[7] = 'v');
    Voltage_CH08.Active := (temp_str[8] = 'v');
    Voltage_CH09.Active := (temp_str[9] = 'v');
    Voltage_CH10.Active := (temp_str[10] = 'v');
    Voltage_CH11.Active := (temp_str[11] = 'v');
    Voltage_CH12.Active := (temp_str[12] = 'v');
    Voltage_CH13.Active := (temp_str[13] = 'v');
    Voltage_CH14.Active := (temp_str[14] = 'v');
    Voltage_CH15.Active := (temp_str[15] = 'v');
    Voltage_CH16.Active := (temp_str[16] = 'v');
    Voltage_CH17.Active := (temp_str[17] = 'v');
    Voltage_CH18.Active := (temp_str[18] = 'v');
    Voltage_CH19.Active := (temp_str[19] = 'v');
    Voltage_CH20.Active := (temp_str[20] = 'v');
    Voltage_CH21.Active := (temp_str[21] = 'v');
    Voltage_CH22.Active := (temp_str[22] = 'v');
    Voltage_CH23.Active := (temp_str[23] = 'v');
    Voltage_CH24.Active := (temp_str[24] = 'v');
    Voltage_CH25.Active := (temp_str[25] = 'v');
    Voltage_CH26.Active := (temp_str[26] = 'v');
    Voltage_CH27.Active := (temp_str[27] = 'v');
    Voltage_CH28.Active := (temp_str[28] = 'v');
    Voltage_CH29.Active := (temp_str[29] = 'v');
    Voltage_CH30.Active := (temp_str[30] = 'v');
    Voltage_CH31.Active := (temp_str[31] = 'v');
    Voltage_CH32.Active := (temp_str[32] = 'v');
    Voltage_CH33.Active := (temp_str[33] = 'v');
    Voltage_CH34.Active := (temp_str[34] = 'v');
    Voltage_CH35.Active := (temp_str[35] = 'v');
    Voltage_CH36.Active := (temp_str[36] = 'v');
    Voltage_CH37.Active := (temp_str[37] = 'v');
    Voltage_CH38.Active := (temp_str[38] = 'v');
    Voltage_CH39.Active := (temp_str[39] = 'v');
    Voltage_CH40.Active := (temp_str[40] = 'v');
    Voltage_CH41.Active := (temp_str[41] = 'v');
    Voltage_CH42.Active := (temp_str[42] = 'v');
    Voltage_CH43.Active := (temp_str[43] = 'v');
    Voltage_CH44.Active := (temp_str[44] = 'v');
    Voltage_CH45.Active := (temp_str[45] = 'v');
    Voltage_CH46.Active := (temp_str[46] = 'v');
    Voltage_CH47.Active := (temp_str[47] = 'v');
    Voltage_CH48.Active := (temp_str[48] = 'v');
    Voltage_CH49.Active := (temp_str[49] = 'v');
    Voltage_CH50.Active := (temp_str[50] = 'v');
    Voltage_CH51.Active := (temp_str[51] = 'v');
    Voltage_CH52.Active := (temp_str[52] = 'v');
    Voltage_CH53.Active := (temp_str[53] = 'v');
    Voltage_CH54.Active := (temp_str[54] = 'v');
    Voltage_CH55.Active := (temp_str[55] = 'v');
    Voltage_CH56.Active := (temp_str[56] = 'v');
    Voltage_CH57.Active := (temp_str[57] = 'v');
    Voltage_CH58.Active := (temp_str[58] = 'v');
    Voltage_CH59.Active := (temp_str[59] = 'v');
    Voltage_CH60.Active := (temp_str[60] = 'v');
    Voltage_CH61.Active := (temp_str[61] = 'v');
    Voltage_CH62.Active := (temp_str[62] = 'v');
    Voltage_CH63.Active := (temp_str[63] = 'v');
    Voltage_CH64.Active := (temp_str[64] = 'v');
    //--
    Current_CH01.Active := not Voltage_CH01.Active;
    Current_CH02.Active := not Voltage_CH02.Active;
    Current_CH03.Active := not Voltage_CH03.Active;
    Current_CH04.Active := not Voltage_CH04.Active;
    Current_CH05.Active := not Voltage_CH05.Active;
    Current_CH06.Active := not Voltage_CH06.Active;
    Current_CH07.Active := not Voltage_CH07.Active;
    Current_CH08.Active := not Voltage_CH08.Active;
    Current_CH09.Active := not Voltage_CH09.Active;
    Current_CH10.Active := not Voltage_CH10.Active;
    Current_CH11.Active := not Voltage_CH11.Active;
    Current_CH12.Active := not Voltage_CH12.Active;
    Current_CH13.Active := not Voltage_CH13.Active;
    Current_CH14.Active := not Voltage_CH14.Active;
    Current_CH15.Active := not Voltage_CH15.Active;
    Current_CH16.Active := not Voltage_CH16.Active;
    Current_CH17.Active := not Voltage_CH17.Active;
    Current_CH18.Active := not Voltage_CH18.Active;
    Current_CH19.Active := not Voltage_CH19.Active;
    Current_CH20.Active := not Voltage_CH20.Active;
    Current_CH21.Active := not Voltage_CH21.Active;
    Current_CH22.Active := not Voltage_CH22.Active;
    Current_CH23.Active := not Voltage_CH23.Active;
    Current_CH24.Active := not Voltage_CH24.Active;
    Current_CH25.Active := not Voltage_CH25.Active;
    Current_CH26.Active := not Voltage_CH26.Active;
    Current_CH27.Active := not Voltage_CH27.Active;
    Current_CH28.Active := not Voltage_CH28.Active;
    Current_CH29.Active := not Voltage_CH29.Active;
    Current_CH30.Active := not Voltage_CH30.Active;
    Current_CH31.Active := not Voltage_CH31.Active;
    Current_CH32.Active := not Voltage_CH32.Active;
    Current_CH33.Active := not Voltage_CH33.Active;
    Current_CH34.Active := not Voltage_CH34.Active;
    Current_CH35.Active := not Voltage_CH35.Active;
    Current_CH36.Active := not Voltage_CH36.Active;
    Current_CH37.Active := not Voltage_CH37.Active;
    Current_CH38.Active := not Voltage_CH38.Active;
    Current_CH39.Active := not Voltage_CH39.Active;
    Current_CH40.Active := not Voltage_CH40.Active;
    Current_CH41.Active := not Voltage_CH41.Active;
    Current_CH42.Active := not Voltage_CH42.Active;
    Current_CH43.Active := not Voltage_CH43.Active;
    Current_CH44.Active := not Voltage_CH44.Active;
    Current_CH45.Active := not Voltage_CH45.Active;
    Current_CH46.Active := not Voltage_CH46.Active;
    Current_CH47.Active := not Voltage_CH47.Active;
    Current_CH48.Active := not Voltage_CH48.Active;
    Current_CH49.Active := not Voltage_CH49.Active;
    Current_CH50.Active := not Voltage_CH50.Active;
    Current_CH51.Active := not Voltage_CH51.Active;
    Current_CH52.Active := not Voltage_CH52.Active;
    Current_CH53.Active := not Voltage_CH53.Active;
    Current_CH54.Active := not Voltage_CH54.Active;
    Current_CH55.Active := not Voltage_CH55.Active;
    Current_CH56.Active := not Voltage_CH56.Active;
    Current_CH57.Active := not Voltage_CH57.Active;
    Current_CH58.Active := not Voltage_CH58.Active;
    Current_CH59.Active := not Voltage_CH59.Active;
    Current_CH60.Active := not Voltage_CH60.Active;
    Current_CH61.Active := not Voltage_CH61.Active;
    Current_CH62.Active := not Voltage_CH62.Active;
    Current_CH63.Active := not Voltage_CH63.Active;
    Current_CH64.Active := not Voltage_CH64.Active;
    //--
  end;
  //--
  // 清空數據
  Voltage_PSU.Clear;
  Current_PSU.Clear;
  Voltage_CH01.Clear;
  Current_CH01.Clear;
  Voltage_CH02.Clear;
  Current_CH02.Clear;
  Voltage_CH03.Clear;
  Current_CH03.Clear;
  Voltage_CH04.Clear;
  Current_CH04.Clear;
  Voltage_CH05.Clear;
  Current_CH05.Clear;
  Voltage_CH06.Clear;
  Current_CH06.Clear;
  Voltage_CH07.Clear;
  Current_CH07.Clear;
  Voltage_CH08.Clear;
  Current_CH08.Clear;
  Voltage_CH09.Clear;
  Current_CH09.Clear;
  Voltage_CH10.Clear;
  Current_CH10.Clear;
  Voltage_CH11.Clear;
  Current_CH11.Clear;
  Voltage_CH12.Clear;
  Current_CH12.Clear;
  Voltage_CH13.Clear;
  Current_CH13.Clear;
  Voltage_CH14.Clear;
  Current_CH14.Clear;
  Voltage_CH15.Clear;
  Current_CH15.Clear;
  Voltage_CH16.Clear;
  Current_CH16.Clear;
  Voltage_CH17.Clear;
  Current_CH17.Clear;
  Voltage_CH18.Clear;
  Current_CH18.Clear;
  Voltage_CH19.Clear;
  Current_CH19.Clear;
  Voltage_CH20.Clear;
  Current_CH20.Clear;
  Voltage_CH21.Clear;
  Current_CH21.Clear;
  Voltage_CH22.Clear;
  Current_CH22.Clear;
  Voltage_CH23.Clear;
  Current_CH23.Clear;
  Voltage_CH24.Clear;
  Current_CH24.Clear;
  Voltage_CH25.Clear;
  Current_CH25.Clear;
  Voltage_CH26.Clear;
  Current_CH26.Clear;
  Voltage_CH27.Clear;
  Current_CH27.Clear;
  Voltage_CH28.Clear;
  Current_CH28.Clear;
  Voltage_CH29.Clear;
  Current_CH29.Clear;
  Voltage_CH30.Clear;
  Current_CH30.Clear;
  Voltage_CH31.Clear;
  Current_CH31.Clear;
  Voltage_CH32.Clear;
  Current_CH32.Clear;
  Voltage_CH33.Clear;
  Current_CH33.Clear;
  Voltage_CH34.Clear;
  Current_CH34.Clear;
  Voltage_CH35.Clear;
  Current_CH35.Clear;
  Voltage_CH36.Clear;
  Current_CH36.Clear;
  Voltage_CH37.Clear;
  Current_CH37.Clear;
  Voltage_CH38.Clear;
  Current_CH38.Clear;
  Voltage_CH39.Clear;
  Current_CH39.Clear;
  Voltage_CH40.Clear;
  Current_CH40.Clear;
  Voltage_CH41.Clear;
  Current_CH41.Clear;
  Voltage_CH42.Clear;
  Current_CH42.Clear;
  Voltage_CH43.Clear;
  Current_CH43.Clear;
  Voltage_CH44.Clear;
  Current_CH44.Clear;
  Voltage_CH45.Clear;
  Current_CH45.Clear;
  Voltage_CH46.Clear;
  Current_CH46.Clear;
  Voltage_CH47.Clear;
  Current_CH47.Clear;
  Voltage_CH48.Clear;
  Current_CH48.Clear;
  Voltage_CH49.Clear;
  Current_CH49.Clear;
  Voltage_CH50.Clear;
  Current_CH50.Clear;
  Voltage_CH51.Clear;
  Current_CH51.Clear;
  Voltage_CH52.Clear;
  Current_CH52.Clear;
  Voltage_CH53.Clear;
  Current_CH53.Clear;
  Voltage_CH54.Clear;
  Current_CH54.Clear;
  Voltage_CH55.Clear;
  Current_CH55.Clear;
  Voltage_CH56.Clear;
  Current_CH56.Clear;
  Voltage_CH57.Clear;
  Current_CH57.Clear;
  Voltage_CH58.Clear;
  Current_CH58.Clear;
  Voltage_CH59.Clear;
  Current_CH59.Clear;
  Voltage_CH60.Clear;
  Current_CH60.Clear;
  Voltage_CH61.Clear;
  Current_CH61.Clear;
  Voltage_CH62.Clear;
  Current_CH62.Clear;
  Voltage_CH63.Clear;
  Current_CH63.Clear;
  Voltage_CH64.Clear;
  Current_CH64.Clear;
  //--
  // 停止更新圖形
  Voltage_PSU.BeginUpdate;
  Current_PSU.BeginUpdate;
  Voltage_CH01.BeginUpdate;
  Current_CH01.BeginUpdate;
  Voltage_CH02.BeginUpdate;
  Current_CH02.BeginUpdate;
  Voltage_CH03.BeginUpdate;
  Current_CH03.BeginUpdate;
  Voltage_CH04.BeginUpdate;
  Current_CH04.BeginUpdate;
  Voltage_CH05.BeginUpdate;
  Current_CH05.BeginUpdate;
  Voltage_CH06.BeginUpdate;
  Current_CH06.BeginUpdate;
  Voltage_CH07.BeginUpdate;
  Current_CH07.BeginUpdate;
  Voltage_CH08.BeginUpdate;
  Current_CH08.BeginUpdate;
  Voltage_CH09.BeginUpdate;
  Current_CH09.BeginUpdate;
  Voltage_CH10.BeginUpdate;
  Current_CH10.BeginUpdate;
  Voltage_CH11.BeginUpdate;
  Current_CH11.BeginUpdate;
  Voltage_CH12.BeginUpdate;
  Current_CH12.BeginUpdate;
  Voltage_CH13.BeginUpdate;
  Current_CH13.BeginUpdate;
  Voltage_CH14.BeginUpdate;
  Current_CH14.BeginUpdate;
  Voltage_CH15.BeginUpdate;
  Current_CH15.BeginUpdate;
  Voltage_CH16.BeginUpdate;
  Current_CH16.BeginUpdate;
  Voltage_CH17.BeginUpdate;
  Current_CH17.BeginUpdate;
  Voltage_CH18.BeginUpdate;
  Current_CH18.BeginUpdate;
  Voltage_CH19.BeginUpdate;
  Current_CH19.BeginUpdate;
  Voltage_CH20.BeginUpdate;
  Current_CH20.BeginUpdate;
  Voltage_CH21.BeginUpdate;
  Current_CH21.BeginUpdate;
  Voltage_CH22.BeginUpdate;
  Current_CH22.BeginUpdate;
  Voltage_CH23.BeginUpdate;
  Current_CH23.BeginUpdate;
  Voltage_CH24.BeginUpdate;
  Current_CH24.BeginUpdate;
  Voltage_CH25.BeginUpdate;
  Current_CH25.BeginUpdate;
  Voltage_CH26.BeginUpdate;
  Current_CH26.BeginUpdate;
  Voltage_CH27.BeginUpdate;
  Current_CH27.BeginUpdate;
  Voltage_CH28.BeginUpdate;
  Current_CH28.BeginUpdate;
  Voltage_CH29.BeginUpdate;
  Current_CH29.BeginUpdate;
  Voltage_CH30.BeginUpdate;
  Current_CH30.BeginUpdate;
  Voltage_CH31.BeginUpdate;
  Current_CH31.BeginUpdate;
  Voltage_CH32.BeginUpdate;
  Current_CH32.BeginUpdate;
  Voltage_CH33.BeginUpdate;
  Current_CH33.BeginUpdate;
  Voltage_CH34.BeginUpdate;
  Current_CH34.BeginUpdate;
  Voltage_CH35.BeginUpdate;
  Current_CH35.BeginUpdate;
  Voltage_CH36.BeginUpdate;
  Current_CH36.BeginUpdate;
  Voltage_CH37.BeginUpdate;
  Current_CH37.BeginUpdate;
  Voltage_CH38.BeginUpdate;
  Current_CH38.BeginUpdate;
  Voltage_CH39.BeginUpdate;
  Current_CH39.BeginUpdate;
  Voltage_CH40.BeginUpdate;
  Current_CH40.BeginUpdate;
  Voltage_CH41.BeginUpdate;
  Current_CH41.BeginUpdate;
  Voltage_CH42.BeginUpdate;
  Current_CH42.BeginUpdate;
  Voltage_CH43.BeginUpdate;
  Current_CH43.BeginUpdate;
  Voltage_CH44.BeginUpdate;
  Current_CH44.BeginUpdate;
  Voltage_CH45.BeginUpdate;
  Current_CH45.BeginUpdate;
  Voltage_CH46.BeginUpdate;
  Current_CH46.BeginUpdate;
  Voltage_CH47.BeginUpdate;
  Current_CH47.BeginUpdate;
  Voltage_CH48.BeginUpdate;
  Current_CH48.BeginUpdate;
  Voltage_CH49.BeginUpdate;
  Current_CH49.BeginUpdate;
  Voltage_CH50.BeginUpdate;
  Current_CH50.BeginUpdate;
  Voltage_CH51.BeginUpdate;
  Current_CH51.BeginUpdate;
  Voltage_CH52.BeginUpdate;
  Current_CH52.BeginUpdate;
  Voltage_CH53.BeginUpdate;
  Current_CH53.BeginUpdate;
  Voltage_CH54.BeginUpdate;
  Current_CH54.BeginUpdate;
  Voltage_CH55.BeginUpdate;
  Current_CH55.BeginUpdate;
  Voltage_CH56.BeginUpdate;
  Current_CH56.BeginUpdate;
  Voltage_CH57.BeginUpdate;
  Current_CH57.BeginUpdate;
  Voltage_CH58.BeginUpdate;
  Current_CH58.BeginUpdate;
  Voltage_CH59.BeginUpdate;
  Current_CH59.BeginUpdate;
  Voltage_CH60.BeginUpdate;
  Current_CH60.BeginUpdate;
  Voltage_CH61.BeginUpdate;
  Current_CH61.BeginUpdate;
  Voltage_CH62.BeginUpdate;
  Current_CH62.BeginUpdate;
  Voltage_CH63.BeginUpdate;
  Current_CH63.BeginUpdate;
  Voltage_CH64.BeginUpdate;
  Current_CH64.BeginUpdate;
  //--
  // 更新圖形數據
  for temp_i := ( (StrToInt(StatusBar1.Panels[3].Text)-1)*30+1 ) to ( StrToInt(StatusBar1.Panels[3].Text)*30 ) do
  begin
    Voltage_PSU.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[2, temp_i+1], NaN));
    Current_PSU.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[3, temp_i+1], NaN));
    Voltage_CH01.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[6, temp_i+1], NaN));
    Current_CH01.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[7, temp_i+1], NaN));
    Voltage_CH02.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[8, temp_i+1], NaN));
    Current_CH02.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[9, temp_i+1], NaN));
    Voltage_CH03.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[10, temp_i+1], NaN));
    Current_CH03.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[11, temp_i+1], NaN));
    Voltage_CH04.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[12, temp_i+1], NaN));
    Current_CH04.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[13, temp_i+1], NaN));
    Voltage_CH05.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[14, temp_i+1], NaN));
    Current_CH05.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[15, temp_i+1], NaN));
    Voltage_CH06.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[16, temp_i+1], NaN));
    Current_CH06.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[17, temp_i+1], NaN));
    Voltage_CH07.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[18, temp_i+1], NaN));
    Current_CH07.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[19, temp_i+1], NaN));
    Voltage_CH08.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[20, temp_i+1], NaN));
    Current_CH08.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[21, temp_i+1], NaN));
    Voltage_CH09.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[22, temp_i+1], NaN));
    Current_CH09.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[23, temp_i+1], NaN));
    Voltage_CH10.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[24, temp_i+1], NaN));
    Current_CH10.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[25, temp_i+1], NaN));
    Voltage_CH11.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[26, temp_i+1], NaN));
    Current_CH11.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[27, temp_i+1], NaN));
    Voltage_CH12.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[28, temp_i+1], NaN));
    Current_CH12.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[29, temp_i+1], NaN));
    Voltage_CH13.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[30, temp_i+1], NaN));
    Current_CH13.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[31, temp_i+1], NaN));
    Voltage_CH14.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[32, temp_i+1], NaN));
    Current_CH14.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[33, temp_i+1], NaN));
    Voltage_CH15.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[34, temp_i+1], NaN));
    Current_CH15.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[35, temp_i+1], NaN));
    Voltage_CH16.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[36, temp_i+1], NaN));
    Current_CH16.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[37, temp_i+1], NaN));
    Voltage_CH17.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[38, temp_i+1], NaN));
    Current_CH17.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[39, temp_i+1], NaN));
    Voltage_CH18.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[40, temp_i+1], NaN));
    Current_CH18.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[41, temp_i+1], NaN));
    Voltage_CH19.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[42, temp_i+1], NaN));
    Current_CH19.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[43, temp_i+1], NaN));
    Voltage_CH20.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[44, temp_i+1], NaN));
    Current_CH20.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[45, temp_i+1], NaN));
    Voltage_CH21.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[46, temp_i+1], NaN));
    Current_CH21.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[47, temp_i+1], NaN));
    Voltage_CH22.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[48, temp_i+1], NaN));
    Current_CH22.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[49, temp_i+1], NaN));
    Voltage_CH23.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[50, temp_i+1], NaN));
    Current_CH23.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[51, temp_i+1], NaN));
    Voltage_CH24.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[52, temp_i+1], NaN));
    Current_CH24.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[53, temp_i+1], NaN));
    Voltage_CH25.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[54, temp_i+1], NaN));
    Current_CH25.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[55, temp_i+1], NaN));
    Voltage_CH26.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[56, temp_i+1], NaN));
    Current_CH26.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[57, temp_i+1], NaN));
    Voltage_CH27.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[58, temp_i+1], NaN));
    Current_CH27.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[59, temp_i+1], NaN));
    Voltage_CH28.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[60, temp_i+1], NaN));
    Current_CH28.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[61, temp_i+1], NaN));
    Voltage_CH29.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[62, temp_i+1], NaN));
    Current_CH29.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[63, temp_i+1], NaN));
    Voltage_CH30.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[64, temp_i+1], NaN));
    Current_CH30.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[65, temp_i+1], NaN));
    Voltage_CH31.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[66, temp_i+1], NaN));
    Current_CH31.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[67, temp_i+1], NaN));
    Voltage_CH32.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[68, temp_i+1], NaN));
    Current_CH32.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[69, temp_i+1], NaN));
    Voltage_CH33.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[70, temp_i+1], NaN));
    Current_CH33.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[71, temp_i+1], NaN));
    Voltage_CH34.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[72, temp_i+1], NaN));
    Current_CH34.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[73, temp_i+1], NaN));
    Voltage_CH35.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[74, temp_i+1], NaN));
    Current_CH35.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[75, temp_i+1], NaN));
    Voltage_CH36.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[76, temp_i+1], NaN));
    Current_CH36.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[77, temp_i+1], NaN));
    Voltage_CH37.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[78, temp_i+1], NaN));
    Current_CH37.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[79, temp_i+1], NaN));
    Voltage_CH38.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[80, temp_i+1], NaN));
    Current_CH38.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[81, temp_i+1], NaN));
    Voltage_CH39.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[82, temp_i+1], NaN));
    Current_CH39.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[83, temp_i+1], NaN));
    Voltage_CH40.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[84, temp_i+1], NaN));
    Current_CH40.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[85, temp_i+1], NaN));
    Voltage_CH41.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[86, temp_i+1], NaN));
    Current_CH41.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[87, temp_i+1], NaN));
    Voltage_CH42.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[88, temp_i+1], NaN));
    Current_CH42.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[89, temp_i+1], NaN));
    Voltage_CH43.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[90, temp_i+1], NaN));
    Current_CH43.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[91, temp_i+1], NaN));
    Voltage_CH44.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[92, temp_i+1], NaN));
    Current_CH44.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[93, temp_i+1], NaN));
    Voltage_CH45.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[94, temp_i+1], NaN));
    Current_CH45.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[95, temp_i+1], NaN));
    Voltage_CH46.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[96, temp_i+1], NaN));
    Current_CH46.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[97, temp_i+1], NaN));
    Voltage_CH47.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[98, temp_i+1], NaN));
    Current_CH47.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[99, temp_i+1], NaN));
    Voltage_CH48.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[100, temp_i+1], NaN));
    Current_CH48.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[101, temp_i+1], NaN));
    Voltage_CH49.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[102, temp_i+1], NaN));
    Current_CH49.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[103, temp_i+1], NaN));
    Voltage_CH50.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[104, temp_i+1], NaN));
    Current_CH50.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[105, temp_i+1], NaN));
    Voltage_CH51.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[106, temp_i+1], NaN));
    Current_CH51.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[107, temp_i+1], NaN));
    Voltage_CH52.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[108, temp_i+1], NaN));
    Current_CH52.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[109, temp_i+1], NaN));
    Voltage_CH53.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[110, temp_i+1], NaN));
    Current_CH53.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[111, temp_i+1], NaN));
    Voltage_CH54.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[112, temp_i+1], NaN));
    Current_CH54.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[113, temp_i+1], NaN));
    Voltage_CH55.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[114, temp_i+1], NaN));
    Current_CH55.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[115, temp_i+1], NaN));
    Voltage_CH56.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[116, temp_i+1], NaN));
    Current_CH56.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[117, temp_i+1], NaN));
    Voltage_CH57.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[118, temp_i+1], NaN));
    Current_CH57.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[119, temp_i+1], NaN));
    Voltage_CH58.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[120, temp_i+1], NaN));
    Current_CH58.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[121, temp_i+1], NaN));
    Voltage_CH59.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[122, temp_i+1], NaN));
    Current_CH59.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[123, temp_i+1], NaN));
    Voltage_CH60.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[124, temp_i+1], NaN));
    Current_CH60.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[125, temp_i+1], NaN));
    Voltage_CH61.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[126, temp_i+1], NaN));
    Current_CH61.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[127, temp_i+1], NaN));
    Voltage_CH62.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[128, temp_i+1], NaN));
    Current_CH62.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[129, temp_i+1], NaN));
    Voltage_CH63.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[130, temp_i+1], NaN));
    Current_CH63.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[131, temp_i+1], NaN));
    Voltage_CH64.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[132, temp_i+1], NaN));
    Current_CH64.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[133, temp_i+1], NaN));
  end;
  //--
  // 允許更新圖形
  Voltage_PSU.EndUpdate;
  Current_PSU.EndUpdate;
  Voltage_CH01.EndUpdate;
  Current_CH01.EndUpdate;
  Voltage_CH02.EndUpdate;
  Current_CH02.EndUpdate;
  Voltage_CH03.EndUpdate;
  Current_CH03.EndUpdate;
  Voltage_CH04.EndUpdate;
  Current_CH04.EndUpdate;
  Voltage_CH05.EndUpdate;
  Current_CH05.EndUpdate;
  Voltage_CH06.EndUpdate;
  Current_CH06.EndUpdate;
  Voltage_CH07.EndUpdate;
  Current_CH07.EndUpdate;
  Voltage_CH08.EndUpdate;
  Current_CH08.EndUpdate;
  Voltage_CH09.EndUpdate;
  Current_CH09.EndUpdate;
  Voltage_CH10.EndUpdate;
  Current_CH10.EndUpdate;
  Voltage_CH11.EndUpdate;
  Current_CH11.EndUpdate;
  Voltage_CH12.EndUpdate;
  Current_CH12.EndUpdate;
  Voltage_CH13.EndUpdate;
  Current_CH13.EndUpdate;
  Voltage_CH14.EndUpdate;
  Current_CH14.EndUpdate;
  Voltage_CH15.EndUpdate;
  Current_CH15.EndUpdate;
  Voltage_CH16.EndUpdate;
  Current_CH16.EndUpdate;
  Voltage_CH17.EndUpdate;
  Current_CH17.EndUpdate;
  Voltage_CH18.EndUpdate;
  Current_CH18.EndUpdate;
  Voltage_CH19.EndUpdate;
  Current_CH19.EndUpdate;
  Voltage_CH20.EndUpdate;
  Current_CH20.EndUpdate;
  Voltage_CH21.EndUpdate;
  Current_CH21.EndUpdate;
  Voltage_CH22.EndUpdate;
  Current_CH22.EndUpdate;
  Voltage_CH23.EndUpdate;
  Current_CH23.EndUpdate;
  Voltage_CH24.EndUpdate;
  Current_CH24.EndUpdate;
  Voltage_CH25.EndUpdate;
  Current_CH25.EndUpdate;
  Voltage_CH26.EndUpdate;
  Current_CH26.EndUpdate;
  Voltage_CH27.EndUpdate;
  Current_CH27.EndUpdate;
  Voltage_CH28.EndUpdate;
  Current_CH28.EndUpdate;
  Voltage_CH29.EndUpdate;
  Current_CH29.EndUpdate;
  Voltage_CH30.EndUpdate;
  Current_CH30.EndUpdate;
  Voltage_CH31.EndUpdate;
  Current_CH31.EndUpdate;
  Voltage_CH32.EndUpdate;
  Current_CH32.EndUpdate;
  Voltage_CH33.EndUpdate;
  Current_CH33.EndUpdate;
  Voltage_CH34.EndUpdate;
  Current_CH34.EndUpdate;
  Voltage_CH35.EndUpdate;
  Current_CH35.EndUpdate;
  Voltage_CH36.EndUpdate;
  Current_CH36.EndUpdate;
  Voltage_CH37.EndUpdate;
  Current_CH37.EndUpdate;
  Voltage_CH38.EndUpdate;
  Current_CH38.EndUpdate;
  Voltage_CH39.EndUpdate;
  Current_CH39.EndUpdate;
  Voltage_CH40.EndUpdate;
  Current_CH40.EndUpdate;
  Voltage_CH41.EndUpdate;
  Current_CH41.EndUpdate;
  Voltage_CH42.EndUpdate;
  Current_CH42.EndUpdate;
  Voltage_CH43.EndUpdate;
  Current_CH43.EndUpdate;
  Voltage_CH44.EndUpdate;
  Current_CH44.EndUpdate;
  Voltage_CH45.EndUpdate;
  Current_CH45.EndUpdate;
  Voltage_CH46.EndUpdate;
  Current_CH46.EndUpdate;
  Voltage_CH47.EndUpdate;
  Current_CH47.EndUpdate;
  Voltage_CH48.EndUpdate;
  Current_CH48.EndUpdate;
  Voltage_CH49.EndUpdate;
  Current_CH49.EndUpdate;
  Voltage_CH50.EndUpdate;
  Current_CH50.EndUpdate;
  Voltage_CH51.EndUpdate;
  Current_CH51.EndUpdate;
  Voltage_CH52.EndUpdate;
  Current_CH52.EndUpdate;
  Voltage_CH53.EndUpdate;
  Current_CH53.EndUpdate;
  Voltage_CH54.EndUpdate;
  Current_CH54.EndUpdate;
  Voltage_CH55.EndUpdate;
  Current_CH55.EndUpdate;
  Voltage_CH56.EndUpdate;
  Current_CH56.EndUpdate;
  Voltage_CH57.EndUpdate;
  Current_CH57.EndUpdate;
  Voltage_CH58.EndUpdate;
  Current_CH58.EndUpdate;
  Voltage_CH59.EndUpdate;
  Current_CH59.EndUpdate;
  Voltage_CH60.EndUpdate;
  Current_CH60.EndUpdate;
  Voltage_CH61.EndUpdate;
  Current_CH61.EndUpdate;
  Voltage_CH62.EndUpdate;
  Current_CH62.EndUpdate;
  Voltage_CH63.EndUpdate;
  Current_CH63.EndUpdate;
  Voltage_CH64.EndUpdate;
  Current_CH64.EndUpdate;
  //--
end;   
```
8. **設定「AllPart_ToolButton」的「Event」頁面下「OnClick」為如下程式碼。**
```pascal
procedure TForm1.AllPart_ToolButtonClick(Sender: TObject);
var
  temp_i: Integer;
begin
  // 沒有資料時不動作
  if v299_csv_StringGrid.Cells[1, 1] <> 'Time[ms](R2MS_Lite_v299csv_v20240510a)' then
  begin
    Exit;
  end;
  // 狀態列資訊
  StatusBar1.Panels[3].Text := '0';
  StatusBar1.Panels[1].Text := '全資料';
  StatusBar1.Panels[0].Text := '載入中，請稍候...';
  //--
  Voltage_PSU.Clear;
  Current_PSU.Clear;
  Voltage_CH01.Clear;
  Current_CH01.Clear;
  Voltage_CH02.Clear;
  Current_CH02.Clear;
  Voltage_CH03.Clear;
  Current_CH03.Clear;
  Voltage_CH04.Clear;
  Current_CH04.Clear;
  Voltage_CH05.Clear;
  Current_CH05.Clear;
  Voltage_CH06.Clear;
  Current_CH06.Clear;
  Voltage_CH07.Clear;
  Current_CH07.Clear;
  Voltage_CH08.Clear;
  Current_CH08.Clear;
  Voltage_CH09.Clear;
  Current_CH09.Clear;
  Voltage_CH10.Clear;
  Current_CH10.Clear;
  Voltage_CH11.Clear;
  Current_CH11.Clear;
  Voltage_CH12.Clear;
  Current_CH12.Clear;
  Voltage_CH13.Clear;
  Current_CH13.Clear;
  Voltage_CH14.Clear;
  Current_CH14.Clear;
  Voltage_CH15.Clear;
  Current_CH15.Clear;
  Voltage_CH16.Clear;
  Current_CH16.Clear;
  Voltage_CH17.Clear;
  Current_CH17.Clear;
  Voltage_CH18.Clear;
  Current_CH18.Clear;
  Voltage_CH19.Clear;
  Current_CH19.Clear;
  Voltage_CH20.Clear;
  Current_CH20.Clear;
  Voltage_CH21.Clear;
  Current_CH21.Clear;
  Voltage_CH22.Clear;
  Current_CH22.Clear;
  Voltage_CH23.Clear;
  Current_CH23.Clear;
  Voltage_CH24.Clear;
  Current_CH24.Clear;
  Voltage_CH25.Clear;
  Current_CH25.Clear;
  Voltage_CH26.Clear;
  Current_CH26.Clear;
  Voltage_CH27.Clear;
  Current_CH27.Clear;
  Voltage_CH28.Clear;
  Current_CH28.Clear;
  Voltage_CH29.Clear;
  Current_CH29.Clear;
  Voltage_CH30.Clear;
  Current_CH30.Clear;
  Voltage_CH31.Clear;
  Current_CH31.Clear;
  Voltage_CH32.Clear;
  Current_CH32.Clear;
  Voltage_CH33.Clear;
  Current_CH33.Clear;
  Voltage_CH34.Clear;
  Current_CH34.Clear;
  Voltage_CH35.Clear;
  Current_CH35.Clear;
  Voltage_CH36.Clear;
  Current_CH36.Clear;
  Voltage_CH37.Clear;
  Current_CH37.Clear;
  Voltage_CH38.Clear;
  Current_CH38.Clear;
  Voltage_CH39.Clear;
  Current_CH39.Clear;
  Voltage_CH40.Clear;
  Current_CH40.Clear;
  Voltage_CH41.Clear;
  Current_CH41.Clear;
  Voltage_CH42.Clear;
  Current_CH42.Clear;
  Voltage_CH43.Clear;
  Current_CH43.Clear;
  Voltage_CH44.Clear;
  Current_CH44.Clear;
  Voltage_CH45.Clear;
  Current_CH45.Clear;
  Voltage_CH46.Clear;
  Current_CH46.Clear;
  Voltage_CH47.Clear;
  Current_CH47.Clear;
  Voltage_CH48.Clear;
  Current_CH48.Clear;
  Voltage_CH49.Clear;
  Current_CH49.Clear;
  Voltage_CH50.Clear;
  Current_CH50.Clear;
  Voltage_CH51.Clear;
  Current_CH51.Clear;
  Voltage_CH52.Clear;
  Current_CH52.Clear;
  Voltage_CH53.Clear;
  Current_CH53.Clear;
  Voltage_CH54.Clear;
  Current_CH54.Clear;
  Voltage_CH55.Clear;
  Current_CH55.Clear;
  Voltage_CH56.Clear;
  Current_CH56.Clear;
  Voltage_CH57.Clear;
  Current_CH57.Clear;
  Voltage_CH58.Clear;
  Current_CH58.Clear;
  Voltage_CH59.Clear;
  Current_CH59.Clear;
  Voltage_CH60.Clear;
  Current_CH60.Clear;
  Voltage_CH61.Clear;
  Current_CH61.Clear;
  Voltage_CH62.Clear;
  Current_CH62.Clear;
  Voltage_CH63.Clear;
  Current_CH63.Clear;
  Voltage_CH64.Clear;
  Current_CH64.Clear;
  //--
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
  Current_PSU.Active := True;
  Current_CH01.Active := True;
  Current_CH02.Active := True;
  Current_CH03.Active := True;
  Current_CH04.Active := True;
  Current_CH05.Active := True;
  Current_CH06.Active := True;
  Current_CH07.Active := True;
  Current_CH08.Active := True;
  Current_CH09.Active := True;
  Current_CH10.Active := True;
  Current_CH11.Active := True;
  Current_CH12.Active := True;
  Current_CH13.Active := True;
  Current_CH14.Active := True;
  Current_CH15.Active := True;
  Current_CH16.Active := True;
  Current_CH17.Active := True;
  Current_CH18.Active := True;
  Current_CH19.Active := True;
  Current_CH20.Active := True;
  Current_CH21.Active := True;
  Current_CH22.Active := True;
  Current_CH23.Active := True;
  Current_CH24.Active := True;
  Current_CH25.Active := True;
  Current_CH26.Active := True;
  Current_CH27.Active := True;
  Current_CH28.Active := True;
  Current_CH29.Active := True;
  Current_CH30.Active := True;
  Current_CH31.Active := True;
  Current_CH32.Active := True;
  Current_CH33.Active := True;
  Current_CH34.Active := True;
  Current_CH35.Active := True;
  Current_CH36.Active := True;
  Current_CH37.Active := True;
  Current_CH38.Active := True;
  Current_CH39.Active := True;
  Current_CH40.Active := True;
  Current_CH41.Active := True;
  Current_CH42.Active := True;
  Current_CH43.Active := True;
  Current_CH44.Active := True;
  Current_CH45.Active := True;
  Current_CH46.Active := True;
  Current_CH47.Active := True;
  Current_CH48.Active := True;
  Current_CH49.Active := True;
  Current_CH50.Active := True;
  Current_CH51.Active := True;
  Current_CH52.Active := True;
  Current_CH53.Active := True;
  Current_CH54.Active := True;
  Current_CH55.Active := True;
  Current_CH56.Active := True;
  Current_CH57.Active := True;
  Current_CH58.Active := True;
  Current_CH59.Active := True;
  Current_CH60.Active := True;
  Current_CH61.Active := True;
  Current_CH62.Active := True;
  Current_CH63.Active := True;
  Current_CH64.Active := True;
  //--
  Voltage_PSU.BeginUpdate;
  Current_PSU.BeginUpdate;
  Voltage_CH01.BeginUpdate;
  Current_CH01.BeginUpdate;
  Voltage_CH02.BeginUpdate;
  Current_CH02.BeginUpdate;
  Voltage_CH03.BeginUpdate;
  Current_CH03.BeginUpdate;
  Voltage_CH04.BeginUpdate;
  Current_CH04.BeginUpdate;
  Voltage_CH05.BeginUpdate;
  Current_CH05.BeginUpdate;
  Voltage_CH06.BeginUpdate;
  Current_CH06.BeginUpdate;
  Voltage_CH07.BeginUpdate;
  Current_CH07.BeginUpdate;
  Voltage_CH08.BeginUpdate;
  Current_CH08.BeginUpdate;
  Voltage_CH09.BeginUpdate;
  Current_CH09.BeginUpdate;
  Voltage_CH10.BeginUpdate;
  Current_CH10.BeginUpdate;
  Voltage_CH11.BeginUpdate;
  Current_CH11.BeginUpdate;
  Voltage_CH12.BeginUpdate;
  Current_CH12.BeginUpdate;
  Voltage_CH13.BeginUpdate;
  Current_CH13.BeginUpdate;
  Voltage_CH14.BeginUpdate;
  Current_CH14.BeginUpdate;
  Voltage_CH15.BeginUpdate;
  Current_CH15.BeginUpdate;
  Voltage_CH16.BeginUpdate;
  Current_CH16.BeginUpdate;
  Voltage_CH17.BeginUpdate;
  Current_CH17.BeginUpdate;
  Voltage_CH18.BeginUpdate;
  Current_CH18.BeginUpdate;
  Voltage_CH19.BeginUpdate;
  Current_CH19.BeginUpdate;
  Voltage_CH20.BeginUpdate;
  Current_CH20.BeginUpdate;
  Voltage_CH21.BeginUpdate;
  Current_CH21.BeginUpdate;
  Voltage_CH22.BeginUpdate;
  Current_CH22.BeginUpdate;
  Voltage_CH23.BeginUpdate;
  Current_CH23.BeginUpdate;
  Voltage_CH24.BeginUpdate;
  Current_CH24.BeginUpdate;
  Voltage_CH25.BeginUpdate;
  Current_CH25.BeginUpdate;
  Voltage_CH26.BeginUpdate;
  Current_CH26.BeginUpdate;
  Voltage_CH27.BeginUpdate;
  Current_CH27.BeginUpdate;
  Voltage_CH28.BeginUpdate;
  Current_CH28.BeginUpdate;
  Voltage_CH29.BeginUpdate;
  Current_CH29.BeginUpdate;
  Voltage_CH30.BeginUpdate;
  Current_CH30.BeginUpdate;
  Voltage_CH31.BeginUpdate;
  Current_CH31.BeginUpdate;
  Voltage_CH32.BeginUpdate;
  Current_CH32.BeginUpdate;
  Voltage_CH33.BeginUpdate;
  Current_CH33.BeginUpdate;
  Voltage_CH34.BeginUpdate;
  Current_CH34.BeginUpdate;
  Voltage_CH35.BeginUpdate;
  Current_CH35.BeginUpdate;
  Voltage_CH36.BeginUpdate;
  Current_CH36.BeginUpdate;
  Voltage_CH37.BeginUpdate;
  Current_CH37.BeginUpdate;
  Voltage_CH38.BeginUpdate;
  Current_CH38.BeginUpdate;
  Voltage_CH39.BeginUpdate;
  Current_CH39.BeginUpdate;
  Voltage_CH40.BeginUpdate;
  Current_CH40.BeginUpdate;
  Voltage_CH41.BeginUpdate;
  Current_CH41.BeginUpdate;
  Voltage_CH42.BeginUpdate;
  Current_CH42.BeginUpdate;
  Voltage_CH43.BeginUpdate;
  Current_CH43.BeginUpdate;
  Voltage_CH44.BeginUpdate;
  Current_CH44.BeginUpdate;
  Voltage_CH45.BeginUpdate;
  Current_CH45.BeginUpdate;
  Voltage_CH46.BeginUpdate;
  Current_CH46.BeginUpdate;
  Voltage_CH47.BeginUpdate;
  Current_CH47.BeginUpdate;
  Voltage_CH48.BeginUpdate;
  Current_CH48.BeginUpdate;
  Voltage_CH49.BeginUpdate;
  Current_CH49.BeginUpdate;
  Voltage_CH50.BeginUpdate;
  Current_CH50.BeginUpdate;
  Voltage_CH51.BeginUpdate;
  Current_CH51.BeginUpdate;
  Voltage_CH52.BeginUpdate;
  Current_CH52.BeginUpdate;
  Voltage_CH53.BeginUpdate;
  Current_CH53.BeginUpdate;
  Voltage_CH54.BeginUpdate;
  Current_CH54.BeginUpdate;
  Voltage_CH55.BeginUpdate;
  Current_CH55.BeginUpdate;
  Voltage_CH56.BeginUpdate;
  Current_CH56.BeginUpdate;
  Voltage_CH57.BeginUpdate;
  Current_CH57.BeginUpdate;
  Voltage_CH58.BeginUpdate;
  Current_CH58.BeginUpdate;
  Voltage_CH59.BeginUpdate;
  Current_CH59.BeginUpdate;
  Voltage_CH60.BeginUpdate;
  Current_CH60.BeginUpdate;
  Voltage_CH61.BeginUpdate;
  Current_CH61.BeginUpdate;
  Voltage_CH62.BeginUpdate;
  Current_CH62.BeginUpdate;
  Voltage_CH63.BeginUpdate;
  Current_CH63.BeginUpdate;
  Voltage_CH64.BeginUpdate;
  Current_CH64.BeginUpdate;
  //--
  for temp_i := 2 to v299_csv_StringGrid.RowCount - 1 do
  begin
      Voltage_PSU.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[2, temp_i], NaN));
      Current_PSU.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[3, temp_i], NaN));
      Voltage_CH01.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[6, temp_i], NaN));
      Current_CH01.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[7, temp_i], NaN));
      Voltage_CH02.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[8, temp_i], NaN));
      Current_CH02.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[9, temp_i], NaN));
      Voltage_CH03.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[10, temp_i], NaN));
      Current_CH03.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[11, temp_i], NaN));
      Voltage_CH04.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[12, temp_i], NaN));
      Current_CH04.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[13, temp_i], NaN));
      Voltage_CH05.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[14, temp_i], NaN));
      Current_CH05.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[15, temp_i], NaN));
      Voltage_CH06.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[16, temp_i], NaN));
      Current_CH06.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[17, temp_i], NaN));
      Voltage_CH07.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[18, temp_i], NaN));
      Current_CH07.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[19, temp_i], NaN));
      Voltage_CH08.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[20, temp_i], NaN));
      Current_CH08.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[21, temp_i], NaN));
      Voltage_CH09.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[22, temp_i], NaN));
      Current_CH09.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[23, temp_i], NaN));
      Voltage_CH10.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[24, temp_i], NaN));
      Current_CH10.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[25, temp_i], NaN));
      Voltage_CH11.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[26, temp_i], NaN));
      Current_CH11.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[27, temp_i], NaN));
      Voltage_CH12.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[28, temp_i], NaN));
      Current_CH12.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[29, temp_i], NaN));
      Voltage_CH13.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[30, temp_i], NaN));
      Current_CH13.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[31, temp_i], NaN));
      Voltage_CH14.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[32, temp_i], NaN));
      Current_CH14.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[33, temp_i], NaN));
      Voltage_CH15.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[34, temp_i], NaN));
      Current_CH15.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[35, temp_i], NaN));
      Voltage_CH16.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[36, temp_i], NaN));
      Current_CH16.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[37, temp_i], NaN));
      Voltage_CH17.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[38, temp_i], NaN));
      Current_CH17.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[39, temp_i], NaN));
      Voltage_CH18.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[40, temp_i], NaN));
      Current_CH18.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[41, temp_i], NaN));
      Voltage_CH19.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[42, temp_i], NaN));
      Current_CH19.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[43, temp_i], NaN));
      Voltage_CH20.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[44, temp_i], NaN));
      Current_CH20.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[45, temp_i], NaN));
      Voltage_CH21.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[46, temp_i], NaN));
      Current_CH21.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[47, temp_i], NaN));
      Voltage_CH22.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[48, temp_i], NaN));
      Current_CH22.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[49, temp_i], NaN));
      Voltage_CH23.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[50, temp_i], NaN));
      Current_CH23.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[51, temp_i], NaN));
      Voltage_CH24.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[52, temp_i], NaN));
      Current_CH24.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[53, temp_i], NaN));
      Voltage_CH25.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[54, temp_i], NaN));
      Current_CH25.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[55, temp_i], NaN));
      Voltage_CH26.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[56, temp_i], NaN));
      Current_CH26.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[57, temp_i], NaN));
      Voltage_CH27.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[58, temp_i], NaN));
      Current_CH27.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[59, temp_i], NaN));
      Voltage_CH28.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[60, temp_i], NaN));
      Current_CH28.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[61, temp_i], NaN));
      Voltage_CH29.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[62, temp_i], NaN));
      Current_CH29.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[63, temp_i], NaN));
      Voltage_CH30.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[64, temp_i], NaN));
      Current_CH30.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[65, temp_i], NaN));
      Voltage_CH31.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[66, temp_i], NaN));
      Current_CH31.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[67, temp_i], NaN));
      Voltage_CH32.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[68, temp_i], NaN));
      Current_CH32.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[69, temp_i], NaN));
      Voltage_CH33.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[70, temp_i], NaN));
      Current_CH33.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[71, temp_i], NaN));
      Voltage_CH34.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[72, temp_i], NaN));
      Current_CH34.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[73, temp_i], NaN));
      Voltage_CH35.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[74, temp_i], NaN));
      Current_CH35.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[75, temp_i], NaN));
      Voltage_CH36.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[76, temp_i], NaN));
      Current_CH36.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[77, temp_i], NaN));
      Voltage_CH37.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[78, temp_i], NaN));
      Current_CH37.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[79, temp_i], NaN));
      Voltage_CH38.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[80, temp_i], NaN));
      Current_CH38.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[81, temp_i], NaN));
      Voltage_CH39.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[82, temp_i], NaN));
      Current_CH39.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[83, temp_i], NaN));
      Voltage_CH40.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[84, temp_i], NaN));
      Current_CH40.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[85, temp_i], NaN));
      Voltage_CH41.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[86, temp_i], NaN));
      Current_CH41.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[87, temp_i], NaN));
      Voltage_CH42.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[88, temp_i], NaN));
      Current_CH42.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[89, temp_i], NaN));
      Voltage_CH43.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[90, temp_i], NaN));
      Current_CH43.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[91, temp_i], NaN));
      Voltage_CH44.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[92, temp_i], NaN));
      Current_CH44.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[93, temp_i], NaN));
      Voltage_CH45.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[94, temp_i], NaN));
      Current_CH45.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[95, temp_i], NaN));
      Voltage_CH46.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[96, temp_i], NaN));
      Current_CH46.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[97, temp_i], NaN));
      Voltage_CH47.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[98, temp_i], NaN));
      Current_CH47.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[99, temp_i], NaN));
      Voltage_CH48.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[100, temp_i], NaN));
      Current_CH48.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[101, temp_i], NaN));
      Voltage_CH49.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[102, temp_i], NaN));
      Current_CH49.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[103, temp_i], NaN));
      Voltage_CH50.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[104, temp_i], NaN));
      Current_CH50.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[105, temp_i], NaN));
      Voltage_CH51.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[106, temp_i], NaN));
      Current_CH51.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[107, temp_i], NaN));
      Voltage_CH52.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[108, temp_i], NaN));
      Current_CH52.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[109, temp_i], NaN));
      Voltage_CH53.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[110, temp_i], NaN));
      Current_CH53.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[111, temp_i], NaN));
      Voltage_CH54.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[112, temp_i], NaN));
      Current_CH54.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[113, temp_i], NaN));
      Voltage_CH55.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[114, temp_i], NaN));
      Current_CH55.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[115, temp_i], NaN));
      Voltage_CH56.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[116, temp_i], NaN));
      Current_CH56.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[117, temp_i], NaN));
      Voltage_CH57.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[118, temp_i], NaN));
      Current_CH57.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[119, temp_i], NaN));
      Voltage_CH58.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[120, temp_i], NaN));
      Current_CH58.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[121, temp_i], NaN));
      Voltage_CH59.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[122, temp_i], NaN));
      Current_CH59.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[123, temp_i], NaN));
      Voltage_CH60.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[124, temp_i], NaN));
      Current_CH60.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[125, temp_i], NaN));
      Voltage_CH61.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[126, temp_i], NaN));
      Current_CH61.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[127, temp_i], NaN));
      Voltage_CH62.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[128, temp_i], NaN));
      Current_CH62.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[129, temp_i], NaN));
      Voltage_CH63.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[130, temp_i], NaN));
      Current_CH63.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[131, temp_i], NaN));
      Voltage_CH64.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[132, temp_i], NaN));
      Current_CH64.AddY(StrToFloatDef(v299_csv_StringGrid.Cells[133, temp_i], NaN));
  end;
  //--
  Voltage_PSU.EndUpdate;
  Current_PSU.EndUpdate;
  Voltage_CH01.EndUpdate;
  Current_CH01.EndUpdate;
  Voltage_CH02.EndUpdate;
  Current_CH02.EndUpdate;
  Voltage_CH03.EndUpdate;
  Current_CH03.EndUpdate;
  Voltage_CH04.EndUpdate;
  Current_CH04.EndUpdate;
  Voltage_CH05.EndUpdate;
  Current_CH05.EndUpdate;
  Voltage_CH06.EndUpdate;
  Current_CH06.EndUpdate;
  Voltage_CH07.EndUpdate;
  Current_CH07.EndUpdate;
  Voltage_CH08.EndUpdate;
  Current_CH08.EndUpdate;
  Voltage_CH09.EndUpdate;
  Current_CH09.EndUpdate;
  Voltage_CH10.EndUpdate;
  Current_CH10.EndUpdate;
  Voltage_CH11.EndUpdate;
  Current_CH11.EndUpdate;
  Voltage_CH12.EndUpdate;
  Current_CH12.EndUpdate;
  Voltage_CH13.EndUpdate;
  Current_CH13.EndUpdate;
  Voltage_CH14.EndUpdate;
  Current_CH14.EndUpdate;
  Voltage_CH15.EndUpdate;
  Current_CH15.EndUpdate;
  Voltage_CH16.EndUpdate;
  Current_CH16.EndUpdate;
  Voltage_CH17.EndUpdate;
  Current_CH17.EndUpdate;
  Voltage_CH18.EndUpdate;
  Current_CH18.EndUpdate;
  Voltage_CH19.EndUpdate;
  Current_CH19.EndUpdate;
  Voltage_CH20.EndUpdate;
  Current_CH20.EndUpdate;
  Voltage_CH21.EndUpdate;
  Current_CH21.EndUpdate;
  Voltage_CH22.EndUpdate;
  Current_CH22.EndUpdate;
  Voltage_CH23.EndUpdate;
  Current_CH23.EndUpdate;
  Voltage_CH24.EndUpdate;
  Current_CH24.EndUpdate;
  Voltage_CH25.EndUpdate;
  Current_CH25.EndUpdate;
  Voltage_CH26.EndUpdate;
  Current_CH26.EndUpdate;
  Voltage_CH27.EndUpdate;
  Current_CH27.EndUpdate;
  Voltage_CH28.EndUpdate;
  Current_CH28.EndUpdate;
  Voltage_CH29.EndUpdate;
  Current_CH29.EndUpdate;
  Voltage_CH30.EndUpdate;
  Current_CH30.EndUpdate;
  Voltage_CH31.EndUpdate;
  Current_CH31.EndUpdate;
  Voltage_CH32.EndUpdate;
  Current_CH32.EndUpdate;
  Voltage_CH33.EndUpdate;
  Current_CH33.EndUpdate;
  Voltage_CH34.EndUpdate;
  Current_CH34.EndUpdate;
  Voltage_CH35.EndUpdate;
  Current_CH35.EndUpdate;
  Voltage_CH36.EndUpdate;
  Current_CH36.EndUpdate;
  Voltage_CH37.EndUpdate;
  Current_CH37.EndUpdate;
  Voltage_CH38.EndUpdate;
  Current_CH38.EndUpdate;
  Voltage_CH39.EndUpdate;
  Current_CH39.EndUpdate;
  Voltage_CH40.EndUpdate;
  Current_CH40.EndUpdate;
  Voltage_CH41.EndUpdate;
  Current_CH41.EndUpdate;
  Voltage_CH42.EndUpdate;
  Current_CH42.EndUpdate;
  Voltage_CH43.EndUpdate;
  Current_CH43.EndUpdate;
  Voltage_CH44.EndUpdate;
  Current_CH44.EndUpdate;
  Voltage_CH45.EndUpdate;
  Current_CH45.EndUpdate;
  Voltage_CH46.EndUpdate;
  Current_CH46.EndUpdate;
  Voltage_CH47.EndUpdate;
  Current_CH47.EndUpdate;
  Voltage_CH48.EndUpdate;
  Current_CH48.EndUpdate;
  Voltage_CH49.EndUpdate;
  Current_CH49.EndUpdate;
  Voltage_CH50.EndUpdate;
  Current_CH50.EndUpdate;
  Voltage_CH51.EndUpdate;
  Current_CH51.EndUpdate;
  Voltage_CH52.EndUpdate;
  Current_CH52.EndUpdate;
  Voltage_CH53.EndUpdate;
  Current_CH53.EndUpdate;
  Voltage_CH54.EndUpdate;
  Current_CH54.EndUpdate;
  Voltage_CH55.EndUpdate;
  Current_CH55.EndUpdate;
  Voltage_CH56.EndUpdate;
  Current_CH56.EndUpdate;
  Voltage_CH57.EndUpdate;
  Current_CH57.EndUpdate;
  Voltage_CH58.EndUpdate;
  Current_CH58.EndUpdate;
  Voltage_CH59.EndUpdate;
  Current_CH59.EndUpdate;
  Voltage_CH60.EndUpdate;
  Current_CH60.EndUpdate;
  Voltage_CH61.EndUpdate;
  Current_CH61.EndUpdate;
  Voltage_CH62.EndUpdate;
  Current_CH62.EndUpdate;
  Voltage_CH63.EndUpdate;
  Current_CH63.EndUpdate;
  Voltage_CH64.EndUpdate;
  Current_CH64.EndUpdate;
  //--
  // 狀態列資訊
  StatusBar1.Panels[0].Text := '載入完成!';
end;  
```
9. **設定「Form1」的「properties」頁面下「KeyPreview」為「True」。。**
10. **去implementation區塊追加宣告全域變數**
```pascal
implementation
//--------------------------------------------------------------------------
//uses add by HsiupoYeh
uses
  Math,LCLType;
//--------------------------------------------------------------------------
//--------------------------------------------------------------------------
//宣告全域變數 add by HsiupoYeh
var
  version_str: AnsiString;
  Current_Folder_Path: AnsiString;
//--------------------------------------------------------------------------

{$R *.lfm}
```
11. **設定「Form1」的「Event」頁面下「OnKeyDown」為如下程式碼。**
```pascal
procedure TForm1.FormKeyDown(Sender: TObject; var Key: Word; Shift: TShiftState
  );
begin
  // 檢查釋放的鍵是否為左箭頭鍵 (VK_LEFT)
  if Key = VK_LEFT then
  begin
    // **檢查是否同時按下了 CTRL 鍵**
    if ssCtrl in Shift then
    begin
      // 1. 阻止預設行為
      Key := 0;
      // 2. 呼叫您指定的按鈕事件
      PreviousPart_ToolButtonClick(PreviousPart_ToolButton);
    end;
  end
  else if Key = VK_RIGHT then
  begin
    // **檢查是否同時按下了 CTRL 鍵**
    if ssCtrl in Shift then
    begin
      // 1. 阻止預設行為
      Key := 0;
      // 2. 呼叫您指定的按鈕事件
      NextPart_ToolButtonClick(NextPart_ToolButton);
    end;
  end;
end;  
```
