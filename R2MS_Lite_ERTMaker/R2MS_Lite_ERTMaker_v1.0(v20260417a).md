# R2MS_Lite_ERTMaker 開發筆記
+ 作者: HsiupoYeh  
+ 更新日期: 2026-04-17
+ 從 **R2MS_Lite_ERTMaker_v1.0(v20260410a).md** 開始，修正錯誤。

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
  ...\ERTMaker_Inversion2D_v20260401a.cpython-312.pyc
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
  + 1.1 第3個分頁「輸入網格預覽」(InversionInputMeshPreview_TabSheet):
    + **1.1.1 拖拉一個「Standard>TToolBar」到「InversionInputMeshPreview_TabSheet」中。預設名稱會是「ToolBar1」，修改「Name」為「InversionInputMeshPreview_ToolBar」。**
      + **1.1.1.1 設定「BorderSpacing>Top」為「1」。**
      + **1.1.1.2 設定「Edge>Borders>ebBottom」為「True」。**
      + **1.1.1.3 設定「Edge>Borders>ebLeft」為「True」。**
      + **1.1.1.4 設定「Edge>Borders>ebRight」為「True」。**
      + **1.1.1.5 設定「ShowCaptions」為「True」。**
    + **1.1.2 在「InversionInputMeshPreview_ToolBar」元件上按「右鍵>New CheckButton」，會增加一個新按鈕。預設名稱會是「ToolButton1」，修改「Name」為「InversionInputMeshPreviewFullMesh_ToolButton」**
      + **1.1.2.1 設定「Caption」為「全區域網格」。**
    + **1.1.3 在「InversionInputMeshPreview_ToolBar」元件上按「右鍵>New CheckButton」，會增加一個新按鈕。預設名稱會是「ToolButton1」，修改「Name」為「InversionInputMeshPreviewStudyAreaMeshMesh_ToolButton」**
      + **1.1.3.1 設定「Caption」為「研究區域網格」。**
      + **1.1.3.2 設定「Down」為「True」。**
    + **1.1.4 拖拉一個「Standard>TGroupBox」到「InversionInputMeshPreview_TabSheet」中。預設名稱會是「GroupBox1」，修改「Name」為「InversionInputMeshPreviewFullMesh_GroupBox」。**
      + **1.1.4.1 設定「Anchors」。**  
        + **1.1.4.1.1 「Top anchoring>Slbling」選為「InversionInputMeshPreview_ToolBar:TToolBar」。維持「Top anchoring>Enable」為打勾。選擇「Top anchoring」右側三個鈕選下面的。** 
        + **1.1.4.1.2 「Left anchoring>Slbling」選為「InversionInputMeshPreview_TabSheet:TTabSheet」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
        + **1.1.4.1.3 「Right anchoring>Slbling」選為「InversionInputMeshPreview_TabSheet:TTabSheet」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
        + **1.1.4.1.4 「Bottom anchoring>Slbling」選為「InversionInputMeshPreview_TabSheet:TTabSheet」。勾選「Bottom anchoring>Enable」為打勾。選擇「Bottom anchoring」右側三個鈕選下面的。** 
        + **1.1.4.1.5 設定完後關閉Anchors設定視窗。**  
      + **1.1.4.2  設定「Caption」為「全區域網格預覽:」。**
    + **1.1.5 拖拉一個「Additional>TImage」到「InversionInputMeshPreviewFullMesh_GroupBox」中。預設名稱會是「Image1」，修改「Name」為「InversionInputMeshPreviewFullMesh_Image」。**
      + **1.1.5.1 設定「Align」為「alClient」。**
      + **1.1.5.2 設定「AntialiasingMode」為「amOn」。**
      + **1.1.5.3 設定「Center」為「True」。**
      + **1.1.5.4 設定「Proportional」為「True」。**
      + **1.1.5.5 設定「Streth」為「True」。**
    + **1.1.6 拖拉一個「Standard>TGroupBox」到「InversionInputMeshPreview_TabSheet」中。預設名稱會是「GroupBox1」，修改「Name」為「InversionInputMeshPreviewStudyAreaMeshMesh_GroupBox」。**
      + **1.1.6.1 設定「Anchors」。**  
        + **1.1.6.1.1 「Top anchoring>Slbling」選為「InversionInputMeshPreview_ToolBar:TToolBar」。維持「Top anchoring>Enable」為打勾。選擇「Top anchoring」右側三個鈕選下面的。** 
        + **1.1.6.1.2 「Left anchoring>Slbling」選為「InversionInputMeshPreview_TabSheet:TTabSheet」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
        + **1.1.6.1.3 「Right anchoring>Slbling」選為「InversionInputMeshPreview_TabSheet:TTabSheet」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
        + **1.1.6.1.4 「Bottom anchoring>Slbling」選為「InversionInputMeshPreview_TabSheet:TTabSheet」。勾選「Bottom anchoring>Enable」為打勾。選擇「Bottom anchoring」右側三個鈕選下面的。** 
        + **1.1.6.1.5 設定完後關閉Anchors設定視窗。**  
      + **1.1.6.2  設定「Caption」為「研究區域網格預覽:」。**
    + **1.1.7 拖拉一個「Additional>TImage」到「InversionInputMeshPreviewStudyAreaMeshMesh_GroupBox」中。預設名稱會是「Image1」，修改「Name」為「InversionInputMeshPreviewStudyAreaMeshMesh_Image」。**
      + **1.1.7.1 設定「Align」為「alClient」。**
      + **1.1.7.2 設定「AntialiasingMode」為「amOn」。**
      + **1.1.7.3 設定「Center」為「True」。**
      + **1.1.7.4 設定「Proportional」為「True」。**
      + **1.1.7.5 設定「Streth」為「True」。**
    + **1.1.8 拖拉一個「System>TTimer」到「Form1」中。預設名稱會是「Timer1」，修改「Name」為「InversionModeling_Timer」。**
      + **1.1.8.1 設定「Enabled」為「False」。** 
