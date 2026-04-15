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
        + **1.10.22.1.3 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
        + **1.10.22.1.4 設定完後關閉Anchors設定視窗。**
      + **1.10.22.2 設定「Caption」為「自動產生逆推網格(不啟用時，逆推網格讀取自「必要輸入檔案#2、#3」；啟用時，免填「必要輸入檔案#2、#3」):」。**
      + **1.10.22.3 設定「Height」為「70」。**
      + **1.10.22.4 設定「Width」為「680」。**
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
      + **1.10.24.4 設定「Hint」為「三角形的最小內角限制角度，單位:度。預設值=32。」。**
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
  + **1.10.26 拖拉一個「Standard>TGroupBox」到「InversionModelingInvAutoMesh_GroupBox」中。預設名稱會是「GroupBox1」，修改「Name」為「InversionModelingInvAutoMesh_paraDepth_GroupBox」。**
      + **1.10.26.1 設定「Anchors」。**  
        + **1.10.26.1.1 「Top anchoring>Slbling」選為「InversionModelingInvAutoMesh_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
        + **1.10.26.1.2 「Left anchoring>Slbling」選為「InversionModelingInvAutoMesh_quality_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選右邊的。**  
        + **1.10.26.1.3 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
        + **1.10.26.1.4 設定完後關閉Anchors設定視窗。**
      + **1.10.26.2 設定「Caption」為「研究區域深度:」。**
      + **1.10.26.3 設定「Height」為「48」。**
      + **1.10.26.4 設定「Hint」為「預設值:-1，單位:[m]，設定小於0的數字會自動設定為測線電極X方向長度的0.4倍。」。**
      + **1.10.26.5 設定「ShowHint」為「True」。**
      + **1.10.26.6 設定「Width」為「100」。**
  + **1.10.27 拖拉一個「Standard>TEdit」到「InversionModelingInvAutoMesh_paraDepth_GroupBox」中。預設名稱會是「Edit1」，修改「Name」為「InversionModelingInvAutoMesh_paraDepth_Edit」**
    + **1.10.27.1 設定「Anchors」。**  
      + **1.10.27.1.1 「Top anchoring>Slbling」選為「InversionModelingInvAutoMesh_paraDepth_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**  
      + **1.10.27.1.2 「Left anchoring>Slbling」選為「InversionModelingInvAutoMesh_paraDepth_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.10.27.1.3 「Right anchoring>Slbling」選為「InversionModelingInvAutoMesh_paraDepth_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
      + **1.10.27.1.4 「Border space」正中間那格填「0」，左邊那格填「8」，右邊那格填「8」。**  
      + **1.10.27.1.5 設定完後關閉Anchors設定視窗。**  
    + **1.10.27.6 設定「Text」為「-1」。**
    + **1.10.27.7 設定「Width」為「80」。**
  + **1.10.28 拖拉一個「Standard>TGroupBox」到「InversionModelingInvAutoMesh_GroupBox」中。預設名稱會是「GroupBox1」，修改「Name」為「InversionModelingInvAutoMesh_boundary_GroupBox」。**
      + **1.10.28.1 設定「Anchors」。**  
        + **1.10.28.1.1 「Top anchoring>Slbling」選為「InversionModelingInvAutoMesh_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
        + **1.10.28.1.2 「Left anchoring>Slbling」選為「InversionModelingInvAutoMesh_paraDepth_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選右邊的。**  
        + **1.10.28.1.3 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
        + **1.10.28.1.4 設定完後關閉Anchors設定視窗。**
      + **1.10.28.2 設定「Caption」為「外邊界倍數:」。**
      + **1.10.28.3 設定「Height」為「48」。**
      + **1.10.28.4 設定「Hint」為「依照此倍數追加左側、右側、下方外網格。追加長度=測線電極X方向長度乘上外邊界倍數。預設值=4。」。**
      + **1.10.28.5 設定「ShowHint」為「True」。**
      + **1.10.28.6 設定「Width」為「100」。**
  + **1.10.29 拖拉一個「Standard>TComboBox」到「InversionModelingInvAutoMesh_quality_GroupBox」中。預設名稱會是「ComboBox1」，修改「Name」為「InversionModelingInvAutoMesh_boundary_ComboBox」。**
      + **1.10.29.1 設定「Anchors」。**  
        + **1.10.29.1.1 「Top anchoring>Slbling」選為「InversionModelingInvAutoMesh_quality_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
        + **1.10.29.1.2 「Left anchoring>Slbling」選為「InversionModelingInvAutoMesh_quality_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
        + **1.10.29.1.3 「Border space」正中間那格填「0」，左邊那格填「8」，右邊那格填「8」。**  
        + **1.10.29.1.4 設定完後關閉Anchors設定視窗。**
      + **1.10.29.2 設定「Enabled」為「False」。**
      + **1.10.29.3 設定「Items」為如下內容。**
      ```
      4
      5
      6
      7
      8
      9
      10
      ```
      + **1.10.29.4 設定「ItemIndex」為「0」。**
      + **1.10.29.5 設定「Style」為「csDropDownList」。**
      + **1.10.29.6 設定「Width」為「80」。**
  + **1.10.30 拖拉一個「Standard>TGroupBox」到「InversionModelingInvAutoMesh_GroupBox」中。預設名稱會是「GroupBox1」，修改「Name」為「InversionModelingInvAutoMesh_paraMaxCellSize_GroupBox」。**
      + **1.10.30.1 設定「Anchors」。**  
        + **1.10.30.1.1 「Top anchoring>Slbling」選為「InversionModelingInvAutoMesh_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
        + **1.10.30.1.2 「Left anchoring>Slbling」選為「InversionModelingInvAutoMesh_boundary_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選右邊的。**  
        + **1.10.30.1.3 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
        + **1.10.30.1.4 設定完後關閉Anchors設定視窗。**
      + **1.10.30.2 設定「Caption」為「網格面積上限:」。**
      + **1.10.30.3 設定「Height」為「48」。**
      + **1.10.30.4 設定「Hint」為「預設值=0，單位:[m^2]，設定0表示不限制單一網格面積上限。」。**
      + **1.10.30.5 設定「ShowHint」為「True」。**
      + **1.10.30.6 設定「Width」為「100」。**
  + **1.10.31 拖拉一個「Standard>TEdit」到「InversionModelingInvAutoMesh_paraMaxCellSize_GroupBox」中。預設名稱會是「Edit1」，修改「Name」為「InversionModelingInvAutoMesh_paraMaxCellSize_Edit」**
    + **1.10.31.1 設定「Anchors」。**  
      + **1.10.31.1.1 「Top anchoring>Slbling」選為「InversionModelingInvAutoMesh_paraMaxCellSize_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**  
      + **1.10.31.1.2 「Left anchoring>Slbling」選為「InversionModelingInvAutoMesh_paraMaxCellSize_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.10.31.1.3 「Right anchoring>Slbling」選為「InversionModelingInvAutoMesh_paraMaxCellSize_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
      + **1.10.31.1.4 「Border space」正中間那格填「0」，左邊那格填「8」，右邊那格填「8」。**  
      + **1.10.31.1.5 設定完後關閉Anchors設定視窗。**  
    + **1.10.31.6 設定「Text」為「0」。**
    + **1.10.31.7 設定「Width」為「80」。**
  + **1.10.32 拖拉一個「Standard>TGroupBox」到「InversionModelingInvAutoMesh_GroupBox」中。預設名稱會是「GroupBox1」，修改「Name」為「InversionModelingInvAutoMesh_addNodes_GroupBox」。**
      + **1.10.32.1 設定「Anchors」。**  
        + **1.10.32.1.1 「Top anchoring>Slbling」選為「InversionModelingInvAutoMesh_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
        + **1.10.32.1.2 「Left anchoring>Slbling」選為「InversionModelingInvAutoMesh_paraMaxCellSize_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選右邊的。**  
        + **1.10.32.1.3 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
        + **1.10.32.1.4 設定完後關閉Anchors設定視窗。**
      + **1.10.32.2 設定「Caption」為「addNodes:」。**
      + **1.10.32.3 設定「Height」為「48」。**
      + **1.10.32.4 設定「Hint」為「限制填入正整數。此值填「1」時無效果，改由paraDX決定增加的方式；填其他正整數則線性均勻增加兩個電極之間的節點數量。預設值=1。」。**
      + **1.10.32.5 設定「ShowHint」為「True」。**
      + **1.10.32.6 設定「Width」為「100」。**
  + **1.10.33 拖拉一個「Standard>TEdit」到「InversionModelingInvAutoMesh_addNodes_GroupBox」中。預設名稱會是「Edit1」，修改「Name」為「InversionModelingInvAutoMesh_addNodes_Edit」**
    + **1.10.33.1 設定「Anchors」。**  
      + **1.10.33.1.1 「Top anchoring>Slbling」選為「InversionModelingInvAutoMesh_addNodes_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**  
      + **1.10.33.1.2 「Left anchoring>Slbling」選為「InversionModelingInvAutoMesh_addNodes_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.10.33.1.3 「Right anchoring>Slbling」選為「InversionModelingInvAutoMesh_addNodes_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
      + **1.10.33.1.4 「Border space」正中間那格填「0」，左邊那格填「8」，右邊那格填「8」。**  
      + **1.10.33.1.5 設定完後關閉Anchors設定視窗。**  
    + **1.10.33.6 設定「Text」為「1」。**
    + **1.10.33.7 設定「Width」為「80」。**
  + **1.10.34 拖拉一個「Standard>TGroupBox」到「InversionModelingInvAutoMesh_GroupBox」中。預設名稱會是「GroupBox1」，修改「Name」為「InversionModelingInvAutoMesh_paraDX_GroupBox」。**
      + **1.10.34.1 設定「Anchors」。**  
        + **1.10.34.1.1 「Top anchoring>Slbling」選為「InversionModelingInvAutoMesh_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
        + **1.10.34.1.2 「Left anchoring>Slbling」選為「InversionModelingInvAutoMesh_addNodes_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選右邊的。**  
        + **1.10.34.1.3 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
        + **1.10.34.1.4 設定完後關閉Anchors設定視窗。**
      + **1.10.34.2 設定「Caption」為「paraDX:」。**
      + **1.10.34.3 設定「Height」為「48」。**
      + **1.10.34.4 設定「Hint」為「限制填入浮點數。僅有addNodes小於等於1的時候有效。此值大於等於0.5時，等於在兩個電極正中間增加一個節點。小於0.5的時候，在每個電極的左右兩邊各增加一個節點，距離是電極間距乘以此浮點數。預設值=1。」。**
      + **1.10.34.5 設定「ShowHint」為「True」。**
      + **1.10.34.6 設定「Width」為「100」。**
  + **1.10.35 拖拉一個「Standard>TEdit」到「InversionModelingInvAutoMesh_paraDX_GroupBox」中。預設名稱會是「Edit1」，修改「Name」為「InversionModelingInvAutoMesh_paraDX_Edit」**
    + **1.10.35.1 設定「Anchors」。**  
      + **1.10.35.1.1 「Top anchoring>Slbling」選為「InversionModelingInvAutoMesh_paraDX_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**  
      + **1.10.35.1.2 「Left anchoring>Slbling」選為「InversionModelingInvAutoMesh_paraDX_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.10.35.1.3 「Right anchoring>Slbling」選為「InversionModelingInvAutoMesh_paraDX_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
      + **1.10.35.1.4 「Border space」正中間那格填「0」，左邊那格填「8」，右邊那格填「8」。**  
      + **1.10.35.1.5 設定完後關閉Anchors設定視窗。**  
    + **1.10.35.6 設定「Text」為「1」。**
    + **1.10.35.7 設定「Width」為「80」。**
  + **1.10.36 拖拉一個「Standard>TGroupBox」到「InversionModelingParameters_GroupBox」中。預設名稱會是「GroupBox1」，修改「Name」為「InversionModelingDataSettings_GroupBox」。**
      + **1.10.36.1 設定「Anchors」。**  
        + **1.10.36.1.1 「Top anchoring>Slbling」選為「InversionModelingInvAutoMesh_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選下面的。**
        + **1.10.36.1.2 「Left anchoring>Slbling」選為「InversionModelingParameters_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
        + **1.10.36.1.3 「Right anchoring>Slbling」選為「InversionModelingParameters_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
        + **1.10.36.1.4 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
        + **1.10.36.1.5 設定完後關閉Anchors設定視窗。**
      + **1.10.36.2 設定「Caption」為「觀測資料設定:」。**
      + **1.10.36.3 設定「Height」為「70」。** 
  + **1.10.37 拖拉一個「Standard>TGroupBox」到「InversionModelingDataSettings_GroupBox」中。預設名稱會是「GroupBox1」，修改「Name」為「InversionModelingDataSettings_ReCalMode_GroupBox」。**
      + **1.10.37.1 設定「Anchors」。**  
        + **1.10.37.1.1 「Top anchoring>Slbling」選為「InversionModelingInvAutoMesh_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
        + **1.10.37.1.2 「Left anchoring>Slbling」選為「InversionModelingInvAutoMesh_paraDepth_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選右邊的。**  
        + **1.10.37.1.3 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
        + **1.10.37.1.4 設定完後關閉Anchors設定視窗。**
      + **1.10.37.2 設定「Caption」為「觀測資料欄位:」。**
      + **1.10.37.3 設定「Height」為「48」。**
      + **1.10.37.4 設定「Hint」為「從觀測資料提取視電阻率的方式。r=使用r欄位，將重算幾何因子求得視電阻率作為觀測資料；rhoa=使用rhoa欄位，直接將視電阻率當作觀測資料。預設值=r。」。**
      + **1.10.37.5 設定「ShowHint」為「True」。**
      + **1.10.37.6 設定「Width」為「100」。**
  + **1.10.38 拖拉一個「Standard>TComboBox」到「InversionModelingDataSettings_ReCalMode_GroupBox」中。預設名稱會是「ComboBox1」，修改「Name」為「InversionModelingDataSettings_ReCalMode_ComboBox」。**
      + **1.10.38.1 設定「Anchors」。**  
        + **1.10.38.1.1 「Top anchoring>Slbling」選為「InversionModelingDataSettings_ReCalMode_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
        + **1.10.38.1.2 「Left anchoring>Slbling」選為「InversionModelingDataSettings_ReCalMode_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
        + **1.10.38.1.3 「Border space」正中間那格填「0」，左邊那格填「8」，右邊那格填「8」。**  
        + **1.10.38.1.4 設定完後關閉Anchors設定視窗。**
      + **1.10.38.2 設定「Items」為如下內容。**
      ```
      r
      rhoa
      ```
      + **1.10.38.3 設定「ItemIndex」為「0」。**
      + **1.10.38.4 設定「Style」為「csDropDownList」。**
      + **1.10.38.5 設定「Width」為「80」。**
  + **1.10.39 拖拉一個「Standard>TGroupBox」到「InversionModelingDataSettings_GroupBox」中。預設名稱會是「GroupBox1」，修改「Name」為「InversionModelingDataSettings_RelativeError_GroupBox」。**
      + **1.10.39.1 設定「Anchors」。**  
        + **1.10.39.1.1 「Top anchoring>Slbling」選為「InversionModelingDataSettings_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
        + **1.10.39.1.2 「Left anchoring>Slbling」選為「InversionModelingDataSettings_ReCalMode_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選右邊的。**  
        + **1.10.39.1.3 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
        + **1.10.39.1.4 設定完後關閉Anchors設定視窗。**
      + **1.10.39.2 設定「Caption」為「資料相對誤差:」。**
      + **1.10.39.3 設定「Height」為「48」。**
      + **1.10.39.4 設定「Hint」為「資料相對誤差，單位:%。在缺乏資料標準差的先驗知識(例如:反向觀測誤差/交互誤差)時，您可以假設數據中存在一定比例的誤差。這個數值通常落在1到5之間(單位:%)，預設值=3。過高的值會產生過於平滑（模糊）的模型；過低的值會強迫逆推程序去擬合數據中的雜訊，進而產生偽影（假像）。」。**
      + **1.10.39.5 設定「ShowHint」為「True」。**
      + **1.10.39.6 設定「Width」為「100」。**
  + **1.10.40 拖拉一個「Standard>TEdit」到「InversionModelingDataSettings_RelativeError_GroupBox」中。預設名稱會是「Edit1」，修改「Name」為「InversionModelingDataSettings_RelativeError_Edit」**
    + **1.10.40.1 設定「Anchors」。**  
      + **1.10.40.1.1 「Top anchoring>Slbling」選為「InversionModelingInvAutoMesh_addNodes_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**  
      + **1.10.40.1.2 「Left anchoring>Slbling」選為「InversionModelingInvAutoMesh_addNodes_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.10.40.1.3 「Right anchoring>Slbling」選為「InversionModelingInvAutoMesh_addNodes_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
      + **1.10.40.1.4 「Border space」正中間那格填「0」，左邊那格填「8」，右邊那格填「8」。**  
      + **1.10.40.1.5 設定完後關閉Anchors設定視窗。**  
    + **1.10.40.6 設定「Text」為「3」。**
    + **1.10.40.7 設定「Width」為「80」。**
  + **1.10.41 拖拉一個「Standard>TGroupBox」到「InversionModelingDataSettings_GroupBox」中。預設名稱會是「GroupBox1」，修改「Name」為「InversionModelingDataSettings_InvK_GroupBox」。**
      + **1.10.41.1 設定「Anchors」。**  
        + **1.10.41.1.1 「Top anchoring>Slbling」選為「InversionModelingDataSettings_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
        + **1.10.41.1.2 「Left anchoring>Slbling」選為「InversionModelingDataSettings_RelativeError_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選右邊的。**  
        + **1.10.41.1.3 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
        + **1.10.41.1.4 設定完後關閉Anchors設定視窗。**
      + **1.10.41.2 設定「Caption」為「逆推用幾何因子:」。**
      + **1.10.41.3 設定「Height」為「48」。**
      + **1.10.41.4 設定「Hint」為「逆推運算內部採用的幾何因子。K_Mesh=使用網格綁定的數值解；K0_Mesh=使用解析解公式，建議僅有水平地層可以用「K0_Mesh」。預設值=「K_Mesh」。」。**
      + **1.10.41.5 設定「ShowHint」為「True」。**
      + **1.10.41.6 設定「Width」為「110」。**
  + **1.10.42 拖拉一個「Standard>TComboBox」到「InversionModelingDataSettings_InvK_GroupBox」中。預設名稱會是「ComboBox1」，修改「Name」為「InversionModelingDataSettings_InvK_ComboBox」。**
      + **1.10.42.1 設定「Anchors」。**  
        + **1.10.42.1.1 「Top anchoring>Slbling」選為「InversionModelingDataSettings_InvK_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
        + **1.10.42.1.2 「Left anchoring>Slbling」選為「InversionModelingDataSettings_InvK_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
        + **1.10.42.1.3 「Border space」正中間那格填「0」，左邊那格填「8」，右邊那格填「8」。**  
        + **1.10.42.1.4 設定完後關閉Anchors設定視窗。**
      + **1.10.42.2 設定「Items」為如下內容。**
      ```
      K_Mesh
      K0_Mesh
      ```
      + **1.10.42.3 設定「ItemIndex」為「0」。**
      + **1.10.42.4 設定「Style」為「csDropDownList」。**
      + **1.10.42.5 設定「Width」為「90」。**
  + **1.10.43 拖拉一個「Standard>TGroupBox」到「InversionModelingDataSettings_GroupBox」中。預設名稱會是「GroupBox1」，修改「Name」為「InversionModelingDataSettings_ReCalK_GroupBox」。**
      + **1.10.43.1 設定「Anchors」。**  
        + **1.10.43.1.1 「Top anchoring>Slbling」選為「InversionModelingDataSettings_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
        + **1.10.43.1.2 「Left anchoring>Slbling」選為「InversionModelingDataSettings_InvK_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選右邊的。**  
        + **1.10.43.1.3 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
        + **1.10.43.1.4 設定完後關閉Anchors設定視窗。**
      + **1.10.43.2 設定「Caption」為「轉換用幾何因子:」。**
      + **1.10.43.3 設定「Height」為「48」。**
      + **1.10.43.4 設定「Hint」為「從觀測資料「r」欄位提取視電阻率所採用的幾何因子。K_Mesh=使用網格綁定的數值解；K0_Mesh=使用解析解公式；Ideal_Mesh=使用自訂理想網格(要填寫網格參數)計算數值解。預設值=「K_Mesh」。」。**
      + **1.10.43.5 設定「ShowHint」為「True」。**
      + **1.10.43.6 設定「Width」為「110」。**
  + **1.10.44 拖拉一個「Standard>TComboBox」到「InversionModelingDataSettings_ReCalK_GroupBox」中。預設名稱會是「ComboBox1」，修改「Name」為「InversionModelingDataSettings_ReCalK_ComboBox」。**
      + **1.10.44.1 設定「Anchors」。**  
        + **1.10.44.1.1 「Top anchoring>Slbling」選為「InversionModelingDataSettings_ReCalK_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
        + **1.10.44.1.2 「Left anchoring>Slbling」選為「InversionModelingDataSettings_ReCalK_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
        + **1.10.44.1.3 「Border space」正中間那格填「0」，左邊那格填「8」，右邊那格填「8」。**  
        + **1.10.44.1.4 設定完後關閉Anchors設定視窗。**
      + **1.10.44.2 設定「Items」為如下內容。**
      ```
      K_Mesh
      K0_Mesh
      Ideal_Mesh
      ```
      + **1.10.44.3 設定「ItemIndex」為「0」。**
      + **1.10.44.4 設定「Style」為「csDropDownList」。**
      + **1.10.44.5 設定「Width」為「90」。**
  + **1.10.45 拖拉一個「Standard>TGroupBox」到「InversionModelingDataSettings_GroupBox」中。預設名稱會是「GroupBox1」，修改「Name」為「InversionModelingDataSettings_quality_GroupBox」。**
      + **1.10.45.1 設定「Anchors」。**  
        + **1.10.45.1.1 「Top anchoring>Slbling」選為「InversionModelingDataSettings_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
        + **1.10.45.1.2 「Left anchoring>Slbling」選為「InversionModelingDataSettings_ReCalK_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
        + **1.10.45.1.3 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
        + **1.10.45.1.4 設定完後關閉Anchors設定視窗。**
      + **1.10.45.2 設定「Caption」為「理想網格品質:」。**
      + **1.10.45.3 設定「Height」為「48」。**
      + **1.10.45.4 設定「Hint」為「理想網格三角形的最小內角限制角度，單位:度。預設值=32。」。**
      + **1.10.45.5 設定「ShowHint」為「True」。**
      + **1.10.45.6 設定「Width」為「100」。**
  + **1.10.46 拖拉一個「Standard>TComboBox」到「InversionModelingDataSettings_quality_GroupBox」中。預設名稱會是「ComboBox1」，修改「Name」為「InversionModelingDataSettings_quality_ComboBox」。**
      + **1.10.46.1 設定「Anchors」。**  
        + **1.10.46.1.1 「Top anchoring>Slbling」選為「InversionModelingDataSettings_quality_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
        + **1.10.46.1.2 「Left anchoring>Slbling」選為「InversionModelingDataSettings_quality_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
        + **1.10.46.1.3 「Border space」正中間那格填「0」，左邊那格填「8」，右邊那格填「8」。**  
        + **1.10.46.1.4 設定完後關閉Anchors設定視窗。**
      + **1.10.46.2 設定「Enabled」為「False」。**
      + **1.10.46.3 設定「Items」為如下內容。**
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
      + **1.10.46.4 設定「ItemIndex」為「0」。**
      + **1.10.46.5 設定「Style」為「csDropDownList」。**
      + **1.10.46.6 設定「Width」為「80」。** 
  + **1.10.47 拖拉一個「Standard>TGroupBox」到「InversionModelingDataSettings_GroupBox」中。預設名稱會是「GroupBox1」，修改「Name」為「InversionModelingDataSettings_paraDepth_GroupBox」。**
      + **1.10.47.1 設定「Anchors」。**  
        + **1.10.47.1.1 「Top anchoring>Slbling」選為「InversionModelingDataSettings_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
        + **1.10.47.1.2 「Left anchoring>Slbling」選為「InversionModelingDataSettings_quality_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選右邊的。**  
        + **1.10.47.1.3 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
        + **1.10.47.1.4 設定完後關閉Anchors設定視窗。**
      + **1.10.47.2 設定「Caption」為「研究區域深度:」。**
      + **1.10.47.3 設定「Height」為「48」。**
      + **1.10.47.4 設定「Hint」為「預設值:-1，單位:[m]，設定小於0的數字會自動設定為測線電極X方向長度的0.4倍。」。**
      + **1.10.47.5 設定「ShowHint」為「True」。**
      + **1.10.47.6 設定「Width」為「100」。**
  + **1.10.48 拖拉一個「Standard>TEdit」到「InversionModelingDataSettings_paraDepth_GroupBox」中。預設名稱會是「Edit1」，修改「Name」為「InversionModelingDataSettings_paraDepth_Edit」**
    + **1.10.48.1 設定「Anchors」。**  
      + **1.10.48.1.1 「Top anchoring>Slbling」選為「InversionModelingDataSettings_paraDepth_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**  
      + **1.10.48.1.2 「Left anchoring>Slbling」選為「InversionModelingDataSettings_paraDepth_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.10.48.1.3 「Right anchoring>Slbling」選為「InversionModelingDataSettings_paraDepth_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
      + **1.10.48.1.4 「Border space」正中間那格填「0」，左邊那格填「8」，右邊那格填「8」。**  
      + **1.10.48.1.5 設定完後關閉Anchors設定視窗。**  
    + **1.10.48.6 設定「Text」為「-1」。**
    + **1.10.48.7 設定「Width」為「80」。**
  + **1.10.49 拖拉一個「Standard>TGroupBox」到「InversionModelingDataSettings_GroupBox」中。預設名稱會是「GroupBox1」，修改「Name」為「InversionModelingDataSettings_boundary_GroupBox」。**
      + **1.10.49.1 設定「Anchors」。**  
        + **1.10.49.1.1 「Top anchoring>Slbling」選為「InversionModelingDataSettings_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
        + **1.10.49.1.2 「Left anchoring>Slbling」選為「InversionModelingDataSettings_paraDepth_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選右邊的。**  
        + **1.10.49.1.3 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
        + **1.10.49.1.4 設定完後關閉Anchors設定視窗。**
      + **1.10.49.2 設定「Caption」為「外邊界倍數:」。**
      + **1.10.49.3 設定「Height」為「48」。**
      + **1.10.49.4 設定「Hint」為「依照此倍數追加左側、右側、下方外網格。追加長度=測線電極X方向長度乘上外邊界倍數。預設值=4。」。**
      + **1.10.49.5 設定「ShowHint」為「True」。**
      + **1.10.49.6 設定「Width」為「100」。**
  + **1.10.50 拖拉一個「Standard>TComboBox」到「InversionModelingDataSettings_boundary_GroupBox」中。預設名稱會是「ComboBox1」，修改「Name」為「InversionModelingDataSettings_boundary_ComboBox」。**
      + **1.10.50.1 設定「Anchors」。**  
        + **1.10.50.1.1 「Top anchoring>Slbling」選為「InversionModelingDataSettings_boundary_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
        + **1.10.50.1.2 「Left anchoring>Slbling」選為「InversionModelingDataSettings_boundary_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
        + **1.10.50.1.3 「Border space」正中間那格填「0」，左邊那格填「8」，右邊那格填「8」。**  
        + **1.10.50.1.4 設定完後關閉Anchors設定視窗。**
      + **1.10.50.2 設定「Enabled」為「False」。**
      + **1.10.50.3 設定「Items」為如下內容。**
      ```
      4
      5
      6
      7
      8
      9
      10
      ```
      + **1.10.50.4 設定「ItemIndex」為「0」。**
      + **1.10.50.5 設定「Style」為「csDropDownList」。**
      + **1.10.50.6 設定「Width」為「80」。**
  + **1.10.51 拖拉一個「Standard>TGroupBox」到「InversionModelingDataSettings_GroupBox」中。預設名稱會是「GroupBox1」，修改「Name」為「InversionModelingDataSettings_paraMaxCellSize_GroupBox」。**
      + **1.10.51.1 設定「Anchors」。**  
        + **1.10.51.1.1 「Top anchoring>Slbling」選為「InversionModelingDataSettings_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
        + **1.10.51.1.2 「Left anchoring>Slbling」選為「InversionModelingDataSettings_boundary_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選右邊的。**  
        + **1.10.51.1.3 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
        + **1.10.51.1.4 設定完後關閉Anchors設定視窗。**
      + **1.10.51.2 設定「Caption」為「網格面積上限:」。**
      + **1.10.51.3 設定「Height」為「48」。**
      + **1.10.51.4 設定「Hint」為「預設值=0，單位:[m^2]，設定0表示不限制單一網格面積上限。」。**
      + **1.10.51.5 設定「ShowHint」為「True」。**
      + **1.10.51.6 設定「Width」為「100」。**
  + **1.10.52 拖拉一個「Standard>TEdit」到「InversionModelingDataSettings_paraMaxCellSize_GroupBox」中。預設名稱會是「Edit1」，修改「Name」為「InversionModelingDataSettings_paraMaxCellSize_Edit」**
    + **1.10.52.1 設定「Anchors」。**  
      + **1.10.52.1.1 「Top anchoring>Slbling」選為「InversionModelingDataSettings_paraMaxCellSize_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**  
      + **1.10.52.1.2 「Left anchoring>Slbling」選為「InversionModelingDataSettings_paraMaxCellSize_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.10.52.1.3 「Right anchoring>Slbling」選為「InversionModelingDataSettings_paraMaxCellSize_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
      + **1.10.52.1.4 「Border space」正中間那格填「0」，左邊那格填「8」，右邊那格填「8」。**  
      + **1.10.52.1.5 設定完後關閉Anchors設定視窗。**  
    + **1.10.52.6 設定「Text」為「0」。**
    + **1.10.52.7 設定「Width」為「80」。**
  + **1.10.53 拖拉一個「Standard>TGroupBox」到「InversionModelingDataSettings_GroupBox」中。預設名稱會是「GroupBox1」，修改「Name」為「InversionModelingDataSettings_addNodes_GroupBox」。**
      + **1.10.53.1 設定「Anchors」。**  
        + **1.10.53.1.1 「Top anchoring>Slbling」選為「InversionModelingDataSettings_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
        + **1.10.53.1.2 「Left anchoring>Slbling」選為「InversionModelingDataSettings_paraMaxCellSize_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選右邊的。**  
        + **1.10.53.1.3 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
        + **1.10.53.1.4 設定完後關閉Anchors設定視窗。**
      + **1.10.53.2 設定「Caption」為「addNodes:」。**
      + **1.10.53.3 設定「Height」為「48」。**
      + **1.10.53.4 設定「Hint」為「限制填入正整數。此值填「1」時無效果，改由paraDX決定增加的方式；填其他正整數則線性均勻增加兩個電極之間的節點數量。預設值=1。」。**
      + **1.10.53.5 設定「ShowHint」為「True」。**
      + **1.10.53.6 設定「Width」為「100」。**
  + **1.10.54 拖拉一個「Standard>TEdit」到「InversionModelingDataSettings_addNodes_GroupBox」中。預設名稱會是「Edit1」，修改「Name」為「InversionModelingDataSettings_addNodes_Edit」**
    + **1.10.54.1 設定「Anchors」。**  
      + **1.10.54.1.1 「Top anchoring>Slbling」選為「InversionModelingDataSettings_addNodes_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**  
      + **1.10.54.1.2 「Left anchoring>Slbling」選為「InversionModelingDataSettings_addNodes_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.10.54.1.3 「Right anchoring>Slbling」選為「InversionModelingDataSettings_addNodes_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
      + **1.10.54.1.4 「Border space」正中間那格填「0」，左邊那格填「8」，右邊那格填「8」。**  
      + **1.10.54.1.5 設定完後關閉Anchors設定視窗。**  
    + **1.10.54.6 設定「Text」為「1」。**
    + **1.10.54.7 設定「Width」為「80」。**
  + **1.10.55 拖拉一個「Standard>TGroupBox」到「InversionModelingDataSettings_GroupBox」中。預設名稱會是「GroupBox1」，修改「Name」為「InversionModelingDataSettings_paraDX_GroupBox」。**
      + **1.10.55.1 設定「Anchors」。**  
        + **1.10.55.1.1 「Top anchoring>Slbling」選為「InversionModelingDataSettings_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
        + **1.10.55.1.2 「Left anchoring>Slbling」選為「InversionModelingDataSettings_addNodes_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選右邊的。**  
        + **1.10.55.1.3 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
        + **1.10.55.1.4 設定完後關閉Anchors設定視窗。**
      + **1.10.55.2 設定「Caption」為「paraDX:」。**
      + **1.10.55.3 設定「Height」為「48」。**
      + **1.10.55.4 設定「Hint」為「限制填入浮點數。僅有addNodes小於等於1的時候有效。此值大於等於0.5時，等於在兩個電極正中間增加一個節點。小於0.5的時候，在每個電極的左右兩邊各增加一個節點，距離是電極間距乘以此浮點數。預設值=1。」。**
      + **1.10.55.5 設定「ShowHint」為「True」。**
      + **1.10.55.6 設定「Width」為「100」。**
  + **1.10.56 拖拉一個「Standard>TEdit」到「InversionModelingDataSettings_paraDX_GroupBox」中。預設名稱會是「Edit1」，修改「Name」為「InversionModelingDataSettings_paraDX_Edit」**
    + **1.10.56.1 設定「Anchors」。**  
      + **1.10.56.1.1 「Top anchoring>Slbling」選為「InversionModelingDataSettings_paraDX_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**  
      + **1.10.56.1.2 「Left anchoring>Slbling」選為「InversionModelingDataSettings_paraDX_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.10.56.1.3 「Right anchoring>Slbling」選為「InversionModelingDataSettings_paraDX_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
      + **1.10.56.1.4 「Border space」正中間那格填「0」，左邊那格填「8」，右邊那格填「8」。**  
      + **1.10.56.1.5 設定完後關閉Anchors設定視窗。**  
    + **1.10.56.6 設定「Text」為「1」。**
    + **1.10.56.7 設定「Width」為「80」。**
  + **1.10.57 拖拉一個「Standard>TGroupBox」到「InversionModelingParameters_GroupBox」中。預設名稱會是「GroupBox1」，修改「Name」為「InversionModelingDataPrepare_GroupBox」。**
      + **1.10.57.1 設定「Anchors」。**  
        + **1.10.57.1.1 「Top anchoring>Slbling」選為「InversionModelingDataSettings_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選下面的。**
        + **1.10.57.1.2 「Left anchoring>Slbling」選為「InversionModelingParameters_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
        + **1.10.57.1.3 「Right anchoring>Slbling」選為「InversionModelingParameters_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
        + **1.10.57.1.4 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
        + **1.10.57.1.5 設定完後關閉Anchors設定視窗。**
      + **1.10.57.2 設定「Caption」為「觀測資料調整:」。**
      + **1.10.57.3 設定「Height」為「70」。** 
  + **1.10.58 拖拉一個「Standard>TCheckBox」到「InversionModelingDataPrepare_GroupBox」中。預設名稱會是「CheckBox1」，修改「Name」為「InversionModelingDataPrepare_UseFakeDataEnable_CheckBox」。**
      + **1.10.58.1 設定「Anchors」。**  
        + **1.10.58.1.1 「Top anchoring>Slbling」選為「InversionModelingInvAutoMesh_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
        + **1.10.58.1.2 「Left anchoring>Slbling」選為「InversionModelingInvAutoMesh_paraDepth_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選右邊的。**  
        + **1.10.58.1.3 「Border space」正中間那格填「1」，左邊那格填「3」，右邊那格填「3」。**  
        + **1.10.58.1.4 設定完後關閉Anchors設定視窗。**
      + **1.10.58.2 設定「Caption」為「所有資料以相同視電阻率取代，單位=[Ohm-m]。改寫為:」。**
  + **1.10.59 拖拉一個「Standard>TEdit」到「InversionModelingDataPrepare_GroupBox」中。預設名稱會是「Edit1」，修改「Name」為「InversionModelingDataPrepare_UseFakeDataRhoa_Edit」**
    + **1.10.59.1 設定「Anchors」。**  
      + **1.10.59.1.1 「Top anchoring>Slbling」選為「InversionModelingDataPrepare_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**  
      + **1.10.59.1.2 「Left anchoring>Slbling」選為「InversionModelingDataPrepare_UseFakeDataEnable_CheckBox:TCheckBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.10.59.1.3 「Border space」正中間那格填「0」，左邊那格填「0」，右邊那格填「0」。**  
      + **1.10.59.1.4 設定完後關閉Anchors設定視窗。**  
    + **1.10.59.2 設定「Text」為「100」。**
    + **1.10.59.3 設定「Width」為「80」。**
  + **1.10.60 拖拉一個「Standard>TGroupBox」到「InversionModelingDataPrepare_GroupBox」中。預設名稱會是「GroupBox1」，修改「Name」為「InversionModelingDataPrepare_RemoveCurrentLowerThan_GroupBox」。**
      + **1.10.60.1 設定「Anchors」。**  
        + **1.10.60.1.1 「Top anchoring>Slbling」選為「InversionModelingDataPrepare_UseFakeDataEnable_CheckBox:TCheckBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選下面的。**
        + **1.10.60.1.2 「Left anchoring>Slbling」選為「InversionModelingDataPrepare_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
        + **1.10.60.1.3 「Border space」正中間那格填「0」，上面那格填「8」，左邊那格填「3」，右邊那格填「3」。**  
        + **1.10.60.1.4 設定完後關閉Anchors設定視窗。**
      + **1.10.60.2 設定「Caption」為「移除電流[A]低於:」。**
      + **1.10.60.3 設定「Height」為「48」。**
      + **1.10.60.4 設定「Hint」為「觀測資料的電流低於指定數值時將移除且不參與後續逆推運算，電流單位:[A]。限制填大於等於0的數字，填「0」表示不移除。注意，全部刪除資料會造成後續異常錯誤。請考量正確的儀器電流情形。預設值=0.01。」。**
      + **1.10.60.5 設定「ShowHint」為「True」。**
      + **1.10.60.6 設定「Width」為「110」。**
  + **1.10.61 拖拉一個「Standard>TEdit」到「InversionModelingDataPrepare_RemoveCurrentLowerThan_GroupBox」中。預設名稱會是「Edit1」，修改「Name」為「InversionModelingDataPrepare_RemoveCurrentLowerThan_Edit」**
    + **1.10.61.1 設定「Anchors」。**  
      + **1.10.61.1.1 「Top anchoring>Slbling」選為「InversionModelingDataSettings_paraDepth_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**  
      + **1.10.61.1.2 「Left anchoring>Slbling」選為「InversionModelingDataSettings_paraDepth_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.10.61.1.3 「Right anchoring>Slbling」選為「InversionModelingDataSettings_paraDepth_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
      + **1.10.61.1.4 「Border space」正中間那格填「0」，左邊那格填「8」，右邊那格填「8」。**  
      + **1.10.61.1.5 設定完後關閉Anchors設定視窗。**  
    + **1.10.61.6 設定「Text」為「0.01」。**
  + **1.10.62 拖拉一個「Standard>TGroupBox」到「InversionModelingDataPrepare_GroupBox」中。預設名稱會是「GroupBox1」，修改「Name」為「InversionModelingDataPrepare_RemoveCurrentHigherThan_GroupBox」。**
      + **1.10.62.1 設定「Anchors」。**  
        + **1.10.62.1.1 「Top anchoring>Slbling」選為「InversionModelingDataPrepare_RemoveCurrentLowerThan_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
        + **1.10.62.1.2 「Left anchoring>Slbling」選為「InversionModelingDataPrepare_RemoveCurrentLowerThan_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選右邊的。**  
        + **1.10.62.1.3 「Border space」正中間那格填「0」，上面那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
        + **1.10.62.1.4 設定完後關閉Anchors設定視窗。**
      + **1.10.62.2 設定「Caption」為「移除電流[A]高於:」。**
      + **1.10.62.3 設定「Height」為「48」。**
      + **1.10.62.4 設定「Hint」為「觀測資料的電流高於指定數值時將移除且不參與後續逆推運算，電流單位:[A]。限制填大於等於0的數字。注意，全部刪除資料會造成後續異常錯誤。請考量正確的儀器電流情形。預設值=99999。」。**
      + **1.10.62.5 設定「ShowHint」為「True」。**
      + **1.10.62.6 設定「Width」為「110」。**
  + **1.10.63 拖拉一個「Standard>TEdit」到「InversionModelingDataPrepare_RemoveCurrentHigherThan_GroupBox」中。預設名稱會是「Edit1」，修改「Name」為「InversionModelingDataPrepare_RemoveCurrentHigherThan_Edit」**
    + **1.10.63.1 設定「Anchors」。**  
      + **1.10.63.1.1 「Top anchoring>Slbling」選為「InversionModelingDataPrepare_RemoveCurrentHigherThan_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**  
      + **1.10.63.1.2 「Left anchoring>Slbling」選為「InversionModelingDataPrepare_RemoveCurrentHigherThan_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.10.63.1.3 「Right anchoring>Slbling」選為「InversionModelingDataPrepare_RemoveCurrentHigherThan_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
      + **1.10.63.1.4 「Border space」正中間那格填「0」，左邊那格填「8」，右邊那格填「8」。**  
      + **1.10.63.1.5 設定完後關閉Anchors設定視窗。**  
    + **1.10.63.6 設定「Text」為「99999」。**
  + **1.10.64 拖拉一個「Standard>TGroupBox」到「InversionModelingDataPrepare_GroupBox」中。預設名稱會是「GroupBox1」，修改「Name」為「InversionModelingDataPrepare_RemoveVoltageLowerThan_GroupBox」。**
      + **1.10.64.1 設定「Anchors」。**  
        + **1.10.64.1.1 「Top anchoring>Slbling」選為「InversionModelingDataPrepare_RemoveCurrentHigherThan_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
        + **1.10.64.1.2 「Left anchoring>Slbling」選為「InversionModelingDataPrepare_RemoveCurrentHigherThan_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選右邊的。**  
        + **1.10.64.1.3 「Border space」正中間那格填「0」，上面那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
        + **1.10.64.1.4 設定完後關閉Anchors設定視窗。**
      + **1.10.64.2 設定「Caption」為「移除電壓[V]低於:」。**
      + **1.10.64.3 設定「Height」為「48」。**
      + **1.10.64.4 設定「Hint」為「觀測資料的電壓低於指定數值時將移除且不參與後續逆推運算，電壓單位:[V]。限制填大於等於0的數字，填「0」表示不移除。注意，全部刪除資料會造成後續異常錯誤。請考量正確的儀器電壓情形。預設值=0.01。」。**
      + **1.10.64.5 設定「ShowHint」為「True」。**
      + **1.10.64.6 設定「Width」為「110」。**
  + **1.10.65 拖拉一個「Standard>TEdit」到「InversionModelingDataPrepare_RemoveVoltageLowerThan_GroupBox」中。預設名稱會是「Edit1」，修改「Name」為「InversionModelingDataPrepare_RemoveVoltageLowerThan_Edit」**
    + **1.10.65.1 設定「Anchors」。**  
      + **1.10.65.1.1 「Top anchoring>Slbling」選為「InversionModelingDataPrepare_RemoveVoltageLowerThan_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**  
      + **1.10.65.1.2 「Left anchoring>Slbling」選為「InversionModelingDataPrepare_RemoveVoltageLowerThan_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.10.65.1.3 「Right anchoring>Slbling」選為「InversionModelingDataPrepare_RemoveVoltageLowerThan_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
      + **1.10.65.1.4 「Border space」正中間那格填「0」，左邊那格填「8」，右邊那格填「8」。**  
      + **1.10.65.1.5 設定完後關閉Anchors設定視窗。**  
    + **1.10.65.6 設定「Text」為「0.01」。**
  + **1.10.66 拖拉一個「Standard>TGroupBox」到「InversionModelingDataPrepare_GroupBox」中。預設名稱會是「GroupBox1」，修改「Name」為「InversionModelingDataPrepare_RemoveVoltageHigherThan_GroupBox」。**
      + **1.10.66.1 設定「Anchors」。**  
        + **1.10.66.1.1 「Top anchoring>Slbling」選為「InversionModelingDataPrepare_RemoveVoltageLowerThan_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
        + **1.10.66.1.2 「Left anchoring>Slbling」選為「InversionModelingDataPrepare_RemoveVoltageLowerThan_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選右邊的。**  
        + **1.10.66.1.3 「Border space」正中間那格填「0」，上面那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
        + **1.10.66.1.4 設定完後關閉Anchors設定視窗。**
      + **1.10.66.2 設定「Caption」為「移除電壓[V]高於:」。**
      + **1.10.66.3 設定「Height」為「48」。**
      + **1.10.66.4 設定「Hint」為「觀測資料的電壓高於指定數值時將移除且不參與後續逆推運算，電壓單位:[V]。限制填大於等於0的數字。注意，全部刪除資料會造成後續異常錯誤。請考量正確的儀器電壓情形。預設值=99999。」。**
      + **1.10.66.5 設定「ShowHint」為「True」。**
      + **1.10.66.6 設定「Width」為「110」。**
  + **1.10.67 拖拉一個「Standard>TEdit」到「InversionModelingDataPrepare_RemoveVoltageHigherThan_GroupBox」中。預設名稱會是「Edit1」，修改「Name」為「InversionModelingDataPrepare_RemoveVoltageHigherThan_Edit」**
    + **1.10.67.1 設定「Anchors」。**  
      + **1.10.67.1.1 「Top anchoring>Slbling」選為「InversionModelingDataPrepare_RemoveVoltageLowerThan_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**  
      + **1.10.67.1.2 「Left anchoring>Slbling」選為「InversionModelingDataPrepare_RemoveVoltageLowerThan_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.10.67.1.3 「Right anchoring>Slbling」選為「InversionModelingDataPrepare_RemoveVoltageLowerThan_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
      + **1.10.67.1.4 「Border space」正中間那格填「0」，左邊那格填「8」，右邊那格填「8」。**  
      + **1.10.67.1.5 設定完後關閉Anchors設定視窗。**  
    + **1.10.67.6 設定「Text」為「99999」。**
  + **1.10.68 拖拉一個「Standard>TGroupBox」到「InversionModelingDataPrepare_GroupBox」中。預設名稱會是「GroupBox1」，修改「Name」為「InversionModelingDataPrepare_RemoveAppResLowerThan_GroupBox」。**
      + **1.10.68.1 設定「Anchors」。**  
        + **1.10.68.1.1 「Top anchoring>Slbling」選為「InversionModelingDataPrepare_RemoveVoltageHigherThan_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
        + **1.10.68.1.2 「Left anchoring>Slbling」選為「InversionModelingDataPrepare_RemoveVoltageHigherThan_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選右邊的。**  
        + **1.10.68.1.3 「Border space」正中間那格填「0」，上面那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
        + **1.10.68.1.4 設定完後關閉Anchors設定視窗。**
      + **1.10.68.2 設定「Caption」為「移除Rhoa[Ohm-m]低於:」。**
      + **1.10.68.3 設定「Height」為「48」。**
      + **1.10.68.4 設定「Hint」為「觀測資料的視電阻率(Rhoa)低於指定數值時將移除且不參與後續逆推運算，Rhoa單位:[Ohm-m]。限制填大於等於0的數字，填「0」表示不移除。注意，全部刪除資料會造成後續異常錯誤。預設值=0。」。**
      + **1.10.68.5 設定「ShowHint」為「True」。**
      + **1.10.68.6 設定「Width」為「150」。**
  + **1.10.69 拖拉一個「Standard>TEdit」到「InversionModelingDataPrepare_RemoveAppResLowerThan_GroupBox」中。預設名稱會是「Edit1」，修改「Name」為「InversionModelingDataPrepare_RemoveAppResLowerThan_Edit」**
    + **1.10.69.1 設定「Anchors」。**  
      + **1.10.69.1.1 「Top anchoring>Slbling」選為「InversionModelingDataPrepare_RemoveAppResLowerThan_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**  
      + **1.10.69.1.2 「Left anchoring>Slbling」選為「InversionModelingDataPrepare_RemoveAppResLowerThan_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.10.69.1.3 「Right anchoring>Slbling」選為「InversionModelingDataPrepare_RemoveAppResLowerThan_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
      + **1.10.69.1.4 「Border space」正中間那格填「0」，左邊那格填「8」，右邊那格填「8」。**  
      + **1.10.69.1.5 設定完後關閉Anchors設定視窗。**  
    + **1.10.69.6 設定「Text」為「0」。**
  + **1.10.70 拖拉一個「Standard>TGroupBox」到「InversionModelingDataPrepare_GroupBox」中。預設名稱會是「GroupBox1」，修改「Name」為「InversionModelingDataPrepare_RemoveAppResHigherThan_GroupBox」。**
      + **1.10.70.1 設定「Anchors」。**  
        + **1.10.70.1.1 「Top anchoring>Slbling」選為「InversionModelingDataPrepare_RemoveAppResLowerThan_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
        + **1.10.70.1.2 「Left anchoring>Slbling」選為「InversionModelingDataPrepare_RemoveAppResLowerThan_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選右邊的。**  
        + **1.10.70.1.3 「Border space」正中間那格填「0」，上面那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
        + **1.10.70.1.4 設定完後關閉Anchors設定視窗。**
      + **1.10.70.2 設定「Caption」為「移除Rhoa[Ohm-m]高於:」。**
      + **1.10.70.3 設定「Height」為「48」。**
      + **1.10.70.4 設定「Hint」為「觀測資料的視電阻率(Rhoa)高於指定數值時將移除且不參與後續逆推運算，Rhoa單位:[Ohm-m]。限制填大於等於0的數字。注意，全部刪除資料會造成後續異常錯誤。預設值=999999。」。**
      + **1.10.70.5 設定「ShowHint」為「True」。**
      + **1.10.70.6 設定「Width」為「150」。**
  + **1.10.71 拖拉一個「Standard>TEdit」到「InversionModelingDataPrepare_RemoveAppResHigherThan_GroupBox」中。預設名稱會是「Edit1」，修改「Name」為「InversionModelingDataPrepare_RemoveAppResHigherThan_Edit」**
    + **1.10.71.1 設定「Anchors」。**  
      + **1.10.71.1.1 「Top anchoring>Slbling」選為「InversionModelingDataPrepare_RemoveAppResHigherThan_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**  
      + **1.10.71.1.2 「Left anchoring>Slbling」選為「InversionModelingDataPrepare_RemoveAppResHigherThan_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.10.71.1.3 「Right anchoring>Slbling」選為「InversionModelingDataPrepare_RemoveAppResHigherThan_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
      + **1.10.71.1.4 「Border space」正中間那格填「0」，左邊那格填「8」，右邊那格填「8」。**  
      + **1.10.71.1.5 設定完後關閉Anchors設定視窗。**  
    + **1.10.71.6 設定「Text」為「999999」。**
  + **1.10.72 拖拉一個「Standard>TCheckBox」到「InversionModelingDataPrepare_GroupBox」中。預設名稱會是「CheckBox1」，修改「Name」為「InversionModelingDataPrepare_RemoveBadElectrode_CheckBox」。**
      + **1.10.72.1 設定「Anchors」。**  
        + **1.10.72.1.1 「Top anchoring>Slbling」選為「InversionModelingDataPrepare_UseFakeDataEnable_CheckBox:TCheckBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
        + **1.10.72.1.2 「Left anchoring>Slbling」選為「InversionModelingDataPrepare_UseFakeDataRhoa_Edit:TEdit」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選右邊的。**  
        + **1.10.72.1.3 「Border space」正中間那格填「0」，左邊那格填「16」，右邊那格填「3」。**  
        + **1.10.72.1.4 設定完後關閉Anchors設定視窗。**
      + **1.10.72.2 設定「Caption」為「移除指定電極有關資料。移除電極索引:」。**
  + **1.10.73 拖拉一個「Standard>TEdit」到「InversionModelingDataPrepare_GroupBox」中。預設名稱會是「Edit1」，修改「Name」為「InversionModelingDataPrepare_RemoveBadElectrode_Edit」**
    + **1.10.73.1 設定「Anchors」。**  
      + **1.10.73.1.1 「Top anchoring>Slbling」選為「InversionModelingDataPrepare_UseFakeDataRhoa_Edit:TEdit」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**  
      + **1.10.73.1.2 「Left anchoring>Slbling」選為「InversionModelingDataPrepare_RemoveBadElectrode_CheckBox:TCheckBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選右邊的。**
      + **1.10.73.1.3 「Right anchoring>Slbling」選為「InversionModelingDataPrepare_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
      + **1.10.73.1.4 「Border space」正中間那格填「0」，左邊那格填「0」，右邊那格填「3」。**  
      + **1.10.73.1.5 設定完後關閉Anchors設定視窗。**  
    + **1.10.73.2 設定「Text」為「1,2,3,25,28」。**
  + **1.10.74 拖拉一個「Standard>TGroupBox」到「InversionModelingParameters_GroupBox」中。預設名稱會是「GroupBox1」，修改「Name」為「InversionModelingInvSettings_GroupBox」。**
      + **1.10.74.1 設定「Anchors」。**  
        + **1.10.74.1.1 「Top anchoring>Slbling」選為「InversionModelingDataPrepare_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選下面的。**
        + **1.10.74.1.2 「Left anchoring>Slbling」選為「InversionModelingParameters_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
        + **1.10.74.1.3 「Right anchoring>Slbling」選為「InversionModelingParameters_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
        + **1.10.74.1.4 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
        + **1.10.74.1.5 設定完後關閉Anchors設定視窗。**
      + **1.10.74.2 設定「Caption」為「逆推參數設定:」。**
      + **1.10.74.3 設定「Height」為「70」。** 
  + **1.10.75 拖拉一個「Standard>TGroupBox」到「InversionModelingInvSettings_GroupBox」中。預設名稱會是「GroupBox1」，修改「Name」為「InversionModelingInvSettings_Lambda_GroupBox」。**
      + **1.10.75.1 設定「Anchors」。**  
        + **1.10.75.1.1 「Top anchoring>Slbling」選為「InversionModelingInvSettings_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
        + **1.10.75.1.2 「Left anchoring>Slbling」選為「InversionModelingInvSettings_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
        + **1.10.75.1.3 「Border space」正中間那格填「0」，上面那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
        + **1.10.75.1.4 設定完後關閉Anchors設定視窗。**
      + **1.10.75.2 設定「Caption」為「Lambda:」。**
      + **1.10.75.3 設定「Height」為「48」。**
      + **1.10.75.4 設定「Hint」為「逆推參數中的Lambda為「正規化參數(Regularization Parameter)」，或通俗地稱為「平滑係數(Smoothing Factor)」。其值越大表示目標函數中資料項影響降低、模型項影響增加，通常導致模型更平滑但資料擬合較差。反之，其值越小，模型主要受資料項影響，通常會產生更粗糙(更不平滑)的模型。預設值=[20]。請填入陣列，限制陣列中元素僅可填入正數，例如:「[100,33.33,1]」。依序選擇陣列中的Lambda進行逆推，當該Lamdba的逆推滿足終止條件時，保留該模型作為起始模型接關使用下一個Lambda繼續逆推，直到所有Lambda都都執行完畢並獲得最終模型為止。」。**
      + **1.10.75.5 設定「ShowHint」為「True」。**
      + **1.10.75.6 設定「Width」為「150」。**
  + **1.10.76 拖拉一個「Standard>TEdit」到「InversionModelingInvSettings_Lambda_GroupBox」中。預設名稱會是「Edit1」，修改「Name」為「InversionModelingInvSettings_Lambda_Edit」**
    + **1.10.76.1 設定「Anchors」。**  
      + **1.10.76.1.1 「Top anchoring>Slbling」選為「InversionModelingInvSettings_Lambda_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**  
      + **1.10.76.1.2 「Left anchoring>Slbling」選為「InversionModelingInvSettings_Lambda_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.10.76.1.3 「Right anchoring>Slbling」選為「InversionModelingInvSettings_Lambda_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
      + **1.10.76.1.4 「Border space」正中間那格填「0」，左邊那格填「8」，右邊那格填「8」。**  
      + **1.10.76.1.5 設定完後關閉Anchors設定視窗。**  
    + **1.10.76.6 設定「Text」為「[20]」。**
  + **1.10.77 拖拉一個「Standard>TGroupBox」到「InversionModelingInvSettings_GroupBox」中。預設名稱會是「GroupBox1」，修改「Name」為「InversionModelingInvSettings_maxIter_GroupBox」。**
      + **1.10.77.1 設定「Anchors」。**  
        + **1.10.77.1.1 「Top anchoring>Slbling」選為「InversionModelingInvSettings_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
        + **1.10.77.1.2 「Left anchoring>Slbling」選為「InversionModelingInvSettings_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選右邊的。**  
        + **1.10.77.1.3 「Border space」正中間那格填「0」，上面那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
        + **1.10.77.1.4 設定完後關閉Anchors設定視窗。**
      + **1.10.77.2 設定「Caption」為「最大迭代次數:」。**
      + **1.10.77.3 設定「Height」為「48」。**
      + **1.10.77.4 設定「Hint」為「預防逆推程式進入無窮迴圈的迭代上限值，一般而言，逆推在20次迭代內就會滿足停止條件。預設值=20。」。**
      + **1.10.77.5 設定「ShowHint」為「True」。**
      + **1.10.77.6 設定「Width」為「100」。**
  + **1.10.78 拖拉一個「Standard>TEdit」到「InversionModelingInvSettings_maxIter_GroupBox」中。預設名稱會是「Edit1」，修改「Name」為「InversionModelingInvSettings_maxIter_Edit」**
    + **1.10.78.1 設定「Anchors」。**  
      + **1.10.78.1.1 「Top anchoring>Slbling」選為「InversionModelingInvSettings_maxIter_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**  
      + **1.10.78.1.2 「Left anchoring>Slbling」選為「InversionModelingInvSettings_maxIter_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.10.78.1.3 「Right anchoring>Slbling」選為「InversionModelingInvSettings_maxIter_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
      + **1.10.78.1.4 「Border space」正中間那格填「0」，左邊那格填「8」，右邊那格填「8」。**  
      + **1.10.78.1.5 設定完後關閉Anchors設定視窗。**  
    + **1.10.78.6 設定「Text」為「20」。**
  + **1.10.79 拖拉一個「Standard>TGroupBox」到「InversionModelingInvSettings_GroupBox」中。預設名稱會是「GroupBox1」，修改「Name」為「InversionModelingInvSettings_limitModelCell_GroupBox」。**
      + **1.10.79.1 設定「Anchors」。**  
        + **1.10.79.1.1 「Top anchoring>Slbling」選為「InversionModelingInvSettings_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
        + **1.10.79.1.2 「Left anchoring>Slbling」選為「InversionModelingInvSettings_maxIter_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選右邊的。**  
        + **1.10.79.1.3 「Border space」正中間那格填「0」，上面那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
        + **1.10.79.1.4 設定完後關閉Anchors設定視窗。**
      + **1.10.79.2 設定「Caption」為「模型電阻率範圍[Ohm-m]:」。**
      + **1.10.79.3 設定「Height」為「48」。**
      + **1.10.79.4 設定「Hint」為「限制逆推過程中，不允許模型網格電阻率被調整超過此範圍，單位:[Ohm-m]。預設值:「[0,0]」，限制填入只有兩個元素的陣列，第一個元素為最小值，第二個元素為最大值，限制填正數或「0」。填「0」表示不限制。」。**
      + **1.10.79.5 設定「ShowHint」為「True」。**
      + **1.10.79.6 設定「Width」為「200」。**
  + **1.10.80 拖拉一個「Standard>TEdit」到「InversionModelingInvSettings_limitModelCell_GroupBox」中。預設名稱會是「Edit1」，修改「Name」為「InversionModelingInvSettings_limitModelCell_Edit」**
    + **1.10.80.1 設定「Anchors」。**  
      + **1.10.80.1.1 「Top anchoring>Slbling」選為「InversionModelingInvSettings_limitModelCell_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**  
      + **1.10.80.1.2 「Left anchoring>Slbling」選為「InversionModelingInvSettings_limitModelCell_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.10.80.1.3 「Right anchoring>Slbling」選為「InversionModelingInvSettings_limitModelCell_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
      + **1.10.80.1.4 「Border space」正中間那格填「0」，左邊那格填「8」，右邊那格填「8」。**  
      + **1.10.80.1.5 設定完後關閉Anchors設定視窗。**  
    + **1.10.80.6 設定「Text」為「[0,0]」。**
  + **1.10.81 拖拉一個「Standard>TGroupBox」到「InversionModelingInvSettings_GroupBox」中。預設名稱會是「GroupBox1」，修改「Name」為「InversionModelingInvSettings_RemoveTimes_GroupBox」。**
      + **1.10.81.1 設定「Anchors」。**  
        + **1.10.81.1.1 「Top anchoring>Slbling」選為「InversionModelingInvSettings_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
        + **1.10.81.1.2 「Left anchoring>Slbling」選為「InversionModelingInvSettings_limitModelCell_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選右邊的。**  
        + **1.10.81.1.3 「Border space」正中間那格填「0」，上面那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
        + **1.10.81.1.4 設定完後關閉Anchors設定視窗。**
      + **1.10.81.2 設定「Caption」為「資料剔除次數:」。**
      + **1.10.81.3 設定「Height」為「48」。**
      + **1.10.81.4 設定「Hint」為「基於最終模型下的擬合誤差進行資料剔除的次數，單位:次。預設值=2。若不刪除任何資料，填「0」。」。**
      + **1.10.81.5 設定「ShowHint」為「True」。**
      + **1.10.81.6 設定「Width」為「100」。**
  + **1.10.82 拖拉一個「Standard>TEdit」到「InversionModelingInvSettings_RemoveTimes_GroupBox」中。預設名稱會是「Edit1」，修改「Name」為「InversionModelingInvSettings_RemoveTimes_Edit」**
    + **1.10.82.1 設定「Anchors」。**  
      + **1.10.82.1.1 「Top anchoring>Slbling」選為「InversionModelingInvSettings_RemoveTimes_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**  
      + **1.10.82.1.2 「Left anchoring>Slbling」選為「InversionModelingInvSettings_RemoveTimes_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.10.82.1.3 「Right anchoring>Slbling」選為「InversionModelingInvSettings_RemoveTimes_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
      + **1.10.82.1.4 「Border space」正中間那格填「0」，左邊那格填「8」，右邊那格填「8」。**  
      + **1.10.82.1.5 設定完後關閉Anchors設定視窗。**  
    + **1.10.82.6 設定「Text」為「2」。**
  + **1.10.83 拖拉一個「Standard>TGroupBox」到「InversionModelingInvSettings_GroupBox」中。預設名稱會是「GroupBox1」，修改「Name」為「InversionModelingInvSettings_RemovePercentage_GroupBox」。**
      + **1.10.83.1 設定「Anchors」。**  
        + **1.10.83.1.1 「Top anchoring>Slbling」選為「InversionModelingInvSettings_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
        + **1.10.83.1.2 「Left anchoring>Slbling」選為「InversionModelingInvSettings_RemoveTimes_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選右邊的。**  
        + **1.10.83.1.3 「Border space」正中間那格填「0」，上面那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
        + **1.10.83.1.4 設定完後關閉Anchors設定視窗。**
      + **1.10.83.2 設定「Caption」為「資料剔除[%]:」。**
      + **1.10.83.3 設定「Height」為「48」。**
      + **1.10.83.4 設定「Hint」為「基於最終模型下的擬合誤差進行資料剔除的百分比，單位:[%]。預設值=5。限制填入1-99的正整數，請注意刪除過多可能導致程式錯誤。」。**
      + **1.10.83.5 設定「ShowHint」為「True」。**
      + **1.10.83.6 設定「Width」為「100」。**
  + **1.10.84 拖拉一個「Standard>TEdit」到「InversionModelingInvSettings_RemovePercentage_GroupBox」中。預設名稱會是「Edit1」，修改「Name」為「InversionModelingInvSettings_RemovePercentage_Edit」**
    + **1.10.84.1 設定「Anchors」。**  
      + **1.10.84.1.1 「Top anchoring>Slbling」選為「InversionModelingInvSettings_RemovePercentage_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**  
      + **1.10.84.1.2 「Left anchoring>Slbling」選為「InversionModelingInvSettings_RemovePercentage_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.10.84.1.3 「Right anchoring>Slbling」選為「InversionModelingInvSettings_RemovePercentage_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
      + **1.10.84.1.4 「Border space」正中間那格填「0」，左邊那格填「8」，右邊那格填「8」。**  
      + **1.10.84.1.5 設定完後關閉Anchors設定視窗。**  
    + **1.10.84.6 設定「Text」為「5」。**
  + **1.10.85 拖拉一個「Standard>TGroupBox」到「InversionModelingInvSettings_GroupBox」中。預設名稱會是「GroupBox1」，修改「Name」為「InversionModelingInvSettings_startModel_GroupBox」。**
      + **1.10.85.1 設定「Anchors」。**  
        + **1.10.85.1.1 「Top anchoring>Slbling」選為「InversionModelingInvSettings_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
        + **1.10.85.1.2 「Left anchoring>Slbling」選為「InversionModelingInvSettings_RemovePercentage_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選右邊的。**  
        + **1.10.85.1.3 「Border space」正中間那格填「0」，上面那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
        + **1.10.85.1.4 設定完後關閉Anchors設定視窗。**
      + **1.10.85.2 設定「Caption」為「逆推起始模型:」。**
      + **1.10.85.3 設定「Height」為「48」。**
      + **1.10.85.4 設定「Hint」為「起始模型，單位:[Ohm-m]。預設值=「-1」。限制填正數或「-1」或「-2」或「-3」。填正數則用該值製作均值起始模型。填「-1」表示使用資料的中位數製作均值起始模型。填「-2」表示使用前面已經載入的全網格MeshVTK內的電阻率值。填「-3」表示使用另外準備的研究區域網格StartModelVTK內的電阻率值，該檔案名稱必須是「????」。」。**
      + **1.10.85.5 設定「ShowHint」為「True」。**
      + **1.10.85.6 設定「Width」為「100」。**
  + **1.10.86 拖拉一個「Standard>TEdit」到「InversionModelingInvSettings_startModel_GroupBox」中。預設名稱會是「Edit1」，修改「Name」為「InversionModelingInvSettings_startModel_Edit」**
    + **1.10.86.1 設定「Anchors」。**  
      + **1.10.86.1.1 「Top anchoring>Slbling」選為「InversionModelingInvSettings_startModel_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**  
      + **1.10.86.1.2 「Left anchoring>Slbling」選為「InversionModelingInvSettings_startModel_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.10.86.1.3 「Right anchoring>Slbling」選為「InversionModelingInvSettings_startModel_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
      + **1.10.86.1.4 「Border space」正中間那格填「0」，左邊那格填「8」，右邊那格填「8」。**  
      + **1.10.86.1.5 設定完後關閉Anchors設定視窗。**  
    + **1.10.86.6 設定「Text」為「-1」。**
  + **1.10.87 拖拉一個「Standard>TGroupBox」到「InversionModelingInvSettings_GroupBox」中。預設名稱會是「GroupBox1」，修改「Name」為「InversionModelingInvSettings_ColorBar_GroupBox」。**
      + **1.10.87.1 設定「Anchors」。**  
        + **1.10.87.1.1 「Top anchoring>Slbling」選為「InversionModelingInvSettings_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
        + **1.10.87.1.2 「Left anchoring>Slbling」選為「InversionModelingInvSettings_startModel_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選右邊的。**  
        + **1.10.87.1.3 「Border space」正中間那格填「0」，上面那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
        + **1.10.87.1.4 設定完後關閉Anchors設定視窗。**
      + **1.10.87.2 設定「Caption」為「色階範圍[Ohm-m]:」。**
      + **1.10.87.3 設定「Height」為「48」。**
      + **1.10.87.4 設定「Hint」為「最終模型展示時的色階範圍，單位:[Ohm-m]。預設值=「[1,10000]」。限制填入只有兩個元素的陣列，第一個元素為最小值，第二個元素為最大值，限制填正數。」。**
      + **1.10.87.5 設定「ShowHint」為「True」。**
      + **1.10.87.6 設定「Width」為「160」。**
  + **1.10.88 拖拉一個「Standard>TEdit」到「InversionModelingInvSettings_ColorBar_GroupBox」中。預設名稱會是「Edit1」，修改「Name」為「InversionModelingInvSettings_ColorBar_Edit」**
    + **1.10.88.1 設定「Anchors」。**  
      + **1.10.88.1.1 「Top anchoring>Slbling」選為「InversionModelingInvSettings_ColorBar_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**  
      + **1.10.88.1.2 「Left anchoring>Slbling」選為「InversionModelingInvSettings_ColorBar_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.10.88.1.3 「Right anchoring>Slbling」選為「InversionModelingInvSettings_ColorBar_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
      + **1.10.88.1.4 「Border space」正中間那格填「0」，左邊那格填「8」，右邊那格填「8」。**  
      + **1.10.88.1.5 設定完後關閉Anchors設定視窗。**  
    + **1.10.88.6 設定「Text」為「[1,10000]」。**
  + **1.10.89 拖拉一個「Standard>TGroupBox」到「InversionModelingInvSettings_GroupBox」中。預設名稱會是「GroupBox1」，修改「Name」為「InversionModelingInvSettings_XYMinMax_GroupBox」。**
      + **1.10.89.1 設定「Anchors」。**  
        + **1.10.89.1.1 「Top anchoring>Slbling」選為「InversionModelingInvSettings_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
        + **1.10.89.1.2 「Left anchoring>Slbling」選為「InversionModelingInvSettings_ColorBar_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選右邊的。**
        + **1.10.89.1.3 「Right anchoring>Slbling」選為「InversionModelingInvSettings_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
        + **1.10.89.1.4 「Border space」正中間那格填「0」，上面那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
        + **1.10.89.1.5 設定完後關閉Anchors設定視窗。**
      + **1.10.89.2 設定「Caption」為「裁切範圍[m]:」。**
      + **1.10.89.3 設定「Height」為「48」。**
      + **1.10.89.4 設定「Hint」為「最終模型展示時的裁切範圍，單位:[m]。要打勾才會啟用，沒有打勾就不裁切。預設值=「[-40,1600,-624,64]」。限制填入只有四個元素的陣列，第一個元素為X最小值，第二個元素為X最大值，第三個元素為Y最小值，第四個元素為Y最大值。請確認數值正確性以避免發生錯誤。」。**
      + **1.10.89.5 設定「ShowHint」為「True」。**
  + **1.10.90 拖拉一個「Standard>TCheckBox」到「InversionModelingInvSettings_XYMinMax_GroupBox」中。預設名稱會是「CheckBox1」，修改「Name」為「InversionModelingInvSettings_XYMinMax_CheckBox」**
    + **1.10.90.1 設定「Anchors」。**  
      + **1.10.90.1.1 「Top anchoring>Slbling」選為「InversionModelingInvSettings_XYMinMax_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
      + **1.10.90.1.2 「Left anchoring>Slbling」選為「InversionModelingInvSettings_XYMinMax_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.10.90.1.3 「Border space」正中間那格填「0」，上面那格填「1」，左邊那格填「8」，右邊那格填「8」。**  
      + **1.10.90.1.4 設定完後關閉Anchors設定視窗。**  
    + **1.10.90.2 設定「Caption」為「啟用裁切:」。**
  + **1.10.91 拖拉一個「Standard>TEdit」到「InversionModelingInvSettings_XYMinMax_GroupBox」中。預設名稱會是「Edit1」，修改「Name」為「InversionModelingInvSettings_XYMinMax_Edit」**
    + **1.10.91.1 設定「Anchors」。**  
      + **1.10.91.1.1 「Top anchoring>Slbling」選為「InversionModelingInvSettings_XYMinMax_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**  
      + **1.10.91.1.2 「Left anchoring>Slbling」選為「InversionModelingInvSettings_XYMinMax_CheckBox:TCheckBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選右邊的。**  
      + **1.10.91.1.3 「Right anchoring>Slbling」選為「InversionModelingInvSettings_XYMinMax_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
      + **1.10.91.1.4 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「8」。**  
      + **1.10.91.1.5 設定完後關閉Anchors設定視窗。**  
    + **1.10.91.6 設定「Text」為「[-40,1600,-624,64]」。**
  + **1.10.92 拖拉一個「Standard>TGroupBox」到「InversionModelingParameters_GroupBox」中。預設名稱會是「GroupBox1」，修改「Name」為「InversionModelingOutputSettings_GroupBox」。**
      + **1.10.92.1 設定「Anchors」。**  
        + **1.10.92.1.1 「Top anchoring>Slbling」選為「InversionModelingInput03MeshJSON_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選下面的。**
        + **1.10.92.1.2 「Left anchoring>Slbling」選為「InversionModelingInvAutoMesh_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選右邊的。**  
        + **1.10.92.1.3 「Right anchoring>Slbling」選為「InversionModelingParameters_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
        + **1.10.92.1.4 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
        + **1.10.92.1.5 設定完後關閉Anchors設定視窗。**
      + **1.10.92.2 設定「Caption」為「輸出設定:」。**
      + **1.10.92.3 設定「Height」為「70」。**
  + **1.10.93 拖拉一個「Standard>TGroupBox」到「InversionModelingOutputSettings_GroupBox」中。預設名稱會是「GroupBox1」，修改「Name」為「InversionModelingOutputSettings_verbose_GroupBox」。**
      + **1.10.93.1 設定「Anchors」。**  
        + **1.10.93.1.1 「Top anchoring>Slbling」選為「InversionModelingOutputSettings_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
        + **1.10.93.1.2 「Left anchoring>Slbling」選為「InversionModelingOutputSettings_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**
        + **1.10.93.1.3 「Border space」正中間那格填「0」，上面那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
        + **1.10.93.1.4 設定完後關閉Anchors設定視窗。**
      + **1.10.93.2 設定「Caption」為「詳細運行資訊:」。**
      + **1.10.93.3 設定「Height」為「48」。**
      + **1.10.93.4 設定「Hint」為「逆推運算詳細運行資訊。預設值=「停用」。」。**
      + **1.10.93.5 設定「ShowHint」為「True」。**
      + **1.10.93.6 設定「Width」為「100」。**
  + **1.10.94 拖拉一個「Standard>TComboBox」到「InversionModelingOutputSettings_verbose_GroupBox」中。預設名稱會是「ComboBox1」，修改「Name」為「InversionModelingOutputSettings_verbose_ComboBox」。**
      + **1.10.94.1 設定「Anchors」。**  
        + **1.10.94.1.1 「Top anchoring>Slbling」選為「InversionModelingOutputSettings_verbose_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
        + **1.10.94.1.2 「Left anchoring>Slbling」選為「InversionModelingOutputSettings_verbose_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**
        + **1.10.94.1.3 「Right anchoring>Slbling」選為「InversionModelingOutputSettings_verbose_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
        + **1.10.94.1.4 「Border space」正中間那格填「0」，左邊那格填「8」，右邊那格填「8」。**  
        + **1.10.94.1.5 設定完後關閉Anchors設定視窗。**
      + **1.10.94.2 設定「Items」為如下內容。**
      ```
      停用
      啟用
      ```
      + **1.10.94.3 設定「ItemIndex」為「0」。**
      + **1.10.94.4 設定「Style」為「csDropDownList」。**
  + **1.10.95 拖拉一個「Standard>TGroupBox」到「InversionModelingOutputSettings_GroupBox」中。預設名稱會是「GroupBox1」，修改「Name」為「InversionModelingOutputSettings_MainName_GroupBox」。**
      + **1.10.95.1 設定「Anchors」。**  
        + **1.10.95.1.1 「Top anchoring>Slbling」選為「InversionModelingOutputSettings_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
        + **1.10.95.1.2 「Left anchoring>Slbling」選為「InversionModelingOutputSettings_verbose_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**
        + **1.10.95.1.3 「Right anchoring>Slbling」選為「InversionModelingOutputSettings_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
        + **1.10.95.1.4 「Border space」正中間那格填「0」，上面那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
        + **1.10.95.1.5 設定完後關閉Anchors設定視窗。**
      + **1.10.95.2 設定「Caption」為「測線名稱:」。**
      + **1.10.95.3 設定「Height」為「48」。**
      + **1.10.95.4 設定「Hint」為「測線名稱。將作為檔案命名用途。」。**
      + **1.10.95.5 設定「ShowHint」為「True」。**
      + **1.10.95.6 設定「Width」為「100」。**
  + **1.10.96 拖拉一個「Standard>TEdit」到「InversionModelingOutputSettings_MainName_GroupBox」中。預設名稱會是「Edit1」，修改「Name」為「InversionModelingOutputSettings_MainName_Edit」**
    + **1.10.96.1 設定「Anchors」。**  
      + **1.10.96.1.1 「Top anchoring>Slbling」選為「InversionModelingOutputSettings_MainName_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**  
      + **1.10.96.1.2 「Left anchoring>Slbling」選為「InversionModelingOutputSettings_MainName_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.10.96.1.3 「Right anchoring>Slbling」選為「InversionModelingOutputSettings_MainName_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
      + **1.10.96.1.4 「Border space」正中間那格填「0」，左邊那格填「8」，右邊那格填「8」。**  
      + **1.10.96.1.5 設定完後關閉Anchors設定視窗。**  
    + **1.10.96.2 設定「Text」為「ProfileName」。**
  + 1.11 第2個分頁「逆推模擬運算參數」(InversionModelingSettings_TabSheet):
    + **1.11.1 拖拉一個「Standard>TGroupBox」到「InversionModelingSettings_TabSheet」中。預設名稱會是「GroupBox1」，修改「Name」為「InversionModelingSettingsCmdLog_GroupBox」。**
      + **1.11.1.1 設定「Align」為「alRight」。**
      + **1.11.1.2 設定「BorderSpacing>Left」為「3」。**
      + **1.11.1.3 設定「BorderSpacing>Right」為「1」。**
      + **1.11.1.4 設定「Caption」為「逆推模擬運算運行紀錄:」。** 
      + **1.11.1.5 設定「Width」為「500」。**
  + **1.11.2 拖拉一個「Standard>TMemo」到「InversionModelingSettingsCmdLog_GroupBox」中。預設名稱會是「Memo1」，修改「Name」為「InversionModelingSettingsCmdLog_Memo」**
    + **1.11.2.1 設定「Anchors」。**  
      + **1.11.2.1.1 「Top anchoring>Slbling」選為「TimeSeriesProcessingSettingsCmdLog_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。選擇「Top anchoring」右側三個鈕選上面的。** 
      + **1.11.2.1.2 「Left anchoring>Slbling」選為「TimeSeriesProcessingSettingsCmdLog_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.11.2.1.3 「Right anchoring>Slbling」選為「TimeSeriesProcessingSettingsCmdLog_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
      + **1.11.2.1.4 「Bottom anchoring>Slbling」選為「TimeSeriesProcessingSettingsCmdLog_GroupBox:TGroupBox」。勾選「Bottom anchoring>Enable」為打勾。選擇「Bottom anchoring」右側三個鈕選下面的。** 
      + **1.11.2.1.5 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」，下面那格填「3」。**  
      + **1.11.2.1.6 設定完後關閉Anchors設定視窗。**  
    + **1.11.2.2 設定「Color」為「clBlack」。**
    + **1.11.2.3 設定「Font>Color」為「clLime」。**
    + **1.11.2.4 設定「Lines」清空。**
    + **1.11.2.5 設定「ReadOnly」為「True」。**
    + **1.11.2.6 設定「ScrollBars」為「ssBoth」。**
    + **1.11.2.7 設定「WordWrap」為「False」。**
  + **1.11.3 拖拉一個「Standard>TGroupBox」到「InversionModelingSettings_TabSheet」中。預設名稱會是「GroupBox1」，修改「Name」為「InversionModelingSettingsDefaultJson_GroupBox」**
    + **1.11.3.1 設定「Anchors」。**  
      + **1.11.3.1.1 「Top anchoring>Slbling」選為「InversionModelingSettings_TabSheet:TTabSheet」。維持「Top anchoring>Enable」為打勾。選擇「Top anchoring」右側三個鈕選上面的。** 
      + **1.11.3.1.2 「Left anchoring>Slbling」選為「InversionModelingSettings_TabSheet:TTabSheet」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.11.3.1.3 「Right anchoring>Slbling」選為「InversionModelingSettingsCmdLog_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選左邊的。**  
      + **1.11.3.1.4 設定完後關閉Anchors設定視窗。**  
    + **1.11.3.2 設定「Caption」為「預設參數(Default_Inversion2D.json):」。**
    + **1.11.3.3 設定「Height」為「200」。**
  + **1.11.4 拖拉一個「Standard>TMemo」到「InversionModelingSettingsDefaultJson_GroupBox」中。預設名稱會是「Memo1」，修改「Name」為「InversionModelingSettingsDefaultJson_Memo」**
    + **1.11.4.1 設定「Anchors」。**  
      + **1.11.4.1.1 「Top anchoring>Slbling」選為「InversionModelingSettingsDefaultJson_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。選擇「Top anchoring」右側三個鈕選上面的。** 
      + **1.11.4.1.2 「Left anchoring>Slbling」選為「InversionModelingSettingsDefaultJson_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.11.4.1.3 「Right anchoring>Slbling」選為「InversionModelingSettingsDefaultJson_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
      + **1.11.4.1.4 「Bottom anchoring>Slbling」選為「InversionModelingSettingsDefaultJson_GroupBox:TGroupBox」。勾選「Bottom anchoring>Enable」為打勾。選擇「Bottom anchoring」右側三個鈕選下面的。** 
      + **1.11.4.1.5 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」，下面那格填「3」。**  
      + **1.11.4.1.6 設定完後關閉Anchors設定視窗。**  
    + **1.11.4.2 設定「Lines」清空。**
    + **1.11.4.3 設定「ReadOnly」為「True」。**
    + **1.11.4.4 設定「ScrollBars」為「ssBoth」。**
    + **1.11.4.5 設定「WordWrap」為「False」。**
  + **1.11.5 拖拉一個「Standard>TGroupBox」到「InversionModelingSettings_TabSheet」中。預設名稱會是「GroupBox1」，修改「Name」為「InversionModelingSettingsNowJson_GroupBox」**
    + **1.11.5.1 設定「Anchors」。**  
      + **1.11.5.1.1 「Top anchoring>Slbling」選為「InversionModelingSettingsDefaultJson_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。選擇「Top anchoring」右側三個鈕選下面的。** 
      + **1.11.5.1.2 「Left anchoring>Slbling」選為「InversionModelingSettingsDefaultJson_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.11.5.1.3 「Right anchoring>Slbling」選為「InversionModelingSettingsDefaultJson_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
      + **1.11.5.1.4 設定完後關閉Anchors設定視窗。**  
    + **1.11.5.2 設定「Caption」為「目前參數(Inversion2D.json):」。**
    + **1.11.5.3 設定「Height」為「200」。**
  + **1.11.6 拖拉一個「Standard>TMemo」到「InversionModelingSettingsNowJson_GroupBox」中。預設名稱會是「Memo1」，修改「Name」為「InversionModelingSettingsNowJson_Memo」**
    + **1.11.6.1 設定「Anchors」。**  
      + **1.11.6.1.1 「Top anchoring>Slbling」選為「InversionModelingSettingsNowJson_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。選擇「Top anchoring」右側三個鈕選上面的。** 
      + **1.11.6.1.2 「Left anchoring>Slbling」選為「InversionModelingSettingsNowJson_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.11.6.1.3 「Right anchoring>Slbling」選為「InversionModelingSettingsNowJson_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
      + **1.11.6.1.4 「Bottom anchoring>Slbling」選為「InversionModelingSettingsNowJson_GroupBox:TGroupBox」。勾選「Bottom anchoring>Enable」為打勾。選擇「Bottom anchoring」右側三個鈕選下面的。** 
      + **1.11.6.1.5 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」，下面那格填「3」。**  
      + **1.11.6.1.6 設定完後關閉Anchors設定視窗。**  
    + **1.11.6.2 設定「Lines」清空。**
    + **1.11.6.3 設定「ReadOnly」為「True」。**
    + **1.11.6.4 設定「ScrollBars」為「ssBoth」。**
    + **1.11.6.5 設定「WordWrap」為「False」。**
  + **1.11.7 拖拉一個「Standard>TGroupBox」到「InversionModelingSettings_TabSheet」中。預設名稱會是「GroupBox1」，修改「Name」為「InversionModelingSettingsReadme_GroupBox」**
    + **1.11.7.1 設定「Anchors」。**  
      + **1.11.7.1.1 「Top anchoring>Slbling」選為「InversionModelingSettingsNowJson_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。選擇「Top anchoring」右側三個鈕選下面的。** 
      + **1.11.7.1.2 「Left anchoring>Slbling」選為「InversionModelingSettingsNowJson_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.11.7.1.3 「Right anchoring>Slbling」選為「InversionModelingSettingsNowJson_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**
      + **1.11.7.1.4 「Bottom anchoring>Slbling」選為「InversionModelingSettings_TabSheet:TTabSheet」。勾選「Bottom anchoring>Enable」為打勾。選擇「Bottom anchoring」右側三個鈕選下面的。** 
      + **1.11.7.1.5 設定完後關閉Anchors設定視窗。**  
    + **1.11.7.2 設定「Caption」為「說明:」。**
  + **1.11.8 拖拉一個「Standard>TMemo」到「InversionModelingSettingsReadme_GroupBox」中。預設名稱會是「Memo1」，修改「Name」為「InversionModelingSettingsReadme_Memo」**
    + **1.11.8.1 設定「Anchors」。**  
      + **1.11.8.1.1 「Top anchoring>Slbling」選為「InversionModelingSettingsReadme_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。選擇「Top anchoring」右側三個鈕選上面的。** 
      + **1.11.8.1.2 「Left anchoring>Slbling」選為「InversionModelingSettingsReadme_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
      + **1.11.8.1.3 「Right anchoring>Slbling」選為「InversionModelingSettingsReadme_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
      + **1.11.8.1.4 「Bottom anchoring>Slbling」選為「InversionModelingSettingsReadme_GroupBox:TGroupBox」。勾選「Bottom anchoring>Enable」為打勾。選擇「Bottom anchoring」右側三個鈕選下面的。** 
      + **1.11.8.1.5 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」，下面那格填「3」。**  
      + **1.11.8.1.6 設定完後關閉Anchors設定視窗。**  
    + **1.11.8.2 設定「Lines」清空。**
    + **1.11.8.3 設定「ReadOnly」為「True」。**
    + **1.11.8.4 設定「ScrollBars」為「ssBoth」。**
    + **1.11.8.5 設定「WordWrap」為「False」。**

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
+ 2.2 去修改「InversionModelingInput01Ohm_ManualSelect_Button」的「Event」頁面下「OnClick」為如下程式碼。
```pascal
procedure TForm1.InversionModelingInput01Ohm_ManualSelect_ButtonClick(
  Sender: TObject);
begin
  //--------------------------------------------------------------------------
  // OpenDialog1設定對話框標題
  OpenDialog1.Title := '請選擇一個ohm或dat檔案';
  // 設定預設開啟的目錄
  OpenDialog1.InitialDir := Current_Folder_Path;
  // 設定過濾檔案名稱
  OpenDialog1.Filter := '資料檔案(*.ohm;*.dat)|*.ohm;*.dat';
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
    InversionModelingInput01Ohm_Edit.Text := OpenDialog1.FileName;
  end
  else
  begin
    // 使用者點擊了「取消」
    //ShowMessage('取消選取');
  end;
end;  
```
+ 2.3 去修改「InversionModelingInput01Ohm_OpenFolder_Button」的「Event」頁面下「OnClick」為如下程式碼。
```pascal
procedure TForm1.InversionModelingInput01Ohm_OpenFolder_ButtonClick(
  Sender: TObject);
var
  temp_str: AnsiString;
begin
  // 1. 從完整路徑中提取資料夾路徑
  temp_str := ExtractFilePath(InversionModelingInput01Ohm_Edit.Text);
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
