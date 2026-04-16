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
+ 2.4 去修改「InversionModelingInput02VTK_ManualSelect_Button」的「Event」頁面下「OnClick」為如下程式碼。
```pascal
procedure TForm1.InversionModelingInput02VTK_ManualSelect_ButtonClick(
  Sender: TObject);
begin
  //--------------------------------------------------------------------------
  // OpenDialog1設定對話框標題
  OpenDialog1.Title := '請選擇一個ohm或dat檔案';
  // 設定預設開啟的目錄
  OpenDialog1.InitialDir := Current_Folder_Path;
  // 設定過濾檔案名稱
  OpenDialog1.Filter := 'vtk檔案(*.vtk)|*.vtk';
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
    InversionModelingInput02VTK_Edit.Text := OpenDialog1.FileName;
  end
  else
  begin
    // 使用者點擊了「取消」
    //ShowMessage('取消選取');
  end;
end;   
```
+ 2.5 去修改「InversionModelingInput02VTK_OpenFolder_Button」的「Event」頁面下「OnClick」為如下程式碼。
```pascal
procedure TForm1.InversionModelingInput02VTK_OpenFolder_ButtonClick(
  Sender: TObject);
var
  temp_str: AnsiString;
begin
  // 1. 從完整路徑中提取資料夾路徑
  temp_str := ExtractFilePath(InversionModelingInput02VTK_Edit.Text);
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
+ 2.6 去修改「InversionModelingInput03MeshJSON_ManualSelect_Button」的「Event」頁面下「OnClick」為如下程式碼。
```pascal
procedure TForm1.InversionModelingInput03MeshJSON_ManualSelect_ButtonClick(
  Sender: TObject);
begin
  //--------------------------------------------------------------------------
  // OpenDialog1設定對話框標題
  OpenDialog1.Title := '請選擇一個ohm或dat檔案';
  // 設定預設開啟的目錄
  OpenDialog1.InitialDir := Current_Folder_Path;
  // 設定過濾檔案名稱
  OpenDialog1.Filter := 'json檔案(*.json)|*.json';
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
    InversionModelingInput03MeshJSON_Edit.Text := OpenDialog1.FileName;
  end
  else
  begin
    // 使用者點擊了「取消」
    //ShowMessage('取消選取');
  end;
end;  
```
+ 2.7 去修改「InversionModelingInput03MeshJSON_OpenFolder_Button」的「Event」頁面下「OnClick」為如下程式碼。
```pascal
procedure TForm1.InversionModelingInput03MeshJSON_OpenFolder_ButtonClick(
  Sender: TObject);
var
  temp_str: AnsiString;
begin
  // 1. 從完整路徑中提取資料夾路徑
  temp_str := ExtractFilePath(InversionModelingInput03MeshJSON_Edit.Text);
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
+ 2.8 去修改「InversionModelingInput02VTK_GroupBox」的「Enabled」為「False」。
+ 2.9 去修改「InversionModelingInput03MeshJSON_GroupBox」的「Enabled」為「False」。
+ 2.10 去修改「InversionModelingInvAutoMesh_Enable_CheckBox」的「Checked」為「True」。
+ 2.11 去修改「InversionModelingInvAutoMesh_Enable_CheckBox」的「Event」頁面下「OnChange」為如下程式碼。
```pascal
procedure TForm1.InversionModelingInvAutoMesh_Enable_CheckBoxChange(
  Sender: TObject);
begin
  //隨 CheckBox 狀態連動 (打勾 = 可用)
  InversionModelingInvAutoMesh_quality_GroupBox.Enabled := InversionModelingInvAutoMesh_Enable_CheckBox.Checked;
  InversionModelingInvAutoMesh_paraDepth_GroupBox.Enabled := InversionModelingInvAutoMesh_Enable_CheckBox.Checked;
  InversionModelingInvAutoMesh_boundary_GroupBox.Enabled := InversionModelingInvAutoMesh_Enable_CheckBox.Checked;
  InversionModelingInvAutoMesh_paraMaxCellSize_GroupBox.Enabled := InversionModelingInvAutoMesh_Enable_CheckBox.Checked;
  InversionModelingInvAutoMesh_addNodes_GroupBox.Enabled := InversionModelingInvAutoMesh_Enable_CheckBox.Checked;
  InversionModelingInvAutoMesh_paraDX_GroupBox.Enabled := InversionModelingInvAutoMesh_Enable_CheckBox.Checked;
  // 與 CheckBox 狀態相反 (打勾 = 不可用)
  InversionModelingInput02VTK_GroupBox.Enabled := not InversionModelingInvAutoMesh_Enable_CheckBox.Checked;
  InversionModelingInput03MeshJSON_GroupBox.Enabled := not InversionModelingInvAutoMesh_Enable_CheckBox.Checked;
end;
```
+ 2.12 去修改「InversionModelingDataSettings_quality_GroupBox」的「Enabled」為「False」。
+ 2.13 去修改「InversionModelingDataSettings_paraDepth_GroupBox」的「Enabled」為「False」。
+ 2.14 去修改「InversionModelingDataSettings_boundary_GroupBox」的「Enabled」為「False」。
+ 2.15 去修改「InversionModelingDataSettings_paraMaxCellSize_GroupBox」的「Enabled」為「False」。
+ 2.16 去修改「InversionModelingDataSettings_addNodes_GroupBox」的「Enabled」為「False」。
+ 2.17 去修改「InversionModelingDataSettings_paraDX_GroupBox」的「Enabled」為「False」。
+ 2.18 去修改「InversionModelingDataSettings_ReCalK_ComboBox」的「Event」頁面下「OnChange」為如下程式碼。
```pascal
procedure TForm1.InversionModelingDataSettings_ReCalK_ComboBoxChange(
  Sender: TObject);
begin
  // 只有當選取的文字完全符合 "Ideal_Mesh" 時才啟用
  InversionModelingDataSettings_quality_GroupBox.Enabled := (InversionModelingDataSettings_ReCalK_ComboBox.Text = 'Ideal_Mesh');
  InversionModelingDataSettings_paraDepth_GroupBox.Enabled := (InversionModelingDataSettings_ReCalK_ComboBox.Text = 'Ideal_Mesh');
  InversionModelingDataSettings_boundary_GroupBox.Enabled := (InversionModelingDataSettings_ReCalK_ComboBox.Text = 'Ideal_Mesh');
  InversionModelingDataSettings_paraMaxCellSize_GroupBox.Enabled := (InversionModelingDataSettings_ReCalK_ComboBox.Text = 'Ideal_Mesh');
  InversionModelingDataSettings_addNodes_GroupBox.Enabled := (InversionModelingDataSettings_ReCalK_ComboBox.Text = 'Ideal_Mesh');
  InversionModelingDataSettings_paraDX_GroupBox.Enabled := (InversionModelingDataSettings_ReCalK_ComboBox.Text = 'Ideal_Mesh');
end; 
```
+ 2.19 去修改「InversionModelingDataPrepare_UseFakeDataRhoa_Edit」的「Enabled」為「False」。
+ 2.20 去修改「InversionModelingDataPrepare_UseFakeDataEnable_CheckBox」的「Event」頁面下「OnChange」為如下程式碼。
```pascal
procedure TForm1.InversionModelingDataPrepare_UseFakeDataEnable_CheckBoxChange(
  Sender: TObject);
begin
  //隨 CheckBox 狀態連動 (打勾 = 可用)
  InversionModelingDataPrepare_UseFakeDataRhoa_Edit.Enabled := InversionModelingDataPrepare_UseFakeDataEnable_CheckBox.Checked;
end;  
```
+ 2.21 去修改「InversionModelingDataPrepare_RemoveBadElectrode_Edit」的「Enabled」為「False」。
+ 2.22 去修改「InversionModelingDataPrepare_RemoveBadElectrode_CheckBox」的「Event」頁面下「OnChange」為如下程式碼。
```pascal
procedure TForm1.InversionModelingDataPrepare_RemoveBadElectrode_CheckBoxChange(
  Sender: TObject);
begin
  //隨 CheckBox 狀態連動 (打勾 = 可用)
  InversionModelingDataPrepare_RemoveBadElectrode_Edit.Enabled := InversionModelingDataPrepare_RemoveBadElectrode_CheckBox.Checked;
end;  
```
+ 2.23 去修改「InversionModelingInvSettings_XYMinMax_Edit」的「Enabled」為「False」。
+ 2.24 去修改「InversionModelingInvSettings_XYMinMax_CheckBox」的「Event」頁面下「OnChange」為如下程式碼。
```pascal
procedure TForm1.InversionModelingDataPrepare_RemoveBadElectrode_CheckBoxChange(
  Sender: TObject);
begin
  //隨 CheckBox 狀態連動 (打勾 = 可用)
  InversionModelingDataPrepare_RemoveBadElectrode_Edit.Enabled := InversionModelingDataPrepare_RemoveBadElectrode_CheckBox.Checked;
end;  
```
+ 2.25 修改「InversionModelingSettingsDefaultJson_Memo」的「Lines」為以下文字。
```
{
"Inversion2D_Version":"v20260401a",
"Inversion2D_Author":"HsiupoYeh",
"InputFile01_Ohm_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"InputFile01_Ohm_FileName":"Input_ERTMaker_Inversion2D/XP2_R01_L01_I01_CalData.dat",
"Mesh_Setting01_AutoMesh_Enable_Readme":"逆推參數-使用自動三角網格。若要啟用請填入字串Yes，若不啟用請填入字串No。若啟用則使用自動產生三角網格，不會讀取VTK檔案與JSON檔案。",
"Mesh_Setting01_AutoMesh_Enable":"Yes",
"Mesh_Setting02_AutoMesh_quality_Readme":"逆推參數-設定自動三角網格的網格品質。控制不要讓三角形太扁長。預設值:32，建議填入32.0~34.0之間的浮點數。此設定只有啟用自動網格才會有效。",
"Mesh_Setting02_AutoMesh_quality":32,
"Mesh_Setting03_AutoMesh_paraDepth_Readme":"逆推參數-設定自動三角網格的研究區域內最大深度。預設值:-1，單位:[m]，設定小於0的數字會自動設定為測線電極X方向長度的0.4倍。此設定只有啟用自動網格才會有效。",
"Mesh_Setting03_AutoMesh_paraDepth":-1,
"Mesh_Setting04_AutoMesh_boundary_Readme":"逆推參數-設定自動三角網格的外邊界倍數。預設值:-1，建議填入4~10之間的整數，設定小於4的數字仍會強制設定為4。此設定只有啟用自動網格才會有效。",
"Mesh_Setting04_AutoMesh_boundary":-1,
"Mesh_Setting05_AutoMesh_paraMaxCellSize_Readme":"逆推參數-設定自動三角網格的網格面積上限。單位:[m^2]。預設值:0，表示自動決定。此設定只有啟用自動網格才會有效。",
"Mesh_Setting05_AutoMesh_paraMaxCellSize":0,
"Mesh_Setting06_AutoMesh_addNodes_Readme":"逆推參數-設定自動三角網格的網格地表電極之間的節點數量。預設值:1。限制填入正整數。此值填「1」時無效果，改由paraDX決定增加的方式；填其他正整數則線性均勻增加兩個電極之間的節點數量。此設定只有啟用自動網格才會有效。",
"Mesh_Setting06_AutoMesh_addNodes":1,
"Mesh_Setting07_AutoMesh_paraDX_Readme":"逆推參數-設定自動三角網格的網格地表電極之間的節點數量。預設值:1。限制填入浮點數。僅有addNodes小於等於1的時候有效。此值大於等於0.5時，等於在兩個電極正中間增加一個節點。小於0.5的時候，在每個電極的左右兩邊各增加一個節點，距離是電極間距乘以此浮點數。此設定只有啟用自動網格才會有效。",
"Mesh_Setting07_AutoMesh_paraDX":1,
"InputFile02_MeshVTK_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"InputFile02_MeshVTK_FileName":"Input_ERTMaker_Inversion2D/InputFile02_MeshVTK_FileName.vtk",
"InputFile03_MeshBCMarkersJSON_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"InputFile03_MeshBCMarkersJSON_FileName":"Input_ERTMaker_Inversion2D/InputFile03_MeshBCMarkersJSON_FileName.json",
"Data_Setting00_RecalculateDataMode_Readme":"逆推參數-重新計算觀測資料方式。預設值:「r」。可填選項:「r」、「rhoa」。",
"Data_Setting00_RecalculateDataMode":"r",
"Data_Setting01_Relative_Error_Percentage_Readme":"逆推參數-資料預估相對誤差百分比，單位:[%]。用於建構目標函數，野外蒐集的資料品質好的推薦使用3[%]，若品質不好則考慮增加。",
"Data_Setting01_Relative_Error_Percentage":3,
"Data_Setting02_GeometricFactorForInversion_Readme":"逆推參數-逆推內部採用的幾何因子計算。若要使用網格綁定的數值解(K_Mesh)請填入字串「K_Mesh」。若要使用解析解公式(K0_Mesh)請填入字串「K0_Mesh」，建議僅有水平地層可以填「K0_Mesh」。",
"Data_Setting02_GeometricFactorForInversion":"K_Mesh",
"Data_Setting03_GeometricFactorForRecalculateData_Readme":"逆推參數-觀測資料轉換視電阻率採用的理想幾何因子計算。若要使用自訂理想網格計算數值解(K_Ideal)請填入字串「Ideal_Mesh」，此時後方要填入網格設定。若要使用前面的網格綁定的數值解(K_Mesh)請填入字串「K_Mesh」。若要使用解析解公式(K0_Mesh)請填入字串「K0_Mesh」，建議僅有水平地層可以填「K0_Mesh」。",
"Data_Setting03_GeometricFactorForRecalculateData":"K_Mesh",
"Ideal_Mesh_Setting01_AutoMesh_quality_Readme":"理想幾何因子計算網格-設定三角網格的網格品質。控制不要讓三角形太扁長。預設值:32，建議填入32.0~34.0之間的浮點數。此設定只有啟用自動網格才會有效。",
"Ideal_Mesh_Setting01_AutoMesh_quality":30,
"Ideal_Mesh_Setting02_AutoMesh_paraDepth_Readme":"理想幾何因子計算網格-設定三角網格的研究區域內最大深度。預設值:-1，單位:[m]，設定小於0的數字會自動設定為測線電極X方向長度的0.4倍。此設定只有啟用自動網格才會有效。",
"Ideal_Mesh_Setting02_AutoMesh_paraDepth":-1,
"Ideal_Mesh_Setting03_AutoMesh_boundary_Readme":"理想幾何因子計算網格-設定三角網格的外邊界倍數。預設值:-1，建議填入4~10之間的整數，設定小於4的數字仍會強制設定為4。此設定只有啟用自動網格才會有效。",
"Ideal_Mesh_Setting03_AutoMesh_boundary":-1,
"Ideal_Mesh_Setting04_AutoMesh_paraMaxCellSize_Readme":"理想幾何因子計算網格-設定三角網格的網格面積上限。單位:[m^2]。預設值:0，表示自動決定。此設定只有啟用自動網格才會有效。",
"Ideal_Mesh_Setting04_AutoMesh_paraMaxCellSize":0,
"Ideal_Mesh_Setting05_AutoMesh_addNodes_Readme":"理想幾何因子計算網格-設定三角網格的網格地表電極之間的節點數量。預設值:1。限制填入正整數。此值填「1」時無效果，改由paraDX決定增加的方式；填其他正整數則線性均勻增加兩個電極之間的節點數量。此設定只有啟用自動網格才會有效。",
"Ideal_Mesh_Setting05_AutoMesh_addNodes":1,
"Ideal_Mesh_Setting06_AutoMesh_paraDX_Readme":"理想幾何因子計算網格-設定三角網格的網格地表電極之間的節點數量。預設值:1。限制填入浮點數。僅有addNodes小於等於1的時候有效。此值大於等於0.5時，等於在兩個電極正中間增加一個節點。小於0.5的時候，在每個電極的左右兩邊各增加一個節點，距離是電極間距乘以此浮點數。此設定只有啟用自動網格才會有效。",
"Ideal_Mesh_Setting06_AutoMesh_paraDX":1,
"Data_Setting03_RemoveLowCurrentData_LowerThan_A_Readme":"逆推參數-移除過低電流觀測資料，單位:[A]。建議值:0.01。限制填大於等於0的數字，填「0」表示不移除。注意，全部刪除資料會造成後續異常錯誤。請考量正確的儀器電流情形。",
"Data_Setting03_RemoveLowCurrentData_LowerThan_A":0,
"Data_Setting04_RemoveHighCurrentData_HigherThan_A_Readme":"逆推參數-移除過大電流觀測資料，單位:[A]。建議值:0.5。限制填大於0的數字。注意，全部刪除資料會造成後續異常錯誤。請考量正確的儀器電流情形。",
"Data_Setting04_RemoveHighCurrentData_HigherThan_A":10.0,
"Data_Setting05_RemoveLowVoltageData_LowerThan_V_Readme":"逆推參數-移除過低電壓觀測資料，單位:[V]。建議值:0.01。限制填大於等於0的數字，填「0」表示不移除。注意，全部刪除資料會造成後續異常錯誤。請考量正確的儀器電流情形。",
"Data_Setting05_RemoveLowVoltageData_LowerThan_V":0,
"Data_Setting06_RemoveHighVoltageData_HigherThan_V_Readme":"逆推參數-移除過大電壓觀測資料，單位:[V]。建議值:400。限制填大於0的數字。注意，全部刪除資料會造成後續異常錯誤。請考量正確的儀器電流情形。",
"Data_Setting06_RemoveHighVoltageData_HigherThan_V":999999,
"Data_Setting07_RemoveLowAppResData_LowerThan_OhmM_Readme":"逆推參數-移除過低視電阻率觀測資料，單位:[Ohm-m]。建議值:0.01。限制填大於等於0的數字，填「0」表示不移除。注意，全部刪除資料會造成後續異常錯誤。請考量正確的儀器電流情形。",
"Data_Setting07_RemoveLowAppResData_LowerThan_OhmM":0,
"Data_Setting08_RemoveHighAppResData_HigherThan_OhmM_Readme":"逆推參數-移除過大視電阻率觀測資料，單位:[Ohm-m]。建議值:100000。限制填大於0的數字。注意，全部刪除資料會造成後續異常錯誤。請考量正確的儀器電流情形。",
"Data_Setting08_RemoveHighAppResData_HigherThan_OhmM":999999,
"Data_Setting09_RemoveBadElectrodeData_List_Readme":"逆推參數-移除損毀電極觀測資料。建議值:[]。限制填正整數。注意，全部刪除資料會造成後續異常錯誤。請考量正確的儀器電流情形。",
"Data_Setting09_RemoveBadElectrodeData_List":[],
"Data_Setting10_RemoveLowMeshEffectData_LowerThan_Percent_Readme":"逆推參數-移除過低網格效應觀測資料，單位:[%]。建議值:99。限制填浮點數。注意，全部刪除資料會造成後續異常錯誤。請考量正確的儀器電流情形。",
"Data_Setting10_RemoveLowMeshEffectData_LowerThan_Percent":-999999,
"Data_Setting11_RemoveHighMeshEffectData_HigherThan_Percent_Readme":"逆推參數-移除過高網格效應觀測資料，單位:[%]。建議值:101。限制浮點數。注意，全部刪除資料會造成後續異常錯誤。請考量正確的儀器電流情形。",
"Data_Setting11_RemoveHighMeshEffectData_HigherThan_Percent":999999,
"Data_Setting12_UseFakeData_Enable_Readme":"逆推參數-使用假的觀測資料。建議值:「No」。若要啟用(把觀測資料變成全部都是1[Ohm-m])請填入字串Yes，若不啟用(使用實際資料)請填入字串No。",
"Data_Setting12_UseFakeData_Enable":"No",
"Data_Setting13_UseFakeData_rhoa_Readme":"逆推參數-使用假的觀測資料值(視電阻率值)，單位:[Ohm-m]。建議值:「100」，限制填正數。",
"Data_Setting13_UseFakeData_rhoa":100,
"OutputFile01_GeometricFactor_K_CrossPlot_PNG_Enable_Readme":"幾何因子分析圖。若要啟用請填入字串Yes，若不啟用請填入字串No。不啟用可節省計算時間。",
"OutputFile01_GeometricFactor_K_CrossPlot_PNG_Enable":"Yes",
"Inversion_Setting00_getOptimizeLamda_Enable_Readme":"查找最佳lamda。若要啟用請填入字串Yes，若不啟用請填入字串No。啟用的話只會跑一次迭代，用L曲線法找出第一次迭代的最佳lamda。",
"Inversion_Setting00_getOptimizeLamda_Enable":"No",
"Inversion_Setting00_getOptimizeLamda_StartingLamda_Readme":"查找最佳lamda的起始值。將固定逐點測試0.8",
"Inversion_Setting00_getOptimizeLamda_StartingLamda":10,
"Inversion_Setting01_Remove_Data_Times_Readme":"逆推參數-刪除擬合不佳的觀測資料次數，單位:[次]。建議值3，若不刪除任何資料，填「0」。",
"Inversion_Setting01_Remove_Data_Times":1,
"Inversion_Setting02_Remove_Data_Percentage_Readme":"逆推參數-刪除擬合不佳的觀測資料百分比，單位:[%]。建議值5[%]。限制填入1-99的正整數，請注意刪除過多可能導致程式錯誤。",
"Inversion_Setting02_Remove_Data_Percentage":5,
"Inversion_Setting03_Lamda_Readme":"逆推參數-權重參數Lamda，單位:[無]。建議值20。更相信資料就將lamda減小。lamda調大通常造成模型更加平滑。lamda太小模型將由資料主宰有過擬合的風險；lamda太大模型將過度平滑無法反映細節。",
"Inversion_Setting03_Lamda":[20],
"Inversion_Setting04_maxIter_Readme":"逆推參數-最大迭代次數，單位:[次]。建議值20。",
"Inversion_Setting04_maxIter":20,
"Inversion_Setting05_startModel_Readme":"逆推參數-起始模型，單位:[Ohm-m]。建議值:「-1」，限制填正數或「-1」或「-2」或「-3」。填「-1」表示使用資料的中位數製作均值起始模型。若填正數則用該值製作均值起始模型。填「-2」表示使用全網格MeshVTK內的電阻率值。填「-3」表示使用研究區域網格StartModelVTK內的電阻率值。",
"Inversion_Setting05_startModel":-1,
"InputFile05_StudyAreaStartModelVTK_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"InputFile05_StudyAreaStartModelVTK_FileName":"Input_ERTMaker_Inversion2D/InputFile05_StudyAreaStartModelVTK_FileName.vtk",
"Inversion_Setting06_verbose_Enable_Readme":"逆推參數-詳細運行資訊。若要啟用請填入字串Yes，若不啟用請填入字串No。",
"Inversion_Setting06_verbose_Enable":"Yes",
"Inversion_Setting07_limitModelCellMinValue_Readme":"逆推參數-模型電阻率下限。預設值:「0」，限制填正數或「0」。填「0」表示不限制。",
"Inversion_Setting07_limitModelCellMinValue":0,
"Inversion_Setting08_limitModelCellMaxValue_Readme":"逆推參數-模型電阻率上限。預設值:「0」，限制填正數或「0」。填「0」表示不限制。",
"Inversion_Setting08_limitModelCellMaxValue":0,
"Inversion_Setting09_SR_Enable_Readme":"逆推參數-奇異值移除技術(SR技術)。若要啟用請填入字串Yes，若不啟用請填入字串No。預設值:「Yes」",
"Inversion_Setting09_SR_Enable":"Yes",
"OutputMainFolderPath_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。務必要使用「/」在結尾，表示資料夾。",
"OutputMainFolderPath":"Output_ERTMaker_Inversion2D/",
"Output_MainFileName_Readme":"會產生很多檔案，這些檔案的主要檔名。",
"Output_MainFileName":"XP2",
"Output_Inversion_ColorBarResistivityMin_Readme":"電阻率色階下限，線性數值，必為正數。建議:1。",
"Output_Inversion_ColorBarResistivityMin":50,
"Output_Inversion_ColorBarResistivityMax_Readme":"電阻率色階上限，線性數值，必為正數。建議:10000。",
"Output_Inversion_ColorBarResistivityMax":2000,
"Output_Inversion_XMinMax_Enable_Readme":"繪製研究區域網格-使用自訂X範圍。若要啟用請填入字串Yes，若不啟用請填入字串No。預設值:「No」",
"Output_Inversion_XMinMax_Enable":"No",
"Output_Inversion_XMin_Readme":"繪製研究區域網格-使用X範圍下限。請填入網格範圍內的數字，必須小於XMax。",
"Output_Inversion_XMin":-40,
"Output_Inversion_XMax_Readme":"繪製研究區域網格-使用X範圍上限。請填入網格範圍內的數字，必須大於XMin。",
"Output_Inversion_XMax":1600,
"Output_Inversion_YMin_Readme":"繪製研究區域網格-使用Y範圍下限。請填入網格範圍內的數字，必須小於YMax。",
"Output_Inversion_YMin":-624,
"Output_Inversion_YMax_Readme":"繪製研究區域網格-使用Y範圍上限。請填入網格範圍內的數字，必須大於YMin。",
"Output_Inversion_YMax":64
}
```
