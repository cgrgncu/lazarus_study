# R2MS_Lite_ERTMaker 開發筆記
+ 作者: HsiupoYeh  
+ 更新日期: 2026-04-10
+ 從 **R2MS_Lite_ERTMaker_v1.0(v20260407a).md** 開始，修正錯誤。

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
+ 1. 繼續編輯有關第2個分頁「逆推工具」(Inversion_TabSheet):  
  + **1.1 拖拉一個「Common Controls>TPageControl」到「Inversion_TabSheet」中。預設名稱會是「PageControl1」，修改「Name」為「Inversion_PageControl」。**  
    + **1.1.1 設定「Align」為「AlClient」。**
    + **1.1.2 設定「TabStop」為「False」。**
  + **1.2 在「Inversion_PageControl」元件上按「右鍵>Add Page」，會增加一個新頁面。預設名稱會是「TabSheet1」，修改「Name」為「InversionModeling_TabSheet」。**
    + **1.2.1 設定「Caption」為「逆推模擬運算」。**
  + **1.3 在「Inversion_PageControl」元件上按「右鍵>Add Page」，會增加一個新頁面。預設名稱會是「TabSheet1」，修改「Name」為「InversionModelingSettings_TabSheet」。**
    + **1.3.1 設定「Caption」為「逆推模擬運算參數」。**
  + **1.4 在「Forward_PageControl」元件上按「右鍵>Add Page」，會增加一個新頁面。預設名稱會是「TabSheet1」，修改「Name」為「InversionInputMeshPreview_TabSheet」。**
    + **1.4.1 設定「Caption」為「輸入網格預覽」。**
  + **1.5 在「Forward_PageControl」元件上按「右鍵>Add Page」，會增加一個新頁面。預設名稱會是「TabSheet1」，修改「Name」為「InversionInputObsDataPreview_TabSheet」。**
    + **1.5.1 設定「Caption」為「觀測資料預覽」。**
  + **1.6 在「Forward_PageControl」元件上按「右鍵>Add Page」，會增加一個新頁面。預設名稱會是「TabSheet1」，修改「Name」為「InversionResultPreview_TabSheet」。**
    + **1.6.1 設定「Caption」為「逆推結果展示」。**
  + **1.7 在「Forward_PageControl」元件上按「右鍵>Add Page」，會增加一個新頁面。預設名稱會是「TabSheet1」，修改「Name」為「InversionAdvancedAnalysisInfo_TabSheet」。**
    + **1.7.1 設定「Caption」為「進階分析資訊」。**
  + **1.8 略過。**
  + **1.9 略過。**
  + 1.10 第1個分頁「逆推模擬運算」(InversionModeling_TabSheet):
    + **1.10.1 拖拉一個「Standard>TToolBar」到「InversionModeling_TabSheet」中。預設名稱會是「ToolBar1」，修改「Name」為「InversionModeling_ToolBar」。**
    + **1.10.2 設定「BorderSpacing>Top」為「1」。**
    + **1.10.3 設定「Edge>Borders>ebBottom」為「True」。**
    + **1.10.4 設定「Edge>Borders>ebLeft」為「True」。**
    + **1.10.5 設定「Edge>Borders>ebRight」為「True」。**
    + **1.10.6 設定「ShowCaptions」為「True」。**
    + **1.10.7 在「ToolBar1」元件上按「右鍵>New Button」，會增加一個新按鈕。預設名稱會是「ToolButton1」，修改「Name」為「InversionModelingRun_ToolButton」**
      + **1.10.7.1 設定「Caption」為「運行逆推模擬」。**
    + **1.10.8 在「ToolBar1」元件上按「右鍵>New Button」，會增加一個新按鈕。預設名稱會是「ToolButton1」，修改「Name」為「InversionModelingOpenOutputFolder_ToolButton」**
      + **1.10.8.1 設定「Caption」為「開啟輸出資料夾」。**
    + **1.10.9 拖拉一個「Standard>TGroupBox」到「InversionModeling_TabSheet」中。預設名稱會是「GroupBox1」，修改「Name」為「InversionModelingParameters_GroupBox」。**
      + **1.10.9.1 設定「Align」為「alClient」。**
      + **1.10.9.2 設定「BorderSpacing>Right」為「1」。**
      + **1.10.9.3 設定「Caption」為「參數設定:」。**
    + **1.10.10 拖拉一個「Standard>TGroupBox」到「InversionModelingParameters_GroupBox」中。預設名稱會是「GroupBox1」，修改「Name」為「InversionModelingInput01Ohm_GroupBox」。**
      + **1.10.10.1 設定「Anchors」。**  
        + **1.10.10.1.1 「Top anchoring>Slbling」選為「InversionModelingParameters_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
        + **1.10.10.1.2 「Left anchoring>Slbling」選為「InversionModelingParameters_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
        + **1.10.10.1.3 「Right anchoring>Slbling」選為「InversionModelingParameters_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
        + **1.10.10.1.4 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
        + **1.10.10.1.5 設定完後關閉Anchors設定視窗。**
      + **1.10.10.2 設定「Caption」為「必要輸入檔案#1(.ohm/.dat):」。**
      + **1.10.10.3 設定「Height」為「48」。**
    + **1.10.11 拖拉一個「Standard>TButton」到「InversionModelingInput01Ohm_GroupBox」中。預設名稱會是「Button1」，修改「Name」為「InversionModelingInput01Ohm_OpenFolder_Button」**
      + **1.10.11.1 設定「Anchors」。**  
        + **1.10.11.1.1 「Top anchoring>Slbling」選為「InversionModelingInput01Ohm_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
        + **1.10.11.1.2 「Left anchoring>Slbling」修改「Left anchoring>Enable」為不打勾。**  
        + **1.10.11.1.3 「Right anchoring>Slbling」選為「InversionModelingInput01Ohm_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
        + **1.10.11.1.4 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
        + **1.10.11.1.5 設定完後關閉Anchors設定視窗。**  
      + **1.10.11.2 設定「Caption」為「開啟資料夾」。**
      + **1.10.11.3 設定「Width」為「75」。**
  + **1.10.12 拖拉一個「Standard>TButton」到「InversionModelingInput01Ohm_GroupBox」中。預設名稱會是「Button1」，修改「Name」為「InversionModelingInput01Ohm_ManualSelect_Button」**
    + **1.10.12.1 設定「Anchors」。**  
      + **1.10.12.1.1 「Top anchoring>Slbling」選為「InversionModelingInput01Ohm_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
      + **1.10.12.1.2 「Left anchoring>Slbling」修改「Left anchoring>Enable」為不打勾。**   
      + **1.10.12.1.3 「Right anchoring>Slbling」選為「InversionModelingInput01Ohm_OpenFolder_Button:TButton」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選左邊的。**  
      + **1.10.12.1.4 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
      + **1.10.12.1.5 設定完後關閉Anchors設定視窗。**  
    + **1.10.12.2 設定「Caption」為「選擇檔案」。**
    + **1.10.12.3 設定「Width」為「75」。**
  + **1.10.13 拖拉一個「Standard>TEdit」到「TimeSeriesProcessingInput01Geo_GroupBox」中。預設名稱會是「Edit1」，修改「Name」為「InversionModelingInput01Ohm_Edit」**
    + **1.10.13.1 設定「Anchors」。**  
      + **1.10.13.1.1 「Top anchoring>Slbling」選為「InversionModelingInput01Ohm_GroupBox:TGroupBox。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**  
      + **1.10.13.1.2 「Left anchoring>Slbling」選為「InversionModelingInput01Ohm_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.10.13.1.3 「Right anchoring>Slbling」選為「InversionModelingInput01Ohm_ManualSelect_Button:TButton」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選左邊的。**  
      + **1.10.13.1.4 「Border space」正中間那格填「0」，上面那格填「1」，左邊那格填「3」，右邊那格填「3」。**  
      + **1.10.13.1.5 設定完後關閉Anchors設定視窗。**  
    + **1.10.13.2 設定「ReadOnly」為「True」。**
    + **1.10.13.3 設定「Text」為空白。**
  + **1.10.14 拖拉一個「Standard>TGroupBox」到「InversionModelingParameters_GroupBox」中。預設名稱會是「GroupBox1」，修改「Name」為「InversionModelingInput02VTK_GroupBox」。**
    + **1.10.14.1 設定「Anchors」。**  
      + **1.10.14.1.1 「Top anchoring>Slbling」選為「InversionModelingInput01Ohm_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選下面的。**
      + **1.10.14.1.2 「Left anchoring>Slbling」選為「InversionModelingParameters_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.10.14.1.3 「Right anchoring>Slbling」選為「InversionModelingParameters_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
      + **1.10.14.1.4 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
      + **1.10.14.1.5 設定完後關閉Anchors設定視窗。**
    + **1.10.14.2 設定「Caption」為「必要輸入檔案#2(.vtk):」。**
    + **1.10.14.3 設定「Height」為「48」。**
  + **1.10.15 拖拉一個「Standard>TButton」到「InversionModelingInput02VTK_GroupBox」中。預設名稱會是「Button1」，修改「Name」為「InversionModelingInput02VTK_OpenFolder_Button」**
    + **1.10.15.1 設定「Anchors」。**  
      + **1.10.15.1.1 「Top anchoring>Slbling」選為「InversionModelingInput02VTK_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
      + **1.10.15.1.2 「Left anchoring>Slbling」修改「Left anchoring>Enable」為不打勾。**  
      + **1.10.15.1.3 「Right anchoring>Slbling」選為「InversionModelingInput02VTK_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
      + **1.10.15.1.4 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
      + **1.10.15.1.5 設定完後關閉Anchors設定視窗。**  
    + **1.10.15.2 設定「Caption」為「開啟資料夾」。**
    + **1.10.15.3 設定「Width」為「75」。**
+ **1.10.16 拖拉一個「Standard>TButton」到「InversionModelingInput02VTK_GroupBox」中。預設名稱會是「Button1」，修改「Name」為「InversionModelingInput02VTK_ManualSelect_Button」**
  + **1.10.16.1 設定「Anchors」。**  
    + **1.10.16.1.1 「Top anchoring>Slbling」選為「InversionModelingInput02VTK_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
    + **1.10.16.1.2 「Left anchoring>Slbling」修改「Left anchoring>Enable」為不打勾。**   
    + **1.10.16.1.3 「Right anchoring>Slbling」選為「InversionModelingInput02VTK_OpenFolder_Button:TButton」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選左邊的。**  
    + **1.10.16.1.4 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
    + **1.10.16.1.5 設定完後關閉Anchors設定視窗。**  
  + **1.10.16.2 設定「Caption」為「選擇檔案」。**
  + **1.10.16.3 設定「Width」為「75」。**
+ **1.10.17 拖拉一個「Standard>TEdit」到「TimeSeriesProcessingInput01Geo_GroupBox」中。預設名稱會是「Edit1」，修改「Name」為「InversionModelingInput02VTK_Edit」**
  + **1.10.17.1 設定「Anchors」。**  
    + **1.10.17.1.1 「Top anchoring>Slbling」選為「InversionModelingInput02VTK_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**  
    + **1.10.17.1.2 「Left anchoring>Slbling」選為「InversionModelingInput02VTK_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
    + **1.10.17.1.3 「Right anchoring>Slbling」選為「InversionModelingInput02VTK_ManualSelect_Button:TButton」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選左邊的。**  
    + **1.10.17.1.4 「Border space」正中間那格填「0」，上面那格填「1」，左邊那格填「3」，右邊那格填「3」。**  
    + **1.10.17.1.5 設定完後關閉Anchors設定視窗。**  
  + **1.10.17.2 設定「ReadOnly」為「True」。**
  + **1.10.17.3 設定「Text」為空白。**
+ **1.10.18 拖拉一個「Standard>TGroupBox」到「InversionModelingParameters_GroupBox」中。預設名稱會是「GroupBox1」，修改「Name」為「InversionModelingInput03MeshJSON_GroupBox」。**
    + **1.10.18.1 設定「Anchors」。**  
      + **1.10.18.1.1 「Top anchoring>Slbling」選為「InversionModelingInput02VTK_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選下面的。**
      + **1.10.18.1.2 「Left anchoring>Slbling」選為「InversionModelingParameters_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.10.18.1.3 「Right anchoring>Slbling」選為「InversionModelingParameters_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
      + **1.10.18.1.4 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
      + **1.10.18.1.5 設定完後關閉Anchors設定視窗。**
    + **1.10.18.2 設定「Caption」為「必要輸入檔案#3(.json):」。**
    + **1.10.18.3 設定「Height」為「48」。**
+ **1.10.19 拖拉一個「Standard>TButton」到「InversionModelingInput03MeshJSON_GroupBox」中。預設名稱會是「Button1」，修改「Name」為「InversionModelingInput03MeshJSON_OpenFolder_Button」**
  + **1.10.19.1 設定「Anchors」。**  
    + **1.10.19.1.1 「Top anchoring>Slbling」選為「InversionModelingInput03MeshJSON_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
    + **1.10.19.1.2 「Left anchoring>Slbling」修改「Left anchoring>Enable」為不打勾。**  
    + **1.10.19.1.3 「Right anchoring>Slbling」選為「InversionModelingInput03MeshJSON_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
    + **1.10.19.1.4 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
    + **1.10.19.1.5 設定完後關閉Anchors設定視窗。**  
  + **1.10.19.2 設定「Caption」為「開啟資料夾」。**
  + **1.10.19.3 設定「Width」為「75」。**
+ **1.10.20 拖拉一個「Standard>TButton」到「InversionModelingInput03MeshJSON_GroupBox」中。預設名稱會是「Button1」，修改「Name」為「InversionModelingInput03MeshJSON_ManualSelect_Button」**
  + **1.10.20.1 設定「Anchors」。**  
    + **1.10.20.1.1 「Top anchoring>Slbling」選為「InversionModelingInput03MeshJSON_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
    + **1.10.20.1.2 「Left anchoring>Slbling」修改「Left anchoring>Enable」為不打勾。**   
    + **1.10.20.1.3 「Right anchoring>Slbling」選為「InversionModelingInput03MeshJSON_OpenFolder_Button:TButton」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選左邊的。**  
    + **1.10.20.1.4 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
    + **1.10.20.1.5 設定完後關閉Anchors設定視窗。**  
  + **1.10.20.2 設定「Caption」為「選擇檔案」。**
  + **1.10.20.3 設定「Width」為「75」。**
+ **1.10.21 拖拉一個「Standard>TEdit」到「InversionModelingInput03MeshJSON_GroupBox」中。預設名稱會是「Edit1」，修改「Name」為「InversionModelingInput03MeshJSON_Edit」**
  + **1.10.21.1 設定「Anchors」。**  
    + **1.10.21.1.1 「Top anchoring>Slbling」選為「InversionModelingInput03MeshJSON_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**  
    + **1.10.21.1.2 「Left anchoring>Slbling」選為「InversionModelingInput03MeshJSON_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
    + **1.10.21.1.3 「Right anchoring>Slbling」選為「InversionModelingInput03MeshJSON_ManualSelect_Button:TButton」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選左邊的。**  
    + **1.10.21.1.4 「Border space」正中間那格填「0」，上面那格填「1」，左邊那格填「3」，右邊那格填「3」。**  
    + **1.10.21.1.5 設定完後關閉Anchors設定視窗。**  
  + **1.10.21.2 設定「ReadOnly」為「True」。**
  + **1.10.21.3 設定「Text」為空白。**
+ **1.10.22 拖拉一個「Standard>TGroupBox」到「InversionModelingParameters_GroupBox」中。預設名稱會是「GroupBox1」，修改「Name」為「InversionModelingInvAutoMesh_GroupBox」。**
    + **1.10.22.1 設定「Anchors」。**  
      + **1.10.22.1.1 「Top anchoring>Slbling」選為「InversionModelingInput03MeshJSON_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選下面的。**
      + **1.10.22.1.2 「Left anchoring>Slbling」選為「InversionModelingParameters_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.10.22.1.3 「Right anchoring>Slbling」選為「InversionModelingParameters_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
      + **1.10.22.1.4 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
      + **1.10.22.1.5 設定完後關閉Anchors設定視窗。**
    + **1.10.22.2 設定「Caption」為「自動產生逆推網格(不啟用本功能時，逆推網格讀取自「必要輸入檔案#2、#3」；若啟用本功能時，免填「必要輸入檔案#2、#3」):」。**
    + **1.10.22.3 設定「Height」為「70」。**
+ **1.10.23 拖拉一個「Standard>TCheckBox」到「InversionModelingInvAutoMesh_GroupBox」中。預設名稱會是「CheckBox1」，修改「Name」為「InversionModelingInvAutoMesh_Enable_CheckBox」**
  + **1.10.23.1 設定「Anchors」。**  
    + **1.10.23.1.1 「Top anchoring>Slbling」選為「InversionModelingInvAutoMesh_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
    + **1.10.23.1.2 「Left anchoring>Slbling」選為「InversionModelingInvAutoMesh_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
    + **1.10.23.1.3 「Border space」正中間那格填「0」，上面那格填「14」，左邊那格填「8」，右邊那格填「8」。**  
    + **1.10.23.1.4 設定完後關閉Anchors設定視窗。**  
  + **1.10.23.2 設定「Caption」為「啟用」。**
+ **1.10.24 拖拉一個「Standard>TGroupBox」到「InversionModelingInvAutoMesh_GroupBox」中。預設名稱會是「GroupBox1」，修改「Name」為「InversionModelingInvAutoMesh_quality_GroupBox」。**
    + **1.10.24.1 設定「Anchors」。**  
      + **1.10.24.1.1 「Top anchoring>Slbling」選為「InversionModelingInvAutoMesh_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
      + **1.10.24.1.2 「Left anchoring>Slbling」選為「InversionModelingInvAutoMesh_Enable_CheckBox:TCheckBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.10.24.1.3 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
      + **1.10.24.1.4 設定完後關閉Anchors設定視窗。**
    + **1.10.24.2 設定「Caption」為「三角網格品質:」。**
    + **1.10.24.3 設定「Height」為「48」。**
    + **1.10.24.4 設定「Hint」為「三角形的最小內角限制角度，單位:度」。**
    + **1.10.24.5 設定「ShowHint」為「True」。**
    + **1.10.24.6 設定「Width」為「100」。**
+ **1.10.25 拖拉一個「Standard>TComboBox」到「InversionModelingInvAutoMesh_quality_GroupBox」中。預設名稱會是「ComboBox1」，修改「Name」為「InversionModelingInvAutoMesh_quality_ComboBox」。**
    + **1.10.25.1 設定「Anchors」。**  
      + **1.10.25.1.1 「Top anchoring>Slbling」選為「InversionModelingInvAutoMesh_quality_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
      + **1.10.25.1.2 「Left anchoring>Slbling」選為「InversionModelingInvAutoMesh_quality_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.10.25.1.3 「Border space」正中間那格填「0」，左邊那格填「8」，右邊那格填「8」。**  
      + **1.10.25.1.4 設定完後關閉Anchors設定視窗。**
    + **1.10.25.2 設定「Enabled」為「False」。**
    + **1.10.25.3 設定「Items」為如下內容。**
    ```
    32.0
    32.1
    32.2
    32.3
    32.4
    32.5
    32.6
    32.7
    32.8
    32.9
    33.0
    33.1
    33.2
    33.3
    33.4
    33.5
    33.6
    33.7
    33.8
    33.9
    34.0
    ```
    + **1.10.25.4 設定「ItemIndex」為「0」。**
    + **1.10.25.5 設定「Style」為「csDropDownList」。**
    + **1.10.25.6 設定「Width」為「80」。** 
+   + 1.3.2 設定「Enabled」為「False」。
  + 1.3.3 設定「Items」為如下內容。
  ```
## 開發紀錄(寫功能部分)
### 版本
+ 2.1選Form，去編輯FormCreate，修改部分:
```pascal
procedure TForm1.FormCreate(Sender: TObject);
begin
  //--------------------------------------------------------------------------
  // 在這裡對全域變數進行初始化
  version_str := 'v1.0(v20260410a)';
  Current_Folder_Path := ExtractFilePath(Application.ExeName);
  //--------------------------------------------------------------------------
  //--------------------------------------------------------------------------
  // 表單標題追加版本
  Form1.Caption := Form1.Caption + ' ' + version_str;
  //--------------------------------------------------------------------------
end;      
```  
