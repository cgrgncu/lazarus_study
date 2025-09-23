# R2MS_Lite_Smart_Scheduler 開發筆記
+ 作者: HsiupoYeh  
+ 更新日期: 2025-09-23
+ 從 **R2MS_Lite_Smart_Scheduler_v20250918b** 開始，繼續更新基本功能。

## 開發環境  
+ 使用「Win10_64_CHT_Lazarus4.2_v20250915a.ova」。**匯入為「Win10_Lazarus4.2_R2MS_Lite_Smart_Scheduler_v20250915a.ova」。**
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

## 備忘  
+ 預設運作平台: Default，應該就是win10 64bit。
+ 依照官網建議，為了避免DPI問題，各項操作建議(安裝IDE、設計窗體、編寫程式碼)在縮放大小100%的情況下進行。編譯出來的軟體可以拿去其他DPI設定下的電腦測試。
  + 使用解析度: 1920x1080
  + 縮放大小: 100%

## 設計規定
+ 多源放電:
  + 目前只支援一種，標準的多源64個電極放電，稱為「M64」。 
    + INI參數應填入「M64」。
    + 軟體UI將顯示「M64」。
+ 單源放電:
  + 單源64個電極放電普通版本(SingleTx_64_Normal)，稱為「S64N」。
    + INI參數應填入「S64N」。
    + 軟體UI將顯示「S64N」。
  + 單源64個電極放電普通版本(SingleTx_64_HighSpeed)，稱為「S64HS」。
    + INI參數應填入「S64HS」。
    + 軟體UI將顯示「S64HS」。
  + 單源64個電極放電普通版本(SingleTx_64_VeryHighSpeed)，稱為「S64VHS」。
    + INI參數應填入「S64VHS」。
    + 軟體UI將顯示「S64VHS」。
+ 系統測試:
    + INI參數應填入「SystemTest」。
    + 軟體UI將顯示「系統測試」。

## 開發紀錄(寫功能部分)
### INI檔案讀寫
+ 1.INI檔案讀寫
  + 1.1 去implementation區塊追加宣告全域變數
```
implementation
//--------------------------------------------------------------------------
//uses add by HsiupoYeh
uses
  IniFiles;
//--------------------------------------------------------------------------
//--------------------------------------------------------------------------
//宣告全域變數 add by HsiupoYeh
var
  version_str: AnsiString;
  Current_Folder_Path: AnsiString;
//--------------------------------------------------------------------------

{$R *.lfm}   
```
  + 選Form，去編輯FormCreate
```
procedure TForm1.FormCreate(Sender: TObject);
var
  temp_str: AnsiString;
  temp_TDateTime: TDateTime;
  need_write_ini_file: boolean;
  my_TMemIniFile: TMemIniFile;
  temp_i: Integer;
begin
  //--------------------------------------------------------------------------
  // 在這裡對全域變數進行初始化
  version_str := 'v20250923a';
  Current_Folder_Path := ExtractFilePath(Application.ExeName);
  //--------------------------------------------------------------------------
  //--------------------------------------------------------------------------
  // 表單標題追加版本
  Form1.Caption := Form1.Caption + ' ' + version_str;
  //--------------------------------------------------------------------------
  //--------------------------------------------------------------------------
  // 設定我指定的路徑作為R2MS_Lite_WorkSpace資料夾，路徑命名格式依照ERT_ver2_9_9_Client的白痴路徑格式。
  // 例如:「/C/Users/USER/Desktop/R2MS_Lite_Smart_Scheduler_v20250920a/R2MS_Lite_WorkSpace/ERT_ver2_9_9_Client_Data」
  ERT_ver2_9_9_Client_SavePath_Edit.Text := '/' + Current_Folder_Path + 'R2MS_Lite_WorkSpace\ERT_ver2_9_9_Client_Data';
  ERT_ver2_9_9_Client_SavePath_Edit.Text := StringReplace(ERT_ver2_9_9_Client_SavePath_Edit.Text, ':', '', []);
  ERT_ver2_9_9_Client_SavePath_Edit.Text := StringReplace(ERT_ver2_9_9_Client_SavePath_Edit.Text, '\', '/', [rfReplaceAll]);
  //--------------------------------------------------------------------------
  //--------------------------------------------------------------------------
  // 讀取INI參數檔案
  // 記得在 uses 區塊中加入 IniFiles
  my_TMemIniFile := TMemIniFile.Create(ChangeFileExt(ExtractFileName(Application.ExeName), '.ini'));

  if FileExists(ChangeFileExt(ExtractFileName(Application.ExeName), '.ini')) then
  begin
    //--
    // 目標INI檔案存在，不需要寫入新的INI。
    need_write_ini_file := False;
    //--
    // INI: [Version] -> Version
    temp_str := my_TMemIniFile.ReadString('Version', 'Version', '');
    //RunningLog_Memo.Lines.Add('INI: [Version] -> Version = ' + temp_str);
    if temp_str = '' then
    begin
      temp_str := '嚴重錯誤:' + #13#10 + '檔案「' +
        ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Version] -> Version 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。';
      Application.MessageBox(PChar(temp_str), '錯誤', 16);
      Application.Terminate;
    end;
    //--
    // INI: [Version] -> Author
    temp_str := my_TMemIniFile.ReadString('Version', 'Author', '');
    //RunningLog_Memo.Lines.Add('INI: [Version] -> Author = ' + temp_str);
    if temp_str = '' then
    begin
      temp_str := '嚴重錯誤:' + #13#10 + '檔案「' +
        ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Version] -> Author 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。';
      Application.MessageBox(PChar(temp_str), '錯誤', 16);
      Application.Terminate;
    end;
    //--
    // INI: [ERT_ver2_9_9_Client] -> ERT_Profile_Name
    temp_str := my_TMemIniFile.ReadString('ERT_ver2_9_9_Client', 'ERT_Profile_Name', '');
    //RunningLog_Memo.Lines.Add('INI: [ERT_ver2_9_9_Client] -> ERT_Profile_Name = ' + temp_str);
    if (Length(temp_str) >= 3) and (temp_str[1] = '"') and (temp_str[Length(temp_str)] = '"') then
    begin
      ERT_Profile_Name_Edit.Text := Copy(temp_str, 2, Length(temp_str) - 2);
      ERT_Profile_Name_Edit.Font.Color := clBlue;
    end
    else
    begin
      temp_str := '嚴重錯誤:' + #13#10 + '檔案「' +
        ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[ERT_ver2_9_9_Client] -> ERT_Profile_Name 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。';
      Application.MessageBox(PChar(temp_str), '錯誤', 16);
      Application.Terminate;
    end;
    //--
    // INI: [ERT_ver2_9_9_Client] -> ERT_Project_Name
    temp_str := my_TMemIniFile.ReadString('ERT_ver2_9_9_Client', 'ERT_Project_Name', '');
    //RunningLog_Memo.Lines.Add('INI: [ERT_ver2_9_9_Client] -> ERT_Project_Name = ' + temp_str);
    if (Length(temp_str) >= 3) and (temp_str[1] = '"') and (temp_str[Length(temp_str)] = '"') then
    begin
      ERT_Project_Name_Edit.Text := Copy(temp_str, 2, Length(temp_str) - 2);
      ERT_Project_Name_Edit.Font.Color := clBlue;
    end
    else
    begin
      temp_str := '嚴重錯誤:' + #13#10 + '檔案「' +
        ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[ERT_ver2_9_9_Client] -> ERT_Project_Name 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。';
      Application.MessageBox(PChar(temp_str), '錯誤', 16);
      Application.Terminate;
    end;
    //--
    // INI: [ERT_ver2_9_9_Client] -> DMM_SN
    temp_str := my_TMemIniFile.ReadString('ERT_ver2_9_9_Client', 'DMM_SN', '');
    //RunningLog_Memo.Lines.Add('INI: [ERT_ver2_9_9_Client] -> DMM_SN = ' + temp_str);
    if (Length(temp_str) >= 3) and (temp_str[1] = '"') and (temp_str[Length(temp_str)] = '"') then
    begin
      DMM_SN_Edit.Text := Copy(temp_str, 2, Length(temp_str) - 2);
      DMM_SN_Edit.Font.Color := clBlue;
    end
    else
    begin
      temp_str := '嚴重錯誤:' + #13#10 + '檔案「' +
        ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[ERT_ver2_9_9_Client] -> DMM_SN 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。';
      Application.MessageBox(PChar(temp_str), '錯誤', 16);
      Application.Terminate;
    end;
    //--
    // INI: [ERT_ver2_9_9_Client] -> DMM_ComPort
    temp_str := my_TMemIniFile.ReadString('ERT_ver2_9_9_Client', 'DMM_ComPort', '');
    //RunningLog_Memo.Lines.Add('INI: [ERT_ver2_9_9_Client] -> DMM_ComPort = ' + temp_str);
    if (Length(temp_str) >= 3) and (temp_str[1] = '"') and (temp_str[Length(temp_str)] = '"') then
    begin
      DMM_Edit.Text := Copy(temp_str, 2, Length(temp_str) - 2);
      DMM_Edit.Font.Color := clBlue;
    end
    else
    begin
      temp_str := '嚴重錯誤:' + #13#10 + '檔案「' +
        ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[ERT_ver2_9_9_Client] -> DMM_ComPort 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。';
      Application.MessageBox(PChar(temp_str), '錯誤', 16);
      Application.Terminate;
    end;
    //--
    // INI: [ERT_ver2_9_9_Client] -> SwitchArray_SN
    temp_str := my_TMemIniFile.ReadString('ERT_ver2_9_9_Client', 'SwitchArray_SN', '');
    //RunningLog_Memo.Lines.Add('INI: [ERT_ver2_9_9_Client] -> SwitchArray_SN = ' + temp_str);
    if (Length(temp_str) >= 3) and (temp_str[1] = '"') and (temp_str[Length(temp_str)] = '"') then
    begin
      ESP32_SN_Edit.Text := Copy(temp_str, 2, Length(temp_str) - 2);
      ESP32_SN_Edit.Font.Color := clBlue;
    end
    else
    begin
      temp_str := '嚴重錯誤:' + #13#10 + '檔案「' +
        ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[ERT_ver2_9_9_Client] -> SwitchArray_SN 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。';
      Application.MessageBox(PChar(temp_str), '錯誤', 16);
      Application.Terminate;
    end;
    //--
    // INI: [ERT_ver2_9_9_Client] -> SwitchArray_ComPort
    temp_str := my_TMemIniFile.ReadString('ERT_ver2_9_9_Client', 'SwitchArray_ComPort', '');
    //RunningLog_Memo.Lines.Add('INI: [ERT_ver2_9_9_Client] -> SwitchArray_ComPort = ' + temp_str);
    if (Length(temp_str) >= 3) and (temp_str[1] = '"') and (temp_str[Length(temp_str)] = '"') then
    begin
      ESP32_Edit.Text := Copy(temp_str, 2, Length(temp_str) - 2);
      ESP32_Edit.Font.Color := clBlue;
    end
    else
    begin
      temp_str := '嚴重錯誤:' + #13#10 + '檔案「' +
        ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[ERT_ver2_9_9_Client] -> SwitchArray_ComPort 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。';
      Application.MessageBox(PChar(temp_str), '錯誤', 16);
      Application.Terminate;
    end;
    //--
    // INI: [ERT_ver2_9_9_Client] -> PSU_SN
    temp_str := my_TMemIniFile.ReadString('ERT_ver2_9_9_Client', 'PSU_SN', '');
    //RunningLog_Memo.Lines.Add('INI: [ERT_ver2_9_9_Client] -> PSU_SN = ' + temp_str);
    if (Length(temp_str) >= 3) and (temp_str[1] = '"') and (temp_str[Length(temp_str)] = '"') then
    begin
      PSU_SN_Edit.Text := Copy(temp_str, 2, Length(temp_str) - 2);
      PSU_SN_Edit.Font.Color := clBlue;
    end
    else
    begin
      temp_str := '嚴重錯誤:' + #13#10 + '檔案「' +
        ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[ERT_ver2_9_9_Client] -> PSU_SN 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。';
      Application.MessageBox(PChar(temp_str), '錯誤', 16);
      Application.Terminate;
    end;
    //--
    // INI: [ERT_ver2_9_9_Client] -> PSU_ComPort
    temp_str := my_TMemIniFile.ReadString('ERT_ver2_9_9_Client', 'PSU_ComPort', '');
    //RunningLog_Memo.Lines.Add('INI: [ERT_ver2_9_9_Client] -> PSU_ComPort = ' + temp_str);
    if (Length(temp_str) >= 3) and (temp_str[1] = '"') and (temp_str[Length(temp_str)] = '"') then
    begin
      PSU_Edit.Text := Copy(temp_str, 2, Length(temp_str) - 2);
      PSU_Edit.Font.Color := clBlue;
    end
    else
    begin
      temp_str := '嚴重錯誤:' + #13#10 + '檔案「' +
        ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[ERT_ver2_9_9_Client] -> PSU_ComPort 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。';
      Application.MessageBox(PChar(temp_str), '錯誤', 16);
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> PSU_RANGE_HIGH_LOW
    temp_str := my_TMemIniFile.ReadString('Schedule', 'PSU_RANGE_HIGH_LOW', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> PSU_RANGE_HIGH_LOW = ' + temp_str);
    if temp_str = '' then
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> PSU_RANGE_HIGH_LOW 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end
    else if temp_str = '"HIGH"' then
    begin
      PSU_RangeHIGH_CheckBox.Checked := True;
      PSU_RangeLOW_CheckBox.Enabled := False;
    end
    else if temp_str = '"LOW"' then
    begin
      PSU_RangeLOW_CheckBox.Checked := True;
      PSU_RangeHIGH_CheckBox.Enabled := False;
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> PSU_RANGE_HIGH_LOW 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> Every_Day_Enable
    temp_str := my_TMemIniFile.ReadString('Schedule', 'Every_Day_Enable', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> Every_Day_Enable = ' + temp_str);
    if temp_str = '' then
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> Every_Day_Enable 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end
    else if temp_str = '"true"' then
    begin
      EveryDay_RadioButton.Checked := True;
      OnlyToday_RadioButton.Enabled := False;
    end
    else if temp_str = '"false"' then
    begin
      OnlyToday_RadioButton.Checked := True;
      EveryDay_RadioButton.Enabled := False;
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> Every_Day_Enable 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_000
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_000', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_000 = ' + temp_str);
    if (Length(temp_str) = 7) and (temp_str[1] = '"') and (temp_str[4] = ':') and (temp_str[7] = '"') and
      TryStrToTime(Copy(temp_str, 2, 5), temp_TDateTime) then
    begin
      ScheduleSetting_ListView.Items.Item[0].SubItems.Strings[0] := FormatDateTime('HH:MM', temp_TDateTime);
      ScheduleSetting_ListView.Items.Item[0].SubItems.Strings[4] := '未知';
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_001 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_000_CurrentMode
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_000_CurrentMode', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_000_CurrentMode = ' + temp_str);
    if temp_str = '"SystemTest"' then
    begin
      ScheduleSetting_ListView.Items[0].SubItems.Strings[1] := '系統檢測';
      if FileExists('Configs\v299_SystemTestCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_SystemTestCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[0].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"M64"' then
    begin
      ScheduleSetting_ListView.Items[0].SubItems.Strings[1] := 'M64';
      if FileExists('Configs\v299_M64CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_M64CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[0].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64N"' then
    begin
      ScheduleSetting_ListView.Items[0].SubItems.Strings[1] := 'S64N';
      if FileExists('Configs\v299_M64CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64NCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[0].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64HS"' then
    begin
      ScheduleSetting_ListView.Items[0].SubItems.Strings[1] := 'S64HS';
      if FileExists('Configs\v299_S64HSCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64HSCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[0].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64VHS"' then
    begin
      ScheduleSetting_ListView.Items[0].SubItems.Strings[1] := 'S64VHS';
      if FileExists('Configs\v299_S64VHSCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64VHSCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[0].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom01"' then
    begin
      ScheduleSetting_ListView.Items[0].SubItems.Strings[1] := 'Custom01';
      if FileExists('Configs\v299_Custom01CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom01CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[0].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom02"' then
    begin
      ScheduleSetting_ListView.Items[0].SubItems.Strings[1] := 'Custom02';
      if FileExists('Configs\v299_Custom02CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom02CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[0].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom03"' then
    begin
      ScheduleSetting_ListView.Items[0].SubItems.Strings[1] := 'Custom03';
      if FileExists('Configs\v299_Custom03CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom03CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[0].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 +
        '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') + '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_000_CurrentMode 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_000_MaxVoltage_V
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_000_MaxVoltage_V', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_000_MaxVoltage_V = ' + temp_str);
    if temp_str = '' then
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_000_MaxVoltage_V 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end
    else
    begin
      if ScheduleSetting_ListView.Items[0].SubItems.Strings[1] = '系統檢測' then
      begin
        ScheduleSetting_ListView.Items[0].SubItems.Strings[2] := '24V';
      end
      else
      begin
        ScheduleSetting_ListView.Items[0].SubItems.Strings[2] := Copy(temp_str, 2, Length(temp_str) - 2) + 'V';
      end
    end;
    //--
    // INI: [Schedule] -> JOB_000_MaxCurrent_A
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_000_MaxCurrent_A', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_000_MaxCurrent_A = ' + temp_str);
    if temp_str = '' then
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_000_MaxCurrent_A 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end
    else
    begin
      if ScheduleSetting_ListView.Items[0].SubItems.Strings[1] = '系統檢測' then
      begin
        ScheduleSetting_ListView.Items[0].SubItems.Strings[3] := '0.1A';
      end
      else
      begin
        ScheduleSetting_ListView.Items[0].SubItems.Strings[3] := Copy(temp_str, 2, Length(temp_str) - 2) + 'A';
      end
    end;
    //--
    // INI: [Schedule] -> JOB_001
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_001', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_001 = ' + temp_str);
    if (Length(temp_str) = 7) and (temp_str[1] = '"') and (temp_str[4] = ':') and (temp_str[7] = '"') and
      TryStrToTime(Copy(temp_str, 2, 5), temp_TDateTime) then
    begin
      ScheduleSetting_ListView.Items.Item[1].SubItems.Strings[0] := FormatDateTime('HH:MM', temp_TDateTime);
      ScheduleSetting_ListView.Items.Item[1].SubItems.Strings[4] := '未知';
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_001 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_001_Enable
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_001_Enable', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_001_Enable = ' + temp_str);
    if temp_str = '"true"' then
    begin
      ScheduleSetting_ListView.Items.Item[1].Checked := True;
    end
    else if temp_str = '"false"' then
    begin
      ScheduleSetting_ListView.Items.Item[1].Checked := False;
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_001_Enable 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
     //--
    // INI: [Schedule] -> JOB_001_CurrentMode
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_001_CurrentMode', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_001_CurrentMode = ' + temp_str);
    if temp_str = '"SystemTest"' then
    begin
      ScheduleSetting_ListView.Items[1].SubItems.Strings[1] := '系統檢測';
      if FileExists('Configs\v299_SystemTestCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_SystemTestCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[1].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"M64"' then
    begin
      ScheduleSetting_ListView.Items[1].SubItems.Strings[1] := 'M64';
      if FileExists('Configs\v299_M64CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_M64CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[1].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64N"' then
    begin
      ScheduleSetting_ListView.Items[1].SubItems.Strings[1] := 'S64N';
      if FileExists('Configs\v299_M64CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64NCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[1].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64HS"' then
    begin
      ScheduleSetting_ListView.Items[1].SubItems.Strings[1] := 'S64HS';
      if FileExists('Configs\v299_S64HSCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64HSCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[1].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64VHS"' then
    begin
      ScheduleSetting_ListView.Items[1].SubItems.Strings[1] := 'S64VHS';
      if FileExists('Configs\v299_S64VHSCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64VHSCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[1].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom01"' then
    begin
      ScheduleSetting_ListView.Items[1].SubItems.Strings[1] := 'Custom01';
      if FileExists('Configs\v299_Custom01CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom01CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[1].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom02"' then
    begin
      ScheduleSetting_ListView.Items[1].SubItems.Strings[1] := 'Custom02';
      if FileExists('Configs\v299_Custom02CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom02CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[1].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom03"' then
    begin
      ScheduleSetting_ListView.Items[1].SubItems.Strings[1] := 'Custom03';
      if FileExists('Configs\v299_Custom03CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom03CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[1].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 +
        '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') + '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_000_CurrentMode 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_001_MaxVoltage_V
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_001_MaxVoltage_V', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_001_MaxVoltage_V = ' + temp_str);
    if temp_str = '' then
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_001_MaxVoltage_V 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end
    else
    begin
      if ScheduleSetting_ListView.Items[1].SubItems.Strings[1] = '系統檢測' then
      begin
        ScheduleSetting_ListView.Items[1].SubItems.Strings[2] := '24V';
      end
      else
      begin
        ScheduleSetting_ListView.Items[1].SubItems.Strings[2] := Copy(temp_str, 2, Length(temp_str) - 2) + 'V';
      end
    end;
    //--
    // INI: [Schedule] -> JOB_001_MaxCurrent_A
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_001_MaxCurrent_A', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_001_MaxCurrent_A = ' + temp_str);
    if temp_str = '' then
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_001_MaxCurrent_A 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end
    else
    begin
      if ScheduleSetting_ListView.Items[1].SubItems.Strings[1] = '系統檢測' then
      begin
        ScheduleSetting_ListView.Items[1].SubItems.Strings[3] := '0.1A';
      end
      else
      begin
        ScheduleSetting_ListView.Items[1].SubItems.Strings[3] := Copy(temp_str, 2, Length(temp_str) - 2) + 'A';
      end
    end;
    //--
    // INI: [Schedule] -> JOB_002
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_002', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_002 = ' + temp_str);
    if (Length(temp_str) = 7) and (temp_str[1] = '"') and (temp_str[4] = ':') and (temp_str[7] = '"') and
      TryStrToTime(Copy(temp_str, 2, 5), temp_TDateTime) then
    begin
      ScheduleSetting_ListView.Items.Item[2].SubItems.Strings[0] := FormatDateTime('HH:MM', temp_TDateTime);
      ScheduleSetting_ListView.Items.Item[2].SubItems.Strings[4] := '未知';
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_002 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_002_Enable
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_002_Enable', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_002_Enable = ' + temp_str);
    if temp_str = '"true"' then
    begin
      ScheduleSetting_ListView.Items.Item[2].Checked := True;
    end
    else if temp_str = '"false"' then
    begin
      ScheduleSetting_ListView.Items.Item[2].Checked := False;
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_002_Enable 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_002_CurrentMode
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_002_CurrentMode', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_002_CurrentMode = ' + temp_str);
    if temp_str = '"SystemTest"' then
    begin
      ScheduleSetting_ListView.Items[2].SubItems.Strings[1] := '系統檢測';
      if FileExists('Configs\v299_SystemTestCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_SystemTestCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[2].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"M64"' then
    begin
      ScheduleSetting_ListView.Items[2].SubItems.Strings[1] := 'M64';
      if FileExists('Configs\v299_M64CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_M64CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[2].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64N"' then
    begin
      ScheduleSetting_ListView.Items[2].SubItems.Strings[1] := 'S64N';
      if FileExists('Configs\v299_M64CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64NCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[2].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64HS"' then
    begin
      ScheduleSetting_ListView.Items[2].SubItems.Strings[1] := 'S64HS';
      if FileExists('Configs\v299_S64HSCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64HSCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[2].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64VHS"' then
    begin
      ScheduleSetting_ListView.Items[2].SubItems.Strings[1] := 'S64VHS';
      if FileExists('Configs\v299_S64VHSCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64VHSCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[2].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom01"' then
    begin
      ScheduleSetting_ListView.Items[2].SubItems.Strings[1] := 'Custom01';
      if FileExists('Configs\v299_Custom01CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom01CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[2].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom02"' then
    begin
      ScheduleSetting_ListView.Items[2].SubItems.Strings[1] := 'Custom02';
      if FileExists('Configs\v299_Custom02CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom02CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[2].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom03"' then
    begin
      ScheduleSetting_ListView.Items[2].SubItems.Strings[1] := 'Custom03';
      if FileExists('Configs\v299_Custom03CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom03CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[2].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 +
        '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') + '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_002_CurrentMode 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_002_MaxVoltage_V
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_002_MaxVoltage_V', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_002_MaxVoltage_V = ' + temp_str);
    if temp_str = '' then
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_002_MaxVoltage_V 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end
    else
    begin
      if ScheduleSetting_ListView.Items[2].SubItems.Strings[1] = '系統檢測' then
      begin
        ScheduleSetting_ListView.Items[2].SubItems.Strings[2] := '24V';
      end
      else
      begin
        ScheduleSetting_ListView.Items[2].SubItems.Strings[2] := Copy(temp_str, 2, Length(temp_str) - 2) + 'V';
      end
    end;
    //--
    // INI: [Schedule] -> JOB_002_MaxCurrent_A
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_002_MaxCurrent_A', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_002_MaxCurrent_A = ' + temp_str);
    if temp_str = '' then
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_002_MaxCurrent_A 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end
    else
    begin
      if ScheduleSetting_ListView.Items[2].SubItems.Strings[1] = '系統檢測' then
      begin
        ScheduleSetting_ListView.Items[2].SubItems.Strings[3] := '0.1A';
      end
      else
      begin
        ScheduleSetting_ListView.Items[2].SubItems.Strings[3] := Copy(temp_str, 2, Length(temp_str) - 2) + 'A';
      end
    end;
    //--
    // INI: [Schedule] -> JOB_003
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_003', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_003 = ' + temp_str);
    if (Length(temp_str) = 7) and (temp_str[1] = '"') and (temp_str[4] = ':') and (temp_str[7] = '"') and
      TryStrToTime(Copy(temp_str, 2, 5), temp_TDateTime) then
    begin
      ScheduleSetting_ListView.Items.Item[3].SubItems.Strings[0] := FormatDateTime('HH:MM', temp_TDateTime);
      ScheduleSetting_ListView.Items.Item[3].SubItems.Strings[4] := '未知';
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_003 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_003_Enable
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_003_Enable', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_003_Enable = ' + temp_str);
    if temp_str = '"true"' then
    begin
      ScheduleSetting_ListView.Items.Item[3].Checked := True;
    end
    else if temp_str = '"false"' then
    begin
      ScheduleSetting_ListView.Items.Item[3].Checked := False;
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_003_Enable 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_003_CurrentMode
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_003_CurrentMode', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_003_CurrentMode = ' + temp_str);
    if temp_str = '"SystemTest"' then
    begin
      ScheduleSetting_ListView.Items[3].SubItems.Strings[1] := '系統檢測';
      if FileExists('Configs\v299_SystemTestCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_SystemTestCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[3].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"M64"' then
    begin
      ScheduleSetting_ListView.Items[3].SubItems.Strings[1] := 'M64';
      if FileExists('Configs\v299_M64CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_M64CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[3].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64N"' then
    begin
      ScheduleSetting_ListView.Items[3].SubItems.Strings[1] := 'S64N';
      if FileExists('Configs\v299_M64CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64NCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[3].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64HS"' then
    begin
      ScheduleSetting_ListView.Items[3].SubItems.Strings[1] := 'S64HS';
      if FileExists('Configs\v299_S64HSCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64HSCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[3].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64VHS"' then
    begin
      ScheduleSetting_ListView.Items[3].SubItems.Strings[1] := 'S64VHS';
      if FileExists('Configs\v299_S64VHSCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64VHSCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[3].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom01"' then
    begin
      ScheduleSetting_ListView.Items[3].SubItems.Strings[1] := 'Custom01';
      if FileExists('Configs\v299_Custom01CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom01CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[3].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom02"' then
    begin
      ScheduleSetting_ListView.Items[3].SubItems.Strings[1] := 'Custom02';
      if FileExists('Configs\v299_Custom02CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom02CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[3].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom03"' then
    begin
      ScheduleSetting_ListView.Items[3].SubItems.Strings[1] := 'Custom03';
      if FileExists('Configs\v299_Custom03CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom03CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[3].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 +
        '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') + '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_003_CurrentMode 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_003_MaxVoltage_V
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_003_MaxVoltage_V', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_003_MaxVoltage_V = ' + temp_str);
    if temp_str = '' then
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_003_MaxVoltage_V 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end
    else
    begin
      if ScheduleSetting_ListView.Items[3].SubItems.Strings[1] = '系統檢測' then
      begin
        ScheduleSetting_ListView.Items[3].SubItems.Strings[2] := '24V';
      end
      else
      begin
        ScheduleSetting_ListView.Items[3].SubItems.Strings[2] := Copy(temp_str, 2, Length(temp_str) - 2) + 'V';
      end
    end;
    //--
    // INI: [Schedule] -> JOB_003_MaxCurrent_A
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_003_MaxCurrent_A', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_003_MaxCurrent_A = ' + temp_str);
    if temp_str = '' then
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_003_MaxCurrent_A 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end
    else
    begin
      if ScheduleSetting_ListView.Items[3].SubItems.Strings[1] = '系統檢測' then
      begin
        ScheduleSetting_ListView.Items[3].SubItems.Strings[3] := '0.1A';
      end
      else
      begin
        ScheduleSetting_ListView.Items[3].SubItems.Strings[3] := Copy(temp_str, 2, Length(temp_str) - 2) + 'A';
      end
    end;
    //--
    // INI: [Schedule] -> JOB_004
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_004', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_004 = ' + temp_str);
    if (Length(temp_str) = 7) and (temp_str[1] = '"') and (temp_str[4] = ':') and (temp_str[7] = '"') and
      TryStrToTime(Copy(temp_str, 2, 5), temp_TDateTime) then
    begin
      ScheduleSetting_ListView.Items.Item[4].SubItems.Strings[0] := FormatDateTime('HH:MM', temp_TDateTime);
      ScheduleSetting_ListView.Items.Item[4].SubItems.Strings[4] := '未知';
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_004 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_004_Enable
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_004_Enable', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_004_Enable = ' + temp_str);
    if temp_str = '"true"' then
    begin
      ScheduleSetting_ListView.Items.Item[4].Checked := True;
    end
    else if temp_str = '"false"' then
    begin
      ScheduleSetting_ListView.Items.Item[4].Checked := False;
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_004_Enable 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_004_CurrentMode
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_004_CurrentMode', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_004_CurrentMode = ' + temp_str);
    if temp_str = '"SystemTest"' then
    begin
      ScheduleSetting_ListView.Items[4].SubItems.Strings[1] := '系統檢測';
      if FileExists('Configs\v299_SystemTestCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_SystemTestCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[4].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"M64"' then
    begin
      ScheduleSetting_ListView.Items[4].SubItems.Strings[1] := 'M64';
      if FileExists('Configs\v299_M64CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_M64CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[4].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64N"' then
    begin
      ScheduleSetting_ListView.Items[4].SubItems.Strings[1] := 'S64N';
      if FileExists('Configs\v299_M64CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64NCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[4].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64HS"' then
    begin
      ScheduleSetting_ListView.Items[4].SubItems.Strings[1] := 'S64HS';
      if FileExists('Configs\v299_S64HSCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64HSCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[4].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64VHS"' then
    begin
      ScheduleSetting_ListView.Items[4].SubItems.Strings[1] := 'S64VHS';
      if FileExists('Configs\v299_S64VHSCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64VHSCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[4].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom01"' then
    begin
      ScheduleSetting_ListView.Items[4].SubItems.Strings[1] := 'Custom01';
      if FileExists('Configs\v299_Custom01CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom01CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[4].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom02"' then
    begin
      ScheduleSetting_ListView.Items[4].SubItems.Strings[1] := 'Custom02';
      if FileExists('Configs\v299_Custom02CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom02CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[4].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom03"' then
    begin
      ScheduleSetting_ListView.Items[4].SubItems.Strings[1] := 'Custom03';
      if FileExists('Configs\v299_Custom03CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom03CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[4].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 +
        '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') + '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_004_CurrentMode 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_004_MaxVoltage_V
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_004_MaxVoltage_V', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_004_MaxVoltage_V = ' + temp_str);
    if temp_str = '' then
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_004_MaxVoltage_V 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end
    else
    begin
      if ScheduleSetting_ListView.Items[4].SubItems.Strings[1] = '系統檢測' then
      begin
        ScheduleSetting_ListView.Items[4].SubItems.Strings[2] := '24V';
      end
      else
      begin
        ScheduleSetting_ListView.Items[4].SubItems.Strings[2] := Copy(temp_str, 2, Length(temp_str) - 2) + 'V';
      end
    end;
    //--
    // INI: [Schedule] -> JOB_004_MaxCurrent_A
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_004_MaxCurrent_A', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_004_MaxCurrent_A = ' + temp_str);
    if temp_str = '' then
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_004_MaxCurrent_A 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end
    else
    begin
      if ScheduleSetting_ListView.Items[4].SubItems.Strings[1] = '系統檢測' then
      begin
        ScheduleSetting_ListView.Items[4].SubItems.Strings[3] := '0.1A';
      end
      else
      begin
        ScheduleSetting_ListView.Items[4].SubItems.Strings[3] := Copy(temp_str, 2, Length(temp_str) - 2) + 'A';
      end
    end;
    //--
    // INI: [Schedule] -> JOB_005
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_005', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_005 = ' + temp_str);
    if (Length(temp_str) = 7) and (temp_str[1] = '"') and (temp_str[4] = ':') and (temp_str[7] = '"') and
      TryStrToTime(Copy(temp_str, 2, 5), temp_TDateTime) then
    begin
      ScheduleSetting_ListView.Items.Item[5].SubItems.Strings[0] := FormatDateTime('HH:MM', temp_TDateTime);
      ScheduleSetting_ListView.Items.Item[5].SubItems.Strings[4] := '未知';
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_005 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_005_Enable
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_005_Enable', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_005_Enable = ' + temp_str);
    if temp_str = '"true"' then
    begin
      ScheduleSetting_ListView.Items.Item[5].Checked := True;
    end
    else if temp_str = '"false"' then
    begin
      ScheduleSetting_ListView.Items.Item[5].Checked := False;
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_005_Enable 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_005_CurrentMode
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_005_CurrentMode', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_005_CurrentMode = ' + temp_str);
    if temp_str = '"SystemTest"' then
    begin
      ScheduleSetting_ListView.Items[5].SubItems.Strings[1] := '系統檢測';
      if FileExists('Configs\v299_SystemTestCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_SystemTestCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[5].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"M64"' then
    begin
      ScheduleSetting_ListView.Items[5].SubItems.Strings[1] := 'M64';
      if FileExists('Configs\v299_M64CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_M64CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[5].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64N"' then
    begin
      ScheduleSetting_ListView.Items[5].SubItems.Strings[1] := 'S64N';
      if FileExists('Configs\v299_M64CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64NCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[5].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64HS"' then
    begin
      ScheduleSetting_ListView.Items[5].SubItems.Strings[1] := 'S64HS';
      if FileExists('Configs\v299_S64HSCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64HSCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[5].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64VHS"' then
    begin
      ScheduleSetting_ListView.Items[5].SubItems.Strings[1] := 'S64VHS';
      if FileExists('Configs\v299_S64VHSCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64VHSCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[5].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom01"' then
    begin
      ScheduleSetting_ListView.Items[5].SubItems.Strings[1] := 'Custom01';
      if FileExists('Configs\v299_Custom01CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom01CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[5].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom02"' then
    begin
      ScheduleSetting_ListView.Items[5].SubItems.Strings[1] := 'Custom02';
      if FileExists('Configs\v299_Custom02CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom02CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[5].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom03"' then
    begin
      ScheduleSetting_ListView.Items[5].SubItems.Strings[1] := 'Custom03';
      if FileExists('Configs\v299_Custom03CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom03CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[5].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 +
        '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') + '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_005_CurrentMode 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_005_MaxVoltage_V
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_005_MaxVoltage_V', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_005_MaxVoltage_V = ' + temp_str);
    if temp_str = '' then
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_005_MaxVoltage_V 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end
    else
    begin
      if ScheduleSetting_ListView.Items[5].SubItems.Strings[1] = '系統檢測' then
      begin
        ScheduleSetting_ListView.Items[5].SubItems.Strings[2] := '24V';
      end
      else
      begin
        ScheduleSetting_ListView.Items[5].SubItems.Strings[2] := Copy(temp_str, 2, Length(temp_str) - 2) + 'V';
      end
    end;
    //--
    // INI: [Schedule] -> JOB_005_MaxCurrent_A
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_005_MaxCurrent_A', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_005_MaxCurrent_A = ' + temp_str);
    if temp_str = '' then
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_005_MaxCurrent_A 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end
    else
    begin
      if ScheduleSetting_ListView.Items[5].SubItems.Strings[1] = '系統檢測' then
      begin
        ScheduleSetting_ListView.Items[5].SubItems.Strings[3] := '0.1A';
      end
      else
      begin
        ScheduleSetting_ListView.Items[5].SubItems.Strings[3] := Copy(temp_str, 2, Length(temp_str) - 2) + 'A';
      end
    end;
    //--
    // INI: [Schedule] -> JOB_006
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_006', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_006 = ' + temp_str);
    if (Length(temp_str) = 7) and (temp_str[1] = '"') and (temp_str[4] = ':') and (temp_str[7] = '"') and
      TryStrToTime(Copy(temp_str, 2, 5), temp_TDateTime) then
    begin
      ScheduleSetting_ListView.Items.Item[6].SubItems.Strings[0] := FormatDateTime('HH:MM', temp_TDateTime);
      ScheduleSetting_ListView.Items.Item[6].SubItems.Strings[4] := '未知';
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_006 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_006_Enable
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_006_Enable', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_006_Enable = ' + temp_str);
    if temp_str = '"true"' then
    begin
      ScheduleSetting_ListView.Items.Item[6].Checked := True;
    end
    else if temp_str = '"false"' then
    begin
      ScheduleSetting_ListView.Items.Item[6].Checked := False;
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_006_Enable 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_006_CurrentMode
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_006_CurrentMode', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_006_CurrentMode = ' + temp_str);
    if temp_str = '"SystemTest"' then
    begin
      ScheduleSetting_ListView.Items[6].SubItems.Strings[1] := '系統檢測';
      if FileExists('Configs\v299_SystemTestCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_SystemTestCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[6].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"M64"' then
    begin
      ScheduleSetting_ListView.Items[6].SubItems.Strings[1] := 'M64';
      if FileExists('Configs\v299_M64CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_M64CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[6].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64N"' then
    begin
      ScheduleSetting_ListView.Items[6].SubItems.Strings[1] := 'S64N';
      if FileExists('Configs\v299_M64CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64NCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[6].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64HS"' then
    begin
      ScheduleSetting_ListView.Items[6].SubItems.Strings[1] := 'S64HS';
      if FileExists('Configs\v299_S64HSCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64HSCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[6].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64VHS"' then
    begin
      ScheduleSetting_ListView.Items[6].SubItems.Strings[1] := 'S64VHS';
      if FileExists('Configs\v299_S64VHSCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64VHSCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[6].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom01"' then
    begin
      ScheduleSetting_ListView.Items[6].SubItems.Strings[1] := 'Custom01';
      if FileExists('Configs\v299_Custom01CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom01CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[6].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom02"' then
    begin
      ScheduleSetting_ListView.Items[6].SubItems.Strings[1] := 'Custom02';
      if FileExists('Configs\v299_Custom02CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom02CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[6].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom03"' then
    begin
      ScheduleSetting_ListView.Items[6].SubItems.Strings[1] := 'Custom03';
      if FileExists('Configs\v299_Custom03CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom03CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[6].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 +
        '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') + '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_006_CurrentMode 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_006_MaxVoltage_V
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_006_MaxVoltage_V', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_006_MaxVoltage_V = ' + temp_str);
    if temp_str = '' then
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_006_MaxVoltage_V 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end
    else
    begin
      if ScheduleSetting_ListView.Items[6].SubItems.Strings[1] = '系統檢測' then
      begin
        ScheduleSetting_ListView.Items[6].SubItems.Strings[2] := '24V';
      end
      else
      begin
        ScheduleSetting_ListView.Items[6].SubItems.Strings[2] := Copy(temp_str, 2, Length(temp_str) - 2) + 'V';
      end
    end;
    //--
    // INI: [Schedule] -> JOB_006_MaxCurrent_A
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_006_MaxCurrent_A', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_006_MaxCurrent_A = ' + temp_str);
    if temp_str = '' then
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_006_MaxCurrent_A 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end
    else
    begin
      if ScheduleSetting_ListView.Items[6].SubItems.Strings[1] = '系統檢測' then
      begin
        ScheduleSetting_ListView.Items[6].SubItems.Strings[3] := '0.1A';
      end
      else
      begin
        ScheduleSetting_ListView.Items[6].SubItems.Strings[3] := Copy(temp_str, 2, Length(temp_str) - 2) + 'A';
      end
    end;
    //--
    // INI: [Schedule] -> JOB_007
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_007', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_007 = ' + temp_str);
    if (Length(temp_str) = 7) and (temp_str[1] = '"') and (temp_str[4] = ':') and (temp_str[7] = '"') and
      TryStrToTime(Copy(temp_str, 2, 5), temp_TDateTime) then
    begin
      ScheduleSetting_ListView.Items.Item[7].SubItems.Strings[0] := FormatDateTime('HH:MM', temp_TDateTime);
      ScheduleSetting_ListView.Items.Item[7].SubItems.Strings[4] := '未知';
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_007 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_007_Enable
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_007_Enable', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_007_Enable = ' + temp_str);
    if temp_str = '"true"' then
    begin
      ScheduleSetting_ListView.Items.Item[7].Checked := True;
    end
    else if temp_str = '"false"' then
    begin
      ScheduleSetting_ListView.Items.Item[7].Checked := False;
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_007_Enable 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_007_CurrentMode
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_007_CurrentMode', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_007_CurrentMode = ' + temp_str);
    if temp_str = '"SystemTest"' then
    begin
      ScheduleSetting_ListView.Items[7].SubItems.Strings[1] := '系統檢測';
      if FileExists('Configs\v299_SystemTestCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_SystemTestCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[7].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"M64"' then
    begin
      ScheduleSetting_ListView.Items[7].SubItems.Strings[1] := 'M64';
      if FileExists('Configs\v299_M64CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_M64CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[7].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64N"' then
    begin
      ScheduleSetting_ListView.Items[7].SubItems.Strings[1] := 'S64N';
      if FileExists('Configs\v299_M64CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64NCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[7].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64HS"' then
    begin
      ScheduleSetting_ListView.Items[7].SubItems.Strings[1] := 'S64HS';
      if FileExists('Configs\v299_S64HSCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64HSCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[7].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64VHS"' then
    begin
      ScheduleSetting_ListView.Items[7].SubItems.Strings[1] := 'S64VHS';
      if FileExists('Configs\v299_S64VHSCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64VHSCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[7].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom01"' then
    begin
      ScheduleSetting_ListView.Items[7].SubItems.Strings[1] := 'Custom01';
      if FileExists('Configs\v299_Custom01CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom01CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[7].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom02"' then
    begin
      ScheduleSetting_ListView.Items[7].SubItems.Strings[1] := 'Custom02';
      if FileExists('Configs\v299_Custom02CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom02CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[7].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom03"' then
    begin
      ScheduleSetting_ListView.Items[7].SubItems.Strings[1] := 'Custom03';
      if FileExists('Configs\v299_Custom03CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom03CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[7].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 +
        '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') + '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_007_CurrentMode 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_007_MaxVoltage_V
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_007_MaxVoltage_V', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_007_MaxVoltage_V = ' + temp_str);
    if temp_str = '' then
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_007_MaxVoltage_V 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end
    else
    begin
      if ScheduleSetting_ListView.Items[7].SubItems.Strings[1] = '系統檢測' then
      begin
        ScheduleSetting_ListView.Items[7].SubItems.Strings[2] := '24V';
      end
      else
      begin
        ScheduleSetting_ListView.Items[7].SubItems.Strings[2] := Copy(temp_str, 2, Length(temp_str) - 2) + 'V';
      end
    end;
    //--
    // INI: [Schedule] -> JOB_007_MaxCurrent_A
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_007_MaxCurrent_A', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_007_MaxCurrent_A = ' + temp_str);
    if temp_str = '' then
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_007_MaxCurrent_A 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end
    else
    begin
      if ScheduleSetting_ListView.Items[7].SubItems.Strings[1] = '系統檢測' then
      begin
        ScheduleSetting_ListView.Items[7].SubItems.Strings[3] := '0.1A';
      end
      else
      begin
        ScheduleSetting_ListView.Items[7].SubItems.Strings[3] := Copy(temp_str, 2, Length(temp_str) - 2) + 'A';
      end
    end;
    //--
    // INI: [Schedule] -> JOB_008
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_008', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_008 = ' + temp_str);
    if (Length(temp_str) = 7) and (temp_str[1] = '"') and (temp_str[4] = ':') and (temp_str[7] = '"') and
      TryStrToTime(Copy(temp_str, 2, 5), temp_TDateTime) then
    begin
      ScheduleSetting_ListView.Items.Item[8].SubItems.Strings[0] := FormatDateTime('HH:MM', temp_TDateTime);
      ScheduleSetting_ListView.Items.Item[8].SubItems.Strings[4] := '未知';
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_008 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_008_Enable
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_008_Enable', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_008_Enable = ' + temp_str);
    if temp_str = '"true"' then
    begin
      ScheduleSetting_ListView.Items.Item[8].Checked := True;
    end
    else if temp_str = '"false"' then
    begin
      ScheduleSetting_ListView.Items.Item[8].Checked := False;
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_008_Enable 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_008_CurrentMode
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_008_CurrentMode', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_008_CurrentMode = ' + temp_str);
    if temp_str = '"SystemTest"' then
    begin
      ScheduleSetting_ListView.Items[8].SubItems.Strings[1] := '系統檢測';
      if FileExists('Configs\v299_SystemTestCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_SystemTestCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[8].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"M64"' then
    begin
      ScheduleSetting_ListView.Items[8].SubItems.Strings[1] := 'M64';
      if FileExists('Configs\v299_M64CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_M64CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[8].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64N"' then
    begin
      ScheduleSetting_ListView.Items[8].SubItems.Strings[1] := 'S64N';
      if FileExists('Configs\v299_M64CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64NCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[8].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64HS"' then
    begin
      ScheduleSetting_ListView.Items[8].SubItems.Strings[1] := 'S64HS';
      if FileExists('Configs\v299_S64HSCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64HSCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[8].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64VHS"' then
    begin
      ScheduleSetting_ListView.Items[8].SubItems.Strings[1] := 'S64VHS';
      if FileExists('Configs\v299_S64VHSCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64VHSCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[8].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom01"' then
    begin
      ScheduleSetting_ListView.Items[8].SubItems.Strings[1] := 'Custom01';
      if FileExists('Configs\v299_Custom01CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom01CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[8].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom02"' then
    begin
      ScheduleSetting_ListView.Items[8].SubItems.Strings[1] := 'Custom02';
      if FileExists('Configs\v299_Custom02CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom02CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[8].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom03"' then
    begin
      ScheduleSetting_ListView.Items[8].SubItems.Strings[1] := 'Custom03';
      if FileExists('Configs\v299_Custom03CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom03CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[8].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 +
        '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') + '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_008_CurrentMode 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_008_MaxVoltage_V
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_008_MaxVoltage_V', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_008_MaxVoltage_V = ' + temp_str);
    if temp_str = '' then
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_008_MaxVoltage_V 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end
    else
    begin
      if ScheduleSetting_ListView.Items[8].SubItems.Strings[1] = '系統檢測' then
      begin
        ScheduleSetting_ListView.Items[8].SubItems.Strings[2] := '24V';
      end
      else
      begin
        ScheduleSetting_ListView.Items[8].SubItems.Strings[2] := Copy(temp_str, 2, Length(temp_str) - 2) + 'V';
      end
    end;
    //--
    // INI: [Schedule] -> JOB_008_MaxCurrent_A
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_008_MaxCurrent_A', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_008_MaxCurrent_A = ' + temp_str);
    if temp_str = '' then
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_008_MaxCurrent_A 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end
    else
    begin
      if ScheduleSetting_ListView.Items[8].SubItems.Strings[1] = '系統檢測' then
      begin
        ScheduleSetting_ListView.Items[8].SubItems.Strings[3] := '0.1A';
      end
      else
      begin
        ScheduleSetting_ListView.Items[8].SubItems.Strings[3] := Copy(temp_str, 2, Length(temp_str) - 2) + 'A';
      end
    end;
    //--
    // INI: [Schedule] -> JOB_009
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_009', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_009 = ' + temp_str);
    if (Length(temp_str) = 7) and (temp_str[1] = '"') and (temp_str[4] = ':') and (temp_str[7] = '"') and
      TryStrToTime(Copy(temp_str, 2, 5), temp_TDateTime) then
    begin
      ScheduleSetting_ListView.Items.Item[9].SubItems.Strings[0] := FormatDateTime('HH:MM', temp_TDateTime);
      ScheduleSetting_ListView.Items.Item[9].SubItems.Strings[4] := '未知';
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_009 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_009_Enable
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_009_Enable', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_009_Enable = ' + temp_str);
    if temp_str = '"true"' then
    begin
      ScheduleSetting_ListView.Items.Item[9].Checked := True;
    end
    else if temp_str = '"false"' then
    begin
      ScheduleSetting_ListView.Items.Item[9].Checked := False;
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_009_Enable 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_009_CurrentMode
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_009_CurrentMode', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_009_CurrentMode = ' + temp_str);
    if temp_str = '"SystemTest"' then
    begin
      ScheduleSetting_ListView.Items[9].SubItems.Strings[1] := '系統檢測';
      if FileExists('Configs\v299_SystemTestCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_SystemTestCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[9].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"M64"' then
    begin
      ScheduleSetting_ListView.Items[9].SubItems.Strings[1] := 'M64';
      if FileExists('Configs\v299_M64CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_M64CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[9].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64N"' then
    begin
      ScheduleSetting_ListView.Items[9].SubItems.Strings[1] := 'S64N';
      if FileExists('Configs\v299_M64CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64NCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[9].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64HS"' then
    begin
      ScheduleSetting_ListView.Items[9].SubItems.Strings[1] := 'S64HS';
      if FileExists('Configs\v299_S64HSCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64HSCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[9].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64VHS"' then
    begin
      ScheduleSetting_ListView.Items[9].SubItems.Strings[1] := 'S64VHS';
      if FileExists('Configs\v299_S64VHSCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64VHSCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[9].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom01"' then
    begin
      ScheduleSetting_ListView.Items[9].SubItems.Strings[1] := 'Custom01';
      if FileExists('Configs\v299_Custom01CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom01CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[9].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom02"' then
    begin
      ScheduleSetting_ListView.Items[9].SubItems.Strings[1] := 'Custom02';
      if FileExists('Configs\v299_Custom02CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom02CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[9].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom03"' then
    begin
      ScheduleSetting_ListView.Items[9].SubItems.Strings[1] := 'Custom03';
      if FileExists('Configs\v299_Custom03CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom03CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[9].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 +
        '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') + '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_009_CurrentMode 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_009_MaxVoltage_V
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_009_MaxVoltage_V', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_009_MaxVoltage_V = ' + temp_str);
    if temp_str = '' then
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_009_MaxVoltage_V 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end
    else
    begin
      if ScheduleSetting_ListView.Items[9].SubItems.Strings[1] = '系統檢測' then
      begin
        ScheduleSetting_ListView.Items[9].SubItems.Strings[2] := '24V';
      end
      else
      begin
        ScheduleSetting_ListView.Items[9].SubItems.Strings[2] := Copy(temp_str, 2, Length(temp_str) - 2) + 'V';
      end
    end;
    //--
    // INI: [Schedule] -> JOB_009_MaxCurrent_A
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_009_MaxCurrent_A', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_009_MaxCurrent_A = ' + temp_str);
    if temp_str = '' then
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_009_MaxCurrent_A 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end
    else
    begin
      if ScheduleSetting_ListView.Items[9].SubItems.Strings[1] = '系統檢測' then
      begin
        ScheduleSetting_ListView.Items[9].SubItems.Strings[3] := '0.1A';
      end
      else
      begin
        ScheduleSetting_ListView.Items[9].SubItems.Strings[3] := Copy(temp_str, 2, Length(temp_str) - 2) + 'A';
      end
    end;
    //--
    // INI: [Schedule] -> JOB_010
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_010', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_010 = ' + temp_str);
    if (Length(temp_str) = 7) and (temp_str[1] = '"') and (temp_str[4] = ':') and (temp_str[7] = '"') and
      TryStrToTime(Copy(temp_str, 2, 5), temp_TDateTime) then
    begin
      ScheduleSetting_ListView.Items.Item[10].SubItems.Strings[0] := FormatDateTime('HH:MM', temp_TDateTime);
      ScheduleSetting_ListView.Items.Item[10].SubItems.Strings[4] := '未知';
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_010 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_010_Enable
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_010_Enable', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_010_Enable = ' + temp_str);
    if temp_str = '"true"' then
    begin
      ScheduleSetting_ListView.Items.Item[10].Checked := True;
    end
    else if temp_str = '"false"' then
    begin
      ScheduleSetting_ListView.Items.Item[10].Checked := False;
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_010_Enable 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_010_CurrentMode
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_010_CurrentMode', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_010_CurrentMode = ' + temp_str);
    if temp_str = '"SystemTest"' then
    begin
      ScheduleSetting_ListView.Items[10].SubItems.Strings[1] := '系統檢測';
      if FileExists('Configs\v299_SystemTestCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_SystemTestCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[10].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"M64"' then
    begin
      ScheduleSetting_ListView.Items[10].SubItems.Strings[1] := 'M64';
      if FileExists('Configs\v299_M64CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_M64CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[10].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64N"' then
    begin
      ScheduleSetting_ListView.Items[10].SubItems.Strings[1] := 'S64N';
      if FileExists('Configs\v299_M64CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64NCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[10].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64HS"' then
    begin
      ScheduleSetting_ListView.Items[10].SubItems.Strings[1] := 'S64HS';
      if FileExists('Configs\v299_S64HSCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64HSCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[10].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64VHS"' then
    begin
      ScheduleSetting_ListView.Items[10].SubItems.Strings[1] := 'S64VHS';
      if FileExists('Configs\v299_S64VHSCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64VHSCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[10].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom01"' then
    begin
      ScheduleSetting_ListView.Items[10].SubItems.Strings[1] := 'Custom01';
      if FileExists('Configs\v299_Custom01CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom01CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[10].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom02"' then
    begin
      ScheduleSetting_ListView.Items[10].SubItems.Strings[1] := 'Custom02';
      if FileExists('Configs\v299_Custom02CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom02CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[10].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom03"' then
    begin
      ScheduleSetting_ListView.Items[10].SubItems.Strings[1] := 'Custom03';
      if FileExists('Configs\v299_Custom03CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom03CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[10].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 +
        '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') + '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_010_CurrentMode 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_010_MaxVoltage_V
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_010_MaxVoltage_V', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_010_MaxVoltage_V = ' + temp_str);
    if temp_str = '' then
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_010_MaxVoltage_V 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end
    else
    begin
      if ScheduleSetting_ListView.Items[10].SubItems.Strings[1] = '系統檢測' then
      begin
        ScheduleSetting_ListView.Items[10].SubItems.Strings[2] := '24V';
      end
      else
      begin
        ScheduleSetting_ListView.Items[10].SubItems.Strings[2] := Copy(temp_str, 2, Length(temp_str) - 2) + 'V';
      end
    end;
    //--
    // INI: [Schedule] -> JOB_010_MaxCurrent_A
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_010_MaxCurrent_A', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_010_MaxCurrent_A = ' + temp_str);
    if temp_str = '' then
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_010_MaxCurrent_A 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end
    else
    begin
      if ScheduleSetting_ListView.Items[10].SubItems.Strings[1] = '系統檢測' then
      begin
        ScheduleSetting_ListView.Items[10].SubItems.Strings[3] := '0.1A';
      end
      else
      begin
        ScheduleSetting_ListView.Items[10].SubItems.Strings[3] := Copy(temp_str, 2, Length(temp_str) - 2) + 'A';
      end
    end;
    //--
    // INI: [Schedule] -> JOB_011
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_011', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_011 = ' + temp_str);
    if (Length(temp_str) = 7) and (temp_str[1] = '"') and (temp_str[4] = ':') and (temp_str[7] = '"') and
      TryStrToTime(Copy(temp_str, 2, 5), temp_TDateTime) then
    begin
      ScheduleSetting_ListView.Items.Item[11].SubItems.Strings[0] := FormatDateTime('HH:MM', temp_TDateTime);
      ScheduleSetting_ListView.Items.Item[11].SubItems.Strings[4] := '未知';
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_011 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_011_Enable
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_011_Enable', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_011_Enable = ' + temp_str);
    if temp_str = '"true"' then
    begin
      ScheduleSetting_ListView.Items.Item[11].Checked := True;
    end
    else if temp_str = '"false"' then
    begin
      ScheduleSetting_ListView.Items.Item[11].Checked := False;
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_011_Enable 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_011_CurrentMode
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_011_CurrentMode', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_011_CurrentMode = ' + temp_str);
    if temp_str = '"SystemTest"' then
    begin
      ScheduleSetting_ListView.Items[11].SubItems.Strings[1] := '系統檢測';
      if FileExists('Configs\v299_SystemTestCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_SystemTestCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[11].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"M64"' then
    begin
      ScheduleSetting_ListView.Items[11].SubItems.Strings[1] := 'M64';
      if FileExists('Configs\v299_M64CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_M64CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[11].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64N"' then
    begin
      ScheduleSetting_ListView.Items[11].SubItems.Strings[1] := 'S64N';
      if FileExists('Configs\v299_M64CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64NCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[11].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64HS"' then
    begin
      ScheduleSetting_ListView.Items[11].SubItems.Strings[1] := 'S64HS';
      if FileExists('Configs\v299_S64HSCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64HSCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[11].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64VHS"' then
    begin
      ScheduleSetting_ListView.Items[11].SubItems.Strings[1] := 'S64VHS';
      if FileExists('Configs\v299_S64VHSCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64VHSCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[11].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom01"' then
    begin
      ScheduleSetting_ListView.Items[11].SubItems.Strings[1] := 'Custom01';
      if FileExists('Configs\v299_Custom01CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom01CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[11].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom02"' then
    begin
      ScheduleSetting_ListView.Items[11].SubItems.Strings[1] := 'Custom02';
      if FileExists('Configs\v299_Custom02CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom02CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[11].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom03"' then
    begin
      ScheduleSetting_ListView.Items[11].SubItems.Strings[1] := 'Custom03';
      if FileExists('Configs\v299_Custom03CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom03CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[11].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 +
        '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') + '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_011_CurrentMode 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_011_MaxVoltage_V
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_011_MaxVoltage_V', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_011_MaxVoltage_V = ' + temp_str);
    if temp_str = '' then
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_011_MaxVoltage_V 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end
    else
    begin
      if ScheduleSetting_ListView.Items[11].SubItems.Strings[1] = '系統檢測' then
      begin
        ScheduleSetting_ListView.Items[11].SubItems.Strings[2] := '24V';
      end
      else
      begin
        ScheduleSetting_ListView.Items[11].SubItems.Strings[2] := Copy(temp_str, 2, Length(temp_str) - 2) + 'V';
      end
    end;
    //--
    // INI: [Schedule] -> JOB_011_MaxCurrent_A
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_011_MaxCurrent_A', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_011_MaxCurrent_A = ' + temp_str);
    if temp_str = '' then
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_011_MaxCurrent_A 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end
    else
    begin
      if ScheduleSetting_ListView.Items[11].SubItems.Strings[1] = '系統檢測' then
      begin
        ScheduleSetting_ListView.Items[11].SubItems.Strings[3] := '0.1A';
      end
      else
      begin
        ScheduleSetting_ListView.Items[11].SubItems.Strings[3] := Copy(temp_str, 2, Length(temp_str) - 2) + 'A';
      end
    end;
    //--
    // INI: [Schedule] -> JOB_012
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_012', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_012 = ' + temp_str);
    if (Length(temp_str) = 7) and (temp_str[1] = '"') and (temp_str[4] = ':') and (temp_str[7] = '"') and
      TryStrToTime(Copy(temp_str, 2, 5), temp_TDateTime) then
    begin
      ScheduleSetting_ListView.Items.Item[12].SubItems.Strings[0] := FormatDateTime('HH:MM', temp_TDateTime);
      ScheduleSetting_ListView.Items.Item[12].SubItems.Strings[4] := '未知';
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_012 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_012_Enable
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_012_Enable', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_012_Enable = ' + temp_str);
    if temp_str = '"true"' then
    begin
      ScheduleSetting_ListView.Items.Item[12].Checked := True;
    end
    else if temp_str = '"false"' then
    begin
      ScheduleSetting_ListView.Items.Item[12].Checked := False;
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_012_Enable 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_012_CurrentMode
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_012_CurrentMode', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_012_CurrentMode = ' + temp_str);
    if temp_str = '"SystemTest"' then
    begin
      ScheduleSetting_ListView.Items[12].SubItems.Strings[1] := '系統檢測';
      if FileExists('Configs\v299_SystemTestCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_SystemTestCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[12].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"M64"' then
    begin
      ScheduleSetting_ListView.Items[12].SubItems.Strings[1] := 'M64';
      if FileExists('Configs\v299_M64CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_M64CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[12].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64N"' then
    begin
      ScheduleSetting_ListView.Items[12].SubItems.Strings[1] := 'S64N';
      if FileExists('Configs\v299_M64CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64NCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[12].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64HS"' then
    begin
      ScheduleSetting_ListView.Items[12].SubItems.Strings[1] := 'S64HS';
      if FileExists('Configs\v299_S64HSCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64HSCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[12].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64VHS"' then
    begin
      ScheduleSetting_ListView.Items[12].SubItems.Strings[1] := 'S64VHS';
      if FileExists('Configs\v299_S64VHSCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64VHSCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[12].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom01"' then
    begin
      ScheduleSetting_ListView.Items[12].SubItems.Strings[1] := 'Custom01';
      if FileExists('Configs\v299_Custom01CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom01CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[12].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom02"' then
    begin
      ScheduleSetting_ListView.Items[12].SubItems.Strings[1] := 'Custom02';
      if FileExists('Configs\v299_Custom02CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom02CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[12].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom03"' then
    begin
      ScheduleSetting_ListView.Items[12].SubItems.Strings[1] := 'Custom03';
      if FileExists('Configs\v299_Custom03CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom03CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[12].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 +
        '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') + '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_012_CurrentMode 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_012_MaxVoltage_V
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_012_MaxVoltage_V', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_012_MaxVoltage_V = ' + temp_str);
    if temp_str = '' then
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_012_MaxVoltage_V 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end
    else
    begin
      if ScheduleSetting_ListView.Items[12].SubItems.Strings[1] = '系統檢測' then
      begin
        ScheduleSetting_ListView.Items[12].SubItems.Strings[2] := '24V';
      end
      else
      begin
        ScheduleSetting_ListView.Items[12].SubItems.Strings[2] := Copy(temp_str, 2, Length(temp_str) - 2) + 'V';
      end
    end;
    //--
    // INI: [Schedule] -> JOB_012_MaxCurrent_A
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_012_MaxCurrent_A', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_012_MaxCurrent_A = ' + temp_str);
    if temp_str = '' then
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_012_MaxCurrent_A 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end
    else
    begin
      if ScheduleSetting_ListView.Items[12].SubItems.Strings[1] = '系統檢測' then
      begin
        ScheduleSetting_ListView.Items[12].SubItems.Strings[3] := '0.1A';
      end
      else
      begin
        ScheduleSetting_ListView.Items[12].SubItems.Strings[3] := Copy(temp_str, 2, Length(temp_str) - 2) + 'A';
      end
    end;
    //--
    // INI: [Schedule] -> JOB_013
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_013', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_013 = ' + temp_str);
    if (Length(temp_str) = 7) and (temp_str[1] = '"') and (temp_str[4] = ':') and (temp_str[7] = '"') and
      TryStrToTime(Copy(temp_str, 2, 5), temp_TDateTime) then
    begin
      ScheduleSetting_ListView.Items.Item[13].SubItems.Strings[0] := FormatDateTime('HH:MM', temp_TDateTime);
      ScheduleSetting_ListView.Items.Item[13].SubItems.Strings[4] := '未知';
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_013 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_013_Enable
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_013_Enable', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_013_Enable = ' + temp_str);
    if temp_str = '"true"' then
    begin
      ScheduleSetting_ListView.Items.Item[13].Checked := True;
    end
    else if temp_str = '"false"' then
    begin
      ScheduleSetting_ListView.Items.Item[13].Checked := False;
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_013_Enable 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_013_CurrentMode
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_013_CurrentMode', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_013_CurrentMode = ' + temp_str);
    if temp_str = '"SystemTest"' then
    begin
      ScheduleSetting_ListView.Items[13].SubItems.Strings[1] := '系統檢測';
      if FileExists('Configs\v299_SystemTestCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_SystemTestCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[13].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"M64"' then
    begin
      ScheduleSetting_ListView.Items[13].SubItems.Strings[1] := 'M64';
      if FileExists('Configs\v299_M64CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_M64CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[13].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64N"' then
    begin
      ScheduleSetting_ListView.Items[13].SubItems.Strings[1] := 'S64N';
      if FileExists('Configs\v299_M64CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64NCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[13].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64HS"' then
    begin
      ScheduleSetting_ListView.Items[13].SubItems.Strings[1] := 'S64HS';
      if FileExists('Configs\v299_S64HSCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64HSCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[13].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64VHS"' then
    begin
      ScheduleSetting_ListView.Items[13].SubItems.Strings[1] := 'S64VHS';
      if FileExists('Configs\v299_S64VHSCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64VHSCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[13].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom01"' then
    begin
      ScheduleSetting_ListView.Items[13].SubItems.Strings[1] := 'Custom01';
      if FileExists('Configs\v299_Custom01CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom01CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[13].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom02"' then
    begin
      ScheduleSetting_ListView.Items[13].SubItems.Strings[1] := 'Custom02';
      if FileExists('Configs\v299_Custom02CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom02CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[13].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom03"' then
    begin
      ScheduleSetting_ListView.Items[13].SubItems.Strings[1] := 'Custom03';
      if FileExists('Configs\v299_Custom03CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom03CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[13].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 +
        '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') + '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_013_CurrentMode 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_013_MaxVoltage_V
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_013_MaxVoltage_V', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_013_MaxVoltage_V = ' + temp_str);
    if temp_str = '' then
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_013_MaxVoltage_V 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end
    else
    begin
      if ScheduleSetting_ListView.Items[13].SubItems.Strings[1] = '系統檢測' then
      begin
        ScheduleSetting_ListView.Items[13].SubItems.Strings[2] := '24V';
      end
      else
      begin
        ScheduleSetting_ListView.Items[13].SubItems.Strings[2] := Copy(temp_str, 2, Length(temp_str) - 2) + 'V';
      end
    end;
    //--
    // INI: [Schedule] -> JOB_013_MaxCurrent_A
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_013_MaxCurrent_A', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_013_MaxCurrent_A = ' + temp_str);
    if temp_str = '' then
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_013_MaxCurrent_A 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end
    else
    begin
      if ScheduleSetting_ListView.Items[13].SubItems.Strings[1] = '系統檢測' then
      begin
        ScheduleSetting_ListView.Items[13].SubItems.Strings[3] := '0.1A';
      end
      else
      begin
        ScheduleSetting_ListView.Items[13].SubItems.Strings[3] := Copy(temp_str, 2, Length(temp_str) - 2) + 'A';
      end
    end;
    //--
    // INI: [Schedule] -> JOB_014
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_014', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_014 = ' + temp_str);
    if (Length(temp_str) = 7) and (temp_str[1] = '"') and (temp_str[4] = ':') and (temp_str[7] = '"') and
      TryStrToTime(Copy(temp_str, 2, 5), temp_TDateTime) then
    begin
      ScheduleSetting_ListView.Items.Item[14].SubItems.Strings[0] := FormatDateTime('HH:MM', temp_TDateTime);
      ScheduleSetting_ListView.Items.Item[14].SubItems.Strings[4] := '未知';
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_014 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_014_Enable
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_014_Enable', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_014_Enable = ' + temp_str);
    if temp_str = '"true"' then
    begin
      ScheduleSetting_ListView.Items.Item[14].Checked := True;
    end
    else if temp_str = '"false"' then
    begin
      ScheduleSetting_ListView.Items.Item[14].Checked := False;
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_014_Enable 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_014_CurrentMode
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_014_CurrentMode', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_014_CurrentMode = ' + temp_str);
    if temp_str = '"SystemTest"' then
    begin
      ScheduleSetting_ListView.Items[14].SubItems.Strings[1] := '系統檢測';
      if FileExists('Configs\v299_SystemTestCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_SystemTestCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[14].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"M64"' then
    begin
      ScheduleSetting_ListView.Items[14].SubItems.Strings[1] := 'M64';
      if FileExists('Configs\v299_M64CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_M64CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[14].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64N"' then
    begin
      ScheduleSetting_ListView.Items[14].SubItems.Strings[1] := 'S64N';
      if FileExists('Configs\v299_M64CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64NCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[14].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64HS"' then
    begin
      ScheduleSetting_ListView.Items[14].SubItems.Strings[1] := 'S64HS';
      if FileExists('Configs\v299_S64HSCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64HSCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[14].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64VHS"' then
    begin
      ScheduleSetting_ListView.Items[14].SubItems.Strings[1] := 'S64VHS';
      if FileExists('Configs\v299_S64VHSCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64VHSCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[14].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom01"' then
    begin
      ScheduleSetting_ListView.Items[14].SubItems.Strings[1] := 'Custom01';
      if FileExists('Configs\v299_Custom01CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom01CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[14].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom02"' then
    begin
      ScheduleSetting_ListView.Items[14].SubItems.Strings[1] := 'Custom02';
      if FileExists('Configs\v299_Custom02CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom02CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[14].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom03"' then
    begin
      ScheduleSetting_ListView.Items[14].SubItems.Strings[1] := 'Custom03';
      if FileExists('Configs\v299_Custom03CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom03CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[14].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 +
        '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') + '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_014_CurrentMode 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_014_MaxVoltage_V
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_014_MaxVoltage_V', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_014_MaxVoltage_V = ' + temp_str);
    if temp_str = '' then
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_014_MaxVoltage_V 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end
    else
    begin
      if ScheduleSetting_ListView.Items[14].SubItems.Strings[1] = '系統檢測' then
      begin
        ScheduleSetting_ListView.Items[14].SubItems.Strings[2] := '24V';
      end
      else
      begin
        ScheduleSetting_ListView.Items[14].SubItems.Strings[2] := Copy(temp_str, 2, Length(temp_str) - 2) + 'V';
      end
    end;
    //--
    // INI: [Schedule] -> JOB_014_MaxCurrent_A
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_014_MaxCurrent_A', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_014_MaxCurrent_A = ' + temp_str);
    if temp_str = '' then
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_014_MaxCurrent_A 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end
    else
    begin
      if ScheduleSetting_ListView.Items[14].SubItems.Strings[1] = '系統檢測' then
      begin
        ScheduleSetting_ListView.Items[14].SubItems.Strings[3] := '0.1A';
      end
      else
      begin
        ScheduleSetting_ListView.Items[14].SubItems.Strings[3] := Copy(temp_str, 2, Length(temp_str) - 2) + 'A';
      end
    end;
    //--
    // INI: [Schedule] -> JOB_015
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_015', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_015 = ' + temp_str);
    if (Length(temp_str) = 7) and (temp_str[1] = '"') and (temp_str[4] = ':') and (temp_str[7] = '"') and
      TryStrToTime(Copy(temp_str, 2, 5), temp_TDateTime) then
    begin
      ScheduleSetting_ListView.Items.Item[15].SubItems.Strings[0] := FormatDateTime('HH:MM', temp_TDateTime);
      ScheduleSetting_ListView.Items.Item[15].SubItems.Strings[4] := '未知';
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_015 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_015_Enable
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_015_Enable', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_015_Enable = ' + temp_str);
    if temp_str = '"true"' then
    begin
      ScheduleSetting_ListView.Items.Item[15].Checked := True;
    end
    else if temp_str = '"false"' then
    begin
      ScheduleSetting_ListView.Items.Item[15].Checked := False;
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_015_Enable 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_015_CurrentMode
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_015_CurrentMode', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_015_CurrentMode = ' + temp_str);
    if temp_str = '"SystemTest"' then
    begin
      ScheduleSetting_ListView.Items[15].SubItems.Strings[1] := '系統檢測';
      if FileExists('Configs\v299_SystemTestCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_SystemTestCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[15].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"M64"' then
    begin
      ScheduleSetting_ListView.Items[15].SubItems.Strings[1] := 'M64';
      if FileExists('Configs\v299_M64CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_M64CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[15].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64N"' then
    begin
      ScheduleSetting_ListView.Items[15].SubItems.Strings[1] := 'S64N';
      if FileExists('Configs\v299_M64CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64NCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[15].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64HS"' then
    begin
      ScheduleSetting_ListView.Items[15].SubItems.Strings[1] := 'S64HS';
      if FileExists('Configs\v299_S64HSCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64HSCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[15].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64VHS"' then
    begin
      ScheduleSetting_ListView.Items[15].SubItems.Strings[1] := 'S64VHS';
      if FileExists('Configs\v299_S64VHSCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64VHSCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[15].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom01"' then
    begin
      ScheduleSetting_ListView.Items[15].SubItems.Strings[1] := 'Custom01';
      if FileExists('Configs\v299_Custom01CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom01CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[15].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom02"' then
    begin
      ScheduleSetting_ListView.Items[15].SubItems.Strings[1] := 'Custom02';
      if FileExists('Configs\v299_Custom02CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom02CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[15].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom03"' then
    begin
      ScheduleSetting_ListView.Items[15].SubItems.Strings[1] := 'Custom03';
      if FileExists('Configs\v299_Custom03CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom03CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[15].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 +
        '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') + '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_015_CurrentMode 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_015_MaxVoltage_V
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_015_MaxVoltage_V', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_015_MaxVoltage_V = ' + temp_str);
    if temp_str = '' then
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_015_MaxVoltage_V 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end
    else
    begin
      if ScheduleSetting_ListView.Items[15].SubItems.Strings[1] = '系統檢測' then
      begin
        ScheduleSetting_ListView.Items[15].SubItems.Strings[2] := '24V';
      end
      else
      begin
        ScheduleSetting_ListView.Items[15].SubItems.Strings[2] := Copy(temp_str, 2, Length(temp_str) - 2) + 'V';
      end
    end;
    //--
    // INI: [Schedule] -> JOB_015_MaxCurrent_A
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_015_MaxCurrent_A', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_015_MaxCurrent_A = ' + temp_str);
    if temp_str = '' then
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_015_MaxCurrent_A 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end
    else
    begin
      if ScheduleSetting_ListView.Items[15].SubItems.Strings[1] = '系統檢測' then
      begin
        ScheduleSetting_ListView.Items[15].SubItems.Strings[3] := '0.1A';
      end
      else
      begin
        ScheduleSetting_ListView.Items[15].SubItems.Strings[3] := Copy(temp_str, 2, Length(temp_str) - 2) + 'A';
      end
    end;
    //--
    // INI: [Schedule] -> JOB_016
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_016', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_016 = ' + temp_str);
    if (Length(temp_str) = 7) and (temp_str[1] = '"') and (temp_str[4] = ':') and (temp_str[7] = '"') and
      TryStrToTime(Copy(temp_str, 2, 5), temp_TDateTime) then
    begin
      ScheduleSetting_ListView.Items.Item[16].SubItems.Strings[0] := FormatDateTime('HH:MM', temp_TDateTime);
      ScheduleSetting_ListView.Items.Item[16].SubItems.Strings[4] := '未知';
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_016 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_016_Enable
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_016_Enable', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_016_Enable = ' + temp_str);
    if temp_str = '"true"' then
    begin
      ScheduleSetting_ListView.Items.Item[16].Checked := True;
    end
    else if temp_str = '"false"' then
    begin
      ScheduleSetting_ListView.Items.Item[16].Checked := False;
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_016_Enable 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_016_CurrentMode
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_016_CurrentMode', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_016_CurrentMode = ' + temp_str);
    if temp_str = '"SystemTest"' then
    begin
      ScheduleSetting_ListView.Items[16].SubItems.Strings[1] := '系統檢測';
      if FileExists('Configs\v299_SystemTestCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_SystemTestCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[16].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"M64"' then
    begin
      ScheduleSetting_ListView.Items[16].SubItems.Strings[1] := 'M64';
      if FileExists('Configs\v299_M64CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_M64CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[16].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64N"' then
    begin
      ScheduleSetting_ListView.Items[16].SubItems.Strings[1] := 'S64N';
      if FileExists('Configs\v299_M64CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64NCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[16].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64HS"' then
    begin
      ScheduleSetting_ListView.Items[16].SubItems.Strings[1] := 'S64HS';
      if FileExists('Configs\v299_S64HSCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64HSCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[16].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64VHS"' then
    begin
      ScheduleSetting_ListView.Items[16].SubItems.Strings[1] := 'S64VHS';
      if FileExists('Configs\v299_S64VHSCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64VHSCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[16].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom01"' then
    begin
      ScheduleSetting_ListView.Items[16].SubItems.Strings[1] := 'Custom01';
      if FileExists('Configs\v299_Custom01CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom01CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[16].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom02"' then
    begin
      ScheduleSetting_ListView.Items[16].SubItems.Strings[1] := 'Custom02';
      if FileExists('Configs\v299_Custom02CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom02CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[16].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom03"' then
    begin
      ScheduleSetting_ListView.Items[16].SubItems.Strings[1] := 'Custom03';
      if FileExists('Configs\v299_Custom03CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom03CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[16].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 +
        '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') + '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_016_CurrentMode 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_016_MaxVoltage_V
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_016_MaxVoltage_V', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_016_MaxVoltage_V = ' + temp_str);
    if temp_str = '' then
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_016_MaxVoltage_V 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end
    else
    begin
      if ScheduleSetting_ListView.Items[16].SubItems.Strings[1] = '系統檢測' then
      begin
        ScheduleSetting_ListView.Items[16].SubItems.Strings[2] := '24V';
      end
      else
      begin
        ScheduleSetting_ListView.Items[16].SubItems.Strings[2] := Copy(temp_str, 2, Length(temp_str) - 2) + 'V';
      end
    end;
    //--
    // INI: [Schedule] -> JOB_016_MaxCurrent_A
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_016_MaxCurrent_A', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_016_MaxCurrent_A = ' + temp_str);
    if temp_str = '' then
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_016_MaxCurrent_A 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end
    else
    begin
      if ScheduleSetting_ListView.Items[16].SubItems.Strings[1] = '系統檢測' then
      begin
        ScheduleSetting_ListView.Items[16].SubItems.Strings[3] := '0.1A';
      end
      else
      begin
        ScheduleSetting_ListView.Items[16].SubItems.Strings[3] := Copy(temp_str, 2, Length(temp_str) - 2) + 'A';
      end
    end;
    //--
    // INI: [Schedule] -> JOB_017
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_017', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_017 = ' + temp_str);
    if (Length(temp_str) = 7) and (temp_str[1] = '"') and (temp_str[4] = ':') and (temp_str[7] = '"') and
      TryStrToTime(Copy(temp_str, 2, 5), temp_TDateTime) then
    begin
      ScheduleSetting_ListView.Items.Item[17].SubItems.Strings[0] := FormatDateTime('HH:MM', temp_TDateTime);
      ScheduleSetting_ListView.Items.Item[17].SubItems.Strings[4] := '未知';
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_017 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_017_Enable
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_017_Enable', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_017_Enable = ' + temp_str);
    if temp_str = '"true"' then
    begin
      ScheduleSetting_ListView.Items.Item[17].Checked := True;
    end
    else if temp_str = '"false"' then
    begin
      ScheduleSetting_ListView.Items.Item[17].Checked := False;
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_017_Enable 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_017_CurrentMode
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_017_CurrentMode', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_017_CurrentMode = ' + temp_str);
    if temp_str = '"SystemTest"' then
    begin
      ScheduleSetting_ListView.Items[17].SubItems.Strings[1] := '系統檢測';
      if FileExists('Configs\v299_SystemTestCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_SystemTestCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[17].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"M64"' then
    begin
      ScheduleSetting_ListView.Items[17].SubItems.Strings[1] := 'M64';
      if FileExists('Configs\v299_M64CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_M64CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[17].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64N"' then
    begin
      ScheduleSetting_ListView.Items[17].SubItems.Strings[1] := 'S64N';
      if FileExists('Configs\v299_M64CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64NCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[17].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64HS"' then
    begin
      ScheduleSetting_ListView.Items[17].SubItems.Strings[1] := 'S64HS';
      if FileExists('Configs\v299_S64HSCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64HSCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[17].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64VHS"' then
    begin
      ScheduleSetting_ListView.Items[17].SubItems.Strings[1] := 'S64VHS';
      if FileExists('Configs\v299_S64VHSCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64VHSCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[17].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom01"' then
    begin
      ScheduleSetting_ListView.Items[17].SubItems.Strings[1] := 'Custom01';
      if FileExists('Configs\v299_Custom01CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom01CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[17].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom02"' then
    begin
      ScheduleSetting_ListView.Items[17].SubItems.Strings[1] := 'Custom02';
      if FileExists('Configs\v299_Custom02CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom02CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[17].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom03"' then
    begin
      ScheduleSetting_ListView.Items[17].SubItems.Strings[1] := 'Custom03';
      if FileExists('Configs\v299_Custom03CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom03CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[17].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 +
        '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') + '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_017_CurrentMode 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_017_MaxVoltage_V
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_017_MaxVoltage_V', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_017_MaxVoltage_V = ' + temp_str);
    if temp_str = '' then
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_017_MaxVoltage_V 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end
    else
    begin
      if ScheduleSetting_ListView.Items[17].SubItems.Strings[1] = '系統檢測' then
      begin
        ScheduleSetting_ListView.Items[17].SubItems.Strings[2] := '24V';
      end
      else
      begin
        ScheduleSetting_ListView.Items[17].SubItems.Strings[2] := Copy(temp_str, 2, Length(temp_str) - 2) + 'V';
      end
    end;
    //--
    // INI: [Schedule] -> JOB_017_MaxCurrent_A
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_017_MaxCurrent_A', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_017_MaxCurrent_A = ' + temp_str);
    if temp_str = '' then
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_017_MaxCurrent_A 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end
    else
    begin
      if ScheduleSetting_ListView.Items[17].SubItems.Strings[1] = '系統檢測' then
      begin
        ScheduleSetting_ListView.Items[17].SubItems.Strings[3] := '0.1A';
      end
      else
      begin
        ScheduleSetting_ListView.Items[17].SubItems.Strings[3] := Copy(temp_str, 2, Length(temp_str) - 2) + 'A';
      end
    end;
    //--
    // INI: [Schedule] -> JOB_018
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_018', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_018 = ' + temp_str);
    if (Length(temp_str) = 7) and (temp_str[1] = '"') and (temp_str[4] = ':') and (temp_str[7] = '"') and
      TryStrToTime(Copy(temp_str, 2, 5), temp_TDateTime) then
    begin
      ScheduleSetting_ListView.Items.Item[18].SubItems.Strings[0] := FormatDateTime('HH:MM', temp_TDateTime);
      ScheduleSetting_ListView.Items.Item[18].SubItems.Strings[4] := '未知';
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_018 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_018_Enable
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_018_Enable', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_018_Enable = ' + temp_str);
    if temp_str = '"true"' then
    begin
      ScheduleSetting_ListView.Items.Item[18].Checked := True;
    end
    else if temp_str = '"false"' then
    begin
      ScheduleSetting_ListView.Items.Item[18].Checked := False;
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_018_Enable 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_018_CurrentMode
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_018_CurrentMode', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_018_CurrentMode = ' + temp_str);
    if temp_str = '"SystemTest"' then
    begin
      ScheduleSetting_ListView.Items[18].SubItems.Strings[1] := '系統檢測';
      if FileExists('Configs\v299_SystemTestCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_SystemTestCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[18].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"M64"' then
    begin
      ScheduleSetting_ListView.Items[18].SubItems.Strings[1] := 'M64';
      if FileExists('Configs\v299_M64CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_M64CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[18].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64N"' then
    begin
      ScheduleSetting_ListView.Items[18].SubItems.Strings[1] := 'S64N';
      if FileExists('Configs\v299_M64CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64NCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[18].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64HS"' then
    begin
      ScheduleSetting_ListView.Items[18].SubItems.Strings[1] := 'S64HS';
      if FileExists('Configs\v299_S64HSCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64HSCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[18].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64VHS"' then
    begin
      ScheduleSetting_ListView.Items[18].SubItems.Strings[1] := 'S64VHS';
      if FileExists('Configs\v299_S64VHSCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64VHSCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[18].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom01"' then
    begin
      ScheduleSetting_ListView.Items[18].SubItems.Strings[1] := 'Custom01';
      if FileExists('Configs\v299_Custom01CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom01CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[18].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom02"' then
    begin
      ScheduleSetting_ListView.Items[18].SubItems.Strings[1] := 'Custom02';
      if FileExists('Configs\v299_Custom02CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom02CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[18].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom03"' then
    begin
      ScheduleSetting_ListView.Items[18].SubItems.Strings[1] := 'Custom03';
      if FileExists('Configs\v299_Custom03CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom03CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[18].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 +
        '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') + '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_018_CurrentMode 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_018_MaxVoltage_V
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_018_MaxVoltage_V', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_018_MaxVoltage_V = ' + temp_str);
    if temp_str = '' then
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_018_MaxVoltage_V 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end
    else
    begin
      if ScheduleSetting_ListView.Items[18].SubItems.Strings[1] = '系統檢測' then
      begin
        ScheduleSetting_ListView.Items[18].SubItems.Strings[2] := '24V';
      end
      else
      begin
        ScheduleSetting_ListView.Items[18].SubItems.Strings[2] := Copy(temp_str, 2, Length(temp_str) - 2) + 'V';
      end
    end;
    //--
    // INI: [Schedule] -> JOB_018_MaxCurrent_A
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_018_MaxCurrent_A', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_018_MaxCurrent_A = ' + temp_str);
    if temp_str = '' then
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_018_MaxCurrent_A 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end
    else
    begin
      if ScheduleSetting_ListView.Items[18].SubItems.Strings[1] = '系統檢測' then
      begin
        ScheduleSetting_ListView.Items[18].SubItems.Strings[3] := '0.1A';
      end
      else
      begin
        ScheduleSetting_ListView.Items[18].SubItems.Strings[3] := Copy(temp_str, 2, Length(temp_str) - 2) + 'A';
      end
    end;
    //--
    // INI: [Schedule] -> JOB_019
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_019', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_019 = ' + temp_str);
    if (Length(temp_str) = 7) and (temp_str[1] = '"') and (temp_str[4] = ':') and (temp_str[7] = '"') and
      TryStrToTime(Copy(temp_str, 2, 5), temp_TDateTime) then
    begin
      ScheduleSetting_ListView.Items.Item[19].SubItems.Strings[0] := FormatDateTime('HH:MM', temp_TDateTime);
      ScheduleSetting_ListView.Items.Item[19].SubItems.Strings[4] := '未知';
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_019 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_019_Enable
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_019_Enable', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_019_Enable = ' + temp_str);
    if temp_str = '"true"' then
    begin
      ScheduleSetting_ListView.Items.Item[19].Checked := True;
    end
    else if temp_str = '"false"' then
    begin
      ScheduleSetting_ListView.Items.Item[19].Checked := False;
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_019_Enable 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_019_CurrentMode
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_019_CurrentMode', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_019_CurrentMode = ' + temp_str);
    if temp_str = '"SystemTest"' then
    begin
      ScheduleSetting_ListView.Items[19].SubItems.Strings[1] := '系統檢測';
      if FileExists('Configs\v299_SystemTestCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_SystemTestCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[19].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"M64"' then
    begin
      ScheduleSetting_ListView.Items[19].SubItems.Strings[1] := 'M64';
      if FileExists('Configs\v299_M64CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_M64CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[19].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64N"' then
    begin
      ScheduleSetting_ListView.Items[19].SubItems.Strings[1] := 'S64N';
      if FileExists('Configs\v299_M64CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64NCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[19].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64HS"' then
    begin
      ScheduleSetting_ListView.Items[19].SubItems.Strings[1] := 'S64HS';
      if FileExists('Configs\v299_S64HSCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64HSCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[19].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64VHS"' then
    begin
      ScheduleSetting_ListView.Items[19].SubItems.Strings[1] := 'S64VHS';
      if FileExists('Configs\v299_S64VHSCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64VHSCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[19].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom01"' then
    begin
      ScheduleSetting_ListView.Items[19].SubItems.Strings[1] := 'Custom01';
      if FileExists('Configs\v299_Custom01CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom01CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[19].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom02"' then
    begin
      ScheduleSetting_ListView.Items[19].SubItems.Strings[1] := 'Custom02';
      if FileExists('Configs\v299_Custom02CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom02CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[19].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom03"' then
    begin
      ScheduleSetting_ListView.Items[19].SubItems.Strings[1] := 'Custom03';
      if FileExists('Configs\v299_Custom03CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom03CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[19].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 +
        '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') + '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_019_CurrentMode 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_019_MaxVoltage_V
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_019_MaxVoltage_V', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_019_MaxVoltage_V = ' + temp_str);
    if temp_str = '' then
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_019_MaxVoltage_V 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end
    else
    begin
      if ScheduleSetting_ListView.Items[19].SubItems.Strings[1] = '系統檢測' then
      begin
        ScheduleSetting_ListView.Items[19].SubItems.Strings[2] := '24V';
      end
      else
      begin
        ScheduleSetting_ListView.Items[19].SubItems.Strings[2] := Copy(temp_str, 2, Length(temp_str) - 2) + 'V';
      end
    end;
    //--
    // INI: [Schedule] -> JOB_019_MaxCurrent_A
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_019_MaxCurrent_A', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_019_MaxCurrent_A = ' + temp_str);
    if temp_str = '' then
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_019_MaxCurrent_A 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end
    else
    begin
      if ScheduleSetting_ListView.Items[19].SubItems.Strings[1] = '系統檢測' then
      begin
        ScheduleSetting_ListView.Items[19].SubItems.Strings[3] := '0.1A';
      end
      else
      begin
        ScheduleSetting_ListView.Items[19].SubItems.Strings[3] := Copy(temp_str, 2, Length(temp_str) - 2) + 'A';
      end
    end;
    //--
    // INI: [Schedule] -> JOB_020
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_020', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_020 = ' + temp_str);
    if (Length(temp_str) = 7) and (temp_str[1] = '"') and (temp_str[4] = ':') and (temp_str[7] = '"') and
      TryStrToTime(Copy(temp_str, 2, 5), temp_TDateTime) then
    begin
      ScheduleSetting_ListView.Items.Item[20].SubItems.Strings[0] := FormatDateTime('HH:MM', temp_TDateTime);
      ScheduleSetting_ListView.Items.Item[20].SubItems.Strings[4] := '未知';
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_020 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_020_Enable
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_020_Enable', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_020_Enable = ' + temp_str);
    if temp_str = '"true"' then
    begin
      ScheduleSetting_ListView.Items.Item[20].Checked := True;
    end
    else if temp_str = '"false"' then
    begin
      ScheduleSetting_ListView.Items.Item[20].Checked := False;
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_020_Enable 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_020_CurrentMode
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_020_CurrentMode', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_020_CurrentMode = ' + temp_str);
    if temp_str = '"SystemTest"' then
    begin
      ScheduleSetting_ListView.Items[20].SubItems.Strings[1] := '系統檢測';
      if FileExists('Configs\v299_SystemTestCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_SystemTestCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[20].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"M64"' then
    begin
      ScheduleSetting_ListView.Items[20].SubItems.Strings[1] := 'M64';
      if FileExists('Configs\v299_M64CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_M64CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[20].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64N"' then
    begin
      ScheduleSetting_ListView.Items[20].SubItems.Strings[1] := 'S64N';
      if FileExists('Configs\v299_M64CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64NCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[20].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64HS"' then
    begin
      ScheduleSetting_ListView.Items[20].SubItems.Strings[1] := 'S64HS';
      if FileExists('Configs\v299_S64HSCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64HSCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[20].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64VHS"' then
    begin
      ScheduleSetting_ListView.Items[20].SubItems.Strings[1] := 'S64VHS';
      if FileExists('Configs\v299_S64VHSCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64VHSCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[20].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom01"' then
    begin
      ScheduleSetting_ListView.Items[20].SubItems.Strings[1] := 'Custom01';
      if FileExists('Configs\v299_Custom01CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom01CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[20].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom02"' then
    begin
      ScheduleSetting_ListView.Items[20].SubItems.Strings[1] := 'Custom02';
      if FileExists('Configs\v299_Custom02CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom02CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[20].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom03"' then
    begin
      ScheduleSetting_ListView.Items[20].SubItems.Strings[1] := 'Custom03';
      if FileExists('Configs\v299_Custom03CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom03CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[20].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 +
        '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') + '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_020_CurrentMode 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_020_MaxVoltage_V
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_020_MaxVoltage_V', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_020_MaxVoltage_V = ' + temp_str);
    if temp_str = '' then
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_020_MaxVoltage_V 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end
    else
    begin
      if ScheduleSetting_ListView.Items[20].SubItems.Strings[1] = '系統檢測' then
      begin
        ScheduleSetting_ListView.Items[20].SubItems.Strings[2] := '24V';
      end
      else
      begin
        ScheduleSetting_ListView.Items[20].SubItems.Strings[2] := Copy(temp_str, 2, Length(temp_str) - 2) + 'V';
      end
    end;
    //--
    // INI: [Schedule] -> JOB_020_MaxCurrent_A
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_020_MaxCurrent_A', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_020_MaxCurrent_A = ' + temp_str);
    if temp_str = '' then
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_020_MaxCurrent_A 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end
    else
    begin
      if ScheduleSetting_ListView.Items[20].SubItems.Strings[1] = '系統檢測' then
      begin
        ScheduleSetting_ListView.Items[20].SubItems.Strings[3] := '0.1A';
      end
      else
      begin
        ScheduleSetting_ListView.Items[20].SubItems.Strings[3] := Copy(temp_str, 2, Length(temp_str) - 2) + 'A';
      end
    end;
    //--
    // INI: [Schedule] -> JOB_021
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_021', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_021 = ' + temp_str);
    if (Length(temp_str) = 7) and (temp_str[1] = '"') and (temp_str[4] = ':') and (temp_str[7] = '"') and
      TryStrToTime(Copy(temp_str, 2, 5), temp_TDateTime) then
    begin
      ScheduleSetting_ListView.Items.Item[21].SubItems.Strings[0] := FormatDateTime('HH:MM', temp_TDateTime);
      ScheduleSetting_ListView.Items.Item[21].SubItems.Strings[4] := '未知';
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_021 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_021_Enable
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_021_Enable', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_021_Enable = ' + temp_str);
    if temp_str = '"true"' then
    begin
      ScheduleSetting_ListView.Items.Item[21].Checked := True;
    end
    else if temp_str = '"false"' then
    begin
      ScheduleSetting_ListView.Items.Item[21].Checked := False;
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_021_Enable 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_021_CurrentMode
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_021_CurrentMode', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_021_CurrentMode = ' + temp_str);
    if temp_str = '"SystemTest"' then
    begin
      ScheduleSetting_ListView.Items[21].SubItems.Strings[1] := '系統檢測';
      if FileExists('Configs\v299_SystemTestCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_SystemTestCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[21].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"M64"' then
    begin
      ScheduleSetting_ListView.Items[21].SubItems.Strings[1] := 'M64';
      if FileExists('Configs\v299_M64CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_M64CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[21].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64N"' then
    begin
      ScheduleSetting_ListView.Items[21].SubItems.Strings[1] := 'S64N';
      if FileExists('Configs\v299_M64CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64NCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[21].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64HS"' then
    begin
      ScheduleSetting_ListView.Items[21].SubItems.Strings[1] := 'S64HS';
      if FileExists('Configs\v299_S64HSCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64HSCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[21].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64VHS"' then
    begin
      ScheduleSetting_ListView.Items[21].SubItems.Strings[1] := 'S64VHS';
      if FileExists('Configs\v299_S64VHSCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64VHSCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[21].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom01"' then
    begin
      ScheduleSetting_ListView.Items[21].SubItems.Strings[1] := 'Custom01';
      if FileExists('Configs\v299_Custom01CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom01CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[21].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom02"' then
    begin
      ScheduleSetting_ListView.Items[21].SubItems.Strings[1] := 'Custom02';
      if FileExists('Configs\v299_Custom02CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom02CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[21].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom03"' then
    begin
      ScheduleSetting_ListView.Items[21].SubItems.Strings[1] := 'Custom03';
      if FileExists('Configs\v299_Custom03CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom03CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[21].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 +
        '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') + '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_021_CurrentMode 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_021_MaxVoltage_V
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_021_MaxVoltage_V', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_021_MaxVoltage_V = ' + temp_str);
    if temp_str = '' then
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_021_MaxVoltage_V 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end
    else
    begin
      if ScheduleSetting_ListView.Items[21].SubItems.Strings[1] = '系統檢測' then
      begin
        ScheduleSetting_ListView.Items[21].SubItems.Strings[2] := '24V';
      end
      else
      begin
        ScheduleSetting_ListView.Items[21].SubItems.Strings[2] := Copy(temp_str, 2, Length(temp_str) - 2) + 'V';
      end
    end;
    //--
    // INI: [Schedule] -> JOB_021_MaxCurrent_A
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_021_MaxCurrent_A', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_021_MaxCurrent_A = ' + temp_str);
    if temp_str = '' then
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_021_MaxCurrent_A 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end
    else
    begin
      if ScheduleSetting_ListView.Items[21].SubItems.Strings[1] = '系統檢測' then
      begin
        ScheduleSetting_ListView.Items[21].SubItems.Strings[3] := '0.1A';
      end
      else
      begin
        ScheduleSetting_ListView.Items[21].SubItems.Strings[3] := Copy(temp_str, 2, Length(temp_str) - 2) + 'A';
      end
    end;
    //--
    // INI: [Schedule] -> JOB_022
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_022', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_022 = ' + temp_str);
    if (Length(temp_str) = 7) and (temp_str[1] = '"') and (temp_str[4] = ':') and (temp_str[7] = '"') and
      TryStrToTime(Copy(temp_str, 2, 5), temp_TDateTime) then
    begin
      ScheduleSetting_ListView.Items.Item[22].SubItems.Strings[0] := FormatDateTime('HH:MM', temp_TDateTime);
      ScheduleSetting_ListView.Items.Item[22].SubItems.Strings[4] := '未知';
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_022 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_022_Enable
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_022_Enable', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_022_Enable = ' + temp_str);
    if temp_str = '"true"' then
    begin
      ScheduleSetting_ListView.Items.Item[22].Checked := True;
    end
    else if temp_str = '"false"' then
    begin
      ScheduleSetting_ListView.Items.Item[22].Checked := False;
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_022_Enable 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_022_CurrentMode
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_022_CurrentMode', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_022_CurrentMode = ' + temp_str);
    if temp_str = '"SystemTest"' then
    begin
      ScheduleSetting_ListView.Items[22].SubItems.Strings[1] := '系統檢測';
      if FileExists('Configs\v299_SystemTestCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_SystemTestCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[22].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"M64"' then
    begin
      ScheduleSetting_ListView.Items[22].SubItems.Strings[1] := 'M64';
      if FileExists('Configs\v299_M64CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_M64CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[22].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64N"' then
    begin
      ScheduleSetting_ListView.Items[22].SubItems.Strings[1] := 'S64N';
      if FileExists('Configs\v299_M64CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64NCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[22].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64HS"' then
    begin
      ScheduleSetting_ListView.Items[22].SubItems.Strings[1] := 'S64HS';
      if FileExists('Configs\v299_S64HSCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64HSCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[22].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64VHS"' then
    begin
      ScheduleSetting_ListView.Items[22].SubItems.Strings[1] := 'S64VHS';
      if FileExists('Configs\v299_S64VHSCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64VHSCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[22].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom01"' then
    begin
      ScheduleSetting_ListView.Items[22].SubItems.Strings[1] := 'Custom01';
      if FileExists('Configs\v299_Custom01CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom01CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[22].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom02"' then
    begin
      ScheduleSetting_ListView.Items[22].SubItems.Strings[1] := 'Custom02';
      if FileExists('Configs\v299_Custom02CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom02CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[22].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom03"' then
    begin
      ScheduleSetting_ListView.Items[22].SubItems.Strings[1] := 'Custom03';
      if FileExists('Configs\v299_Custom03CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom03CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[22].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 +
        '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') + '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_022_CurrentMode 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_022_MaxVoltage_V
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_022_MaxVoltage_V', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_022_MaxVoltage_V = ' + temp_str);
    if temp_str = '' then
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_022_MaxVoltage_V 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end
    else
    begin
      if ScheduleSetting_ListView.Items[22].SubItems.Strings[1] = '系統檢測' then
      begin
        ScheduleSetting_ListView.Items[22].SubItems.Strings[2] := '24V';
      end
      else
      begin
        ScheduleSetting_ListView.Items[22].SubItems.Strings[2] := Copy(temp_str, 2, Length(temp_str) - 2) + 'V';
      end
    end;
    //--
    // INI: [Schedule] -> JOB_022_MaxCurrent_A
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_022_MaxCurrent_A', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_022_MaxCurrent_A = ' + temp_str);
    if temp_str = '' then
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_022_MaxCurrent_A 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end
    else
    begin
      if ScheduleSetting_ListView.Items[22].SubItems.Strings[1] = '系統檢測' then
      begin
        ScheduleSetting_ListView.Items[22].SubItems.Strings[3] := '0.1A';
      end
      else
      begin
        ScheduleSetting_ListView.Items[22].SubItems.Strings[3] := Copy(temp_str, 2, Length(temp_str) - 2) + 'A';
      end
    end;
    //--
    // INI: [Schedule] -> JOB_023
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_023', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_023 = ' + temp_str);
    if (Length(temp_str) = 7) and (temp_str[1] = '"') and (temp_str[4] = ':') and (temp_str[7] = '"') and
      TryStrToTime(Copy(temp_str, 2, 5), temp_TDateTime) then
    begin
      ScheduleSetting_ListView.Items.Item[23].SubItems.Strings[0] := FormatDateTime('HH:MM', temp_TDateTime);
      ScheduleSetting_ListView.Items.Item[23].SubItems.Strings[4] := '未知';
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_023 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_023_Enable
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_023_Enable', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_023_Enable = ' + temp_str);
    if temp_str = '"true"' then
    begin
      ScheduleSetting_ListView.Items.Item[23].Checked := True;
    end
    else if temp_str = '"false"' then
    begin
      ScheduleSetting_ListView.Items.Item[23].Checked := False;
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_023_Enable 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_023_CurrentMode
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_023_CurrentMode', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_023_CurrentMode = ' + temp_str);
    if temp_str = '"SystemTest"' then
    begin
      ScheduleSetting_ListView.Items[23].SubItems.Strings[1] := '系統檢測';
      if FileExists('Configs\v299_SystemTestCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_SystemTestCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[23].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"M64"' then
    begin
      ScheduleSetting_ListView.Items[23].SubItems.Strings[1] := 'M64';
      if FileExists('Configs\v299_M64CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_M64CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[23].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64N"' then
    begin
      ScheduleSetting_ListView.Items[23].SubItems.Strings[1] := 'S64N';
      if FileExists('Configs\v299_M64CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64NCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[23].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64HS"' then
    begin
      ScheduleSetting_ListView.Items[23].SubItems.Strings[1] := 'S64HS';
      if FileExists('Configs\v299_S64HSCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64HSCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[23].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64VHS"' then
    begin
      ScheduleSetting_ListView.Items[23].SubItems.Strings[1] := 'S64VHS';
      if FileExists('Configs\v299_S64VHSCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64VHSCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[23].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom01"' then
    begin
      ScheduleSetting_ListView.Items[23].SubItems.Strings[1] := 'Custom01';
      if FileExists('Configs\v299_Custom01CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom01CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[23].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom02"' then
    begin
      ScheduleSetting_ListView.Items[23].SubItems.Strings[1] := 'Custom02';
      if FileExists('Configs\v299_Custom02CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom02CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[23].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom03"' then
    begin
      ScheduleSetting_ListView.Items[23].SubItems.Strings[1] := 'Custom03';
      if FileExists('Configs\v299_Custom03CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom03CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[23].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 +
        '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') + '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_023_CurrentMode 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_023_MaxVoltage_V
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_023_MaxVoltage_V', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_023_MaxVoltage_V = ' + temp_str);
    if temp_str = '' then
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_023_MaxVoltage_V 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end
    else
    begin
      if ScheduleSetting_ListView.Items[23].SubItems.Strings[1] = '系統檢測' then
      begin
        ScheduleSetting_ListView.Items[23].SubItems.Strings[2] := '24V';
      end
      else
      begin
        ScheduleSetting_ListView.Items[23].SubItems.Strings[2] := Copy(temp_str, 2, Length(temp_str) - 2) + 'V';
      end
    end;
    //--
    // INI: [Schedule] -> JOB_023_MaxCurrent_A
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_023_MaxCurrent_A', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_023_MaxCurrent_A = ' + temp_str);
    if temp_str = '' then
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_023_MaxCurrent_A 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end
    else
    begin
      if ScheduleSetting_ListView.Items[23].SubItems.Strings[1] = '系統檢測' then
      begin
        ScheduleSetting_ListView.Items[23].SubItems.Strings[3] := '0.1A';
      end
      else
      begin
        ScheduleSetting_ListView.Items[23].SubItems.Strings[3] := Copy(temp_str, 2, Length(temp_str) - 2) + 'A';
      end
    end;
    //--
    // INI: [Schedule] -> JOB_024
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_024', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_024 = ' + temp_str);
    if (Length(temp_str) = 7) and (temp_str[1] = '"') and (temp_str[4] = ':') and (temp_str[7] = '"') and
      TryStrToTime(Copy(temp_str, 2, 5), temp_TDateTime) then
    begin
      ScheduleSetting_ListView.Items.Item[24].SubItems.Strings[0] := FormatDateTime('HH:MM', temp_TDateTime);
      ScheduleSetting_ListView.Items.Item[24].SubItems.Strings[4] := '未知';
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_024 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_024_Enable
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_024_Enable', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_024_Enable = ' + temp_str);
    if temp_str = '"true"' then
    begin
      ScheduleSetting_ListView.Items.Item[24].Checked := True;
    end
    else if temp_str = '"false"' then
    begin
      ScheduleSetting_ListView.Items.Item[24].Checked := False;
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_024_Enable 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_024_CurrentMode
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_024_CurrentMode', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_024_CurrentMode = ' + temp_str);
    if temp_str = '"SystemTest"' then
    begin
      ScheduleSetting_ListView.Items[24].SubItems.Strings[1] := '系統檢測';
      if FileExists('Configs\v299_SystemTestCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_SystemTestCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[24].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"M64"' then
    begin
      ScheduleSetting_ListView.Items[24].SubItems.Strings[1] := 'M64';
      if FileExists('Configs\v299_M64CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_M64CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[24].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64N"' then
    begin
      ScheduleSetting_ListView.Items[24].SubItems.Strings[1] := 'S64N';
      if FileExists('Configs\v299_M64CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64NCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[24].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64HS"' then
    begin
      ScheduleSetting_ListView.Items[24].SubItems.Strings[1] := 'S64HS';
      if FileExists('Configs\v299_S64HSCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64HSCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[24].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"S64VHS"' then
    begin
      ScheduleSetting_ListView.Items[24].SubItems.Strings[1] := 'S64VHS';
      if FileExists('Configs\v299_S64VHSCurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_S64VHSCurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[24].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom01"' then
    begin
      ScheduleSetting_ListView.Items[24].SubItems.Strings[1] := 'Custom01';
      if FileExists('Configs\v299_Custom01CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom01CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[24].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom02"' then
    begin
      ScheduleSetting_ListView.Items[24].SubItems.Strings[1] := 'Custom02';
      if FileExists('Configs\v299_Custom02CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom02CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[24].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else if temp_str = '"Custom03"' then
    begin
      ScheduleSetting_ListView.Items[24].SubItems.Strings[1] := 'Custom03';
      if FileExists('Configs\v299_Custom03CurrentMode\Current Mode.csv') then
      begin
        TXT_Memo.WordWrap := False;
        TXT_Memo.Lines.LoadFromFile('Configs\v299_Custom03CurrentMode\Current Mode.csv');
        // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
        //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
        //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
        ScheduleSetting_ListView.Items[24].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      end;
    end
    else
    begin
      ShowMessage('嚴重錯誤:' + #13#10 +
        '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') + '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_024_CurrentMode 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end;
    //--
    // INI: [Schedule] -> JOB_024_MaxVoltage_V
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_024_MaxVoltage_V', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_024_MaxVoltage_V = ' + temp_str);
    if temp_str = '' then
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_024_MaxVoltage_V 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end
    else
    begin
      if ScheduleSetting_ListView.Items[24].SubItems.Strings[1] = '系統檢測' then
      begin
        ScheduleSetting_ListView.Items[24].SubItems.Strings[2] := '24V';
      end
      else
      begin
        ScheduleSetting_ListView.Items[24].SubItems.Strings[2] := Copy(temp_str, 2, Length(temp_str) - 2) + 'V';
      end
    end;
    //--
    // INI: [Schedule] -> JOB_024_MaxCurrent_A
    temp_str := my_TMemIniFile.ReadString('Schedule', 'JOB_024_MaxCurrent_A', '');
    //RunningLog_Memo.Lines.Add('INI: [Schedule] -> JOB_024_MaxCurrent_A = ' + temp_str);
    if temp_str = '' then
    begin
      ShowMessage('嚴重錯誤:' + #13#10 + '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
        '」內容異常!程式將立即終止!' + #13#10 +
        '請檢查[Schedule] -> JOB_024_MaxCurrent_A 設定。' + #13#10 +
        '或刪除該檔案後重新運行程式，取得預設INI檔案。');
      Application.Terminate;
    end
    else
    begin
      if ScheduleSetting_ListView.Items[24].SubItems.Strings[1] = '系統檢測' then
      begin
        ScheduleSetting_ListView.Items[24].SubItems.Strings[3] := '0.1A';
      end
      else
      begin
        ScheduleSetting_ListView.Items[24].SubItems.Strings[3] := Copy(temp_str, 2, Length(temp_str) - 2) + 'A';
      end
    end;
    //--
    // JOB_999(立刻單次系統檢測專用)
    ScheduleSetting_ListView.Items[25].SubItems.Strings[1] := '系統檢測';
    ScheduleSetting_ListView.Items.Item[25].SubItems.Strings[4] := '未知';
    if FileExists('Configs\v299_SystemTestCurrentMode\Current Mode.csv') then
    begin
      TXT_Memo.WordWrap := False;
      TXT_Memo.Lines.LoadFromFile('Configs\v299_SystemTestCurrentMode\Current Mode.csv');
      // 取出第一行檢查放電描述的數量，有N個字代表每秒設定並搭配有N-1個逗號。可推算出放電時間。
      //RunningLog_Memo.Lines.Add('放電時間(秒) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2));
      //RunningLog_Memo.Lines.Add('放電時間(分) = ' + FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60));
      ScheduleSetting_ListView.Items[25].SubItems.Strings[7] := FloatToStr((Length(TXT_Memo.Lines[0])+1)/2/60);
      ScheduleSetting_ListView.Items[25].SubItems.Strings[2] := '24V';
    end;
    ScheduleSetting_ListView.Items[25].SubItems.Strings[2] := '24V';
    ScheduleSetting_ListView.Items[25].SubItems.Strings[3] := '0.1A';
    //--
  end
  else
  begin
    //--
    // 目標INI檔案不存在，需要寫入新的INI。
    need_write_ini_file := True;
    //--
  end;
  //--------------------------------------------------------------------------
  //--------------------------------------------------------------------------
  // 寫入參數檔案
  if need_write_ini_file then
  begin
    RunningLog_Memo.Lines.Add('「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') + '」不存在!');
    my_TMemIniFile.WriteString('Version', 'Version', '"' + version_str + '"');
    my_TMemIniFile.WriteString('Version', 'Author', '"HsiupoYeh"');
    my_TMemIniFile.WriteString('ERT_ver2_9_9_Client', 'ERT_Profile_Name', '"XP1"');
    my_TMemIniFile.WriteString('ERT_ver2_9_9_Client', 'ERT_Project_Name', '"[20240409A]Taiwan-Taipei-XiaoYouKengScenicPlatform(XYKP)"');
    my_TMemIniFile.WriteString('ERT_ver2_9_9_Client', 'DMM_SN', '"MY60077634"');
    my_TMemIniFile.WriteString('ERT_ver2_9_9_Client', 'DMM_ComPort', '"USB0::0x2A8D::0x0101::MY60077634::INSTR"');
    my_TMemIniFile.WriteString('ERT_ver2_9_9_Client', 'SwitchArray_SN', '"S000"');
    my_TMemIniFile.WriteString('ERT_ver2_9_9_Client', 'SwitchArray_ComPort', '"COM3"');
    my_TMemIniFile.WriteString('ERT_ver2_9_9_Client', 'PSU_SN', '"508I23118"');
    my_TMemIniFile.WriteString('ERT_ver2_9_9_Client', 'PSU_ComPort', '"COM4"');
    my_TMemIniFile.WriteString('Schedule', 'Every_Day_Enable', '"true"');
    my_TMemIniFile.WriteString('Schedule', 'PSU_RANGE_HIGH_LOW', '"HIGH"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_000', '"00:00"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_000_CurrentMode', '"S64VHS"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_000_MaxVoltage_V', '"600"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_000_MaxCurrent_A', '"0.35"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_001', '"00:01"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_001_Enable', '"true"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_001_CurrentMode', '"S64HS"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_001_MaxVoltage_V', '"600"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_001_MaxCurrent_A', '"0.35"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_002', '"01:01"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_002_Enable', '"true"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_002_CurrentMode', '"S64N"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_002_MaxVoltage_V', '"600"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_002_MaxCurrent_A', '"0.35"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_003', '"02:01"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_003_Enable', '"true"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_003_CurrentMode', '"M64"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_003_MaxVoltage_V', '"600"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_003_MaxCurrent_A', '"0.35"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_004', '"03:01"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_004_Enable', '"true"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_004_CurrentMode', '"SystemTest"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_004_MaxVoltage_V', '"600"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_004_MaxCurrent_A', '"0.35"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_005', '"04:01"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_005_Enable', '"true"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_005_CurrentMode', '"Custom01"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_005_MaxVoltage_V', '"600"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_005_MaxCurrent_A', '"0.35"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_006', '"05:01"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_006_Enable', '"true"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_006_CurrentMode', '"Custom02"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_006_MaxVoltage_V', '"600"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_006_MaxCurrent_A', '"0.35"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_007', '"06:01"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_007_Enable', '"true"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_007_CurrentMode', '"Custom03"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_007_MaxVoltage_V', '"600"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_007_MaxCurrent_A', '"0.35"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_008', '"07:01"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_008_Enable', '"false"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_008_CurrentMode', '"S64VHS"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_008_MaxVoltage_V', '"600"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_008_MaxCurrent_A', '"0.35"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_009', '"08:01"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_009_Enable', '"false"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_009_CurrentMode', '"S64VHS"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_009_MaxVoltage_V', '"600"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_009_MaxCurrent_A', '"0.35"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_010', '"09:01"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_010_Enable', '"false"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_010_CurrentMode', '"S64VHS"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_010_MaxVoltage_V', '"600"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_010_MaxCurrent_A', '"0.35"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_011', '"10:01"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_011_Enable', '"false"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_011_CurrentMode', '"S64VHS"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_011_MaxVoltage_V', '"600"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_011_MaxCurrent_A', '"0.35"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_012', '"11:01"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_012_Enable', '"false"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_012_CurrentMode', '"S64VHS"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_012_MaxVoltage_V', '"600"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_012_MaxCurrent_A', '"0.35"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_013', '"12:01"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_013_Enable', '"false"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_013_CurrentMode', '"S64VHS"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_013_MaxVoltage_V', '"600"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_013_MaxCurrent_A', '"0.35"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_014', '"13:01"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_014_Enable', '"false"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_014_CurrentMode', '"S64VHS"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_014_MaxVoltage_V', '"600"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_014_MaxCurrent_A', '"0.35"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_015', '"14:01"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_015_Enable', '"false"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_015_CurrentMode', '"S64VHS"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_015_MaxVoltage_V', '"600"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_015_MaxCurrent_A', '"0.35"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_016', '"15:01"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_016_Enable', '"false"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_016_CurrentMode', '"S64VHS"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_016_MaxVoltage_V', '"600"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_016_MaxCurrent_A', '"0.35"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_017', '"16:01"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_017_Enable', '"false"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_017_CurrentMode', '"S64VHS"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_017_MaxVoltage_V', '"600"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_017_MaxCurrent_A', '"0.35"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_018', '"17:01"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_018_Enable', '"false"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_018_CurrentMode', '"S64VHS"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_018_MaxVoltage_V', '"600"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_018_MaxCurrent_A', '"0.35"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_019', '"18:01"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_019_Enable', '"false"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_019_CurrentMode', '"S64VHS"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_019_MaxVoltage_V', '"600"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_019_MaxCurrent_A', '"0.35"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_020', '"19:01"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_020_Enable', '"false"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_020_CurrentMode', '"S64VHS"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_020_MaxVoltage_V', '"600"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_020_MaxCurrent_A', '"0.35"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_021', '"20:01"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_021_Enable', '"false"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_021_CurrentMode', '"S64VHS"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_021_MaxVoltage_V', '"600"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_021_MaxCurrent_A', '"0.35"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_022', '"21:01"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_022_Enable', '"false"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_022_CurrentMode', '"S64VHS"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_022_MaxVoltage_V', '"600"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_022_MaxCurrent_A', '"0.35"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_023', '"22:01"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_023_Enable', '"false"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_023_CurrentMode', '"S64VHS"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_023_MaxVoltage_V', '"600"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_023_MaxCurrent_A', '"0.35"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_024', '"23:01"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_024_Enable', '"false"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_024_CurrentMode', '"S64VHS"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_024_MaxVoltage_V', '"600"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_024_MaxCurrent_A', '"0.35"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_999', '"23:59"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_999_CurrentMode', '"SystemTest"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_999_MaxVoltage_V', '"600"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_999_MaxCurrent_A', '"0.35"');
    my_TMemIniFile.UpdateFile();
    temp_str := '提示:' + #13#10 + '檔案「' +
      ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
      '」不存在!程式將立即終止!' + #13#10 +
      '本程式已自動重新產生預設INI檔案。' + #13#10 +
      '請按下確定後手動重新運行本程式。';
    Application.MessageBox(PChar(temp_str), '提示', 64);
    Application.Terminate;
  end;
  //--------------------------------------------------------------------------
  my_TMemIniFile.Free;
  //--------------------------------------------------------------------------
  //--------------------------------------------------------------------------
  // 設定按鈕啟用禁用
  NowRun_Button.Enabled := False;
  NowSystemTest_Button.Enabled := False;
  ScheduleRun_Button.Enabled := False;
  //--------------------------------------------------------------------------
  //--------------------------------------------------------------------------
  // 檢查PSU電壓設定
  if PSU_RangeHIGH_CheckBox.Checked = True then
  begin
    for temp_i := 0 to ScheduleSetting_ListView.Items.Count - 1 do
    begin
      // 訪問每個SubItems (限電壓)
      //RunningLog_Memo.Lines.Add( ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[2] );
      // 處理超過範圍的部分(大於600V)
      if StrToFloat(Copy(ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[2], 1, Length(ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[2]) - 1)) > 600.0 then
      begin
        CheckExternalDevices_Button.Enabled := False;
      end;
      // 訪問每個SubItems (限電流)
      //RunningLog_Memo.Lines.Add( ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[3] );
      // 處理超過範圍的部分(大於0.35A)
      if StrToFloat(Copy(ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[3], 1, Length(ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[3]) - 1)) > 0.35 then
      begin
        CheckExternalDevices_Button.Enabled := False;
      end;
    end;
  end;
  //--
  if PSU_RangeLOW_CheckBox.Checked = True then
  begin
    for temp_i := 0 to ScheduleSetting_ListView.Items.Count - 1 do
    begin
      // 訪問每個SubItems (限電壓)
      //RunningLog_Memo.Lines.Add( ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[2] );
      // 處理超過範圍的部分(大於400V)
      if StrToFloat(Copy(ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[2], 1, Length(ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[2]) - 1)) > 400.0 then
      begin
        CheckExternalDevices_Button.Enabled := False;
      end;
      // 訪問每個SubItems (限電流)
      //RunningLog_Memo.Lines.Add( ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[3] );
      // 處理超過範圍的部分(大於0.5A)
      if StrToFloat(Copy(ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[3], 1, Length(ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[3]) - 1)) > 0.5 then
      begin
        CheckExternalDevices_Button.Enabled := False;
      end;
    end;
  end;
  //--------------------------------------------------------------------------
  //--------------------------------------------------------------------------
  if CheckExternalDevices_Button.Enabled = False then
  begin
    Application.MessageBox(
      PChar('檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') +
            '」內容異常!' + #13#10 + #13#10 +
            '請檢查PSU電壓電流設定(HIGH/LOW)與以及排程內設定的限電壓限電流。' + #13#10 +
            '或刪除該檔案後重新運行程式，取得預設INI檔案。'),
      PChar('錯誤:'), // 訊息框標題
      16
    );
    RunningLog_Memo.Lines.Add( '錯誤:' );
    RunningLog_Memo.Lines.Add( '檔案「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') + '」內容異常!' );
    RunningLog_Memo.Lines.Add( '請檢查PSU電壓電流設定(HIGH/LOW)與以及排程內設定的限電壓限電流。' );
    RunningLog_Memo.Lines.Add( '或刪除該檔案後重新運行程式，取得預設INI檔案。' );
  end;
  //--------------------------------------------------------------------------
end;   
```

### 開啟資料夾
+ 3.1 去implementation區塊追加宣告uses
```pascal
implementation
//--------------------------------------------------------------------------
//uses add by HsiupoYeh
uses
  IniFiles, Windows, ShellApi;
//--------------------------------------------------------------------------
//--------------------------------------------------------------------------
//宣告全域變數 add by HsiupoYeh
var
  version_str: AnsiString;
  Current_Folder_Path: AnsiString;
//--------------------------------------------------------------------------

{$R *.lfm}
```
+ 3.2 選「ERT_ver2_9_9_Client_OpenSavePath_Button」，去編輯「OnClick」
```pascal
procedure TForm1.ERT_ver2_9_9_Client_OpenSavePath_ButtonClick(Sender: TObject);
var
  target_Folder_path: string;
begin
  // 取得程式執行目錄下的子資料夾路徑
  target_Folder_path := ExtractFilePath(Application.ExeName) + 'R2MS_Lite_WorkSpace\ERT_ver2_9_9_Client_Data\';
  // 建立指定的資料夾，如果它不存在的話
  ForceDirectories(target_Folder_path);
  // 記得在 uses 區塊中加入 Windows, ShellApi
  // 使用 ShellExecute 打開該資料夾
  ShellExecute(0, 'open', PChar(target_Folder_path), nil, nil, SW_SHOWNORMAL);
end;  
```

### 禁用勾選
+ 4.1 選「ScheduleSetting_ListView」，去編輯「OnItemChecked」
```pascal
procedure TForm1.ScheduleSetting_ListViewItemChecked(Sender: TObject;
  Item: TListItem);
begin
  //--------------------------------------------------------------------------
  // 強制維持項目的原始勾選狀態。即使使用者點擊，也無法改變其勾選或取消勾選的狀態。
  if Item.Checked = False then
  begin
    Item.Checked := True;
  end
  else
  begin
    Item.Checked := False;
  end;
  //--------------------------------------------------------------------------
end;      
//-------------------------------------------------------------------------- 
```
+ 4.2 選「PSU_RangeHIGH_CheckBox」，去編輯「OnKeyDown」
```pascal
procedure TForm1.PSU_RangeHIGH_CheckBoxKeyDown(Sender: TObject; var Key: Word;
  Shift: TShiftState);
begin
  // 阻止使用者改變 CheckBox 的勾選狀態
  PSU_RangeHIGH_CheckBox.Checked := not PSU_RangeHIGH_CheckBox.Checked;
end;
```
+ 4.3 選「PSU_RangeHIGH_CheckBox」，去編輯「MouseDown」
```pascal
procedure TForm1.PSU_RangeHIGH_CheckBoxMouseDown(Sender: TObject;
  Button: TMouseButton; Shift: TShiftState; X, Y: Integer);
begin
  // 阻止使用者改變 CheckBox 的勾選狀態
  PSU_RangeHIGH_CheckBox.Checked := not PSU_RangeHIGH_CheckBox.Checked;
end; 
```
+ 4.4 選「PSU_RangeLOW_CheckBox」，去編輯「OnKeyDown」
```pascal
procedure TForm1.PSU_RangeLOW_CheckBoxKeyDown(Sender: TObject; var Key: Word;
  Shift: TShiftState);
begin
  // 阻止使用者改變 CheckBox 的勾選狀態
  PSU_RangeLOW_CheckBox.Checked := not PSU_RangeLOW_CheckBox.Checked;
end; 
```
+ 4.5 選「PSU_RangeLOW_CheckBox」，去編輯「MouseDown」
```pascal
procedure TForm1.PSU_RangeLOW_CheckBoxMouseDown(Sender: TObject;
  Button: TMouseButton; Shift: TShiftState; X, Y: Integer);
begin
  // 阻止使用者改變 CheckBox 的勾選狀態
  PSU_RangeLOW_CheckBox.Checked := not PSU_RangeLOW_CheckBox.Checked;
end;  
```

### 偵測外部裝置
+ 5.1 去implementation區塊追加宣告uses
```pascal
implementation
//--------------------------------------------------------------------------
//uses add by HsiupoYeh
uses
  IniFiles, Windows, ShellApi, ComObj, ActiveX, Variants;
//--------------------------------------------------------------------------
//--------------------------------------------------------------------------
//宣告全域變數 add by HsiupoYeh
var
  version_str: AnsiString;
  Current_Folder_Path: AnsiString;
//--------------------------------------------------------------------------

{$R *.lfm} 
```
+ 5.2 拖拉一個「LazSerial>TLazSerial」到「Form1」中。預設名稱會是「LazSerial1」。
+ 5.3 選「CheckExternalDevices_Button」，去編輯「OnClick」
```pascal
procedure TForm1.CheckExternalDevices_ButtonClick(Sender: TObject);
var
  MyWmiLocator: OleVariant;          // 用於創建與 WMI 服務連接的定位器物件；注意：使用 OleVariant 需要 uses Variants 單元
  MyWmiServices: OleVariant;         // 代表與特定 WMI 命名空間（例如 ROOT\CIMV2）的連接；注意：使用 OleVariant 需要 uses Variants 單元
  MyWmiObjectSet: OleVariant;        // 儲存 WQL 查詢返回的 WMI 物件集合（可能包含多個物件）；注意：使用 OleVariant 需要 uses Variants 單元
  MyWmiObject: OleVariant;           // 在遍歷 MyWmiObjectSet 時，指向集合中的單個 WMI 物件；注意：使用 OleVariant 需要 uses Variants 單元
  MyWmiObjectEnumerator: IEnumVariant; // 用於枚舉（遍歷）WMI 物件集合的 COM 介面，讓我們可以逐個訪問物件
  MyWmiObjectItem: OleVariant;       // 在枚舉過程中，臨時儲存當前遍歷到的單個 WMI 物件；注意：使用 OleVariant 需要 uses Variants 單元
  MyWmiObjectItemFetched: LongWord;  // 記錄在單次呼叫枚舉器的 Next 方法時，實際成功獲取的 WMI 物件的數量
  Response: String;
begin
  CheckExternalDevices_Button.Enabled := False;
  //--------------------------------------------------------------------------
  // 用WMI查詢外部裝置的資訊(包含VISA路徑或COM值)
  RunningLog_Memo.Lines.Add('正在使用 WMI 查詢 Win32_PnPEntity...');
  RunningLog_Memo.Lines.Add('');
  try
    RunningLog_Memo.Lines.Add('步驟 1: 建立 MyWmiLocator 物件...');
    MyWmiLocator := CreateOleObject('WbemScripting.SWbemLocator');
    RunningLog_Memo.Lines.Add('步驟 1 完成。');

    RunningLog_Memo.Lines.Add('步驟 2: 連接到 ROOT\CIMV2 命名空間...');
    MyWmiServices := MyWmiLocator.ConnectServer(OleVariant(StringToOleStr('.')), OleVariant(StringToOleStr('ROOT\CIMV2')), OleVariant(StringToOleStr('')), OleVariant(StringToOleStr('')));
    RunningLog_Memo.Lines.Add('步驟 2 完成。');

    RunningLog_Memo.Lines.Add('步驟 3: 執行 WQL 查詢...');
    MyWmiObjectSet := MyWmiServices.ExecQuery(OleVariant(StringToOleStr('SELECT Caption, Description, DeviceID, PNPClass FROM Win32_PnPEntity')));
    RunningLog_Memo.Lines.Add('步驟 3 完成。找到 ' + IntToStr(MyWmiObjectSet.Count) + ' 個 PnP 裝置。');

    RunningLog_Memo.Lines.Add('步驟 4: 遍歷查詢結果並顯示資訊...');
    MyWmiObjectEnumerator := IUnknown(MyWmiObjectSet._NewEnum) as IEnumVariant;
    MyWmiObjectItem := Unassigned;
    while MyWmiObjectEnumerator.Next(1, MyWmiObjectItem, MyWmiObjectItemFetched) = 0 do
    begin
      MyWmiObject := MyWmiObjectItem;
      //RunningLog_Memo.Lines.Add('  裝置名稱: ' + VarToStrDef(MyWmiObject.Properties_.Item('Caption').Value, ''));
      //RunningLog_Memo.Lines.Add('  描述: ' + VarToStrDef(MyWmiObject.Properties_.Item('Description').Value, ''));
      //RunningLog_Memo.Lines.Add('  裝置 ID: ' + VarToStrDef(MyWmiObject.Properties_.Item('DeviceID').Value, ''));
      //RunningLog_Memo.Lines.Add('  PNPClass: ' + VarToStrDef(MyWmiObject.Properties_.Item('PNPClass').Value, ''));
      //RunningLog_Memo.Lines.Add('');
      // 找到目標裝置#1
      if Pos('Silicon Labs CP210x USB to UART Bridge', VarToStrDef(MyWmiObject.Properties_.Item('Description').Value, '')) > 0 then
      begin
        RunningLog_Memo.Lines.Add('!!!找到目標裝置(Silicon Labs CP210x USB to UART Bridge)。SN = ' + ExtractFileName(VarToStrDef(MyWmiObject.Properties_.Item('DeviceID').Value, '')) + '。');
        // 使用 IndexOf 檢查序號是否在PSU支援清單中
        if PSU_SupportedSerialNumbers_Memo.Lines.IndexOf( ExtractFileName(VarToStrDef(MyWmiObject.Properties_.Item('DeviceID').Value, '')) ) <> -1 then
        begin
             RunningLog_Memo.Lines.Add('!!!該裝置為已支援的PSU裝置。SN = ' + ExtractFileName(VarToStrDef(MyWmiObject.Properties_.Item('DeviceID').Value, '')) + '。更新資訊至主頁面板中...');
             PSU_SN_Edit.Text := ExtractFileName(VarToStrDef(MyWmiObject.Properties_.Item('DeviceID').Value, ''));
             PSU_SN_Edit.Font.Color := clGreen;
             RunningLog_Memo.Lines.Add('!!!該裝置為已支援的PSU裝置。描述 = ' + ExtractFileName(VarToStrDef(MyWmiObject.Properties_.Item('Caption').Value, '')) + '。');
             PSU_Edit.Text := ExtractFileName(VarToStrDef(MyWmiObject.Properties_.Item('Caption').Value, ''));
             PSU_Edit.Text := StringReplace(PSU_Edit.Text, 'Silicon Labs CP210x USB to UART Bridge (', '', []);
             PSU_Edit.Text := StringReplace(PSU_Edit.Text, ')', '', []);
             PSU_Edit.Font.Color := clGreen;
             RunningLog_Memo.Lines.Add('!!!該裝置為已支援的PSU裝置。SN = ' + ExtractFileName(VarToStrDef(MyWmiObject.Properties_.Item('DeviceID').Value, '')) + '。更新資訊至主頁面板中...完成!');
        end;
        // 使用 IndexOf 檢查序號是否在SwitchArray支援清單中
        if ESP32_SupportedSerialNumbers_Memo.Lines.IndexOf( ExtractFileName(VarToStrDef(MyWmiObject.Properties_.Item('DeviceID').Value, '')) ) <> -1 then
        begin
             RunningLog_Memo.Lines.Add('!!!該裝置為已支援的SwitchArray裝置。SN = ' + ExtractFileName(VarToStrDef(MyWmiObject.Properties_.Item('DeviceID').Value, '')) + '。更新資訊至主頁面板中...');
             ESP32_SN_Edit.Text := ExtractFileName(VarToStrDef(MyWmiObject.Properties_.Item('DeviceID').Value, ''));
             ESP32_SN_Edit.Text := StringReplace(ESP32_SN_Edit.Text, '2025-', '', []);
             ESP32_SN_Edit.Font.Color := clGreen;
             ESP32_Edit.Text := ExtractFileName(VarToStrDef(MyWmiObject.Properties_.Item('Caption').Value, ''));
             ESP32_Edit.Text := StringReplace(ESP32_Edit.Text, 'Silicon Labs CP210x USB to UART Bridge (', '', []);
             ESP32_Edit.Text := StringReplace(ESP32_Edit.Text, ')', '', []);
             ESP32_Edit.Font.Color := clGreen;
             RunningLog_Memo.Lines.Add('!!!該裝置為已支援的SwitchArray裝置。SN = ' + ExtractFileName(VarToStrDef(MyWmiObject.Properties_.Item('DeviceID').Value, '')) + '。更新資訊至主頁面板中...完成!');
        end;
      end;
      // 找到目標裝置#2
      if Pos('USB\VID_2A8D&PID_0101\', VarToStrDef(MyWmiObject.Properties_.Item('DeviceID').Value, '')) > 0 then
      begin
        RunningLog_Memo.Lines.Add('!!!找到目標裝置(USB\VID_2A8D&PID_0101\)。SN = ' + ExtractFileName(VarToStrDef(MyWmiObject.Properties_.Item('DeviceID').Value, '')) + '。');
        // 使用 IndexOf 檢查序號是否在DMM支援清單中
        if DMM_SupportedSerialNumbers_Memo.Lines.IndexOf( ExtractFileName(VarToStrDef(MyWmiObject.Properties_.Item('DeviceID').Value, '')) ) <> -1 then
        begin
             RunningLog_Memo.Lines.Add('!!!該裝置為已支援的DMM裝置。SN = ' + ExtractFileName(VarToStrDef(MyWmiObject.Properties_.Item('DeviceID').Value, '')) + '。更新資訊至主頁面板中...');
             DMM_SN_Edit.Text := ExtractFileName(VarToStrDef(MyWmiObject.Properties_.Item('DeviceID').Value, ''));
             DMM_SN_Edit.Font.Color := clGreen;
             DMM_Edit.Text := 'USB0::0x2A8D::0x0101::' + DMM_SN_Edit.Text + '::INSTR';
             DMM_Edit.Font.Color := clGreen;
             RunningLog_Memo.Lines.Add('!!!該裝置為已支援的DMM裝置。SN = ' + ExtractFileName(VarToStrDef(MyWmiObject.Properties_.Item('DeviceID').Value, '')) + '。更新資訊至主頁面板中...完成!');
        end;
      end;
      MyWmiObject := Unassigned;
    end;
    RunningLog_Memo.Lines.Add('步驟 4 完成。');
  except
    on E: Exception do
      RunningLog_Memo.Lines.Add('查詢 WMI 時發生錯誤: ' + E.Message);
  end;
  RunningLog_Memo.Lines.Add('正在使用 WMI 查詢 Win32_PnPEntity...完成!');
  //--------------------------------------------------------------------------
  //--------------------------------------------------------------------------
  // 開啟PSU的COM裝置來檢查是否設定正確
  if (PSU_Edit.Font.Color = clBlue) then
  begin
    RunningLog_Memo.Lines.Add('警告：PSU裝置資訊異常。是否沒有連接PSU?或是不在支援清單中?');
    PSU_Edit.Color := clYellow;
    Exit;
  end;
  //--
  PSU_Edit.Font.Color := clRed;
  Application.ProcessMessages;
  LazSerial1.Device := PSU_Edit.Text;
  LazSerial1.BaudRate:= br_57600;
  try
    // 步驟0：開啟COM裝置
    RunningLog_Memo.Lines.Add('嘗試開啟序列埠(' + LazSerial1.Device + ')...');
    LazSerial1.Open;
    RunningLog_Memo.Lines.Add('嘗試開啟序列埠(' + LazSerial1.Device + ')...完成!');
    // 步驟1：發送命令查儀器基本資訊(*IDN?)
    LazSerial1.WriteData('*IDN?' + #13#10);
    Sleep(100);
    Response := LazSerial1.ReadData;
    RunningLog_Memo.Lines.Add('PSU基本資訊 =' + Trim(Response));
    // 步驟2：發送命令查型號(MODEL?)
    LazSerial1.WriteData('MODEL?' + #13#10);
    Sleep(100);
    Response := LazSerial1.ReadData;
    RunningLog_Memo.Lines.Add('PSU型號 = ' + Trim(Response));
    // 步驟3：發送命令查序號(SYS:SER?)
    LazSerial1.WriteData('SYS:SER?' + #13#10);
    Sleep(100);
    Response := LazSerial1.ReadData;
    RunningLog_Memo.Lines.Add('PSU序號 = ' + Trim(Response));
    // 步驟4：發送命令查韌體版本(VERsion?)
    LazSerial1.WriteData('VERsion?' + #13#10);
    Sleep(100);
    Response := LazSerial1.ReadData;
    RunningLog_Memo.Lines.Add('PSU韌體版本 = ' + Trim(Response));
    // 步驟5：發送命令查電壓範圍(SOUR:VOLT:RANG?)
    if PSU_RangeHIGH_CheckBox.Checked then
    begin
      LazSerial1.WriteData('SOUR:VOLT:RANG HIGH' + #13#10);
    end
    else
    begin
      LazSerial1.WriteData('SOUR:VOLT:RANG LOW' + #13#10);
    end;
    LazSerial1.WriteData('SOUR:VOLT:RANG?' + #13#10);
    Sleep(100);
    Response := LazSerial1.ReadData;
    RunningLog_Memo.Lines.Add('PSU電壓範圍(HIGH=600V/0.35A, LOW=400V/0.5A) = ' + Trim(Response));
    if SameText(Trim(Response), 'HIGH') then
    begin
      if PSU_RangeHIGH_CheckBox.Checked then
      begin
        PSU_Edit.Font.Color := clGreen;
        Application.ProcessMessages;
      end;
    end
    else if SameText(Trim(Response), 'LOW') then
    begin
      if PSU_RangeLOW_CheckBox.Checked then
      begin
        PSU_Edit.Font.Color := clGreen;
        Application.ProcessMessages;
      end;
    end
    else
    begin
      RunningLog_Memo.Lines.Add('警告：PSU電壓範圍無效或與預期不符。是否沒有開啟PSU電源?或是PSU被其他軟體占用?');
    end;
    // 步驟6：發送命令查蜂鳴器設定(BEEP OFF)(SYS:BEEP?)
    LazSerial1.WriteData('BEEP OFF' + #13#10);
    LazSerial1.WriteData('SYS:BEEP?' + #13#10);
    Sleep(100);
    Response := LazSerial1.ReadData;
    RunningLog_Memo.Lines.Add('PSU蜂鳴器設定 = ' + Trim(Response));
    // 步驟7：發送命令查LED模式設定(SYS:LED ON)(SYS:LED?)
    LazSerial1.WriteData('SYS:LED ON' + #13#10);
    LazSerial1.WriteData('SYS:LED?' + #13#10);
    Sleep(100);
    Response := LazSerial1.ReadData;
    RunningLog_Memo.Lines.Add('PSULED模式 = ' + Trim(Response));
    // 步驟999：關閉COM裝置
    RunningLog_Memo.Lines.Add('嘗試關閉序列埠(' + LazSerial1.Device + ')...');
    LazSerial1.Close;
    RunningLog_Memo.Lines.Add('嘗試關閉序列埠(' + LazSerial1.Device + ')...完成!');
  except
    on E: Exception do
      RunningLog_Memo.Lines.Add('開啟序列埠失敗：' + E.Message);
  end;
  //--------------------------------------------------------------------------
  //--------------------------------------------------------------------------
  // 設定按鈕啟用禁用
  if (DMM_Edit.Font.Color = clGreen) and (ESP32_Edit.Font.Color = clGreen) and (PSU_Edit.Font.Color = clGreen) then
  begin
    NowRun_Button.Enabled := True;
    NowSystemTest_Button.Enabled := True;
    ScheduleRun_Button.Enabled := True;
  end;
  //--------------------------------------------------------------------------
end;     
```
+ 4.4 選「PSU_SupportedSerialNumbers_Memo」，去編輯「Lines」
```pascal
508F24126
```

### 啟動智慧排程
+ 6.1 去implementation區塊追加宣告
```pascal
implementation
//--------------------------------------------------------------------------
//uses add by HsiupoYeh
uses
  IniFiles, Windows, ShellApi, ComObj, ActiveX, Variants, FileUtil;
//--------------------------------------------------------------------------
//--------------------------------------------------------------------------
//宣告全域變數 add by HsiupoYeh
var
  version_str: AnsiString;
  Current_Folder_Path: AnsiString;
  ERT_ver2_9_9_Client_Schedule_flag: Integer = 0; // 0=不要做事,1=查找下一個即將,2=針對即將的時間倒數計時.
//--------------------------------------------------------------------------

{$R *.lfm} 
```
+ 6.2 選「ScheduleRun_Button」，去編輯「OnClick」
```pascal
procedure TForm1.ScheduleRun_ButtonClick(Sender: TObject);
var
  temp_str: string;
  temp_i: Integer;
begin
  //
  ScheduleRun_Button.Enabled := false;
  //--
  RunningLog_Memo.Lines.Add('啟動檢測排程...');
  //--
  // 清空工作空間
  if DirectoryExists('.\R2MS_Lite_WorkSpace\') then
  begin
    DeleteDirectory('.\R2MS_Lite_WorkSpace\', True); // 第二個參數帶true表示遞迴刪除內容
  end;
  ForceDirectories('.\R2MS_Lite_WorkSpace\ERT_ver2_9_9_Client_Data\');
  //--
  // 複製執行檔「.\Programs\ERT_ver2_9_9_Client\ERT_ver2_9_9_Client.aliases」
  if FileExists('.\Programs\ERT_ver2_9_9_Client\ERT_ver2_9_9_Client.aliases') then
  begin
    RunningLog_Memo.Lines.Add('「.\Programs\ERT_ver2_9_9_Client\ERT_ver2_9_9_Client.aliases」存在，開始複製...');
    CopyFile(PChar('.\Programs\ERT_ver2_9_9_Client\ERT_ver2_9_9_Client.aliases'), PChar('.\R2MS_Lite_WorkSpace\ERT_ver2_9_9_Client.aliases'), False); // false=覆蓋,true=略過
    RunningLog_Memo.Lines.Add('「.\Programs\ERT_ver2_9_9_Client\ERT_ver2_9_9_Client.aliases」存在，開始複製...結束!');
  end
  else
  begin
    RunningLog_Memo.Lines.Add('錯誤!「.\Programs\ERT_ver2_9_9_Client\ERT_ver2_9_9_Client.aliases」不存在!');
    Application.MessageBox(PChar('目標「ERT_ver2_9_9_Client.aliases」檔案不存在'), PChar('錯誤'), MB_OK + MB_ICONERROR);
    ScheduleRun_Button.Enabled := True;
    Exit;
  end;
  //--
  // 複製執行檔「.\Programs\ERT_ver2_9_9_Client\ERT_ver2_9_9_Client.exe」
  if FileExists('.\Programs\ERT_ver2_9_9_Client\ERT_ver2_9_9_Client.exe') then
  begin
    RunningLog_Memo.Lines.Add('「.\Programs\ERT_ver2_9_9_Client\ERT_ver2_9_9_Client.exe」存在，開始複製...');
    CopyFile(PChar('.\Programs\ERT_ver2_9_9_Client\ERT_ver2_9_9_Client.exe'), PChar('.\R2MS_Lite_WorkSpace\ERT_ver2_9_9_Client.exe'), False); // false=覆蓋,true=略過
    RunningLog_Memo.Lines.Add('「.\Programs\ERT_ver2_9_9_Client\ERT_ver2_9_9_Client.exe」存在，開始複製...結束!');
  end
  else
  begin
    RunningLog_Memo.Lines.Add('錯誤!「.\Programs\ERT_ver2_9_9_Client\ERT_ver2_9_9_Client.exe」不存在!');
    Application.MessageBox(PChar('目標「ERT_ver2_9_9_Client.exe」檔案不存在'), PChar('錯誤'), MB_OK + MB_ICONERROR);
    ScheduleRun_Button.Enabled := True;
    Exit;
  end;
  //--
  // 複製執行檔「.\Programs\ERT_ver2_9_9_Client\ERT_ver2_9_9_Client.ini」
  if FileExists('.\Programs\ERT_ver2_9_9_Client\ERT_ver2_9_9_Client.ini') then
  begin
    RunningLog_Memo.Lines.Add('「.\Programs\ERT_ver2_9_9_Client\ERT_ver2_9_9_Client.ini」存在，開始複製...');
    CopyFile(PChar('.\Programs\ERT_ver2_9_9_Client\ERT_ver2_9_9_Client.ini'), PChar('.\R2MS_Lite_WorkSpace\ERT_ver2_9_9_Client.ini'), False); // false=覆蓋,true=略過
    RunningLog_Memo.Lines.Add('「.\Programs\ERT_ver2_9_9_Client\ERT_ver2_9_9_Client.ini」存在，開始複製...結束!');
  end
  else
  begin
    RunningLog_Memo.Lines.Add('錯誤!「.\Programs\ERT_ver2_9_9_Client\ERT_ver2_9_9_Client.ini」不存在!');
    Application.MessageBox(PChar('目標「ERT_ver2_9_9_Client.ini」檔案不存在'), PChar('錯誤'), MB_OK + MB_ICONERROR);
    ScheduleRun_Button.Enabled := True;
    Exit;
  end;
  //--
  // 檢查CurrentMode檔「.\Configs\v299_SystemTestCurrentMode\Current Mode.csv」
  if FileExists('.\Configs\v299_SystemTestCurrentMode\Current Mode.csv') then
  begin
    RunningLog_Memo.Lines.Add('「.\Configs\v299_SystemTestCurrentMode\Current Mode.csv」存在!');
  end
  else
  begin
    RunningLog_Memo.Lines.Add('錯誤!「.\Configs\v299_SystemTestCurrentMode\Current Mode.csv」不存在!');
    Application.MessageBox(PChar('目標「.\Configs\v299_SystemTestCurrentMode\Current Mode.csv」檔案不存在'), PChar('錯誤'), MB_OK + MB_ICONERROR);
    ScheduleRun_Button.Enabled := True;
    Exit;
  end;
  //--
  // 檢查CurrentMode檔「.\Configs\v299_M64CurrentMode\Current Mode.csv」
  if FileExists('.\Configs\v299_M64CurrentMode\Current Mode.csv') then
  begin
    RunningLog_Memo.Lines.Add('「.\Configs\v299_M64CurrentMode\Current Mode.csv」存在!');
  end
  else
  begin
    RunningLog_Memo.Lines.Add('錯誤!「.\Configs\v299_M64CurrentMode\Current Mode.csv」不存在!');
    Application.MessageBox(PChar('目標「.\Configs\v299_M64CurrentMode\Current Mode.csv」檔案不存在'), PChar('錯誤'), MB_OK + MB_ICONERROR);
    ScheduleRun_Button.Enabled := True;
    Exit;
  end;
  //--
  // 檢查CurrentMode檔「.\Configs\v299_S64VHSCurrentMode\Current Mode.csv」
  if FileExists('.\Configs\v299_S64VHSCurrentMode\Current Mode.csv') then
  begin
    RunningLog_Memo.Lines.Add('「.\Configs\v299_S64VHSCurrentMode\Current Mode.csv」存在!');
  end
  else
  begin
    RunningLog_Memo.Lines.Add('錯誤!「.\Configs\v299_S64VHSCurrentMode\Current Mode.csv」不存在!');
    Application.MessageBox(PChar('目標「.\Configs\v299_S64VHSCurrentMode\Current Mode.csv」檔案不存在'), PChar('錯誤'), MB_OK + MB_ICONERROR);
    ScheduleRun_Button.Enabled := True;
    Exit;
  end;
  //--
  // 檢查CurrentMode檔「.\Configs\v299_S64HSCurrentMode\Current Mode.csv」
  if FileExists('.\Configs\v299_S64HSCurrentMode\Current Mode.csv') then
  begin
    RunningLog_Memo.Lines.Add('「.\Configs\v299_S64HSCurrentMode\Current Mode.csv」存在!');
  end
  else
  begin
    RunningLog_Memo.Lines.Add('錯誤!「.\Configs\v299_S64HSCurrentMode\Current Mode.csv」不存在!');
    Application.MessageBox(PChar('目標「.\Configs\v299_S64HSCurrentMode\Current Mode.csv」檔案不存在'), PChar('錯誤'), MB_OK + MB_ICONERROR);
    ScheduleRun_Button.Enabled := True;
    Exit;
  end;
  //--
  // 檢查CurrentMode檔「.\Configs\v299_S64NCurrentMode\Current Mode.csv」
  if FileExists('.\Configs\v299_S64NCurrentMode\Current Mode.csv') then
  begin
    RunningLog_Memo.Lines.Add('「.\Configs\v299_S64NCurrentMode\Current Mode.csv」存在!');
  end
  else
  begin
    RunningLog_Memo.Lines.Add('錯誤!「.\Configs\v299_S64NCurrentMode\Current Mode.csv」不存在!');
    Application.MessageBox(PChar('目標「.\Configs\v299_S64NCurrentMode\Current Mode.csv」檔案不存在'), PChar('錯誤'), MB_OK + MB_ICONERROR);
    ScheduleRun_Button.Enabled := True;
    Exit;
  end;
  //--
  // 檢查CurrentMode檔「.\Configs\v299_Custom01CurrentMode\Current Mode.csv」
  if FileExists('.\Configs\v299_Custom01CurrentMode\Current Mode.csv') then
  begin
    RunningLog_Memo.Lines.Add('「.\Configs\v299_Custom01CurrentMode\Current Mode.csv」存在!');
  end
  else
  begin
    RunningLog_Memo.Lines.Add('錯誤!「.\Configs\v299_Custom01CurrentMode\Current Mode.csv」不存在!');
    Application.MessageBox(PChar('目標「.\Configs\v299_Custom01CurrentMode\Current Mode.csv」檔案不存在'), PChar('錯誤'), MB_OK + MB_ICONERROR);
    ScheduleRun_Button.Enabled := True;
    Exit;
  end;
  //--
  // 檢查CurrentMode檔「.\Configs\v299_Custom02CurrentMode\Current Mode.csv」
  if FileExists('.\Configs\v299_Custom02CurrentMode\Current Mode.csv') then
  begin
    RunningLog_Memo.Lines.Add('「.\Configs\v299_Custom02CurrentMode\Current Mode.csv」存在!');
  end
  else
  begin
    RunningLog_Memo.Lines.Add('錯誤!「.\Configs\v299_Custom02CurrentMode\Current Mode.csv」不存在!');
    Application.MessageBox(PChar('目標「.\Configs\v299_Custom02CurrentMode\Current Mode.csv」檔案不存在'), PChar('錯誤'), MB_OK + MB_ICONERROR);
    ScheduleRun_Button.Enabled := True;
    Exit;
  end;
  //--
  // 檢查CurrentMode檔「.\Configs\v299_Custom03CurrentMode\Current Mode.csv」
  if FileExists('.\Configs\v299_Custom03CurrentMode\Current Mode.csv') then
  begin
    RunningLog_Memo.Lines.Add('「.\Configs\v299_Custom03CurrentMode\Current Mode.csv」存在!');
  end
  else
  begin
    RunningLog_Memo.Lines.Add('錯誤!「.\Configs\v299_Custom03CurrentMode\Current Mode.csv」不存在!');
    Application.MessageBox(PChar('目標「.\Configs\v299_Custom03CurrentMode\Current Mode.csv」檔案不存在'), PChar('錯誤'), MB_OK + MB_ICONERROR);
    ScheduleRun_Button.Enabled := True;
    Exit;
  end;
  //--
  // 檢查setting檔「.\Configs\v299_SystemTestSetting\setting.txt」
  if FileExists('.\Configs\v299_SystemTestSetting\setting.txt') then
  begin
    RunningLog_Memo.Lines.Add('「.\Configs\v299_SystemTestSetting\setting.txt」存在!');
  end
  else
  begin
    RunningLog_Memo.Lines.Add('錯誤!「.\Configs\v299_SystemTestSetting\setting.txt」不存在!');
    Application.MessageBox(PChar('目標「.\Configs\v299_SystemTestSetting\setting.txt」檔案不存在'), PChar('錯誤'), MB_OK + MB_ICONERROR);
    ScheduleRun_Button.Enabled := True;
    Exit;
  end;
  //--
  // 檢查setting檔「.\Configs\v299_M64Setting\setting.txt」
  if FileExists('.\Configs\v299_M64Setting\setting.txt') then
  begin
    RunningLog_Memo.Lines.Add('「.\Configs\v299_M64Setting\setting.txt」存在!');
  end
  else
  begin
    RunningLog_Memo.Lines.Add('錯誤!「.\Configs\v299_M64Setting\setting.txt」不存在!');
    Application.MessageBox(PChar('目標「.\Configs\v299_M64Setting\setting.txt」檔案不存在'), PChar('錯誤'), MB_OK + MB_ICONERROR);
    ScheduleRun_Button.Enabled := True;
    Exit;
  end;
  //--
  // 檢查setting檔「.\Configs\v299_S64VHSSetting\setting.txt」
  if FileExists('.\Configs\v299_S64VHSSetting\setting.txt') then
  begin
    RunningLog_Memo.Lines.Add('「.\Configs\v299_S64VHSSetting\setting.txt」存在!');
  end
  else
  begin
    RunningLog_Memo.Lines.Add('錯誤!「.\Configs\v299_S64VHSSetting\setting.txt」不存在!');
    Application.MessageBox(PChar('目標「.\Configs\v299_S64VHSSetting\setting.txt」檔案不存在'), PChar('錯誤'), MB_OK + MB_ICONERROR);
    ScheduleRun_Button.Enabled := True;
    Exit;
  end;
  //--
  // 檢查setting檔「.\Configs\v299_S64HSSetting\setting.txt」
  if FileExists('.\Configs\v299_S64HSSetting\setting.txt') then
  begin
    RunningLog_Memo.Lines.Add('「.\Configs\v299_S64HSSetting\setting.txt」存在!');
  end
  else
  begin
    RunningLog_Memo.Lines.Add('錯誤!「.\Configs\v299_S64HSSetting\setting.txt」不存在!');
    Application.MessageBox(PChar('目標「.\Configs\v299_S64HSSetting\setting.txt」檔案不存在'), PChar('錯誤'), MB_OK + MB_ICONERROR);
    ScheduleRun_Button.Enabled := True;
    Exit;
  end;
  //--
  // 檢查setting檔「.\Configs\v299_S64NSetting\setting.txt」
  if FileExists('.\Configs\v299_S64NSetting\setting.txt') then
  begin
    RunningLog_Memo.Lines.Add('「.\Configs\v299_S64NSetting\setting.txt」存在!');
  end
  else
  begin
    RunningLog_Memo.Lines.Add('錯誤!「.\Configs\v299_S64NSetting\setting.txt」不存在!');
    Application.MessageBox(PChar('目標「.\Configs\v299_S64NSetting\setting.txt」檔案不存在'), PChar('錯誤'), MB_OK + MB_ICONERROR);
    ScheduleRun_Button.Enabled := True;
    Exit;
  end;
  //--
  // 檢查setting檔「.\Configs\v299_Custom01Setting\setting.txt」
  if FileExists('.\Configs\v299_Custom01Setting\setting.txt') then
  begin
    RunningLog_Memo.Lines.Add('「.\Configs\v299_Custom01Setting\setting.txt」存在!');
  end
  else
  begin
    RunningLog_Memo.Lines.Add('錯誤!「.\Configs\v299_Custom01Setting\setting.txt」不存在!');
    Application.MessageBox(PChar('目標「.\Configs\v299_Custom01Setting\setting.txt」檔案不存在'), PChar('錯誤'), MB_OK + MB_ICONERROR);
    ScheduleRun_Button.Enabled := True;
    Exit;
  end;
  //--
  // 檢查setting檔「.\Configs\v299_Custom02Setting\setting.txt」
  if FileExists('.\Configs\v299_Custom02Setting\setting.txt') then
  begin
    RunningLog_Memo.Lines.Add('「.\Configs\v299_Custom02Setting\setting.txt」存在!');
  end
  else
  begin
    RunningLog_Memo.Lines.Add('錯誤!「.\Configs\v299_Custom02Setting\setting.txt」不存在!');
    Application.MessageBox(PChar('目標「.\Configs\v299_Custom02Setting\setting.txt」檔案不存在'), PChar('錯誤'), MB_OK + MB_ICONERROR);
    ScheduleRun_Button.Enabled := True;
    Exit;
  end;
  //--
  // 檢查setting檔「.\Configs\v299_Custom03Setting\setting.txt」
  if FileExists('.\Configs\v299_Custom03Setting\setting.txt') then
  begin
    RunningLog_Memo.Lines.Add('「.\Configs\v299_Custom03Setting\setting.txt」存在!');
  end
  else
  begin
    RunningLog_Memo.Lines.Add('錯誤!「.\Configs\v299_Custom03Setting\setting.txt」不存在!');
    Application.MessageBox(PChar('目標「.\Configs\v299_Custom03Setting\setting.txt」檔案不存在'), PChar('錯誤'), MB_OK + MB_ICONERROR);
    ScheduleRun_Button.Enabled := True;
    Exit;
  end;
  //--
  for temp_i := 0 to ScheduleSetting_ListView.Items.Count - 1 do
  begin
    // 檢查項目的勾選狀態
    if not ScheduleSetting_ListView.Items[temp_i].Checked then
    begin
      ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[3] := '停用';
    end;
  end;
  //--
  // 開始ERT_ver2_9_9_Client計時器監控
  RunningLog_Memo.Lines.Add('開始ERT_ver2_9_9_Client計時器監控...');
  ERT_ver2_9_9_Client_Schedule_flag := 1;  // 0=不要做事,1=查找下一個即將,2=針對即將的時間倒數計時.
  RunningLog_Memo.Lines.Add('開始ERT_ver2_9_9_Client計時器監控...結束');
  //--
end;    
```
