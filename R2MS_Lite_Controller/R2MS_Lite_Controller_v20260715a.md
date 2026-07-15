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

+ 2.2 選「ESP32_SupportedSerialNumbers_Memo」，去編輯「Lines」
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

+ 2.3 選「PSU_SupportedSerialNumbers_Memo」，去編輯「Lines」
```
508I23118
508F24126
508F24128
508F24129
508F24130
508F24133
508F25105
```


### 偵測外部裝置
+ 3.1 去implementation區塊追加宣告uses
```pascal
implementation
//--------------------------------------------------------------------------
// uses add by HsiupoYeh
uses
  IniFiles, Windows, ShellApi, ComObj, ActiveX, Variants;
//--------------------------------------------------------------------------
//--------------------------------------------------------------------------
// 宣告全域變數 add by HsiupoYeh
var
  version_str: AnsiString;
  Current_Folder_Path: AnsiString;
//--------------------------------------------------------------------------
//--------------------------------------------------------------------------
// ======= 1. 底層 VISA 資料型態宣告 =======
type
  ViSession = LongWord;
  ViObject  = LongWord;
  ViStatus  = LongInt;
  ViRsrc    = PAnsiChar;
  ViBuf     = PAnsiChar;
  ViPBuf    = PAnsiChar;
  ViUInt32  = LongWord;
  ViPUInt32 = ^ViUInt32;
// ======= 2. 常數與 DLL 檔案名稱定義 =======
const
  VI_SUCCESS = 0;
  VI_NULL = 0;
  VI_ATTR_TMO_VALUE = $3FFF001A;

  {$IFDEF WIN64}
  VisaDLL = 'visa64.dll';
  {$ELSE}
  VisaDLL = 'visa32.dll';
  {$ENDIF}
// ======= 3. 綁定 Windows 系統內 NI-VISA 的 C 語言函式 =======
function viOpenDefaultRM(sesn: ViPUInt32): ViStatus; stdcall; external VisaDLL;
function viOpen(sesn: ViSession; rsrcName: ViRsrc; accessMode: ViUInt32; timeout: ViUInt32; vi: ViPUInt32): ViStatus; stdcall; external VisaDLL;
function viWrite(vi: ViSession; buf: ViBuf; count: ViUInt32; retCount: ViPUInt32): ViStatus; stdcall; external VisaDLL;
function viRead(vi: ViSession; buf: ViPBuf; count: ViUInt32; retCount: ViPUInt32): ViStatus; stdcall; external VisaDLL;
function viSetAttribute(vi: ViObject; attrName: ViUInt32; attrValue: ViUInt32): ViStatus; stdcall; external VisaDLL;
function viClose(vi: ViObject): ViStatus; stdcall; external VisaDLL;
function viClear(vi: ViSession): ViStatus; stdcall; external VisaDLL;
// ======= 4. 宣告常駐型全域變數，初始值給定為 VI_NULL (0)式 =======
var
  GlobalRM: ViSession = VI_NULL;
  GlobalSession: ViSession = VI_NULL;
//--------------------------------------------------------------------------
{$R *.lfm}
```
+ 3.2 拖拉一個「LazSerial>TLazSerial」到「Form1」中。預設名稱會是「LazSerial1」。修改「Name」為「PSU_LazSerial」。
+ 3.3 拖拉一個「LazSerial>TLazSerial」到「Form1」中。預設名稱會是「LazSerial1」。修改「Name」為「ESP32_LazSerial」。
+ 3.4 
+ 3.5 選「CheckExternalDevices_Button」，去編輯「OnClick」
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
  temp_i: Integer;
begin
  CheckExternalDevices_Button.Enabled := False;
  PSU_Edit.Font.Color := clBlue;
  ESP32_Edit.Font.Color := clBlue;
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
  PSU_LazSerial.Device := PSU_Edit.Text;
  PSU_LazSerial.BaudRate:= br_57600;
  try
    // 步驟0：開啟COM裝置
    RunningLog_Memo.Lines.Add('嘗試開啟序列埠(' + PSU_LazSerial.Device + ')...');
    PSU_LazSerial.Open;
    RunningLog_Memo.Lines.Add('嘗試開啟序列埠(' + PSU_LazSerial.Device + ')...完成!');
    // 步驟1：發送命令查儀器基本資訊(*IDN?)
    PSU_LazSerial.WriteData('*IDN?' + #13#10);
    Sleep(100);
    Response := PSU_LazSerial.ReadData;
    RunningLog_Memo.Lines.Add('PSU基本資訊 =' + Trim(Response));
    // 步驟2：發送命令查型號(MODEL?)
    PSU_LazSerial.WriteData('MODEL?' + #13#10);
    Sleep(100);
    Response := PSU_LazSerial.ReadData;
    RunningLog_Memo.Lines.Add('PSU型號 = ' + Trim(Response));
    // 步驟3：發送命令查序號(SYS:SER?)
    PSU_LazSerial.WriteData('SYS:SER?' + #13#10);
    Sleep(100);
    Response := PSU_LazSerial.ReadData;
    RunningLog_Memo.Lines.Add('PSU序號 = ' + Trim(Response));
    // 步驟4：發送命令查韌體版本(VERsion?)
    PSU_LazSerial.WriteData('VERsion?' + #13#10);
    Sleep(100);
    Response := PSU_LazSerial.ReadData;
    RunningLog_Memo.Lines.Add('PSU韌體版本 = ' + Trim(Response));
    // 步驟5：發送命令查電壓範圍(SOUR:VOLT:RANG LOW)(SOUR:VOLT:RANG?)
    PSU_LazSerial.WriteData('SOUR:VOLT:RANG LOW' + #13#10);
    PSU_LazSerial.WriteData('SOUR:VOLT:RANG?' + #13#10);
    Sleep(100);
    Response := PSU_LazSerial.ReadData;
    RunningLog_Memo.Lines.Add('PSU電壓範圍(HIGH=600V/0.35A, LOW=400V/0.5A) = ' + Trim(Response));
    if SameText(Trim(Response), 'LOW') then
    begin
      PSU_Edit.Font.Color := clGreen;
      Application.ProcessMessages;
    end
    else
    begin
      RunningLog_Memo.Lines.Add('警告：PSU電壓範圍無效或與預期不符。是否沒有開啟PSU電源?或是PSU被其他軟體占用?');
    end;
    // 步驟6：發送命令查蜂鳴器設定(BEEP OFF)(SYS:BEEP?)
    PSU_LazSerial.WriteData('BEEP OFF' + #13#10);
    PSU_LazSerial.WriteData('SYS:BEEP?' + #13#10);
    Sleep(100);
    Response := PSU_LazSerial.ReadData;
    RunningLog_Memo.Lines.Add('PSU蜂鳴器設定 = ' + Trim(Response));
    // 步驟7：發送命令查LED模式設定(SYS:LED ON)(SYS:LED?)
    PSU_LazSerial.WriteData('SYS:LED ON' + #13#10);
    PSU_LazSerial.WriteData('SYS:LED?' + #13#10);
    Sleep(100);
    Response := PSU_LazSerial.ReadData;
    RunningLog_Memo.Lines.Add('PSULED模式 = ' + Trim(Response));
    PSU_Edit.Font.Color := clGreen;
    // 步驟999：關閉COM裝置
    RunningLog_Memo.Lines.Add('嘗試關閉序列埠(' + PSU_LazSerial.Device + ')...');
    PSU_LazSerial.Close;
    RunningLog_Memo.Lines.Add('嘗試關閉序列埠(' + PSU_LazSerial.Device + ')...完成!');
  except
    on E: Exception do
      RunningLog_Memo.Lines.Add('開啟序列埠失敗：' + E.Message);
  end;
  //--------------------------------------------------------------------------
  //--------------------------------------------------------------------------
  // 開啟ESP32的COM裝置來檢查是否設定正確
  if (ESP32_Edit.Font.Color = clBlue) then
  begin
    RunningLog_Memo.Lines.Add('警告：SwitchArray裝置資訊異常。是否沒有連接SwitchArray?或是不在支援清單中?');
    PSU_Edit.Color := clYellow;
    Exit;
  end;
  //--
  ESP32_Edit.Font.Color := clRed;
  Application.ProcessMessages;
  ESP32_LazSerial.Device := ESP32_Edit.Text;
  ESP32_LazSerial.BaudRate:= br115200;
  try
    // 步驟0：開啟COM裝置
    RunningLog_Memo.Lines.Add('嘗試開啟序列埠(' + ESP32_LazSerial.Device + ')...');
    ESP32_LazSerial.Open;
    RunningLog_Memo.Lines.Add('嘗試開啟序列埠(' + ESP32_LazSerial.Device + ')...完成!');
    // 步驟1：發送命令查儀器基本資訊(*IDN?)
    ESP32_LazSerial.WriteData('*IDN?' + #10);
    Sleep(100);
    Response := ESP32_LazSerial.ReadData;
    if Trim(Response) = 'IDN= ESP32_2_9' then
    begin
      //RunningLog_Memo.Lines.Add('與ControlBoard溝通成功!');
      RunningLog_Memo.Lines.Add(Trim(Response));
      ESP32_Edit.Font.Color := clGreen;
      Application.ProcessMessages;
    end
    else
    begin
      RunningLog_Memo.Lines.Add('錯誤!與ControlBoard溝通失敗!');
    end;
    // 步驟999：關閉COM裝置
    RunningLog_Memo.Lines.Add('嘗試關閉序列埠(' + ESP32_LazSerial.Device + ')...');
    ESP32_LazSerial.Close;
    RunningLog_Memo.Lines.Add('嘗試關閉序列埠(' + ESP32_LazSerial.Device + ')...完成!');
  except
    on E: Exception do
      RunningLog_Memo.Lines.Add('開啟序列埠失敗：' + E.Message);
  end;
  //--------------------------------------------------------------------------


  //--------------------------------------------------------------------------
  // 設定按鈕啟用禁用
  if (DMM_Edit.Font.Color = clGreen) and (ESP32_Edit.Font.Color = clGreen) and (PSU_Edit.Font.Color = clGreen) then
  begin
    RunningLog_Memo.Lines.Add('設定按鈕啟用禁用...');
  end;
  //--------------------------------------------------------------------------
end; 
```

### 更新紀錄
+ 4.1 選「Update_Memo」，去編輯「Lines」
```
軟體名稱: R2MS_Lite_控制器(R2MS_Lite_Controller)
作者: HsiupoYeh
--
v20260715a:
重新調整UI。
--
v20260713a:
初版
```
