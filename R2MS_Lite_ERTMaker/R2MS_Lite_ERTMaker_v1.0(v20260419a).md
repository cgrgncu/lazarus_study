# R2MS_Lite_ERTMaker 開發筆記
+ 作者: HsiupoYeh  
+ 更新日期: 2026-04-19
+ 從 **R2MS_Lite_ERTMaker_v1.0(v20260418a).md** 開始，新增功能。

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

## 開發紀錄(寫UI部分)
### 追加設計UI
+ 1. 繼續編輯有關第2個分頁「逆推工具」(Inversion_TabSheet):  
  + 1.1 第6個分頁「逆推結果展示」(InversionAdvancedAnalysisInfo_TabSheet):
    + **1.1.1 拖拉一個「Standard>TToolBar」到「InversionAdvancedAnalysisInfo_TabSheet」中。預設名稱會是「ToolBar1」，修改「Name」為「InversionAdvancedAnalysisInfo_ToolBar」。**
      + **1.1.1.1 設定「BorderSpacing>Top」為「1」。**
      + **1.1.1.2 設定「Edge>Borders>ebBottom」為「True」。**
      + **1.1.1.3 設定「Edge>Borders>ebLeft」為「True」。**
      + **1.1.1.4 設定「Edge>Borders>ebRight」為「True」。**
      + **1.1.1.5 設定「ShowCaptions」為「True」。**
    + **1.1.2 拖拉一個「Standerd>TPopupMenu」到「Form1」中。預設名稱會是「PopupMenu1」，修改「Name」為「InversionAdvancedAnalysisInfo_PopupMenu」。**
      + **1.1.2.1 點兩下元件，進入編輯模式，建立第一層第一個選單，「Caption」設為「幾何因子交叉圖(K Cross Plot)」，「Name」設為「InversionAdvancedAnalysisInfo_PopupMenu_1_1」。**
      + **1.1.2.2 點兩下元件，進入編輯模式，建立第一層第一個選單，「Caption」設為「觀測資料(視電阻率)統計直方圖」，「Name」設為「InversionAdvancedAnalysisInfo_PopupMenu_1_2」。**
      + **1.1.2.3 點兩下元件，進入編輯模式，建立第一層第一個選單，「Caption」設為「逆推運算迭代收斂曲線」，「Name」設為「InversionAdvancedAnalysisInfo_PopupMenu_1_3」。**
    + **1.1.3 在「InversionAdvancedAnalysisInfo_ToolBar」元件上按「右鍵>New Button」，會增加一個新按鈕。預設名稱會是「ToolButton1」，修改「Name」為「InversionAdvancedAnalysisInfoType_ToolButton」**
      + **1.1.3.1 設定「Caption」為「逆推運算迭代收斂曲線」。**
      + **1.1.3.2 設定「DropdownMenu」為「InversionAdvancedAnalysisInfo_PopupMenu」。**
      + **1.1.3.3 設定「Style」為「tbsButtonDrop」。**
      + **1.1.3.4 設定「Enabled」為「False」。**   
    + **1.1.4 拖拉一個「Standard>TGroupBox」到「InversionAdvancedAnalysisInfo_TabSheet」中。預設名稱會是「GroupBox1」，修改「Name」為「InversionAdvancedAnalysisInfoImage_GroupBox」。**
      + **1.1.4.1 設定「Anchors」。**  
        + **1.1.4.1.1 「Top anchoring>Slbling」選為「InversionAdvancedAnalysisInfo_ToolBar:TToolBar」。維持「Top anchoring>Enable」為打勾。選擇「Top anchoring」右側三個鈕選下面的。** 
        + **1.1.4.1.2 「Left anchoring>Slbling」選為「InversionAdvancedAnalysisInfo_TabSheet:TTabSheet」。維持「Left anchoring>Enable」為打勾。選擇「Left anchoring」右側三個鈕選左邊的。**  
        + **1.1.4.1.3 「Right anchoring>Slbling」選為「InversionAdvancedAnalysisInfo_TabSheet:TTabSheet」。勾選「Right anchoring>Enable」為打勾。選擇「Right anchoring」右側三個鈕選左邊的。**  
        + **1.1.4.1.4 「Bottom anchoring>Slbling」選為「InversionAdvancedAnalysisInfo_TabSheet:TTabSheet」。勾選「Bottom anchoring>Enable」為打勾。選擇「Bottom anchoring」右側三個鈕選下面的。**
        + **1.1.4.1.5 「Border space」正中間那格填「0」，左邊那格填「0」，右邊那格填「3」。**
        + **1.1.4.1.6 設定完後關閉Anchors設定視窗。**  
      + **1.1.4.2  設定「Caption」為「進階分析資訊預覽:」。**
    + **1.1.5 拖拉一個「Additional>TImage」到「InversionAdvancedAnalysisInfoImage_GroupBox」中。預設名稱會是「Image1」，修改「Name」為「InversionAdvancedAnalysisInfoImage_Image」。**
      + **1.1.5.1 設定「Align」為「alClient」。**
      + **1.1.5.2 設定「AntialiasingMode」為「amOn」。**
      + **1.1.5.3 設定「Center」為「True」。**
      + **1.1.5.4 設定「Proportional」為「True」。**
      + **1.1.5.5 設定「Stretch」為「True」。**      

## 開發紀錄(寫功能部分)
### 版本
+ 2.1選Form，去編輯FormCreate，修改部分:
```pascal
procedure TForm1.FormCreate(Sender: TObject);
begin
  //--------------------------------------------------------------------------
  // 在這裡對全域變數進行初始化
  version_str := 'v1.0(v20260419a)';
  Current_Folder_Path := ExtractFilePath(Application.ExeName);
  //--------------------------------------------------------------------------
  //--------------------------------------------------------------------------
  // 表單標題追加版本
  Form1.Caption := Form1.Caption + ' ' + version_str;
  //--------------------------------------------------------------------------
end;      
```
### 功能 
+ 2.2 去修改「InversionAdvancedAnalysisInfo_PopupMenu_1_1」的「Event」頁面下「OnClick」為如下程式碼。
```pascal
procedure TForm1.InversionAdvancedAnalysisInfo_PopupMenu_1_1Click(
  Sender: TObject);
begin
  InversionAdvancedAnalysisInfoType_ToolButton.Caption:='幾何因子交叉圖(K Cross Plot)';
  InversionAdvancedAnalysisInfoImage_Image.Picture.LoadFromFile('Output_ERTMaker_Inversion2D/'+InversionModelingOutputSettings_MainName_Edit.Text+'_02_CrossPlot_K.png');
end; 
```
+ 2.3 去修改「InversionAdvancedAnalysisInfo_PopupMenu_1_2」的「Event」頁面下「OnClick」為如下程式碼。
```pascal
procedure TForm1.InversionAdvancedAnalysisInfo_PopupMenu_1_2Click(
  Sender: TObject);
begin
  InversionAdvancedAnalysisInfoType_ToolButton.Caption:='觀測資料(視電阻率)統計直方圖';
  InversionAdvancedAnalysisInfoImage_Image.Picture.LoadFromFile('Output_ERTMaker_Inversion2D/'+InversionModelingOutputSettings_MainName_Edit.Text+'_02_Histogram_AppRes_Accept.png');
end;   
```
+ 2.4 去修改「InversionAdvancedAnalysisInfo_PopupMenu_1_3」的「Event」頁面下「OnClick」為如下程式碼。
```pascal
procedure TForm1.InversionAdvancedAnalysisInfo_PopupMenu_1_3Click(
  Sender: TObject);
begin
  InversionAdvancedAnalysisInfoType_ToolButton.Caption:='逆推運算迭代收斂曲線';
  InversionAdvancedAnalysisInfoImage_Image.Picture.LoadFromFile('Output_ERTMaker_Inversion2D/'+InversionModelingOutputSettings_MainName_Edit.Text+'_04_Convergence_Curve.png');
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
    if not FileExists('ERTMaker_Inversion2D_v20260419a.cpython-312.pyc') then
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
    InversionModeling_AsyncProcess.Parameters.Add('set PYTHONIOENCODING=utf-8 && .\PythonEnv\Python.exe -u ERTMaker_Inversion2D_v20260419a.cpython-312.pyc');
    InversionModeling_AsyncProcess.Options:=[poUsePipes, poStderrToOutPut, poNoConsole];
    InversionModeling_AsyncProcess.Execute;
    // 啟用Timer
    InversionModeling_Timer.Enabled:=True;
    StatusBar1.Panels[1].Text:='1';
    StatusBar1.Panels[2].Text:='';
    StatusBar1.Panels[3].Text:='';
    StatusBar1.Panels[4].Text:='正在自動更新資訊';
    // 禁用元件，等背景運作完再從事件重新啟用按鈕
    Forward_TabSheet.Enabled:=False;
    InversionModelingRun_ToolButton.Enabled:=False;
    InversionModelingParameters_GroupBox.Enabled:=False;
    InversionInputObsDataPreviewLeft_ToolButton.Enabled:=False;
    InversionInputObsDataPreviewLeft_ToolButton.Caption:='所有的觀測資料';
    InversionInputObsDataPreviewRight_ToolButton.Enabled:=False;
    InversionInputObsDataPreviewRight_ToolButton.Caption:='採用的觀測資料';
    InversionInputMeshPreviewBoth_ToolButtonClick(InversionInputMeshPreviewBoth_ToolButton);
    InversionResultPreviewType_ToolButton.Enabled:=False;
    InversionResultPreviewType_ToolButton.Caption:='逆推結果(有網格線自動色階無覆蓋度)';
    InversionResultPreviewPrevious_ToolButton.Enabled:=False;
    InversionResultPreviewNext_ToolButton.Enabled:=False;
    InversionResultPreviewFinal_ToolButton.Enabled:=False;
    InversionAdvancedAnalysisInfoType_ToolButton.Enabled:=False;
    InversionAdvancedAnalysisInfoType_ToolButton.Caption:='逆推運算迭代收斂曲線';
    // 清空圖片
    InversionInputMeshPreviewFullMesh_Image.Picture.Clear;
    InversionInputMeshPreviewStudyAreaMeshMesh_Image.Picture.Clear;
    InversionInputObsDataPreviewLeft_Image.Picture.Clear;
    InversionInputObsDataPreviewRight_Image.Picture.Clear;
    InversionResultPreviewImage_Image.Picture.Clear;
    InversionAdvancedAnalysisInfoImage_Image.Picture.Clear;
    //--------------------------------------------------------------------------
  finally
    RegexObj.Free; // 釋放 TRegExpr 物件
  end;
  //--------------------------------------------------------------------------
end;
```
+ 2.6 去修改「InversionModeling_Timer」的「Event」頁面下「OnTimer」為如下程式碼。
```pascal
procedure TForm1.InversionModeling_TimerTimer(Sender: TObject);
var
  temp_FileStream: TFileStream;
  temp_i: Integer;
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
        StatusBar1.Panels[1].Text:='2';
        Exit;
      except
        on E: Exception do
        begin
          RunningLog_Memo.Lines.Add('載入圖片失敗！錯誤原因：' + E.Message);
        end;
      end;
    end;
    //--------------------------------------------------------------------------
  end;
  if StrToInt(StatusBar1.Panels[1].Text) = 2 then
  begin
    //--------------------------------------------------------------------------
    // 載入圖片
    if FileExists('Output_ERTMaker_Inversion2D/'+InversionModelingOutputSettings_MainName_Edit.Text+'_02_Histogram_AppRes_Accept.png') then
    begin
      try
        InversionInputObsDataPreviewLeft_Image.Picture.LoadFromFile('Output_ERTMaker_Inversion2D/'+InversionModelingOutputSettings_MainName_Edit.Text+'_Distribution_AppRes_All.png');
        InversionInputObsDataPreviewRight_Image.Picture.LoadFromFile('Output_ERTMaker_Inversion2D/'+InversionModelingOutputSettings_MainName_Edit.Text+'_Distribution_AppRes_Accept.png');
        Inversion_PageControl.ActivePage:=InversionInputObsDataPreview_TabSheet;
        InversionInputObsDataPreviewLeft_ToolButton.Enabled:=True;
        InversionInputObsDataPreviewRight_ToolButton.Enabled:=True;
        StatusBar1.Panels[1].Text:='3';
        Exit;
      except
        on E: Exception do
        begin
          RunningLog_Memo.Lines.Add('載入圖片失敗！錯誤原因：' + E.Message);
        end;
      end;
    end;
    //--------------------------------------------------------------------------
  end;
  if StrToInt(StatusBar1.Panels[1].Text) = 3 then
  begin
    //--------------------------------------------------------------------------
    // 初次載入csv，紀錄行數
    if FileExists('Output_ERTMaker_Inversion2D/'+InversionModelingOutputSettings_MainName_Edit.Text+'_03_Inversion_Log.csv') then
    begin
      try
        temp_FileStream := TFileStream.Create(('Output_ERTMaker_Inversion2D/'+InversionModelingOutputSettings_MainName_Edit.Text+'_03_Inversion_Log.csv'), fmOpenRead or fmShareDenyNone);
        try
          //Inversion_PageControl.ActivePage:=InversionResultPreview_TabSheet;
          InversionResultPreviewRLI_ToolButton.Caption:='';
          InversionResultPreviewGrid_StringGrid.AutoFillColumns:=False;
          InversionResultPreviewGrid_StringGrid.LoadFromCSVStream(temp_FileStream, ',', True);
          // 逐行調整尺寸，避免用戶覺得卡住。
          for temp_i := InversionResultPreviewGrid_StringGrid.FixedCols to InversionResultPreviewGrid_StringGrid.ColCount - 1 do
          begin
            InversionResultPreviewGrid_StringGrid.AutoSizeColumn(temp_i);
            Application.ProcessMessages;
          end;
        finally
          temp_FileStream.Free;
        end;
        StatusBar1.Panels[1].Text:='4';
        StatusBar1.Panels[2].Text:=IntToStr(InversionResultPreviewGrid_StringGrid.RowCount);
        Exit;
      except
        on E: Exception do
        begin
          RunningLog_Memo.Lines.Add('載入Log.csv失敗!錯誤原因：' + E.Message);
        end;
      end;
    end;
    //--------------------------------------------------------------------------
  end;
  if StrToInt(StatusBar1.Panels[1].Text) = 4 then
  begin
    //--------------------------------------------------------------------------
    // 重複載入csv，行數變化才做事
    if FileExists('Output_ERTMaker_Inversion2D/'+InversionModelingOutputSettings_MainName_Edit.Text+'_03_Inversion_Log.csv') then
    begin
      try
        temp_FileStream := TFileStream.Create(('Output_ERTMaker_Inversion2D/'+InversionModelingOutputSettings_MainName_Edit.Text+'_03_Inversion_Log.csv'), fmOpenRead or fmShareDenyNone);
        try
          InversionResultPreviewGrid_StringGrid.AutoFillColumns:=False;
          InversionResultPreviewGrid_StringGrid.LoadFromCSVStream(temp_FileStream, ',', True);
          // 逐行調整尺寸，避免用戶覺得卡住。
          for temp_i := InversionResultPreviewGrid_StringGrid.FixedCols to InversionResultPreviewGrid_StringGrid.ColCount - 1 do
          begin
            InversionResultPreviewGrid_StringGrid.AutoSizeColumn(temp_i);
            Application.ProcessMessages;
          end;
        finally
          temp_FileStream.Free;
        end;
        //--
        if StatusBar1.Panels[2].Text=IntToStr(InversionResultPreviewGrid_StringGrid.RowCount) then
        begin
          if FileExists('Output_ERTMaker_Inversion2D/'+InversionModelingOutputSettings_MainName_Edit.Text+'_04_Convergence_Curve.png') then
          begin
            // 畫圖
            try
              InversionAdvancedAnalysisInfoImage_Image.Picture.LoadFromFile('Output_ERTMaker_Inversion2D/'+InversionModelingOutputSettings_MainName_Edit.Text+'_04_Convergence_Curve.png');
            except
              on E: Exception do
              begin
                RunningLog_Memo.Lines.Add('載入圖片失敗！錯誤原因：' + E.Message);
              end;
            end;
            StatusBar1.Panels[1].Text:='5';
            InversionModeling_Timer.Enabled:=False;
            StatusBar1.Panels[4].Text:='停止自動更新資訊';
            InversionResultPreviewType_ToolButton.Enabled:=True;
            InversionResultPreviewPrevious_ToolButton.Enabled:=True;
            InversionResultPreviewNext_ToolButton.Enabled:=True;
            InversionResultPreviewFinal_ToolButton.Enabled:=True;
            InversionAdvancedAnalysisInfoType_ToolButton.Enabled:=True;
          end;
          if InversionResultPreviewRLI_ToolButton.Enabled=False then
          begin
            //--------------------------------------------------------------------------
            // 載入圖片
            if FileExists('Output_ERTMaker_Inversion2D/'+InversionModelingOutputSettings_MainName_Edit.Text+'_'+InversionResultPreviewRLI_ToolButton.Caption+'_INV.png') then
            begin
              // 第一次畫圖就跳轉頁面
              if InversionResultPreviewRLI_ToolButton.Caption = 'R00_L01_I00' then
              begin
                 Inversion_PageControl.ActivePage:=InversionResultPreview_TabSheet;
                 InversionResultPreviewShowImage_ToolButtonClick(InversionResultPreviewShowImage_ToolButton);
              end;
              // 畫圖
              try
                InversionResultPreviewImage_Image.Picture.LoadFromFile('Output_ERTMaker_Inversion2D/'+InversionModelingOutputSettings_MainName_Edit.Text+'_'+InversionResultPreviewRLI_ToolButton.Caption+'_INV.png');
                InversionResultPreviewRLI_ToolButton.Enabled:=True;
                Exit;
              except
                on E: Exception do
                begin
                  RunningLog_Memo.Lines.Add('載入圖片失敗！錯誤原因：' + E.Message);
                end;
              end;
            end;
            //--------------------------------------------------------------------------
          end;
        end
        else
        begin
          StatusBar1.Panels[2].Text:=IntToStr(InversionResultPreviewGrid_StringGrid.RowCount);
          // 將目前的選中列設為最後一行 (索引是總列數 - 1)
          InversionResultPreviewGrid_StringGrid.Row := InversionResultPreviewGrid_StringGrid.RowCount - 1;
          // 確保滾輪也捲動到那裡，讓最後一行出現在畫面上
          InversionResultPreviewGrid_StringGrid.TopRow := InversionResultPreviewGrid_StringGrid.RowCount - 1;
          // 標註繪圖要用哪個Row。
          StatusBar1.Panels[3].Text:=IntToStr(InversionResultPreviewGrid_StringGrid.Row);
          // 準備繪圖用的圖檔部分名稱
          temp_str := 'R' + InversionResultPreviewGrid_StringGrid.Cells[3, StrToInt(StatusBar1.Panels[3].Text)].Trim.PadLeft(2, '0');
          temp_str := temp_str + '_L' + InversionResultPreviewGrid_StringGrid.Cells[28, StrToInt(StatusBar1.Panels[3].Text)].Trim.PadLeft(2, '0');
          temp_str := temp_str + '_I' + InversionResultPreviewGrid_StringGrid.Cells[5, StrToInt(StatusBar1.Panels[3].Text)].Trim.PadLeft(2, '0');
          InversionResultPreviewRLI_ToolButton.Caption:=temp_str;
          InversionResultPreviewRLI_ToolButton.Enabled:=False;
        end;
        Exit;
      except
        on E: Exception do
        begin
          RunningLog_Memo.Lines.Add('載入Log.csv失敗!錯誤原因：' + E.Message);
        end;
      end;
    end;
    //--------------------------------------------------------------------------
  end;
end;
```
+ 2.6 修改「InversionModelingSettingsDefaultJson_Memo」的「Lines」為以下文字。
```
{
"Inversion2D_Version":"v20260419a",
"Inversion2D_Author":"HsiupoYeh",
"InputFile01_Ohm_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"InputFile01_Ohm_FileName":"Input_ERTMaker_Inversion2D/XP2_R01_L01_I01_CalData.dat",
"Mesh_Setting01_AutoMesh_Enable_Readme":"逆推參數-使用自動三角網格。若要啟用請填入字串Yes，若不啟用請填入字串No。若啟用則使用自動產生三角網格，不會讀取VTK檔案與JSON檔案。",
"Mesh_Setting01_AutoMesh_Enable":"Yes",
"Mesh_Setting02_AutoMesh_quality_Readme":"逆推參數-設定自動三角網格的網格品質。控制不要讓三角形太扁長。預設值:32，建議填入32.0~34.0之間的浮點數。此設定只有啟用自動網格才會有效。",
"Mesh_Setting02_AutoMesh_quality":33,
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
"Ideal_Mesh_Setting01_AutoMesh_quality":33,
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
+ 2.7 去修改「InversionResultPreviewType_ToolButton」的「Hint」為「可以使用快捷鍵:「Ctrl+方向鍵上」、「Ctrl+方向鍵下」進行快速切換。」。
+ 2.8 去修改「InversionResultPreviewType_ToolButton」的「ShowHint」為「True」。
+ 2.9 去修改「InversionResultPreviewPrevious_ToolButton」的「Hint」為「可以使用快捷鍵:「Ctrl+方向鍵左」進行快速切換。」。
+ 2.10 去修改「InversionResultPreviewPrevious_ToolButton」的「ShowHint」為「True」。
+ 2.11 去修改「InversionResultPreviewNext_ToolButton」的「Hint」為「可以使用快捷鍵:「Ctrl+方向鍵右」進行快速切換。」。
+ 2.12 去修改「InversionResultPreviewNext_ToolButton」的「ShowHint」為「True」。
+ 3.1 去修改「ForwardModelingOutputPNGEnable_CheckBox」的「Checked」為「False」。

### 更新紀錄
+ 4.1 選「UpdateLog_Memo」，去編輯「Lines」
```
軟體名稱: R2MS_Lite_ERTMaker
作者: HsiupoYeh
--
v20260419a
完整功能版 (包含順推模擬:建立模型網格、模擬時間序列蒐集、時間序列資料處理以及逆推模擬)
**建議作業系統: Windows 10
**ERTMaker主版本號: v1.0
**ERTMaker圖形化介面版本號: v20260419a
**Python版本: 3.12.7 (64-bit)
**PyGimli版本: v1.5.4
***已知問題: 逆推運算的平行運算預設不使用所有核心運作(會保留兩個核心)。但在Intel第12代的CPU(i7-1260P)環境下設定失效。這可能會使作業系統極度忙碌，導致網路等功能無回應。這並非軟體設計問題，而是這版本的CPU調度邏輯不良。
***支援快捷鍵: 「Ctrl+方向鍵」來快速瀏覽歷次逆推結果。
--
v20251105a
初版 (僅設計排版)
```
