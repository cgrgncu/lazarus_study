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
