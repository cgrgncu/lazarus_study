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
### 版本
+ 1.1選選Form，去編輯FormCreate，修改部分:
```pascal
  //--------------------------------------------------------------------------
  // 在這裡對全域變數進行初始化
  version_str := 'v20250923b';
  Current_Folder_Path := ExtractFilePath(Application.ExeName);
  //--------------------------------------------------------------------------  
```

### 啟動智慧排程
+ 2.1 選「ScheduleRun_Button」，去編輯「OnClick」，修改部分:
```pascal
begin
  //--------------------------------------------------------------------------
  // 設定按鈕啟用禁用
  NowRun_Button.Enabled := False;
  NowSystemTest_Button.Enabled := False;
  ScheduleRun_Button.Enabled := False;
  //--------------------------------------------------------------------------
  //--
  RunningLog_Memo.Lines.Add('啟動檢測排程...');  
```

### ERT_ver2_9_9_Client_Schedule_Timer
+ 3.1 選「ERT_ver2_9_9_Client_Schedule_Timer」，去編輯「OnTimer」。
```pascal
procedure TForm1.ERT_ver2_9_9_Client_Schedule_TimerTimer(Sender: TObject);
var
  temp_now_TDateTime: TDateTime;
  temp_Job_TDateTime: TDateTime;
  temp_NextJob_TDateTime: TDateTime;
  temp_str, temp_str2: string;
  temp_SecondsBetween: Integer;
  temp_i: Integer;
  temp_TStringList: TStringList;
  temp_Zipper: TZipper;
begin
  //--------------------------------------------------------------------------
  // 更新時間
  temp_now_TDateTime := Now;
  StatusBar1.Panels[4].Text := FormatDateTime('yyyy/mm/dd HH:MM:SS', temp_now_TDateTime);
  //--------------------------------------------------------------------------
  //--------------------------------------------------------------------------
  // 避免忙碌時進行過多操作
  if ERT_ver2_9_9_Client_Schedule_Timer_busy then
    Exit;
  //--------------------------------------------------------------------------
  //--------------------------------------------------------------------------
  // 開始做事前設定為忙碌
  ERT_ver2_9_9_Client_Schedule_Timer_busy := True;
  //--------------------------------------------------------------------------


  //--------------------------------------------------------------------------
  // 狀態 1: 查找下個即將執行的排程
  // ---
  if ERT_ver2_9_9_Client_Schedule_flag = 1 then
  begin
    //--------------------------------------------------------------------------
    // 迴圈遍歷 ScheduleSetting_ListView 中的所有項目，計算排程時間與當前時間的秒數差。
    // 這裡使用索引 i 來遍歷列表項目。
    for temp_i := 0 to ScheduleSetting_ListView.Items.Count - 1 do
    begin
      if ScheduleSetting_ListView.Items[temp_i].Checked then
      begin
        //RunningLog_Memo.Lines.Add('--');
        //RunningLog_Memo.Lines.Add('有勾選的排程編號: ' + ScheduleSetting_ListView.Items[temp_i].Caption + '，對應目標時間: ' + ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[0] + '。');
        temp_Job_TDateTime := StrToDateTime(FormatDateTime('yyyy/mm/dd ', temp_now_TDateTime) + ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[0] + ':01');
        //RunningLog_Memo.Lines.Add(FormatDateTime('yyyy/mm/dd HH:MM:SS', temp_Job_TDateTime));
        //
        if CompareDateTime(temp_now_TDateTime, temp_Job_TDateTime) = -1 then
        begin
          ////RunningLog_Memo.Lines.Add(FormatDateTime('yyyy/mm/dd HH:MM:SS', temp_now_TDateTime) + ' < ' + FormatDateTime('yyyy/mm/dd HH:MM:SS', temp_Job_TDateTime));
          RunningLog_Memo.Lines.Add('排程時間在未來:');
          temp_SecondsBetween := SecondsBetween(temp_now_TDateTime, temp_Job_TDateTime);
          //RunningLog_Memo.Lines.Add('還相差: ' + IntToStr(temp_SecondsBetween) + ' 秒');
          ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[4] := IntToStr(temp_SecondsBetween);
          ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[6] := FormatDateTime('yyyy/mm/dd HH:MM:SS', temp_Job_TDateTime);
        end
        else
        begin
          //RunningLog_Memo.Lines.Add('排程時間在過去:');
          temp_SecondsBetween := -1 * SecondsBetween(temp_now_TDateTime, temp_Job_TDateTime);
          //RunningLog_Memo.Lines.Add('還相差: ' + IntToStr(temp_SecondsBetween) + ' 秒');
          if EveryDay_RadioButton.Checked then
          begin
            //RunningLog_Memo.Lines.Add('增加排程時間1天:');
            temp_SecondsBetween := SecondsBetween(temp_now_TDateTime, IncDay(temp_Job_TDateTime, 1));
            //RunningLog_Memo.Lines.Add('還相差: ' + IntToStr(temp_SecondsBetween) + ' 秒');
            ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[4] := IntToStr(temp_SecondsBetween);
            ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[6] := FormatDateTime('yyyy/mm/dd HH:MM:SS', IncDay(temp_Job_TDateTime, 1));
          end
          else
          begin
            ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[4] := IntToStr(temp_SecondsBetween);
          end;
        end;
      end;
    end;
    //--------------------------------------------------------------------------
    //--------------------------------------------------------------------------
    // 尋找最小正秒數的排程
    ScheduleSetting_ListView.ItemIndex := -1;
    for temp_i := 0 to ScheduleSetting_ListView.Items.Count - 1 do
    begin
      if ScheduleSetting_ListView.Items[temp_i].Checked and (StrToIntDef(ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[4], 0) > 0) then
      begin
        if ScheduleSetting_ListView.ItemIndex = -1 then
        begin
          ScheduleSetting_ListView.ItemIndex := temp_i;
          //----------------------------------------------------------
          // 選到該列，並使其可被看到(調整捲軸)
          ScheduleSetting_ListView.SetFocus;
          ScheduleSetting_ListView.Items[ScheduleSetting_ListView.ItemIndex].Selected := True;
          ScheduleSetting_ListView.Items[ScheduleSetting_ListView.ItemIndex].Focused := True;
          ScheduleSetting_ListView.Items[ScheduleSetting_ListView.ItemIndex].MakeVisible(False);
          Application.ProcessMessages;
          //----------------------------------------------------------
        end;
        RunningLog_Memo.Lines.Add('目前選擇的(#' + ScheduleSetting_ListView.Items[ScheduleSetting_ListView.ItemIndex].Caption + ')最小正秒數 = ' + ScheduleSetting_ListView.Items[ScheduleSetting_ListView.ItemIndex].SubItems.Strings[4]);
        if StrToIntDef(ScheduleSetting_ListView.Items[ScheduleSetting_ListView.ItemIndex].SubItems.Strings[4], 0) > StrToIntDef(ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[4], 0) then
        begin
          ScheduleSetting_ListView.ItemIndex := temp_i;
          //----------------------------------------------------------
          // 選到該列，並使其可被看到(調整捲軸)
          ScheduleSetting_ListView.SetFocus;
          ScheduleSetting_ListView.Items[ScheduleSetting_ListView.ItemIndex].Selected := True;
          ScheduleSetting_ListView.Items[ScheduleSetting_ListView.ItemIndex].Focused := True;
          ScheduleSetting_ListView.Items[ScheduleSetting_ListView.ItemIndex].MakeVisible(False);
          Application.ProcessMessages;
          //----------------------------------------------------------
          RunningLog_Memo.Lines.Add('重新選擇的最小正秒數 = ' + ScheduleSetting_ListView.Items[ScheduleSetting_ListView.ItemIndex].SubItems.Strings[4]);
        end;
      end;
    end;
    //--
    if ScheduleSetting_ListView.ItemIndex = -1 then
    begin
      //--------------------------------------------------------------------------
      // 調整為狀態0
      ERT_ver2_9_9_Client_Schedule_flag := 0; //0=不要做事,1=查找下個即將,2=針對即將進行工作,3=針對初始化進行工作,4=針對等待運作進行工作
      StatusBar1.Panels[1].Text := '排程狀態=' +IntToStr(ERT_ver2_9_9_Client_Schedule_flag);
      //--------------------------------------------------------------------------
      Application.MessageBox('提示:' + #13#10 + '沒有任何工作需要進行!停止排程工作!', '提示', 64);
      Exit;
    end;
    //--------------------------------------------------------------------------
    //--------------------------------------------------------------------------
    // 設定即將執行的排程
    for temp_i := 0 to ScheduleSetting_ListView.Items.Count - 1 do
    begin
      if ScheduleSetting_ListView.Items[temp_i].Checked then
      begin
        if ScheduleSetting_ListView.ItemIndex = temp_i then
        begin
          ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[4] := '即將';
          temp_NextJob_TDateTime := StrToDateTime(ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[6]);

          //--
          if ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[1] = '系統檢測' then
          begin
            RunningLog_Memo.Lines.Add('「.\Configs\v299_SystemTestCurrentMode\Current Mode.csv」，開始複製...');
            CopyFile(PChar('.\Configs\v299_SystemTestCurrentMode\Current Mode.csv'), PChar('.\R2MS_Lite_WorkSpace\Current Mode.csv'), False); // false=覆蓋,true=略過
            RunningLog_Memo.Lines.Add('「.\Configs\v299_SystemTestCurrentMode\Current Mode.csv」，開始複製...結束!');
            v299_DefaultSetting_Memo.Lines.LoadFromFile('.\Configs\v299_SystemTestSetting\setting.txt');
            //--
            v299_NowSetting_Memo.Lines.Assign(v299_DefaultSetting_Memo.Lines);
            v299_NowSetting_Memo.Lines.Strings[1] := 'DMM VISA = "' + DMM_Edit.Text + '"';
            v299_NowSetting_Memo.Lines.Strings[2] := 'ESP32 VISA = "' + ESP32_Edit.Text + '"';
            v299_NowSetting_Memo.Lines.Strings[7] := 'Data Directory = "' + ERT_ver2_9_9_Client_SavePath_Edit.Text + '"';
            v299_NowSetting_Memo.Lines.Strings[12] := 'PSU VISA = "' + PSU_Edit.Text + '"';
            v299_NowSetting_Memo.Lines.Strings[31] := 'Scan Start Timer = "' + FormatDateTime('yyyy_mm_dd_HH_MM', IncMinute(temp_NextJob_TDateTime,1)  ) + ',"';
            v299_NowSetting_Memo.Lines.Strings[32] := 'Leave Timer = "' + FormatDateTime('yyyy_mm_dd_HH_MM', IncMinute(temp_NextJob_TDateTime,Ceil(StrToFloat(ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[7])*1.15)+1)  ) + '"';
            ForceDirectories('.\R2MS_Lite_WorkSpace\');
            v299_NowSetting_Memo.Lines.SaveToFile('.\R2MS_Lite_WorkSpace\setting.txt');
          end
          else if ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[1] = 'M64' then
          begin
            RunningLog_Memo.Lines.Add('「.\Configs\v299_M64CurrentMode\Current Mode.csv」，開始複製...');
            CopyFile(PChar('.\Configs\v299_M64CurrentMode\Current Mode.csv'), PChar('.\R2MS_Lite_WorkSpace\Current Mode.csv'), False); // false=覆蓋,true=略過
            RunningLog_Memo.Lines.Add('「.\Configs\v299_M64CurrentMode\Current Mode.csv」，開始複製...結束!');
            v299_DefaultSetting_Memo.Lines.LoadFromFile('.\Configs\v299_M64Setting\setting.txt');
            //--
            v299_NowSetting_Memo.Lines.Assign(v299_DefaultSetting_Memo.Lines);
            v299_NowSetting_Memo.Lines.Strings[1] := 'DMM VISA = "' + DMM_Edit.Text + '"';
            v299_NowSetting_Memo.Lines.Strings[2] := 'ESP32 VISA = "' + ESP32_Edit.Text + '"';
            v299_NowSetting_Memo.Lines.Strings[7] := 'Data Directory = "' + ERT_ver2_9_9_Client_SavePath_Edit.Text + '"';
            v299_NowSetting_Memo.Lines.Strings[12] := 'PSU VISA = "' + PSU_Edit.Text + '"';
            v299_NowSetting_Memo.Lines.Strings[21] := 'Output Max Voltage = ' + StringReplace(ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[2], 'V', '', []);
            v299_NowSetting_Memo.Lines.Strings[22] := 'Output Max Current = ' + StringReplace(ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[3], 'A', '', []);
            v299_NowSetting_Memo.Lines.Strings[31] := 'Scan Start Timer = "' + FormatDateTime('yyyy_mm_dd_HH_MM', IncMinute(temp_NextJob_TDateTime, 1)) + ',"';
            v299_NowSetting_Memo.Lines.Strings[32] := 'Leave Timer = "' + FormatDateTime('yyyy_mm_dd_HH_MM', IncMinute(temp_NextJob_TDateTime, Ceil(StrToFloat(ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[7])*1.15)+1 )) + '"';
            ForceDirectories('.\R2MS_Lite_WorkSpace\');
            v299_NowSetting_Memo.Lines.SaveToFile('.\R2MS_Lite_WorkSpace\setting.txt');
          end
          else if ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[1] = 'S64N' then
          begin
            RunningLog_Memo.Lines.Add('「.\Configs\v299_S64NCurrentMode\Current Mode.csv」，開始複製...');
            CopyFile(PChar('.\Configs\v299_S64NCurrentMode\Current Mode.csv'), PChar('.\R2MS_Lite_WorkSpace\Current Mode.csv'), False); // false=覆蓋,true=略過
            RunningLog_Memo.Lines.Add('「.\Configs\v299_S64NCurrentMode\Current Mode.csv」，開始複製...結束!');
            v299_DefaultSetting_Memo.Lines.LoadFromFile('.\Configs\v299_S64NSetting\setting.txt');
            //--
            v299_NowSetting_Memo.Lines.Assign(v299_DefaultSetting_Memo.Lines);
            v299_NowSetting_Memo.Lines.Strings[1] := 'DMM VISA = "' + DMM_Edit.Text + '"';
            v299_NowSetting_Memo.Lines.Strings[2] := 'ESP32 VISA = "' + ESP32_Edit.Text + '"';
            v299_NowSetting_Memo.Lines.Strings[7] := 'Data Directory = "' + ERT_ver2_9_9_Client_SavePath_Edit.Text + '"';
            v299_NowSetting_Memo.Lines.Strings[12] := 'PSU VISA = "' + PSU_Edit.Text + '"';
            v299_NowSetting_Memo.Lines.Strings[21] := 'Output Max Voltage = ' + StringReplace(ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[2], 'V', '', []);
            v299_NowSetting_Memo.Lines.Strings[22] := 'Output Max Current = ' + StringReplace(ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[3], 'A', '', []);
            v299_NowSetting_Memo.Lines.Strings[31] := 'Scan Start Timer = "' + FormatDateTime('yyyy_mm_dd_HH_MM', IncMinute(temp_NextJob_TDateTime, 1)) + ',"';
            v299_NowSetting_Memo.Lines.Strings[32] := 'Leave Timer = "' + FormatDateTime('yyyy_mm_dd_HH_MM', IncMinute(temp_NextJob_TDateTime, Ceil(StrToFloat(ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[7])*1.15)+1 )) + '"';
            ForceDirectories('.\R2MS_Lite_WorkSpace\');
            v299_NowSetting_Memo.Lines.SaveToFile('.\R2MS_Lite_WorkSpace\setting.txt');
          end
          else if ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[1] = 'S64HS' then
          begin
            RunningLog_Memo.Lines.Add('「.\Configs\v299_S64HSCurrentMode\Current Mode.csv」，開始複製...');
            CopyFile(PChar('.\Configs\v299_S64HSCurrentMode\Current Mode.csv'), PChar('.\R2MS_Lite_WorkSpace\Current Mode.csv'), False); // false=覆蓋,true=略過
            RunningLog_Memo.Lines.Add('「.\Configs\v299_S64HSCurrentMode\Current Mode.csv」，開始複製...結束!');
            v299_DefaultSetting_Memo.Lines.LoadFromFile('.\Configs\v299_S64HSSetting\setting.txt');
            //--
            v299_NowSetting_Memo.Lines.Assign(v299_DefaultSetting_Memo.Lines);
            v299_NowSetting_Memo.Lines.Strings[1] := 'DMM VISA = "' + DMM_Edit.Text + '"';
            v299_NowSetting_Memo.Lines.Strings[2] := 'ESP32 VISA = "' + ESP32_Edit.Text + '"';
            v299_NowSetting_Memo.Lines.Strings[7] := 'Data Directory = "' + ERT_ver2_9_9_Client_SavePath_Edit.Text + '"';
            v299_NowSetting_Memo.Lines.Strings[12] := 'PSU VISA = "' + PSU_Edit.Text + '"';
            v299_NowSetting_Memo.Lines.Strings[21] := 'Output Max Voltage = ' + StringReplace(ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[2], 'V', '', []);
            v299_NowSetting_Memo.Lines.Strings[22] := 'Output Max Current = ' + StringReplace(ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[3], 'A', '', []);
            v299_NowSetting_Memo.Lines.Strings[31] := 'Scan Start Timer = "' + FormatDateTime('yyyy_mm_dd_HH_MM', IncMinute(temp_NextJob_TDateTime, 1)) + ',"';
            v299_NowSetting_Memo.Lines.Strings[32] := 'Leave Timer = "' + FormatDateTime('yyyy_mm_dd_HH_MM', IncMinute(temp_NextJob_TDateTime, Ceil(StrToFloat(ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[7])*1.15)+1 )) + '"';
            ForceDirectories('.\R2MS_Lite_WorkSpace\');
            v299_NowSetting_Memo.Lines.SaveToFile('.\R2MS_Lite_WorkSpace\setting.txt');
          end
          else if ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[1] = 'S64VHS' then
          begin
            RunningLog_Memo.Lines.Add('「.\Configs\v299_S64VHSCurrentMode\Current Mode.csv」，開始複製...');
            CopyFile(PChar('.\Configs\v299_S64VHSCurrentMode\Current Mode.csv'), PChar('.\R2MS_Lite_WorkSpace\Current Mode.csv'), False); // false=覆蓋,true=略過
            RunningLog_Memo.Lines.Add('「.\Configs\v299_S64VHSCurrentMode\Current Mode.csv」，開始複製...結束!');
            v299_DefaultSetting_Memo.Lines.LoadFromFile('.\Configs\v299_S64VHSSetting\setting.txt');
            //--
            v299_NowSetting_Memo.Lines.Assign(v299_DefaultSetting_Memo.Lines);
            v299_NowSetting_Memo.Lines.Strings[1] := 'DMM VISA = "' + DMM_Edit.Text + '"';
            v299_NowSetting_Memo.Lines.Strings[2] := 'ESP32 VISA = "' + ESP32_Edit.Text + '"';
            v299_NowSetting_Memo.Lines.Strings[7] := 'Data Directory = "' + ERT_ver2_9_9_Client_SavePath_Edit.Text + '"';
            v299_NowSetting_Memo.Lines.Strings[12] := 'PSU VISA = "' + PSU_Edit.Text + '"';
            v299_NowSetting_Memo.Lines.Strings[21] := 'Output Max Voltage = ' + StringReplace(ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[2], 'V', '', []);
            v299_NowSetting_Memo.Lines.Strings[22] := 'Output Max Current = ' + StringReplace(ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[3], 'A', '', []);
            v299_NowSetting_Memo.Lines.Strings[31] := 'Scan Start Timer = "' + FormatDateTime('yyyy_mm_dd_HH_MM', IncMinute(temp_NextJob_TDateTime, 1)) + ',"';
            v299_NowSetting_Memo.Lines.Strings[32] := 'Leave Timer = "' + FormatDateTime('yyyy_mm_dd_HH_MM', IncMinute(temp_NextJob_TDateTime, Ceil(StrToFloat(ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[7])*1.15)+1 )) + '"';
            ForceDirectories('.\R2MS_Lite_WorkSpace\');
            v299_NowSetting_Memo.Lines.SaveToFile('.\R2MS_Lite_WorkSpace\setting.txt');
          end
          else if ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[1] = 'Custom01' then
          begin
            RunningLog_Memo.Lines.Add('「.\Configs\v299_Custom01CurrentMode\Current Mode.csv」，開始複製...');
            CopyFile(PChar('.\Configs\v299_Custom01CurrentMode\Current Mode.csv'), PChar('.\R2MS_Lite_WorkSpace\Current Mode.csv'), False); // false=覆蓋,true=略過
            RunningLog_Memo.Lines.Add('「.\Configs\v299_Custom01CurrentMode\Current Mode.csv」，開始複製...結束!');
            v299_DefaultSetting_Memo.Lines.LoadFromFile('.\Configs\v299_Custom01Setting\setting.txt');
            //--
            v299_NowSetting_Memo.Lines.Assign(v299_DefaultSetting_Memo.Lines);
            v299_NowSetting_Memo.Lines.Strings[1] := 'DMM VISA = "' + DMM_Edit.Text + '"';
            v299_NowSetting_Memo.Lines.Strings[2] := 'ESP32 VISA = "' + ESP32_Edit.Text + '"';
            v299_NowSetting_Memo.Lines.Strings[7] := 'Data Directory = "' + ERT_ver2_9_9_Client_SavePath_Edit.Text + '"';
            v299_NowSetting_Memo.Lines.Strings[12] := 'PSU VISA = "' + PSU_Edit.Text + '"';
            v299_NowSetting_Memo.Lines.Strings[21] := 'Output Max Voltage = ' + StringReplace(ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[2], 'V', '', []);
            v299_NowSetting_Memo.Lines.Strings[22] := 'Output Max Current = ' + StringReplace(ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[3], 'A', '', []);
            v299_NowSetting_Memo.Lines.Strings[31] := 'Scan Start Timer = "' + FormatDateTime('yyyy_mm_dd_HH_MM', IncMinute(temp_NextJob_TDateTime, 1)) + ',"';
            v299_NowSetting_Memo.Lines.Strings[32] := 'Leave Timer = "' + FormatDateTime('yyyy_mm_dd_HH_MM', IncMinute(temp_NextJob_TDateTime, Ceil(StrToFloat(ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[7])*1.15)+1 )) + '"';
            ForceDirectories('.\R2MS_Lite_WorkSpace\');
            v299_NowSetting_Memo.Lines.SaveToFile('.\R2MS_Lite_WorkSpace\setting.txt');
          end
          else if ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[1] = 'Custom02' then
          begin
            RunningLog_Memo.Lines.Add('「.\Configs\v299_Custom02CurrentMode\Current Mode.csv」，開始複製...');
            CopyFile(PChar('.\Configs\v299_Custom02CurrentMode\Current Mode.csv'), PChar('.\R2MS_Lite_WorkSpace\Current Mode.csv'), False); // false=覆蓋,true=略過
            RunningLog_Memo.Lines.Add('「.\Configs\v299_Custom02CurrentMode\Current Mode.csv」，開始複製...結束!');
            v299_DefaultSetting_Memo.Lines.LoadFromFile('.\Configs\v299_Custom02Setting\setting.txt');
            //--
            v299_NowSetting_Memo.Lines.Assign(v299_DefaultSetting_Memo.Lines);
            v299_NowSetting_Memo.Lines.Strings[1] := 'DMM VISA = "' + DMM_Edit.Text + '"';
            v299_NowSetting_Memo.Lines.Strings[2] := 'ESP32 VISA = "' + ESP32_Edit.Text + '"';
            v299_NowSetting_Memo.Lines.Strings[7] := 'Data Directory = "' + ERT_ver2_9_9_Client_SavePath_Edit.Text + '"';
            v299_NowSetting_Memo.Lines.Strings[12] := 'PSU VISA = "' + PSU_Edit.Text + '"';
            v299_NowSetting_Memo.Lines.Strings[21] := 'Output Max Voltage = ' + StringReplace(ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[2], 'V', '', []);
            v299_NowSetting_Memo.Lines.Strings[22] := 'Output Max Current = ' + StringReplace(ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[3], 'A', '', []);
            v299_NowSetting_Memo.Lines.Strings[31] := 'Scan Start Timer = "' + FormatDateTime('yyyy_mm_dd_HH_MM', IncMinute(temp_NextJob_TDateTime, 1)) + ',"';
            v299_NowSetting_Memo.Lines.Strings[32] := 'Leave Timer = "' + FormatDateTime('yyyy_mm_dd_HH_MM', IncMinute(temp_NextJob_TDateTime, Ceil(StrToFloat(ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[7])*1.15)+1 )) + '"';
            ForceDirectories('.\R2MS_Lite_WorkSpace\');
            v299_NowSetting_Memo.Lines.SaveToFile('.\R2MS_Lite_WorkSpace\setting.txt');
          end
          else if ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[1] = 'Custom03' then
          begin
            RunningLog_Memo.Lines.Add('「.\Configs\v299_Custom03CurrentMode\Current Mode.csv」，開始複製...');
            CopyFile(PChar('.\Configs\v299_Custom03CurrentMode\Current Mode.csv'), PChar('.\R2MS_Lite_WorkSpace\Current Mode.csv'), False); // false=覆蓋,true=略過
            RunningLog_Memo.Lines.Add('「.\Configs\v299_Custom03CurrentMode\Current Mode.csv」，開始複製...結束!');
            v299_DefaultSetting_Memo.Lines.LoadFromFile('.\Configs\v299_Custom03Setting\setting.txt');
            //--
            v299_NowSetting_Memo.Lines.Assign(v299_DefaultSetting_Memo.Lines);
            v299_NowSetting_Memo.Lines.Strings[1] := 'DMM VISA = "' + DMM_Edit.Text + '"';
            v299_NowSetting_Memo.Lines.Strings[2] := 'ESP32 VISA = "' + ESP32_Edit.Text + '"';
            v299_NowSetting_Memo.Lines.Strings[7] := 'Data Directory = "' + ERT_ver2_9_9_Client_SavePath_Edit.Text + '"';
            v299_NowSetting_Memo.Lines.Strings[12] := 'PSU VISA = "' + PSU_Edit.Text + '"';
            v299_NowSetting_Memo.Lines.Strings[21] := 'Output Max Voltage = ' + StringReplace(ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[2], 'V', '', []);
            v299_NowSetting_Memo.Lines.Strings[22] := 'Output Max Current = ' + StringReplace(ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[3], 'A', '', []);
            v299_NowSetting_Memo.Lines.Strings[31] := 'Scan Start Timer = "' + FormatDateTime('yyyy_mm_dd_HH_MM', IncMinute(temp_NextJob_TDateTime, 1)) + ',"';
            v299_NowSetting_Memo.Lines.Strings[32] := 'Leave Timer = "' + FormatDateTime('yyyy_mm_dd_HH_MM', IncMinute(temp_NextJob_TDateTime, Ceil(StrToFloat(ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[7])*1.15)+1 )) + '"';
            ForceDirectories('.\R2MS_Lite_WorkSpace\');
            v299_NowSetting_Memo.Lines.SaveToFile('.\R2MS_Lite_WorkSpace\setting.txt');
          end
          else //萬一真的沒辦法就用系統檢測的檔案
          begin
            RunningLog_Memo.Lines.Add('「.\Configs\v299_SystemTestCurrentMode\Current Mode.csv」，開始複製...');
            CopyFile(PChar('.\Configs\v299_SystemTestCurrentMode\Current Mode.csv'), PChar('.\R2MS_Lite_WorkSpace\Current Mode.csv'), False); // false=覆蓋,true=略過
            RunningLog_Memo.Lines.Add('「.\Configs\v299_SystemTestCurrentMode\Current Mode.csv」，開始複製...結束!');
            v299_DefaultSetting_Memo.Lines.LoadFromFile('.\Configs\v299_SystemTestSetting\setting.txt');
            //--
            v299_NowSetting_Memo.Lines.Assign(v299_DefaultSetting_Memo.Lines);
            v299_NowSetting_Memo.Lines.Strings[1] := 'DMM VISA = "' + DMM_Edit.Text + '"';
            v299_NowSetting_Memo.Lines.Strings[2] := 'ESP32 VISA = "' + ESP32_Edit.Text + '"';
            v299_NowSetting_Memo.Lines.Strings[7] := 'Data Directory = "' + ERT_ver2_9_9_Client_SavePath_Edit.Text + '"';
            v299_NowSetting_Memo.Lines.Strings[12] := 'PSU VISA = "' + PSU_Edit.Text + '"';
            v299_NowSetting_Memo.Lines.Strings[31] := 'Scan Start Timer = "' + FormatDateTime('yyyy_mm_dd_HH_MM', IncMinute(temp_NextJob_TDateTime, 1)) + ',"';
            v299_NowSetting_Memo.Lines.Strings[32] := 'Leave Timer = "' + FormatDateTime('yyyy_mm_dd_HH_MM', IncMinute(temp_NextJob_TDateTime, 5)) + '"';
            ForceDirectories('.\R2MS_Lite_WorkSpace\');
            v299_NowSetting_Memo.Lines.SaveToFile('.\R2MS_Lite_WorkSpace\setting.txt');
          end;
        end
        else if StrToIntDef(ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[4], 0) <= 0 then
        begin
          if Length(ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[5]) = 0 then
          begin
            ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[4] := '略過';
          end
          else
          begin
            ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[4] := '完成';
          end;
        end
        else
        begin
          ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[4] := '待機';
        end;
      end;
    end;
    //--------------------------------------------------------------------------
    //--------------------------------------------------------------------------
    // 調整為狀態2
    ERT_ver2_9_9_Client_Schedule_flag := 2; //0=不要做事,1=查找下個即將,2=針對即將進行工作,3=針對初始化進行工作,4=針對等待運作進行工作
    StatusBar1.Panels[1].Text := '排程狀態=' +IntToStr(ERT_ver2_9_9_Client_Schedule_flag);
    //--------------------------------------------------------------------------
  end
  //--------------------------------------------------------------------------
  // 狀態 2: 針對即將進行工作
  // ---
  else if ERT_ver2_9_9_Client_Schedule_flag = 2 then //0=不要做事,1=查找下個即將,2=針對即將進行工作,3=針對初始化進行工作,4=針對等待運作進行工作
  begin
    for temp_i := 0 to ScheduleSetting_ListView.Items.Count - 1 do
    begin
      if ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[4] = '即將' then
      begin
        temp_NextJob_TDateTime := StrToDateTime(ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[6]);
        if CompareDateTime(temp_now_TDateTime, temp_NextJob_TDateTime) = -1 then
        begin
          //RunningLog_Memo.Lines.Add(FormatDateTime('yyyy/mm/dd HH:MM:SS', temp_now_TDateTime) + ' < ' + FormatDateTime('yyyy/mm/dd HH:MM:SS', temp_NextJob_TDateTime));
          //RunningLog_Memo.Lines.Add('排程時間在未來:');
          temp_SecondsBetween := SecondsBetween(temp_now_TDateTime, temp_NextJob_TDateTime);
          //RunningLog_Memo.Lines.Add('還相差: ' + IntToStr(temp_SecondsBetween) + ' 秒');
        end
        else
        begin
          //RunningLog_Memo.Lines.Add('排程時間在過去:');
          temp_SecondsBetween := -1 * SecondsBetween(temp_now_TDateTime, temp_NextJob_TDateTime);
          //RunningLog_Memo.Lines.Add('還相差: ' + IntToStr(temp_SecondsBetween) + ' 秒');
        end;
        ScheduleRun_Button.Caption := '倒數' + IntToStr(temp_SecondsBetween) + '秒';
        if temp_SecondsBetween < 0 then
        begin
          if FileExists('.\R2MS_Lite_WorkSpace\setting.txt') then
          begin
            RunningLog_Memo.Lines.Add('「.\R2MS_Lite_WorkSpace\setting.txt」存在，開始呼叫執行檔...');
            //--------------------------------------------------------------------------
            // 調整為狀態3
            ERT_ver2_9_9_Client_Schedule_flag := 3; //0=不要做事,1=查找下個即將,2=針對即將進行工作,3=針對初始化進行工作,4=針對等待運作進行工作
            StatusBar1.Panels[1].Text := '排程狀態=' +IntToStr(ERT_ver2_9_9_Client_Schedule_flag);
            //--------------------------------------------------------------------------
            ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[4] := '初始化';
            ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[6] := FormatDateTime('yyyy/mm/dd HH:MM', temp_NextJob_TDateTime) + ':46';
            ShellExecute(0, 'open', '.\R2MS_Lite_WorkSpace\ERT_ver2_9_9_Client.exe', nil, nil, SW_SHOWNORMAL);
            RunningLog_Memo.Lines.Add('「.\R2MS_Lite_WorkSpace\setting.txt」存在，開始呼叫執行檔...結束');
          end
          else
          begin
            RunningLog_Memo.Lines.Add('「.\R2MS_Lite_WorkSpace\setting.txt」不存在!還在存檔嗎?');
          end;
        end;
      end;
    end;
  end
  //--------------------------------------------------------------------------
  //--------------------------------------------------------------------------
  // 狀態 3: 針對初始化進行工作
  else if ERT_ver2_9_9_Client_Schedule_flag = 3 then //0=不要做事,1=查找下個即將,2=針對即將進行工作,3=針對初始化進行工作,4=針對等待運作進行工作
  begin
    for temp_i := 0 to ScheduleSetting_ListView.Items.Count - 1 do
    begin
      if ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[4] = '初始化' then
      begin
        temp_NextJob_TDateTime := StrToDateTime(ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[6]);
        if CompareDateTime(temp_now_TDateTime, temp_NextJob_TDateTime) = -1 then
        begin
          //RunningLog_Memo.Lines.Add(FormatDateTime('yyyy/mm/dd HH:MM:SS', temp_now_TDateTime) + ' < ' + FormatDateTime('yyyy/mm/dd HH:MM:SS', temp_NextJob_TDateTime));
          //RunningLog_Memo.Lines.Add('排程時間在未來:');
          temp_SecondsBetween := SecondsBetween(temp_now_TDateTime, temp_NextJob_TDateTime);
          //RunningLog_Memo.Lines.Add('還相差: ' + IntToStr(temp_SecondsBetween) + ' 秒');
        end
        else
        begin
          //RunningLog_Memo.Lines.Add('排程時間在過去:');
          temp_SecondsBetween := -1 * SecondsBetween(temp_now_TDateTime, temp_NextJob_TDateTime);
          //RunningLog_Memo.Lines.Add('還相差: ' + IntToStr(temp_SecondsBetween) + ' 秒');
        end;
        ScheduleRun_Button.Caption := '倒數' + IntToStr(temp_SecondsBetween) + '秒';
        if temp_SecondsBetween < 0 then
        begin
          if FileExists('.\R2MS_Lite_WorkSpace\ERT_ver2_9_9_Client_Data\' + FormatDateTime('yyyy_mm_dd_HH_MM', temp_NextJob_TDateTime) + '_Connected.log') then
          begin
            RunningLog_Memo.Lines.Add('「.\R2MS_Lite_WorkSpace\ERT_ver2_9_9_Client_Data\' + FormatDateTime('yyyy_mm_dd_HH_MM', temp_NextJob_TDateTime) + '_Connected.log」存在，表示初始化成功...可進行下個步驟');
            //--------------------------------------------------------------------------
            // 調整為狀態4
            ERT_ver2_9_9_Client_Schedule_flag := 4; //0=不要做事,1=查找下個即將,2=針對即將進行工作,3=針對初始化進行工作,4=針對等待運作進行工作
            StatusBar1.Panels[1].Text := '排程狀態=' +IntToStr(ERT_ver2_9_9_Client_Schedule_flag);
            //--------------------------------------------------------------------------
            ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[4] := '等待運作';
            if ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[1] = '系統檢測' then
            begin
              ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[6] := FormatDateTime('yyyy/mm/dd HH:MM', IncMinute(temp_NextJob_TDateTime, Ceil(StrToFloat(ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[7])*1.15)+1  )) + ':10';
            end
            else
            begin
              ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[6] := FormatDateTime('yyyy/mm/dd HH:MM', IncMinute(temp_NextJob_TDateTime, Ceil(StrToFloat(ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[7])*1.15)+1  )) + ':10';
            end;
          end
          else if FileExists('.\R2MS_Lite_WorkSpace\ERT_ver2_9_9_Client_Data\' + FormatDateTime('yyyy_mm_dd_HH_MM', temp_NextJob_TDateTime) + '_Failed.log') then
          begin
            RunningLog_Memo.Lines.Add('「.\R2MS_Lite_WorkSpace\ERT_ver2_9_9_Client_Data\' + FormatDateTime('yyyy_mm_dd_HH_MM', temp_NextJob_TDateTime) + '_Failed.log」存在，表示初始化失敗...其他工作都不能執行');
            //--------------------------------------------------------------------------
            // 調整為狀態0
            ERT_ver2_9_9_Client_Schedule_flag := 0; //0=不要做事,1=查找下個即將,2=針對即將進行工作,3=針對初始化進行工作,4=針對等待運作進行工作
            StatusBar1.Panels[1].Text := '排程狀態=' +IntToStr(ERT_ver2_9_9_Client_Schedule_flag);
            //--------------------------------------------------------------------------
            ScheduleRun_Button.Caption := '初始化失敗';
            ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[4] := '初始化失敗';
            Application.MessageBox('提示:' + #13#10 + '初始化失敗!停止排程工作!', '提示', MB_OK + MB_ICONINFORMATION);
            Exit;
          end
          else
          begin
            RunningLog_Memo.Lines.Add('「.\R2MS_Lite_WorkSpace\ERT_ver2_9_9_Client_Data\' + FormatDateTime('yyyy_mm_dd_HH_MM', temp_NextJob_TDateTime) + '_*.log」不存在，表示初始化失敗...其他工作都不能執行');
            //--------------------------------------------------------------------------
            // 調整為狀態0
            ERT_ver2_9_9_Client_Schedule_flag := 0; //0=不要做事,1=查找下個即將,2=針對即將進行工作,3=針對初始化進行工作,4=針對等待運作進行工作
            StatusBar1.Panels[1].Text := '排程狀態=' +IntToStr(ERT_ver2_9_9_Client_Schedule_flag);
            //--------------------------------------------------------------------------
            ScheduleRun_Button.Caption := '初始化失敗';
            ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[4] := '初始化失敗';
            Application.MessageBox('提示:' + #13#10 + '初始化失敗!沒有正確安裝LabVIEW嗎?停止排程工作!', '提示', MB_OK + MB_ICONINFORMATION);
            Exit;
          end;
        end;
      end;
    end;
  end
  //--------------------------------------------------------------------------
  //--------------------------------------------------------------------------
  // 狀態 4: 針對等待運作進行工作
  else if ERT_ver2_9_9_Client_Schedule_flag = 4 then //0=不要做事,1=查找下個即將,2=針對即將進行工作,3=針對初始化進行工作,4=針對等待運作進行工作
  begin
    for temp_i := 0 to ScheduleSetting_ListView.Items.Count - 1 do
    begin
      if ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[4] = '等待運作' then
      begin
        temp_NextJob_TDateTime := StrToDateTime(ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[6]);
        if CompareDateTime(temp_now_TDateTime, temp_NextJob_TDateTime) = -1 then
        begin
          //RunningLog_Memo.Lines.Add(FormatDateTime('yyyy/mm/dd HH:MM:SS', temp_now_TDateTime) + ' < ' + FormatDateTime('yyyy/mm/dd HH:MM:SS', temp_NextJob_TDateTime));
          //RunningLog_Memo.Lines.Add('排程時間在未來:');
          temp_SecondsBetween := SecondsBetween(temp_now_TDateTime, temp_NextJob_TDateTime);
          //RunningLog_Memo.Lines.Add('還相差: ' + IntToStr(temp_SecondsBetween) + ' 秒');
        end
        else
        begin
          //RunningLog_Memo.Lines.Add('排程時間在過去:');
          temp_SecondsBetween := -1 * SecondsBetween(temp_now_TDateTime, temp_NextJob_TDateTime);
          //RunningLog_Memo.Lines.Add('還相差: ' + IntToStr(temp_SecondsBetween) + ' 秒');
        end;
        ScheduleRun_Button.Caption := '倒數' + IntToStr(temp_SecondsBetween) + '秒';

        if temp_SecondsBetween < 0 then
        begin
          // Pascal 中的 FindWindow 需要使用 PChar 或 nil
          if FindWindow(nil, 'ERT_ver2_9_9_Client.vi') <> 0 then
          begin
            RunningLog_Memo.Lines.Add('倒數' + IntToStr(temp_SecondsBetween) + '秒...視窗軟體「ERT_ver2_9_9_Client.vi」存在!');
            ERT_ver2_9_9_Client_Schedule_flag := 0; //0=不要做事,1=查找下個即將,2=針對即將進行工作,3=針對初始化進行工作,4=針對等待運作進行工作
            ScheduleRun_Button.Caption := '運作失敗';
            ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[4] := '運作失敗';
            Application.MessageBox('提示:' + #13#10 + '運作失敗!視窗軟體「ERT_ver2_9_9_Client.vi」沒有自動關閉!停止排程工作!', '提示', MB_OK + MB_ICONINFORMATION);
            Exit;
          end
          else
          begin
            RunningLog_Memo.Lines.Add('倒數' + IntToStr(temp_SecondsBetween) + '秒...視窗軟體「ERT_ver2_9_9_Client.vi」不存在!');
          end;
          if FileExists('.\R2MS_Lite_WorkSpace\ERT_ver2_9_9_Client_Data\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(21-1, 16) + '.csv') then
          begin
            RunningLog_Memo.Lines.Add('「.\R2MS_Lite_WorkSpace\ERT_ver2_9_9_Client_Data\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(21-1, 16) + '.csv」存在，表示運作完成...可進行下個步驟');

            ForceDirectories('.\Local\' + ERT_Project_Name_Edit.Text + '\Recorder\' + ERT_Profile_Name_Edit.Text +
              '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(21-1, 4) +
              '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(26-1, 2) +
              '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(29-1, 2) +
              '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(32-1, 2) + v299_NowSetting_Memo.Lines.Strings[31].Substring(35-1, 2) +
              '\Part01\1');

            temp_TStringList := TStringList.Create;
            temp_Zipper := TZipper.Create;
            try
              temp_TStringList.LoadFromFile('.\R2MS_Lite_WorkSpace\ERT_ver2_9_9_Client_Data\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(21-1, 16) + '.csv');
              temp_TStringList.Insert(0, 'Time[ms](R2MS_Lite_v299csv_v20240510a),Tx_Voltage[V],Rx_Current[A],Mode_Index,Electrode_Status(v/+/-),CH01_Voltage[V],CH01_Current[A],CH02_Voltage[V],CH02_Current[A],CH03_Voltage[V],CH03_Current[A],CH04_Voltage[V],CH04_Current[A],CH05_Voltage[V],CH05_Current[A],CH06_Voltage[V],CH06_Current[A],CH07_Voltage[V],CH07_Current[A],CH08_Voltage[V],CH08_Current[A],CH09_Voltage[V],CH09_Current[A],CH10_Voltage[V],CH10_Current[A],CH11_Voltage[V],CH11_Current[A],CH12_Voltage[V],CH12_Current[A],CH13_Voltage[V],CH13_Current[A],CH14_Voltage[V],CH14_Current[A],CH15_Voltage[V],CH15_Current[A],CH16_Voltage[V],CH16_Current[A],CH17_Voltage[V],CH17_Current[A],CH18_Voltage[V],CH18_Current[A],CH19_Voltage[V],CH19_Current[A],CH20_Voltage[V],CH20_Current[A],CH21_Voltage[V],CH21_Current[A],CH22_Voltage[V],CH22_Current[A],CH23_Voltage[V],CH23_Current[A],CH24_Voltage[V],CH24_Current[A],CH25_Voltage[V],CH25_Current[A],CH26_Voltage[V],CH26_Current[A],CH27_Voltage[V],CH27_Current[A],CH28_Voltage[V],CH28_Current[A],CH29_Voltage[V],CH29_Current[A],CH30_Voltage[V],CH30_Current[A],CH31_Voltage[V],CH31_Current[A],CH32_Voltage[V],CH32_Current[A],CH33_Voltage[V],CH33_Current[A],CH34_Voltage[V],CH34_Current[A],CH35_Voltage[V],CH35_Current[A],CH36_Voltage[V],CH36_Current[A],CH37_Voltage[V],CH37_Current[A],CH38_Voltage[V],CH38_Current[A],CH39_Voltage[V],CH39_Current[A],CH40_Voltage[V],CH40_Current[A],CH41_Voltage[V],CH41_Current[A],CH42_Voltage[V],CH42_Current[A],CH43_Voltage[V],CH43_Current[A],CH44_Voltage[V],CH44_Current[A],CH45_Voltage[V],CH45_Current[A],CH46_Voltage[V],CH46_Current[A],CH47_Voltage[V],CH47_Current[A],CH48_Voltage[V],CH48_Current[A],CH49_Voltage[V],CH49_Current[A],CH50_Voltage[V],CH50_Current[A],CH51_Voltage[V],CH51_Current[A],CH52_Voltage[V],CH52_Current[A],CH53_Voltage[V],CH53_Current[A],CH54_Voltage[V],CH54_Current[A],CH55_Voltage[V],CH55_Current[A],CH56_Voltage[V],CH56_Current[A],CH57_Voltage[V],CH57_Current[A],CH58_Voltage[V],CH58_Current[A],CH59_Voltage[V],CH59_Current[A],CH60_Voltage[V],CH60_Current[A],CH61_Voltage[V],CH61_Current[A],CH62_Voltage[V],CH62_Current[A],CH63_Voltage[V],CH63_Current[A],CH64_Voltage[V],CH64_Current[A]');
              if ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[1] = '系統檢測' then
              begin
                RunningLog_Memo.Lines.Add('整理「系統檢測」為.v299T.csv');
                temp_TStringList.SaveToFile('.\Local\' + ERT_Project_Name_Edit.Text + '\Recorder\' + ERT_Profile_Name_Edit.Text +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(21-1, 4) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(26-1, 2) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(29-1, 2) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(32-1, 2) + v299_NowSetting_Memo.Lines.Strings[31].Substring(35-1, 2) +
                  '\Part01\1\' + ESP32_SN_Edit.Text +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(21-1, 4) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(26-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(29-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(32-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(35-1, 2) +
                  '.v299T.csv');
                // 設定要建立的 ZIP 檔名
                temp_Zipper.FileName := '.\Local\' + ERT_Project_Name_Edit.Text + '\Recorder\' + ERT_Profile_Name_Edit.Text +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(21-1, 4) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(26-1, 2) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(29-1, 2) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(32-1, 2) + v299_NowSetting_Memo.Lines.Strings[31].Substring(35-1, 2) +
                  '\Part01\1\' + ESP32_SN_Edit.Text +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(21-1, 4) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(26-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(29-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(32-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(35-1, 2) +
                  '.v299T.csv.zip';
                RunningLog_Memo.Lines.Add('新壓縮檔檔名:' + temp_Zipper.FileName);
                // 將 目標檔案 加入壓縮檔中
                temp_Zipper.Entries.AddFileEntry(
                  ('.\Local\' + ERT_Project_Name_Edit.Text + '\Recorder\' + ERT_Profile_Name_Edit.Text +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(21-1, 4) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(26-1, 2) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(29-1, 2) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(32-1, 2) + v299_NowSetting_Memo.Lines.Strings[31].Substring(35-1, 2) +
                  '\Part01\1\' + ESP32_SN_Edit.Text +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(21-1, 4) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(26-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(29-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(32-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(35-1, 2) +
                  '.v299T.csv'),
                  (ESP32_SN_Edit.Text +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(21-1, 4) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(26-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(29-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(32-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(35-1, 2) +
                  '.v299T.csv')
                  );
                // 執行壓縮並寫入檔案
                RunningLog_Memo.Lines.Add('執行壓縮並寫入檔案...');
                temp_Zipper.ZipAllFiles;
                RunningLog_Memo.Lines.Add('執行壓縮並寫入檔案...完成!');
                // 刪除壓縮前的檔案
                RunningLog_Memo.Lines.Add('刪除壓縮前的檔案...');
                DeleteFile(PChar('.\Local\' + ERT_Project_Name_Edit.Text + '\Recorder\' + ERT_Profile_Name_Edit.Text +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(21-1, 4) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(26-1, 2) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(29-1, 2) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(32-1, 2) + v299_NowSetting_Memo.Lines.Strings[31].Substring(35-1, 2) +
                  '\Part01\1\' + ESP32_SN_Edit.Text +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(21-1, 4) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(26-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(29-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(32-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(35-1, 2) +
                  '.v299T.csv'));
                RunningLog_Memo.Lines.Add('刪除壓縮前的檔案...完成!');
              end
              else if ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[1] = 'M64' then
              begin
                RunningLog_Memo.Lines.Add('整理「M64」為.v299M.csv');
                temp_TStringList.SaveToFile('.\Local\' + ERT_Project_Name_Edit.Text + '\Recorder\' + ERT_Profile_Name_Edit.Text +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(21-1, 4) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(26-1, 2) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(29-1, 2) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(32-1, 2) + v299_NowSetting_Memo.Lines.Strings[31].Substring(35-1, 2) +
                  '\Part01\1\' + ESP32_SN_Edit.Text +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(21-1, 4) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(26-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(29-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(32-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(35-1, 2) +
                  '.v299M.csv');
                // 設定要建立的 ZIP 檔名
                temp_Zipper.FileName := '.\Local\' + ERT_Project_Name_Edit.Text + '\Recorder\' + ERT_Profile_Name_Edit.Text +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(21-1, 4) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(26-1, 2) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(29-1, 2) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(32-1, 2) + v299_NowSetting_Memo.Lines.Strings[31].Substring(35-1, 2) +
                  '\Part01\1\' + ESP32_SN_Edit.Text +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(21-1, 4) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(26-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(29-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(32-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(35-1, 2) +
                  '.v299M.csv.zip';
                RunningLog_Memo.Lines.Add('新壓縮檔檔名:' + temp_Zipper.FileName);
                // 將 目標檔案 加入壓縮檔中
                temp_Zipper.Entries.AddFileEntry(
                  ('.\Local\' + ERT_Project_Name_Edit.Text + '\Recorder\' + ERT_Profile_Name_Edit.Text +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(21-1, 4) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(26-1, 2) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(29-1, 2) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(32-1, 2) + v299_NowSetting_Memo.Lines.Strings[31].Substring(35-1, 2) +
                  '\Part01\1\' + ESP32_SN_Edit.Text +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(21-1, 4) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(26-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(29-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(32-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(35-1, 2) +
                  '.v299M.csv'),
                  (ESP32_SN_Edit.Text +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(21-1, 4) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(26-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(29-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(32-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(35-1, 2) +
                  '.v299M.csv')
                  );
                // 執行壓縮並寫入檔案
                RunningLog_Memo.Lines.Add('執行壓縮並寫入檔案...');
                temp_Zipper.ZipAllFiles;
                RunningLog_Memo.Lines.Add('執行壓縮並寫入檔案...完成!');
                // 刪除壓縮前的檔案
                RunningLog_Memo.Lines.Add('刪除壓縮前的檔案...');
                DeleteFile(PChar('.\Local\' + ERT_Project_Name_Edit.Text + '\Recorder\' + ERT_Profile_Name_Edit.Text +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(21-1, 4) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(26-1, 2) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(29-1, 2) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(32-1, 2) + v299_NowSetting_Memo.Lines.Strings[31].Substring(35-1, 2) +
                  '\Part01\1\' + ESP32_SN_Edit.Text +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(21-1, 4) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(26-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(29-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(32-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(35-1, 2) +
                  '.v299M.csv'));
                RunningLog_Memo.Lines.Add('刪除壓縮前的檔案...完成!');
              end
              else if ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[1] = 'S64N' then
              begin
                RunningLog_Memo.Lines.Add('整理「S64N」為.v299S.csv');
                temp_TStringList.SaveToFile('.\Local\' + ERT_Project_Name_Edit.Text + '\Recorder\' + ERT_Profile_Name_Edit.Text +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(21-1, 4) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(26-1, 2) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(29-1, 2) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(32-1, 2) + v299_NowSetting_Memo.Lines.Strings[31].Substring(35-1, 2) +
                  '\Part01\1\' + ESP32_SN_Edit.Text +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(21-1, 4) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(26-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(29-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(32-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(35-1, 2) +
                  '.v299S.csv');
                // 設定要建立的 ZIP 檔名
                temp_Zipper.FileName := '.\Local\' + ERT_Project_Name_Edit.Text + '\Recorder\' + ERT_Profile_Name_Edit.Text +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(21-1, 4) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(26-1, 2) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(29-1, 2) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(32-1, 2) + v299_NowSetting_Memo.Lines.Strings[31].Substring(35-1, 2) +
                  '\Part01\1\' + ESP32_SN_Edit.Text +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(21-1, 4) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(26-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(29-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(32-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(35-1, 2) +
                  '.v299S.csv.zip';
                RunningLog_Memo.Lines.Add('新壓縮檔檔名:' + temp_Zipper.FileName);
                // 將 目標檔案 加入壓縮檔中
                temp_Zipper.Entries.AddFileEntry(
                  ('.\Local\' + ERT_Project_Name_Edit.Text + '\Recorder\' + ERT_Profile_Name_Edit.Text +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(21-1, 4) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(26-1, 2) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(29-1, 2) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(32-1, 2) + v299_NowSetting_Memo.Lines.Strings[31].Substring(35-1, 2) +
                  '\Part01\1\' + ESP32_SN_Edit.Text +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(21-1, 4) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(26-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(29-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(32-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(35-1, 2) +
                  '.v299S.csv'),
                  (ESP32_SN_Edit.Text +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(21-1, 4) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(26-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(29-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(32-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(35-1, 2) +
                  '.v299S.csv')
                  );
                // 執行壓縮並寫入檔案
                RunningLog_Memo.Lines.Add('執行壓縮並寫入檔案...');
                temp_Zipper.ZipAllFiles;
                RunningLog_Memo.Lines.Add('執行壓縮並寫入檔案...完成!');
                // 刪除壓縮前的檔案
                RunningLog_Memo.Lines.Add('刪除壓縮前的檔案...');
                DeleteFile(PChar('.\Local\' + ERT_Project_Name_Edit.Text + '\Recorder\' + ERT_Profile_Name_Edit.Text +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(21-1, 4) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(26-1, 2) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(29-1, 2) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(32-1, 2) + v299_NowSetting_Memo.Lines.Strings[31].Substring(35-1, 2) +
                  '\Part01\1\' + ESP32_SN_Edit.Text +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(21-1, 4) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(26-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(29-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(32-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(35-1, 2) +
                  '.v299S.csv'));
                RunningLog_Memo.Lines.Add('刪除壓縮前的檔案...完成!');
              end
              else if ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[1] = 'S64HS' then
              begin
                RunningLog_Memo.Lines.Add('整理「S64HS」為.v299S.csv');
                temp_TStringList.SaveToFile('.\Local\' + ERT_Project_Name_Edit.Text + '\Recorder\' + ERT_Profile_Name_Edit.Text +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(21-1, 4) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(26-1, 2) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(29-1, 2) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(32-1, 2) + v299_NowSetting_Memo.Lines.Strings[31].Substring(35-1, 2) +
                  '\Part01\1\' + ESP32_SN_Edit.Text +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(21-1, 4) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(26-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(29-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(32-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(35-1, 2) +
                  '.v299S.csv');
                // 設定要建立的 ZIP 檔名
                temp_Zipper.FileName := '.\Local\' + ERT_Project_Name_Edit.Text + '\Recorder\' + ERT_Profile_Name_Edit.Text +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(21-1, 4) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(26-1, 2) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(29-1, 2) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(32-1, 2) + v299_NowSetting_Memo.Lines.Strings[31].Substring(35-1, 2) +
                  '\Part01\1\' + ESP32_SN_Edit.Text +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(21-1, 4) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(26-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(29-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(32-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(35-1, 2) +
                  '.v299S.csv.zip';
                RunningLog_Memo.Lines.Add('新壓縮檔檔名:' + temp_Zipper.FileName);
                // 將 目標檔案 加入壓縮檔中
                temp_Zipper.Entries.AddFileEntry(
                  ('.\Local\' + ERT_Project_Name_Edit.Text + '\Recorder\' + ERT_Profile_Name_Edit.Text +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(21-1, 4) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(26-1, 2) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(29-1, 2) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(32-1, 2) + v299_NowSetting_Memo.Lines.Strings[31].Substring(35-1, 2) +
                  '\Part01\1\' + ESP32_SN_Edit.Text +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(21-1, 4) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(26-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(29-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(32-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(35-1, 2) +
                  '.v299S.csv'),
                  (ESP32_SN_Edit.Text +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(21-1, 4) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(26-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(29-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(32-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(35-1, 2) +
                  '.v299S.csv')
                  );
                // 執行壓縮並寫入檔案
                RunningLog_Memo.Lines.Add('執行壓縮並寫入檔案...');
                temp_Zipper.ZipAllFiles;
                RunningLog_Memo.Lines.Add('執行壓縮並寫入檔案...完成!');
                // 刪除壓縮前的檔案
                RunningLog_Memo.Lines.Add('刪除壓縮前的檔案...');
                DeleteFile(PChar('.\Local\' + ERT_Project_Name_Edit.Text + '\Recorder\' + ERT_Profile_Name_Edit.Text +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(21-1, 4) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(26-1, 2) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(29-1, 2) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(32-1, 2) + v299_NowSetting_Memo.Lines.Strings[31].Substring(35-1, 2) +
                  '\Part01\1\' + ESP32_SN_Edit.Text +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(21-1, 4) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(26-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(29-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(32-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(35-1, 2) +
                  '.v299S.csv'));
                RunningLog_Memo.Lines.Add('刪除壓縮前的檔案...完成!');
              end
              else if ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[1] = 'S64VHS' then
              begin
                RunningLog_Memo.Lines.Add('整理「S64VHS」為.v299S.csv');
                temp_TStringList.SaveToFile('.\Local\' + ERT_Project_Name_Edit.Text + '\Recorder\' + ERT_Profile_Name_Edit.Text +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(21-1, 4) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(26-1, 2) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(29-1, 2) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(32-1, 2) + v299_NowSetting_Memo.Lines.Strings[31].Substring(35-1, 2) +
                  '\Part01\1\' + ESP32_SN_Edit.Text +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(21-1, 4) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(26-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(29-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(32-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(35-1, 2) +
                  '.v299S.csv');
                // 設定要建立的 ZIP 檔名
                temp_Zipper.FileName := '.\Local\' + ERT_Project_Name_Edit.Text + '\Recorder\' + ERT_Profile_Name_Edit.Text +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(21-1, 4) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(26-1, 2) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(29-1, 2) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(32-1, 2) + v299_NowSetting_Memo.Lines.Strings[31].Substring(35-1, 2) +
                  '\Part01\1\' + ESP32_SN_Edit.Text +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(21-1, 4) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(26-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(29-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(32-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(35-1, 2) +
                  '.v299S.csv.zip';
                RunningLog_Memo.Lines.Add('新壓縮檔檔名:' + temp_Zipper.FileName);
                // 將 目標檔案 加入壓縮檔中
                temp_Zipper.Entries.AddFileEntry(
                  ('.\Local\' + ERT_Project_Name_Edit.Text + '\Recorder\' + ERT_Profile_Name_Edit.Text +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(21-1, 4) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(26-1, 2) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(29-1, 2) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(32-1, 2) + v299_NowSetting_Memo.Lines.Strings[31].Substring(35-1, 2) +
                  '\Part01\1\' + ESP32_SN_Edit.Text +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(21-1, 4) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(26-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(29-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(32-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(35-1, 2) +
                  '.v299S.csv'),
                  (ESP32_SN_Edit.Text +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(21-1, 4) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(26-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(29-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(32-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(35-1, 2) +
                  '.v299S.csv')
                  );
                // 執行壓縮並寫入檔案
                RunningLog_Memo.Lines.Add('執行壓縮並寫入檔案...');
                temp_Zipper.ZipAllFiles;
                RunningLog_Memo.Lines.Add('執行壓縮並寫入檔案...完成!');
                // 刪除壓縮前的檔案
                RunningLog_Memo.Lines.Add('刪除壓縮前的檔案...');
                DeleteFile(PChar('.\Local\' + ERT_Project_Name_Edit.Text + '\Recorder\' + ERT_Profile_Name_Edit.Text +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(21-1, 4) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(26-1, 2) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(29-1, 2) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(32-1, 2) + v299_NowSetting_Memo.Lines.Strings[31].Substring(35-1, 2) +
                  '\Part01\1\' + ESP32_SN_Edit.Text +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(21-1, 4) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(26-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(29-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(32-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(35-1, 2) +
                  '.v299S.csv'));
                RunningLog_Memo.Lines.Add('刪除壓縮前的檔案...完成!');
              end
              else
              begin
                RunningLog_Memo.Lines.Add('整理「其他」為.v299.csv');
                temp_TStringList.SaveToFile('.\Local\' + ERT_Project_Name_Edit.Text + '\Recorder\' + ERT_Profile_Name_Edit.Text +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(21-1, 4) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(26-1, 2) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(29-1, 2) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(32-1, 2) + v299_NowSetting_Memo.Lines.Strings[31].Substring(35-1, 2) +
                  '\Part01\1\' + ESP32_SN_Edit.Text +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(21-1, 4) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(26-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(29-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(32-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(35-1, 2) +
                  '.v299.csv');
                // 設定要建立的 ZIP 檔名
                temp_Zipper.FileName := '.\Local\' + ERT_Project_Name_Edit.Text + '\Recorder\' + ERT_Profile_Name_Edit.Text +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(21-1, 4) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(26-1, 2) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(29-1, 2) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(32-1, 2) + v299_NowSetting_Memo.Lines.Strings[31].Substring(35-1, 2) +
                  '\Part01\1\' + ESP32_SN_Edit.Text +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(21-1, 4) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(26-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(29-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(32-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(35-1, 2) +
                  '.v299.csv.zip';
                RunningLog_Memo.Lines.Add('新壓縮檔檔名:' + temp_Zipper.FileName);
                // 將 目標檔案 加入壓縮檔中
                temp_Zipper.Entries.AddFileEntry(
                  ('.\Local\' + ERT_Project_Name_Edit.Text + '\Recorder\' + ERT_Profile_Name_Edit.Text +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(21-1, 4) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(26-1, 2) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(29-1, 2) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(32-1, 2) + v299_NowSetting_Memo.Lines.Strings[31].Substring(35-1, 2) +
                  '\Part01\1\' + ESP32_SN_Edit.Text +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(21-1, 4) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(26-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(29-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(32-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(35-1, 2) +
                  '.v299.csv'),
                  (ESP32_SN_Edit.Text +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(21-1, 4) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(26-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(29-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(32-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(35-1, 2) +
                  '.v299.csv')
                  );
                // 執行壓縮並寫入檔案
                RunningLog_Memo.Lines.Add('執行壓縮並寫入檔案...');
                temp_Zipper.ZipAllFiles;
                RunningLog_Memo.Lines.Add('執行壓縮並寫入檔案...完成!');
                // 刪除壓縮前的檔案
                RunningLog_Memo.Lines.Add('刪除壓縮前的檔案...');
                DeleteFile(PChar('.\Local\' + ERT_Project_Name_Edit.Text + '\Recorder\' + ERT_Profile_Name_Edit.Text +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(21-1, 4) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(26-1, 2) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(29-1, 2) +
                  '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(32-1, 2) + v299_NowSetting_Memo.Lines.Strings[31].Substring(35-1, 2) +
                  '\Part01\1\' + ESP32_SN_Edit.Text +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(21-1, 4) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(26-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(29-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(32-1, 2) +
                  v299_NowSetting_Memo.Lines.Strings[31].Substring(35-1, 2) +
                  '.v299.csv'));
                RunningLog_Memo.Lines.Add('刪除壓縮前的檔案...完成!');
              end;
            finally
              temp_TStringList.Free;
              temp_Zipper.Free;
            end;
            //--
            // project1.ini
            temp_Str := ChangeFileExt(ExtractFileName(Application.ExeName), '.ini');
            temp_Str2 := '.\Local\' + ERT_Project_Name_Edit.Text + '\Recorder\' + ERT_Profile_Name_Edit.Text +
              '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(21-1, 4) +
              '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(26-1, 2) +
              '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(29-1, 2) +
              '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(32-1, 2) + v299_NowSetting_Memo.Lines.Strings[31].Substring(35-1, 2) +
              '\Part01\1\' + ESP32_SN_Edit.Text +
              v299_NowSetting_Memo.Lines.Strings[31].Substring(21-1, 4) +
              v299_NowSetting_Memo.Lines.Strings[31].Substring(26-1, 2) +
              v299_NowSetting_Memo.Lines.Strings[31].Substring(29-1, 2) +
              v299_NowSetting_Memo.Lines.Strings[31].Substring(32-1, 2) +
              v299_NowSetting_Memo.Lines.Strings[31].Substring(35-1, 2) +
              '.' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini');
            CopyFile(PChar(temp_Str), PChar(temp_Str2), False); // false=覆蓋,true=略過
            //--
            // setting.txt
            temp_Str := '.\Local\' + ERT_Project_Name_Edit.Text + '\Recorder\' + ERT_Profile_Name_Edit.Text +
              '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(21-1, 4) +
              '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(26-1, 2) +
              '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(29-1, 2) +
              '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(32-1, 2) + v299_NowSetting_Memo.Lines.Strings[31].Substring(35-1, 2) +
              '\Part01\1\' + ESP32_SN_Edit.Text +
              v299_NowSetting_Memo.Lines.Strings[31].Substring(21-1, 4) +
              v299_NowSetting_Memo.Lines.Strings[31].Substring(26-1, 2) +
              v299_NowSetting_Memo.Lines.Strings[31].Substring(29-1, 2) +
              v299_NowSetting_Memo.Lines.Strings[31].Substring(32-1, 2) +
              v299_NowSetting_Memo.Lines.Strings[31].Substring(35-1, 2) +
              '.setting.txt';
            CopyFile(PChar('.\R2MS_Lite_WorkSpace\setting.txt'), PChar(temp_Str), False); // false=覆蓋,true=略過
            //--
            // Current Mode.csv
            temp_Str := '.\Local\' + ERT_Project_Name_Edit.Text + '\Recorder\' + ERT_Profile_Name_Edit.Text +
              '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(21-1, 4) +
              '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(26-1, 2) +
              '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(29-1, 2) +
              '\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(32-1, 2) + v299_NowSetting_Memo.Lines.Strings[31].Substring(35-1, 2) +
              '\Part01\1\' + ESP32_SN_Edit.Text +
              v299_NowSetting_Memo.Lines.Strings[31].Substring(21-1, 4) +
              v299_NowSetting_Memo.Lines.Strings[31].Substring(26-1, 2) +
              v299_NowSetting_Memo.Lines.Strings[31].Substring(29-1, 2) +
              v299_NowSetting_Memo.Lines.Strings[31].Substring(32-1, 2) +
              v299_NowSetting_Memo.Lines.Strings[31].Substring(35-1, 2) +
              '.Current Mode.csv';
            CopyFile(PChar('.\R2MS_Lite_WorkSpace\Current Mode.csv'), PChar(temp_Str), False); // false=覆蓋,true=略過
            //--
            //--------------------------------------------------------------------------
            // 調整為狀態1
            ERT_ver2_9_9_Client_Schedule_flag := 1; //0=不要做事,1=查找下個即將,2=針對即將進行工作,3=針對初始化進行工作,4=針對等待運作進行工作
            StatusBar1.Panels[1].Text := '排程狀態=' +IntToStr(ERT_ver2_9_9_Client_Schedule_flag);
            //--------------------------------------------------------------------------
            ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[4] := '完成';
            ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[5] := FormatDateTime('yyyy/mm/dd HH:MM:SS', temp_now_TDateTime);
            ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[6] := ''; //FormatDateTime('yyyy/mm/dd HH:MM:SS', IncMinute(temp_NextJob_TDateTime, 1));
          end
          else
          begin
            RunningLog_Memo.Lines.Add('「.\R2MS_Lite_WorkSpace\ERT_ver2_9_9_Client_Data\' + v299_NowSetting_Memo.Lines.Strings[31].Substring(21-1, 16) + '.csv」不存在，表示運作失敗...其他工作都不能執行');
            //ERT_ver2_9_9_Client_Schedule_Timer.Enabled=false;
            //--------------------------------------------------------------------------
            // 調整為狀態0
            ERT_ver2_9_9_Client_Schedule_flag := 0; //0=不要做事,1=查找下個即將,2=針對即將進行工作,3=針對初始化進行工作,4=針對等待運作進行工作
            StatusBar1.Panels[1].Text := '排程狀態=' +IntToStr(ERT_ver2_9_9_Client_Schedule_flag);
            //--------------------------------------------------------------------------
            ScheduleRun_Button.Caption := '運作失敗';
            ScheduleSetting_ListView.Items[temp_i].SubItems.Strings[4] := '運作失敗';
            Application.MessageBox('提示:' + #13#10 + '運作失敗!停止排程工作!', '提示', MB_OK + MB_ICONINFORMATION);
            Exit;
          end;
        end;
      end;
    end;
  end;
  //--------------------------------------------------------------------------

  //--------------------------------------------------------------------------
  // 做完事設定為不忙碌
  ERT_ver2_9_9_Client_Schedule_Timer_busy := False;
  //--------------------------------------------------------------------------

end;
```

### 更新紀錄
+ 4.1 選「Update_Memo」，去編輯「Lines」
```pascal
軟體名稱: R2MS_Lite_智慧排程工具(R2MS_Lite__Smart_Scheduler)
作者: HsiupoYeh
--
v20250923b
修正UI流程。修正自訂放電無效果的問題。
--
v20250923a
實作基本功能。穩定可用版。
--
v20250918b
重新調整UI。
--
v20240510a:
初版

```
