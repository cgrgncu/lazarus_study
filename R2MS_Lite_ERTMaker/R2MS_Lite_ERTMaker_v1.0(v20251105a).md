# R2MS_Lite_ERTMaker 開發筆記
+ 作者: HsiupoYeh  
+ 更新日期: 2025-11-05

## 開發環境  
+ 使用「Win10_64_CHT_Lazarus4.2_v20250915a.ova」。**匯入為「Win10_Lazarus4.2_R2MS_Lite_ERTMaker_v20251105a.ova」。**
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
  + 準備VC檔案，沒有安裝過散發套件的電腦會需要。
  ```
  ...\PythonEnv\vcruntime140.dll
  ...\PythonEnv\vcruntime140_1.dll
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
          version_str := 'v1.0(v20251105a)';
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
          temp_str:='作者: HsiupoYeh.'+#13#10+'程式版本: '+version_str+#13#10+'數值模擬運算核心: PyGIMLi v1.5.4';
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
      + **1.1.1 設定「TabStop」為「False」。**
    + **1.2 在「PageControl1」元件上按「右鍵>Add Page」，會增加一個新頁面。預設名稱會是「TabSheet1」，修改「Name」為「Forward_TabSheet」。**
      + **1.2.1 設定「Caption」為「順推工具」。**
    + **1.3 在「PageControl1」元件上按「右鍵>Add Page」，會增加一個新頁面。預設名稱會是「TabSheet1」，修改「Name」為「Inversion_TabSheet」。**
      + **1.3.1 設定「Caption」為「逆推工具」。**
    + **1.4 在「PageControl1」元件上按「右鍵>Add Page」，會增加一個新頁面。預設名稱會是「TabSheet1」，修改「Name」為「RunningLog_TabSheet」。**
      + **1.4.1 設定「Caption」為「運行紀錄」。**
    + **1.5 在「PageControl1」元件上按「右鍵>Add Page」，會增加一個新頁面。預設名稱會是「TabSheet1」，修改「Name」為「UpdateLog_TabSheet」。**
      + **1.5.1 設定「Caption」為「更新紀錄」。**
            
2. 第1個分頁「順推工具」(Forward_TabSheet):  
    + **2.1 拖拉一個「Common Controls>TPageControl」到「Forward_TabSheet」中。預設名稱會是「PageControl1」，修改「Name」為「Forward_PageControl」。**  
      + **2.1.1 設定「Align」為「AlClient」。**
      + **2.1.2 設定「TabStop」為「False」。**
   + **2.2 在「Forward_PageControl」元件上按「右鍵>Add Page」，會增加一個新頁面。預設名稱會是「TabSheet1」，修改「Name」為「CreateMesh_TabSheet」。**
      + **2.2.1 設定「Caption」為「模型網格建立」。**
    + **2.3 在「Forward_PageControl」元件上按「右鍵>Add Page」，會增加一個新頁面。預設名稱會是「TabSheet1」，修改「Name」為「CreateMeshSettings_TabSheet」。**
      + **2.3.1 設定「Caption」為「模型網格建立參數」。**
    + **2.4 在「Forward_PageControl」元件上按「右鍵>Add Page」，會增加一個新頁面。預設名稱會是「TabSheet1」，修改「Name」為「ForwardModeling_TabSheet」。**
      + **2.4.1 設定「Caption」為「順推模擬運算」。**
    + **2.5 在「Forward_PageControl」元件上按「右鍵>Add Page」，會增加一個新頁面。預設名稱會是「TabSheet1」，修改「Name」為「ForwardModelingSettings_TabSheet」。**
      + **2.5.1 設定「Caption」為「順推模擬運算參數」。**
    + **2.6 在「Forward_PageControl」元件上按「右鍵>Add Page」，會增加一個新頁面。預設名稱會是「TabSheet1」，修改「Name」為「TimeSeriesProcessing_TabSheet」。**
      + **2.6.1 設定「Caption」為「時間序列解算」。**
    + **2.7 在「Forward_PageControl」元件上按「右鍵>Add Page」，會增加一個新頁面。預設名稱會是「TabSheet1」，修改「Name」為「TimeSeriesProcessingSettings_TabSheet」。**
      + **2.7.1 設定「Caption」為「時間序列解算參數」。**
    + **2.8 略過。**
    + **2.9 略過。** 
    + 2.10 第1個分頁「模型網格建立」(CreateMesh_TabSheet):
      + **2.10.1 拖拉一個「Standard>TToolBar」到「CreateMesh_TabSheet」中。預設名稱會是「ToolBar1」，修改「Name」為「CreateMesh_ToolBar」。**
      + **2.10.2 設定「BorderSpacing>Top」為「1」。**
      + **2.10.3 設定「Edge>Borders>ebBottom」為「True」。**
      + **2.10.4 設定「Edge>Borders>ebLeft」為「True」。**
      + **2.10.5 設定「Edge>Borders>ebRight」為「True」。**
      + **2.10.6 設定「ShowCaptions」為「True」。** 
      + **2.10.7 在「ToolBar1」元件上按「右鍵>New CheckButton」，會增加一個新按鈕。預設名稱會是「ToolButton1」，修改「Name」為「CreateMeshLoad_ToolButton」**
        + **2.10.7.1 設定「Caption」為「載入參數」。**
        + **2.10.7.2 設定「CreateMeshLoad_ToolButton」的「Event」頁面下「OnClick」為如下程式碼。**
        ```pascal

        ```
      + **2.10.8 在「ToolBar1」元件上按「右鍵>New CheckButton」，會增加一個新按鈕。預設名稱會是「ToolButton1」，修改「Name」為「CreateMeshRun_ToolButton」**
        + **2.10.8.1 設定「Caption」為「建立模型網格」。**
        + **2.10.8.2 設定「CreateMeshRun_ToolButton」的「Event」頁面下「OnClick」為如下程式碼。**
        ```pascal

        ```
      + **2.10.9 在「ToolBar1」元件上按「右鍵>New CheckButton」，會增加一個新按鈕。預設名稱會是「ToolButton1」，修改「Name」為「CreateMeshOpenOutputFolder_ToolButton」**
        + **2.10.9.1 設定「Caption」為「開啟輸出資料夾」。**
        + **2.10.9.2 設定「CreateMeshOpenOutputFolder_ToolButton」的「Event」頁面下「OnClick」為如下程式碼。**
        ```pascal

        ```   
      + **2.10.10 拖拉一個「Standard>TGroupBox」到「CreateMesh_TabSheet」中。預設名稱會是「GroupBox1」，修改「Name」為「CreateMeshParameters_GroupBox」。**
        + **2.10.10.1 設定「Align」為「alRight」。**
        + **2.10.10.2 設定「BorderSpacing>Left」為「3」。**
        + **2.10.10.3 設定「BorderSpacing>Right」為「1」。**
        + **2.10.10.4 設定「Caption」為「參數設定:」。** 
        + **2.10.10.5 設定「Width」為「500」。**
      + **2.10.11 拖拉一個「Standard>TGroupBox」到「CreateMesh_TabSheet」中。預設名稱會是「GroupBox1」，修改「Name」為「CreateMeshPreview_GroupBox」。**
        + **2.10.11.1 設定「Align」為「alClient」。**
        + **2.10.11.2 設定「Caption」為「模型預覽:」。** 
      + **2.10.12 拖拉一個「Additional>TImage」到「CreateMeshPreview_GroupBox」中。預設名稱會是「Image1」，修改「Name」為「CreateMeshPreview_Image」。**
        + **2.10.12.1 設定「Align」為「alClient」。**
        + **2.10.12.2 設定「AntialiasingMode」為「amOn」。**
        + **2.10.12.3 設定「Center」為「True」。**
        + **2.10.12.4 設定「Proportional」為「True」。**
        + **2.10.12.5 設定「Streth」為「True」。**
        + 加程式碼
        ```pascal
        implementation
        //--------------------------------------------------------------------------
        //宣告全域變數 add by HsiupoYeh
        var
          version_str: AnsiString;
          Current_Folder_Path: AnsiString;
          Form1CreateMeshPreview_ImageWidthDiffPx: Integer;
          Form1CreateMeshPreview_ImageHeightDiffPx: Integer;
        //--------------------------------------------------------------------------
        
        {$R *.lfm} 
        ```
     
        ```pascal
        procedure TForm1.FormShow(Sender: TObject);
        begin
          //--------------------------------------------------------------------------
          // 計算經過DPI縮放後的圖片與主視窗的寬高差，用於修補TImage在不可見的時候會自行改變尺寸的bug。修補方式:手動限制尺寸。
          Form1CreateMeshPreview_ImageWidthDiffPx:=Form1.Width-CreateMeshPreview_Image.Width;
          Form1CreateMeshPreview_ImageHeightDiffPx:=Form1.Height-CreateMeshPreview_Image.Height;
          //--------------------------------------------------------------------------
        end;       
        ```
     
        ```pascal
        procedure TForm1.FormResize(Sender: TObject);
        begin
          //--------------------------------------------------------------------------
          // 限制圖片尺寸，來修補圖片尺寸異常的Bug
          if Forward_PageControl.ActivePage = CreateMesh_TabSheet then
          begin
            CreateMeshPreview_Image.Constraints.MaxWidth:=Form1.Width-Form1CreateMeshPreview_ImageWidthDiffPx;
            CreateMeshPreview_Image.Constraints.MaxHeight:=Form1.Height-Form1CreateMeshPreview_ImageHeightDiffPx;
          end;
          //--------------------------------------------------------------------------
        end;  
        ```
      + **2.10.13 拖拉一個「Standard>TGroupBox」到「CreateMeshParameters_GroupBox」中。預設名稱會是「GroupBox1」，修改「Name」為「CreateMeshModelName_GroupBox」。**
        + **2.10.13.1 設定「Anchors」。**  
          + **2.10.13.1.1 「Top anchoring>Slbling」選為「CreateMeshParameters_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
          + **2.10.13.1.2 「Left anchoring>Slbling」選為「CreateMeshParameters_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
          + **2.10.13.1.3 「Right anchoring>Slbling」選為「CreateMeshParameters_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
          + **2.10.13.1.4 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
          + **2.10.13.1.5 設定完後關閉Anchors設定視窗。**
        + **2.10.13.2 設定「Caption」為「模型名稱:」。**
        + **2.10.13.3 設定「Height」為「48」。**
      + **2.10.14 拖拉一個「Standard>TEdit」到「CreateMeshModelName_GroupBox」中。預設名稱會是「Edit1」，修改「Name」為「CreateMeshModelName_Edit」**
        + **2.10.14.1 設定「Anchors」。**  
          + **2.10.14.1.1 「Top anchoring>Slbling」選為「CreateMeshModelName_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**  
          + **2.10.14.1.2 「Left anchoring>Slbling」選為「CreateMeshModelName_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
          + **2.10.14.1.3 「Right anchoring>Slbling」選為「CreateMeshModelName_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
          + **2.10.14.1.4 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
          + **2.10.14.1.5 設定完後關閉Anchors設定視窗。**  
        + **2.10.14.2 設定「Text」為「Demo」。**  
      + **2.10.15 拖拉一個「Standard>TGroupBox」到「CreateMeshParameters_GroupBox」中。預設名稱會是「GroupBox1」，修改「Name」為「CreateMeshColorbarMinMax_GroupBox」。**
        + **2.10.15.1 設定「Anchors」。**  
          + **2.10.15.1.1 「Top anchoring>Slbling」選為「CreateMeshModelName_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選下面的。**
          + **2.10.15.1.2 「Left anchoring>Slbling」選為「CreateMeshModelName_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選右邊的。**  
          + **2.10.15.1.3 「Right anchoring>Slbling」選為「CreateMeshModelName_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
          + **2.10.15.1.4 「Border space」正中間那格填「0」，左邊那格填「0」，右邊那格填「0」。**  
          + **2.10.15.1.5 設定完後關閉Anchors設定視窗。**
        + **2.10.15.2 設定「Caption」為「電阻率色階(min,max)，單位=歐姆米:」。**
        + **2.10.15.3 設定「Height」為「48」。**
      + **2.10.16 拖拉一個「Standard>TEdit」到「CreateMeshColorbarMinMax_GroupBox」中。預設名稱會是「Edit1」，修改「Name」為「CreateMeshColorbarMinMax_Edit」**
        + **2.10.16.1 設定「Anchors」。**  
          + **2.10.16.1.1 「Top anchoring>Slbling」選為「CreateMeshColorbarMinMax_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**  
          + **2.10.16.1.2 「Left anchoring>Slbling」選為「CreateMeshColorbarMinMax_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
          + **2.10.16.1.3 「Right anchoring>Slbling」選為「CreateMeshColorbarMinMax_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
          + **2.10.16.1.4 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
          + **2.10.16.1.5 設定完後關閉Anchors設定視窗。**  
        + **2.10.16.2 設定「Text」為「1,10000」。**  
      + **2.10.17 拖拉一個「Standard>TGroupBox」到「CreateMeshParameters_GroupBox」中。預設名稱會是「GroupBox1」，修改「Name」為「CreateMeshStudyAreaMeshLayerSettings_GroupBox」。**
        + **2.10.17.1 設定「Anchors」。**  
          + **2.10.17.1.1 「Top anchoring>Slbling」選為「CreateMeshColorbarMinMax_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選下面的。**
          + **2.10.17.1.2 「Left anchoring>Slbling」選為「CreateMeshColorbarMinMax_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
          + **2.10.17.1.3 「Right anchoring>Slbling」選為「CreateMeshColorbarMinMax_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
          + **2.10.17.1.4 「Border space」正中間那格填「0」，左邊那格填「0」，右邊那格填「0」。**  
          + **2.10.17.1.5 設定完後關閉Anchors設定視窗。**
        + **2.10.17.2 設定「Caption」為「研究區域內網格設定(地層數,厚度因子,首層厚度)，單位=公尺:」。**
        + **2.10.17.3 設定「Height」為「48」。**
      + **2.10.18 拖拉一個「Standard>TEdit」到「CreateMeshStudyAreaMeshLayerSettings_GroupBox」中。預設名稱會是「Edit1」，修改「Name」為「CreateMeshStudyAreaMeshLayerSettings_Edit」**
        + **2.10.18.1 設定「Anchors」。**  
          + **2.10.18.1.1 「Top anchoring>Slbling」選為「CreateMeshStudyAreaMeshLayerSettings_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**  
          + **2.10.18.1.2 「Left anchoring>Slbling」選為「CreateMeshStudyAreaMeshLayerSettings_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
          + **2.10.18.1.3 「Right anchoring>Slbling」選為「CreateMeshStudyAreaMeshLayerSettings_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
          + **2.10.18.1.4 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
          + **2.10.18.1.5 設定完後關閉Anchors設定視窗。**  
        + **2.10.18.2 設定「Text」為「30,1,1」。**  
      + **2.10.19 拖拉一個「Standard>TGroupBox」到「CreateMeshParameters_GroupBox」中。預設名稱會是「GroupBox1」，修改「Name」為「CreateMeshPaddingMeshLeftSettings_GroupBox」。**
        + **2.10.19.1 設定「Anchors」。**  
          + **2.10.19.1.1 「Top anchoring>Slbling」選為「CreateMeshStudyAreaMeshLayerSettings_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選下面的。**
          + **2.10.19.1.2 「Left anchoring>Slbling」選為「CreateMeshStudyAreaMeshLayerSettings_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
          + **2.10.19.1.3 「Right anchoring>Slbling」選為「CreateMeshStudyAreaMeshLayerSettings_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
          + **2.10.19.1.4 「Border space」正中間那格填「0」，左邊那格填「0」，右邊那格填「0」。**  
          + **2.10.19.1.5 設定完後關閉Anchors設定視窗。**
        + **2.10.19.2 設定「Caption」為「附加區域左側網格設定(層數,寬度因子,首層寬度)，單位=公尺:」。**
        + **2.10.19.3 設定「Height」為「48」。**
      + **2.10.20 拖拉一個「Standard>TEdit」到「CreateMeshPaddingMeshLeftSettings_GroupBox」中。預設名稱會是「Edit1」，修改「Name」為「CreateMeshPaddingMeshLeftSettings_Edit」**
        + **2.10.20.1 設定「Anchors」。**  
          + **2.10.20.1.1 「Top anchoring>Slbling」選為「CreateMeshPaddingMeshLeftSettings_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**  
          + **2.10.20.1.2 「Left anchoring>Slbling」選為「CreateMeshPaddingMeshLeftSettings_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
          + **2.10.20.1.3 「Right anchoring>Slbling」選為「CreateMeshPaddingMeshLeftSettings_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
          + **2.10.20.1.4 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
          + **2.10.20.1.5 設定完後關閉Anchors設定視窗。**  
        + **2.10.20.2 設定「Text」為「5,1.2,-1」。**  
      + **2.10.21 拖拉一個「Standard>TGroupBox」到「CreateMeshParameters_GroupBox」中。預設名稱會是「GroupBox1」，修改「Name」為「CreateMeshPaddingMeshRightSettings_GroupBox」。**
        + **2.10.21.1 設定「Anchors」。**  
          + **2.10.21.1.1 「Top anchoring>Slbling」選為「CreateMeshPaddingMeshLeftSettings_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選下面的。**
          + **2.10.21.1.2 「Left anchoring>Slbling」選為「CreateMeshPaddingMeshLeftSettings_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
          + **2.10.21.1.3 「Right anchoring>Slbling」選為「CreateMeshPaddingMeshLeftSettings_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
          + **2.10.21.1.4 「Border space」正中間那格填「0」，左邊那格填「0」，右邊那格填「0」。**  
          + **2.10.21.1.5 設定完後關閉Anchors設定視窗。**
        + **2.10.21.2 設定「Caption」為「附加區域右側網格設定(層數,寬度因子,首層寬度)，單位=公尺:」。**
        + **2.10.21.3 設定「Height」為「48」。**
      + **2.10.22 拖拉一個「Standard>TEdit」到「CreateMeshPaddingMeshRightSettings_GroupBox」中。預設名稱會是「Edit1」，修改「Name」為「CreateMeshPaddingMeshRightSettings_Edit」**
        + **2.10.22.1 設定「Anchors」。**  
          + **2.10.22.1.1 「Top anchoring>Slbling」選為「CreateMeshPaddingMeshRightSettings_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**  
          + **2.10.22.1.2 「Left anchoring>Slbling」選為「CreateMeshPaddingMeshRightSettings_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
          + **2.10.22.1.3 「Right anchoring>Slbling」選為「CreateMeshPaddingMeshRightSettings_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
          + **2.10.22.1.4 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
          + **2.10.22.1.5 設定完後關閉Anchors設定視窗。**  
        + **2.10.22.2 設定「Text」為「5,1.2,-1」。**  
      + **2.10.23 拖拉一個「Standard>TGroupBox」到「CreateMeshParameters_GroupBox」中。預設名稱會是「GroupBox1」，修改「Name」為「CreateMeshPaddingMeshBottomSettings_GroupBox」。**
        + **2.10.23.1 設定「Anchors」。**  
          + **2.10.23.1.1 「Top anchoring>Slbling」選為「CreateMeshPaddingMeshRightSettings_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選下面的。**
          + **2.10.23.1.2 「Left anchoring>Slbling」選為「CreateMeshPaddingMeshRightSettings_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
          + **2.10.23.1.3 「Right anchoring>Slbling」選為「CreateMeshPaddingMeshRightSettings_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
          + **2.10.23.1.4 「Border space」正中間那格填「0」，左邊那格填「0」，右邊那格填「0」。**  
          + **2.10.23.1.5 設定完後關閉Anchors設定視窗。**
        + **2.10.23.2 設定「Caption」為「附加區域下方網格設定(層數,寬度因子,首層寬度)，單位=公尺:」。**
        + **2.10.23.3 設定「Height」為「48」。**
      + **2.10.24 拖拉一個「Standard>TEdit」到「CreateMeshPaddingMeshBottomSettings_GroupBox」中。預設名稱會是「Edit1」，修改「Name」為「CreateMeshPaddingMeshBottomSettings_Edit」**
        + **2.10.24.1 設定「Anchors」。**  
          + **2.10.24.1.1 「Top anchoring>Slbling」選為「CreateMeshPaddingMeshBottomSettings_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**  
          + **2.10.24.1.2 「Left anchoring>Slbling」選為「CreateMeshPaddingMeshBottomSettings_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
          + **2.10.24.1.3 「Right anchoring>Slbling」選為「CreateMeshPaddingMeshBottomSettings_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
          + **2.10.24.1.4 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
          + **2.10.24.1.5 設定完後關閉Anchors設定視窗。**  
        + **2.10.24.2 設定「Text」為「5,1.2,-1」。**  
      + **2.10.25 拖拉一個「Standard>TGroupBox」到「CreateMeshParameters_GroupBox」中。預設名稱會是「GroupBox1」，修改「Name」為「CreateMeshSurfaceNodeSettings_GroupBox」。**
        + **2.10.25.1 設定「Anchors」。**  
          + **2.10.25.1.1 「Top anchoring>Slbling」選為「CreateMeshPaddingMeshBottomSettings_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選下面的。**
          + **2.10.25.1.2 「Left anchoring>Slbling」選為「CreateMeshPaddingMeshBottomSettings_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
          + **2.10.25.1.3 「Bottom anchoring>Slbling」選為「CreateMeshPaddingMeshRightSettings_GroupBox:TGroupBox」。勾選「Bottom anchoring>Enable」為打勾。選擇「Bottom anchoring」右側三個鈕選下面的。**  
          + **2.10.25.1.4 「Border space」正中間那格填「0」，右邊那格填「3」，下面那格填「3」。**  
          + **2.10.25.1.5 設定完後關閉Anchors設定視窗。**
        + **2.10.25.2 設定「Caption」為「地表節點(X,Z,電極索引)，單位=公尺:」。**
      + **2.10.26 拖拉一個「Standard>TMemo」到「CreateMeshSurfaceNodeSettings_GroupBox」中。預設名稱會是「Memo1」，修改「Name」為「CreateMeshSurfaceNodeSettings_Memo」**
        + **2.10.26.1 設定「Anchors」。**  
          + **2.10.26.1.1 「Top anchoring>Slbling」選為「CreateMeshSurfaceNodeSettings_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**  
          + **2.10.26.1.2 「Left anchoring>Slbling」選為「CreateMeshSurfaceNodeSettings_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
          + **2.10.26.1.3 「Right anchoring>Slbling」選為「CreateMeshSurfaceNodeSettings_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**
          + **2.10.26.1.4 「Bottom anchoring>Slbling」選為「CreateMeshSurfaceNodeSettings_GroupBox:TGroupBox」。勾選「Bottom anchoring>Enable」為打勾。選擇「Bottom anchoring」右側三個鈕選下面的。**  
          + **2.10.26.1.5 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」，下面那格填「3」。**  
          + **2.10.26.1.6 設定完後關閉Anchors設定視窗。**  
        + **2.10.26.2 設定「Lines」為以下文字。**
        ```
        0,813.007,1
        1,813.066,2
        2,813.068,3
        3,813.185,4
        4,812.873,5
        5,813.145,6
        6,813.649,7
        7,813.882,8
        8,813.713,9
        9,813.868,10
        10,814.04,11
        11,814.393,12
        12,814.382,13
        13,814.489,14
        14,814.349,15
        15,814.126,16
        16,814.035,17
        17,813.696,18
        18,813.977,19
        19,814.02,20
        20,813.968,21
        21,813.98,22
        22,814.006,23
        23,814.344,24
        24,814.124,25
        25,814.28,26
        26,814.222,27
        27,814.09,28
        28,813.919,29
        29,813.723,30
        30,813.634,31
        31,813.444,32
        32,813.197,33
        33,813.406,34
        34,813.3,35
        35,813.211,36
        36,813.197,37
        37,812.852,38
        38,812.893,39
        39,812.5795,40
        40,812.266,41
        41,812.3885,42
        42,812.511,43
        43,812.451,44
        44,812.158,45
        45,812.278,46
        46,812.203,47
        47,812.181,48
        48,812.99,49
        49,813.05,50
        50,814.5972,51
        51,null,0
        52,null,0
        53,null,0
        54,817.008,52
        55,816.981,53
        56,816.385,54
        57,816.409,55
        58,816.421,56
        59,817.016,57
        60,817.308,58
        61,818.08,59
        62,818.571,60
        63,819.079,61
        64,819.638,62
        65,820.346,63
        66,821.833,64
        67,822.459,65
        ```
        + **2.10.26.3 設定「ScrollBars」為「ssBoth」。**
        + **2.10.26.4 設定「WordWrap」為「False」。**
      + **2.10.27 拖拉一個「Standard>TGroupBox」到「CreateMeshParameters_GroupBox」中。預設名稱會是「GroupBox1」，修改「Name」為「CreateMeshModifyMeshResistivitySettings_GroupBox」。**
        + **2.10.27.1 設定「Anchors」。**  
          + **2.10.27.1.1 「Top anchoring>Slbling」選為「CreateMeshPaddingMeshBottomSettings_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選下面的。**
          + **2.10.27.1.2 「Left anchoring>Slbling」選為「CreateMeshSurfaceNodeSettings_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選右邊的。**
          + **2.10.27.1.3 「Right anchoring>Slbling」選為「CreateMeshPaddingMeshBottomSettings_GroupBox:TGroupBox」。維持「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
          + **2.10.27.1.4 「Bottom anchoring>Slbling」選為「CreateMeshParameters_GroupBox:TGroupBox」。勾選「Bottom anchoring>Enable」為打勾。選擇「Bottom anchoring」右側三個鈕選下面的。**  
          + **2.10.27.1.5 「Border space」正中間那格填「0」，左邊那格填「3」，下面那格填「3」。**  
          + **2.10.27.1.6 設定完後關閉Anchors設定視窗。**
        + **2.10.27.2 設定「Caption」為「電阻率規則(xmin,xmax,zmin,zmax,newRes):」。**
      + **2.10.28 拖拉一個「Standard>TMemo」到「CreateMeshModifyMeshResistivitySettings_GroupBox」中。預設名稱會是「Memo1」，修改「Name」為「CreateMeshModifyMeshResistivitySettings_Memo」**
        + **2.10.28.1 設定「Anchors」。**  
          + **2.10.28.1.1 「Top anchoring>Slbling」選為「CreateMeshModifyMeshResistivitySettings_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**  
          + **2.10.28.1.2 「Left anchoring>Slbling」選為「CreateMeshModifyMeshResistivitySettings_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
          + **2.10.28.1.3 「Right anchoring>Slbling」選為「CreateMeshModifyMeshResistivitySettings_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**
          + **2.10.28.1.4 「Bottom anchoring>Slbling」選為「CreateMeshModifyMeshResistivitySettings_GroupBox:TGroupBox」。勾選「Bottom anchoring>Enable」為打勾。選擇「Bottom anchoring」右側三個鈕選下面的。**  
          + **2.10.28.1.5 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」，下面那格填「3」。**  
          + **2.10.28.1.6 設定完後關閉Anchors設定視窗。**  
        + **2.10.28.2 設定「Lines」為以下文字。**
        ```
        -9999, 9999, -9999, 9999, 100
        15, 25, 795, 805, 10
        50, 60, 795, 805, 3000
        ```
        + **2.10.26.3 設定「ScrollBars」為「ssBoth」。**
        + **2.10.26.4 設定「WordWrap」為「False」。**
    + 2.11 第2個分頁「模型網格建立參數」(CreateMeshSettings_TabSheet):      
      + **2.11.1 拖拉一個「Standard>TGroupBox」到「CreateMeshSettings_TabSheet」中。預設名稱會是「GroupBox1」，修改「Name」為「CreateMeshSettingsCmdLog_GroupBox」。**
        + **2.11.1.1 設定「Align」為「alRight」。**
        + **2.11.1.2 設定「BorderSpacing>Left」為「3」。**
        + **2.11.1.3 設定「BorderSpacing>Right」為「1」。**
        + **2.11.1.4 設定「Caption」為「建立模型網格運行紀錄:」。** 
        + **2.11.1.5 設定「Width」為「500」。**
      + **2.11.2 拖拉一個「Standard>TMemo」到「CreateMeshSettingsCmdLog_GroupBox」中。預設名稱會是「Memo1」，修改「Name」為「CreateMeshSettingsCmdLog_Memo」**
        + **2.11.2.1 設定「Anchors」。**  
          + **2.11.2.1.1 「Top anchoring>Slbling」選為「CreateMeshSettingsCmdLog_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。選擇「Top anchoring」右側三個鈕選上面的。** 
          + **2.11.2.1.2 「Left anchoring>Slbling」選為「CreateMeshSettingsCmdLog_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
          + **2.11.2.1.3 「Right anchoring>Slbling」選為「CreateMeshSettingsCmdLog_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
          + **2.11.2.1.4 「Bottom anchoring>Slbling」選為「CreateMeshSettingsCmdLog_GroupBox:TGroupBox」。勾選「Bottom anchoring>Enable」為打勾。選擇「Bottom anchoring」右側三個鈕選下面的。** 
          + **2.11.2.1.5 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」，下面那格填「3」。**  
          + **2.11.2.1.6 設定完後關閉Anchors設定視窗。**  
        + **2.11.2.2 設定「Color」為「clBlack」。**
        + **2.11.2.3 設定「Font>Color」為「clLime」。**
        + **2.11.2.4 設定「Lines」清空。**
        + **2.11.2.5 設定「ReadOnly」為「True」。**
        + **2.11.2.6 設定「ScrollBars」為「ssBoth」。**
        + **2.11.2.7 設定「WordWrap」為「False」。**
      + **2.11.3 拖拉一個「Standard>TGroupBox」到「CreateMeshSettings_TabSheet」中。預設名稱會是「GroupBox1」，修改「Name」為「CreateMeshSettingsDefaultJson_GroupBox」**
        + **2.11.3.1 設定「Anchors」。**  
          + **2.11.3.1.1 「Top anchoring>Slbling」選為「CreateMeshSettings_TabSheet:TTabSheet」。維持「Top anchoring>Enable」為打勾。選擇「Top anchoring」右側三個鈕選上面的。** 
          + **2.11.3.1.2 「Left anchoring>Slbling」選為「CreateMeshSettings_TabSheet:TTabSheet」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
          + **2.11.3.1.3 「Right anchoring>Slbling」選為「CreateMeshSettingsCmdLog_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選左邊的。**  
          + **2.11.3.1.4 設定完後關閉Anchors設定視窗。**  
        + **2.11.3.2 設定「Caption」為「預設參數(Default_CreateAndModifyMeshSettings.json):」。**
        + **2.11.3.3 設定「Height」為「200」。**
      + **2.11.4 拖拉一個「Standard>TMemo」到「CreateMeshSettingsDefaultJson_GroupBox」中。預設名稱會是「Memo1」，修改「Name」為「CreateMeshSettingsDefaultJson_Memo」**
        + **2.11.4.1 設定「Anchors」。**  
          + **2.11.4.1.1 「Top anchoring>Slbling」選為「CreateMeshSettingsDefaultJson_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。選擇「Top anchoring」右側三個鈕選上面的。** 
          + **2.11.4.1.2 「Left anchoring>Slbling」選為「CreateMeshSettingsDefaultJson_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
          + **2.11.4.1.3 「Right anchoring>Slbling」選為「CreateMeshSettingsDefaultJson_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
          + **2.11.4.1.4 「Bottom anchoring>Slbling」選為「CreateMeshSettingsDefaultJson_GroupBox:TGroupBox」。勾選「Bottom anchoring>Enable」為打勾。選擇「Bottom anchoring」右側三個鈕選下面的。** 
          + **2.11.4.1.5 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」，下面那格填「3」。**  
          + **2.11.4.1.6 設定完後關閉Anchors設定視窗。**  
        + **2.11.4.2 設定「Lines」清空。**
        + **2.11.4.3 設定「ReadOnly」為「True」。**
        + **2.11.4.4 設定「ScrollBars」為「ssBoth」。**
        + **2.11.4.5 設定「WordWrap」為「False」。**
      + **2.11.5 拖拉一個「Standard>TGroupBox」到「CreateMeshSettings_TabSheet」中。預設名稱會是「GroupBox1」，修改「Name」為「CreateMeshSettingsNowJson_GroupBox」**
        + **2.11.5.1 設定「Anchors」。**  
          + **2.11.5.1.1 「Top anchoring>Slbling」選為「CreateMeshSettingsDefaultJson_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。選擇「Top anchoring」右側三個鈕選下面的。** 
          + **2.11.5.1.2 「Left anchoring>Slbling」選為「CreateMeshSettingsDefaultJson_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
          + **2.11.5.1.3 「Right anchoring>Slbling」選為「CreateMeshSettingsDefaultJson_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
          + **2.11.5.1.4 設定完後關閉Anchors設定視窗。**  
        + **2.11.5.2 設定「Caption」為「目前參數(CreateAndModifyMeshSettings.json):」。**
        + **2.11.5.3 設定「Height」為「200」。**
      + **2.11.6 拖拉一個「Standard>TMemo」到「CreateMeshSettingsNowJson_GroupBox」中。預設名稱會是「Memo1」，修改「Name」為「CreateMeshSettingsNowJson_Memo」**
        + **2.11.6.1 設定「Anchors」。**  
          + **2.11.6.1.1 「Top anchoring>Slbling」選為「CreateMeshSettingsNowJson_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。選擇「Top anchoring」右側三個鈕選上面的。** 
          + **2.11.6.1.2 「Left anchoring>Slbling」選為「CreateMeshSettingsNowJson_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
          + **2.11.6.1.3 「Right anchoring>Slbling」選為「CreateMeshSettingsNowJson_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
          + **2.11.6.1.4 「Bottom anchoring>Slbling」選為「CreateMeshSettingsNowJson_GroupBox:TGroupBox」。勾選「Bottom anchoring>Enable」為打勾。選擇「Bottom anchoring」右側三個鈕選下面的。** 
          + **2.11.6.1.5 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」，下面那格填「3」。**  
          + **2.11.6.1.6 設定完後關閉Anchors設定視窗。**  
        + **2.11.6.2 設定「Lines」清空。**
        + **2.11.6.3 設定「ReadOnly」為「True」。**
        + **2.11.6.4 設定「ScrollBars」為「ssBoth」。**
        + **2.11.6.5 設定「WordWrap」為「False」。**
      + **2.11.7 拖拉一個「Standard>TGroupBox」到「CreateMeshSettings_TabSheet」中。預設名稱會是「GroupBox1」，修改「Name」為「CreateMeshSettingsReadme_GroupBox」**
        + **2.11.7.1 設定「Anchors」。**  
          + **2.11.7.1.1 「Top anchoring>Slbling」選為「CreateMeshSettingsNowJson_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。選擇「Top anchoring」右側三個鈕選下面的。** 
          + **2.11.7.1.2 「Left anchoring>Slbling」選為「CreateMeshSettingsNowJson_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
          + **2.11.7.1.3 「Right anchoring>Slbling」選為「CreateMeshSettingsNowJson_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**
          + **2.11.7.1.4 「Bottom anchoring>Slbling」選為「CreateMeshSettings_TabSheet:TTabSheet」。勾選「Bottom anchoring>Enable」為打勾。選擇「Bottom anchoring」右側三個鈕選右邊的。** 
          + **2.11.7.1.5 設定完後關閉Anchors設定視窗。**  
        + **2.11.7.2 設定「Caption」為「說明:」。**
      + **2.11.8 拖拉一個「Standard>TMemo」到「CreateMeshSettingsReadme_GroupBox」中。預設名稱會是「Memo1」，修改「Name」為「CreateMeshSettingsReadme_Memo」**
        + **2.11.8.1 設定「Anchors」。**  
          + **2.11.8.1.1 「Top anchoring>Slbling」選為「CreateMeshSettingsReadme_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。選擇「Top anchoring」右側三個鈕選上面的。** 
          + **2.11.8.1.2 「Left anchoring>Slbling」選為「CreateMeshSettingsReadme_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
          + **2.11.8.1.3 「Right anchoring>Slbling」選為「CreateMeshSettingsReadme_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
          + **2.11.8.1.4 「Bottom anchoring>Slbling」選為「CreateMeshSettingsReadme_GroupBox:TGroupBox」。勾選「Bottom anchoring>Enable」為打勾。選擇「Bottom anchoring」右側三個鈕選下面的。** 
          + **2.11.8.1.5 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」，下面那格填「3」。**  
          + **2.11.8.1.6 設定完後關閉Anchors設定視窗。**  
        + **2.11.8.2 設定「Lines」清空。**
        + **2.11.8.3 設定「ReadOnly」為「True」。**
        + **2.11.8.4 設定「ScrollBars」為「ssBoth」。**
        + **2.11.6.5 設定「WordWrap」為「False」。**
    + 2.12 第3個分頁「順推模擬運算」(ForwardModeling_TabSheet):
      + **2.12.1 拖拉一個「Standard>TToolBar」到「ForwardModeling_TabSheet」中。預設名稱會是「ToolBar1」，修改「Name」為「ForwardModeling_ToolBar」。**
      + **2.12.2 設定「BorderSpacing>Top」為「1」。**
      + **2.12.3 設定「Edge>Borders>ebBottom」為「True」。**
      + **2.12.4 設定「Edge>Borders>ebLeft」為「True」。**
      + **2.12.5 設定「Edge>Borders>ebRight」為「True」。**
      + **2.12.6 設定「ShowCaptions」為「True」。** 
      + **2.12.7 在「ToolBar1」元件上按「右鍵>New CheckButton」，會增加一個新按鈕。預設名稱會是「ToolButton1」，修改「Name」為「ForwardModelingRun_ToolButton」**
        + **2.12.7.1 設定「Caption」為「運行順推模擬」。**
        + **2.12.7.2 設定「CreateMeshRun_ToolButton」的「Event」頁面下「OnClick」為如下程式碼。**
        ```pascal

        ```
      + **2.12.8 在「ToolBar1」元件上按「右鍵>New CheckButton」，會增加一個新按鈕。預設名稱會是「ToolButton1」，修改「Name」為「ForwardModelingOpenOutputFolder_ToolButton」**
        + **2.12.8.1 設定「Caption」為「開啟輸出資料夾」。**
        + **2.12.8.2 設定「CreateMeshOpenOutputFolder_ToolButton」的「Event」頁面下「OnClick」為如下程式碼。**
        ```pascal

        ```   
      + **2.12.9 拖拉一個「Standard>TGroupBox」到「ForwardModeling_TabSheet」中。預設名稱會是「GroupBox1」，修改「Name」為「ForwardModelingParameters_GroupBox」。**
        + **2.12.9.1 設定「Align」為「alRight」。**
        + **2.12.9.2 設定「BorderSpacing>Left」為「3」。**
        + **2.12.9.3 設定「BorderSpacing>Right」為「1」。**
        + **2.12.9.4 設定「Caption」為「參數設定:」。** 
        + **2.12.9.5 設定「Width」為「500」。**
      + **2.12.10 拖拉一個「Standard>TGroupBox」到「ForwardModeling_TabSheet」中。預設名稱會是「GroupBox1」，修改「Name」為「ForwardModelingPreview_GroupBox」。**
        + **2.12.10.1 設定「Align」為「alClient」。**
        + **2.12.10.2 設定「Caption」為「模型預覽:」。** 
      + **2.12.11 拖拉一個「Additional>TImage」到「ForwardModeling_TabSheet」中。預設名稱會是「Image1」，修改「Name」為「ForwardModelingPreview_Image」。**
        + **2.12.11.1 設定「Align」為「alClient」。**
        + **2.12.11.2 設定「AntialiasingMode」為「amOn」。**
        + **2.12.11.3 設定「Center」為「True」。**
        + **2.12.11.4 設定「Proportional」為「True」。**
        + **2.12.11.5 設定「Streth」為「True」。**
        + 加程式碼
        ```pascal
        procedure TForm1.FormResize(Sender: TObject);
        begin
          if Forward_PageControl.ActivePage = CreateMesh_TabSheet then
          begin
            CreateMeshPreview_Image.Constraints.MaxWidth:=Form1.Width-(1100-572); //原本Form1寬度=1100，原本CreateMeshPreview_Image寬度=572，可知其他占用空間為(1100-572)
            CreateMeshPreview_Image.Constraints.MaxHeight:=Form1.Height-(600-471); //原本Form1高度=600，原本CreateMeshPreview_Image高度=471，可知其他占用空間為(600-471)
          end;
          if Forward_PageControl.ActivePage = ForwardModeling_TabSheet then
          begin
            ForwardModelingPreview_Image.Constraints.MaxWidth:=Form1.Width-(1100-572); //原本Form1寬度=1100，原本CreateMeshPreview_Image寬度=572，可知其他占用空間為(1100-572)
            ForwardModelingPreview_Image.Constraints.MaxHeight:=Form1.Height-(600-471); //原本Form1高度=600，原本CreateMeshPreview_Image高度=471，可知其他占用空間為(600-471)
          end;
        end;
        ```
        + 加程式碼
        ```pascal
        procedure TForm1.Forward_PageControlChange(Sender: TObject);
        begin
          if Forward_PageControl.ActivePage = CreateMesh_TabSheet then
          begin
            CreateMeshPreview_Image.Constraints.MaxWidth:=Form1.Width-(1100-572); //原本Form1寬度=1100，原本CreateMeshPreview_Image寬度=572，可知其他占用空間為(1100-572)
            CreateMeshPreview_Image.Constraints.MaxHeight:=Form1.Height-(600-471); //原本Form1高度=600，原本CreateMeshPreview_Image高度=471，可知其他占用空間為(600-471)
          end;
          if Forward_PageControl.ActivePage = ForwardModeling_TabSheet then
          begin
            ForwardModelingPreview_Image.Constraints.MaxWidth:=Form1.Width-(1100-572); //原本Form1寬度=1100，原本CreateMeshPreview_Image寬度=572，可知其他占用空間為(1100-572)
            ForwardModelingPreview_Image.Constraints.MaxHeight:=Form1.Height-(600-471); //原本Form1高度=600，原本CreateMeshPreview_Image高度=471，可知其他占用空間為(600-471)
          end;
        end 
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
