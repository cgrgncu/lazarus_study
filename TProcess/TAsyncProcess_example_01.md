# TAsyncProcess_example_01 開發筆記  
+ 作者: HsiupoYeh  
+ 更新日期: 2024-11-05

## 開發環境  
+ Windows 10 22H2 64bit 企業版 繁體中文(SW_DVD9_Win_Pro_10_22H2.19_64BIT_ChnTrad_Pro_Ent_EDU_N_MLF_X23-74684.ISO)  
+ Lazarus 3.6 (lazarus-3.6-fpc-3.2.2-win64.exe)
  + 有需要可以用「anchordockingdsgn 1.0」、「dockedformediteor 0.0」固定IDE。

## 運行需求  
- Windosw作業系統。  
- 可運行64位元應用程式。  
- 允許程式通過防火牆。  
- 預設使用Port: 18002。  

### 備忘  
+ 預設運作平台: Default，應該就是win10 64bit。
+ 依照官網建議，為了避免DPI問題，各項操作建議(安裝IDE、設計窗體、編寫程式碼)在縮放大小100%的情況下進行。編譯出來的軟體可以拿去其他DPI設定下的電腦測試。
  + 使用解析度: 1920x1080
  + 縮放大小: 100%

### 開發紀錄(製作APP Icon)  
+ 使用工具1: https://icon.kitchen/  
  + IOS Icon  
  + Text: 「TProcess」  
  + BackGround>Type=「Gradient」  
  + More>Badge = 「example01」  
+ 再使用工具2: https://tfin.ch/iosicon.html  
  + 輸出有圓角的圖片  
+ 再使用工具3: PNG轉ICO線上工具 例如:https://www.aconvert.com/tw/icon/png-to-ico/
  + 需要的多解析度包含:  
    + 全部清單中有的
+ ICO下載位置: [右鍵另存](https://raw.githubusercontent.com/cgrgncu/lazarus_study/refs/heads/main/TProcess/ico/TAsyncProcess_example_01.ico) 

> ICO:  
>  ![image](https://raw.githubusercontent.com/cgrgncu/lazarus_study/refs/heads/main/TProcess/ico/TAsyncProcess_example_01.ico)  

## 開發紀錄(基礎框架部分)
1. 建立新專案。  
    + **1.1 按下「Project>New Project ...」。**  
    + **1.2 選擇「Project>Application 」之後按「OK」。**  
2. 獨立把專案存到指定資料夾中，以利日後管理。大部分自訂程式碼都會是相對路徑，未來只需要帶走這個資料夾即可在其他地方重新使用。  
    + **2.1 在桌面建立「TAsyncProcess_example_01」資料夾**  
    + **2.2 回到Lazarus，按下「Project>Save Project As」。**  
    + **2.3 將Project檔案(.lpi)存到桌面上的「R2MS_Lite_API_Server｣資料夾中，檔案命名為「TAsyncProcess_example_01.lpi」。**  
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
      + **4.1.2 設定「Form1」的「properties」頁面下「Caption」為「TAsyncProcess_example_01」。**  
        + 4.1.2.1 說明:設定窗體的標題列文字。  
      + **4.1.3 設定「Form1」的「properties」頁面下「Color」為「clWindow」。**  
        + 4.1.3.1 說明:設定成白色使工具列元件底色是白色。  
      + **4.1.4 設定「Form1」的「properties」頁面下「Height」為「600」。**  
        + 4.1.4.1 說明:設定窗體的外框高度。  
      + **4.1.5 設定「Form1」的「properties」頁面下「Position」為「poScreenCenter」。**  
        + 4.1.5.1 說明:設定窗體的出現位置，建議是用「poScreenCenter」，在螢幕中央。  
      + **4.1.6 設定「Form1」的「properties」頁面下「Width」為「800」。**  
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
          version_str: AnsiString = 'v20241105a';
          //--  
        ```  
        + **5.3.1.2 設定「MainMenu1_1_1」的「Event」頁面下「OnClick」為如下程式碼。**  
        ```pascal  
        procedure TForm1.MainMenu1_1_1Click(Sender: TObject);
        var
          temp_str: AnsiString;
        begin
          temp_str:='作者: HsiuPoYeh.'+#13#10+'程式版本: '+version_str;
          Application.MessageBox(PChar(temp_str),'關於我',64);
        end;  
        ```  

### 開發紀錄(主頁面)  
1. 外部應用程式(External Program):  
    + **1.1 拖拉一個「Standard>TLabel」到「Form1」中。預設名稱會是「Label1」。**  
      + **1.1.1 設定「Anchors」。**  
        + **1.1.1.1 「Top anchoring>Slbling」選為「Form1:TForm1」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**  
        + **1.1.1.2 「Left anchoring>Slbling」選為「Form1:TForm1」。維持「Left anchoring>Enable」為打勾。維持「Left anchoring」右側三個鈕選左邊的。**  
        + **1.1.1.3 「Border space」正中間那格填「20」。**  
        + **1.1.1.4 設定完後關閉Anchors設定視窗。**  
      + **1.1.2 設定「Caption」為「外部應用程式:」。**  
      + **1.1.3 設定「Name」為「ExternalProgram_Label」。**  
    + **1.2 拖拉一個「Standard>TEdit」到「Form1」中。預設名稱會是「Edit1」。**  
      + **1.2.1 設定「Anchors」。**  
        + **1.2.1.1 「Top anchoring>Slbling」選為「Form1:TForm1」。維持「Top anchoring>Enable」為打勾。選擇「Top anchoring」右側三個鈕選上面的。**  
        + **1.2.1.2 「Left anchoring>Slbling」選為「ExternalProgram_Label:TLabel」。維持「Left anchoring>Enable」為打勾。維持「Left anchoring」右側三個鈕選右邊的。**  
        + **1.2.1.3 「Right anchoring>Slbling」選為「Form1:TForm1」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
        + **1.2.1.4 「Border space」正中間那格填「10」，上面那格填「7」，右邊那格填「20」。**  
        + **1.2.1.5 設定完後關閉Anchors設定視窗。**  
      + **1.2.2 設定「Name」為「ExternalProgram_Edit」。**  
      + **1.2.3 設定「Text」為「cmd.exe」。**
2. 外部應用程式參數(External Program Parameters):  
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
3. 外部應用程式按鈕(External Program Button):  
    + **3.1 拖拉一個「Standard>TButton」到「Form1」中。預設名稱會是「Button1」。**  
      + **3.1.1 設定「Anchors」。**  
        + **3.1.1.1 「Top anchoring>Slbling」選為「ExternalProgramParameters_Edit:TEdit」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選下面的。**  
        + **3.1.1.2 「Left anchoring>Slbling」選為「Form1:TForm1」。維持「Left anchoring>Enable」為打勾。維持「Left anchoring」右側三個鈕選左邊的。**  
        + **3.1.1.3 「Border space」正中間那格填「20」。**  
        + **3.1.1.4 設定完後關閉Anchors設定視窗。**  
      + **2.1.2 設定「Caption」為「RUN」。**  
      + **2.1.3 設定「Name」為「ExternalProgramRUN_Button」。**
    + **3.2 拖拉一個「Standard>TButton」到「Form1」中。預設名稱會是「Button1」。**  
      + **3.2.1 設定「Anchors」。**  
        + **3.2.1.1 「Top anchoring>Slbling」選為「ExternalProgramParameters_Edit:TEdit」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選下面的。**  
        + **3.2.1.2 「Left anchoring>Slbling」選為「ExternalProgramRUN_Button:TButton」。維持「Left anchoring>Enable」為打勾。維持「Left anchoring」右側三個鈕選右邊的。**  
        + **3.2.1.3 「Border space」正中間那格填「20」。**  
        + **3.2.1.4 設定完後關閉Anchors設定視窗。**  
      + **3.2.2 設定「Caption」為「STOP」。**  
      + **3.2.3 設定「Name」為「ExternalProgramSTOP_Button」。** 
    + **3.3 拖拉一個「Standard>TButton」到「Form1」中。預設名稱會是「Button1」。**  
      + **3.3.1 設定「Anchors」。**  
        + **3.3.1.1 「Top anchoring>Slbling」選為「ExternalProgramParameters_Edit:TEdit」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選下面的。**  
        + **3.3.1.2 「Left anchoring>Slbling」選為「ExternalProgramSTOP_Button:TButton」。維持「Left anchoring>Enable」為打勾。維持「Left anchoring」右側三個鈕選右邊的。**  
        + **3.3.1.3 「Border space」正中間那格填「20」。**  
        + **3.3.1.4 設定完後關閉Anchors設定視窗。**  
      + **3.3.2 設定「Caption」為「CLEAR」。**  
      + **3.3.3 設定「Name」為「Memo1Clear_Button」。**
4. Memo1:
    + **4.1 拖拉一個「Standard>TMemo」到「Form1」中。預設名稱會是「Memo1」。**  
      + **4.1.1 設定「Anchors」。**  
        + **4.1.1.1 「Top anchoring>Slbling」選為「ExternalProgramRUN_Button:TButton」。維持「Top anchoring>Enable」為打勾。選擇「Top anchoring」右側三個鈕選下面的。**  
        + **4.1.1.2 「Left anchoring>Slbling」選為「Form1:TForm1」。維持「Left anchoring>Enable」為打勾。維持「Left anchoring」右側三個鈕選左邊的。**  
        + **4.1.1.3 「Right anchoring>Slbling」選為「Form1:TForm1」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
        + **4.1.1.4 「Bottom anchoring>Slbling」選為「Form1:TForm1」。勾選「Bottom anchoring>Enable」為打勾。選擇「Bottom anchoring」右側三個鈕選下面的。**  
        + **4.1.1.3 「Border space」正中間那格填「20」。**  
        + **4.1.1.4 設定完後關閉Anchors設定視窗。**
      + **4.1.2 設定「Lines」的內容清空**
      + **4.1.3 設定「ScrollBars」為「ssBoth」。**
      + **4.1.4 設定「WordWrap」為「false」。**   
   + **4.2 設定「Memo1Clear_Button」的「Event」頁面下「OnClick」為如下程式碼。**
      ```pascal
      procedure TForm1.Memo1Clear_ButtonClick(Sender: TObject);
      begin
        Memo1.Clear;
      end;    
      ```
5. AsyncProcess1:
    + **5.1 拖拉一個「System>TAsyncProcess」到「Form1」中。預設名稱會是「AsyncProcess1」。**
    + 從程式碼最頂端的uses區塊會看到如下內容:
      ```pascal
      uses
        Classes, SysUtils, Forms, Controls, Graphics, Dialogs, Menus, ExtCtrls,
        StdCtrls, AsyncProcess;  
      ```
    +  **5.2 在uses區塊手動添加「Process」、「LazUTF8」。使其變成如下內容:**
      ```pascal
      uses
        Classes, SysUtils, Forms, Controls, Graphics, Dialogs, Menus, ExtCtrls,
        StdCtrls, AsyncProcess, Process, LazUTF8;       
      ```
    + **5.3 設定「AsyncProcess1」的「Event」頁面下「OnReadData」為如下程式碼。**
      ```pascal
      procedure TForm1.AsyncProcess1ReadData(Sender: TObject);
      var
        temp_Buffer:string='';
        temp_BytesAvailable:DWord;
      begin
        Memo1.Lines.Add('AsyncProcess1ReadData...');
        temp_BytesAvailable:=AsyncProcess1.Output.NumBytesAvailable;
        if temp_BytesAvailable>0 Then begin
          setlength(temp_Buffer,temp_BytesAvailable);
          AsyncProcess1.Output.Read(temp_Buffer[1],temp_BytesAvailable);
          Memo1.Lines.Add(WinCPToUTF8(temp_Buffer));
        end;
      end;   
      ```
    + **5.4 設定「AsyncProcess1」的「Event」頁面下「OnTerminate」為如下程式碼。**
      ```pascal
      procedure TForm1.AsyncProcess1Terminate(Sender: TObject);
      begin
        Memo1.Lines.Add('AsyncProcess1Terminate...');
      end;  
      ```
    + **5.5 設定「ExternalProgramRUN_Button」的「Event」頁面下「OnClick」為如下程式碼。**
      ```pascal
      procedure TForm1.AsyncProcess1Terminate(Sender: TObject);
      begin
        AsyncProcess1.Executable:='notepad.exe';
        AsyncProcess1.Parameters.Clear;
        AsyncProcess1.Parameters.Add('/c');
        AsyncProcess1.Parameters.Add('dir');
        AsyncProcess1.Options:=[poUsePipes];
        AsyncProcess1.Execute; 
      end;  
      ```
