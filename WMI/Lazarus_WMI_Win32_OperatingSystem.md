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

### 步驟3
+ 雙擊 **Button1** 元件，進入編輯 **Button1**的程式碼。
+ 原本:
  ```pascal
  procedure TForm1.Button1Click(Sender: TObject);
  
  begin
  
  end;   
  ```
+ 改成:
  ```pascal
  procedure TForm1.Button1Click(Sender: TObject);
  var
    MyWmiLocator: OleVariant;          // 用於創建與 WMI 服務連接的定位器物件；注意：使用 OleVariant 需要 uses Variants 單元
    MyWmiServices: OleVariant;         // 代表與特定 WMI 命名空間（例如 ROOT\CIMV2）的連接；注意：使用 OleVariant 需要 uses Variants 單元
    MyWmiObjectSet: OleVariant;        // 儲存 WQL 查詢返回的 WMI 物件集合（可能包含多個物件）；注意：使用 OleVariant 需要 uses Variants 單元
    MyWmiObject: OleVariant;           // 在遍歷 MyWmiObjectSet 時，指向集合中的單個 WMI 物件；注意：使用 OleVariant 需要 uses Variants 單元
    MyWmiObjectEnumerator: IEnumVariant; // 用於枚舉（遍歷）WMI 物件集合的 COM 介面，讓我們可以逐個訪問物件
    MyWmiObjectItem: OleVariant;       // 在枚舉過程中，臨時儲存當前遍歷到的單個 WMI 物件；注意：使用 OleVariant 需要 uses Variants 單元
    MyWmiObjectItemFetched: LongWord;  // 記錄在單次呼叫枚舉器的 Next 方法時，實際成功獲取的 WMI 物件的數量
  begin
    Memo1.Clear;
    Memo1.Lines.Add('正在使用 WMI 查詢 Win32_OperatingSystem...');
    Memo1.Lines.Add('');
    try
      Memo1.Lines.Add('步驟 1: 建立 MyWmiLocator 物件...');
      MyWmiLocator := CreateOleObject('WbemScripting.SWbemLocator');
      Memo1.Lines.Add('步驟 1 完成。');
  
      Memo1.Lines.Add('步驟 2: 連接到 ROOT\CIMV2 命名空間...');
      MyWmiServices := MyWmiLocator.ConnectServer(OleVariant(StringToOleStr('.')), OleVariant(StringToOleStr('ROOT\CIMV2')), OleVariant(StringToOleStr('')), OleVariant(StringToOleStr('')));
      Memo1.Lines.Add('步驟 2 完成。');
  
      Memo1.Lines.Add('步驟 3: 執行 WQL 查詢...');
      MyWmiObjectSet := MyWmiServices.ExecQuery(OleVariant(StringToOleStr('SELECT Caption, Version, BuildNumber, Manufacturer, RegisteredUser FROM Win32_OperatingSystem')));
      Memo1.Lines.Add('步驟 3 完成。找到 ' + IntToStr(MyWmiObjectSet.Count) + ' 個作業系統實例。');
  
      Memo1.Lines.Add('步驟 4: 遍歷查詢結果並顯示資訊...');
      MyWmiObjectEnumerator := IUnknown(MyWmiObjectSet._NewEnum) as IEnumVariant;
      MyWmiObjectItem := Unassigned;
      while MyWmiObjectEnumerator.Next(1, MyWmiObjectItem, MyWmiObjectItemFetched) = 0 do
      begin
        MyWmiObject := MyWmiObjectItem;
        Memo1.Lines.Add('  作業系統名稱: ' + String(MyWmiObject.Properties_.Item('Caption').Value));
        Memo1.Lines.Add('  版本: ' + String(MyWmiObject.Properties_.Item('Version').Value));
        Memo1.Lines.Add('  組建編號: ' + String(MyWmiObject.Properties_.Item('BuildNumber').Value));
        Memo1.Lines.Add('  製造商: ' + String(MyWmiObject.Properties_.Item('Manufacturer').Value));
        Memo1.Lines.Add('  註冊使用者: ' + String(MyWmiObject.Properties_.Item('RegisteredUser').Value));
        Memo1.Lines.Add('');
        MyWmiObject := Unassigned;
      end;
      Memo1.Lines.Add('步驟 4 完成。');
  
    except
      on E: Exception do
        Memo1.Lines.Add('查詢 WMI 時發生錯誤: ' + E.Message);
    end;
  
    Memo1.Lines.Add('Win32_OperatingSystem 查詢完成。');
  end;       
  ```
### 步驟4
+ 編譯後，運行軟體。
+ 點擊按鈕就可以看到效果。
