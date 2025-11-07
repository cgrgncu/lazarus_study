# R2MS_Lite_ERTMaker 開發筆記
+ 作者: HsiupoYeh  
+ 更新日期: 2025-11-06
+ 從 **R2MS_Lite_ERTMaker_v1.0(v20251105a).md** 開始，繼續更新基本功能。

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
  version_str := 'v1.0(v20251106a)';
  Current_Folder_Path := ExtractFilePath(Application.ExeName);
  //--------------------------------------------------------------------------
  //--------------------------------------------------------------------------
  // 表單標題追加版本
  Form1.Caption := Form1.Caption + ' ' + version_str;
  //--------------------------------------------------------------------------
end;      
```

### 寫功能
+ 1.1 去修改「CreateMeshSettingsDefaultJson_Memo」的「Lines」為以下文字。
```
{
"CreateAndModifyMeshSettings_Version":"v20251031a",
"CreateAndModifyMeshSettings_Author":"HsiupoYeh",
"TempFile01_BasicMeshPNG_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"TempFile01_BasicMeshPNG_FileName":"Temp/CreateMesh_01_BasicMesh.png",
"TempFile02_BasicMeshVTK_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"TempFile02_BasicMeshVTK_FileName":"Temp/CreateMesh_01_BasicMesh.vtk",
"TempFile03_BasicMeshPNG_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"TempFile03_BasicMeshPNG_FileName":"Temp/CreateMesh_02_BasicMeshSetMarker1.png",
"TempFile04_BasicMeshVTK_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"TempFile04_BasicMeshVTK_FileName":"Temp/CreateMesh_02_BasicMeshSetMarker1.vtk",
"TempFile05_BasicMeshPNG_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"TempFile05_BasicMeshPNG_FileName":"Temp/CreateMesh_03_BasicMeshSetMarker2.png",
"TempFile06_BasicMeshVTK_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"TempFile06_BasicMeshVTK_FileName":"Temp/CreateMesh_03_BasicMeshSetMarker2.vtk",
"TempFile07_BasicMeshPNG_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"TempFile07_BasicMeshPNG_FileName":"Temp/CreateMesh_04_BasicMeshSetBCMarker1.png",
"TempFile08_BasicMeshVTK_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"TempFile08_BasicMeshVTK_FileName":"Temp/CreateMesh_04_BasicMeshSetBCMarker1.vtk",
"TempFile09_BasicMeshPNG_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"TempFile09_BasicMeshPNG_FileName":"Temp/CreateMesh_05_BasicMeshSetBCMarker2.png",
"TempFile10_BasicMeshVTK_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"TempFile10_BasicMeshVTK_FileName":"Temp/CreateMesh_05_BasicMeshSetBCMarker2.vtk",
"TempFile11_BasicMeshPNG_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"TempFile11_BasicMeshPNG_FileName":"Temp/CreateMesh_06_BasicMeshSetTopo.png",
"TempFile12_BasicMeshPNG_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"TempFile12_BasicMeshPNG_FileName":"Temp/CreateMesh_07_BasicMeshSetTopoRes.png",
"TempFile13_BasicMeshVTK_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"TempFile13_BasicMeshVTK_FileName":"Temp/CreateMesh_07_BasicMeshSetTopoRes.vtk",
"OutputFile01_MeshBCMarkersJSON_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"OutputFile01_MeshBCMarkersJSON_FileName":"Output_ERTMaker_CreateAndModifyMesh/Demo_SyntheticModelBCMarkers.json",
"OutputFile02_MeshBCMarkersVTK_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"OutputFile02_MeshBCMarkersVTK_FileName":"Output_ERTMaker_CreateAndModifyMesh/Demo_SyntheticModel.vtk",
"OutputFile03_GEO_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"OutputFile03_GEO_FileName":"Output_ERTMaker_CreateAndModifyMesh/Demo_SyntheticModel.geo",
"OutputFile04_TRN_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"OutputFile04_TRN_FileName":"Output_ERTMaker_CreateAndModifyMesh/Demo_SyntheticModel.trn",
"OutputFile05_OHM_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"OutputFile05_OHM_FileName":"Output_ERTMaker_CreateAndModifyMesh/Demo_SyntheticModel.ohm",
"OutputFile06_BasicMeshPNG_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"OutputFile06_BasicMeshPNG_FileName":"Output_ERTMaker_CreateAndModifyMesh/Demo_SyntheticModel.png",
"OutputFile06_BasicMeshPNG_DPI_Readme":"圖片輸出的DPI，建議:100。",
"OutputFile06_BasicMeshPNG_DPI":100,
"OutputFile06_BasicMeshPNG_Width_Readme":"圖片輸出的寬度，建議:1280。",
"OutputFile06_BasicMeshPNG_Width":1280,
"OutputFile06_BasicMeshPNG_Height_Readme":"圖片輸出的寬度，建議:720。",
"OutputFile06_BasicMeshPNG_Height":720,
"OutputFile06_BasicMeshPNG_Title_Readme":"圖片的標題。",
"OutputFile06_BasicMeshPNG_Title":"XP1 Synthetic Model",
"OutputFile06_BasicMeshPNG_ColorBarResistivityMin_Readme":"電阻率色階下限，線性數值，必為正數。建議:1。",
"OutputFile06_BasicMeshPNG_ColorBarResistivityMin":1,
"OutputFile06_BasicMeshPNG_ColorBarResistivityMax_Readme":"電阻率色階下限，線性數值，必為正數。建議:10000。",
"OutputFile06_BasicMeshPNG_ColorBarResistivityMax":10000,
"StudyAreaMesh_GridStyle_Readme":"研究區域的網格類型。目前只支援「Rectangle」，就是指以矩形為基礎的網格類型。",
"StudyAreaMesh_GridStyle":"Rectangle",
"StudyAreaMesh_RectangleGrid_LayerCount_Readme":"研究區域的網格層數。就是指Z方向的地層數量。此值必須是正整數。",
"StudyAreaMesh_RectangleGrid_LayerCount":30,
"StudyAreaMesh_RectangleGrid_LayerThicknessFacfor_Readme":"研究區域的地層厚度控制因子。此值必須是正數。1表示厚度不變化，0.9表示厚度變小為原本的90%，1.1表示厚度變大為原本的110%。",
"StudyAreaMesh_RectangleGrid_LayerThicknessFacfor":1,
"StudyAreaMesh_RectangleGrid_FirstLayerThickness_Readme":"研究區域的第一層厚度。單位:[m]。此值必須是正數。",
"StudyAreaMesh_RectangleGrid_FirstLayerThickness":1,
"StudyAreaMesh_GridSurfaceNode_Readme":"逐個點在X方向上應為遞增的數字，不可填入null；在Z方向上填入對應高程，允許部分缺值(填入null)，將進行內插；Electrode_Index從1開始依序增加。若設為0或負值表示該位置不設置電極",
"StudyAreaMesh_GridSurfaceNode_Readme2":"X[m],Z[m],Electrode_Index",
"StudyAreaMesh_GridSurfaceNode":[
],
"PaddingMesh_GridStyle_Readme":"附加區域的網格類型。目前只支援「Rectangle」，就是指以矩形為基礎的網格類型。",
"PaddingMesh_GridStyle":"Rectangle",
"PaddingMesh_RectangleGrid_LeftPaddingLayerCount_Readme":"附加區域左側要追加的網格層數。就是指-X方向追加的地層數量。此值必須是0或是正整數。",
"PaddingMesh_RectangleGrid_LeftPaddingLayerCount":5,
"PaddingMesh_RectangleGrid_LeftPaddingLayerWidthFacfor_Readme":"附加區域左側的地層寬度控制因子。此值必須是正數。1表示寬度不變化，0.9表示寬度變小為原本的90%，1.1表示寬度變大為原本的110%。",
"PaddingMesh_RectangleGrid_LeftPaddingLayerWidthFacfor":1.2,
"PaddingMesh_RectangleGrid_FirstLeftPaddingLayerWidth_Readme":"附加區域左側的第一層寬度。單位:[m]。此值小於等於0的時候，表示自動使用最靠近的一層搭配厚度因子計算出第一層寬度，否則直接使用指定數值為第一層寬度。",
"PaddingMesh_RectangleGrid_FirstLeftPaddingLayerWidth":-1,
"PaddingMesh_RectangleGrid_RightPaddingLayerCount_Readme":"附加區域右側要追加的網格層數。就是指+X方向追加的地層數量。此值必須是0或是正整數。",
"PaddingMesh_RectangleGrid_RightPaddingLayerCount":5,
"PaddingMesh_RectangleGrid_RightPaddingLayerWidthFacfor_Readme":"附加區域右側的地層寬度控制因子。此值必須是正數。1表示寬度不變化，0.9表示寬度變小為原本的90%，1.1表示寬度變大為原本的110%。",
"PaddingMesh_RectangleGrid_RightPaddingLayerWidthFacfor":1.2,
"PaddingMesh_RectangleGrid_FirstRightPaddingLayerWidth_Readme":"附加區域右側的第一層寬度。單位:[m]。此值小於等於0的時候，表示自動使用最靠近的一層搭配厚度因子計算出第一層寬度，否則直接使用指定數值為第一層寬度。",
"PaddingMesh_RectangleGrid_FirstRightPaddingLayerWidth":-1,
"PaddingMesh_RectangleGrid_BottomPaddingLayerCount_Readme":"附加區域下方的網格層數。是指Z方向的地層數量。此值必須是0或正整數。",
"PaddingMesh_RectangleGrid_BottomPaddingLayerCount":5,
"PaddingMesh_RectangleGrid_BottomPaddingLayerThicknessFacfor_Readme":"附加區域右側的地層寬度控制因子。此值必須是正數。1表示寬度不變化，0.9表示寬度變小為原本的90%，1.1表示寬度變大為原本的110%。",
"PaddingMesh_RectangleGrid_BottomPaddingLayerThicknessFacfor":1.2,
"PaddingMesh_RectangleGrid_FirstBottomPaddingLayerThickness_Readme":"附加區域的第一層厚度。單位:[m]。此值小於等於0的時候，表示自動使用最靠近的一層搭配厚度因子計算出第一層厚度，否則直接使用指定數值為第一層厚度。",
"PaddingMesh_RectangleGrid_FirstBottomPaddingLayerThickness":-1,
"ModifyMeshResistivity_Readme":"依序填入多則修改範圍，將依照規則順序修改模型。",
"ModifyMeshResistivity_Readme2":"x_min[m],x_max[m],z_min[m],z_max[m],New_Resistivity[ohm-m]",
"ModifyMeshResistivity":[
]
}
```
+ 1.2 去修改「CreateMeshLoad_ToolButton」的「Visible」為「False」。
+ 1.3 去修改「CreateMeshRun_ToolButton」的「Event」頁面下「OnClick」為如下程式碼。
```pascal
procedure TForm1.CreateMeshRun_ToolButtonClick(Sender: TObject);
var
  temp_i: Integer;
  RegexObj: TRegExpr;
  temp_str: AnsiString;
begin
  CreateMeshSettingsNowJson_Memo.Clear;
  //--------------------------------------------------------------------------
  //
  for temp_i := 0 to 64-1 do
  begin
    CreateMeshSettingsNowJson_Memo.Lines.Add(CreateMeshSettingsDefaultJson_Memo.Lines.Strings[temp_i]);
  end;
  //--------------------------------------------------------------------------

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
  finally
    RegexObj.Free; // 釋放 TRegExpr 物件
  end;
  //--------------------------------------------------------------------------
end;     
```
+ 1.4 原始碼最前面部分先宣告修改為如下程式碼。
```
implementation
uses
  RegExpr,LazUTF8;
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
+ 1.5 拖拉一個「System>TProcess」到「Form1」中。預設名稱會是「Process1」，修改「Name」為「CreateMesh_AsyncProcess」。其實只是想要uses Process。
  + 1.5.1 其實只是想要「uses Process」才能用程式碼設定「Option」。與其自行添加不如就使用元件自動引入的「uses Process」。目前這個Process1是沒用到的。 
+ 1.6 拖拉一個「System>TAsyncProcess」到「Form1」中。預設名稱會是「AsyncProcess1」，修改「Name」為「CreateMesh_AsyncProcess」。
+ 1.7 去修改「CreateMesh_AsyncProcess」的「Event」頁面下「OnReadData」為如下程式碼。
```pascal
procedure TForm1.CreateMesh_AsyncProcessReadData(Sender: TObject);
var
  temp_Buffer:string='';
  temp_BytesAvailable:DWord;
begin
  temp_BytesAvailable:=CreateMesh_AsyncProcess.Output.NumBytesAvailable;
  if temp_BytesAvailable>0 Then begin
    setlength(temp_Buffer,temp_BytesAvailable);
    CreateMesh_AsyncProcess.Output.Read(temp_Buffer[1],temp_BytesAvailable);
    temp_Buffer := StringReplace(temp_Buffer, #13#10, #10, [rfReplaceAll]);
    temp_Buffer := StringReplace(temp_Buffer, #10, #13#10, [rfReplaceAll]);
    CreateMeshSettingsCmdLog_Memo.Lines.Add(WinCPToUTF8(temp_Buffer));
  end;
end;
```
+ 1.8 去修改「CreateMesh_AsyncProcess」的「Event」頁面下「OnClick」為如下程式碼。
```pascal
procedure TForm1.CreateMesh_AsyncProcessTerminate(Sender: TObject);
var
  temp_str: AnsiString;
begin
  //--------------------------------------------------------------------------
  // 載入結果圖片
  temp_str:=CreateMeshSettingsDefaultJson_Memo.Lines.Strings[41-1];
  temp_str:=StringReplace(temp_str, '"OutputFile06_BasicMeshPNG_FileName":"', '', [rfReplaceAll]);
  temp_str:=StringReplace(temp_str, '",', '', [rfReplaceAll]);
  CreateMeshPreview_Image.Picture.LoadFromFile(temp_str);
  //--------------------------------------------------------------------------
  //--------------------------------------------------------------------------
  // 啟用元件
  CreateMeshRun_ToolButton.Enabled:=True;
  CreateMeshModelName_Edit.Enabled:=True;
  CreateMeshColorbarMinMax_Edit.Enabled:=True;
  CreateMeshStudyAreaMeshLayerSettings_Edit.Enabled:=True;
  CreateMeshPaddingMeshLeftSettings_Edit.Enabled:=True;
  CreateMeshPaddingMeshRightSettings_Edit.Enabled:=True;
  CreateMeshPaddingMeshBottomSettings_Edit.Enabled:=True;
  CreateMeshSurfaceNodeSettings_Memo.Enabled:=True;
  CreateMeshModifyMeshResistivitySettings_Memo.Enabled:=True;
  //--------------------------------------------------------------------------
  StatusBar1.Panels[0].Text:='建立模型網格...完成!';
end;
```
+ 1.9 去修改「CreateMeshRun_ToolButton」的「Event」頁面下「OnClick」為如下程式碼。
```
    //--------------------------------------------------------------------------
    // 使用 CreateMesh_AsyncProcess 運行外部程式
    CreateMesh_AsyncProcess.Executable:='cmd.exe';
    CreateMesh_AsyncProcess.Parameters.Clear;
    CreateMesh_AsyncProcess.Parameters.Add('/c');
    CreateMesh_AsyncProcess.Parameters.Add('.\PythonEnv\Python.exe -u ERTMaker_CreateAndModifyMesh_v20251031a.cpython-312.pyc');
    CreateMesh_AsyncProcess.Options:=[poUsePipes] + [poNoConsole];
    CreateMesh_AsyncProcess.Execute;
    //--------------------------------------------------------------------------
```  
