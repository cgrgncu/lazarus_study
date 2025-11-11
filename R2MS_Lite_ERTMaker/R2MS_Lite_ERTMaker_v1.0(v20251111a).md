# R2MS_Lite_ERTMaker 開發筆記
+ 作者: HsiupoYeh  
+ 更新日期: 2025-11-11
+ 從 **R2MS_Lite_ERTMaker_v1.0(v20251106a).md** 開始，繼續更新基本功能。

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
  ...\PythonEnv\
  ...\PythonEnv\python.exe
  ...\PythonPrograms\... (相關的python程式)
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
  version_str := 'v1.0(v20251111a)';
  Current_Folder_Path := ExtractFilePath(Application.ExeName);
  //--------------------------------------------------------------------------
  //--------------------------------------------------------------------------
  // 表單標題追加版本
  Form1.Caption := Form1.Caption + ' ' + version_str;
  //--------------------------------------------------------------------------
end;      
```

### 調整UI
+ 1.1 拖拉一個「Standard>TGroupBox」到「ForwardModelingParameters_GroupBox」中。預設名稱會是「GroupBox1」，修改「Name」為「ForwardModelingCurrentMode_GroupBox」。
  + 1.1.1 設定「Anchors」。   
    + 1.1.1.1 「Top anchoring>Slbling」選為「ForwardModelingParameters_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
    + 1.1.1.2 「Left anchoring>Slbling」選為「ForwardModelingParameters_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
    + 1.1.1.3 「Right anchoring>Slbling」選為「ForwardModelingParameters_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
    + 1.1.1.4 「Border space」正中間那格填「0」，左邊那格填「3」，右邊那格填「3」。**  
    + 1.1.1.5 設定完後關閉Anchors設定視窗。**
  + 1.1.2 設定「Caption」為「AB電極的放電設定:」。
  + 1.1.3 設定「Height」為「70」。
+ 1.2 拖拉一個「Standard>TCheckBox」到「ForwardModelingCurrentMode_GroupBox」中。預設名稱會是「CheckBox1」，修改「Name」為「ForwardModelingCurrentMode_CheckBox」。
  + 1.2.1 設定「Anchors」。   
    + 1.2.1.1 「Top anchoring>Slbling」選為「ForwardModelingCurrentMode_GroupBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選上面的。**
    + 1.2.1.2 「Left anchoring>Slbling」選為「ForwardModelingCurrentMode_GroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
    + 1.2.1.3 「Border space」正中間那格填「0」，左邊那格填「3」。**  
    + 1.2.1.4 設定完後關閉Anchors設定視窗。**
  + 1.2.2 設定「Caption」為「自動設定」。
  + 1.2.3 設定「Checked」為「True」。
  + 1.2.4「Event」頁面下「OnClick」為如下程式碼。
  ```
  procedure TForm1.ForwardModelingCurrentMode_CheckBoxClick(Sender: TObject);
  begin
    //--------------------------------------------------------------------------
    // 控制下拉選單狀態
    if ForwardModelingCurrentMode_CheckBox.Checked then
    begin
      ForwardModelingCurrentMode_ComboBox.Enabled := False;
    end
    else
    begin
      ForwardModelingCurrentMode_ComboBox.Enabled := True;
      if ForwardModelingCurrentMode_ComboBox.ItemIndex < 0 then
      begin
        ForwardModelingCurrentMode_ComboBox.ItemIndex:=0;
      end;
    end;
    //--------------------------------------------------------------------------
  end; 
  ```
+ 1.3 拖拉一個「Standard>TComboBox」到「ForwardModelingCurrentMode_GroupBox」中。預設名稱會是「ComboBox1」，修改「Name」為「ForwardModelingCurrentMode_ComboBox」。
  + 1.3.1 設定「Anchors」。   
    + 1.3.1.1 「Top anchoring>Slbling」選為「ForwardModelingCurrentMode_CheckBox:TCheckBox」。維持「Top anchoring>Enable」為打勾。維持「Top anchoring」右側三個鈕選下面的。**
    + 1.3.1.2 「Left anchoring>Slbling」選為「ForwardModelingCurrentMode_GroupBox:TGroupBox」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
    + 1.3.1.3 「Right anchoring>Slbling」選為「ForwardModelingCurrentMode_GroupBox:TGroupBox」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選右邊的。**  
    + 1.3.1.4 「Border space」正中間那格填「0」，上面那格填「3」，左邊那格填「3」，右邊那格填「3」。**  
    + 1.3.1.5 設定完後關閉Anchors設定視窗。**
  + 1.3.2 設定「Enabled」為「False」。
  + 1.3.3 設定「Items」為如下內容。
  ```
  S10N
  S10HS
  S10VHS
  S11N
  S11HS
  S11VHS
  S12N
  S12HS
  S12VHS
  S13N
  S13HS
  S13VHS
  S14N
  S14HS
  S14VHS
  S15N
  S15HS
  S15VHS
  S16N
  S16HS
  S16VHS
  S17N
  S17HS
  S17VHS
  S18N
  S18HS
  S18VHS
  S19N
  S19HS
  S19VHS
  S20N
  S20HS
  S20VHS
  S21N
  S21HS
  S21VHS
  S22N
  S22HS
  S22VHS
  S23N
  S23HS
  S23VHS
  S24N
  S24HS
  S24VHS
  S25N
  S25HS
  S25VHS
  S26N
  S26HS
  S26VHS
  S27N
  S27HS
  S27VHS
  S28N
  S28HS
  S28VHS
  S29N
  S29HS
  S29VHS
  S30N
  S30HS
  S30VHS
  S31N
  S31HS
  S31VHS
  S32N
  S32HS
  S32VHS
  S33N
  S33HS
  S33VHS
  S34N
  S34HS
  S34VHS
  S35N
  S35HS
  S35VHS
  S36N
  S36HS
  S36VHS
  S37N
  S37HS
  S37VHS
  S38N
  S38HS
  S38VHS
  S39N
  S39HS
  S39VHS
  S40N
  S40HS
  S40VHS
  S41N
  S41HS
  S41VHS
  S42N
  S42HS
  S42VHS
  S43N
  S43HS
  S43VHS
  S44N
  S44HS
  S44VHS
  S45N
  S45HS
  S45VHS
  S46N
  S46HS
  S46VHS
  S47N
  S47HS
  S47VHS
  S48N
  S48HS
  S48VHS
  S49N
  S49HS
  S49VHS
  S50N
  S50HS
  S50VHS
  S51N
  S51HS
  S51VHS
  S52N
  S52HS
  S52VHS
  S53N
  S53HS
  S53VHS
  S54N
  S54HS
  S54VHS
  S55N
  S55HS
  S55VHS
  S56N
  S56HS
  S56VHS
  S57N
  S57HS
  S57VHS
  S58N
  S58HS
  S58VHS
  S59N
  S59HS
  S59VHS
  S60N
  S60HS
  S60VHS
  S61N
  S61HS
  S61VHS
  S62N
  S62HS
  S62VHS
  S63N
  S63HS
  S63VHS
  S64N
  S64HS
  S64VHS
  Custom01
  Custom02
  Custom03
  ```
  + 1.3.4 設定「Style」為「csDropDownList」。
  + 1.3.5 設定「Text」為空白。
+ 1.4 修改「ForwardModelingElectrodeIndexABTxVoltageMax_GroupBox」。
  + 1.1.1 設定「Anchors」。   
    + 1.1.1.1 「Top anchoring>Slbling」選為「ForwardModelingCurrentMode_GroupBox:TGroupBox」。維持「Top anchoring>Enable」為打勾。選擇「Top anchoring」右側三個鈕選下面的。**
    + 1.1.1.2 設定完後關閉Anchors設定視窗。**

### 寫功能
+ 2.1 去修改「CreateMeshRun_ToolButton」的「Event」頁面下「OnClick」為如下程式碼。
```pascal
procedure TForm1.CreateMeshRun_ToolButtonClick(Sender: TObject);
var
  temp_i: Integer;
  RegexObj: TRegExpr;
  temp_str: AnsiString;
  temp_StartTime: DWord;
begin
  CreateMeshSettingsNowJson_Memo.Clear;
  CreateMeshPreview_Image.Picture.Clear;
  StatusBar1.Panels[0].Text:='建立模型網格...請稍後!';
  //--------------------------------------------------------------------------
  //
  for temp_i := 0 to 64-1 do
  begin
    CreateMeshSettingsNowJson_Memo.Lines.Add(CreateMeshSettingsDefaultJson_Memo.Lines.Strings[temp_i]);
  end;
  //--------------------------------------------------------------------------
  //--
  // 刪除圖片
  temp_str:=CreateMeshSettingsNowJson_Memo.Lines.Strings[41-1];
  temp_str:=StringReplace(temp_str, '"OutputFile06_BasicMeshPNG_FileName":"', '', [rfReplaceAll]);
  temp_str:=StringReplace(temp_str, '",', '', [rfReplaceAll]);
  if FileExists(temp_str) then
  begin
    DeleteFile(PChar(temp_str));
    StatusBar1.Panels[0].Text:='建立模型網格...已刪除舊的預覽圖片，請稍後!';
  end;
  //--
  RegexObj := TRegExpr.Create;
  try
    //--------------------------------------------------------------------------
    // CreateMeshModelName_Edit
    RegexObj.Expression := '[<>:"/\\|?*]';
    if RegexObj.Exec(CreateMeshModelName_Edit.Text) then
    begin
      // --- 發現非法字元 ---
      temp_str := '錯誤:' + #13#10 +
        '模型名稱為不接受的字元。' + #13#10 +
        '不可使用以下字元: <>:"/\\|?*';
      Application.MessageBox(PChar(temp_str), '錯誤', 16);
      Exit;
    end;
    CreateMeshSettingsNowJson_Memo.Lines.Strings[31-1] := ('"OutputFile01_MeshBCMarkersJSON_FileName":"Output_ERTMaker_CreateAndModifyMesh/'+CreateMeshModelName_Edit.Text+'_SyntheticModelBCMarkers.json",');
    CreateMeshSettingsNowJson_Memo.Lines.Strings[33-1] := ('"OutputFile02_MeshBCMarkersVTK_FileName":"Output_ERTMaker_CreateAndModifyMesh/'+CreateMeshModelName_Edit.Text+'_SyntheticModel.vtk",');
    CreateMeshSettingsNowJson_Memo.Lines.Strings[35-1] := ('"OutputFile03_GEO_FileName":"Output_ERTMaker_CreateAndModifyMesh/'+CreateMeshModelName_Edit.Text+'_SyntheticModel.geo",');
    CreateMeshSettingsNowJson_Memo.Lines.Strings[37-1] := ('"OutputFile04_TRN_FileName":"Output_ERTMaker_CreateAndModifyMesh/'+CreateMeshModelName_Edit.Text+'_SyntheticModel.trn",');
    CreateMeshSettingsNowJson_Memo.Lines.Strings[39-1] := ('"OutputFile05_OHM_FileName":"Output_ERTMaker_CreateAndModifyMesh/'+CreateMeshModelName_Edit.Text+'_SyntheticModel.ohm",');
    CreateMeshSettingsNowJson_Memo.Lines.Strings[41-1] := ('"OutputFile06_BasicMeshPNG_FileName":"Output_ERTMaker_CreateAndModifyMesh/'+CreateMeshModelName_Edit.Text+'_SyntheticModel.png",');
    CreateMeshSettingsNowJson_Memo.Lines.Strings[49-1] := ('"OutputFile06_BasicMeshPNG_Title":"'+CreateMeshModelName_Edit.Text+' Synthetic Model",');
    //--------------------------------------------------------------------------
    //--------------------------------------------------------------------------
    // CreateMeshColorbarMinMax_Edit
    RegexObj.Expression := '^\s*([-+]?(\d+\.?\d*|\.\d+))\s*,\s*([-+]?(\d+\.?\d*|\.\d+))\s*$';
    if not RegexObj.Exec(CreateMeshColorbarMinMax_Edit.Text) then
    begin
      // 錯誤的資料內容
      temp_str := '錯誤:' + #13#10 +
        '電阻率色階數值錯誤。請檢查。';
      Application.MessageBox(PChar(temp_str), '錯誤', 16);
      Exit;
    end;
    if StrToFloat(RegexObj.Match[1]) >= StrToFloat(RegexObj.Match[3]) then
    begin
      temp_str := '錯誤:' + #13#10 +
        '電阻率色階最小值必須小於最大值。';
      Application.MessageBox(PChar(temp_str), '錯誤', 16);
      Exit;
    end;
    CreateMeshSettingsNowJson_Memo.Lines.Strings[51-1] := ('"OutputFile06_BasicMeshPNG_ColorBarResistivityMin":'+RegexObj.Match[1]+',');
    CreateMeshSettingsNowJson_Memo.Lines.Strings[53-1] := ('"OutputFile06_BasicMeshPNG_ColorBarResistivityMax":'+RegexObj.Match[3]+',');
    //--------------------------------------------------------------------------
    //--------------------------------------------------------------------------
    // CreateMeshStudyAreaMeshLayerSettings_Edit
    RegexObj.Expression := '^\s*(\d+)\s*,\s*(\d*\.?\d+)\s*,\s*(\d*\.?\d+)\s*$';
    if not RegexObj.Exec(CreateMeshStudyAreaMeshLayerSettings_Edit.Text) then
    begin
      // 錯誤的資料內容
      temp_str := '錯誤:' + #13#10 +
        '研究區域內網格設定(地層數,厚度因子,首層厚度)值錯誤。請檢查。';
      Application.MessageBox(PChar(temp_str), '錯誤', 16);
      Exit;
    end;
    if StrToFloat(RegexObj.Match[1]) <= 0 then
    begin
      temp_str := '錯誤:' + #13#10 +
        '研究區域內網格設定(地層數,厚度因子,首層厚度)值第1個數字必須為正整數。';
      Application.MessageBox(PChar(temp_str), '錯誤', 16);
      Exit;
    end;
    if StrToFloat(RegexObj.Match[2]) <= 0 then
    begin
      temp_str := '錯誤:' + #13#10 +
        '研究區域內網格設定(地層數,厚度因子,首層厚度)值第2個數字必須為正數。';
      Application.MessageBox(PChar(temp_str), '錯誤', 16);
      Exit;
    end;
    if StrToFloat(RegexObj.Match[3]) <= 0 then
    begin
      temp_str := '錯誤:' + #13#10 +
        '研究區域內網格設定(地層數,厚度因子,首層厚度)值第3個數字必須為正數。';
      Application.MessageBox(PChar(temp_str), '錯誤', 16);
      Exit;
    end;
    CreateMeshSettingsNowJson_Memo.Lines.Strings[57-1] := ('"StudyAreaMesh_RectangleGrid_LayerCount":'+RegexObj.Match[1]+',');
    CreateMeshSettingsNowJson_Memo.Lines.Strings[59-1] := ('"StudyAreaMesh_RectangleGrid_LayerThicknessFacfor":'+RegexObj.Match[2]+',');
    CreateMeshSettingsNowJson_Memo.Lines.Strings[61-1] := ('"StudyAreaMesh_RectangleGrid_FirstLayerThickness":'+RegexObj.Match[3]+',');
    //--------------------------------------------------------------------------
    //--------------------------------------------------------------------------
    // CreateMeshSurfaceNodeSettings_Memo
    RegexObj.Expression := '^\s*([-+]?(\d+\.?\d*|\.\d+))\s*,\s*(null|([-+]?(\d+\.?\d*|\.\d+)))\s*,\s*(\d+)\s*$';
    for temp_i := 0 to CreateMeshSurfaceNodeSettings_Memo.Lines.Count-1 do
    begin
      //--
      if Trim(CreateMeshSurfaceNodeSettings_Memo.Lines.Strings[temp_i]) = '' then
      begin
        // 錯誤的資料內容
        temp_str := '錯誤:' + #13#10 +
          '地表節點(X,Z,電極索引)值不允許有空白行。請檢查。';
        Application.MessageBox(PChar(temp_str), '錯誤', 16);
        Exit;
      end;
      //--
      if not RegexObj.Exec(CreateMeshSurfaceNodeSettings_Memo.Lines.Strings[temp_i]) then
      begin
        // 錯誤的資料內容
        temp_str := '錯誤:' + #13#10 +
          '地表節點(X,Z,電極索引)值錯誤。請檢查。';
        Application.MessageBox(PChar(temp_str), '錯誤', 16);
        Exit;
      end;
      if temp_i = CreateMeshSurfaceNodeSettings_Memo.Lines.Count-1 then
      begin
        CreateMeshSettingsNowJson_Memo.Lines.Add('['+Trim(CreateMeshSurfaceNodeSettings_Memo.Lines.Strings[temp_i])+']');
      end
      else
      begin
        CreateMeshSettingsNowJson_Memo.Lines.Add('['+Trim(CreateMeshSurfaceNodeSettings_Memo.Lines.Strings[temp_i])+'],');
      end;
      //--
    end;
    CreateMeshSettingsNowJson_Memo.Lines.Add('],');
    //--------------------------------------------------------------------------
    //--------------------------------------------------------------------------
    // CreateMeshPaddingMeshLeftSettings_Edit
    RegexObj.Expression := '^\s*(\d+)\s*,\s*(\d*\.?\d+)\s*,\s*([-+]?(\d+\.?\d*|\.\d+))\s*$';
    if not RegexObj.Exec(CreateMeshPaddingMeshLeftSettings_Edit.Text) then
    begin
      // 錯誤的資料內容
      temp_str := '錯誤:' + #13#10 +
        '附加區域左側網格設定(層數,寬度因子,首層寬度)值錯誤。請檢查。';
      Application.MessageBox(PChar(temp_str), '錯誤', 16);
      Exit;
    end;
    if StrToFloat(RegexObj.Match[1]) <= 0 then
    begin
      temp_str := '錯誤:' + #13#10 +
        '附加區域左側網格設定(層數,寬度因子,首層寬度)值第1個數字必須為正整數。';
      Application.MessageBox(PChar(temp_str), '錯誤', 16);
      Exit;
    end;
    if StrToFloat(RegexObj.Match[2]) <= 0 then
    begin
      temp_str := '錯誤:' + #13#10 +
        '附加區域左側網格設定(層數,寬度因子,首層寬度)值第2個數字必須為正數。';
      Application.MessageBox(PChar(temp_str), '錯誤', 16);
      Exit;
    end;
    CreateMeshSettingsNowJson_Memo.Lines.Add('"PaddingMesh_GridStyle_Readme":"附加區域的網格類型。目前只支援「Rectangle」，就是指以矩形為基礎的網格類型。",');
    CreateMeshSettingsNowJson_Memo.Lines.Add('"PaddingMesh_GridStyle":"Rectangle",');
    CreateMeshSettingsNowJson_Memo.Lines.Add('"PaddingMesh_RectangleGrid_LeftPaddingLayerCount_Readme":"附加區域左側要追加的網格層數。就是指-X方向追加的地層數量。此值必須是0或是正整數。",');
    CreateMeshSettingsNowJson_Memo.Lines.Add('"PaddingMesh_RectangleGrid_LeftPaddingLayerCount":'+RegexObj.Match[1]+',');
    CreateMeshSettingsNowJson_Memo.Lines.Add('"PaddingMesh_RectangleGrid_LeftPaddingLayerWidthFacfor_Readme":"附加區域左側的地層寬度控制因子。此值必須是正數。1表示寬度不變化，0.9表示寬度變小為原本的90%，1.1表示寬度變大為原本的110%。",');
    CreateMeshSettingsNowJson_Memo.Lines.Add('"PaddingMesh_RectangleGrid_LeftPaddingLayerWidthFacfor":'+RegexObj.Match[2]+',');
    CreateMeshSettingsNowJson_Memo.Lines.Add('"PaddingMesh_RectangleGrid_FirstLeftPaddingLayerWidth_Readme":"附加區域左側的第一層寬度。單位:[m]。此值小於等於0的時候，表示自動使用最靠近的一層搭配厚度因子計算出第一層寬度，否則直接使用指定數值為第一層寬度。",');
    CreateMeshSettingsNowJson_Memo.Lines.Add('"PaddingMesh_RectangleGrid_FirstLeftPaddingLayerWidth":'+RegexObj.Match[3]+',');
    //--------------------------------------------------------------------------
    //--------------------------------------------------------------------------
    // CreateMeshPaddingMeshRightSettings_Edit
    RegexObj.Expression := '^\s*(\d+)\s*,\s*(\d*\.?\d+)\s*,\s*([-+]?(\d+\.?\d*|\.\d+))\s*$';
    if not RegexObj.Exec(CreateMeshPaddingMeshRightSettings_Edit.Text) then
    begin
      // 錯誤的資料內容
      temp_str := '錯誤:' + #13#10 +
        '附加區域右側網格設定(層數,寬度因子,首層寬度)值錯誤。請檢查。';
      Application.MessageBox(PChar(temp_str), '錯誤', 16);
      Exit;
    end;
    if StrToFloat(RegexObj.Match[1]) <= 0 then
    begin
      temp_str := '錯誤:' + #13#10 +
        '附加區域右側網格設定(層數,寬度因子,首層寬度)值第1個數字必須為正整數。';
      Application.MessageBox(PChar(temp_str), '錯誤', 16);
      Exit;
    end;
    if StrToFloat(RegexObj.Match[2]) <= 0 then
    begin
      temp_str := '錯誤:' + #13#10 +
        '附加區域右側網格設定(層數,寬度因子,首層寬度)值第2個數字必須為正數。';
      Application.MessageBox(PChar(temp_str), '錯誤', 16);
      Exit;
    end;
    CreateMeshSettingsNowJson_Memo.Lines.Add('"PaddingMesh_RectangleGrid_RightPaddingLayerCount_Readme":"附加區域右側要追加的網格層數。就是指+X方向追加的地層數量。此值必須是0或是正整數。",');
    CreateMeshSettingsNowJson_Memo.Lines.Add('"PaddingMesh_RectangleGrid_RightPaddingLayerCount":'+RegexObj.Match[1]+',');
    CreateMeshSettingsNowJson_Memo.Lines.Add('"PaddingMesh_RectangleGrid_RightPaddingLayerWidthFacfor_Readme":"附加區域右側的地層寬度控制因子。此值必須是正數。1表示寬度不變化，0.9表示寬度變小為原本的90%，1.1表示寬度變大為原本的110%。",');
    CreateMeshSettingsNowJson_Memo.Lines.Add('"PaddingMesh_RectangleGrid_RightPaddingLayerWidthFacfor":'+RegexObj.Match[2]+',');
    CreateMeshSettingsNowJson_Memo.Lines.Add('"PaddingMesh_RectangleGrid_FirstRightPaddingLayerWidth_Readme":"附加區域右側的第一層寬度。單位:[m]。此值小於等於0的時候，表示自動使用最靠近的一層搭配厚度因子計算出第一層寬度，否則直接使用指定數值為第一層寬度。",');
    CreateMeshSettingsNowJson_Memo.Lines.Add('"PaddingMesh_RectangleGrid_FirstRightPaddingLayerWidth":'+RegexObj.Match[3]+',');
    //--------------------------------------------------------------------------
    //--------------------------------------------------------------------------
    // CreateMeshPaddingMeshBottomSettings_Edit
    RegexObj.Expression := '^\s*(\d+)\s*,\s*(\d*\.?\d+)\s*,\s*([-+]?(\d+\.?\d*|\.\d+))\s*$';
    if not RegexObj.Exec(CreateMeshPaddingMeshBottomSettings_Edit.Text) then
    begin
      // 錯誤的資料內容
      temp_str := '錯誤:' + #13#10 +
        '附加區域下方網格設定(層數,寬度因子,首層寬度)值錯誤。請檢查。';
      Application.MessageBox(PChar(temp_str), '錯誤', 16);
      Exit;
    end;
    if StrToFloat(RegexObj.Match[1]) <= 0 then
    begin
      temp_str := '錯誤:' + #13#10 +
        '附加區域下方網格設定(層數,寬度因子,首層寬度)值第1個數字必須為正整數。';
      Application.MessageBox(PChar(temp_str), '錯誤', 16);
      Exit;
    end;
    if StrToFloat(RegexObj.Match[2]) <= 0 then
    begin
      temp_str := '錯誤:' + #13#10 +
        '附加區域下方網格設定(層數,寬度因子,首層寬度)值第2個數字必須為正數。';
      Application.MessageBox(PChar(temp_str), '錯誤', 16);
      Exit;
    end;
    CreateMeshSettingsNowJson_Memo.Lines.Add('"PaddingMesh_RectangleGrid_BottomPaddingLayerCount_Readme":"附加區域下方的網格層數。是指Z方向的地層數量。此值必須是0或正整數。",');
    CreateMeshSettingsNowJson_Memo.Lines.Add('"PaddingMesh_RectangleGrid_BottomPaddingLayerCount":'+RegexObj.Match[1]+',');
    CreateMeshSettingsNowJson_Memo.Lines.Add('"PaddingMesh_RectangleGrid_BottomPaddingLayerThicknessFacfor_Readme":"附加區域右側的地層寬度控制因子。此值必須是正數。1表示寬度不變化，0.9表示寬度變小為原本的90%，1.1表示寬度變大為原本的110%。",');
    CreateMeshSettingsNowJson_Memo.Lines.Add('"PaddingMesh_RectangleGrid_BottomPaddingLayerThicknessFacfor":'+RegexObj.Match[2]+',');
    CreateMeshSettingsNowJson_Memo.Lines.Add('"PaddingMesh_RectangleGrid_FirstBottomPaddingLayerThickness_Readme":"附加區域的第一層厚度。單位:[m]。此值小於等於0的時候，表示自動使用最靠近的一層搭配厚度因子計算出第一層厚度，否則直接使用指定數值為第一層厚度。",');
    CreateMeshSettingsNowJson_Memo.Lines.Add('"PaddingMesh_RectangleGrid_FirstBottomPaddingLayerThickness":'+RegexObj.Match[3]+',');
    //--------------------------------------------------------------------------
    //--------------------------------------------------------------------------
    // CreateMeshModifyMeshResistivitySettings_Memo
    CreateMeshSettingsNowJson_Memo.Lines.Add('"ModifyMeshResistivity_Readme":"依序填入多則修改範圍，將依照規則順序修改模型。",');
    CreateMeshSettingsNowJson_Memo.Lines.Add('"ModifyMeshResistivity_Readme2":"x_min[m],x_max[m],z_min[m],z_max[m],New_Resistivity[ohm-m]",');
    CreateMeshSettingsNowJson_Memo.Lines.Add('"ModifyMeshResistivity":[');
    RegexObj.Expression := '^\s*([-+]?(\d+\.?\d*|\.\d+))\s*,\s*([-+]?(\d+\.?\d*|\.\d+))\s*,\s*([-+]?(\d+\.?\d*|\.\d+))\s*,\s*([-+]?(\d+\.?\d*|\.\d+))\s*,\s*(\d*\.?\d+)\s*$';
    for temp_i := 0 to CreateMeshModifyMeshResistivitySettings_Memo.Lines.Count-1 do
    begin
      //--
      if Trim(CreateMeshModifyMeshResistivitySettings_Memo.Lines.Strings[temp_i]) = '' then
      begin
        // 錯誤的資料內容
        temp_str := '錯誤:' + #13#10 +
          '電阻率規則(xmin,xmax,zmin,zmax,newRes)值不允許有空白行。請檢查。';
        Application.MessageBox(PChar(temp_str), '錯誤', 16);
        Exit;
      end;
      //--
      if not RegexObj.Exec(CreateMeshModifyMeshResistivitySettings_Memo.Lines.Strings[temp_i]) then
      begin
        // 錯誤的資料內容
        temp_str := '錯誤:' + #13#10 +
          '電阻率規則(xmin,xmax,zmin,zmax,newRes):值錯誤。請檢查。';
        Application.MessageBox(PChar(temp_str), '錯誤', 16);
        Exit;
      end;
      if temp_i = CreateMeshModifyMeshResistivitySettings_Memo.Lines.Count-1 then
      begin
        CreateMeshSettingsNowJson_Memo.Lines.Add('['+Trim(CreateMeshModifyMeshResistivitySettings_Memo.Lines.Strings[temp_i])+']');
      end
      else
      begin
        CreateMeshSettingsNowJson_Memo.Lines.Add('['+Trim(CreateMeshModifyMeshResistivitySettings_Memo.Lines.Strings[temp_i])+'],');
      end;
      //--
    end;
    CreateMeshSettingsNowJson_Memo.Lines.Add(']');
    CreateMeshSettingsNowJson_Memo.Lines.Add('}');
    //--------------------------------------------------------------------------
    // 儲存預設JSON檔案
    ForceDirectories('Input_ERTMaker_CreateAndModifyMesh');
    CreateMeshSettingsNowJson_Memo.Lines.SaveToFile('Input_ERTMaker_CreateAndModifyMesh/CreateAndModifyMeshSettings.json',TEncoding.UTF8);
    //--------------------------------------------------------------------------
    //--------------------------------------------------------------------------
    // 等待1秒，讓刪除檔案有時間完成
    temp_StartTime := GetTickCount; // 獲取當前系統時間(ms)
    while(GetTickCount() - temp_StartTime < 1000*1) do//1000*1=等1秒
    begin
      Application.ProcessMessages;
      Sleep(1);//休眠1msend;
    end;
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
    if not FileExists('ERTMaker_CreateAndModifyMesh_v20251031a.cpython-312.pyc') then
    begin
      temp_str := '錯誤:' + #13#10 +
        '外部Python程式不存在。';
      Application.MessageBox(PChar(temp_str), '錯誤', 16);
      Exit;
    end;
    //--------------------------------------------------------------------------
    //--------------------------------------------------------------------------
    CreateMeshSettingsCmdLog_Memo.Lines.Clear;
    // 使用 CreateMesh_AsyncProcess 運行外部程式
    CreateMesh_AsyncProcess.Executable:='cmd.exe';
    CreateMesh_AsyncProcess.Parameters.Clear;
    CreateMesh_AsyncProcess.Parameters.Add('/c');
    CreateMesh_AsyncProcess.Parameters.Add('.\PythonEnv\Python.exe -u ERTMaker_CreateAndModifyMesh_v20251031a.cpython-312.pyc');
    CreateMesh_AsyncProcess.Options:=[poUsePipes] + [poNoConsole];
    CreateMesh_AsyncProcess.Execute;
    // 禁用元件，等背景運作完再從事件重新啟用按鈕
    CreateMeshRun_ToolButton.Enabled:=False;
    CreateMeshParameters_GroupBox.Enabled:=False;
    ForwardModelingParameters_GroupBox.Enabled:=False;
    //--------------------------------------------------------------------------
  finally
    RegexObj.Free; // 釋放 TRegExpr 物件
  end;
  //--------------------------------------------------------------------------
end;
```
+ 2.2 去修改「CreateMesh_AsyncProcess」的「Event」頁面下「OnTerminate」為如下程式碼。
```pascal
procedure TForm1.CreateMesh_AsyncProcessTerminate(Sender: TObject);
var
  temp_str: AnsiString;
begin
  //--------------------------------------------------------------------------
  // 啟用元件
  CreateMeshRun_ToolButton.Enabled:=True;
  CreateMeshParameters_GroupBox.Enabled:=True;
  ForwardModelingParameters_GroupBox.Enabled:=True;
  //--------------------------------------------------------------------------
  //--------------------------------------------------------------------------
  // 更新狀態列
  //--
  if CreateMeshSettingsCmdLog_Memo.Lines.Count=0 Then
  begin
    StatusBar1.Panels[0].Text:='建立模型網格...異常結束!';
    Exit;
  end;
  //--
  // 檢查結果圖片
  temp_str:=CreateMeshSettingsNowJson_Memo.Lines.Strings[41-1];
  temp_str:=StringReplace(temp_str, '"OutputFile06_BasicMeshPNG_FileName":"', '', [rfReplaceAll]);
  temp_str:=StringReplace(temp_str, '",', '', [rfReplaceAll]);
  if FileExists(temp_str) then
  begin
    CreateMeshPreview_Image.Picture.LoadFromFile(temp_str);
    StatusBar1.Panels[0].Text:='建立模型網格...完成!';
    // 成功提示
    temp_str := '成功:' + #13#10 +
      '建立模型網格...完成!';
    Application.MessageBox(PChar(temp_str), '提示', 64);
    Exit;
  end
  else
  begin
    StatusBar1.Panels[0].Text:='建立模型網格...運作異常，請檢查紀錄!';
    Exit;
  end;
  //--------------------------------------------------------------------------
end;
```
+ 2.3 修改「ForwardModelingSettingsDefaultJson_Memo」。
  + 2.3.1 設定「Lines」為以下文字。
  ```
  {
  "SimulateForTimeSeriesSettings_Version":"v20251031a",
  "SimulateForTimeSeriesSettings_Author":"HsiupoYeh",
  "InputFile01_MeshVTK_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
  "InputFile01_MeshVTK_FileName":"Input_ERTMaker_SimulateForTimeSeries/XP1_SyntheticModel.vtk",
  "InputFile02_MeshBCMarkersJSON_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
  "InputFile02_MeshBCMarkersJSON_FileName":"Input_ERTMaker_SimulateForTimeSeries/XP1_SyntheticModelBCMarkers.json",
  "InputFile03_OHM_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
  "InputFile03_OHM_FileName":"Input_ERTMaker_SimulateForTimeSeries/XP1_SyntheticModel.ohm",
  "InputFile04_CurrentMode_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
  "InputFile04_CurrentMode_FileName":"Input_ERTMaker_SimulateForTimeSeries/Configs/v299_S64NCurrentMode/Current Mode.csv",
  "MeshPNG_DPI_Readme":"圖片輸出的DPI，建議:100。",
  "MeshPNG_DPI":100,
  "MeshPNG_Width_Readme":"圖片輸出的寬度，建議:1280。",
  "MeshPNG_Width":1280,
  "MeshPNG_Height_Readme":"圖片輸出的寬度，建議:720。",
  "MeshPNG_Height":720,
  "MeshPNG_Title_Readme":"圖片的標題。",
  "MeshPNG_Title":"XP1 Synthetic Model",
  "MeshPNG_ColorBarResistivityMin_Readme":"電阻率色階下限，線性數值，必為正數。建議:1。",
  "MeshPNG_ColorBarResistivityMin":1,
  "MeshPNG_ColorBarResistivityMax_Readme":"電阻率色階下限，線性數值，必為正數。建議:10000。",
  "MeshPNG_ColorBarResistivityMax":10000,
  "OutputFolderPath_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。務必要使用「/」在結尾，表示資料夾。",
  "OutputFolderPath":"Output_ERTMaker_SimulateForTimeSeries/",
  "Output_MainFileName_Readme":"會產生很多檔案，這些檔案的主要檔名。",
  "Output_MainFileName":"XP1_SyntheticModel",
  "Output_PNG_Enable_Readme":"是否啟用輸出PNG。若要啟用請填入字串Yes，若不啟用請填入字串No。輸出PNG每張圖約耗時",
  "Output_PNG_Enable":"Yes",
  "ElectrodeIndexAB_TxVoltageMax_Readme":"AB電極的發射器最大電壓，單位[V]",
  "ElectrodeIndexAB_TxVoltageMax":400,
  "ElectrodeIndexAB_TxCurrentMax_Readme":"AB電極的發射器最大電流，單位[A]",
  "ElectrodeIndexAB_TxCurrentMax":0.5,
  "ElectrodeIndexAB_Resistance_Readme":"AB電極的接地電阻，單位[Ohm]",
  "ElectrodeIndexAB_Resistance":1000
  }
  ```
+ 2.4 拖拉一個「System>TAsyncProcess」到「Form1」中。預設名稱會是「AsyncProcess1」，修改「Name」為「ForwardModeling_AsyncProcess」。
+ 2.5 去修改「ForwardModeling_AsyncProcess」的「Event」頁面下「OnReadData」為如下程式碼。
```pascal
procedure TForm1.ForwardModeling_AsyncProcessReadData(Sender: TObject);
var
  temp_Buffer:string='';
  temp_BytesAvailable:DWord;
begin
  //--------------------------------------------------------------------------
  // 顯示外部cmd.exe運行內容
  temp_BytesAvailable:=ForwardModeling_AsyncProcess.Output.NumBytesAvailable;
  if temp_BytesAvailable>0 Then begin
    setlength(temp_Buffer,temp_BytesAvailable);
    ForwardModeling_AsyncProcess.Output.Read(temp_Buffer[1],temp_BytesAvailable);
    temp_Buffer := StringReplace(temp_Buffer, #13#10, #10, [rfReplaceAll]);
    temp_Buffer := StringReplace(temp_Buffer, #10, #13#10, [rfReplaceAll]);
    ForwardModelingSettingsCmdLog_Memo.Lines.Add(WinCPToUTF8(temp_Buffer));
  end;
  //--------------------------------------------------------------------------
end;
```
+ 2.6 去修改「ForwardModeling_AsyncProcess」的「Event」頁面下「OnTerminate」為如下程式碼。
```pascal
procedure TForm1.ForwardModeling_AsyncProcessTerminate(Sender: TObject);
var
  temp_str: AnsiString;
begin
  //--------------------------------------------------------------------------
  // 啟用元件
  ForwardModelingRun_ToolButton.Enabled:=True;
  CreateMeshParameters_GroupBox.Enabled:=True;
  ForwardModelingParameters_GroupBox.Enabled:=True;
  //--------------------------------------------------------------------------
  //--------------------------------------------------------------------------
  // 更新狀態列
  //--
  if ForwardModelingSettingsCmdLog_Memo.Lines.Count=0 Then
  begin
    StatusBar1.Panels[0].Text:='運行順推模擬...異常結束!';
    ForwardModeling_Timer.Enabled:=False;
    StatusBar1.Panels[4].Text:='自動播放: 停用';
    Exit;
  end;
  //--
  // 檢查結果csv
  temp_str:=ForwardModelingSettingsNowJson_Memo.Lines.Strings[24]+ForwardModelingSettingsNowJson_Memo.Lines.Strings[26];
  temp_str:=StringReplace(temp_str, '"OutputFolderPath":"', '', [rfReplaceAll]);
  temp_str:=StringReplace(temp_str, '","Output_MainFileName":"', '', [rfReplaceAll]);
  temp_str:=StringReplace(temp_str, '",', '', [rfReplaceAll]);
  temp_str:=temp_str+'_CurrentFlowLinesAB.v299S.csv';
  if FileExists(temp_str) then
  begin
    StatusBar1.Panels[0].Text:='運行順推模擬...完成!';
    // 成功提示
    temp_str := '成功:' + #13#10 +
      '運行順推模擬...完成!';
    Application.MessageBox(PChar(temp_str), '提示', 64);
    Exit;
  end
  else
  begin
    StatusBar1.Panels[0].Text:='運行順推模擬...運作異常，請檢查紀錄!';
    ForwardModeling_Timer.Enabled:=False;
    StatusBar1.Panels[4].Text:='自動播放: 停用';
    Exit;
  end;
  //--------------------------------------------------------------------------
end;
```
+ 2.7 拖拉一個「System>TTimer」到「Form1」中。預設名稱會是「Timer1」，修改「Name」為「ForwardModeling_Timer」。
+ 2.8 去修改「ForwardModeling_Timer」的「Event」頁面下「OnTimer」為如下程式碼。
```pascal
procedure TForm1.ForwardModeling_TimerTimer(Sender: TObject);
var
  temp_str: AnsiString;
begin
  if StrToInt(StatusBar1.Panels[1].Text) = 0 then
  begin
    //--------------------------------------------------------------------------
    // 載入圖片
    temp_str:=ForwardModelingSettingsNowJson_Memo.Lines.Strings[24]+ForwardModelingSettingsNowJson_Memo.Lines.Strings[26];
    temp_str:=StringReplace(temp_str, '"OutputFolderPath":"', '', [rfReplaceAll]);
    temp_str:=StringReplace(temp_str, '","Output_MainFileName":"', '', [rfReplaceAll]);
    temp_str:=StringReplace(temp_str, '",', '', [rfReplaceAll]);
    temp_str:=temp_str;
    if FileExists(temp_str+'_electric_potential.npy') then
    begin
      ForwardModelingPreview_Image.Picture.LoadFromFile(temp_str+'.png');
      if not ForwardModelingOutputPNGEnable_CheckBox.Checked then
      begin
        ForwardModeling_Timer.Enabled:=False;
        StatusBar1.Panels[4].Text:='自動播放: 停用';
      end
      else
      begin
        StatusBar1.Panels[1].Text:='1';
        Exit;
      end;
    end;
    //--------------------------------------------------------------------------
  end;
  //--
  if StrToInt(StatusBar1.Panels[1].Text) > 0 then
  begin
    //--------------------------------------------------------------------------
    // 載入圖片
    temp_str:=ForwardModelingSettingsNowJson_Memo.Lines.Strings[24]+ForwardModelingSettingsNowJson_Memo.Lines.Strings[26];
    temp_str:=StringReplace(temp_str, '"OutputFolderPath":"', '', [rfReplaceAll]);
    temp_str:=StringReplace(temp_str, '","Output_MainFileName":"', '', [rfReplaceAll]);
    temp_str:=StringReplace(temp_str, '",', '', [rfReplaceAll]);
    temp_str:=temp_str;
    // 分開檔案的.v299S.csv寫入後再讀取PNG到預覽區
    if FileExists(temp_str+'_CurrentFlowLinesAB_'+Format('%.4d', [StrToInt(StatusBar1.Panels[1].Text)])+'.v299S.csv') and FileExists(temp_str+'_electric_potential.npy') then
    begin
      ForwardModelingPreview_Image.Picture.LoadFromFile(temp_str+'_CurrentFlowLinesAB_'+Format('%.4d', [StrToInt(StatusBar1.Panels[1].Text)])+'.png');
      StatusBar1.Panels[1].Text:=IntToStr(StrToInt(StatusBar1.Panels[1].Text)+1);
    end
    else if ForwardModelingRun_ToolButton.Enabled then
    begin
      ForwardModeling_Timer.Enabled:=False;
      StatusBar1.Panels[4].Text:='自動播放: 停用';
    end;;
    //--------------------------------------------------------------------------
  end;
  //--
end;
```
+ 2.9 去修改「ForwardModelingRun_ToolButton」的「Event」頁面下「OnClick」為如下程式碼。
```pascal
procedure TForm1.ForwardModelingRun_ToolButtonClick(Sender: TObject);
var
  temp_i: Integer;
  RegexObj: TRegExpr;
  temp_str: AnsiString;
  temp_StartTime: DWord;
begin
  ForwardModelingSettingsNowJson_Memo.Clear;
  ForwardModelingPreview_Image.Picture.Clear;
  //--
  StatusBar1.Panels[0].Text:='運行順推模擬...請稍後!';
  StatusBar1.Panels[1].Text:='0';
  //--------------------------------------------------------------------------
  ForwardModelingSettingsNowJson_Memo.Lines:=ForwardModelingSettingsDefaultJson_Memo.Lines;
  //--
  if ForwardModelingOutputPNGEnable_CheckBox.Checked then
  begin
    ForwardModelingSettingsNowJson_Memo.Lines.Strings[1] := '"SimulateForTimeSeriesSettings_Version":"v20251101a",';
  end
  else
  begin
    ForwardModelingSettingsNowJson_Memo.Lines.Strings[1] := '"SimulateForTimeSeriesSettings_Version":"v20251031a",';
  end;
  //--
  temp_str:='Output_ERTMaker_CreateAndModifyMesh/'+CreateMeshModelName_Edit.Text+'_SyntheticModel.vtk';
  if FileExists(temp_str) then
  begin
     ForwardModelingSettingsNowJson_Memo.Lines.Strings[4] := '"InputFile01_MeshVTK_FileName":"'+temp_str+'",';
  end
  else
  begin
    temp_str := '錯誤:' + #13#10 +
      'vtk檔案不存在。';
    Application.MessageBox(PChar(temp_str), '錯誤', 16);
    Exit;
  end;
  //--
  temp_str:='Output_ERTMaker_CreateAndModifyMesh/'+CreateMeshModelName_Edit.Text+'_SyntheticModelBCMarkers.json';
  if FileExists(temp_str) then
  begin
     ForwardModelingSettingsNowJson_Memo.Lines.Strings[6] := '"InputFile02_MeshBCMarkersJSON_FileName":"'+temp_str+'",';
  end
  else
  begin
    temp_str := '錯誤:' + #13#10 +
      'json檔案不存在。';
    Application.MessageBox(PChar(temp_str), '錯誤', 16);
    Exit;
  end;
  //--
  temp_str:='Output_ERTMaker_CreateAndModifyMesh/'+CreateMeshModelName_Edit.Text+'_SyntheticModel.ohm';
  if FileExists(temp_str) then
  begin
     ForwardModelingSettingsNowJson_Memo.Lines.Strings[8] := '"InputFile03_OHM_FileName":"'+temp_str+'",';
     ForwardModelingSettingsCmdLog_Memo.Lines.LoadFromFile(temp_str);
  end
  else
  begin
    temp_str := '錯誤:' + #13#10 +
      'ohm檔案不存在。';
    Application.MessageBox(PChar(temp_str), '錯誤', 16);
    Exit;
  end;
  //--
  if ForwardModelingCurrentMode_CheckBox.Checked then
  begin
    for temp_i := 0 to ForwardModelingSettingsCmdLog_Memo.Lines.Count - 1 do
    begin
      if Pos('#x z', ForwardModelingSettingsCmdLog_Memo.Lines[temp_i]) = 1 then
      begin
        // 找到了！
        ForwardModelingCurrentMode_ComboBox.ItemIndex:=-1;
        if ForwardModelingSettingsCmdLog_Memo.Lines.Strings[temp_i-1].ToInteger > 64 then
        begin
          ForwardModelingSettingsNowJson_Memo.Lines.Strings[10] := '"InputFile04_CurrentMode_FileName":"Input_ERTMaker_SimulateForTimeSeries/Configs/v299_S64NCurrentMode/Current Mode.csv",';
        end
        else
        begin
          ForwardModelingSettingsNowJson_Memo.Lines.Strings[10] := '"InputFile04_CurrentMode_FileName":"Input_ERTMaker_SimulateForTimeSeries/Configs/v299_S'+ForwardModelingSettingsCmdLog_Memo.Lines.Strings[temp_i-1]+'NCurrentMode/Current Mode.csv",';
        end;
        Break;          // 立即跳出迴圈，因為找到了第一個目標
      end;
    end;
  end
  else
  begin
    ForwardModelingSettingsNowJson_Memo.Lines.Strings[10] := '"InputFile04_CurrentMode_FileName":"Input_ERTMaker_SimulateForTimeSeries/Configs/v299_'+ForwardModelingCurrentMode_ComboBox.Text+'CurrentMode/Current Mode.csv",';
  end;
  //--
  ForwardModelingSettingsNowJson_Memo.Lines.Strings[18] := '"MeshPNG_Title":"'+CreateMeshModelName_Edit.Text+' Synthetic Model",';
  //--
  ForwardModelingSettingsNowJson_Memo.Lines.Strings[26] := '"Output_MainFileName":"'+CreateMeshModelName_Edit.Text+'_SyntheticModel",';
  //--
  if ForwardModelingOutputPNGEnable_CheckBox.Checked then
  begin
    ForwardModelingSettingsNowJson_Memo.Lines.Strings[28] := '"Output_PNG_Enable":"Yes",';
  end
  else
  begin
    ForwardModelingSettingsNowJson_Memo.Lines.Strings[28] := '"Output_PNG_Enable":"No",';
  end;
  //--
  RegexObj := TRegExpr.Create;
  try
    //--------------------------------------------------------------------------
    // CreateMeshColorbarMinMax_Edit
    RegexObj.Expression := '^\s*([-+]?(\d+\.?\d*|\.\d+))\s*,\s*([-+]?(\d+\.?\d*|\.\d+))\s*$';
    if not RegexObj.Exec(CreateMeshColorbarMinMax_Edit.Text) then
    begin
      // 錯誤的資料內容
      temp_str := '錯誤:' + #13#10 +
        '電阻率色階數值錯誤。請檢查。';
      Application.MessageBox(PChar(temp_str), '錯誤', 16);
      Exit;
    end;
    if StrToFloat(RegexObj.Match[1]) >= StrToFloat(RegexObj.Match[3]) then
    begin
      temp_str := '錯誤:' + #13#10 +
        '電阻率色階最小值必須小於最大值。';
      Application.MessageBox(PChar(temp_str), '錯誤', 16);
      Exit;
    end;
    ForwardModelingSettingsNowJson_Memo.Lines.Strings[20] := ('"MeshPNG_ColorBarResistivityMin":'+RegexObj.Match[1]+',');
    ForwardModelingSettingsNowJson_Memo.Lines.Strings[22] := ('"MeshPNG_ColorBarResistivityMax":'+RegexObj.Match[3]+',');
    //--------------------------------------------------------------------------
    //--------------------------------------------------------------------------
    // ForwardModelingElectrodeIndexABTxVoltageMax_Edit
    RegexObj.Expression := '^[-+]?(\d+(\.\d*)?|\.\d+)$';
    if not RegexObj.Exec(ForwardModelingElectrodeIndexABTxVoltageMax_Edit.Text) then
    begin
      // 錯誤的資料內容
      temp_str := '錯誤:' + #13#10 +
        'AB電極的發射器最大電壓數值錯誤。請檢查。';
      Application.MessageBox(PChar(temp_str), '錯誤', 16);
      Exit;
    end;
    ForwardModelingSettingsNowJson_Memo.Lines.Strings[30] := '"ElectrodeIndexAB_TxVoltageMax":'+ForwardModelingElectrodeIndexABTxVoltageMax_Edit.Text+',';
    //--------------------------------------------------------------------------
    //--------------------------------------------------------------------------
    // ForwardModelingElectrodeIndexABTxCurrentMax_Edit
    RegexObj.Expression := '^[-+]?(\d+(\.\d*)?|\.\d+)$';
    if not RegexObj.Exec(ForwardModelingElectrodeIndexABTxCurrentMax_Edit.Text) then
    begin
      // 錯誤的資料內容
      temp_str := '錯誤:' + #13#10 +
        'AB電極的發射器最大電流數值錯誤。請檢查。';
      Application.MessageBox(PChar(temp_str), '錯誤', 16);
      Exit;
    end;
    ForwardModelingSettingsNowJson_Memo.Lines.Strings[32] := '"ElectrodeIndexAB_TxCurrentMax":'+ForwardModelingElectrodeIndexABTxCurrentMax_Edit.Text+',';
    //--------------------------------------------------------------------------
    //--------------------------------------------------------------------------
    // ForwardModelingElectrodeIndexABResistance_Edit
    RegexObj.Expression := '^[-+]?(\d+(\.\d*)?|\.\d+)$';
    if not RegexObj.Exec(ForwardModelingElectrodeIndexABResistance_Edit.Text) then
    begin
      // 錯誤的資料內容
      temp_str := '錯誤:' + #13#10 +
        'AB電極的接地電阻數值錯誤。請檢查。';
      Application.MessageBox(PChar(temp_str), '錯誤', 16);
      Exit;
    end;
    ForwardModelingSettingsNowJson_Memo.Lines.Strings[34] := '"ElectrodeIndexAB_Resistance":'+ForwardModelingElectrodeIndexABResistance_Edit.Text;
    //--------------------------------------------------------------------------
    //--------------------------------------------------------------------------
    // 儲存預設JSON檔案
    ForceDirectories('Input_ERTMaker_SimulateForTimeSeries');
    ForwardModelingSettingsNowJson_Memo.Lines.SaveToFile('Input_ERTMaker_SimulateForTimeSeries/SimulateForTimeSeriesSettings.json',TEncoding.UTF8);
    //--------------------------------------------------------------------------
    //--------------------------------------------------------------------------
    // 刪除預覽png
    temp_str:=ForwardModelingSettingsNowJson_Memo.Lines.Strings[24]+ForwardModelingSettingsNowJson_Memo.Lines.Strings[26];
    temp_str:=StringReplace(temp_str, '"OutputFolderPath":"', '', [rfReplaceAll]);
    temp_str:=StringReplace(temp_str, '","Output_MainFileName":"', '', [rfReplaceAll]);
    temp_str:=StringReplace(temp_str, '",', '', [rfReplaceAll]);
    temp_str:=temp_str;
    if FileExists(temp_str+'.png') then
    begin
      DeleteFile(PChar(temp_str));
      StatusBar1.Panels[0].Text:='建立模型網格...已刪除舊的預覽圖片，請稍後!';
    end;
    // 刪除結果npy'
    if FileExists(temp_str+'_electric_potential.npy') then
    begin
      DeleteFile(PChar(temp_str));
      StatusBar1.Panels[0].Text:='建立模型網格...已刪除舊的npy，請稍後!';
    end;
    // 刪除結果csv
    if FileExists(temp_str+'_CurrentFlowLinesAB.v299S.csv') then
    begin
      DeleteFile(PChar(temp_str));
      StatusBar1.Panels[0].Text:='建立模型網格...已刪除舊的csv，請稍後!';
    end;
    //--------------------------------------------------------------------------
    //--------------------------------------------------------------------------
    // 等待1秒，讓刪除檔案有時間完成
    temp_StartTime := GetTickCount; // 獲取當前系統時間(ms)
    while(GetTickCount() - temp_StartTime < 1000*1) do//1000*1=等1秒
    begin
      Application.ProcessMessages;
      Sleep(1);//休眠1msend;
    end;
    //--------------------------------------------------------------------------
    //--------------------------------------------------------------------------
    ForwardModelingSettingsCmdLog_Memo.Lines.Clear;
    // 使用 CreateMesh_AsyncProcess 運行外部程式
    ForwardModeling_AsyncProcess.Executable:='cmd.exe';
    ForwardModeling_AsyncProcess.Parameters.Clear;
    ForwardModeling_AsyncProcess.Parameters.Add('/c');
    if ForwardModelingOutputPNGEnable_CheckBox.Checked then
    begin
      ForwardModeling_AsyncProcess.Parameters.Add('.\PythonEnv\Python.exe -u ERTMaker_SimulateForTimeSeries_v20251101a.cpython-312.pyc');
    end
    else
    begin
      ForwardModeling_AsyncProcess.Parameters.Add('.\PythonEnv\Python.exe -u ERTMaker_SimulateForTimeSeries_v20251031a.cpython-312.pyc');
    end;
    ForwardModeling_AsyncProcess.Options:=[poUsePipes] + [poNoConsole];
    ForwardModeling_AsyncProcess.Execute;
    // 啟用Timer
    ForwardModeling_Timer.Enabled:=True;
    StatusBar1.Panels[4].Text:='自動播放: 啟用';
    // 禁用元件，等背景運作完再從事件重新啟用按鈕
    ForwardModelingRun_ToolButton.Enabled:=False;
    CreateMeshParameters_GroupBox.Enabled:=False;
    ForwardModelingParameters_GroupBox.Enabled:=False;
    //--------------------------------------------------------------------------
  finally
    RegexObj.Free; // 釋放 TRegExpr 物件
  end;
end;
```
+ 2.10 去修改「ForwardModelingOpenOutputFolder_ToolButton」的「Event」頁面下「OnClick」為如下程式碼。
```pascal
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
  // 記得在 uses 區塊中加入 Windows, ShellApi
  // 使用 ShellExecute 打開該資料夾
  ShellExecute(0, 'open', PChar(temp_str), nil, nil, SW_SHOWNORMAL);
end;     
```
