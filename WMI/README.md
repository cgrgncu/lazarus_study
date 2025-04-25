# 使用Windows Management Instrumentation（WMI）範例
+ 作者: HsiupoYeh
+ 更新日期： 2025-04-25
+ 開發環境：Win10_64_CHT_Lazarus3.6_v20241102a.ova
  + 基本上就是: lazarus-3.6-fpc-3.2.2-win64.exe
  + 並且預裝: Indy10、固定視窗IDE

### Windows Management Instrumentation（WMI）簡介
+ 是 Windows 提供的一套 API，可讓你查詢與管理系統資訊。它提供一種類似 SQL 的語法（WQL）來存取資訊，如：
  - 處理器（CPU）
  - 記憶體、硬碟、主機板
  - 安裝的軟體或驅動程式
  - USB 裝置、COM 埠
  - 執行中的程序、服務、網路資訊等

### WMI 可以做什麼？

| 資訊類型         | WMI 類別名              | 範例 WQL 查詢                                |
|------------------|-------------------------|---------------------------------------------|
| CPU              | `Win32_Processor`       | `SELECT * FROM Win32_Processor`             |
| 主機板           | `Win32_BaseBoard`       | `SELECT * FROM Win32_BaseBoard`             |
| 記憶體           | `Win32_PhysicalMemory`  | `SELECT * FROM Win32_PhysicalMemory`        |
| 硬碟             | `Win32_DiskDrive`       | `SELECT * FROM Win32_DiskDrive`             |
| USB 裝置         | `Win32_PnPEntity`       | `SELECT * FROM Win32_PnPEntity`             |
| COM 埠裝置       | `Win32_PnPEntity`       | `SELECT * FROM Win32_PnPEntity WHERE Name LIKE "%(COM%"` |

### Lazarus程式範例：查詢 CPU 名稱

```pascal
uses
  ComObj, ActiveX, Variants;

procedure GetCPUName;
var
  Locator, Services, Enum, Item: OleVariant;
begin
  CoInitialize(nil);
  try
    Locator := CreateOleObject('WbemScripting.SWbemLocator');
    Services := Locator.ConnectServer('localhost', 'root\CIMV2');
    Enum := Services.ExecQuery('SELECT * FROM Win32_Processor');

    for Item in Enum do
      Writeln('CPU: ', Item.Name);
  finally
    CoUninitialize;
  end;
end;
