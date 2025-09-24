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

### 更新紀錄
+ 3.1 選「Update_Memo」，去編輯「Lines」
```pascal
軟體名稱: R2MS_Lite_智慧排程工具(R2MS_Lite__Smart_Scheduler)
作者: HsiupoYeh
--
v20250923b
修正UI流程。
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
