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
+ 2.1 修改「ForwardModelingSettingsDefaultJson_Memo」。
  + 2.1.1 設定「Lines」為以下文字。
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

  + 2.2 拖拉一個「System>TAsyncProcess」到「Form1」中。預設名稱會是「AsyncProcess1」，修改「Name」為「ForwardModeling_AsyncProcess」。
  + 2.3 去修改「ForwardModeling_AsyncProcess」的「Event」頁面下「OnReadData」為如下程式碼。
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
  + 2.4 去修改「ForwardModeling_AsyncProcess」的「Event」頁面下「OnTerminate」為如下程式碼。
  ```pascal
  ```
  + 2.2 去修改「ForwardModelingRun_ToolButton」的「Event」頁面下「OnClick」為如下程式碼。
  + 
