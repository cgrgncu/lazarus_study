# R2MS_Lite_ERTMaker 開發筆記
+ 作者: HsiupoYeh  
+ 更新日期: 2025-11-03

## 開發環境  
+ 使用「Win10_64_CHT_Lazarus4.2_v20250915a.ova」。**匯入為「Win10_Lazarus4.2_R2MS_Lite_ERTMaker_v20251103a.ova」。**
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
+ 需要使用外部python程式
  + 使用Lite_PyGimli_Env (pygimli==1.5.4)
    + https://github.com/cgrgncu/pyGimli_dev/releases/tag/Lite_PyGimli_Env
  + 將目錄規定為以下結構
  ```
  ...\R2MS_Lite_ERTMaker.exe
  ...\PythonEnv\
  ...\PythonEnv\python.exe
  ...\PythonPrograms\... (相關的python程式)
  ```

### 開發紀錄(製作APP Icon)  
+ 使用工具1: https://icon.kitchen/  
  + IOS Icon  
  + Text: 「R2MS Lite」  
  + BackGround>Type=「Gradient」  
  + More>Badge = 「ERTMaker」  
+ 再使用工具2: https://tfin.ch/iosicon.html  
  + 輸出有圓角的圖片。通常選 **android/play_store_512.png** 來做圓角。
+ 再使用工具3: PNG轉ICO線上工具 例如:https://www.aconvert.com/tw/icon/png-to-ico/
  + 需要的多解析度包含:  
    + 全部清單中有的
+ ICO下載位置: [右鍵另存](https://raw.githubusercontent.com/cgrgncu/lazarus_study/refs/heads/main/R2MS_Lite_ERTMaker/ico/R2MS_Lite_ERTMaker.ico) 

> ICO:  
>  ![image](https://raw.githubusercontent.com/cgrgncu/lazarus_study/refs/heads/main/R2MS_Lite_ERTMaker/ico/R2MS_Lite_ERTMaker.ico)  

## 開發紀錄(基礎框架部分)
1. 建立新專案。  
    + **1.1 按下「Project>New Project ...」。**  
    + **1.2 選擇「Project>Application 」之後按「OK」。**  
2. 獨立把專案存到指定資料夾中，以利日後管理。大部分自訂程式碼都會是相對路徑，未來只需要帶走這個資料夾即可在其他地方重新使用。  
    + **2.1 在桌面建立「R2MS_Lite_ERTMaker」資料夾**  
    + **2.2 回到Lazarus，按下「Project>Save Project As」。**  
    + **2.3 將Project檔案(.lpi)存到桌面上的「R2MS_Lite_ERTMaker｣資料夾中，檔案命名為「R2MS_Lite_ERTMaker.lpi」。**  
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
      + **4.1.2 設定「Form1」的「properties」頁面下「Caption」為「R2MS_Lite_ERTMaker」。**  
        + 4.1.2.1 說明:設定窗體的標題列文字。  
      + **4.1.3 設定「Form1」的「properties」頁面下「Color」為「clWindow」。**  
        + 4.1.3.1 說明:設定成白色使工具列元件底色是白色。  
      + **4.1.4 設定「Form1」的「properties」頁面下「Height」為「600」。**  
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
          version_str := 'v20251103a';
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
    + **5.2 點兩下元件，進入編輯模式，建立第一層第二個選單「說明(&H)」，「Caption」設為「說明(&H)」，「Name」設為「MainMenu1_1」。**  
    + **5.3 繼續建立第一層第二個選單的子選單「說明(&H)->關於我(&A)」，「Caption」設為「關於我(&A)」，「Name」設為「MainMenu1_1_1」。**  
      + 5.3.1 設定「MainMenu1_1_1」的「Event」。
        + **5.3.1.1 設定「MainMenu1_1_1」的「Event」頁面下「OnClick」為如下程式碼。**  
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
    + **7.1.1 設定「Align」為「AlClient」。**
  + **7.2 拖拉一個「Standard>TPanel」到「ScrollBox1」中。預設名稱會是「Panel1」，修改「Name」為「EmptyPanel」**
    + 7.2.1 設定「EmptyPanel」的「Properties」。
      + **7.2.1.1 設定「EmptyPanel」的「Properties」頁面下「BevelOuter」為「bvNone」。**
      + **7.2.1.2 設定「EmptyPanel」的「Properties」頁面下「Caption」為空白。**
      + **7.2.1.3 設定「EmptyPanel」的「Properties」頁面下「Left」為「926」。**
      + **7.2.1.4 設定「EmptyPanel」的「Properties」頁面下「Top」為「524」。**

### 開發紀錄(主頁面)  
1. 五個分頁(TPageControl):  
    + **1.1 拖拉一個「Common Controls>TPageControl」到「ScrollBox1」中。預設名稱會是「PageControl1」，修改「Name」為「Main_PageControl」。**  
      + **1.1.1 設定「Align」為「AlClient」。**
    + **1.2 在「PageControl1」元件上按「右鍵>Add Page」，會增加一個新頁面。預設名稱會是「TabSheet1」，修改「Name」為「Forward_TabSheet」。**
      + **1.2.1 設定「Caption」為「順推工具」。**
    + **1.3 在「PageControl1」元件上按「右鍵>Add Page」，會增加一個新頁面。預設名稱會是「TabSheet1」，修改「Name」為「Inversion_TabSheet」。**
      + **1.3.1 設定「Caption」為「逆推工具」。**
    + **1.4 在「PageControl1」元件上按「右鍵>Add Page」，會增加一個新頁面。預設名稱會是「TabSheet1」，修改「Name」為「RunningLog_TabSheet」。**
      + **1.4.1 設定「Caption」為「運行紀錄」。**
    + **1.5 在「PageControl1」元件上按「右鍵>Add Page」，會增加一個新頁面。預設名稱會是「TabSheet1」，修改「Name」為「UpdateLog_TabSheet」。**
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
