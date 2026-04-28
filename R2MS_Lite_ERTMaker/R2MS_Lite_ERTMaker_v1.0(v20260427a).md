# R2MS_Lite_ERTMaker 開發筆記
+ 作者: HsiupoYeh  
+ 更新日期: 2026-04-27
+ 從 **R2MS_Lite_ERTMaker_v1.0(v20260423a).md** 開始，新增功能。

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
  ...\ERTMaker_Inversion2D_v20260419a.cpython-312.pyc
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

## 開發紀錄(寫功能部分)
### 版本
+ 1.1選Form，去編輯FormCreate，修改部分:
```pascal
procedure TForm1.FormCreate(Sender: TObject);
begin
  //--------------------------------------------------------------------------
  // 在這裡對全域變數進行初始化
  version_str := 'v1.0(v20260427a)';
  Current_Folder_Path := ExtractFilePath(Application.ExeName);
  //--------------------------------------------------------------------------
  //--------------------------------------------------------------------------
  // 表單標題追加版本
  Form1.Caption := Form1.Caption + ' ' + version_str;
  //--------------------------------------------------------------------------
end;      
```
### 功能 
+ 2.1 原始碼最前面部分先宣告修改為如下程式碼。
```pascal
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
  Form1InversionInputMeshPreviewFullMesh_ImageWidthDiffPx: Integer;
  Form1InversionInputMeshPreviewFullMesh_ImageHeightDiffPx: Integer;
  Form1InversionInputMeshPreviewStudyAreaMeshMesh_ImageWidthDiffPx: Integer;
  Form1InversionInputMeshPreviewStudyAreaMeshMesh_ImageHeightDiffPx: Integer;
  Form1InversionInputObsDataPreviewLeft_ImageWidthDiffPx: Integer;
  Form1InversionInputObsDataPreviewLeft_ImageHeightDiffPx: Integer;
  Form1InversionInputObsDataPreviewRight_ImageWidthDiffPx: Integer;
  Form1InversionInputObsDataPreviewRight_ImageHeightDiffPx: Integer;
  Form1InversionResultPreviewImage_ImageWidthDiffPx: Integer;
  Form1InversionResultPreviewImage_ImageHeightDiffPx: Integer;
  Form1InversionAdvancedAnalysisInfoImage_ImageWidthDiffPx: Integer;
  Form1InversionAdvancedAnalysisInfoImage_ImageHeightDiffPx: Integer;
//--------------------------------------------------------------------------

{$R *.lfm}
```
+ 2.2
```pascal
procedure TForm1.FormShow(Sender: TObject);
begin
  // 使焦點落在一個沒有用處的東西上面，避免使用者鍵盤操作發生不預期的影響。
  EmptyPanel.SetFocus;
  //--------------------------------------------------------------------------
  // 計算經過DPI縮放後的圖片與主視窗的寬高差，用於修補TImage在不可見的時候會自行改變尺寸的bug。修補方式:手動限制尺寸。
  Form1CreateMeshPreview_ImageWidthDiffPx:=Form1.Width-CreateMeshPreview_Image.Width;
  Form1CreateMeshPreview_ImageHeightDiffPx:=Form1.Height-CreateMeshPreview_Image.Height;
  //--------------------------------------------------------------------------
  //--------------------------------------------------------------------------
  // 計算經過DPI縮放後的圖片與主視窗的寬高差，用於修補TImage在不可見的時候會自行改變尺寸的bug。修補方式:手動限制尺寸。
  // 只能用一開始就有顯示的元件尺寸
  Form1ForwardModelingPreview_ImageWidthDiffPx:=Form1CreateMeshPreview_ImageWidthDiffPx;
  Form1ForwardModelingPreview_ImageHeightDiffPx:=Form1CreateMeshPreview_ImageHeightDiffPx;
  //--------------------------------------------------------------------------
  //--------------------------------------------------------------------------
  // 計算經過DPI縮放後的圖片與主視窗的寬高差，用於修補TImage在不可見的時候會自行改變尺寸的bug。修補方式:手動限制尺寸。
  // 只能用一開始就有顯示的元件尺寸
  Form1InversionInputMeshPreviewFullMesh_ImageWidthDiffPx:=Form1.Width-InversionInputMeshPreviewFullMesh_Image.Width;
  Form1InversionInputMeshPreviewFullMesh_ImageHeightDiffPx:=Form1.Height-InversionInputMeshPreviewFullMesh_Image.Height;
  //--------------------------------------------------------------------------
  //--------------------------------------------------------------------------
  // 計算經過DPI縮放後的圖片與主視窗的寬高差，用於修補TImage在不可見的時候會自行改變尺寸的bug。修補方式:手動限制尺寸。
  // 只能用一開始就有顯示的元件尺寸
  Form1InversionInputMeshPreviewStudyAreaMeshMesh_ImageWidthDiffPx:=Form1.Width-InversionInputMeshPreviewStudyAreaMeshMesh_Image.Width;
  Form1InversionInputMeshPreviewStudyAreaMeshMesh_ImageHeightDiffPx:=Form1.Height-InversionInputMeshPreviewStudyAreaMeshMesh_Image.Height;
  //--------------------------------------------------------------------------
  //--------------------------------------------------------------------------
  // 計算經過DPI縮放後的圖片與主視窗的寬高差，用於修補TImage在不可見的時候會自行改變尺寸的bug。修補方式:手動限制尺寸。
  // 只能用一開始就有顯示的元件尺寸
  Form1InversionInputObsDataPreviewLeft_ImageWidthDiffPx:=Form1.Width-InversionInputObsDataPreviewLeft_Image.Width;
  Form1InversionInputObsDataPreviewLeft_ImageHeightDiffPx:=Form1.Height-InversionInputObsDataPreviewLeft_Image.Height;
  //--------------------------------------------------------------------------
  //--------------------------------------------------------------------------
  // 計算經過DPI縮放後的圖片與主視窗的寬高差，用於修補TImage在不可見的時候會自行改變尺寸的bug。修補方式:手動限制尺寸。
  // 只能用一開始就有顯示的元件尺寸
  Form1InversionInputObsDataPreviewRight_ImageWidthDiffPx:=Form1.Width-InversionInputObsDataPreviewRight_Image.Width;
  Form1InversionInputObsDataPreviewRight_ImageHeightDiffPx:=Form1.Height-InversionInputObsDataPreviewRight_Image.Height;
  //--------------------------------------------------------------------------
  //--------------------------------------------------------------------------
  // 計算經過DPI縮放後的圖片與主視窗的寬高差，用於修補TImage在不可見的時候會自行改變尺寸的bug。修補方式:手動限制尺寸。
  // 只能用一開始就有顯示的元件尺寸
  Form1InversionResultPreviewImage_ImageWidthDiffPx:=Form1.Width-InversionResultPreviewImage_Image.Width;
  Form1InversionResultPreviewImage_ImageHeightDiffPx:=Form1.Height-InversionResultPreviewImage_Image.Height;
  //--------------------------------------------------------------------------
  //--------------------------------------------------------------------------
  // 計算經過DPI縮放後的圖片與主視窗的寬高差，用於修補TImage在不可見的時候會自行改變尺寸的bug。修補方式:手動限制尺寸。
  // 只能用一開始就有顯示的元件尺寸
  Form1InversionAdvancedAnalysisInfoImage_ImageWidthDiffPx:=Form1.Width-InversionAdvancedAnalysisInfoImage_Image.Width;
  Form1InversionAdvancedAnalysisInfoImage_ImageHeightDiffPx:=Form1.Height-InversionAdvancedAnalysisInfoImage_Image.Height;
  //--------------------------------------------------------------------------
end; 
```
+ 2.3
```pascal
procedure TForm1.FormResize(Sender: TObject);
begin
  //--------------------------------------------------------------------------
  // 限制圖片尺寸，來修補圖片尺寸異常的Bug
  if Forward_PageControl.ActivePage = CreateMesh_TabSheet then
  begin
    CreateMeshPreview_Image.Constraints.MaxWidth:=Form1.Width-Form1CreateMeshPreview_ImageWidthDiffPx;
    CreateMeshPreview_Image.Constraints.MaxHeight:=Form1.Height-Form1CreateMeshPreview_ImageHeightDiffPx;
    ForwardModelingPreview_Image.Constraints.MaxWidth:=Form1.Width-Form1ForwardModelingPreview_ImageWidthDiffPx;
    ForwardModelingPreview_Image.Constraints.MaxHeight:=Form1.Height-Form1ForwardModelingPreview_ImageHeightDiffPx;
  end;
  //--------------------------------------------------------------------------
  //--------------------------------------------------------------------------
  // 限制圖片尺寸，來修補圖片尺寸異常的Bug
  if Forward_PageControl.ActivePage = ForwardModeling_TabSheet then
  begin
    CreateMeshPreview_Image.Constraints.MaxWidth:=Form1.Width-Form1CreateMeshPreview_ImageWidthDiffPx;
    CreateMeshPreview_Image.Constraints.MaxHeight:=Form1.Height-Form1CreateMeshPreview_ImageHeightDiffPx;
    ForwardModelingPreview_Image.Constraints.MaxWidth:=Form1.Width-Form1ForwardModelingPreview_ImageWidthDiffPx;
    ForwardModelingPreview_Image.Constraints.MaxHeight:=Form1.Height-Form1ForwardModelingPreview_ImageHeightDiffPx;
  end;
  //--------------------------------------------------------------------------
  //--------------------------------------------------------------------------
  // 限制圖片尺寸，來修補圖片尺寸異常的Bug
  if Main_PageControl.ActivePage = Inversion_TabSheet then
  begin
    //--
    InversionAdvancedAnalysisInfoImage_Image.Constraints.MaxWidth:=Form1.Width-Form1InversionAdvancedAnalysisInfoImage_ImageWidthDiffPx;
    InversionAdvancedAnalysisInfoImage_Image.Constraints.MaxHeight:=Form1.Height-Form1InversionAdvancedAnalysisInfoImage_ImageHeightDiffPx;
    //--
    InversionInputObsDataPreviewLeft_Image.Constraints.MaxWidth:=Form1.Width-Form1InversionInputObsDataPreviewLeft_ImageWidthDiffPx;
    InversionInputObsDataPreviewLeft_Image.Constraints.MaxHeight:=Form1.Height-Form1InversionInputObsDataPreviewLeft_ImageHeightDiffPx;
    //--
    InversionInputObsDataPreviewRight_Image.Constraints.MaxWidth:=Form1.Width-Form1InversionInputObsDataPreviewRight_ImageWidthDiffPx;
    InversionInputObsDataPreviewRight_Image.Constraints.MaxHeight:=Form1.Height-Form1InversionInputObsDataPreviewRight_ImageHeightDiffPx;
    //--
    if InversionInputMeshPreviewBoth_ToolButton.Down then
    begin
      InversionInputMeshPreviewFullMesh_Image.Constraints.MaxWidth:=InversionInputObsDataPreviewLeft_Image.Constraints.MaxWidth;
      InversionInputMeshPreviewFullMesh_Image.Constraints.MaxHeight:=InversionInputObsDataPreviewLeft_Image.Constraints.MaxHeight;
      InversionInputMeshPreviewStudyAreaMeshMesh_Image.Constraints.MaxWidth:=InversionInputObsDataPreviewRight_Image.Constraints.MaxWidth;
      InversionInputMeshPreviewStudyAreaMeshMesh_Image.Constraints.MaxHeight:=InversionInputObsDataPreviewRight_Image.Constraints.MaxHeight;
    end;
    if InversionInputMeshPreviewFullMesh_ToolButton.Down then
    begin
      InversionInputMeshPreviewFullMesh_Image.Constraints.MaxWidth:=InversionAdvancedAnalysisInfoImage_Image.Constraints.MaxWidth;
      InversionInputMeshPreviewFullMesh_Image.Constraints.MaxHeight:=InversionAdvancedAnalysisInfoImage_Image.Constraints.MaxHeight;
      InversionInputMeshPreviewStudyAreaMeshMesh_Image.Constraints.MaxWidth:=InversionAdvancedAnalysisInfoImage_Image.Constraints.MaxWidth;
      InversionInputMeshPreviewStudyAreaMeshMesh_Image.Constraints.MaxHeight:=InversionAdvancedAnalysisInfoImage_Image.Constraints.MaxHeight;
    end;
    if InversionInputMeshPreviewStudyAreaMeshMesh_ToolButton.Down then
    begin
      InversionInputMeshPreviewFullMesh_Image.Constraints.MaxWidth:=InversionAdvancedAnalysisInfoImage_Image.Constraints.MaxWidth;
      InversionInputMeshPreviewFullMesh_Image.Constraints.MaxHeight:=InversionAdvancedAnalysisInfoImage_Image.Constraints.MaxHeight;
      InversionInputMeshPreviewStudyAreaMeshMesh_Image.Constraints.MaxWidth:=InversionAdvancedAnalysisInfoImage_Image.Constraints.MaxWidth;
      InversionInputMeshPreviewStudyAreaMeshMesh_Image.Constraints.MaxHeight:=InversionAdvancedAnalysisInfoImage_Image.Constraints.MaxHeight;
    end;
    //--
    if InversionResultPreviewShowImage_ToolButton.Down then
    begin
      InversionResultPreviewImage_Image.Constraints.MaxWidth:=InversionAdvancedAnalysisInfoImage_Image.Constraints.MaxWidth;
      InversionResultPreviewImage_Image.Constraints.MaxHeight:=InversionAdvancedAnalysisInfoImage_Image.Constraints.MaxHeight;
    end;
    if InversionResultPreviewShowGrid_ToolButton.Down then
    begin
      InversionResultPreviewImage_Image.Constraints.MaxWidth:=InversionAdvancedAnalysisInfoImage_Image.Constraints.MaxWidth;
      InversionResultPreviewImage_Image.Constraints.MaxHeight:=InversionAdvancedAnalysisInfoImage_Image.Constraints.MaxHeight;
    end;
    if InversionResultPreviewShowBoth_ToolButton.Down then
    begin
      InversionResultPreviewImage_Image.Constraints.MaxWidth:=InversionInputObsDataPreviewLeft_Image.Constraints.MaxWidth;
      InversionResultPreviewImage_Image.Constraints.MaxHeight:=InversionInputObsDataPreviewLeft_Image.Constraints.MaxHeight;
    end;
    //--
  end;
  //--------------------------------------------------------------------------
end;
```
+2.4
```
procedure TForm1.InversionInputMeshPreviewFullMesh_ToolButtonClick(
  Sender: TObject);
begin
  InversionInputMeshPreviewFullMesh_ToolButton.Down:=True;
  InversionInputMeshPreviewFullMesh_GroupBox.Visible:=True;
  InversionInputMeshPreviewFullMesh_GroupBox.AnchorSideRight.Control := InversionInputMeshPreview_TabSheet;
  InversionInputMeshPreviewFullMesh_GroupBox.AnchorSideRight.Side := asrRight;
  InversionInputMeshPreviewStudyAreaMeshMesh_ToolButton.Down:=False;
  InversionInputMeshPreviewStudyAreaMeshMesh_GroupBox.Visible:=False;
  InversionInputMeshPreviewBoth_ToolButton.Down:=False;
  Form1.FormResize(Form1);
end;
```
+ 2.5
```
procedure TForm1.InversionInputMeshPreviewStudyAreaMeshMesh_ToolButtonClick(
  Sender: TObject);
begin
  InversionInputMeshPreviewFullMesh_ToolButton.Down:=False;
  InversionInputMeshPreviewFullMesh_GroupBox.Visible:=False;
  InversionInputMeshPreviewStudyAreaMeshMesh_ToolButton.Down:=True;
  InversionInputMeshPreviewStudyAreaMeshMesh_GroupBox.Visible:=True;
  InversionInputMeshPreviewStudyAreaMeshMesh_GroupBox.AnchorSideLeft.Control := InversionInputMeshPreview_TabSheet;
  InversionInputMeshPreviewStudyAreaMeshMesh_GroupBox.AnchorSideLeft.Side := asrLeft;
  InversionInputMeshPreviewBoth_ToolButton.Down:=False;
  Form1.FormResize(Form1);
end;
```
+2.6
```
procedure TForm1.InversionInputMeshPreviewBoth_ToolButtonClick(Sender: TObject);
begin
  InversionInputMeshPreviewFullMesh_ToolButton.Down:=False;
  InversionInputMeshPreviewFullMesh_GroupBox.Visible:=True;
  InversionInputMeshPreviewFullMesh_GroupBox.AnchorSideRight.Control := InversionInputMeshPreview_Panel;
  InversionInputMeshPreviewFullMesh_GroupBox.AnchorSideRight.Side := asrLeft;
  InversionInputMeshPreviewStudyAreaMeshMesh_ToolButton.Down:=False;
  InversionInputMeshPreviewStudyAreaMeshMesh_GroupBox.Visible:=True;
  InversionInputMeshPreviewStudyAreaMeshMesh_GroupBox.AnchorSideLeft.Control := InversionInputMeshPreview_Panel;
  InversionInputMeshPreviewStudyAreaMeshMesh_GroupBox.AnchorSideLeft.Side := asrRight;
  InversionInputMeshPreviewBoth_ToolButton.Down:=True;
  Form1.FormResize(Form1);
end;  
```
+ 2.7
```
procedure TForm1.InversionResultPreviewShowImage_ToolButtonClick(Sender: TObject
  );
begin
  InversionResultPreviewShowImage_ToolButton.Down:=True;
  InversionResultPreviewImage_GroupBox.Visible:=True;
  InversionResultPreviewImage_GroupBox.AnchorSideRight.Control := InversionResultPreview_TabSheet;
  InversionResultPreviewImage_GroupBox.AnchorSideRight.Side := asrRight;
  InversionResultPreviewShowGrid_ToolButton.Down:=False;
  InversionResultPreviewGrid_GroupBox.Visible:=False;
  InversionResultPreviewShowBoth_ToolButton.Down:=False;
  Form1.FormResize(Form1);
end;
```
+ 2.8
```
procedure TForm1.InversionResultPreviewShowGrid_ToolButtonClick(Sender: TObject
  );
begin
  InversionResultPreviewShowImage_ToolButton.Down:=False;
  InversionResultPreviewImage_GroupBox.Visible:=False;
  InversionResultPreviewShowGrid_ToolButton.Down:=True;
  InversionResultPreviewGrid_GroupBox.Visible:=True;
  InversionResultPreviewGrid_GroupBox.AnchorSideLeft.Control := InversionResultPreview_TabSheet;
  InversionResultPreviewGrid_GroupBox.AnchorSideLeft.Side := asrLeft;
  InversionResultPreviewShowBoth_ToolButton.Down:=False;
  Form1.FormResize(Form1);
end;
```
+ 2.9
```
procedure TForm1.InversionResultPreviewShowBoth_ToolButtonClick(Sender: TObject
  );
begin
  InversionResultPreviewShowImage_ToolButton.Down:=False;
  InversionResultPreviewImage_GroupBox.Visible:=True;
  InversionResultPreviewImage_GroupBox.AnchorSideRight.Control := InversionResultPreview_Panel;
  InversionResultPreviewImage_GroupBox.AnchorSideRight.Side := asrLeft;
  InversionResultPreviewShowGrid_ToolButton.Down:=False;
  InversionResultPreviewGrid_GroupBox.Visible:=True;
  InversionResultPreviewGrid_GroupBox.AnchorSideLeft.Control := InversionResultPreview_Panel;
  InversionResultPreviewGrid_GroupBox.AnchorSideLeft.Side := asrRight;
  InversionResultPreviewShowBoth_ToolButton.Down:=True;
  Form1.FormResize(Form1);
end;
```
  
### 更新紀錄
+ 3.1 選「UpdateLog_Memo」，去編輯「Lines」
```
軟體名稱: R2MS_Lite_ERTMaker
作者: HsiupoYeh
--
v20260427a
修正不同DPI圖片縮放錯誤的問題。
--
v20260423a
完整功能版 (包含順推模擬:建立模型網格、模擬時間序列蒐集、時間序列資料處理以及逆推模擬)
**建議作業系統: Windows 10
**ERTMaker主版本號: v1.0
**ERTMaker圖形化介面版本號: v20260423a
**Python版本: 3.12.7 (64-bit)
**PyGimli版本: v1.5.4
***已知問題: 逆推運算的平行運算預設不使用所有核心運作(會保留兩個核心)。但在Intel第12代的CPU(i7-1260P)環境下設定失效。這可能會使作業系統極度忙碌，導致網路等功能無回應。這並非軟體設計問題，而是這版本的CPU調度邏輯不良。
***支援快捷鍵: 「Ctrl+方向鍵」來快速瀏覽歷次逆推結果。
--
v20251105a
初版 (僅設計排版)
```
