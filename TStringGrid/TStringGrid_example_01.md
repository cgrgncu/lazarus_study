# TStringGrid_example_01 開發筆記  
+ 作者: HsiupoYeh  
+ 更新日期: 2025-10-10

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
- Windosw作業系統。  
- 可運行64位元應用程式。  


### 備忘  
+ 預設運作平台: Default，應該就是win10 64bit。
+ 依照官網建議，為了避免DPI問題，各項操作建議(安裝IDE、設計窗體、編寫程式碼)在縮放大小100%的情況下進行。編譯出來的軟體可以拿去其他DPI設定下的電腦測試。
  + 使用解析度: 1920x1080
  + 縮放大小: 100%

### 開發紀錄(製作APP Icon)  
+ 使用工具1: https://icon.kitchen/  
  + IOS Icon  
  + Text: 「TStringGrid」  
  + BackGround>Type=「Gradient」  
  + More>Badge = 「example01」  
+ 再使用工具2: https://tfin.ch/iosicon.html  
  + 輸出有圓角的圖片  
+ 再使用工具3: PNG轉ICO線上工具 例如:https://www.aconvert.com/tw/icon/png-to-ico/
  + 需要的多解析度包含:  
    + 全部清單中有的
+ ICO下載位置: [右鍵另存](https://raw.githubusercontent.com/cgrgncu/lazarus_study/refs/heads/main/TStringGrid/ico/TStringGrid_example_01.ico) 

> ICO:  
>  ![image](https://raw.githubusercontent.com/cgrgncu/lazarus_study/refs/heads/main/TStringGrid/ico/TStringGrid_example_01.ico)  

## 開發紀錄(基礎框架部分)
1. 建立新專案。  
    + **1.1 按下「Project>New Project ...」。**  
    + **1.2 選擇「Project>Application 」之後按「OK」。**  
2. 獨立把專案存到指定資料夾中，以利日後管理。大部分自訂程式碼都會是相對路徑，未來只需要帶走這個資料夾即可在其他地方重新使用。  
    + **2.1 在桌面建立「TStringGrid_example_01」資料夾**  
    + **2.2 回到Lazarus，按下「Project>Save Project As」。**  
    + **2.3 將Project檔案(.lpi)存到桌面上的「TStringGrid_example_01｣資料夾中，檔案命名為「TStringGrid_example_01.lpi」。**  
3. 專案設定需要調整。  
    + **3.1 選擇「Project>Project Options ...」設定「Application」頁面下利用「Load Icon 」按鈕更換自訂圖示。**  
      + 3.1.1 說明: 程式圖示。  
    + 3.2 檢查「Application」頁面下「Execution Level 」為「As Invoker」。  
      + 3.2.1 說明: 「As Invoker、Highest Available」都是普通權限就可以執行，「requireAdministrator」是執行前請求獲得管理者權限才能執行。
    + **3.3 選擇「Project>Project Options ...」設定「Compiler Options」頁面下，按下「Build modes」最右邊的「...」按鈕。再按「Create Debug and Release modes」，並將「Release」勾選「Active」，再按「OK」，再按「OK」**
      + 3.3.1 說明: 用Debug有時候防毒會報錯。  
4. 預設狀況下就有一個Form元件存在，預設名稱叫做「Form1」，先做一些基礎設定以利後續排版。  
    + 4.1 設定「Form1」的「properties」。這裡主要列出常用的一些。  
      + 4.1.1 檢查「Form1」的「properties」頁面下「BorderIcons」為「[biSystemMenu,biMinimize,biMaximize]」。  
        + 4.1.1.1 說明:設定窗體右上角的按鈕如果存在的話要不要被啟用。「[biSystemMenu,biMinimize,biMaximize]」表示這三個被打勾。  
      + 4.1.2 檢查「Form1」的「properties」頁面下「BorderStyle」為「bsSizeable」。  
        + 4.1.2.1 說明:設定窗體的樣式。可依需求嘗試不同風格決定是否符合需求。  
      + **4.1.2 設定「Form1」的「properties」頁面下「Caption」為「TStringGrid_example_01」。**  
        + 4.1.2.1 說明:設定窗體的標題列文字。  
      + **4.1.3 設定「Form1」的「properties」頁面下「Color」為「clWindow」。**  
        + 4.1.3.1 說明:設定成白色使工具列元件底色是白色。  
      + **4.1.4 設定「Form1」的「properties」頁面下「Height」為「600」。**  
        + 4.1.4.1 說明:設定窗體的外框高度。  
      + **4.1.5 設定「Form1」的「properties」頁面下「Position」為「poScreenCenter」。**  
        + 4.1.5.1 說明:設定窗體的出現位置，建議是用「poScreenCenter」，在螢幕中央。  
      + **4.1.6 設定「Form1」的「properties」頁面下「Width」為「800」。**  
        + 4.1.6.1 說明:設定窗體的外框寬度。
      + **4.1.7 設定「Form1」的「Event」頁面下的「OnCreate」為如下程式碼。**
        ```pascal
        procedure TForm1.FormCreate(Sender: TObject);
        begin
          //--------------------------------------------------------------------------
          // 在這裡對全域變數進行初始化
          version_str := 'v20251010a';
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
    + **5.2 點兩下元件，進入編輯模式，建立第一層第一個選單「說明(&H)」，「Caption」設為「說明(&H)」，「Name」設為「MainMenu1_1」。**  
    + **5.3 繼續建立子選單「說明(&H)->關於我(&A)」，「Caption」設為「關於我(&A)」，「Name」設為「MainMenu1_1_1」。**  
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

### 開發紀錄(主頁面)  
+ 1.1 拖拉一個「Standard>TMemo」到「Form1」中。預設名稱會是「Memo1」，修改「Name」為「Log_Memo」。
  + 1.1.1 設定「Align」為「alBottom」。
  + 1.1.2 設定「Height」為「300」。
  + 1.1.3 設定「Lines」清空。
  + 1.1.4 設定「ReadOnly」為「True」。
  + 1.1.5 設定「ScrollBars」為「ssBoth」。
  + 1.1.6 設定「WordWrap」為「False」。
+ 1.2 拖拉一個「Additional>TStringGrid」到「Form1」中。預設名稱會是「StringGrid1」。
  + 1.1.1 設定「Align」為「alClient」。
  + 1.1.2 設定「ColCount」為「100」。
  + 1.1.3 設定「MouseWheelOption」為「mwGrid」。
  + 1.1.4 設定「Options>goColSizing」為「True」。
  + 1.1.5 設定「Options>goDblClickAutoSize」為「True」。
  + 1.1.6 設定「RowCount」為「100」。
+ 1.3 拖拉一個「Dialogs>TOpenDialog」到「Form1」中。預設名稱會是「OpenDialog1」。
+ 1.4 選擇「MainMenu1」，進入編輯模式，建立第一層第二個選單「開啟檔案」，「Caption」設為「開啟檔案(&O)」，「Name」設為「MainMenu1_2」。
  + 1.4.1 設定「MainMenu1_2」的「Event」。
        + **1.4.1.1 設定「MainMenu1_2」的「Event」頁面下「OnClick」為如下程式碼。**  
        ```pascal  
        procedure TForm1.MainMenu1_1_1Click(Sender: TObject);
        var
          temp_str: AnsiString;
        begin
          temp_str:='作者: HsiupoYeh.'+#13#10+'程式版本: '+version_str;
          Application.MessageBox(PChar(temp_str),'關於我',64);
        end;  
        ``` 
