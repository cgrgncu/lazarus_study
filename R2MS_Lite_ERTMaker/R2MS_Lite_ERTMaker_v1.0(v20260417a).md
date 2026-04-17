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
      + **1.1.5.5 設定「Stretch」為「True」。**
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
      + **1.1.7.5 設定「Stretch」為「True」。**
    + **1.1.8 拖拉一個「System>TTimer」到「Form1」中。預設名稱會是「Timer1」，修改「Name」為「InversionModeling_Timer」。**
      + **1.1.8.1 設定「Enabled」為「False」。** 
  + 1.2 第4個分頁「觀測資料預覽」(InversionInputObsDataPreview_TabSheet):
    + **1.2.1 拖拉一個「Standard>TToolBar」到「InversionInputObsDataPreview_TabSheet」中。預設名稱會是「ToolBar1」，修改「Name」為「InversionInputObsDataPreview_ToolBar」。**
      + **1.2.1.1 設定「BorderSpacing>Top」為「1」。**
      + **1.2.1.2 設定「Edge>Borders>ebBottom」為「True」。**
      + **1.2.1.3 設定「Edge>Borders>ebLeft」為「True」。**
      + **1.2.1.4 設定「Edge>Borders>ebRight」為「True」。**
      + **1.2.1.5 設定「ShowCaptions」為「True」。**
    + **1.2.2 在「InversionInputObsDataPreview_ToolBar」元件上按「右鍵>New Button」，會增加一個新按鈕。預設名稱會是「ToolButton1」，修改「Name」為「InversionInputObsDataPreviewLeft_ToolButton」**
      + **1.2.2.1 設定「Caption」為「所有的觀測資料」。**
      + **1.2.2.1 設定「Style」為「tbsButtonDrop」。**
    + **1.2.3 在「InversionInputObsDataPreview_ToolBar」元件上按「右鍵>New Divider」，會增加一個新按鈕。預設名稱會是「ToolButton1」，修改「Name」為「InversionInputObsDataPreviewDivider1_ToolButton」**
    + **1.2.4 在「InversionInputObsDataPreview_ToolBar」元件上按「右鍵>New Button」，會增加一個新按鈕。預設名稱會是「ToolButton1」，修改「Name」為「InversionInputObsDataPreviewRight_ToolButton」**
      + **1.2.4.1 設定「Caption」為「採用的觀測資料」。**
      + **1.2.4.1 設定「Style」為「tbsButtonDrop」。**
    + **1.2.5 拖拉一個「Standard>TPanel」到「InversionInputObsDataPreview_TabSheet」中。預設名稱會是「Panel1」，修改「Name」為「InversionInputObsDataPreview_Panel」。**
      + **1.2.5.1 設定「Anchors」。**  
        + **1.2.5.1.1 「Top anchoring>Slbling」選為「InversionInputObsDataPreview_ToolBar:TToolBar」。維持「Top anchoring>Enable」為打勾。選擇「Top anchoring」右側三個鈕選下面的。** 
        + **1.2.5.1.2 「Left anchoring>Slbling」選為「InversionInputObsDataPreview_TabSheet:TTabSheet」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選中邊的。**  
        + **1.2.5.1.3 「Bottom anchoring>Slbling」選為「InversionInputObsDataPreview_TabSheet:TTabSheet」。勾選「Bottom anchoring>Enable」為打勾。選擇「Bottom anchoring」右側三個鈕選下面的。** 
        + **1.2.5.1.4 設定完後關閉Anchors設定視窗。**  
      + **1.2.5.2  設定「Caption」為空白。**
      + **1.2.5.3  設定「Width」為「0」。**
    + **1.2.6 拖拉一個「Standard>TGroupBox」到「InversionInputObsDataPreview_TabSheet」中。預設名稱會是「GroupBox1」，修改「Name」為「InversionInputObsDataPreviewLeft_GroupBox」。**
      + **1.2.6.1 設定「Anchors」。**  
        + **1.2.6.1.1 「Top anchoring>Slbling」選為「InversionInputObsDataPreview_ToolBar:TToolBar」。維持「Top anchoring>Enable」為打勾。選擇「Top anchoring」右側三個鈕選下面的。** 
        + **1.2.6.1.2 「Left anchoring>Slbling」選為「InversionInputObsDataPreview_TabSheet:TTabSheet」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
        + **1.2.6.1.3 「Right anchoring>Slbling」選為「InversionInputObsDataPreview_Panel:TPanel」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選左邊的。**  
        + **1.2.6.1.4 「Bottom anchoring>Slbling」選為「InversionInputObsDataPreview_TabSheet:TTabSheet」。勾選「Bottom anchoring>Enable」為打勾。選擇「Bottom anchoring」右側三個鈕選下面的。**
        + **1.2.6.1.5 「Border space」正中間那格填「0」，左邊那格填「0」，右邊那格填「3」。**  
        + **1.2.6.1.6 設定完後關閉Anchors設定視窗。**  
      + **1.2.6.2  設定「Caption」為「所有的觀測資料:」。**
    + **1.2.7 拖拉一個「Additional>TImage」到「InversionInputObsDataPreviewLeft_GroupBox」中。預設名稱會是「Image1」，修改「Name」為「InversionInputObsDataPreviewLeft_Image」。**
      + **1.2.7.1 設定「Align」為「alClient」。**
      + **1.2.7.2 設定「AntialiasingMode」為「amOn」。**
      + **1.2.7.3 設定「Center」為「True」。**
      + **1.2.7.4 設定「Proportional」為「True」。**
      + **1.2.7.5 設定「Stretch」為「True」。**
    + **1.2.8 拖拉一個「Standard>TGroupBox」到「InversionInputObsDataPreview_TabSheet」中。預設名稱會是「GroupBox1」，修改「Name」為「InversionInputObsDataPreviewRight_GroupBox」。**
      + **1.2.8.1 設定「Anchors」。**  
        + **1.2.8.1.1 「Top anchoring>Slbling」選為「InversionInputObsDataPreview_ToolBar:TToolBar」。維持「Top anchoring>Enable」為打勾。選擇「Top anchoring」右側三個鈕選下面的。** 
        + **1.2.8.1.2 「Left anchoring>Slbling」選為「InversionInputObsDataPreview_Panel:TPanel」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選右邊的。**  
        + **1.2.8.1.3 「Right anchoring>Slbling」選為「InversionInputObsDataPreview_TabSheet:TTabSheet」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
        + **1.2.8.1.4 「Bottom anchoring>Slbling」選為「InversionInputObsDataPreview_TabSheet:TTabSheet」。勾選「Bottom anchoring>Enable」為打勾。選擇「Bottom anchoring」右側三個鈕選下面的。**
        + **1.2.8.1.5 「Border space」正中間那格填「0」，左邊那格填「0」，右邊那格填「3」。**  
        + **1.2.8.1.6 設定完後關閉Anchors設定視窗。**
      + **1.2.8.2  設定「Caption」為「採用的觀測資料:」。**
    + **1.2.9 拖拉一個「Additional>TImage」到「InversionInputObsDataPreviewRight_GroupBox」中。預設名稱會是「Image1」，修改「Name」為「InversionInputObsDataPreviewRight_Image」。**
      + **1.2.9.1 設定「Align」為「alClient」。**
      + **1.2.9.2 設定「AntialiasingMode」為「amOn」。**
      + **1.2.9.3 設定「Center」為「True」。**
      + **1.2.9.4 設定「Proportional」為「True」。**
      + **1.2.9.5 設定「Stretch」為「True」。**
## 開發紀錄(寫功能部分)
### 版本
+ 2.1選Form，去編輯FormCreate，修改部分:
```pascal
procedure TForm1.FormCreate(Sender: TObject);
begin
  //--------------------------------------------------------------------------
  // 在這裡對全域變數進行初始化
  version_str := 'v1.0(v20260417a)';
  Current_Folder_Path := ExtractFilePath(Application.ExeName);
  //--------------------------------------------------------------------------
  //--------------------------------------------------------------------------
  // 表單標題追加版本
  Form1.Caption := Form1.Caption + ' ' + version_str;
  //--------------------------------------------------------------------------
end;      
```
### 功能 
+ 2.2 去修改「InversionInputMeshPreviewFullMesh_ToolButton」的「Event」頁面下「OnClick」為如下程式碼。
```pascal
procedure TForm1.InversionInputMeshPreviewFullMesh_ToolButtonClick(
  Sender: TObject);
begin
  InversionInputMeshPreviewFullMesh_ToolButton.Down:=True;
  InversionInputMeshPreviewFullMesh_GroupBox.Visible:=True;
  InversionInputMeshPreviewStudyAreaMeshMesh_ToolButton.Down:=False;
  InversionInputMeshPreviewStudyAreaMeshMesh_GroupBox.Visible:=False;
end;     
```
+ 2.3 去修改「InversionInputMeshPreviewStudyAreaMeshMesh_ToolButton」的「Event」頁面下「OnClick」為如下程式碼。
```pascal
procedure TForm1.InversionInputMeshPreviewStudyAreaMeshMesh_ToolButtonClick(
  Sender: TObject);
begin
  InversionInputMeshPreviewFullMesh_ToolButton.Down:=False;
  InversionInputMeshPreviewFullMesh_GroupBox.Visible:=False;
  InversionInputMeshPreviewStudyAreaMeshMesh_ToolButton.Down:=True;
  InversionInputMeshPreviewStudyAreaMeshMesh_GroupBox.Visible:=True;
end;    
```
+ 2.4 去修改「InversionModeling_Timer」的「Event」頁面下「OnClick」為如下程式碼。
```pascal
procedure TForm1.InversionModeling_TimerTimer(Sender: TObject);
var
  temp_str: AnsiString;
begin
  if StrToInt(StatusBar1.Panels[1].Text) = 1 then
  begin
    //--------------------------------------------------------------------------
    // 載入圖片
    if FileExists('Output_ERTMaker_Inversion2D/'+InversionModelingOutputSettings_MainName_Edit.Text+'_01_StudyAreaMesh.png') then
    begin
      try
        InversionInputMeshPreviewFullMesh_Image.Picture.LoadFromFile('Output_ERTMaker_Inversion2D/'+InversionModelingOutputSettings_MainName_Edit.Text+'_01_InputFullMesh.png');
        InversionInputMeshPreviewStudyAreaMeshMesh_Image.Picture.LoadFromFile('Output_ERTMaker_Inversion2D/'+InversionModelingOutputSettings_MainName_Edit.Text+'_01_StudyAreaMesh.png');
        Inversion_PageControl.ActivePage:=InversionInputMeshPreview_TabSheet;
        InversionModeling_Timer.Enabled:=False;
        StatusBar1.Panels[4].Text:='停止自動更新資訊';
        StatusBar1.Panels[1].Text:='1';
        Exit;
      except
        StatusBar1.Panels[0].Text:='載入失敗: '+InversionModelingOutputSettings_MainName_Edit.Text+'_01_InputFullMesh.png';
      end;
    end;
    //--------------------------------------------------------------------------
  end;
end; 
```
+ 2.5 去修改「InversionModelingRun_ToolButton」的「Event」頁面下「OnClick」為如下程式碼。
```pascal
procedure TForm1.InversionModelingRun_ToolButtonClick(Sender: TObject);
var
  temp_str: AnsiString;
  temp_str1: AnsiString;
  temp_str2: AnsiString;
  temp_str3: AnsiString;
  temp_str4: AnsiString;
  RegexObj: TRegExpr;
begin
  //--------------------------------------------------------------------------
  InversionModelingSettingsCmdLog_Memo.Clear;
  //--
  StatusBar1.Panels[0].Text:='運行逆推模擬...請稍後!';
  StatusBar1.Panels[1].Text:='';
  StatusBar1.Panels[2].Text:='';
  StatusBar1.Panels[3].Text:='';
  StatusBar1.Panels[4].Text:='';
  //--
  if DirectoryExists('Output_ERTMaker_Inversion2D') then
  begin
    // 記得在 uses 區塊中加入 FileUtil
    DeleteDirectory('Output_ERTMaker_Inversion2D', True);// 第二個參數 True 表示如果資料夾裡面有檔案也一併刪除
  end;
  //--
  InversionModelingSettingsNowJson_Memo.Lines:=InversionModelingSettingsDefaultJson_Memo.Lines;
  //--------------------------------------------------------------------------
  temp_str:=InversionModelingInput01Ohm_Edit.Text;
  if FileExists(temp_str) then
  begin
    ForceDirectories('Output_ERTMaker_Inversion2D\Input_Inversion2D\');
    CopyFileW(PWideChar(UTF8ToUTF16(temp_str)),
          PWideChar(UTF8ToUTF16('Output_ERTMaker_Inversion2D\Input_Inversion2D\'+ExtractFileName(temp_str))),
          False);// 第三個參數為 False 表示如果目標已存在則覆蓋 (Overwrite)
    temp_str := 'Output_ERTMaker_Inversion2D\Input_Inversion2D\'+ExtractFileName(temp_str);
    temp_str:=StringReplace(temp_str, '\', '/', [rfReplaceAll]);
    InversionModelingSettingsNowJson_Memo.Lines.Strings[5-1] := ('"InputFile01_Ohm_FileName":"'+temp_str+'",');
  end
  else
  begin
    temp_str := '錯誤:' + #13#10 +
      'ohm/dat檔案不存在。';
    Application.MessageBox(PChar(temp_str), '錯誤', 16);
    Exit;
  end;
  //--------------------------------------------------------------------------
  RegexObj := TRegExpr.Create;
  try
    //--------------------------------------------------------------------------
    if InversionModelingInvAutoMesh_Enable_CheckBox.Checked then
    begin
      //--
      InversionModelingSettingsNowJson_Memo.Lines.Strings[7-1] := ('"Mesh_Setting01_AutoMesh_Enable":"Yes",');
      //--
      InversionModelingSettingsNowJson_Memo.Lines.Strings[9-1] := ('"Mesh_Setting02_AutoMesh_quality":'+InversionModelingInvAutoMesh_quality_ComboBox.Text+',');
      //--
      RegexObj.Expression := '^[-+]?(\d+(\.\d*)?|\.\d+)$';
      if not RegexObj.Exec(InversionModelingInvAutoMesh_paraDepth_Edit.Text) then
      begin
        // 錯誤的資料內容
        temp_str := '錯誤:' + #13#10 +
          '研究區域深度數值錯誤。請檢查。';
        Application.MessageBox(PChar(temp_str), '錯誤', 16);
        Exit;
      end;
      InversionModelingSettingsNowJson_Memo.Lines.Strings[11-1] := ('"Mesh_Setting03_AutoMesh_paraDepth":'+InversionModelingInvAutoMesh_paraDepth_Edit.Text+',');
      //--
      InversionModelingSettingsNowJson_Memo.Lines.Strings[13-1] := ('"Mesh_Setting04_AutoMesh_boundary":'+InversionModelingInvAutoMesh_boundary_ComboBox.Text+',');
      //--
      RegexObj.Expression := '^[+]?(\d+(\.\d*)?|\.\d+)$';
      if not RegexObj.Exec(InversionModelingInvAutoMesh_paraMaxCellSize_Edit.Text) then
      begin
        // 錯誤的資料內容
        temp_str := '錯誤:' + #13#10 +
          '網格面積上限數值錯誤。請檢查。';
        Application.MessageBox(PChar(temp_str), '錯誤', 16);
        Exit;
      end;
      InversionModelingSettingsNowJson_Memo.Lines.Strings[15-1] := ('"Mesh_Setting05_AutoMesh_paraMaxCellSize":'+InversionModelingInvAutoMesh_paraMaxCellSize_Edit.Text+',');
      //--
      RegexObj.Expression := '^\+?\d+$';
      if not RegexObj.Exec(InversionModelingInvAutoMesh_addNodes_Edit.Text) then
      begin
        // 錯誤的資料內容
        temp_str := '錯誤:' + #13#10 +
          'addNodes數值錯誤。請檢查。';
        Application.MessageBox(PChar(temp_str), '錯誤', 16);
        Exit;
      end;
      InversionModelingSettingsNowJson_Memo.Lines.Strings[17-1] := ('"Mesh_Setting06_AutoMesh_addNodes":'+InversionModelingInvAutoMesh_addNodes_Edit.Text+',');
      //--
      RegexObj.Expression := '^[+]?(\d+(\.\d*)?|\.\d+)$';
      if not RegexObj.Exec(InversionModelingInvAutoMesh_paraDX_Edit.Text) then
      begin
        // 錯誤的資料內容
        temp_str := '錯誤:' + #13#10 +
          'paraDX數值錯誤。請檢查。';
        Application.MessageBox(PChar(temp_str), '錯誤', 16);
        Exit;
      end;
      InversionModelingSettingsNowJson_Memo.Lines.Strings[19-1] := ('"Mesh_Setting07_AutoMesh_paraDX":'+InversionModelingInvAutoMesh_paraDX_Edit.Text+',');
      //--
    end
    else
    begin
      temp_str:=InversionModelingInput02VTK_Edit.Text;
      if FileExists(temp_str) then
      begin
        ForceDirectories('Output_ERTMaker_Inversion2D\Input_Inversion2D\');
        CopyFileW(PWideChar(UTF8ToUTF16(temp_str)),
              PWideChar(UTF8ToUTF16('Output_ERTMaker_Inversion2D\Input_Inversion2D\'+ExtractFileName(temp_str))),
              False);// 第三個參數為 False 表示如果目標已存在則覆蓋 (Overwrite)
        temp_str := 'Output_ERTMaker_Inversion2D\Input_Inversion2D\'+ExtractFileName(temp_str);
        temp_str:=StringReplace(temp_str, '\', '/', [rfReplaceAll]);
        InversionModelingSettingsNowJson_Memo.Lines.Strings[21-1] := ('"InputFile02_MeshVTK_FileName":"'+temp_str+'",');
      end
      else
      begin
        temp_str := '錯誤:' + #13#10 +
          'vtk檔案不存在。';
        Application.MessageBox(PChar(temp_str), '錯誤', 16);
        Exit;
      end;
      temp_str:=InversionModelingInput03MeshJSON_Edit.Text;
      if FileExists(temp_str) then
      begin
        ForceDirectories('Output_ERTMaker_Inversion2D\Input_Inversion2D\');
        CopyFileW(PWideChar(UTF8ToUTF16(temp_str)),
              PWideChar(UTF8ToUTF16('Output_ERTMaker_Inversion2D\Input_Inversion2D\'+ExtractFileName(temp_str))),
              False);// 第三個參數為 False 表示如果目標已存在則覆蓋 (Overwrite)
        temp_str := 'Output_ERTMaker_Inversion2D\Input_Inversion2D\'+ExtractFileName(temp_str);
        temp_str:=StringReplace(temp_str, '\', '/', [rfReplaceAll]);
        InversionModelingSettingsNowJson_Memo.Lines.Strings[23-1] := ('"InputFile03_MeshBCMarkersJSON_FileName":"'+temp_str+'",');
      end
      else
      begin
        temp_str := '錯誤:' + #13#10 +
          'json檔案不存在。';
        Application.MessageBox(PChar(temp_str), '錯誤', 16);
        Exit;
      end;
    end;
    //--------------------------------------------------------------------------
    InversionModelingSettingsNowJson_Memo.Lines.Strings[25-1] := ('"Data_Setting00_RecalculateDataMode":"'+InversionModelingDataSettings_ReCalMode_ComboBox.Text+'",');
    //--
    RegexObj.Expression := '^\+?((0\.[0-9]*[1-9][0-9]*)|([1-9][0-9]*(\.[0-9]*)?))$';
    if not RegexObj.Exec(InversionModelingDataSettings_RelativeError_Edit.Text) then
    begin
      // 錯誤的資料內容
      temp_str := '錯誤:' + #13#10 +
        '資料相對誤差數值錯誤。請檢查。';
      Application.MessageBox(PChar(temp_str), '錯誤', 16);
      Exit;
    end;
    InversionModelingSettingsNowJson_Memo.Lines.Strings[27-1] := ('"Data_Setting01_Relative_Error_Percentage":'+InversionModelingDataSettings_RelativeError_Edit.Text+',');
    //--
    InversionModelingSettingsNowJson_Memo.Lines.Strings[29-1] := ('"Data_Setting02_GeometricFactorForInversion":"'+InversionModelingDataSettings_InvK_ComboBox.Text+'",');
    //--
    InversionModelingSettingsNowJson_Memo.Lines.Strings[31-1] := ('"Data_Setting03_GeometricFactorForRecalculateData":"'+InversionModelingDataSettings_ReCalK_ComboBox.Text+'",');
    if  (InversionModelingDataSettings_ReCalK_ComboBox.Text = 'Ideal_Mesh') then
    begin
      //--
      InversionModelingSettingsNowJson_Memo.Lines.Strings[33-1] := ('"Ideal_Mesh_Setting01_AutoMesh_quality":'+InversionModelingDataSettings_quality_ComboBox.Text+',');
      //--
      RegexObj.Expression := '^[-+]?(\d+(\.\d*)?|\.\d+)$';
      if not RegexObj.Exec(InversionModelingDataSettings_paraDepth_Edit.Text) then
      begin
        // 錯誤的資料內容
        temp_str := '錯誤:' + #13#10 +
          '理想網格研究區域深度數值錯誤。請檢查。';
        Application.MessageBox(PChar(temp_str), '錯誤', 16);
        Exit;
      end;
      InversionModelingSettingsNowJson_Memo.Lines.Strings[35-1] := ('"Ideal_Mesh_Setting02_AutoMesh_paraDepth":'+InversionModelingDataSettings_paraDepth_Edit.Text+',');
      //--
      InversionModelingSettingsNowJson_Memo.Lines.Strings[37-1] := ('"Ideal_Mesh_Setting03_AutoMesh_boundary":'+InversionModelingDataSettings_boundary_ComboBox.Text+',');
      //--
      RegexObj.Expression := '^[+]?(\d+(\.\d*)?|\.\d+)$';
      if not RegexObj.Exec(InversionModelingDataSettings_paraMaxCellSize_Edit.Text) then
      begin
        // 錯誤的資料內容
        temp_str := '錯誤:' + #13#10 +
          '理想網格網格面積上限數值錯誤。請檢查。';
        Application.MessageBox(PChar(temp_str), '錯誤', 16);
        Exit;
      end;
      InversionModelingSettingsNowJson_Memo.Lines.Strings[39-1] := ('"Ideal_Mesh_Setting04_AutoMesh_paraMaxCellSize":'+InversionModelingDataSettings_paraMaxCellSize_Edit.Text+',');
      //--
      RegexObj.Expression := '^\+?\d+$';
      if not RegexObj.Exec(InversionModelingDataSettings_addNodes_Edit.Text) then
      begin
        // 錯誤的資料內容
        temp_str := '錯誤:' + #13#10 +
          '理想網格addNodes數值錯誤。請檢查。';
        Application.MessageBox(PChar(temp_str), '錯誤', 16);
        Exit;
      end;
      InversionModelingSettingsNowJson_Memo.Lines.Strings[41-1] := ('"Ideal_Mesh_Setting05_AutoMesh_addNodes":'+InversionModelingDataSettings_addNodes_Edit.Text+',');
      //--
      RegexObj.Expression := '^[+]?(\d+(\.\d*)?|\.\d+)$';
      if not RegexObj.Exec(InversionModelingDataSettings_paraDX_Edit.Text) then
      begin
        // 錯誤的資料內容
        temp_str := '錯誤:' + #13#10 +
          '理想網格paraDX數值錯誤。請檢查。';
        Application.MessageBox(PChar(temp_str), '錯誤', 16);
        Exit;
      end;
      InversionModelingSettingsNowJson_Memo.Lines.Strings[43-1] := ('"Ideal_Mesh_Setting06_AutoMesh_paraDX":'+InversionModelingDataSettings_paraDX_Edit.Text+',');
      //--
    end;
    //--
    //--------------------------------------------------------------------------
    RegexObj.Expression := '^[+]?(\d+(\.\d*)?|\.\d+)$';
    if not RegexObj.Exec(InversionModelingDataPrepare_RemoveCurrentLowerThan_Edit.Text) then
    begin
      // 錯誤的資料內容
      temp_str := '錯誤:' + #13#10 +
        '移除電流低於數值錯誤。請檢查。';
      Application.MessageBox(PChar(temp_str), '錯誤', 16);
      Exit;
    end;
    InversionModelingSettingsNowJson_Memo.Lines.Strings[45-1] := ('"Data_Setting03_RemoveLowCurrentData_LowerThan_A":'+InversionModelingDataPrepare_RemoveCurrentLowerThan_Edit.Text+',');
    //--
    RegexObj.Expression := '^\+?((0\.[0-9]*[1-9][0-9]*)|([1-9][0-9]*(\.[0-9]*)?))$';
    if not RegexObj.Exec(InversionModelingDataPrepare_RemoveCurrentHigherThan_Edit.Text) then
    begin
      // 錯誤的資料內容
      temp_str := '錯誤:' + #13#10 +
        '移除電流高於數值錯誤。請檢查。';
      Application.MessageBox(PChar(temp_str), '錯誤', 16);
      Exit;
    end;
    InversionModelingSettingsNowJson_Memo.Lines.Strings[47-1] := ('"Data_Setting04_RemoveHighCurrentData_HigherThan_A":'+InversionModelingDataPrepare_RemoveCurrentHigherThan_Edit.Text+',');
    //--
    RegexObj.Expression := '^[-+]?(\d+(\.\d*)?|\.\d+)$';
    if not RegexObj.Exec(InversionModelingDataPrepare_RemoveVoltageLowerThan_Edit.Text) then
    begin
      // 錯誤的資料內容
      temp_str := '錯誤:' + #13#10 +
        '移除電壓低於數值錯誤。請檢查。';
      Application.MessageBox(PChar(temp_str), '錯誤', 16);
      Exit;
    end;
    InversionModelingSettingsNowJson_Memo.Lines.Strings[49-1] := ('"Data_Setting05_RemoveLowVoltageData_LowerThan_V":'+InversionModelingDataPrepare_RemoveVoltageLowerThan_Edit.Text+',');
    //--
    RegexObj.Expression := '^[-+]?(\d+(\.\d*)?|\.\d+)$';
    if not RegexObj.Exec(InversionModelingDataPrepare_RemoveVoltageHigherThan_Edit.Text) then
    begin
      // 錯誤的資料內容
      temp_str := '錯誤:' + #13#10 +
        '移除電壓高於數值錯誤。請檢查。';
      Application.MessageBox(PChar(temp_str), '錯誤', 16);
      Exit;
    end;
    InversionModelingSettingsNowJson_Memo.Lines.Strings[51-1] := ('"Data_Setting06_RemoveHighVoltageData_HigherThan_V":'+InversionModelingDataPrepare_RemoveVoltageHigherThan_Edit.Text+',');
    //--
    RegexObj.Expression := '^[+]?(\d+(\.\d*)?|\.\d+)$';
    if not RegexObj.Exec(InversionModelingDataPrepare_RemoveAppResLowerThan_Edit.Text) then
    begin
      // 錯誤的資料內容
      temp_str := '錯誤:' + #13#10 +
        '移除視電阻率低於數值錯誤。請檢查。';
      Application.MessageBox(PChar(temp_str), '錯誤', 16);
      Exit;
    end;
    InversionModelingSettingsNowJson_Memo.Lines.Strings[53-1] := ('"Data_Setting07_RemoveLowAppResData_LowerThan_OhmM":'+InversionModelingDataPrepare_RemoveAppResLowerThan_Edit.Text+',');
    //--
    RegexObj.Expression := '^[+]?(\d+(\.\d*)?|\.\d+)$';
    if not RegexObj.Exec(InversionModelingDataPrepare_RemoveAppResHigherThan_Edit.Text) then
    begin
      // 錯誤的資料內容
      temp_str := '錯誤:' + #13#10 +
        '移除視電阻率高於數值錯誤。請檢查。';
      Application.MessageBox(PChar(temp_str), '錯誤', 16);
      Exit;
    end;
    InversionModelingSettingsNowJson_Memo.Lines.Strings[55-1] := ('"Data_Setting08_RemoveHighAppResData_HigherThan_OhmM":'+InversionModelingDataPrepare_RemoveAppResHigherThan_Edit.Text+',');
    //--
    if InversionModelingDataPrepare_RemoveBadElectrode_CheckBox.Checked then
    begin
      RegexObj.Expression := '^\d+(,\d+)*$';
      if not RegexObj.Exec(InversionModelingDataPrepare_RemoveBadElectrode_Edit.Text) then
      begin
        // 錯誤的資料內容
        temp_str := '錯誤:' + #13#10 +
          '移除電極索引數值錯誤。請檢查。';
        Application.MessageBox(PChar(temp_str), '錯誤', 16);
        Exit;
      end;
      InversionModelingSettingsNowJson_Memo.Lines.Strings[57-1] := ('"Data_Setting09_RemoveBadElectrodeData_List":['+InversionModelingDataPrepare_RemoveBadElectrode_Edit.Text+'],');
      //--
    end;
    //--
    if InversionModelingDataPrepare_UseFakeDataEnable_CheckBox.Checked then
    begin
      InversionModelingSettingsNowJson_Memo.Lines.Strings[63-1] := ('"Data_Setting12_UseFakeData_Enable":"Yes",');
      //--
      RegexObj.Expression := '^\+?((0\.[0-9]*[1-9][0-9]*)|([1-9][0-9]*(\.[0-9]*)?))$';
      if not RegexObj.Exec(InversionModelingDataPrepare_UseFakeDataRhoa_Edit.Text) then
      begin
        // 錯誤的資料內容
        temp_str := '錯誤:' + #13#10 +
          '電阻率取代數值錯誤。請檢查。';
        Application.MessageBox(PChar(temp_str), '錯誤', 16);
        Exit;
      end;
      InversionModelingSettingsNowJson_Memo.Lines.Strings[65-1] := ('"Data_Setting13_UseFakeData_rhoa":'+InversionModelingDataPrepare_UseFakeDataRhoa_Edit.Text+',');
      //--
    end;
    //--
    RegexObj.Expression := '^\+?\d+$';
    if not RegexObj.Exec(InversionModelingInvSettings_RemoveTimes_Edit.Text) then
    begin
      // 錯誤的資料內容
      temp_str := '錯誤:' + #13#10 +
        '資料剔除次數數值錯誤。請檢查。';
      Application.MessageBox(PChar(temp_str), '錯誤', 16);
      Exit;
    end;
    InversionModelingSettingsNowJson_Memo.Lines.Strings[73-1] := ('"Inversion_Setting01_Remove_Data_Times":'+InversionModelingInvSettings_RemoveTimes_Edit.Text+',');
    //--
    RegexObj.Expression := '^\+?([1-9][0-9]?)$';
    if not RegexObj.Exec(InversionModelingInvSettings_RemovePercentage_Edit.Text) then
    begin
      // 錯誤的資料內容
      temp_str := '錯誤:' + #13#10 +
        '資料剔除[%]數值錯誤。請檢查。';
      Application.MessageBox(PChar(temp_str), '錯誤', 16);
      Exit;
    end;
    InversionModelingSettingsNowJson_Memo.Lines.Strings[75-1] := ('"Inversion_Setting02_Remove_Data_Percentage":'+InversionModelingInvSettings_RemovePercentage_Edit.Text+',');
    //--
    RegexObj.Expression := '^\[\s*\d+(\.\d*)?\s*(,\s*\d+(\.\d*)?\s*)*\]$';
    if not RegexObj.Exec(InversionModelingInvSettings_Lambda_Edit.Text) then
    begin
      // 錯誤的資料內容
      temp_str := '錯誤:' + #13#10 +
        'Lambda數值錯誤。請檢查。';
      Application.MessageBox(PChar(temp_str), '錯誤', 16);
      Exit;
    end;
    InversionModelingSettingsNowJson_Memo.Lines.Strings[77-1] := ('"Inversion_Setting03_Lamda":'+InversionModelingInvSettings_Lambda_Edit.Text+',');
    //--
    RegexObj.Expression := '^\+?\d+$';
    if not RegexObj.Exec(InversionModelingInvSettings_maxIter_Edit.Text) then
    begin
      // 錯誤的資料內容
      temp_str := '錯誤:' + #13#10 +
        '最大迭代次數數值錯誤。請檢查。';
      Application.MessageBox(PChar(temp_str), '錯誤', 16);
      Exit;
    end;
    InversionModelingSettingsNowJson_Memo.Lines.Strings[79-1] := ('"Inversion_Setting04_maxIter":'+InversionModelingInvSettings_maxIter_Edit.Text+',');
    //--
    RegexObj.Expression := '^([1-9]\d*|-(1|2|3))$';
    if not RegexObj.Exec(InversionModelingInvSettings_startModel_Edit.Text) then
    begin
      // 錯誤的資料內容
      temp_str := '錯誤:' + #13#10 +
        '逆推起始模型數值錯誤。請檢查。';
      Application.MessageBox(PChar(temp_str), '錯誤', 16);
      Exit;
    end;
    InversionModelingSettingsNowJson_Memo.Lines.Strings[81-1] := ('"Inversion_Setting05_startModel":'+InversionModelingInvSettings_startModel_Edit.Text+',');
    //--
    if (InversionModelingInvSettings_startModel_Edit.Text = '-3') then
    begin
      temp_str:='Input_ERTMaker_Inversion2D/InputFile05_StudyAreaStartModelVTK.vtk';
      if FileExists(temp_str) then
      begin
        InversionModelingSettingsNowJson_Memo.Lines.Strings[83-1] := ('"InputFile05_StudyAreaStartModelVTK_FileName":"'+temp_str+'",');
        CopyFileW(PWideChar(UTF8ToUTF16(temp_str)),
          PWideChar(UTF8ToUTF16('Output_ERTMaker_Inversion2D/Input_Inversion2D/'+ExtractFileName(temp_str))),
          False);// 第三個參數為 False 表示如果目標已存在則覆蓋 (Overwrite)
      end
      else
      begin
        temp_str := '錯誤:' + #13#10 +
          '逆推起始模型vtk檔案不存在。';
        Application.MessageBox(PChar(temp_str), '錯誤', 16);
        Exit;
      end;
    end;
    //--
    if InversionModelingOutputSettings_verbose_ComboBox.Text = '停用' then
    begin
      InversionModelingSettingsNowJson_Memo.Lines.Strings[85-1] := ('"Inversion_Setting06_verbose_Enable":"No",');
    end;
    //--
    RegexObj.Expression := '^\[\s*(\d+(\.\d*)?|\.\d+)\s*,\s*(\d+(\.\d*)?|\.\d+)\s*\]$';
    if not RegexObj.Exec(InversionModelingInvSettings_limitModelCell_Edit.Text) then
    begin
      // 錯誤的資料內容
      temp_str := '錯誤:' + #13#10 +
        '模型電阻率範圍數值錯誤。請檢查。';
      Application.MessageBox(PChar(temp_str), '錯誤', 16);
      Exit;
    end;
    temp_str := InversionModelingInvSettings_limitModelCell_Edit.Text;
    temp_str1 := temp_str.Substring(1, temp_str.IndexOf(',') - 1);
    //--
    temp_str := InversionModelingInvSettings_limitModelCell_Edit.Text;
    temp_str2 := temp_str.Substring(temp_str.IndexOf(',') + 1).Trim([']']);
    //--
    if (StrToFloat(temp_str1) = 0) and (StrToFloat(temp_str2) = 0) then
    begin
      InversionModelingSettingsNowJson_Memo.Lines.Strings[87-1] := ('"Inversion_Setting07_limitModelCellMinValue":'+temp_str1+',');
      InversionModelingSettingsNowJson_Memo.Lines.Strings[89-1] := ('"Inversion_Setting08_limitModelCellMaxValue":'+temp_str2+',');
    end
    else if StrToFloat(temp_str1) < StrToFloat(temp_str2) then
    begin
      InversionModelingSettingsNowJson_Memo.Lines.Strings[87-1] := ('"Inversion_Setting07_limitModelCellMinValue":'+temp_str1+',');
      InversionModelingSettingsNowJson_Memo.Lines.Strings[89-1] := ('"Inversion_Setting08_limitModelCellMaxValue":'+temp_str2+',');
    end
    else
    begin
      // 錯誤的資料內容
      temp_str := '錯誤:' + #13#10 +
        '模型電阻率範圍數值大小錯誤。請檢查。';
      Application.MessageBox(PChar(temp_str), '錯誤', 16);
      Exit;
    end;
    //--
    RegexObj.Expression := '^[^\\/:\*\?"<>\|]+$';
    if not RegexObj.Exec(InversionModelingOutputSettings_MainName_Edit.Text) then
    begin
      // 錯誤的資料內容
      temp_str := '錯誤:' + #13#10 +
        '測線名稱錯誤。請檢查。';
      Application.MessageBox(PChar(temp_str), '錯誤', 16);
      Exit;
    end;
    InversionModelingSettingsNowJson_Memo.Lines.Strings[95-1] := ('"Output_MainFileName":"'+InversionModelingOutputSettings_MainName_Edit.Text+'",');
    //--
    RegexObj.Expression := '^\[\s*(\d+(\.\d*)?|\.\d+)\s*,\s*(\d+(\.\d*)?|\.\d+)\s*\]$';
    if not RegexObj.Exec(InversionModelingInvSettings_ColorBar_Edit.Text) then
    begin
      // 錯誤的資料內容
      temp_str := '錯誤:' + #13#10 +
        '色階範圍數值錯誤。請檢查。';
      Application.MessageBox(PChar(temp_str), '錯誤', 16);
      Exit;
    end;
    temp_str := InversionModelingInvSettings_ColorBar_Edit.Text;
    temp_str1 := temp_str.Substring(1, temp_str.IndexOf(',') - 1);
    //--
    temp_str := InversionModelingInvSettings_ColorBar_Edit.Text;
    temp_str2 := temp_str.Substring(temp_str.IndexOf(',') + 1).Trim([']']);
    //--
    if StrToFloat(temp_str1) < StrToFloat(temp_str2) then
    begin
      InversionModelingSettingsNowJson_Memo.Lines.Strings[97-1] := ('"Output_Inversion_ColorBarResistivityMin":'+temp_str1+',');
      InversionModelingSettingsNowJson_Memo.Lines.Strings[99-1] := ('"Output_Inversion_ColorBarResistivityMax":'+temp_str2+',');
    end
    else
    begin
      // 錯誤的資料內容
      temp_str := '錯誤:' + #13#10 +
        '裁切範圍數值大小錯誤。請檢查。';
      Application.MessageBox(PChar(temp_str), '錯誤', 16);
      Exit;
    end;
    //--
    if InversionModelingInvSettings_XYMinMax_CheckBox.Checked then
    begin
      InversionModelingSettingsNowJson_Memo.Lines.Strings[101-1] := ('"Output_Inversion_XMinMax_Enable":"Yes",');
      //--
      RegexObj.Expression := '^\[\s*(-?\d+(\.\d*)?|-?\.\d+)\s*,\s*(-?\d+(\.\d*)?|-?\.\d+)\s*,\s*(-?\d+(\.\d*)?|-?\.\d+)\s*,\s*(-?\d+(\.\d*)?|-?\.\d+)\s*\]$';
      if not RegexObj.Exec(InversionModelingInvSettings_XYMinMax_Edit.Text) then
      begin
        // 錯誤的資料內容
        temp_str := '錯誤:' + #13#10 +
          '裁切範圍數值錯誤。請檢查。';
        Application.MessageBox(PChar(temp_str), '錯誤', 16);
        Exit;
      end;
      temp_str := InversionModelingInvSettings_XYMinMax_Edit.Text;
      temp_str1 := temp_str.Substring(1, temp_str.IndexOf(',') - 1);
      //--
      temp_str := InversionModelingInvSettings_XYMinMax_Edit.Text;
      temp_str := temp_str.Substring(temp_str.IndexOf(',') + 1);
      temp_str2 := temp_str.Substring(0, temp_str.IndexOf(','));
      //--
      temp_str := InversionModelingInvSettings_XYMinMax_Edit.Text;
      temp_str := temp_str.Substring(temp_str.IndexOf(',') + 1);
      temp_str := temp_str.Substring(temp_str.IndexOf(',') + 1);
      temp_str3 := temp_str.Substring(0, temp_str.IndexOf(','));
      //--
      temp_str := InversionModelingInvSettings_XYMinMax_Edit.Text;
      temp_str := temp_str.Substring(temp_str.IndexOf(',') + 1);
      temp_str := temp_str.Substring(temp_str.IndexOf(',') + 1);
      temp_str := temp_str.Substring(temp_str.IndexOf(',') + 1);
      temp_str4 := temp_str.Substring(0, temp_str.Length - 1);
      //--
      if StrToFloat(temp_str1) < StrToFloat(temp_str2) then
      begin
        InversionModelingSettingsNowJson_Memo.Lines.Strings[103-1] := ('"Output_Inversion_XMin":'+temp_str1+',');
        InversionModelingSettingsNowJson_Memo.Lines.Strings[105-1] := ('"Output_Inversion_XMax":'+temp_str2+',');
      end
      else
      begin
        // 錯誤的資料內容
        temp_str := '錯誤:' + #13#10 +
          '裁切範圍數值大小錯誤。請檢查。';
        Application.MessageBox(PChar(temp_str), '錯誤', 16);
        Exit;
      end;
      //--
      if StrToFloat(temp_str3) < StrToFloat(temp_str4) then
      begin
        InversionModelingSettingsNowJson_Memo.Lines.Strings[107-1] := ('"Output_Inversion_YMin":'+temp_str3+',');
       InversionModelingSettingsNowJson_Memo.Lines.Strings[109-1] := ('"Output_Inversion_YMax":'+temp_str4+',');
      end
      else
      begin
        // 錯誤的資料內容
        temp_str := '錯誤:' + #13#10 +
          '裁切範圍數值大小錯誤。請檢查。';
        Application.MessageBox(PChar(temp_str), '錯誤', 16);
        Exit;
      end;
      //--
    end;
    //--------------------------------------------------------------------------
    //--------------------------------------------------------------------------
    // 儲存預設JSON檔案
    ForceDirectories('Input_ERTMaker_Inversion2D');
    InversionModelingSettingsNowJson_Memo.Lines.SaveToFile('Input_ERTMaker_Inversion2D/Inversion2D.json',TEncoding.UTF8);
    InversionModelingSettingsNowJson_Memo.Lines.SaveToFile('Output_ERTMaker_Inversion2D/Input_Inversion2D/Inversion2D.json',TEncoding.UTF8);
    //--------------------------------------------------------------------------
    //--------------------------------------------------------------------------
    // 外部Python程式檢查
    if not FileExists('PythonEnv\Python.exe') then
    begin
      temp_str := '錯誤:' + #13#10 +
        '外部Python環境不存在。';
      Application.MessageBox(PChar(temp_str), '錯誤', 16);
      Exit;
    end;
    if not FileExists('ERTMaker_Inversion2D_v20260401a.cpython-312.pyc') then
    begin
      temp_str := '錯誤:' + #13#10 +
        '外部Python程式不存在。';
      Application.MessageBox(PChar(temp_str), '錯誤', 16);
      Exit;
    end;
    //--------------------------------------------------------------------------
    //--------------------------------------------------------------------------
    InversionModelingSettingsCmdLog_Memo.Lines.Clear;
    // 使用 InversionModeling_AsyncProcess 運行外部程式
    InversionModeling_AsyncProcess.Executable:='cmd.exe';
    InversionModeling_AsyncProcess.Parameters.Clear;
    InversionModeling_AsyncProcess.Parameters.Add('/c');
    InversionModeling_AsyncProcess.Parameters.Add('set PYTHONIOENCODING=utf-8 && .\PythonEnv\Python.exe -u ERTMaker_Inversion2D_v20260401a.cpython-312.pyc');
    InversionModeling_AsyncProcess.Options:=[poUsePipes, poStderrToOutPut, poNoConsole];
    InversionModeling_AsyncProcess.Execute;
    // 啟用Timer
    InversionModeling_Timer.Enabled:=True;
    StatusBar1.Panels[1].Text:='1';
    StatusBar1.Panels[4].Text:='正在自動更新資訊';
    // 禁用元件，等背景運作完再從事件重新啟用按鈕
    Forward_TabSheet.Enabled:=False;
    InversionModelingRun_ToolButton.Enabled:=False;
    InversionModelingParameters_GroupBox.Enabled:=False;
    //--------------------------------------------------------------------------
  finally
    RegexObj.Free; // 釋放 TRegExpr 物件
  end;
  //--------------------------------------------------------------------------
end;
```

