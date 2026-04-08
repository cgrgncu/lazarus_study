
# R2MS_Lite_ERTMaker 開發筆記
+ 作者: HsiupoYeh  
+ 更新日期: 2026-04-07
+ 從 **R2MS_Lite_ERTMaker_v1.0(v20260113a).md** 開始，修正錯誤。

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
  ...\ERTMaker_CreateAndModifyMesh_v20260112a.cpython-312.pyc
  ...\ERTMaker_SimulateForTimeSeries_v20251031b.cpython-312.pyc
  ...\ERTMaker_SimulateForTimeSeries_v20251101b.cpython-312.pyc
  ...\Input_ERTMaker_CreateAndModifyMesh\
  ...\Input_ERTMaker_SimulateForTimeSeries\
  ...\Input_ERTMaker_SimulateForTimeSeries\Configs\
  ...\Input_ERTMaker_SimulateForTimeSeries\Configs\... (許多放電檔案)
  ...\PythonEnv\
  ...\PythonEnv\python.exe
  ...\PythonEnv\... (許多其他python所需檔案)
  ```
  + 準備VC檔案，沒有安裝過散發套件的電腦會需要。
  ```
  ...\PythonEnv\vcruntime140.dll
  ...\PythonEnv\vcruntime140_1.dll
  ```

## 開發紀錄(寫功能部分)
### 版本
+ 1.1選Form，去編輯FormCreate，修改部分:
```pascal
procedure TForm1.FormCreate(Sender: TObject);
begin
  //--------------------------------------------------------------------------
  // 在這裡對全域變數進行初始化
  version_str := 'v1.0(v20260407a)';
  Current_Folder_Path := ExtractFilePath(Application.ExeName);
  //--------------------------------------------------------------------------
  //--------------------------------------------------------------------------
  // 表單標題追加版本
  Form1.Caption := Form1.Caption + ' ' + version_str;
  //--------------------------------------------------------------------------
end;      
```
### 追加設計UI
+ 1.1 第5個分頁「時間序列解算」(TimeSeriesProcessing_TabSheet):
  + **1.1.1 拖拉一個「Standard>TToolBar」到「TimeSeriesProcessing_TabSheet」中。預設名稱會是「ToolBar1」，修改「Name」為「TimeSeriesProcessing_ToolBar」。**
  + **1.1.2 設定「BorderSpacing>Top」為「1」。**
  + **1.1.3 設定「Edge>Borders>ebBottom」為「True」。**
  + **1.1.4 設定「Edge>Borders>ebLeft」為「True」。**
  + **1.1.5 設定「Edge>Borders>ebRight」為「True」。**
  + **1.1.6 設定「ShowCaptions」為「True」。**
  + **1.1.7 在「ToolBar1」元件上按「右鍵>New CheckButton」，會增加一個新按鈕。預設名稱會是「ToolButton1」，修改「Name」為「TimeSeriesProcessingRun_ToolButton」**
    + **1.1.7.1 設定「Caption」為「運行時間序列解算」。**
    + **1.1.7.2 設定「TimeSeriesProcessingRun_ToolButton」的「Event」頁面下「OnClick」為如下程式碼。**
    ```pascal

    ```
+ 1.2 第5個分頁「時間序列解算」(TimeSeriesProcessing_TabSheet):
  + **1.1.1 拖拉一個「Standard>TToolBar」到「TimeSeriesProcessing_TabSheet」中。預設名稱會是「ToolBar1」，修改「Name」為「TimeSeriesProcessing_ToolBar」。**
  + **1.1.2 設定「BorderSpacing>Top」為「1」。**
  + **1.1.3 設定「Edge>Borders>ebBottom」為「True」。**
  + **1.1.4 設定「Edge>Borders>ebLeft」為「True」。**
  + **1.1.5 設定「Edge>Borders>ebRight」為「True」。**
  + **1.1.6 設定「ShowCaptions」為「True」。**
  + **1.1.7 在「ToolBar1」元件上按「右鍵>New CheckButton」，會增加一個新按鈕。預設名稱會是「ToolButton1」，修改「Name」為「TimeSeriesProcessingRun_ToolButton」**
    + **1.1.7.1 設定「Caption」為「運行時間序列解算」。**
    + **1.1.7.2 設定「TimeSeriesProcessingRun_ToolButton」的「Event」頁面下「OnClick」為如下程式碼。**
    ```pascal

    ```
  + **1.1.8 在「ToolBar1」元件上按「右鍵>New CheckButton」，會增加一個新按鈕。預設名稱會是「ToolButton1」，修改「Name」為「TimeSeriesProcessingOpenOutputFolder_ToolButton」**
    + **1.1.8.1 設定「Caption」為「開啟輸出資料夾」。**
    + **1.1.8.2 設定「TimeSeriesProcessingOpenOutputFolder_ToolButton」的「Event」頁面下「OnClick」為如下程式碼。**
    ```pascal

    ```
  + **1.1.9 拖拉一個「Standard>TGroupBox」到「TimeSeriesProcessing_TabSheet」中。預設名稱會是「GroupBox1」，修改「Name」為「TimeSeriesProcessingParameters_GroupBox」。**
    + **1.1.9.1 設定「Align」為「alClient」。**
    + **1.1.9.2 設定「BorderSpacing>Right」為「1」。**
    + **1.1.9.3 設定「Caption」為「參數設定:」。** 
  + **1.1.10 拖拉一個「Standard>TGroupBox」到「TimeSeriesProcessingParameters_GroupBox」中。預設名稱會是「GroupBox1」，修改「Name」為「TimeSeriesProcessingInput01Geo_GroupBox」。**
    + **1.1.10.1 設定「Anchors」。**  
      + **1.1.10.1.1 「Top anchoring>Slbling」選為「TimeSeriesProcessingParameters_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
      + **1.1.10.1.2 「Left anchoring>Slbling」選為「TimeSeriesProcessingParameters_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.1.10.1.3 「Right anchoring>Slbling」選為「TimeSeriesProcessingParameters_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
      + **1.1.10.1.4 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
      + **1.1.10.1.5 設定完後關閉Anchors設定視窗。**
    + **1.1.10.2 設定「Caption」為「必要輸入檔案#1(.geo):」。**
    + **1.1.10.3 設定「Height」為「48」。**
  + **1.1.11 拖拉一個「Standard>TButton」到「TimeSeriesProcessingInput01Geo_GroupBox」中。預設名稱會是「Button1」，修改「Name」為「TimeSeriesProcessingInput01Geo_OpenFolder_Button」**
    + **1.1.11.1 設定「Anchors」。**  
      + **1.1.11.1.1 「Top anchoring>Slbling」選為「TimeSeriesProcessingInput01Geo_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
      + **1.1.11.1.2 「Left anchoring>Slbling」修改「Left anchoring>Enable」為不打勾。**  
      + **1.1.11.1.3 「Right anchoring>Slbling」選為「TimeSeriesProcessingInput01Geo_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
      + **1.1.11.1.4 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
      + **1.1.11.1.5 設定完後關閉Anchors設定視窗。**  
    + **1.1.11.2 設定「Caption」為「開啟資料夾」。**
    + **1.1.11.3 設定「Width」為「75」。**
  + **1.1.12 拖拉一個「Standard>TButton」到「TimeSeriesProcessingInput01Geo_GroupBox」中。預設名稱會是「Button1」，修改「Name」為「TimeSeriesProcessingInput01Geo_AutoDetect_Button」**
    + **1.1.12.1 設定「Anchors」。**  
      + **1.1.12.1.1 「Top anchoring>Slbling」選為「TimeSeriesProcessingInput01Geo_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
      + **1.1.12.1.2 「Left anchoring>Slbling」修改「Left anchoring>Enable」為不打勾。**   
      + **1.1.12.1.3 「Right anchoring>Slbling」選為「TimeSeriesProcessingInput01Geo_OpenFolder_Button:TButton」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選左邊的。**  
      + **1.1.12.1.4 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
      + **1.1.12.1.5 設定完後關閉Anchors設定視窗。**  
    + **1.1.12.2 設定「Caption」為「自動偵測」。**
    + **1.1.12.3 設定「Width」為「75」。**
  + **1.1.13 拖拉一個「Standard>TButton」到「TimeSeriesProcessingInput01Geo_GroupBox」中。預設名稱會是「Button1」，修改「Name」為「TimeSeriesProcessingInput01Geo_ManualSelect_Button」**
    + **1.1.13.1 設定「Anchors」。**  
      + **1.1.13.1.1 「Top anchoring>Slbling」選為「TimeSeriesProcessingInput01Geo_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
      + **1.1.13.1.2 「Left anchoring>Slbling」修改「Left anchoring>Enable」為不打勾。**   
      + **1.1.13.1.3 「Right anchoring>Slbling」選為「TimeSeriesProcessingInput01Geo_AutoDetect_Button:TButton」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選左邊的。**  
      + **1.1.13.1.4 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
      + **1.1.13.1.5 設定完後關閉Anchors設定視窗。**  
    + **1.1.13.2 設定「Caption」為「手動選擇」。**
    + **1.1.13.3 設定「Width」為「75」。**
  + **1.1.14 拖拉一個「Standard>TEdit」到「TimeSeriesProcessingInput01Geo_GroupBox」中。預設名稱會是「Edit1」，修改「Name」為「TimeSeriesProcessingInput01Geo_Edit」**
    + **1.1.14.1 設定「Anchors」。**  
      + **1.1.14.1.1 「Top anchoring>Slbling」選為「TimeSeriesProcessingInput01Geo_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**  
      + **1.1.14.1.2 「Left anchoring>Slbling」選為「TimeSeriesProcessingInput01Geo_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.1.14.1.3 「Right anchoring>Slbling」選為「TimeSeriesProcessingInput01Geo_ManualSelect_Button:TButton」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選左邊的。**  
      + **1.1.14.1.4 「Border space」正中間那格填「0」，上面那格填「1」，左邊那格填「3」，右邊那格填「3」。**  
      + **1.1.14.1.5 設定完後關閉Anchors設定視窗。**  
    + **1.1.14.2 設定「ReadOnly」為「True」。**
    + **1.1.14.3 設定「Text」為空白。**
  + **1.1.15 拖拉一個「Standard>TGroupBox」到「TimeSeriesProcessingParameters_GroupBox」中。預設名稱會是「GroupBox1」，修改「Name」為「TimeSeriesProcessingInput02Ohm_GroupBox」。**
    + **1.1.15.1 設定「Anchors」。**  
      + **1.1.15.1.1 「Top anchoring>Slbling」選為「TimeSeriesProcessingInput01Geo_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選下面的。**
      + **1.1.15.1.2 「Left anchoring>Slbling」選為「TimeSeriesProcessingParameters_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.1.15.1.3 「Right anchoring>Slbling」選為「TimeSeriesProcessingParameters_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
      + **1.1.15.1.4 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
      + **1.1.15.1.5 設定完後關閉Anchors設定視窗。**
    + **1.1.15.2 設定「Caption」為「必要輸入檔案#2(.ohm):」。**
    + **1.1.15.3 設定「Height」為「48」。**
  + **1.1.16 拖拉一個「Standard>TButton」到「TimeSeriesProcessingInput02Ohm_GroupBox」中。預設名稱會是「Button1」，修改「Name」為「TimeSeriesProcessingInput02Ohm_OpenFolder_Button」**
    + **1.1.16.1 設定「Anchors」。**  
      + **1.1.16.1.1 「Top anchoring>Slbling」選為「TimeSeriesProcessingInput02Ohm_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
      + **1.1.16.1.2 「Left anchoring>Slbling」修改「Left anchoring>Enable」為不打勾。**   
      + **1.1.16.1.3 「Right anchoring>Slbling」選為「TimeSeriesProcessingInput02Ohm_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
      + **1.1.16.1.4 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
      + **1.1.16.1.5 設定完後關閉Anchors設定視窗。**  
    + **1.1.16.2 設定「Caption」為「開啟資料夾」。**
    + **1.1.16.3 設定「Width」為「75」。**
  + **1.1.17 拖拉一個「Standard>TButton」到「TimeSeriesProcessingInput02Ohm_GroupBox」中。預設名稱會是「Button1」，修改「Name」為「TimeSeriesProcessingInput02Ohm_AutoDetect_Button」**
    + **1.1.17.1 設定「Anchors」。**  
      + **1.1.17.1.1 「Top anchoring>Slbling」選為「TimeSeriesProcessingInput02Ohm_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
      + **1.1.17.1.2 「Left anchoring>Slbling」修改「Left anchoring>Enable」為不打勾。**   
      + **1.1.17.1.3 「Right anchoring>Slbling」選為「TimeSeriesProcessingInput02Ohm_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
      + **1.1.17.1.4 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
      + **1.1.17.1.5 設定完後關閉Anchors設定視窗。**  
    + **1.1.17.2 設定「Caption」為「自動偵測」。**
    + **1.1.17.3 設定「Width」為「75」。**
  + **1.1.18 拖拉一個「Standard>TButton」到「TimeSeriesProcessingInput02Ohm_GroupBox」中。預設名稱會是「Button1」，修改「Name」為「TimeSeriesProcessingInput02Ohm_ManualSelect_Button」**
    + **1.1.18.1 設定「Anchors」。**  
      + **1.1.18.1.1 「Top anchoring>Slbling」選為「TimeSeriesProcessingInput02Ohm_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
      + **1.1.18.1.2 「Left anchoring>Slbling」修改「Left anchoring>Enable」為不打勾。** 
      + **1.1.18.1.3 「Right anchoring>Slbling」選為「TimeSeriesProcessingInput02Ohm_AutoDetect_Button:TButton」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選左邊的。**  
      + **1.1.18.1.4 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
      + **1.1.18.1.5 設定完後關閉Anchors設定視窗。**  
    + **1.1.18.2 設定「Caption」為「手動選擇」。**
    + **1.1.18.3 設定「Width」為「75」。**
  + **1.1.19 拖拉一個「Standard>TEdit」到「TimeSeriesProcessingInput02Ohm_GroupBox」中。預設名稱會是「Edit1」，修改「Name」為「TimeSeriesProcessingInput02Ohm_Edit」**
    + **1.1.19.1 設定「Anchors」。**  
      + **1.1.19.1.1 「Top anchoring>Slbling」選為「TimeSeriesProcessingInput02Ohm_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**  
      + **1.1.19.1.2 「Left anchoring>Slbling」選為「TimeSeriesProcessingInput02Ohm_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.1.19.1.3 「Right anchoring>Slbling」選為「TimeSeriesProcessingInput02Ohm_ManualSelect_Button:TButton」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選左邊的。**  
      + **1.1.19.1.4 「Border space」正中間那格填「0」，上面那格填「1」，左邊那格填「3」，右邊那格填「3」。**  
      + **1.1.19.1.5 設定完後關閉Anchors設定視窗。**  
    + **1.1.19.2 設定「ReadOnly」為「True」。**
    + **1.1.19.3 設定「Text」為空白。**
  + **1.1.20 拖拉一個「Standard>TGroupBox」到「TimeSeriesProcessingParameters_GroupBox」中。預設名稱會是「GroupBox1」，修改「Name」為「TimeSeriesProcessingInput03v299Scsv_GroupBox」。**
    + **1.1.20.1 設定「Anchors」。**  
      + **1.1.20.1.1 「Top anchoring>Slbling」選為「TimeSeriesProcessingInput02Ohm_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選下面的。**
      + **1.1.20.1.2 「Left anchoring>Slbling」選為「TimeSeriesProcessingParameters_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.1.20.1.3 「Right anchoring>Slbling」選為「TimeSeriesProcessingParameters_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
      + **1.1.20.1.4 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
      + **1.1.20.1.5 設定完後關閉Anchors設定視窗。**
    + **1.1.20.2 設定「Caption」為「必要輸入檔案#3(.v299S.csv):」。**
    + **1.1.20.3 設定「Height」為「48」。**
  + **1.1.21 拖拉一個「Standard>TButton」到「TimeSeriesProcessingInput03v299Scsv_GroupBox」中。預設名稱會是「Button1」，修改「Name」為「TimeSeriesProcessingInput03v299Scsv_OpenFolder_Button」**
    + **1.1.21.1 設定「Anchors」。**  
      + **1.1.21.1.1 「Top anchoring>Slbling」選為「TimeSeriesProcessingInput03v299Scsv_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
      + **1.1.21.1.2 「Left anchoring>Slbling」修改「Left anchoring>Enable」為不打勾。**  
      + **1.1.21.1.3 「Right anchoring>Slbling」選為「TimeSeriesProcessingInput03v299Scsv_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
      + **1.1.21.1.4 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
      + **1.1.21.1.5 設定完後關閉Anchors設定視窗。**  
    + **1.1.21.2 設定「Caption」為「開啟資料夾」。**
    + **1.1.21.3 設定「Width」為「75」。**
  + **1.1.22 拖拉一個「Standard>TButton」到「TimeSeriesProcessingInput03v299Scsv_GroupBox」中。預設名稱會是「Button1」，修改「Name」為「TimeSeriesProcessingInput03v299Scsv_AutoDetect_Button」**
    + **1.1.22.1 設定「Anchors」。**  
      + **1.1.22.1.1 「Top anchoring>Slbling」選為「TimeSeriesProcessingInput03v299Scsv_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
      + **1.1.22.1.2 「Left anchoring>Slbling」修改「Left anchoring>Enable」為不打勾。**  
      + **1.1.22.1.3 「Right anchoring>Slbling」選為「TimeSeriesProcessingInput03v299Scsv_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
      + **1.1.22.1.4 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
      + **1.1.22.1.5 設定完後關閉Anchors設定視窗。**  
    + **1.1.22.2 設定「Caption」為「自動偵測」。**
    + **1.1.22.3 設定「Width」為「75」。**
  + **1.1.23 拖拉一個「Standard>TButton」到「TimeSeriesProcessingInput03v299Scsv_GroupBox」中。預設名稱會是「Button1」，修改「Name」為「TimeSeriesProcessingInput03v299Scsv_ManualSelect_Button」**
    + **1.1.23.1 設定「Anchors」。**  
      + **1.1.23.1.1 「Top anchoring>Slbling」選為「TimeSeriesProcessingInput03v299Scsv_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
      + **1.1.23.1.2 「Left anchoring>Slbling」修改「Left anchoring>Enable」為不打勾。**  
      + **1.1.23.1.3 「Right anchoring>Slbling」選為「TimeSeriesProcessingInput03v299Scsv_AutoDetect_Button:TButton」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選左邊的。**  
      + **1.1.23.1.4 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
      + **1.1.23.1.5 設定完後關閉Anchors設定視窗。**  
    + **1.1.23.2 設定「Caption」為「手動選擇」。**
    + **1.1.23.3 設定「Width」為「75」。**
  + **1.1.24 拖拉一個「Standard>TEdit」到「TimeSeriesProcessingInput03v299Scsv_GroupBox」中。預設名稱會是「Edit1」，修改「Name」為「TimeSeriesProcessingInput03v299Scsv_Edit」**
    + **1.1.24.1 設定「Anchors」。**  
      + **1.1.24.1.1 「Top anchoring>Slbling」選為「TimeSeriesProcessingInput03v299Scsv_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**  
      + **1.1.24.1.2 「Left anchoring>Slbling」選為「TimeSeriesProcessingInput03v299Scsv_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.1.24.1.3 「Right anchoring>Slbling」選為「TimeSeriesProcessingInput03v299Scsv_ManualSelect_Button:TButton」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選左邊的。**  
      + **1.1.24.1.4 「Border space」正中間那格填「0」，上面那格填「1」，左邊那格填「3」，右邊那格填「3」。**  
      + **1.1.24.1.5 設定完後關閉Anchors設定視窗。**  
    + **1.1.24.2 設定「ReadOnly」為「True」。**
    + **1.1.24.3 設定「Text」為空白。**
  + **1.1.25 拖拉一個「Standard>TGroupBox」到「TimeSeriesProcessingParameters_GroupBox」中。預設名稱會是「GroupBox1」，修改「Name」為「TimeSeriesProcessingInput04ABMN_GroupBox」。**
    + **1.1.25.1 設定「Anchors」。**  
      + **1.1.25.1.1 「Top anchoring>Slbling」選為「TimeSeriesProcessingInput03v299Scsv_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選下面的。**
      + **1.1.25.1.2 「Left anchoring>Slbling」選為「TimeSeriesProcessingParameters_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.1.25.1.3 「Right anchoring>Slbling」選為「TimeSeriesProcessingParameters_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
      + **1.1.25.1.4 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
      + **1.1.25.1.5 設定完後關閉Anchors設定視窗。**
    + **1.1.25.2 設定「Caption」為「必要輸入檔案#4(.abmn):」。**
    + **1.1.25.3 設定「Height」為「48」。**
  + **1.1.26 拖拉一個「Standard>TButton」到「TimeSeriesProcessingInput04ABMN_GroupBox」中。預設名稱會是「Button1」，修改「Name」為「TimeSeriesProcessingInput04ABMN_OpenFolder_Button」**
    + **1.1.26.1 設定「Anchors」。**  
      + **1.1.26.1.1 「Top anchoring>Slbling」選為「TimeSeriesProcessingInput04ABMN_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
      + **1.1.26.1.2 「Left anchoring>Slbling」修改「Left anchoring>Enable」為不打勾。**  
      + **1.1.26.1.2 「Right anchoring>Slbling」選為「TimeSeriesProcessingInput04ABMN_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
      + **1.1.26.1.3 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
      + **1.1.26.1.4 設定完後關閉Anchors設定視窗。**  
    + **1.1.26.2 設定「Caption」為「開啟資料夾」。**
    + **1.1.26.3 設定「Width」為「75」。**
  + **1.1.27 拖拉一個「Standard>TButton」到「TimeSeriesProcessingInput04ABMN_GroupBox」中。預設名稱會是「Button1」，修改「Name」為「TimeSeriesProcessingInput04ABMN_AutoDetect_Button」**
    + **1.1.27.1 設定「Anchors」。**  
      + **1.1.27.1.1 「Top anchoring>Slbling」選為「TimeSeriesProcessingInput04ABMN_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
      + **1.1.27.1.2 「Left anchoring>Slbling」修改「Left anchoring>Enable」為不打勾。**  
      + **1.1.27.1.3 「Right anchoring>Slbling」選為「TimeSeriesProcessingInput04ABMN_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
      + **1.1.27.1.4 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
      + **1.1.27.1.5 設定完後關閉Anchors設定視窗。**  
    + **1.1.27.2 設定「Caption」為「自動偵測」。**
    + **1.1.27.3 設定「Width」為「75」。**
  + **1.1.28 拖拉一個「Standard>TButton」到「TimeSeriesProcessingInput04ABMN_GroupBox」中。預設名稱會是「Button1」，修改「Name」為「TimeSeriesProcessingInput04ABMN_ManualSelect_Button」**
    + **1.1.28.1 設定「Anchors」。**  
      + **1.1.28.1.1 「Top anchoring>Slbling」選為「TimeSeriesProcessingInput04ABMN_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
      + **1.1.28.1.2 「Left anchoring>Slbling」修改「Left anchoring>Enable」為不打勾。**  
      + **1.1.28.1.3 「Right anchoring>Slbling」選為「TimeSeriesProcessingInput04ABMN_AutoDetect_Button:TButton」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選左邊的。**  
      + **1.1.28.1.4 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
      + **1.1.28.1.5 設定完後關閉Anchors設定視窗。**  
    + **1.1.28.2 設定「Caption」為「手動選擇」。**
    + **1.1.28.3 設定「Width」為「75」。**
  + **1.1.29 拖拉一個「Standard>TEdit」到「TimeSeriesProcessingInput04ABMN_GroupBox」中。預設名稱會是「Edit1」，修改「Name」為「TimeSeriesProcessingInput04ABMN_Edit」**
    + **1.1.29.1 設定「Anchors」。**  
      + **1.1.29.1.1 「Top anchoring>Slbling」選為「TimeSeriesProcessingInput04ABMN_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**  
      + **1.1.29.1.2 「Left anchoring>Slbling」選為「TimeSeriesProcessingInput04ABMN_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.1.29.1.3 「Right anchoring>Slbling」選為「TimeSeriesProcessingInput04ABMN_ManualSelect_Button:TButton」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選左邊的。**  
      + **1.1.29.1.4 「Border space」正中間那格填「0」，上面那格填「1」，左邊那格填「3」，右邊那格填「3」。**  
      + **1.1.29.1.5 設定完後關閉Anchors設定視窗。**  
    + **1.1.29.2 設定「ReadOnly」為「True」。**
    + **1.1.29.3 設定「Text」為空白。**
  + **1.1.30 拖拉一個「Standard>TGroupBox」到「TimeSeriesProcessingParameters_GroupBox」中。預設名稱會是「GroupBox1」，修改「Name」為「TimeSeriesProcessingOutputPNG_GroupBox」。**
    + **1.1.30.1 設定「Anchors」。**  
      + **1.1.30.1.1 「Top anchoring>Slbling」選為「TimeSeriesProcessingInput04ABMN_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選下面的。**
      + **1.1.30.1.2 「Left anchoring>Slbling」選為「TimeSeriesProcessingParameters_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.1.30.1.3 「Right anchoring>Slbling」選為「TimeSeriesProcessingParameters_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
      + **1.1.30.1.4 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
      + **1.1.30.1.5 設定完後關閉Anchors設定視窗。**
    + **1.1.30.2 設定「Caption」為「儲存PNG檔案(若不勾選，將省略繪製分佈圖，耗時較短。):」。**
    + **1.1.30.3 設定「Height」為「48」。**
  + **1.1.31 拖拉一個「Standard>TCheckBox」到「TimeSeriesProcessingOutputPNG_GroupBox」中。預設名稱會是「CheckBox1」，修改「Name」為「TimeSeriesProcessingOutputPNG_All_CheckBox」**
    + **1.1.31.1 設定「Anchors」。**  
      + **1.1.31.1.1 「Top anchoring>Slbling」選為「TimeSeriesProcessingOutputPNG_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
      + **1.1.31.1.2 「Left anchoring>Slbling」選為「TimeSeriesProcessingOutputPNG_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.1.31.1.3 「Border space」正中間那格填「0」，左邊那格填「8」，右邊那格填「8」。**  
      + **1.1.31.1.4 設定完後關閉Anchors設定視窗。**  
    + **1.1.31.2 設定「Caption」為「All」。**
    + **1.1.31.3 設定「Hint」為「所有四極陣列(All)」。**
    + **1.1.31.4 設定「ShowHint」為「True」。**
  + **1.1.32 拖拉一個「Standard>TCheckBox」到「TimeSeriesProcessingOutputPNG_GroupBox」中。預設名稱會是「CheckBox1」，修改「Name」為「TimeSeriesProcessingOutputPNG_WS_CheckBox」**
    + **1.1.32.1 設定「Anchors」。**  
      + **1.1.32.1.1 「Top anchoring>Slbling」選為「TimeSeriesProcessingOutputPNG_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
      + **1.1.32.1.2 「Left anchoring>Slbling」選為「TimeSeriesProcessingOutputPNG_All_CheckBox:TCheckBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選右邊的。**  
      + **1.1.32.1.3 「Border space」正中間那格填「0」，左邊那格填「8」，右邊那格填「8」。**  
      + **1.1.32.1.4 設定完後關閉Anchors設定視窗。**  
    + **1.1.32.2 設定「Caption」為「WS」。**
    + **1.1.32.3 設定「Hint」為「Wenner-Schlumberger(WS)」。**
    + **1.1.32.4 設定「ShowHint」為「True」。**
  + **1.1.33 拖拉一個「Standard>TCheckBox」到「TimeSeriesProcessingOutputPNG_GroupBox」中。預設名稱會是「CheckBox1」，修改「Name」為「TimeSeriesProcessingOutputPNG_MGD_CheckBox」**
    + **1.1.33.1 設定「Anchors」。**  
      + **1.1.33.1.1 「Top anchoring>Slbling」選為「TimeSeriesProcessingOutputPNG_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
      + **1.1.33.1.2 「Left anchoring>Slbling」選為「TimeSeriesProcessingOutputPNG_WS_CheckBox:TCheckBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選右邊的。**  
      + **1.1.33.1.3 「Border space」正中間那格填「0」，左邊那格填「8」，右邊那格填「8」。**  
      + **1.1.33.1.4 設定完後關閉Anchors設定視窗。**  
    + **1.1.33.2 設定「Caption」為「MGD」。**
    + **1.1.33.3 設定「Hint」為「Multiple or Moving Gradient Array(MGD)」。**
    + **1.1.33.4 設定「ShowHint」為「True」。**
  + **1.1.34 拖拉一個「Standard>TCheckBox」到「TimeSeriesProcessingOutputPNG_GroupBox」中。預設名稱會是「CheckBox1」，修改「Name」為「TimeSeriesProcessingOutputPNG_MPR_CheckBox」**
    + **1.1.34.1 設定「Anchors」。**  
      + **1.1.34.1.1 「Top anchoring>Slbling」選為「TimeSeriesProcessingOutputPNG_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
      + **1.1.34.1.2 「Left anchoring>Slbling」選為「TimeSeriesProcessingOutputPNG_MGD_CheckBox:TCheckBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選右邊的。**  
      + **1.1.34.1.3 「Border space」正中間那格填「0」，左邊那格填「8」，右邊那格填「8」。**  
      + **1.1.34.1.4 設定完後關閉Anchors設定視窗。**  
    + **1.1.34.2 設定「Caption」為「MPR」。**
    + **1.1.34.3 設定「Hint」為「Multiple or Moving Gradient Array(MGD)」。**
    + **1.1.34.4 設定「ShowHint」為「True」。**
  + **1.1.35 拖拉一個「Standard>TCheckBox」到「TimeSeriesProcessingOutputPNG_GroupBox」中。預設名稱會是「CheckBox1」，修改「Name」為「TimeSeriesProcessingOutputPNG_GD_CheckBox」**
    + **1.1.35.1 設定「Anchors」。**  
      + **1.1.35.1.1 「Top anchoring>Slbling」選為「TimeSeriesProcessingOutputPNG_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
      + **1.1.35.1.2 「Left anchoring>Slbling」選為「TimeSeriesProcessingOutputPNG_MPR_CheckBox:TCheckBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選右邊的。**  
      + **1.1.35.1.3 「Border space」正中間那格填「0」，左邊那格填「8」，右邊那格填「8」。**  
      + **1.1.35.1.4 設定完後關閉Anchors設定視窗。**  
    + **1.1.35.2 設定「Caption」為「GD」。**
    + **1.1.35.3 設定「Hint」為「Gradient(GD) Array」。**
    + **1.1.35.4 設定「ShowHint」為「True」。**
  + **1.1.36 拖拉一個「Standard>TCheckBox」到「TimeSeriesProcessingOutputPNG_GroupBox」中。預設名稱會是「CheckBox1」，修改「Name」為「TimeSeriesProcessingOutputPNG_Select_CheckBox」**
    + **1.1.36.1 設定「Anchors」。**  
      + **1.1.36.1.1 「Top anchoring>Slbling」選為「TimeSeriesProcessingOutputPNG_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
      + **1.1.36.1.2 「Left anchoring>Slbling」選為「TimeSeriesProcessingOutputPNG_MPR_CheckBox:TCheckBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選右邊的。**  
      + **1.1.36.1.3 「Border space」正中間那格填「0」，左邊那格填「8」，右邊那格填「8」。**  
      + **1.1.36.1.4 設定完後關閉Anchors設定視窗。**  
    + **1.1.36.2 設定「Caption」為「Select」。**
    + **1.1.36.3 設定「Hint」為「指定陣列加篩選條件(Select)」。**
    + **1.1.36.4 設定「ShowHint」為「True」。**
  + **1.1.37 拖拉一個「Standard>TGroupBox」到「TimeSeriesProcessingParameters_GroupBox」中。預設名稱會是「GroupBox1」，修改「Name」為「TimeSeriesProcessingSelectSettings_GroupBox」。**
    + **1.1.37.1 設定「Anchors」。**  
      + **1.1.37.1.1 「Top anchoring>Slbling」選為「TimeSeriesProcessingOutputPNG_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選下面的。**
      + **1.1.37.1.2 「Left anchoring>Slbling」選為「TimeSeriesProcessingParameters_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.1.37.1.3 「Right anchoring>Slbling」選為「TimeSeriesProcessingParameters_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
      + **1.1.37.1.4 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
      + **1.1.37.1.5 設定完後關閉Anchors設定視窗。**
    + **1.1.37.2 設定「Caption」為「Select篩選條件:」。**
    + **1.1.37.3 設定「Height」為「48」。**
### 寫功能

