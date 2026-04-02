# R2MS_Lite_Smart_Scheduler 開發筆記
+ 作者: HsiupoYeh  
+ 更新日期: 2026-04-01
+ 從 **R2MS_Lite_Smart_Scheduler_v20250923b** 開始，繼續更新基本功能。

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
  + 目前只支援一種，標準的多源64個電極放電(MultipleTx_64)，稱為「M64」。 
    + INI參數應填入「M64」。
    + 軟體UI將顯示「M64」。
+ 單源放電:
  + 單源64個電極放電普通版本(SingleTx_64_Normal)，稱為「S64N」。
    + INI參數應填入「S64N」。
    + 軟體UI將顯示「S64N」。
  + 單源64個電極放電普通版本(SingleTx_64_HighSpeed)，稱為「S64HS」。
    + INI參數應填入「S64HS」。
    + 軟體UI將顯示「S64HS」。
    + 基於S64N放電，減少一些部分放電組合。具體行為可使用ERTMaker來模擬。
  + 單源64個電極放電普通版本(SingleTx_64_VeryHighSpeed)，稱為「S64VHS」。
    + INI參數應填入「S64VHS」。
    + 軟體UI將顯示「S64VHS」。
    + 基於S64N放電，減少更多部分放電組合。具體行為可使用ERTMaker來模擬。
+ 系統測試:
    + INI參數應填入「SystemTest」。
    + 軟體UI將顯示「系統測試」。
+ 自訂放電:
  + 請將設計好的「Current Mode.csv」取代「v299_Custom01CurrentMode」資料夾中的同名檔案。
  + 請將設計好的「setting.txt」取代「v299_Custom01Setting」資料夾中的同名檔案。
  + INI參數應填入「Custom01」。
  + 軟體UI將顯示「Custom01」。
  + 同理，可使用三種自訂放電，分別為「Custom01」、「Custom02」、「Custom03」。

## 開發紀錄(寫功能部分)
### 版本
+ 1.1選選Form，去編輯FormCreate，修改部分:
```pascal
  //--------------------------------------------------------------------------
  // 在這裡對全域變數進行初始化
  version_str := 'v20260401a';
  Current_Folder_Path := ExtractFilePath(Application.ExeName);
  //--------------------------------------------------------------------------  
```

```
  //--------------------------------------------------------------------------
  // 寫入參數檔案
  if need_write_ini_file then
  begin
    RunningLog_Memo.Lines.Add('「' + ChangeFileExt(ExtractFileName(Application.ExeName), '.ini') + '」不存在!');
    my_TMemIniFile.WriteString('Version', 'Version', '"' + version_str + '"');
    my_TMemIniFile.WriteString('Version', 'Author', '"HsiupoYeh"');
    my_TMemIniFile.WriteString('ERT_ver2_9_9_Client', 'ERT_Profile_Name', '"QC01"');
    my_TMemIniFile.WriteString('ERT_ver2_9_9_Client', 'ERT_Project_Name', '"[20260401A]Taiwan-Taoyuan-Factory(QC01)"');
    my_TMemIniFile.WriteString('ERT_ver2_9_9_Client', 'DMM_SN', '"MY60077634"');
    my_TMemIniFile.WriteString('ERT_ver2_9_9_Client', 'DMM_ComPort', '"USB0::0x2A8D::0x0101::MY60077634::INSTR"');
    my_TMemIniFile.WriteString('ERT_ver2_9_9_Client', 'SwitchArray_SN', '"S000"');
    my_TMemIniFile.WriteString('ERT_ver2_9_9_Client', 'SwitchArray_ComPort', '"COM3"');
    my_TMemIniFile.WriteString('ERT_ver2_9_9_Client', 'PSU_SN', '"508I23118"');
    my_TMemIniFile.WriteString('ERT_ver2_9_9_Client', 'PSU_ComPort', '"COM4"');
    my_TMemIniFile.WriteString('Schedule', 'Every_Day_Enable', '"true"');
    my_TMemIniFile.WriteString('Schedule', 'PSU_RANGE_HIGH_LOW', '"LOW"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_000', '"00:00"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_000_CurrentMode', '"S64N"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_000_MaxVoltage_V', '"400"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_000_MaxCurrent_A', '"0.5"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_001', '"00:01"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_001_Enable', '"true"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_001_CurrentMode', '"S64HS"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_001_MaxVoltage_V', '"400"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_001_MaxCurrent_A', '"0.5"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_002', '"01:01"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_002_Enable', '"true"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_002_CurrentMode', '"S64VHS"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_002_MaxVoltage_V', '"400"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_002_MaxCurrent_A', '"0.5"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_003', '"02:01"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_003_Enable', '"true"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_003_CurrentMode', '"M64"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_003_MaxVoltage_V', '"400"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_003_MaxCurrent_A', '"0.5"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_004', '"03:01"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_004_Enable', '"true"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_004_CurrentMode', '"SystemTest"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_004_MaxVoltage_V', '"400"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_004_MaxCurrent_A', '"0.5"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_005', '"04:01"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_005_Enable', '"true"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_005_CurrentMode', '"Custom01"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_005_MaxVoltage_V', '"400"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_005_MaxCurrent_A', '"0.5"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_006', '"05:01"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_006_Enable', '"true"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_006_CurrentMode', '"Custom02"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_006_MaxVoltage_V', '"400"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_006_MaxCurrent_A', '"0.5"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_007', '"06:01"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_007_Enable', '"true"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_007_CurrentMode', '"Custom03"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_007_MaxVoltage_V', '"400"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_007_MaxCurrent_A', '"0.5"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_008', '"07:01"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_008_Enable', '"false"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_008_CurrentMode', '"S64VHS"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_008_MaxVoltage_V', '"400"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_008_MaxCurrent_A', '"0.5"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_009', '"08:01"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_009_Enable', '"false"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_009_CurrentMode', '"S64VHS"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_009_MaxVoltage_V', '"400"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_009_MaxCurrent_A', '"0.5"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_010', '"09:01"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_010_Enable', '"false"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_010_CurrentMode', '"S64VHS"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_010_MaxVoltage_V', '"400"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_010_MaxCurrent_A', '"0.5"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_011', '"10:01"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_011_Enable', '"false"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_011_CurrentMode', '"S64VHS"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_011_MaxVoltage_V', '"400"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_011_MaxCurrent_A', '"0.5"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_012', '"11:01"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_012_Enable', '"false"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_012_CurrentMode', '"S64VHS"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_012_MaxVoltage_V', '"400"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_012_MaxCurrent_A', '"0.5"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_013', '"12:01"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_013_Enable', '"false"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_013_CurrentMode', '"S64VHS"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_013_MaxVoltage_V', '"400"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_013_MaxCurrent_A', '"0.5"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_014', '"13:01"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_014_Enable', '"false"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_014_CurrentMode', '"S64VHS"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_014_MaxVoltage_V', '"400"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_014_MaxCurrent_A', '"0.5"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_015', '"14:01"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_015_Enable', '"false"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_015_CurrentMode', '"S64VHS"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_015_MaxVoltage_V', '"400"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_015_MaxCurrent_A', '"0.5"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_016', '"15:01"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_016_Enable', '"false"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_016_CurrentMode', '"S64VHS"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_016_MaxVoltage_V', '"400"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_016_MaxCurrent_A', '"0.5"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_017', '"16:01"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_017_Enable', '"false"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_017_CurrentMode', '"S64VHS"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_017_MaxVoltage_V', '"400"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_017_MaxCurrent_A', '"0.5"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_018', '"17:01"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_018_Enable', '"false"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_018_CurrentMode', '"S64VHS"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_018_MaxVoltage_V', '"400"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_018_MaxCurrent_A', '"0.5"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_019', '"18:01"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_019_Enable', '"false"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_019_CurrentMode', '"S64VHS"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_019_MaxVoltage_V', '"400"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_019_MaxCurrent_A', '"0.5"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_020', '"19:01"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_020_Enable', '"false"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_020_CurrentMode', '"S64VHS"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_020_MaxVoltage_V', '"400"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_020_MaxCurrent_A', '"0.5"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_021', '"20:01"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_021_Enable', '"false"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_021_CurrentMode', '"S64VHS"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_021_MaxVoltage_V', '"400"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_021_MaxCurrent_A', '"0.5"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_022', '"21:01"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_022_Enable', '"false"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_022_CurrentMode', '"S64VHS"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_022_MaxVoltage_V', '"400"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_022_MaxCurrent_A', '"0.5"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_023', '"22:01"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_023_Enable', '"false"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_023_CurrentMode', '"S64VHS"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_023_MaxVoltage_V', '"400"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_023_MaxCurrent_A', '"0.5"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_024', '"23:01"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_024_Enable', '"false"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_024_CurrentMode', '"S64VHS"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_024_MaxVoltage_V', '"400"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_024_MaxCurrent_A', '"0.5"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_999', '"23:59"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_999_CurrentMode', '"SystemTest"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_999_MaxVoltage_V', '"400"');
    my_TMemIniFile.WriteString('Schedule', 'JOB_999_MaxCurrent_A', '"0.5"');
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
```


### 編輯支援序號清單
+ 2.1 選「ESP32_SupportedSerialNumbers_Memo」，去編輯「Lines」
```
2025-S000
2025-S001
2025-S002
2025-S003
2025-S004
2025-S005
2025-S006
2026-S000
2026-S007
2026-S008
2026-S009
2026-S010
```

### 修正錯誤UI文字
+ 3.1 修改「Name」為「PSU_RangeHIGH_CheckBox」的元件:
  + 3.1.1 設定「Caption」為「HIGH(600V/0.35A)」。
+ 3.2 修改「Name」為「PSU_RangeLOW_CheckBox」的元件:
  + 3.2.1 設定「Caption」為「HIGH(400V/0.5A)」。
+ 3.3 修改「Name」為「ReadMe_Memo」的元件:
  + 3.3.1 ，去編輯「Lines」。
```
==============================================
使用說明:
(1)請準備好正確內容的INI參數檔案(本工具軟體同名的INI檔案，放在執行檔所在目錄)，再運行本程式。
 *若內容有誤，請重新編輯INI檔案後重新運行本工具軟體，直到內容正確。
 *若INI檔案損毀，請手動刪除該檔案後重新運行本工具軟體，將會產生一個新的預設INI檔案。
(2)按下「偵測外部裝置」，軟體將自動偵測外部裝置，並取得序號等資訊。
 *剛開啟軟體時，各項內容呈現藍色字。此為上一次運作時的外部裝置。一般而言沒有特殊用處。
 *按下「偵測外部裝置」按鈕後，系統將偵測外部裝置，過程中可能使各項內容呈現不同顏色的字。若成功，則全部呈現綠色，且其他按鈕會顯示可用。
 *若有任何一項內容呈現不同的顏色，表示偵測外部裝置失敗，請查詢「運行記錄」頁面判斷錯誤原因。一般而言，將外部裝置重新啟動電源並正確使用連接線後，重新運行本工具軟體即可解決問題。
 *已知可能導致異常的情形及其解決方式:
  - 限制只能同時連接一個DMM、一個PSU、一個SwitchArray，否則偵測可能錯誤。請確保運行過程中沒有插入任何其他COM裝置。
  - 電腦中記憶的COM裝置太多，需手動清理。清理方式：「裝置管理員」>「檢視」>勾選「顯示隱藏裝置」，然後查看「連接埠（COM和LPT）」，將所有紀錄的COM裝置解除安裝。
  - 忘了開啟DMM或PSU電源，請等待外部裝置正確開機完成。
  - USB連接線損壞，請更換備品。
  - 不支援的裝置，請確認支援裝置清單中是否有該序號。
  - 連接錯誤的USB插座，請使用電腦端的USB3.0插座。
 *若多次嘗試仍無法解決問題，請詢問製造商。
(3)確認內容正確後，依照墓地進行常見下列三種操作：
 *系統檢查:
  - 按下「立刻單次檢測」，工作清單將會跳轉至索引999，並使其運作時間約在下一分鐘。請盡速再按下「啟動智慧排程」，軟體將自動依照排程進行工作。
 *立刻進行野外資料蒐集:
  - 按下「立刻單次運行」，工作清單將會跳轉至索引000，並使其運作時間約在下一分鐘。請盡速再按下「啟動智慧排程」，軟體將自動依照排程進行工作。
 *依照INI規劃的時間表進行野外資料蒐集:
  - 按下「啟動智慧排程」，軟體將自動依照排程進行工作。
(4)啟動智慧排程的注意事項：
 *每次按下「啟動檢測排程」按鈕，都會清空「R2MS_Lite_WorkSpace」資料夾。
 *每次排程工作都會把所需的資料存到「local」資料夾中，因此「R2MS_Lite_WorkSpace」資料夾中的內容不太重要。
 *因外部程式「ERT_ver2_9_9_Client.exe」輸出資料無檔頭，不利後續資料分享與使用。本軟體將另存為有檔頭的CSV檔案。因此請靜待外部程式自動關閉，並且工作排程顯示完成，才表示檔案已經搬運至「local」資料夾中。之後，可靜待下一個工作排程或關閉軟體。
(5)不想要繼續進行工作時，直接關閉本工具軟體。
 *若正在運行外部程式，建議等待工作完成。或手動操作該外部程式，使其正常關閉。異常操作可能導致不可預期的錯誤，可能需要嘗試電腦重開機來排除錯誤。
 *待外部軟體工作結束後，趁還在倒數的時候關閉軟體。請注意是否會觸發下一個工作排程。
==============================================

==============================================
設計規定:
(1)多源放電:
 *目前只支援一種，標準的多源64個電極放電(MultipleTx_64)，稱為「M64」。 
  - INI參數應填入「M64」。
  - 軟體UI將顯示「M64」。
(2)單源放電:
 *單源64個電極放電普通版本(SingleTx_64_Normal)，稱為「S64N」。
  - INI參數應填入「S64N」。
  - 軟體UI將顯示「S64N」。
 *單源64個電極放電高速版本(SingleTx_64_HighSpeed)，稱為「S64HS」。
  - INI參數應填入「S64HS」。
  - 軟體UI將顯示「S64HS」。
  - 基於S64N放電，減少一些部分放電組合。具體行為可使用ERTMaker來模擬。
 *單源64個電極放電超高速版本(SingleTx_64_VeryHighSpeed)，稱為「S64VHS」。
  - INI參數應填入「S64VHS」。
  - 軟體UI將顯示「S64VHS」。
  - 基於S64N放電，減少更多部分放電組合。具體行為可使用ERTMaker來模擬。
(3)系統測試:
 *使用「24V」鄰近電極輪流放電測試，稱為「系統測試」。
  - INI參數應填入「SystemTest」。
  - 軟體UI將顯示「系統測試」。
(4)自訂放電:
 *進階使用，請熟悉原理並測試成功後再實際使用。
 *請將設計好的「Current Mode.csv」取代「v299_Custom01CurrentMode」資料夾中的同名檔案。
 *請將設計好的「setting.txt」取代「v299_Custom01Setting」資料夾中的同名檔案。
 *INI參數應填入「Custom01」。
 *軟體UI將顯示「Custom01」。
 *同理，可使用三種自訂放電，分別為「Custom01」、「Custom02」、「Custom03」。
 *ERTMaker提供少於64個電極的放電模擬工具及對應設計好的檔案，放在「Configs」資料夾中。
==============================================

==============================================
工作清單(啟動時間設定)說明:
未知:
軟體開啟後，尚未確認工作內容時，狀態為「未知」。 表示軟體剛開啟。

停用:
按下「啟動智慧排程」按鈕後，未勾選的排程，狀態為「停用」。表示此排程將不會被使用。

待機:
按下「啟動智慧排程」按鈕後，經查詢清單後，可用的工作排程之狀態填入「待機」。其「下次時間」欄位則填入預期進行工作的確切時間。

略過:
按下「啟動智慧排程」按鈕後，經查詢清單後，若在「只有今天」的設定下，已經超過當下時間的工作排程，其狀態表示為「略過」。被略過不運行的工作，其「上次時間」欄位將保持空白。

完成:
正常運作完成的排程工作狀態將顯示為「完成」。其「上次時間」欄位將填入其工作完成時間。在「只有今天」的設定下，狀態維持為「完成」；在「每天」設定下，狀態會再次改為「待機」。

即將:
按下「啟動智慧排程」按鈕後，經查詢清單後，將下一個可運作的工作狀態表示為「即將」。同時按鈕會倒數計時，當秒數小於0時進行呼叫「ERT_ver2_9_9_Client.exe」軟體的工作。

初始化:
「即將」的工作時間到後，將呼叫外部程式，該程式進行初始化檢驗的時候，狀態為「初始化」。進入下個階段的時間將顯示在「下次時間」欄位中。注意，倒數時間會比外部程式所需實際時間更久一些。

等待運作:
「初始化」的工作時間到後，外部程式自動進行運作，狀態填入「等待運作」。目前的檢測工作內容耗時要4分鐘。時間到會自動關閉外部程式。當工作正常運作後，狀態將顯示為「完成」。

運作失敗:
各項工作若遇到不正常的狀況，將在狀態顯示「運作失敗」。錯誤原因可查詢「運行記錄」頁面。多數情況下，異常發生與儀器設備異常有關，將停止排程以避免發生進一步損害。使用者應於異常排除後重新運行本軟體。
==============================================
```


### 更新紀錄
+ 4.1 選「Update_Memo」，去編輯「Lines」
```
軟體名稱: R2MS_Lite_智慧排程工具(R2MS_Lite__Smart_Scheduler)
作者: HsiupoYeh
--
v20260401a:
新增支援的裝置。2026-S000、2026-S007、2026-S008、2026-S009、2026-S010。
修正錯誤UI文字。
更新INI檔案內容。預設PSU使用LOW模式。漏電問題未解決時，請勿使用HIGH模式，否則可能造成儀器損毀。
--
v20250923b:
修正UI流程。修正自訂放電無效果的問題。
--
v20250923a:
實作基本功能。穩定可用版。
--
v20250918b:
重新調整UI。
--
v20240510a:
初版

```
