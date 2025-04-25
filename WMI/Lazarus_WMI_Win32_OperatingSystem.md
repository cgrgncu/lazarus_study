# Lazarus用WMI查詢Win32_OperatingSystem資訊
+ 作者: HsiupoYeh
+ 更新日期：2025-04-25

### 目標
+ 可以用PowerShell先確認想查的資訊：
  ```
  PowerShell "Get-WMIObject -Class Win32_OperatingSystem"
  ```
+ 確認好之後就練習用Lazarus查出相同內容。

### 步驟1
+ 在 **Form1** 上用拖拉的方式添加一個 **Button1** 以及一個 **TMemo1**。

### 步驟2
+ 在 **unit1.pas** 程式碼中，添加 **ComObj**、**ActiveX** 和 **Variants** 到 **interface** 與 **type** 之間的 **uses** 中。
+ 原本:  
  ```pascal
  uses
    Classes, SysUtils, Forms, Controls, Graphics, Dialogs; 
  ```
+ 改成:  
  ```pascal
  uses
    Classes, SysUtils, Forms, Controls, Graphics, Dialogs, ComObj, ActiveX, Variants; 
  ```
  > **ComObj** 和 **ActiveX** 用於 COM 物件的創建和操作。  
  > **Variants** 用於處理從 COM 物件返回的變體類型資料。  

