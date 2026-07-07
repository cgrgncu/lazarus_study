# R2MS_Lite_Controller
+ 作者: HsiupoYeh
+ 更新日期: 2026-07-07

### 預設方案(沿用ControlBoard韌體)
+ ControlBoard韌體(ESP32): SwitchArray_ver2_9韌體
  + 使用ESP32的DEBUG_USB溝通，也就是預設的UART0。實際上這裡是使用CP2102，被PC抓到的是COM裝置，鮑率=115200。


### 檢查是否可以溝通
+ 測試
```pascal
procedure TForm1.Button1Click(Sender: TObject);
var
  Response: String;
begin
  LazSerial1.Device := ESP32_COM_Edit.Text;
  LazSerial1.BaudRate:= br115200;
  try
    // 步驟0：開啟COM裝置
    Memo1.Lines.Add('嘗試開啟序列埠(' + LazSerial1.Device + ')...');
    LazSerial1.Open;
    Memo1.Lines.Add('嘗試開啟序列埠(' + LazSerial1.Device + ')...完成!');
    // 步驟1：發送命令查儀器基本資訊(*IDN?)
    Memo1.Lines.Add('發送命令查儀器基本資訊(*IDN?)...');
    LazSerial1.WriteData('*IDN?' + #10);
    Sleep(100);
    Response := LazSerial1.ReadData;
    if Trim(Response) = 'IDN= ESP32_2_9' then
    begin
      Memo1.Lines.Add('與ControlBoard溝通成功!');
    end
    else
    begin
      Memo1.Lines.Add('錯誤!與ControlBoard溝通失敗!');
    end;
    Memo1.Lines.Add('發送命令查儀器基本資訊(*IDN?)...完成!');
    // 步驟999：關閉COM裝置
    Memo1.Lines.Add('嘗試關閉序列埠(' + LazSerial1.Device + ')...');
    LazSerial1.Close;
    Memo1.Lines.Add('嘗試關閉序列埠(' + LazSerial1.Device + ')...完成!');
  except
    on E: Exception do
      Memo1.Lines.Add('開啟序列埠失敗：' + E.Message);
  end;
  //--------------------------------------------------------------------------
end;
```

### 設定操作模式(73 64 64 00 00 01 00 00 64)
+ 常常重新設定
```pascal
procedure TForm1.Button2Click(Sender: TObject);
var
  DataBuffer: array[0..8] of Byte;
begin
  //--------------------------------------------------------------------------
  // --- 韌體 's' 指令參數結構 (總計 9 bytes) ---
  // 順序說明：由韌體接收 USBread() 的順序定義
  DataBuffer[0] := $73; // [Byte 0] 指令碼 's' (ASCII 115)，觸發韌體進入參數設定模式
  DataBuffer[1] := $64; // [Byte 1] switchTurnOnTime: 高電位導通時間 (0x64 = 100us)
  DataBuffer[2] := $64; // [Byte 2] switchTurnOffTime: 低電位截止時間 (0x64 = 100us)
  // [Byte 3] setting_temp: Bitwise 設定
  // Bit 0: accurate_I_only (1=僅精準電流模式, 0=一般模式)
  // Bit 1: enable_accurate (1=啟用精準測量, 0=關閉)
  DataBuffer[3] := $00; // 使用一般模式並關閉精準測量
  // --- 樣本數 (16-bit, 小端序 LSB first) ---
  // 韌體邏輯: (Byte 4) + (Byte 5 * 256)
  DataBuffer[4] := $00; // [Byte 4] sample_number 低位元 (LSB)
  DataBuffer[5] := $01; // [Byte 5] sample_number 高位元 (MSB) -> 0x0100 = 256 筆
  // --- 掃描模式 ---
  DataBuffer[6] := $00; // [Byte 6] scan_mode: 0=單次掃描, 1=連續循環掃描
  // --- 測量週期 (16-bit, 小端序 LSB first) ---
  // 韌體邏輯: (Byte 7) + (Byte 8 * 256)
  DataBuffer[7] := $00; // [Byte 7] measuring_time_temp 低位元 (LSB)
  DataBuffer[8] := $64; // [Byte 8] measuring_time_temp 高位元 (MSB) -> 0x6400 = 25600ms
  //--------------------------------------------------------------------------
  LazSerial1.Device := ESP32_COM_Edit.Text;
  LazSerial1.BaudRate:= br115200;
  try
    // 步驟0：開啟COM裝置
    Memo1.Lines.Add('嘗試開啟序列埠(' + LazSerial1.Device + ')...');
    LazSerial1.Open;
    Memo1.Lines.Add('嘗試開啟序列埠(' + LazSerial1.Device + ')...完成!');
    // 步驟1：發送命令設定操作模式(73 64 64 00 00 01 00 00 64)
    Memo1.Lines.Add('發送命令設定操作模式(73 64 64 00 00 01 00 00 64)...');
    LazSerial1.WriteBuffer(DataBuffer[0], 9);
    Sleep(100);
    Memo1.Lines.Add('發送命令設定操作模式(73 64 64 00 00 01 00 00 64)...完成!');
    // 步驟999：關閉COM裝置
    Memo1.Lines.Add('嘗試關閉序列埠(' + LazSerial1.Device + ')...');
    LazSerial1.Close;
    Memo1.Lines.Add('嘗試關閉序列埠(' + LazSerial1.Device + ')...完成!');
  except
    on E: Exception do
      Memo1.Lines.Add('開啟序列埠失敗：' + E.Message);
  end;
  //--------------------------------------------------------------------------
end;
```
