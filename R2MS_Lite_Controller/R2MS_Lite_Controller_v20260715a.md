# R2MS_Lite_Controller 開發筆記
+ 作者: HsiupoYeh  
+ 更新日期: 2026-07-15

## 開發環境  
+ 使用「Win10_64_CHT_Lazarus4.2_v20250915a.ova」。**匯入為「Win10_Lazarus4.2_R2MS_Lite_Controller_v20250915a.ova」。**
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

## 開發紀錄(寫功能部分)
### 版本
+ 1.1 選「Form」，去編輯「FormCreate」，修改部分:
```pascal
  //--------------------------------------------------------------------------
  // 在這裡對全域變數進行初始化
  version_str := 'v20260715a';
  Current_Folder_Path := ExtractFilePath(Application.ExeName);
  //--------------------------------------------------------------------------  
```

### 編輯支援序號清單
+ 2.1 選「DMM_SupportedSerialNumbers_Memo」，去編輯「Lines」
```
MY59004074
MY60077634
MY60090449
MY60076563
MY60078728
MY60681084
MY64001649
MY64004458
```

+ 2.2 選「PSU_SupportedSerialNumbers_Memo」，去編輯「Lines」
```
508I23118
508F24126
508F24128
508F24129
508F24130
508F24133
508F25105
```
+ 2.3 選「ESP32_SupportedSerialNumbers_Memo」，去編輯「Lines」
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

### 更新紀錄
+ 3.1 選「Update_Memo」，去編輯「Lines」
```
軟體名稱: R2MS_Lite_控制器(R2MS_Lite_Controller)
作者: HsiupoYeh
--
v20250918b:
重新調整UI。
--
v20260713a:
初版
```
