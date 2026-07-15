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

### A命令(切換64個頻道並送出索引值)
+ 範例
```pascal
procedure TForm1.Button9Click(Sender: TObject);
var
  DataBuffer: array[0..3] of Byte;
  ByteValue: Byte;
  i: Integer;
  Response: String;
  A_currentstate_decimal: Integer;
begin
  //--------------------------------------------------------------------------
  // --- 韌體 'a' 指令參數結構 (總計 4 bytes) ---
  // 順序說明：由韌體接收 USBread() 的順序定義
  DataBuffer[0] := $61; // [Byte 0] 指令碼 'a' (ASCII 97)，觸發韌體進入通道模式
  DataBuffer[1] := $40; // [Byte 1] channel_count: 通道數量 (0x40 = 64 個通道)
  // 索引值(0~65534，65535被保留): (Byte 2 * 256) + (Byte 3)
  A_currentstate_decimal := StrToInt(A_Index.Text);
  DataBuffer[2] := A_currentstate_decimal div 256; // [Byte 2] current_state 高位元 (MSB)
  DataBuffer[3] := A_currentstate_decimal mod 256; // [Byte 3] current_state 低位元 (LSB)
  //--------------------------------------------------------------------------
  LazSerial1.Device := ESP32_COM_Edit.Text;
  LazSerial1.BaudRate:= br115200;
  try
    // 步驟0：開啟COM裝置
    Memo1.Lines.Add('嘗試開啟序列埠(' + LazSerial1.Device + ')...');
    LazSerial1.Open;
    Memo1.Lines.Add('嘗試開啟序列埠(' + LazSerial1.Device + ')...完成!');
    // 步驟1：發送A命令
    Memo1.Lines.Add('發送A命令(0x'+IntToHex(DataBuffer[0], 2)+', 0x'+IntToHex(DataBuffer[1], 2)+', 0x'+IntToHex(DataBuffer[2], 2)+', 0x'+IntToHex(DataBuffer[3], 2)+')...');
    LazSerial1.WriteBuffer(DataBuffer[0], 4);
    Memo1.Lines.Add('發送A命令...完成!');
    // 步驟2：發送64個頻道設定
    Memo1.Lines.Add('發送64個頻道設定...');
    for i := 1 to 8 do
    begin
      ByteValue := StrToInt(LabeledEdit1.Text[i]);
      LazSerial1.WriteBuffer(ByteValue, 1);
    end;
    for i := 1 to 8 do
    begin
      ByteValue := StrToInt(LabeledEdit2.Text[i]);
      LazSerial1.WriteBuffer(ByteValue, 1);
    end;
    for i := 1 to 8 do
    begin
      ByteValue := StrToInt(LabeledEdit3.Text[i]);
      LazSerial1.WriteBuffer(ByteValue, 1);
    end;
    for i := 1 to 8 do
    begin
      ByteValue := StrToInt(LabeledEdit4.Text[i]);
      LazSerial1.WriteBuffer(ByteValue, 1);
    end;
    for i := 1 to 8 do
    begin
      ByteValue := StrToInt(LabeledEdit5.Text[i]);
      LazSerial1.WriteBuffer(ByteValue, 1);
    end;
    for i := 1 to 8 do
    begin
      ByteValue := StrToInt(LabeledEdit6.Text[i]);
      LazSerial1.WriteBuffer(ByteValue, 1);
    end;
    for i := 1 to 8 do
    begin
      ByteValue := StrToInt(LabeledEdit7.Text[i]);
      LazSerial1.WriteBuffer(ByteValue, 1);
    end;
    for i := 1 to 8 do
    begin
      ByteValue := StrToInt(LabeledEdit8.Text[i]);
      LazSerial1.WriteBuffer(ByteValue, 1);
    end;
    Sleep(100);
    Response := Trim(LazSerial1.ReadData);//因為韌體回傳必為數字的文字並代換行字元，但在特殊情況下會回傳#開頭的文字
    if Pos('#', Response) > 0 then
    begin
      Memo1.Lines.Add(Response);
    end
    else
    begin
      Memo1.Lines.Add(Response);
      if StrToInt(Response) = DataBuffer[2]*256 + DataBuffer[3] then
      begin
        Memo1.Lines.Add('回傳了正確的編號!(' + Response + ')');
      end;
    end;
    Memo1.Lines.Add('發送64個頻道設定...完成!');
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

# BK 9185B溝通

### CP2102
+ 測試溝通的命令
```pascal
procedure TForm1.Button11Click(Sender: TObject);
var
  Response: String;
  i: Integer;
begin
  LazSerial2.Device := PSU_COM_Edit.Text;
  LazSerial2.BaudRate:= br_57600;
  try
    // 步驟0：開啟COM裝置
    Memo1.Lines.Add('嘗試開啟序列埠(' + LazSerial2.Device + ')...');
    LazSerial2.Open;
    Memo1.Lines.Add('嘗試開啟序列埠(' + LazSerial2.Device + ')...完成!');
    // 步驟1：發送命令查儀器基本資訊(*IDN?)
    Memo1.Lines.Add('發送命令查儀器基本資訊(*IDN?)...');
    LazSerial2.WriteData('*IDN?' + #13#10);
    // 給儀器 200ms 的總等待窗口
    for i := 1 to 200 do
    begin
      if LazSerial2.DataAvailable then
      begin
        Response := LazSerial2.ReadData;
        Memo1.Lines.Add('儀器回應耗時: ' + IntToStr(i) + ' ms');
        Break;
      end;
      Sleep(1);
    end;
    Memo1.Lines.Add('PSU基本資訊 =' + Trim(Response));
    Memo1.Lines.Add('發送命令查儀器基本資訊(*IDN?)...完成!');
    // 步驟2：發送命令查型號(MODEL?)
    Memo1.Lines.Add('發送命令查型號(MODEL?)...');
    LazSerial2.WriteData('MODEL?' + #13#10);
    // 給儀器 200ms 的總等待窗口
    for i := 1 to 200 do
    begin
      if LazSerial2.DataAvailable then
      begin
        Response := LazSerial2.ReadData;
        Memo1.Lines.Add('儀器回應耗時: ' + IntToStr(i) + ' ms');
        Break;
      end;
      Sleep(1);
    end;
    Memo1.Lines.Add('PSU型號 = ' + Trim(Response));
    Memo1.Lines.Add('發送命令查型號(MODEL?)...完成!');
    // 步驟3：發送命令查序號(SYS:SER?)
    Memo1.Lines.Add('發送命令查序號(SYS:SER?)...');
    LazSerial2.WriteData('SYS:SER?' + #13#10);
    // 給儀器 200ms 的總等待窗口
    for i := 1 to 200 do
    begin
      if LazSerial2.DataAvailable then
      begin
        Response := LazSerial2.ReadData;
        Memo1.Lines.Add('儀器回應耗時: ' + IntToStr(i) + ' ms');
        Break;
      end;
      Sleep(1);
    end;
    Memo1.Lines.Add('PSU序號 = ' + Trim(Response));
    Memo1.Lines.Add('發送命令查序號(SYS:SER?)...完成!');
    // 步驟4：發送命令查韌體版本(VERsion?)
    Memo1.Lines.Add('發送命令查韌體版本(VERsion?)...');
    LazSerial2.WriteData('VERsion?' + #13#10);
    // 給儀器 200ms 的總等待窗口
    for i := 1 to 200 do
    begin
      if LazSerial2.DataAvailable then
      begin
        Response := LazSerial2.ReadData;
        Memo1.Lines.Add('儀器回應耗時: ' + IntToStr(i) + ' ms');
        Break;
      end;
      Sleep(1);
    end;
    Memo1.Lines.Add('PSU韌體版本 = ' + Trim(Response));
    Memo1.Lines.Add('發送命令查韌體版本(VERsion?)...完成!');
    // 步驟5：發送命令設定並查電壓範圍(SOUR:VOLT:RANG LOW)(SOUR:VOLT:RANG?)
    Memo1.Lines.Add('發送命令設定並查電壓範圍(SOUR:VOLT:RANG LOW)(SOUR:VOLT:RANG?)...');
    LazSerial2.WriteData('SOUR:VOLT:RANG LOW' + #13#10);
    LazSerial2.WriteData('SOUR:VOLT:RANG?' + #13#10);
    // 給儀器 200ms 的總等待窗口
    for i := 1 to 200 do
    begin
      if LazSerial2.DataAvailable then
      begin
        Response := LazSerial2.ReadData;
        Memo1.Lines.Add('儀器回應耗時: ' + IntToStr(i) + ' ms');
        Break;
      end;
      Sleep(1);
    end;
    Memo1.Lines.Add('PSU電壓範圍(HIGH=600V/0.35A, LOW=400V/0.5A) = ' + Trim(Response));
    Memo1.Lines.Add('發送命令設定並查電壓範圍(SOUR:VOLT:RANG LOW)(SOUR:VOLT:RANG?)...完成!');
    // 步驟6：發送命令設定並查蜂鳴器設定(BEEP OFF)(SYS:BEEP?)
    Memo1.Lines.Add('發送命令設定並查蜂鳴器設定(BEEP OFF)(SYS:BEEP?)...');
    LazSerial2.WriteData('BEEP OFF' + #13#10);
    LazSerial2.WriteData('SYS:BEEP?' + #13#10);
    // 給儀器 200ms 的總等待窗口
    for i := 1 to 200 do
    begin
      if LazSerial2.DataAvailable then
      begin
        Response := LazSerial2.ReadData;
        Memo1.Lines.Add('儀器回應耗時: ' + IntToStr(i) + ' ms');
        Break;
      end;
      Sleep(1);
    end;
    Memo1.Lines.Add('PSU蜂鳴器設定 = ' + Trim(Response));
    Memo1.Lines.Add('發送命令設定並查蜂鳴器設定(BEEP OFF)(SYS:BEEP?)...完成!');
    // 步驟7：發送命令設定並查LED模式設定(SYS:LED ON)(SYS:LED?)
    Memo1.Lines.Add('發送命令設定並查LED模式設定(SYS:LED ON)(SYS:LED?)...');
    LazSerial2.WriteData('SYS:LED ON' + #13#10);
    LazSerial2.WriteData('SYS:LED?' + #13#10);
    // 給儀器 200ms 的總等待窗口
    for i := 1 to 200 do
    begin
      if LazSerial2.DataAvailable then
      begin
        Response := LazSerial2.ReadData;
        Memo1.Lines.Add('儀器回應耗時: ' + IntToStr(i) + ' ms');
        Break;
      end;
      Sleep(1);
    end;
    Memo1.Lines.Add('PSULED模式 = ' + Trim(Response));
     Memo1.Lines.Add('發送命令設定並查LED模式設定(SYS:LED ON)(SYS:LED?)...完成!');
    // 步驟999：關閉COM裝置
    Memo1.Lines.Add('嘗試關閉序列埠(' + LazSerial2.Device + ')...');
    LazSerial2.Close;
    Memo1.Lines.Add('嘗試關閉序列埠(' + LazSerial2.Device + ')...完成!');
  except
    on E: Exception do
      Memo1.Lines.Add('開啟序列埠失敗：' + E.Message);
  end;
  //--------------------------------------------------------------------------
end;
```

### 放電
```pascal
procedure TForm1.Button12Click(Sender: TObject);
var
  Response: String;
  i: Integer;
begin
  LazSerial2.Device := PSU_COM_Edit.Text;
  LazSerial2.BaudRate:= br_57600;
  try
    // 步驟0：開啟COM裝置
    Memo1.Lines.Add('嘗試開啟序列埠(' + LazSerial2.Device + ')...');
    LazSerial2.Open;
    Memo1.Lines.Add('嘗試開啟序列埠(' + LazSerial2.Device + ')...完成!');
    // 步驟1：發送命令設定並查上限電壓(SOUR:VOLT 5.000)(SOUR:VOLT?)
    Memo1.Lines.Add('發送命令設定並查上限電壓(SOUR:VOLT 5.000)(SOUR:VOLT?)...');
    LazSerial2.WriteData('SOUR:VOLT 5.000' + #13#10);
    LazSerial2.WriteData('SOUR:VOLT?' + #13#10);
    // 給儀器 200ms 的總等待窗口
    for i := 1 to 200 do
    begin
      if LazSerial2.DataAvailable then
      begin
        Response := LazSerial2.ReadData;
        Memo1.Lines.Add('儀器回應耗時: ' + IntToStr(i) + ' ms');
        Break;
      end;
      Sleep(1);
    end;
    Memo1.Lines.Add('上限電壓(V) = ' + Trim(Response));
    Memo1.Lines.Add('發送命令設定並查上限電壓(SOUR:VOLT 5.000)(SOUR:VOLT?)...完成!');
    // 步驟2：發送命令設定並查上限電流(SOUR:CURR 0.05)(SOUR:CURR?)
    Memo1.Lines.Add('發送命令設定並查上限電流(SOUR:CURR 0.05)(SOUR:CURR?)...');
    LazSerial2.WriteData('SOUR:CURR 0.05' + #13#10);
    LazSerial2.WriteData('SOUR:CURR?' + #13#10);
    // 給儀器 200ms 的總等待窗口
    for i := 1 to 200 do
    begin
      if LazSerial2.DataAvailable then
      begin
        Response := LazSerial2.ReadData;
        Memo1.Lines.Add('儀器回應耗時: ' + IntToStr(i) + ' ms');
        Break;
      end;
      Sleep(1);
    end;
    Memo1.Lines.Add('上限電流(A) = ' + Trim(Response));
    Memo1.Lines.Add('發送命令設定並查上限電流(SOUR:CURR 0.05)(SOUR:CURR?)...完成!');
    // 步驟3：發送命令設定並查輸出狀態(OUT ON)(OUT?)
    Memo1.Lines.Add('發送命令設定並查輸出狀態(OUT ON)(OUT?)...');
    LazSerial2.WriteData('OUT ON' + #13#10);
    LazSerial2.WriteData('OUT?' + #13#10);
    // 給儀器 200ms 的總等待窗口
    for i := 1 to 200 do
    begin
      if LazSerial2.DataAvailable then
      begin
        Response := LazSerial2.ReadData;
        Memo1.Lines.Add('儀器回應耗時: ' + IntToStr(i) + ' ms');
        Break;
      end;
      Sleep(1);
    end;
    Memo1.Lines.Add('輸出狀態(ON/OFF) = ' + Trim(Response));
    Memo1.Lines.Add('發送命令設定並查輸出狀態(OUT ON)(OUT?)...完成!');
    // 等待一秒(讓PSU持續放電1秒)
    Sleep(1000);
    // 步驟4：發送命令查電壓(VOUT?)
    Memo1.Lines.Add('發送命令查電壓(VOUT?)...');
    LazSerial2.WriteData('VOUT?' + #13#10);
    // 給儀器 200ms 的總等待窗口
    for i := 1 to 200 do
    begin
      if LazSerial2.DataAvailable then
      begin
        Response := LazSerial2.ReadData;
        Memo1.Lines.Add('儀器回應耗時: ' + IntToStr(i) + ' ms');
        Break;
      end;
      Sleep(1);
    end;
    Memo1.Lines.Add('目前電壓(V) = ' + Trim(Response));
    Memo1.Lines.Add('發送命令查電壓(VOUT?)...完成!');
    // 步驟5：發送命令查電流(IOUT?)
    Memo1.Lines.Add('發送命令查電流(IOUT?)...');
    LazSerial2.WriteData('IOUT?' + #13#10);
    // 給儀器 200ms 的總等待窗口
    for i := 1 to 200 do
    begin
      if LazSerial2.DataAvailable then
      begin
        Response := LazSerial2.ReadData;
        Memo1.Lines.Add('儀器回應耗時: ' + IntToStr(i) + ' ms');
        Break;
      end;
      Sleep(1);
    end;
    Memo1.Lines.Add('目前電流(A) = ' + Trim(Response));
    Memo1.Lines.Add('發送命令查電壓(IOUT?)...完成!');
    // 步驟6：發送命令設定並查輸出狀態(OUT OFF)(OUT?)
    Memo1.Lines.Add('發送命令設定並查輸出狀態(OUT OFF)(OUT?)...');
    LazSerial2.WriteData('OUT OFF' + #13#10);
    LazSerial2.WriteData('OUT?' + #13#10);
    // 給儀器 200ms 的總等待窗口
    for i := 1 to 200 do
    begin
      if LazSerial2.DataAvailable then
      begin
        Response := LazSerial2.ReadData;
        Memo1.Lines.Add('儀器回應耗時: ' + IntToStr(i) + ' ms');
        Break;
      end;
      Sleep(1);
    end;
    Memo1.Lines.Add('輸出狀態(ON/OFF) = ' + Trim(Response));
    Memo1.Lines.Add('發送命令設定並查輸出狀態(OUT OFF)(OUT?)...完成!');
    // 步驟7：發送命令查電壓(VOUT?)
    Memo1.Lines.Add('發送命令查電壓(VOUT?)...');
    LazSerial2.WriteData('VOUT?' + #13#10);
    // 給儀器 200ms 的總等待窗口
    for i := 1 to 200 do
    begin
      if LazSerial2.DataAvailable then
      begin
        Response := LazSerial2.ReadData;
        Memo1.Lines.Add('儀器回應耗時: ' + IntToStr(i) + ' ms');
        Break;
      end;
      Sleep(1);
    end;
    Memo1.Lines.Add('目前電壓(V) = ' + Trim(Response));
    Memo1.Lines.Add('發送命令查電壓(VOUT?)...完成!');
    // 步驟8：發送命令查電流(IOUT?)
    Memo1.Lines.Add('發送命令查電流(IOUT?)...');
    LazSerial2.WriteData('IOUT?' + #13#10);
    // 給儀器 200ms 的總等待窗口
    for i := 1 to 200 do
    begin
      if LazSerial2.DataAvailable then
      begin
        Response := LazSerial2.ReadData;
        Memo1.Lines.Add('儀器回應耗時: ' + IntToStr(i) + ' ms');
        Break;
      end;
      Sleep(1);
    end;
    Memo1.Lines.Add('目前電流(A) = ' + Trim(Response));
    Memo1.Lines.Add('發送命令查電壓(IOUT?)...完成!');
    // 步驟999：關閉COM裝置
    Memo1.Lines.Add('嘗試關閉序列埠(' + LazSerial2.Device + ')...');
    LazSerial2.Close;
    Memo1.Lines.Add('嘗試關閉序列埠(' + LazSerial2.Device + ')...完成!');
  except
    on E: Exception do
      Memo1.Lines.Add('開啟序列埠失敗：' + E.Message);
  end;
  //--------------------------------------------------------------------------
end;
```

## DMM(34465A)溝通
+ REF: https://www.batronix.com/files/Keysight/DMM/34460-34470/34460-70-Manual.pdf
+ 通訊協定: SCPI指令集。
+ 傳輸方式:
  + USB
    + 中層: VISA(例如封裝為visa.dll。稍微高階一些與多數儀器方案相同的API但其實裡面就包含了等效於底層SICL的溝通。)
    + 底層: SICL(例如封裝為sicl.dll。允許直接使用底層方式溝通，理論上效率可更高。)
    + 以上兩個命令都提供了viClear這樣的功能，可以插隊中斷目前不明原因卡住的指令。
  + LAN
    + 底層: TCP socket(使用OS原生dll)

+ 開啟
```pascal
procedure TForm1.Button1Click(Sender: TObject);
var
  Status: ViStatus;
  AddressName: AnsiString;
begin
  //------------------------------------------------------------------
  // 錯誤預防
  if GlobalSession <> VI_NULL then
  begin
    Memo1.Lines.Add('【提示】 DMM連線已經建立，無需重複執行');
    Exit;
  end;
  //------------------------------------------------------------------
  //------------------------------------------------------------------
  AddressName := Edit1.Text; // 填入實際位址
  // 初始化 VISA 資源管理器
  Status := viOpenDefaultRM(@GlobalRM);

  if Status < VI_SUCCESS then
  begin
    Memo1.Lines.Add('【錯誤】 無法初始化 NI-VISA 驅動層。');
    Exit;
  end;
  //------------------------------------------------------------------
  //------------------------------------------------------------------
  // 建立實體連線通道並存入 GlobalSession
  Status := viOpen(GlobalRM, PAnsiChar(AddressName), VI_NULL, VI_NULL, @GlobalSession);

  if Status = VI_SUCCESS then
  begin
    // 成功提示
    Memo1.Lines.Add('【成功】已建立與儀器的連線。');
  end
  else
  begin
    // 失敗提示 (詳細錯誤處理)
    Memo1.Lines.Add('【錯誤】無法開啟 USB 裝置 (錯誤代碼: ' + IntToHex(Status, 8) + ')');

    // 釋放資源
    viClose(GlobalRM);
    GlobalRM := VI_NULL;
    GlobalSession := VI_NULL;
    Exit;
  end;
  //------------------------------------------------------------------
  //------------------------------------------------------------------
  // 儀器緩衝區清空
  Status := viClear(GlobalSession);

  if Status = VI_SUCCESS then
  begin
    Memo1.Lines.Add('【設定】儀器緩衝區清空，狀態已重置。');
  end
  else
  begin
    Memo1.Lines.Add('【錯誤】無法執行 viClear，錯誤碼: ' + IntToHex(Status, 8));
  end;
  //------------------------------------------------------------------
  //------------------------------------------------------------------
  // 設定連線超時時間為 1 秒
  Status := viSetAttribute(GlobalSession, VI_ATTR_TMO_VALUE, 1000);

  if Status = VI_SUCCESS then
  begin
    Memo1.Lines.Add('【設定】超時參數已更新為 1000ms。');
    Memo1.Lines.Add('【成功】USB 通訊管道已成功開啟並保持常駐！');
  end
  else
  begin
    Memo1.Lines.Add('【警告】連線已建立，但設定屬性失敗 (錯誤碼: ' + IntToHex(Status, 8) + ')。');
  end;
  //------------------------------------------------------------------
end;   
```

+ 關閉
```pascal
procedure TForm1.Button3Click(Sender: TObject);
begin
  // 如果本來就沒連線，直接無視
  if GlobalSession = VI_NULL then
  begin
    Memo1.Lines.Add('提示：目前本來就沒有開啟任何連線。');
    Exit;
  end;

  // 安全關閉 Session 釋放 USB 資源
  viClose(GlobalSession);
  viClose(GlobalRM);

  // 關鍵：將全域變數重設為空，讓程式知道管線已經斷了
  GlobalSession := VI_NULL;
  GlobalRM := VI_NULL;

  Memo1.Lines.Add('【安全釋放】USB 通訊管道已成功關閉。');
end; 
```

### 流程
+ 開啟PSU
+ 開啟ESP32
+ 開啟DMM
+ 關閉PSU
+ 關閉ESP32
+ 關閉DMM
