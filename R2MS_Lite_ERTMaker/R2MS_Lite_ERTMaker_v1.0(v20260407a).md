
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
  ...\ERTMaker_v299ScsvToUrf_v20260217a.cpython-312.pyc
  ...\Input_ERTMaker_CreateAndModifyMesh\
  ...\Input_ERTMaker_SimulateForTimeSeries\
  ...\Input_ERTMaker_SimulateForTimeSeries\Configs\
  ...\Input_ERTMaker_SimulateForTimeSeries\Configs\... (許多放電檔案)
  ...\Input_ERTMaker_v299ScsvToUrf\
  ...\PythonEnv\
  ...\PythonEnv\python.exe
  ...\PythonEnv\... (許多其他python所需檔案)
  ```
  + 準備VC檔案，沒有安裝過散發套件的電腦會需要。
  ```
  ...\PythonEnv\vcruntime140.dll
  ...\PythonEnv\vcruntime140_1.dll
  ```

## 開發紀錄(寫UI部分)
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
  + **1.1.8 在「ToolBar1」元件上按「右鍵>New CheckButton」，會增加一個新按鈕。預設名稱會是「ToolButton1」，修改「Name」為「TimeSeriesProcessingOpenOutputFolder_ToolButton」**
    + **1.1.8.1 設定「Caption」為「開啟輸出資料夾」。**
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
    + **1.1.30.3 設定「Height」為「42」。**
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
  + **1.1.37 拖拉一個「Standard>TRadioGroup」到「TimeSeriesProcessingParameters_GroupBox」中。預設名稱會是「RadioGroup1」，修改「Name」為「TimeSeriesProcessingSelectFrom_RadioGroup」。**
    + **1.1.37.1 設定「Anchors」。**  
      + **1.1.37.1.1 「Top anchoring>Slbling」選為「TimeSeriesProcessingOutputPNG_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選下面的。**
      + **1.1.37.1.2 「Left anchoring>Slbling」選為「TimeSeriesProcessingParameters_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.1.37.1.3 「Right anchoring>Slbling」選為「TimeSeriesProcessingParameters_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
      + **1.1.37.1.4 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
      + **1.1.37.1.5 設定完後關閉Anchors設定視窗。**
    + **1.1.37.2 設定「Caption」為「Select From:」。**
    + **1.1.37.2 設定「ColumnLayout」為「clVerticalThenHorizontal」。**
    + **1.1.37.3 設定「Height」為「42」。**
  + **1.1.38 拖拉一個「Standard>TRadioButton」到「TimeSeriesProcessingSelectFrom_RadioGroup」中。預設名稱會是「RadioButton1」，修改「Name」為「TimeSeriesProcessingSelectFrom_All_RadioButton」。**
    + **1.1.38.1 設定「Anchors」。**  
      + **1.1.38.1.1 「Top anchoring>Slbling」選為「TimeSeriesProcessingSelectFrom_RadioGroup:TRadioGroup」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
      + **1.1.38.1.2 「Left anchoring>Slbling」選為「TimeSeriesProcessingSelectFrom_RadioGroup:TRadioGroup」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.1.38.1.3 「Border space」正中間那格填「0」，左邊那格填「8」，右邊那格填「8」。**  
      + **1.1.38.1.4 設定完後關閉Anchors設定視窗。**  
    + **1.1.38.2 設定「Caption」為「All」。**
    + **1.1.38.3 設定「Checked」為「True」。**
  + **1.1.39 拖拉一個「Standard>TRadioButton」到「TimeSeriesProcessingSelectFrom_RadioGroup」中。預設名稱會是「RadioButton1」，修改「Name」為「TimeSeriesProcessingSelectFrom_WS_RadioButton」。**
    + **1.1.39.1 設定「Anchors」。**  
      + **1.1.39.1.1 「Top anchoring>Slbling」選為「TimeSeriesProcessingSelectFrom_RadioGroup:TRadioGroup」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
      + **1.1.39.1.2 「Left anchoring>Slbling」選為「TimeSeriesProcessingSelectFrom_All_RadioButton:TRadioButton」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.1.39.1.3 「Border space」正中間那格填「0」，左邊那格填「8」，右邊那格填「8」。**  
      + **1.1.39.1.4 設定完後關閉Anchors設定視窗。**  
    + **1.1.39.2 設定「Caption」為「WS」。**
  + **1.1.40 拖拉一個「Standard>TRadioButton」到「TimeSeriesProcessingSelectFrom_RadioGroup」中。預設名稱會是「RadioButton1」，修改「Name」為「TimeSeriesProcessingSelectFrom_MGD_RadioButton」。**
    + **1.1.40.1 設定「Anchors」。**  
      + **1.1.40.1.1 「Top anchoring>Slbling」選為「TimeSeriesProcessingSelectFrom_RadioGroup:TRadioGroup」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
      + **1.1.40.1.2 「Left anchoring>Slbling」選為「TimeSeriesProcessingSelectFrom_WS_RadioButton:TRadioButton」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.1.40.1.3 「Border space」正中間那格填「0」，左邊那格填「8」，右邊那格填「8」。**  
      + **1.1.40.1.4 設定完後關閉Anchors設定視窗。**  
    + **1.1.40.2 設定「Caption」為「MGD」。**
  + **1.1.41 拖拉一個「Standard>TRadioButton」到「TimeSeriesProcessingSelectFrom_RadioGroup」中。預設名稱會是「RadioButton1」，修改「Name」為「TimeSeriesProcessingSelectFrom_MPR_RadioButton」。**
    + **1.1.41.1 設定「Anchors」。**  
      + **1.1.41.1.1 「Top anchoring>Slbling」選為「TimeSeriesProcessingSelectFrom_RadioGroup:TRadioGroup」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
      + **1.1.41.1.2 「Left anchoring>Slbling」選為「TimeSeriesProcessingSelectFrom_MGD_RadioButton:TRadioButton」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.1.41.1.3 「Border space」正中間那格填「0」，左邊那格填「8」，右邊那格填「8」。**  
      + **1.1.41.1.4 設定完後關閉Anchors設定視窗。**  
    + **1.1.41.2 設定「Caption」為「MPR」。**
  + **1.1.42 拖拉一個「Standard>TRadioButton」到「TimeSeriesProcessingSelectFrom_RadioGroup」中。預設名稱會是「RadioButton1」，修改「Name」為「TimeSeriesProcessingSelectFrom_GD_RadioButton」。**
    + **1.1.42.1 設定「Anchors」。**  
      + **1.1.42.1.1 「Top anchoring>Slbling」選為「TimeSeriesProcessingSelectFrom_RadioGroup:TRadioGroup」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
      + **1.1.42.1.2 「Left anchoring>Slbling」選為「TimeSeriesProcessingSelectFrom_MPR_RadioButton:TRadioButton」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.1.42.1.3 「Border space」正中間那格填「0」，左邊那格填「8」，右邊那格填「8」。**  
      + **1.1.42.1.4 設定完後關閉Anchors設定視窗。**  
    + **1.1.42.2 設定「Caption」為「GD」。**
  + **1.1.43 拖拉一個「Standard>TGroupBox」到「TimeSeriesProcessingParameters_GroupBox」中。預設名稱會是「GroupBox1」，修改「Name」為「TimeSeriesProcessingSelectKeepA_GroupBox」。**
    + **1.1.43.1 設定「Anchors」。**  
      + **1.1.43.1.1 「Top anchoring>Slbling」選為「TimeSeriesProcessingSelectFrom_RadioGroup:TRadioGroup」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選下面的。**
      + **1.1.43.1.2 「Left anchoring>Slbling」選為「TimeSeriesProcessingParameters_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.1.43.1.3 「Right anchoring>Slbling」選為「TimeSeriesProcessingParameters_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
      + **1.1.43.1.4 「Border space」正中間那格填「0」，左邊那格填「8」，右邊那格填「8」。**  
      + **1.1.43.1.5 設定完後關閉Anchors設定視窗。**
    + **1.1.43.2 設定「Caption」為「Select保留A端電極:」。**
    + **1.1.43.3 設定「Height」為「48」。**
  + **1.1.44 拖拉一個「Standard>TEdit」到「TimeSeriesProcessingParameters_GroupBox」中。預設名稱會是「Edit1」，修改「Name」為「TimeSeriesProcessingSelectKeepA_Edit」。**
    + **1.1.44.1 設定「Anchors」。**  
      + **1.1.44.1.1 「Top anchoring>Slbling」選為「TimeSeriesProcessingSelectKeepA_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
      + **1.1.44.1.2 「Left anchoring>Slbling」選為「TimeSeriesProcessingSelectKeepA_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.1.44.1.3 「Right anchoring>Slbling」選為「TimeSeriesProcessingSelectKeepA_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
      + **1.1.44.1.4 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
      + **1.1.44.1.5 設定完後關閉Anchors設定視窗。**
    + **1.1.44.2 設定「Text」為「1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64」。**
  + **1.1.45 拖拉一個「Standard>TGroupBox」到「TimeSeriesProcessingParameters_GroupBox」中。預設名稱會是「GroupBox1」，修改「Name」為「TimeSeriesProcessingSelectKeepB_GroupBox」。**
    + **1.1.45.1 設定「Anchors」。**  
      + **1.1.45.1.1 「Top anchoring>Slbling」選為「TimeSeriesProcessingSelectFrom_RadioGroup:TRadioGroup」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選下面的。**
      + **1.1.45.1.2 「Left anchoring>Slbling」選為「TimeSeriesProcessingParameters_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.1.45.1.3 「Right anchoring>Slbling」選為「TimeSeriesProcessingParameters_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
      + **1.1.45.1.4 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
      + **1.1.45.1.5 設定完後關閉Anchors設定視窗。**
    + **1.1.45.2 設定「Caption」為「Select保留B端電極:」。**
    + **1.1.45.3 設定「Height」為「48」。**
  + **1.1.46 拖拉一個「Standard>TEdit」到「TimeSeriesProcessingParameters_GroupBox」中。預設名稱會是「Edit1」，修改「Name」為「TimeSeriesProcessingSelectKeepB_Edit」。**
    + **1.1.46.1 設定「Anchors」。**  
      + **1.1.46.1.1 「Top anchoring>Slbling」選為「TimeSeriesProcessingSelectKeepB_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
      + **1.1.46.1.2 「Left anchoring>Slbling」選為「TimeSeriesProcessingSelectKeepB_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.1.46.1.3 「Right anchoring>Slbling」選為「TimeSeriesProcessingSelectKeepB_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
      + **1.1.46.1.4 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
      + **1.1.46.1.5 設定完後關閉Anchors設定視窗。**
    + **1.1.46.2 設定「Text」為「1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64」。**
  + **1.1.47 拖拉一個「Standard>TGroupBox」到「TimeSeriesProcessingParameters_GroupBox」中。預設名稱會是「GroupBox1」，修改「Name」為「TimeSeriesProcessingSelectKeepM_GroupBox」。**
    + **1.1.47.1 設定「Anchors」。**  
      + **1.1.47.1.1 「Top anchoring>Slbling」選為「TimeSeriesProcessingSelectFrom_RadioGroup:TRadioGroup」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選下面的。**
      + **1.1.47.1.2 「Left anchoring>Slbling」選為「TimeSeriesProcessingParameters_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.1.47.1.3 「Right anchoring>Slbling」選為「TimeSeriesProcessingParameters_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
      + **1.1.47.1.4 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
      + **1.1.47.1.5 設定完後關閉Anchors設定視窗。**
    + **1.1.47.2 設定「Caption」為「Select保留M端電極:」。**
    + **1.1.47.3 設定「Height」為「48」。**
  + **1.1.48 拖拉一個「Standard>TEdit」到「TimeSeriesProcessingParameters_GroupBox」中。預設名稱會是「Edit1」，修改「Name」為「TimeSeriesProcessingSelectKeepM_Edit」。**
    + **1.1.48.1 設定「Anchors」。**  
      + **1.1.48.1.1 「Top anchoring>Slbling」選為「TimeSeriesProcessingSelectKeepM_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
      + **1.1.48.1.2 「Left anchoring>Slbling」選為「TimeSeriesProcessingSelectKeepM_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.1.48.1.3 「Right anchoring>Slbling」選為「TimeSeriesProcessingSelectKeepM_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
      + **1.1.48.1.4 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
      + **1.1.48.1.5 設定完後關閉Anchors設定視窗。**
    + **1.1.48.2 設定「Text」為「1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64」。**
  + **1.1.49 拖拉一個「Standard>TGroupBox」到「TimeSeriesProcessingParameters_GroupBox」中。預設名稱會是「GroupBox1」，修改「Name」為「TimeSeriesProcessingSelectKeepN_GroupBox」。**
    + **1.1.49.1 設定「Anchors」。**  
      + **1.1.49.1.1 「Top anchoring>Slbling」選為「TimeSeriesProcessingSelectFrom_RadioGroup:TRadioGroup」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選下面的。**
      + **1.1.49.1.2 「Left anchoring>Slbling」選為「TimeSeriesProcessingParameters_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.1.49.1.3 「Right anchoring>Slbling」選為「TimeSeriesProcessingParameters_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
      + **1.1.49.1.4 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
      + **1.1.49.1.5 設定完後關閉Anchors設定視窗。**
    + **1.1.49.2 設定「Caption」為「Select保留N端電極:」。**
    + **1.1.49.3 設定「Height」為「48」。**
  + **1.1.50 拖拉一個「Standard>TEdit」到「TimeSeriesProcessingParameters_GroupBox」中。預設名稱會是「Edit1」，修改「Name」為「TimeSeriesProcessingSelectKeepN_Edit」。**
    + **1.1.50.1 設定「Anchors」。**  
      + **1.1.50.1.1 「Top anchoring>Slbling」選為「TimeSeriesProcessingSelectKeepN_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
      + **1.1.50.1.2 「Left anchoring>Slbling」選為「TimeSeriesProcessingSelectKeepN_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.1.50.1.3 「Right anchoring>Slbling」選為「TimeSeriesProcessingSelectKeepN_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
      + **1.1.50.1.4 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
      + **1.1.50.1.5 設定完後關閉Anchors設定視窗。**
    + **1.1.50.2 設定「Text」為「1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64」。**
+ 1.2 第6個分頁「時間序列解算參數」(TimeSeriesProcessingSettings_TabSheet):
  + **1.2.1 拖拉一個「Standard>TGroupBox」到「TimeSeriesProcessingSettings_TabSheet」中。預設名稱會是「GroupBox1」，修改「Name」為「TimeSeriesProcessingSettingsCmdLog_GroupBox」。**
    + **1.2.1.1 設定「Align」為「alRight」。**
    + **1.2.1.2 設定「BorderSpacing>Left」為「3」。**
    + **1.2.1.3 設定「BorderSpacing>Right」為「1」。**
    + **1.2.1.4 設定「Caption」為「時間序列解算運行紀錄:」。** 
    + **1.2.1.5 設定「Width」為「500」。**
  + **1.2.2 拖拉一個「Standard>TMemo」到「TimeSeriesProcessingSettingsCmdLog_GroupBox」中。預設名稱會是「Memo1」，修改「Name」為「TimeSeriesProcessingSettingsCmdLog_Memo」**
    + **1.2.2.1 設定「Anchors」。**  
      + **1.2.2.1.1 「Top anchoring>Slbling」選為「TimeSeriesProcessingSettingsCmdLog_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。選擇「Top anchoring」右側三個鈕選上面的。** 
      + **1.2.2.1.2 「Left anchoring>Slbling」選為「TimeSeriesProcessingSettingsCmdLog_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.2.2.1.3 「Right anchoring>Slbling」選為「TimeSeriesProcessingSettingsCmdLog_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
      + **1.2.2.1.4 「Bottom anchoring>Slbling」選為「TimeSeriesProcessingSettingsCmdLog_GroupBox:TGroupBox」。勾選「Bottom anchoring>Enable」為打勾。選擇「Bottom anchoring」右側三個鈕選下面的。** 
      + **1.2.2.1.5 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」，下面那格填「3」。**  
      + **1.2.2.1.6 設定完後關閉Anchors設定視窗。**  
    + **1.2.2.2 設定「Color」為「clBlack」。**
    + **1.2.2.3 設定「Font>Color」為「clLime」。**
    + **1.2.2.4 設定「Lines」清空。**
    + **1.2.2.5 設定「ReadOnly」為「True」。**
    + **1.2.2.6 設定「ScrollBars」為「ssBoth」。**
    + **1.2.2.7 設定「WordWrap」為「False」。**
  + **1.2.3 拖拉一個「Standard>TGroupBox」到「TimeSeriesProcessingSettings_TabSheet」中。預設名稱會是「GroupBox1」，修改「Name」為「TimeSeriesProcessingSettingsDefaultJson_GroupBox」**
    + **1.2.3.1 設定「Anchors」。**  
      + **1.2.3.1.1 「Top anchoring>Slbling」選為「TimeSeriesProcessingSettings_TabSheet:TTabSheet」。維持「Top anchoring>Enable」為打勾。選擇「Top anchoring」右側三個鈕選上面的。** 
      + **1.2.3.1.2 「Left anchoring>Slbling」選為「TimeSeriesProcessingSettings_TabSheet:TTabSheet」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.2.3.1.3 「Right anchoring>Slbling」選為「TimeSeriesProcessingSettingsCmdLog_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選左邊的。**  
      + **1.2.3.1.4 設定完後關閉Anchors設定視窗。**  
    + **1.2.3.2 設定「Caption」為「預設參數(Default_v299ScsvToUrf.json):」。**
    + **1.2.3.3 設定「Height」為「200」。**
  + **1.2.4 拖拉一個「Standard>TMemo」到「TimeSeriesProcessingSettingsDefaultJson_GroupBox」中。預設名稱會是「Memo1」，修改「Name」為「TimeSeriesProcessingSettingsDefaultJson_Memo」**
    + **1.2.4.1 設定「Anchors」。**  
      + **1.2.4.1.1 「Top anchoring>Slbling」選為「TimeSeriesProcessingSettingsDefaultJson_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。選擇「Top anchoring」右側三個鈕選上面的。** 
      + **1.2.4.1.2 「Left anchoring>Slbling」選為「TimeSeriesProcessingSettingsDefaultJson_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.2.4.1.3 「Right anchoring>Slbling」選為「TimeSeriesProcessingSettingsDefaultJson_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
      + **1.2.4.1.4 「Bottom anchoring>Slbling」選為「TimeSeriesProcessingSettingsDefaultJson_GroupBox:TGroupBox」。勾選「Bottom anchoring>Enable」為打勾。選擇「Bottom anchoring」右側三個鈕選下面的。** 
      + **1.2.4.1.5 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」，下面那格填「3」。**  
      + **1.2.4.1.6 設定完後關閉Anchors設定視窗。**  
    + **1.2.4.2 設定「Lines」清空。**
    + **1.2.4.3 設定「ReadOnly」為「True」。**
    + **1.2.4.4 設定「ScrollBars」為「ssBoth」。**
    + **1.2.4.5 設定「WordWrap」為「False」。**
  + **1.2.5 拖拉一個「Standard>TGroupBox」到「TimeSeriesProcessingSettings_TabSheet」中。預設名稱會是「GroupBox1」，修改「Name」為「TimeSeriesProcessingSettingsNowJson_GroupBox」**
    + **1.2.5.1 設定「Anchors」。**  
      + **1.2.5.1.1 「Top anchoring>Slbling」選為「TimeSeriesProcessingSettingsDefaultJson_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。選擇「Top anchoring」右側三個鈕選下面的。** 
      + **1.2.5.1.2 「Left anchoring>Slbling」選為「TimeSeriesProcessingSettingsDefaultJson_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.2.5.1.3 「Right anchoring>Slbling」選為「TimeSeriesProcessingSettingsDefaultJson_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
      + **1.2.5.1.4 設定完後關閉Anchors設定視窗。**  
    + **1.2.5.2 設定「Caption」為「目前參數(v299ScsvToUrf.json):」。**
    + **1.2.5.3 設定「Height」為「200」。**
  + **1.2.6 拖拉一個「Standard>TMemo」到「TimeSeriesProcessingSettingsNowJson_GroupBox」中。預設名稱會是「Memo1」，修改「Name」為「TimeSeriesProcessingSettingsNowJson_Memo」**
    + **1.2.6.1 設定「Anchors」。**  
      + **1.2.6.1.1 「Top anchoring>Slbling」選為「TimeSeriesProcessingSettingsNowJson_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。選擇「Top anchoring」右側三個鈕選上面的。** 
      + **1.2.6.1.2 「Left anchoring>Slbling」選為「TimeSeriesProcessingSettingsNowJson_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.2.6.1.3 「Right anchoring>Slbling」選為「TimeSeriesProcessingSettingsNowJson_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
      + **1.2.6.1.4 「Bottom anchoring>Slbling」選為「TimeSeriesProcessingSettingsNowJson_GroupBox:TGroupBox」。勾選「Bottom anchoring>Enable」為打勾。選擇「Bottom anchoring」右側三個鈕選下面的。** 
      + **1.2.6.1.5 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」，下面那格填「3」。**  
      + **1.2.6.1.6 設定完後關閉Anchors設定視窗。**  
    + **1.2.6.2 設定「Lines」清空。**
    + **1.2.6.3 設定「ReadOnly」為「True」。**
    + **1.2.6.4 設定「ScrollBars」為「ssBoth」。**
    + **1.2.6.5 設定「WordWrap」為「False」。**
  + **1.2.7 拖拉一個「Standard>TGroupBox」到「TimeSeriesProcessingSettings_TabSheet」中。預設名稱會是「GroupBox1」，修改「Name」為「TimeSeriesProcessingSettingsReadme_GroupBox」**
    + **1.2.7.1 設定「Anchors」。**  
      + **1.2.7.1.1 「Top anchoring>Slbling」選為「TimeSeriesProcessingSettingsNowJson_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。選擇「Top anchoring」右側三個鈕選下面的。** 
      + **1.2.7.1.2 「Left anchoring>Slbling」選為「TimeSeriesProcessingSettingsNowJson_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.2.7.1.3 「Right anchoring>Slbling」選為「TimeSeriesProcessingSettingsNowJson_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**
      + **1.2.7.1.4 「Bottom anchoring>Slbling」選為「TimeSeriesProcessingSettings_TabSheet:TTabSheet」。勾選「Bottom anchoring>Enable」為打勾。選擇「Bottom anchoring」右側三個鈕選下面的。** 
      + **1.2.7.1.5 設定完後關閉Anchors設定視窗。**  
    + **1.2.7.2 設定「Caption」為「說明:」。**
  + **1.2.8 拖拉一個「Standard>TMemo」到「TimeSeriesProcessingSettingsReadme_GroupBox」中。預設名稱會是「Memo1」，修改「Name」為「TimeSeriesProcessingSettingsReadme_Memo」**
    + **1.2.8.1 設定「Anchors」。**  
      + **1.2.8.1.1 「Top anchoring>Slbling」選為「TimeSeriesProcessingSettingsReadme_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。選擇「Top anchoring」右側三個鈕選上面的。** 
      + **1.2.8.1.2 「Left anchoring>Slbling」選為「TimeSeriesProcessingSettingsReadme_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.2.8.1.3 「Right anchoring>Slbling」選為「TimeSeriesProcessingSettingsReadme_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
      + **1.2.8.1.4 「Bottom anchoring>Slbling」選為「TimeSeriesProcessingSettingsReadme_GroupBox:TGroupBox」。勾選「Bottom anchoring>Enable」為打勾。選擇「Bottom anchoring」右側三個鈕選下面的。** 
      + **1.2.8.1.5 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」，下面那格填「3」。**  
      + **1.2.8.1.6 設定完後關閉Anchors設定視窗。**  
    + **1.2.8.2 設定「Lines」清空。**
    + **1.2.8.3 設定「ReadOnly」為「True」。**
    + **1.2.8.4 設定「ScrollBars」為「ssBoth」。**
    + **1.2.8.5 設定「WordWrap」為「False」。**
## 開發紀錄(寫功能部分)
### 版本
+ 2.1選Form，去編輯FormCreate，修改部分:
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
### 修改功能
+ 2.2 去修改「CreateMeshOpenOutputFolder_ToolButton」的「Event」頁面下「OnClick」為如下程式碼。
```
procedure TForm1.CreateMeshOpenOutputFolder_ToolButtonClick(Sender: TObject);
var
  temp_str: string;
begin
  // 取得資料夾路徑
  temp_str:=CreateMeshSettingsDefaultJson_Memo.Lines.Strings[41-1];
  temp_str:=StringReplace(temp_str, '"OutputFile06_BasicMeshPNG_FileName":"', '', [rfReplaceAll]);
  temp_str:=StringReplace(temp_str, '",', '', [rfReplaceAll]);
  temp_str:=ExtractFilePath(Application.ExeName)+temp_str;
  temp_str:=ExtractFilePath(temp_str);
  ForceDirectories(temp_str);
  // 記得在 uses 區塊中加入 Windows, ShellApi
  // 使用 ShellExecute 打開該資料夾
  //ShellExecute(0, 'open', PChar(temp_str), nil, nil, SW_SHOWNORMAL);//這個不支援中文，改用ShellExecuteW
  ShellExecuteW(0, 'open', PWideChar(UTF8ToUTF16(temp_str)), nil, nil, SW_SHOWNORMAL);
end; 
```
+ 2.3 去修改「CreateMeshOpenOutputFolder_ToolButton」的「Event」頁面下「OnClick」為如下程式碼。
```
procedure TForm1.ForwardModelingOpenOutputFolder_ToolButtonClick(Sender: TObject
  );
var
  temp_str: string;
begin
  // 取得資料夾路徑
  temp_str:=ForwardModelingSettingsDefaultJson_Memo.Lines.Strings[25-1];
  temp_str:=StringReplace(temp_str, '"OutputFolderPath":"', '', [rfReplaceAll]);
  temp_str:=StringReplace(temp_str, '",', '', [rfReplaceAll]);
  temp_str:=ExtractFilePath(Application.ExeName)+temp_str;
  temp_str:=ExtractFilePath(temp_str);
  ForceDirectories(temp_str);
  // 記得在 uses 區塊中加入 Windows, ShellApi
  // 使用 ShellExecute 打開該資料夾
  //ShellExecute(0, 'open', PChar(temp_str), nil, nil, SW_SHOWNORMAL);//這個不支援中文，改用ShellExecuteW
  ShellExecuteW(0, 'open', PWideChar(UTF8ToUTF16(temp_str)), nil, nil, SW_SHOWNORMAL);
end;  
```
### 寫功能
+ 2.4拖拉一個「Dialog>TOpenDialog」到「Form1」中。預設名稱會是「OpenDialog1」。
+ 2.5 去修改「TimeSeriesProcessingInput01Geo_ManualSelect_Button」的「Event」頁面下「OnClick」為如下程式碼。
```pascal
procedure TForm1.TimeSeriesProcessingInput01Geo_ManualSelect_ButtonClick(
  Sender: TObject);
begin
  //--------------------------------------------------------------------------
  // OpenDialog1設定對話框標題
  OpenDialog1.Title := '請選擇一個geo檔案';
  // 設定預設開啟的目錄
  OpenDialog1.InitialDir := Current_Folder_Path;
  // 設定過濾檔案名稱
  OpenDialog1.Filter := 'geo檔案(*.geo)|*.geo';
  // 設定開啟選項 (Options)
  // ofFileMustExist: 強制檔案必須存在，否則會跳出警告
  // ofPathMustExist: 強制路徑必須存在
  // ofEnableSizing: 允許調整對話框大小
  OpenDialog1.Options := OpenDialog1.Options + [ofFileMustExist, ofPathMustExist];
  //--
  // 執行選取，如果使用者點擊「開啟」則回傳 True
  if OpenDialog1.Execute then
  begin
    // 取得選取的完整路徑
    //ShowMessage('你選擇了：' + OpenDialog1.FileName);
    TimeSeriesProcessingInput01Geo_Edit.Text := OpenDialog1.FileName;
  end
  else
  begin
    // 使用者點擊了「取消」
    //ShowMessage('取消選取');
  end;
end;   
```
+ 2.6 去修改「TimeSeriesProcessingInput01Geo_AutoDetect_Button」的「Event」頁面下「OnClick」為如下程式碼。
```pascal
procedure TForm1.TimeSeriesProcessingInput01Geo_AutoDetect_ButtonClick(
  Sender: TObject);
var
  temp_str: AnsiString;
begin
  temp_str := 'Output_ERTMaker_CreateAndModifyMesh\'+CreateMeshModelName_Edit.Text+'_SyntheticModel.geo';
  TimeSeriesProcessingInput01Geo_Edit.Text := temp_str;
  if FileExists(temp_str) then
  begin
    //Application.MessageBox(PChar(temp_str), '檔案存在!', 64);
  end
  else
  begin
    temp_str := '錯誤:' + #13#10 + 'geo檔案不存在。';
    Application.MessageBox(PChar(temp_str), '錯誤', 16);
    TimeSeriesProcessingInput01Geo_Edit.Text := '';
    Exit;
  end;
end; 
```
+ 2.7 去修改「TimeSeriesProcessingInput01Geo_OpenFolder_Button」的「Event」頁面下「OnClick」為如下程式碼。
```pascal
procedure TForm1.TimeSeriesProcessingInput01Geo_OpenFolder_ButtonClick(
  Sender: TObject);
var
  temp_str: AnsiString;
begin
  // 1. 從完整路徑中提取資料夾路徑
  temp_str := ExtractFilePath(TimeSeriesProcessingInput01Geo_Edit.Text);
  // 2. 檢查路徑是否存在
  if DirectoryExists(temp_str) then
  begin
    // 3. 使用系統預設檔案瀏覽器開啟資料夾
    // 記得在 uses 區塊中加入 Windows, ShellApi
    // 使用 ShellExecute 打開該資料夾
    //ShellExecute(0, 'open', PChar(temp_str), nil, nil, SW_SHOWNORMAL);//這個不支援中文，改用ShellExecuteW
    ShellExecuteW(0, 'open', PWideChar(UTF8ToUTF16(temp_str)), nil, nil, SW_SHOWNORMAL);
  end
  else
  begin
    // 如果路徑無效，給予使用者提示
    temp_str := '錯誤:' + #13#10 +
      '資料夾不存在。';
    Application.MessageBox(PChar(temp_str), '錯誤', 16);
  end;
end; 
```
+ 2.8 去修改「TimeSeriesProcessingInput02Ohm_ManualSelect_Button」的「Event」頁面下「OnClick」為如下程式碼。
```pascal
procedure TForm1.TimeSeriesProcessingInput02Ohm_ManualSelect_ButtonClick(
  Sender: TObject);
begin
  //--------------------------------------------------------------------------
  // OpenDialog1設定對話框標題
  OpenDialog1.Title := '請選擇一個ohm檔案';
  // 設定預設開啟的目錄
  OpenDialog1.InitialDir := Current_Folder_Path;
  // 設定過濾檔案名稱
  OpenDialog1.Filter := 'ohm檔案(*.ohm)|*.ohm';
  // 設定開啟選項 (Options)
  // ofFileMustExist: 強制檔案必須存在，否則會跳出警告
  // ofPathMustExist: 強制路徑必須存在
  // ofEnableSizing: 允許調整對話框大小
  OpenDialog1.Options := OpenDialog1.Options + [ofFileMustExist, ofPathMustExist];
  //--
  // 執行選取，如果使用者點擊「開啟」則回傳 True
  if OpenDialog1.Execute then
  begin
    // 取得選取的完整路徑
    //ShowMessage('你選擇了：' + OpenDialog1.FileName);
    TimeSeriesProcessingInput02Ohm_Edit.Text := OpenDialog1.FileName;
  end
  else
  begin
    // 使用者點擊了「取消」
    //ShowMessage('取消選取');
  end;
end;  
```
+ 2.9 去修改「TimeSeriesProcessingInput02Ohm_AutoDetect_Button」的「Event」頁面下「OnClick」為如下程式碼。
```pascal
procedure TForm1.TimeSeriesProcessingInput02Ohm_AutoDetect_ButtonClick(
  Sender: TObject);
var
  temp_str: AnsiString;
begin
  temp_str := 'Output_ERTMaker_CreateAndModifyMesh\'+CreateMeshModelName_Edit.Text+'_SyntheticModel.ohm';
  TimeSeriesProcessingInput02Ohm_Edit.Text := temp_str;
  if FileExists(temp_str) then
  begin
    //Application.MessageBox(PChar(temp_str), '檔案存在!', 64);
  end
  else
  begin
    temp_str := '錯誤:' + #13#10 + 'ohm檔案不存在。';
    Application.MessageBox(PChar(temp_str), '錯誤', 16);
    TimeSeriesProcessingInput02Ohm_Edit.Text := '';
    Exit;
  end;
end;  
```
+ 2.10 去修改「TimeSeriesProcessingInput02Ohm_OpenFolder_Button」的「Event」頁面下「OnClick」為如下程式碼。
```pascal
procedure TForm1.TimeSeriesProcessingInput02Ohm_OpenFolder_ButtonClick(
  Sender: TObject);
var
  temp_str: AnsiString;
begin
  // 1. 從完整路徑中提取資料夾路徑
  temp_str := ExtractFilePath(TimeSeriesProcessingInput02Ohm_Edit.Text);
  // 2. 檢查路徑是否存在
  if DirectoryExists(temp_str) then
  begin
    // 3. 使用系統預設檔案瀏覽器開啟資料夾
    // 記得在 uses 區塊中加入 Windows, ShellApi
    // 使用 ShellExecute 打開該資料夾
    //ShellExecute(0, 'open', PChar(temp_str), nil, nil, SW_SHOWNORMAL);//這個不支援中文，改用ShellExecuteW
    ShellExecuteW(0, 'open', PWideChar(UTF8ToUTF16(temp_str)), nil, nil, SW_SHOWNORMAL);
  end
  else
  begin
    // 如果路徑無效，給予使用者提示
    temp_str := '錯誤:' + #13#10 +
      '資料夾不存在。';
    Application.MessageBox(PChar(temp_str), '錯誤', 16);
  end;
end; 
```
+ 2.11 去修改「TimeSeriesProcessingInput03v299Scsv_ManualSelect_Button」的「Event」頁面下「OnClick」為如下程式碼。
```pascal
procedure TForm1.TimeSeriesProcessingInput03v299Scsv_ManualSelect_ButtonClick(
  Sender: TObject);
begin
  //--------------------------------------------------------------------------
  // OpenDialog1設定對話框標題
  OpenDialog1.Title := '請選擇一個v299Scsv檔案';
  // 設定預設開啟的目錄
  OpenDialog1.InitialDir := Current_Folder_Path;
  // 設定過濾檔案名稱
  OpenDialog1.Filter := 'v299Scsv檔案(*.csv)|*.csv';
  // 設定開啟選項 (Options)
  // ofFileMustExist: 強制檔案必須存在，否則會跳出警告
  // ofPathMustExist: 強制路徑必須存在
  // ofEnableSizing: 允許調整對話框大小
  OpenDialog1.Options := OpenDialog1.Options + [ofFileMustExist, ofPathMustExist];
  //--
  // 執行選取，如果使用者點擊「開啟」則回傳 True
  if OpenDialog1.Execute then
  begin
    // 取得選取的完整路徑
    //ShowMessage('你選擇了：' + OpenDialog1.FileName);
    TimeSeriesProcessingInput03v299Scsv_Edit.Text := OpenDialog1.FileName;
  end
  else
  begin
    // 使用者點擊了「取消」
    //ShowMessage('取消選取');
  end;
end;   
```
+ 2.12 去修改「TimeSeriesProcessingInput03v299Scsv_AutoDetect_Button」的「Event」頁面下「OnClick」為如下程式碼。
```pascal
procedure TForm1.TimeSeriesProcessingInput03v299Scsv_AutoDetect_ButtonClick(
  Sender: TObject);
var
  temp_str: AnsiString;
begin
  temp_str := 'Output_ERTMaker_SimulateForTimeSeries\'+CreateMeshModelName_Edit.Text+'_SyntheticModel_CurrentFlowLinesAB.v299S.csv';
  TimeSeriesProcessingInput03v299Scsv_Edit.Text := temp_str;
  if FileExists(temp_str) then
  begin
    //Application.MessageBox(PChar(temp_str), '檔案存在!', 64);
  end
  else
  begin
    temp_str := '錯誤:' + #13#10 + 'csv檔案不存在。';
    Application.MessageBox(PChar(temp_str), '錯誤', 16);
    TimeSeriesProcessingInput03v299Scsv_Edit.Text := '';
    Exit;
  end;
end;
```
+ 2.13 去修改「TimeSeriesProcessingInput03v299Scsv_OpenFolder_Button」的「Event」頁面下「OnClick」為如下程式碼。
```pascal
procedure TForm1.TimeSeriesProcessingInput03v299Scsv_OpenFolder_ButtonClick(
  Sender: TObject);
var
  temp_str: AnsiString;
begin
  // 1. 從完整路徑中提取資料夾路徑
  temp_str := ExtractFilePath(TimeSeriesProcessingInput03v299Scsv_Edit.Text);
  // 2. 檢查路徑是否存在
  if DirectoryExists(temp_str) then
  begin
    // 3. 使用系統預設檔案瀏覽器開啟資料夾
    // 記得在 uses 區塊中加入 Windows, ShellApi
    // 使用 ShellExecute 打開該資料夾
    //ShellExecute(0, 'open', PChar(temp_str), nil, nil, SW_SHOWNORMAL);//這個不支援中文，改用ShellExecuteW
    ShellExecuteW(0, 'open', PWideChar(UTF8ToUTF16(temp_str)), nil, nil, SW_SHOWNORMAL);
  end
  else
  begin
    // 如果路徑無效，給予使用者提示
    temp_str := '錯誤:' + #13#10 +
      '資料夾不存在。';
    Application.MessageBox(PChar(temp_str), '錯誤', 16);
  end;
end; 
```
+ 2.14 去修改「TimeSeriesProcessingInput04ABMN_ManualSelect_Button」的「Event」頁面下「OnClick」為如下程式碼。
```pascal
procedure TForm1.TimeSeriesProcessingInput04ABMN_ManualSelect_ButtonClick(
  Sender: TObject);
begin
  //--------------------------------------------------------------------------
  // OpenDialog1設定對話框標題
  OpenDialog1.Title := '請選擇一個abmn檔案';
  // 設定預設開啟的目錄
  OpenDialog1.InitialDir := Current_Folder_Path;
  // 設定過濾檔案名稱
  OpenDialog1.Filter := 'abmn檔案(*.abmn)|*.abmn';
  // 設定開啟選項 (Options)
  // ofFileMustExist: 強制檔案必須存在，否則會跳出警告
  // ofPathMustExist: 強制路徑必須存在
  // ofEnableSizing: 允許調整對話框大小
  OpenDialog1.Options := OpenDialog1.Options + [ofFileMustExist, ofPathMustExist];
  //--
  // 執行選取，如果使用者點擊「開啟」則回傳 True
  if OpenDialog1.Execute then
  begin
    // 取得選取的完整路徑
    //ShowMessage('你選擇了：' + OpenDialog1.FileName);
    TimeSeriesProcessingInput04ABMN_Edit.Text := OpenDialog1.FileName;
  end
  else
  begin
    // 使用者點擊了「取消」
    //ShowMessage('取消選取');
  end;
end; 
```
+ 2.15 去修改「TimeSeriesProcessingInput04ABMN_AutoDetect_Button」的「Event」頁面下「OnClick」為如下程式碼。
```pascal
procedure TForm1.TimeSeriesProcessingInput04ABMN_AutoDetect_ButtonClick(
  Sender: TObject);
var
  temp_str: AnsiString;
begin
  temp_str := 'Output_ERTMaker_CreateAndModifyMesh\'+CreateMeshModelName_Edit.Text+'_SyntheticModel.abmn';
  TimeSeriesProcessingInput04ABMN_Edit.Text := temp_str;
  if FileExists(temp_str) then
  begin
    //Application.MessageBox(PChar(temp_str), '檔案存在!', 64);
  end
  else
  begin
    // 檔案不存在，建立一個空白檔案
    try
      // 如果資料夾不存在，ForceDirectories 會自動建立它們
      ForceDirectories(ExtractFilePath(temp_str));
      // 建立一個空白檔案
      TFileStream.Create(temp_str, fmCreate).Free;
      temp_str := '提示:' + #13#10 + '找不到abmn檔案，已自動創建空白檔案！';
      Application.MessageBox(PChar(temp_str), '提示', 64);
    except
      on E: Exception do
      begin
        // 捕捉所有錯誤（例如資料夾不存在、唯讀權限、檔名非法字元等）
        temp_str := '錯誤:' + #13#10 + '建立檔案時發生意外錯誤：' + E.Message;
        Application.MessageBox(PChar(temp_str), '錯誤', 16);
        TimeSeriesProcessingInput04ABMN_Edit.Text := '';
        Exit;
      end;
    end;
  end;
end;
```
+ 2.16 去修改「TimeSeriesProcessingInput04ABMN_OpenFolder_Button」的「Event」頁面下「OnClick」為如下程式碼。
```pascal
procedure TForm1.TimeSeriesProcessingInput04ABMN_OpenFolder_ButtonClick(
  Sender: TObject);
var
  temp_str: AnsiString;
begin
  // 1. 從完整路徑中提取資料夾路徑
  temp_str := ExtractFilePath(TimeSeriesProcessingInput04ABMN_Edit.Text);
  // 2. 檢查路徑是否存在
  if DirectoryExists(temp_str) then
  begin
    // 3. 使用系統預設檔案瀏覽器開啟資料夾
    // 記得在 uses 區塊中加入 Windows, ShellApi
    // 使用 ShellExecute 打開該資料夾
    //ShellExecute(0, 'open', PChar(temp_str), nil, nil, SW_SHOWNORMAL);//這個不支援中文，改用ShellExecuteW
    ShellExecuteW(0, 'open', PWideChar(UTF8ToUTF16(temp_str)), nil, nil, SW_SHOWNORMAL);
  end
  else
  begin
    // 如果路徑無效，給予使用者提示
    temp_str := '錯誤:' + #13#10 +
      '資料夾不存在。';
    Application.MessageBox(PChar(temp_str), '錯誤', 16);
  end;
end; 
```
+ 2.17 拖拉一個「Standerd>TPopupMenu」到「Form1」中。預設名稱會是「PopupMenu1」，修改「Name」為「TimeSeriesProcessingSelectKeepA_PopupMenu」。
  + 2.17.1 點兩下元件，進入編輯模式，建立第一層第一個選單「使用預設值」，「Caption」設為「使用預設值」，「Name」設為「TimeSeriesProcessingSelectKeepA_PopupMenu_1_1」。
    + 2.17.1.1 設定「TimeSeriesProcessingSelectKeepA_PopupMenu_1_1」的「Event」頁面下「OnClick」為如下程式碼。**
    ```pascal
    procedure TForm1.TimeSeriesProcessingSelectKeepA_PopupMenu_1_1Click(
      Sender: TObject);
    begin
      TimeSeriesProcessingSelectKeepA_Edit.Text := '1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64';
    end;  
    ```
+ 2.18 修改「TimeSeriesProcessingSelectKeepA_GroupBox」的「PopupMenu」為「TimeSeriesProcessingSelectKeepA_PopupMenu」。
+ 2.19 拖拉一個「Standerd>TPopupMenu」到「Form1」中。預設名稱會是「PopupMenu1」，修改「Name」為「TimeSeriesProcessingSelectKeepB_PopupMenu」。
  + 2.19.1 點兩下元件，進入編輯模式，建立第一層第一個選單「使用預設值」，「Caption」設為「使用預設值」，「Name」設為「TimeSeriesProcessingSelectKeepB_PopupMenu_1_1」。
    + 2.19.1.1 設定「TimeSeriesProcessingSelectKeepB_PopupMenu_1_1」的「Event」頁面下「OnClick」為如下程式碼。**
    ```pascal
    procedure TForm1.TimeSeriesProcessingSelectKeepB_PopupMenu_1_1Click(
      Sender: TObject);
    begin
      TimeSeriesProcessingSelectKeepB_Edit.Text := '1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64';
    end; 
    ```
+ 2.20 修改「TimeSeriesProcessingSelectKeepB_GroupBox」的「PopupMenu」為「TimeSeriesProcessingSelectKeepB_PopupMenu」。 
+ 2.21 拖拉一個「Standerd>TPopupMenu」到「Form1」中。預設名稱會是「PopupMenu1」，修改「Name」為「TimeSeriesProcessingSelectKeepM_PopupMenu」。
  + 2.21.1 點兩下元件，進入編輯模式，建立第一層第一個選單「使用預設值」，「Caption」設為「使用預設值」，「Name」設為「TimeSeriesProcessingSelectKeepM_PopupMenu_1_1」。
    + 2.21.1.1 設定「TimeSeriesProcessingSelectKeepM_PopupMenu_1_1」的「Event」頁面下「OnClick」為如下程式碼。**
    ```pascal
    procedure TForm1.TimeSeriesProcessingSelectKeepM_PopupMenu_1_1Click(
      Sender: TObject);
    begin
      TimeSeriesProcessingSelectKeepM_Edit.Text := '1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64';
    end; 
    ```
+ 2.22 修改「TimeSeriesProcessingSelectKeepM_GroupBox」的「PopupMenu」為「TimeSeriesProcessingSelectKeepM_PopupMenu」。
+ 2.23 拖拉一個「Standerd>TPopupMenu」到「Form1」中。預設名稱會是「PopupMenu1」，修改「Name」為「TimeSeriesProcessingSelectKeepN_PopupMenu」。
  + 2.23.1 點兩下元件，進入編輯模式，建立第一層第一個選單「使用預設值」，「Caption」設為「使用預設值」，「Name」設為「TimeSeriesProcessingSelectKeepN_PopupMenu_1_1」。
    + 2.23.1.1 設定「TimeSeriesProcessingSelectKeepN_PopupMenu_1_1」的「Event」頁面下「OnClick」為如下程式碼。**
    ```pascal
    procedure TForm1.TimeSeriesProcessingSelectKeepN_PopupMenu_1_1Click(
      Sender: TObject);
    begin
      TimeSeriesProcessingSelectKeepN_Edit.Text := '1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64';
    end; 
    ```
+ 2.24 修改「TimeSeriesProcessingSelectKeepN_GroupBox」的「PopupMenu」為「TimeSeriesProcessingSelectKeepN_PopupMenu」。
+ 2.25 修改「TimeSeriesProcessingSettingsDefaultJson_Memo」的「Lines」為以下文字。
```
{
"v299ScsvToUrf_Version":"v20260217a",
"v299ScsvToUrf_Author":"HsiupoYeh",
"InputFile01_Geo_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"InputFile01_Geo_FileName":"Input_ERTMaker_v299ScsvToUrf/Demo_SyntheticModel.geo",
"InputFile02_Ohm_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"InputFile02_Ohm_FileName":"Input_ERTMaker_v299ScsvToUrf/Demo_SyntheticModel.ohm",
"InputFile03_v299Scsv_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"InputFile03_v299Scsv_FileName":"Input_ERTMaker_v299ScsvToUrf/Demo_SyntheticModel.v299S.csv",
"InputFile04_Keep_ABMN_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"InputFile04_Keep_ABMN_FileName":"Input_ERTMaker_v299ScsvToUrf/Demo_SyntheticModel.abmn",
"OutputFile01_Urf_All_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"OutputFile01_Urf_All_FileName":"Output_ERTMaker_v299ScsvToUrf/Demo_SyntheticModel.v299S.urf",
"OutputFile02_Urf_All_SNR_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"OutputFile02_Urf_All_SNR_FileName":"Output_ERTMaker_v299ScsvToUrf/Demo_SyntheticModel.v299S.SNR.urf",
"OutputFile03_Ohm_All_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"OutputFile03_Ohm_All_FileName":"Output_ERTMaker_v299ScsvToUrf/Demo_SyntheticModel.v299S.ohm",
"OutputFile04_PNG_All_MainFileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"OutputFile04_PNG_All_MainFileName":"Output_ERTMaker_v299ScsvToUrf/Demo_SyntheticModel.v299S",
"OutputFile04_PNG_All_Enable_Readme":"是否啟用輸出PNG。若要啟用請填入字串Yes，若不啟用請填入字串No。輸出PNG每張圖約耗時?",
"OutputFile04_PNG_All_Enable":"No",
"OutputFile05_Urf_WS_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"OutputFile05_Urf_WS_FileName":"Output_ERTMaker_v299ScsvToUrf/Demo_SyntheticModel.v299S.WS.urf",
"OutputFile06_Urf_WS_SNR_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"OutputFile06_Urf_WS_SNR_FileName":"Output_ERTMaker_v299ScsvToUrf/Demo_SyntheticModel.v299S.WS.SNR.urf",
"OutputFile07_Ohm_WS_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"OutputFile07_Ohm_WS_FileName":"Output_ERTMaker_v299ScsvToUrf/Demo_SyntheticModel.v299S.WS.ohm",
"OutputFile08_PNG_WS_MainFileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"OutputFile08_PNG_WS_MainFileName":"Output_ERTMaker_v299ScsvToUrf/Demo_SyntheticModel.v299S.WS",
"OutputFile08_PNG_WS_Enable_Readme":"是否啟用輸出PNG。若要啟用請填入字串Yes，若不啟用請填入字串No。輸出PNG每張圖約耗時?",
"OutputFile08_PNG_WS_Enable":"No",
"OutputFile09_Urf_MGD_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"OutputFile09_Urf_MGD_FileName":"Output_ERTMaker_v299ScsvToUrf/Demo_SyntheticModel.v299S.MGD.urf",
"OutputFile10_Urf_MGD_SNR_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"OutputFile10_Urf_MGD_SNR_FileName":"Output_ERTMaker_v299ScsvToUrf/Demo_SyntheticModel.v299S.MGD.SNR.urf",
"OutputFile11_Ohm_MGD_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"OutputFile11_Ohm_MGD_FileName":"Output_ERTMaker_v299ScsvToUrf/Demo_SyntheticModel.v299S.MGD.ohm",
"OutputFile12_PNG_MGD_MainFileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"OutputFile12_PNG_MGD_MainFileName":"Output_ERTMaker_v299ScsvToUrf/Demo_SyntheticModel.v299S.MGD",
"OutputFile12_PNG_MGD_Enable_Readme":"是否啟用輸出PNG。若要啟用請填入字串Yes，若不啟用請填入字串No。輸出PNG每張圖約耗時?",
"OutputFile12_PNG_MGD_Enable":"No",
"OutputFile13_Urf_MPR_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"OutputFile13_Urf_MPR_FileName":"Output_ERTMaker_v299ScsvToUrf/Demo_SyntheticModel.v299S.MPR.urf",
"OutputFile14_Urf_MPR_SNR_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"OutputFile14_Urf_MPR_SNR_FileName":"Output_ERTMaker_v299ScsvToUrf/Demo_SyntheticModel.v299S.MPR.SNR.urf",
"OutputFile15_Ohm_MPR_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"OutputFile15_Ohm_MPR_FileName":"Output_ERTMaker_v299ScsvToUrf/Demo_SyntheticModel.v299S.MPR.ohm",
"OutputFile16_PNG_MPR_MainFileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"OutputFile16_PNG_MPR_MainFileName":"Output_ERTMaker_v299ScsvToUrf/Demo_SyntheticModel.v299S.MPR",
"OutputFile16_PNG_MPR_Enable_Readme":"是否啟用輸出PNG。若要啟用請填入字串Yes，若不啟用請填入字串No。輸出PNG每張圖約耗時?",
"OutputFile16_PNG_MPR_Enable":"No",
"OutputFile17_Urf_GD_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"OutputFile17_Urf_GD_FileName":"Output_ERTMaker_v299ScsvToUrf/Demo_SyntheticModel.v299S.GD.urf",
"OutputFile18_Urf_GD_SNR_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"OutputFile18_Urf_GD_SNR_FileName":"Output_ERTMaker_v299ScsvToUrf/Demo_SyntheticModel.v299S.GD.SNR.urf",
"OutputFile19_Ohm_GD_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"OutputFile19_Ohm_GD_FileName":"Output_ERTMaker_v299ScsvToUrf/Demo_SyntheticModel.v299S.GD.ohm",
"OutputFile20_PNG_GD_MainFileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"OutputFile20_PNG_GD_MainFileName":"Output_ERTMaker_v299ScsvToUrf/Demo_SyntheticModel.v299S.GD",
"OutputFile20_PNG_GD_Enable_Readme":"是否啟用輸出PNG。若要啟用請填入字串Yes，若不啟用請填入字串No。輸出PNG每張圖約耗時?",
"OutputFile20_PNG_GD_Enable":"No",
"Select_From_Readme":"請填入字串「All」、「WS」、「MGD」、「MPR」或「GD」。",
"Select_From":"All",
"Select_Keep_A_index_Readme":"請填入陣列。例如：「[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64]」",
"Select_Keep_A_index":[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64],
"Select_Keep_B_index_Readme":"請填入陣列。例如：「[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64]」",
"Select_Keep_B_index":[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64],
"Select_Keep_M_index_Readme":"請填入陣列。例如：「[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64]」",
"Select_Keep_M_index":[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64],
"Select_Keep_N_index_Readme":"請填入陣列。例如：「[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64]」",
"Select_Keep_N_index":[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64],
"Select_Keep_ABMN_Enable_Readme":"是否啟用ABMN檔案清單篩選。若要啟用請填入字串Yes，若不啟用請填入字串No。",
"Select_Keep_ABMN_Enable":"No",
"OutputFile21_Urf_Select_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"OutputFile21_Urf_Select_FileName":"Output_ERTMaker_v299ScsvToUrf/Demo_SyntheticModel.v299S.Select.urf",
"OutputFile22_Urf_Select_SNR_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"OutputFile22_Urf_Select_SNR_FileName":"Output_ERTMaker_v299ScsvToUrf/Demo_SyntheticModel.v299S.Select.SNR.urf",
"OutputFile23_Ohm_Select_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"OutputFile23_Ohm_Select_FileName":"Output_ERTMaker_v299ScsvToUrf/Demo_SyntheticModel.v299S.Select.ohm",
"OutputFile24_PNG_Select_MainFileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"OutputFile24_PNG_Select_MainFileName":"Output_ERTMaker_v299ScsvToUrf/Demo_SyntheticModel.v299S.Select",
"OutputFile24_PNG_Select_Enable_Readme":"是否啟用輸出PNG。若要啟用請填入字串Yes，若不啟用請填入字串No。輸出PNG每張圖約耗時?",
"OutputFile24_PNG_Select_Enable":"No"
}
```
+ 2.26 拖拉一個「System>TAsyncProcess」到「Form1」中。預設名稱會是「AsyncProcess1」，修改「Name」為「TimeSeriesProcessing_AsyncProcess」。
+ 2.27 去修改「TimeSeriesProcessing_AsyncProcess」的「Event」頁面下「OnReadData」為如下程式碼。
```pascal
procedure TForm1.TimeSeriesProcessing_AsyncProcessReadData(Sender: TObject);
var
  temp_Buffer:string='';
  temp_BytesAvailable:DWord;
begin
  //--------------------------------------------------------------------------
  // 顯示外部cmd.exe運行內容
  temp_BytesAvailable:=TimeSeriesProcessing_AsyncProcess.Output.NumBytesAvailable;
  if temp_BytesAvailable>0 Then begin
    setlength(temp_Buffer,temp_BytesAvailable);
    TimeSeriesProcessing_AsyncProcess.Output.Read(temp_Buffer[1],temp_BytesAvailable);
    temp_Buffer := StringReplace(temp_Buffer, #13#10, #10, [rfReplaceAll]);
    temp_Buffer := StringReplace(temp_Buffer, #10, #13#10, [rfReplaceAll]);
    TimeSeriesProcessingSettingsCmdLog_Memo.Lines.Add(WinCPToUTF8(temp_Buffer));
  end;
  //--------------------------------------------------------------------------
end;    
```
+ 2.28 去修改「TimeSeriesProcessing_AsyncProcess」的「Event」頁面下「OnTerminate」為如下程式碼。
```pascal
procedure TForm1.TimeSeriesProcessing_AsyncProcessTerminate(Sender: TObject);
var
  temp_str: AnsiString;
begin
  //--------------------------------------------------------------------------
  // 啟用元件
  CreateMeshRun_ToolButton.Enabled:=True;
  ForwardModelingRun_ToolButton.Enabled:=True;
  TimeSeriesProcessingRun_ToolButton.Enabled:=True;
  CreateMeshParameters_GroupBox.Enabled:=True;
  ForwardModelingParameters_GroupBox.Enabled:=True;
  TimeSeriesProcessingParameters_GroupBox.Enabled:=True;
  //--------------------------------------------------------------------------
  //--------------------------------------------------------------------------
  // 更新狀態列
  //--
  if TimeSeriesProcessingSettingsCmdLog_Memo.Lines.Count=0 Then
  begin
    StatusBar1.Panels[0].Text:='運行時間序列解算...異常結束!';
    Exit;
  end;
  //--
  // 檢查結果ohm
  temp_str:=TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[79-1];
  temp_str:=StringReplace(temp_str, '"OutputFile23_Ohm_Select_FileName":"', '', [rfReplaceAll]);
  temp_str:=StringReplace(temp_str, '",', '', [rfReplaceAll]);
  if FileExists(temp_str) then
  begin
    StatusBar1.Panels[0].Text:='運行時間序列解算...完成!';
    // 成功提示
    temp_str := '成功:' + #13#10 +
      '時間序列解算...完成!';
    Application.MessageBox(PChar(temp_str), '提示', 64);
    Exit;
  end
  else
  begin
    StatusBar1.Panels[0].Text:='運行時間序列解算...運作異常，請檢查紀錄!';
    Exit;
  end;
  //--------------------------------------------------------------------------
end; 
```
+ 2.29 原始碼最前面部分宣告修改為如下程式碼。
```
implementation
uses
  RegExpr, LazUTF8, Windows, ShellApi, FileUtil;
//--------------------------------------------------------------------------
//宣告全域變數 add by HsiupoYeh
var
  version_str: AnsiString;
  Current_Folder_Path: AnsiString;
  Form1CreateMeshPreview_ImageWidthDiffPx: Integer;
  Form1CreateMeshPreview_ImageHeightDiffPx: Integer;
  Form1ForwardModelingPreview_ImageWidthDiffPx: Integer;
  Form1ForwardModelingPreview_ImageHeightDiffPx: Integer;
//--------------------------------------------------------------------------

{$R *.lfm}
```
+ 2.30 去修改「TimeSeriesProcessingRun_ToolButton」的「Event」頁面下「OnClick」為如下程式碼。
```pascal
procedure TForm1.TimeSeriesProcessingRun_ToolButtonClick(Sender: TObject);
var
  temp_str: AnsiString;
begin
  //--------------------------------------------------------------------------
  TimeSeriesProcessingSettingsNowJson_Memo.Clear;
  //--
  StatusBar1.Panels[0].Text:='運行時間序列解算...請稍後!';
  //--
  if DirectoryExists('Output_ERTMaker_v299ScsvToUrf') then
  begin
    // 記得在 uses 區塊中加入 FileUtil
    DeleteDirectory('Output_ERTMaker_v299ScsvToUrf', True);// 第二個參數 True 表示如果資料夾裡面有檔案也一併刪除
  end;
  //--
  TimeSeriesProcessingSettingsNowJson_Memo.Lines:=TimeSeriesProcessingSettingsDefaultJson_Memo.Lines;
  //--------------------------------------------------------------------------
  temp_str:=TimeSeriesProcessingInput01Geo_Edit.Text;
  if FileExists(temp_str) then
  begin
    ForceDirectories('Output_ERTMaker_v299ScsvToUrf\Input_v299ScsvToUrf\');
    CopyFileW(PWideChar(UTF8ToUTF16(temp_str)),
          PWideChar(UTF8ToUTF16('Output_ERTMaker_v299ScsvToUrf\Input_v299ScsvToUrf\'+ExtractFileName(temp_str))),
          False);// 第三個參數為 False 表示如果目標已存在則覆蓋 (Overwrite)
    temp_str := 'Output_ERTMaker_v299ScsvToUrf\Input_v299ScsvToUrf\'+ExtractFileName(temp_str);
    temp_str:=StringReplace(temp_str, '\', '/', [rfReplaceAll]);
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[5-1] := ('"InputFile01_Geo_FileName":"'+temp_str+'",');
  end
  else
  begin
    temp_str := '錯誤:' + #13#10 +
      'geo檔案不存在。';
    Application.MessageBox(PChar(temp_str), '錯誤', 16);
    StatusBar1.Panels[0].Text:='錯誤:geo檔案不存在。';
    Exit;
  end;
  //--------------------------------------------------------------------------
  temp_str:=TimeSeriesProcessingInput02Ohm_Edit.Text;
  if FileExists(temp_str) then
  begin
    ForceDirectories('Output_ERTMaker_v299ScsvToUrf\Input_v299ScsvToUrf\');
    CopyFileW(PWideChar(UTF8ToUTF16(temp_str)),
          PWideChar(UTF8ToUTF16('Output_ERTMaker_v299ScsvToUrf\Input_v299ScsvToUrf\'+ExtractFileName(temp_str))),
          False);// 第三個參數為 False 表示如果目標已存在則覆蓋 (Overwrite)
    temp_str := 'Output_ERTMaker_v299ScsvToUrf\Input_v299ScsvToUrf\'+ExtractFileName(temp_str);
    temp_str:=StringReplace(temp_str, '\', '/', [rfReplaceAll]);
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[7-1] := ('"InputFile02_Ohm_FileName":"'+temp_str+'",');
  end
  else
  begin
    temp_str := '錯誤:' + #13#10 +
      'ohm檔案不存在。';
    Application.MessageBox(PChar(temp_str), '錯誤', 16);
    StatusBar1.Panels[0].Text:='錯誤:ohm檔案不存在。';
    Exit;
  end;
  //--------------------------------------------------------------------------
  temp_str:=TimeSeriesProcessingInput03v299Scsv_Edit.Text;
  if FileExists(temp_str) then
  begin
    ForceDirectories('Output_ERTMaker_v299ScsvToUrf\Input_v299ScsvToUrf\');
    CopyFileW(PWideChar(UTF8ToUTF16(temp_str)),
          PWideChar(UTF8ToUTF16('Output_ERTMaker_v299ScsvToUrf\Input_v299ScsvToUrf\'+ExtractFileName(temp_str))),
          False);// 第三個參數為 False 表示如果目標已存在則覆蓋 (Overwrite)
    //--
    temp_str := 'Output_ERTMaker_v299ScsvToUrf\Input_v299ScsvToUrf\'+ExtractFileName(temp_str);
    temp_str  :=StringReplace(temp_str, '\', '/', [rfReplaceAll]);
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[9-1] := ('"InputFile03_v299Scsv_FileName":"'+temp_str+'",');
    //--
    temp_str := 'Output_ERTMaker_v299ScsvToUrf\Output_v299ScsvToUrf\'+ChangeFileExt(ExtractFileName(temp_str),'.urf');
    temp_str := StringReplace(temp_str, '\', '/', [rfReplaceAll]);
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[13-1] := ('"OutputFile01_Urf_All_FileName":"'+temp_str+'",');
    //--
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[15-1] := TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[13-1];
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[15-1] := StringReplace(TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[15-1], 'OutputFile01_Urf_All_FileName', 'OutputFile02_Urf_All_SNR_FileName', [rfReplaceAll]);
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[15-1] := StringReplace(TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[15-1], '.urf"', '.SNR.urf"', [rfReplaceAll]);
    //--
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[17-1] := TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[13-1];
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[17-1] := StringReplace(TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[17-1], 'OutputFile01_Urf_All_FileName', 'OutputFile03_Ohm_All_FileName', [rfReplaceAll]);
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[17-1] := StringReplace(TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[17-1], '.urf"', '.ohm"', [rfReplaceAll]);
    //--
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[19-1] := TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[13-1];
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[19-1] := StringReplace(TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[19-1], 'OutputFile01_Urf_All_FileName', 'OutputFile04_PNG_All_MainFileName', [rfReplaceAll]);
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[19-1] := StringReplace(TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[19-1], '.urf"', '"', [rfReplaceAll]);
    //--
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[23-1] := TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[13-1];
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[23-1] := StringReplace(TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[23-1], 'OutputFile01_Urf_All_FileName', 'OutputFile05_Urf_WS_FileName', [rfReplaceAll]);
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[23-1] := StringReplace(TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[23-1], '.urf"', '.WS.urf"', [rfReplaceAll]);
    //--
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[25-1] := TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[13-1];
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[25-1] := StringReplace(TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[25-1], 'OutputFile01_Urf_All_FileName', 'OutputFile06_Urf_WS_SNR_FileName', [rfReplaceAll]);
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[25-1] := StringReplace(TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[25-1], '.urf"', '.WS.SNR.urf"', [rfReplaceAll]);
    //--
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[27-1] := TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[13-1];
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[27-1] := StringReplace(TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[27-1], 'OutputFile01_Urf_All_FileName', 'OutputFile07_Ohm_WS_FileName', [rfReplaceAll]);
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[27-1] := StringReplace(TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[27-1], '.urf"', '.WS.ohm"', [rfReplaceAll]);
    //--
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[29-1] := TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[13-1];
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[29-1] := StringReplace(TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[29-1], 'OutputFile01_Urf_All_FileName', 'OutputFile08_PNG_WS_MainFileName', [rfReplaceAll]);
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[29-1] := StringReplace(TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[29-1], '.urf"', '.WS"', [rfReplaceAll]);
    //--
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[33-1] := TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[13-1];
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[33-1] := StringReplace(TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[33-1], 'OutputFile01_Urf_All_FileName', 'OutputFile09_Urf_MGD_FileName', [rfReplaceAll]);
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[33-1] := StringReplace(TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[33-1], '.urf"', '.MGD.urf"', [rfReplaceAll]);
    //--
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[35-1] := TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[13-1];
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[35-1] := StringReplace(TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[35-1], 'OutputFile01_Urf_All_FileName', 'OutputFile10_Urf_MGD_SNR_FileName', [rfReplaceAll]);
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[35-1] := StringReplace(TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[35-1], '.urf"', '.MGD.SNR.urf"', [rfReplaceAll]);
    //--
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[37-1] := TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[13-1];
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[37-1] := StringReplace(TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[37-1], 'OutputFile01_Urf_All_FileName', 'OutputFile11_Ohm_MGD_FileName', [rfReplaceAll]);
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[37-1] := StringReplace(TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[37-1], '.urf"', '.MGD.ohm"', [rfReplaceAll]);
    //--
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[39-1] := TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[13-1];
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[39-1] := StringReplace(TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[39-1], 'OutputFile01_Urf_All_FileName', 'OutputFile12_PNG_MGD_MainFileName', [rfReplaceAll]);
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[39-1] := StringReplace(TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[39-1], '.urf"', '.MGD"', [rfReplaceAll]);
    //--
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[43-1] := TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[13-1];
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[43-1] := StringReplace(TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[43-1], 'OutputFile01_Urf_All_FileName', 'OutputFile13_Urf_MPR_FileName', [rfReplaceAll]);
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[43-1] := StringReplace(TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[43-1], '.urf"', '.MPR.urf"', [rfReplaceAll]);
    //--
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[45-1] := TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[13-1];
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[45-1] := StringReplace(TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[45-1], 'OutputFile01_Urf_All_FileName', 'OutputFile14_Urf_MPR_SNR_FileName', [rfReplaceAll]);
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[45-1] := StringReplace(TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[45-1], '.urf"', '.MPR.SNR.urf"', [rfReplaceAll]);
    //--
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[47-1] := TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[13-1];
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[47-1] := StringReplace(TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[47-1], 'OutputFile01_Urf_All_FileName', 'OutputFile15_Ohm_MPR_FileName', [rfReplaceAll]);
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[47-1] := StringReplace(TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[47-1], '.urf"', '.MPR.ohm"', [rfReplaceAll]);
    //--
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[49-1] := TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[13-1];
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[49-1] := StringReplace(TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[49-1], 'OutputFile01_Urf_All_FileName', 'OutputFile16_PNG_MPR_MainFileName', [rfReplaceAll]);
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[49-1] := StringReplace(TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[49-1], '.urf"', '.MPR"', [rfReplaceAll]);
    //--
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[53-1] := TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[13-1];
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[53-1] := StringReplace(TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[53-1], 'OutputFile01_Urf_All_FileName', 'OutputFile17_Urf_GD_FileName', [rfReplaceAll]);
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[53-1] := StringReplace(TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[53-1], '.urf"', '.GD.urf"', [rfReplaceAll]);
    //--
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[55-1] := TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[13-1];
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[55-1] := StringReplace(TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[55-1], 'OutputFile01_Urf_All_FileName', 'OutputFile18_Urf_GD_SNR_FileName', [rfReplaceAll]);
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[55-1] := StringReplace(TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[55-1], '.urf"', '.GD.SNR.urf"', [rfReplaceAll]);
    //--
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[57-1] := TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[13-1];
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[57-1] := StringReplace(TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[57-1], 'OutputFile01_Urf_All_FileName', 'OutputFile19_Ohm_GD_FileName', [rfReplaceAll]);
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[57-1] := StringReplace(TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[57-1], '.urf"', '.GD.ohm"', [rfReplaceAll]);
    //--
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[59-1] := TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[13-1];
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[59-1] := StringReplace(TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[59-1], 'OutputFile01_Urf_All_FileName', 'OutputFile20_PNG_GD_MainFileName', [rfReplaceAll]);
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[59-1] := StringReplace(TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[59-1], '.urf"', '.GD"', [rfReplaceAll]);
    //--
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[75-1] := TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[13-1];
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[75-1] := StringReplace(TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[75-1], 'OutputFile01_Urf_All_FileName', 'OutputFile21_Urf_Select_FileName', [rfReplaceAll]);
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[75-1] := StringReplace(TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[75-1], '.urf"', '.Select.urf"', [rfReplaceAll]);
    //--
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[77-1] := TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[13-1];
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[77-1] := StringReplace(TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[77-1], 'OutputFile01_Urf_All_FileName', 'OutputFile22_Urf_Select_SNR_FileName', [rfReplaceAll]);
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[77-1] := StringReplace(TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[77-1], '.urf"', '.Select.SNR.urf"', [rfReplaceAll]);
    //--
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[79-1] := TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[13-1];
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[79-1] := StringReplace(TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[79-1], 'OutputFile01_Urf_All_FileName', 'OutputFile23_Ohm_Select_FileName', [rfReplaceAll]);
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[79-1] := StringReplace(TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[79-1], '.urf"', '.Select.ohm"', [rfReplaceAll]);
    //--
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[81-1] := TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[13-1];
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[81-1] := StringReplace(TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[81-1], 'OutputFile01_Urf_All_FileName', 'OutputFile24_PNG_Select_MainFileName', [rfReplaceAll]);
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[81-1] := StringReplace(TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[81-1], '.urf"', '.Select"', [rfReplaceAll]);
    //--
  end
  else
  begin
    temp_str := '錯誤:' + #13#10 +
      'csv檔案不存在。';
    Application.MessageBox(PChar(temp_str), '錯誤', 16);
    StatusBar1.Panels[0].Text:='錯誤:csv檔案不存在。';
    Exit;
  end;
  //--------------------------------------------------------------------------
  temp_str:=TimeSeriesProcessingInput04ABMN_Edit.Text;
  if FileExists(temp_str) then
  begin
    // ABMN檔案是否為空，空的就關閉檔案清單篩選
    if (FileSize(temp_str) > 0) then
    begin
      TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[73-1] := '"Select_Keep_ABMN_Enable":"Yes",';
    end
    else
    begin
      TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[73-1] := '"Select_Keep_ABMN_Enable":"No",';
    end;
    ForceDirectories('Output_ERTMaker_v299ScsvToUrf\Input_v299ScsvToUrf\');
    CopyFileW(PWideChar(UTF8ToUTF16(temp_str)),
          PWideChar(UTF8ToUTF16('Output_ERTMaker_v299ScsvToUrf\Input_v299ScsvToUrf\'+ExtractFileName(temp_str))),
          False);// 第三個參數為 False 表示如果目標已存在則覆蓋 (Overwrite)
    temp_str := 'Output_ERTMaker_v299ScsvToUrf\Input_v299ScsvToUrf\'+ExtractFileName(temp_str);
    temp_str:=StringReplace(temp_str, '\', '/', [rfReplaceAll]);
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[11-1] := ('"InputFile04_Keep_ABMN_FileName":"'+temp_str+'",');
  end
  else
  begin
    temp_str := '錯誤:' + #13#10 +
      'abmn檔案不存在。';
    Application.MessageBox(PChar(temp_str), '錯誤', 16);
    StatusBar1.Panels[0].Text:='錯誤:abmn檔案不存在。';
    Exit;
  end;
  //--------------------------------------------------------------------------
  if TimeSeriesProcessingOutputPNG_All_CheckBox.Checked then
  begin
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[21-1] := '"OutputFile04_PNG_All_Enable":"Yes",';
  end;
  if TimeSeriesProcessingOutputPNG_WS_CheckBox.Checked then
  begin
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[31-1] := '"OutputFile08_PNG_WS_Enable":"Yes",';
  end;
  if TimeSeriesProcessingOutputPNG_MGD_CheckBox.Checked then
  begin
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[41-1] := '"OutputFile12_PNG_MGD_Enable":"Yes",';
  end;
  if TimeSeriesProcessingOutputPNG_MPR_CheckBox.Checked then
  begin
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[51-1] := '"OutputFile16_PNG_MPR_Enable":"Yes",';
  end;
  if TimeSeriesProcessingOutputPNG_GD_CheckBox.Checked then
  begin
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[61-1] := '"OutputFile20_PNG_GD_Enable":"Yes",';
  end;
  if TimeSeriesProcessingOutputPNG_Select_CheckBox.Checked then
  begin
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[83-1] := '"OutputFile24_PNG_Select_Enable":"Yes",';
  end;
  //--------------------------------------------------------------------------
  if TimeSeriesProcessingSelectFrom_All_RadioButton.Checked then
  begin
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[63-1] := '"Select_From":"All",';
  end;
  if TimeSeriesProcessingSelectFrom_WS_RadioButton.Checked then
  begin
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[63-1] := '"Select_From":"WS",';
  end;
  if TimeSeriesProcessingSelectFrom_MGD_RadioButton.Checked then
  begin
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[63-1] := '"Select_From":"MGD",';
  end;
  if TimeSeriesProcessingSelectFrom_MPR_RadioButton.Checked then
  begin
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[63-1] := '"Select_From":"MPR",';
  end;
  if TimeSeriesProcessingSelectFrom_GD_RadioButton.Checked then
  begin
    TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[63-1] := '"Select_From":"GD",';
  end;
  //--------------------------------------------------------------------------
  TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[65-1] := ('"Select_Keep_A_index":['+TimeSeriesProcessingSelectKeepA_Edit.Text+'],');
  TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[67-1] := ('"Select_Keep_B_index":['+TimeSeriesProcessingSelectKeepB_Edit.Text+'],');
  TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[69-1] := ('"Select_Keep_M_index":['+TimeSeriesProcessingSelectKeepM_Edit.Text+'],');
  TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[71-1] := ('"Select_Keep_N_index":['+TimeSeriesProcessingSelectKeepN_Edit.Text+'],');
  //--------------------------------------------------------------------------
  //--------------------------------------------------------------------------
  // 儲存預設JSON檔案
  ForceDirectories('Input_ERTMaker_v299ScsvToUrf');
  TimeSeriesProcessingSettingsNowJson_Memo.Lines.SaveToFile('Input_ERTMaker_v299ScsvToUrf/v299ScsvToUrf.json',TEncoding.UTF8);
  TimeSeriesProcessingSettingsNowJson_Memo.Lines.SaveToFile('Output_ERTMaker_v299ScsvToUrf/Input_v299ScsvToUrf/v299ScsvToUrf.json',TEncoding.UTF8);
  //--------------------------------------------------------------------------
  //--------------------------------------------------------------------------
  // 外部Python程式檢查
  if not FileExists('PythonEnv\Python.exe') then
  begin
    temp_str := '錯誤:' + #13#10 +
      '外部Python環境不存在。';
    Application.MessageBox(PChar(temp_str), '錯誤', 16);
    StatusBar1.Panels[0].Text:='錯誤:外部Python環境不存在。';
    Exit;
  end;
  if not FileExists('ERTMaker_v299ScsvToUrf_v20260217a.cpython-312.pyc') then
  begin
    temp_str := '錯誤:' + #13#10 +
      '外部Python程式不存在。';
    Application.MessageBox(PChar(temp_str), '錯誤', 16);
    StatusBar1.Panels[0].Text:='錯誤:外部Python程式不存在。';
    Exit;
  end;
  //--------------------------------------------------------------------------
  //--------------------------------------------------------------------------
  TimeSeriesProcessingSettingsCmdLog_Memo.Lines.Clear;
  // 使用 TimeSeriesProcessing_AsyncProcess 運行外部程式
  TimeSeriesProcessing_AsyncProcess.Executable:='cmd.exe';
  TimeSeriesProcessing_AsyncProcess.Parameters.Clear;
  TimeSeriesProcessing_AsyncProcess.Parameters.Add('/c');
  TimeSeriesProcessing_AsyncProcess.Parameters.Add('.\PythonEnv\Python.exe -u ERTMaker_v299ScsvToUrf_v20260217a.cpython-312.pyc');
  TimeSeriesProcessing_AsyncProcess.Options:=[poUsePipes] + [poNoConsole];
  TimeSeriesProcessing_AsyncProcess.Execute;
  // 禁用元件，等背景運作完再從事件重新啟用按鈕
  CreateMeshRun_ToolButton.Enabled:=False;
  ForwardModelingRun_ToolButton.Enabled:=False;
  TimeSeriesProcessingRun_ToolButton.Enabled:=False;
  CreateMeshParameters_GroupBox.Enabled:=False;
  ForwardModelingParameters_GroupBox.Enabled:=False;
  TimeSeriesProcessingParameters_GroupBox.Enabled:=False;
  //--------------------------------------------------------------------------
end;
```
+ 2.31 去修改「TimeSeriesProcessingOpenOutputFolder_ToolButton」的「Event」頁面下「OnClick」為如下程式碼。
```pascal
procedure TForm1.TimeSeriesProcessingOpenOutputFolder_ToolButtonClick(
  Sender: TObject);
var
  temp_str: string;
begin
  // 取得資料夾路徑
  temp_str:='Output_ERTMaker_v299ScsvToUrf';
  ForceDirectories(temp_str);
  // 記得在 uses 區塊中加入 Windows, ShellApi
  // 使用 ShellExecute 打開該資料夾
  //ShellExecute(0, 'open', PChar(temp_str), nil, nil, SW_SHOWNORMAL);//這個不支援中文，改用ShellExecuteW
  ShellExecuteW(0, 'open', PWideChar(UTF8ToUTF16(temp_str)), nil, nil, SW_SHOWNORMAL);
end;   
```


