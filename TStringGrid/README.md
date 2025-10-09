# TStringGrid 範例

### 簡單作法:
+ 1.1 拖拉一個「Dialogs>TOpenDialog」到「Form1」中。預設名稱會是「OpenDialog1」。
+ 1.2 拖拉一個「Additional>TStringGrid」到「Form1」中。預設名稱會是「StringGrid1」。
+ 1.3 拖拉一個「Standard>TMemo」到「Form1」中。預設名稱會是「Memo1」，修改「Name」為「Log_Memo」
+ 1.4 拖拉一個「Standard>TMainMenu」到「Form1」中。預設名稱會是「MainMenu1」。
+  點兩下元件，進入編輯模式，建立第一層第一個選單「開啟檔案」，「Caption」設為「開啟檔案(&O)」，「Name」設為「MainMenu1_1」。
```pascal
procedure TForm1.MenuItem1Click(Sender: TObject);
var
  all_csv_TStringList: TStringList;
  one_line_csv_TStringList: TStringList;
  y: Integer;
  x: Integer;
begin
  // OpenDialog1 的初始資料夾路徑
  // [注意] 在 Delphi 中，通常使用 ChangeDir 或直接依賴作業系統的記憶功能，
  // 設置 InitialDir 也可以，但 ExtractFileDir(Application.ExeName) 在 VCL 中更常用。
  OpenDialog1.InitialDir := ExtractFileDir(Application.ExeName) + PathDelim; // 使用 PathDelim 確保跨平台相容性

  // OpenDialog1 下拉選單過濾檔名
  OpenDialog1.Filter := 'R2MS_Lite Raw CSV (*.csv)|*.csv';

  // 呼叫 OpenDialog1
  if OpenDialog1.Execute then
  begin
    // 展示檔案名稱
    Log_Memo.Lines.Add('選擇檔案名稱 = ' + OpenDialog1.FileName);

    // 宣告 TStringList
    all_csv_TStringList := TStringList.Create;
    one_line_csv_TStringList := TStringList.Create;

    try
      // ----------------------------------------------------------------------
      // 利用 TStringList 讀取 CSV 檔案到 StringGrid1 元件中

      // 讀取檔案到物件中
      all_csv_TStringList.LoadFromFile(OpenDialog1.FileName);
      Log_Memo.Lines.Add('檔案行數 = ' + IntToStr(all_csv_TStringList.Count));

      // 調整 StringGrid1 的 RowCount (Y方向行數)。+1 是為了增加一個表格編號行。
      StringGrid1.RowCount := all_csv_TStringList.Count + 1;

      // 逐行解析
      for y := 0 to all_csv_TStringList.Count - 1 do
      begin
        // 把一行逗號分隔的文字放入物件中，使其依照逗號分隔拆分
        // [重要] TStringList 的 Delimiter 和 DelimitedText 屬性用於 CSV 解析
        one_line_csv_TStringList.Delimiter := ',';
        one_line_csv_TStringList.StrictDelimiter := True; // 確保只使用 ',' 分隔
        one_line_csv_TStringList.DelimitedText := all_csv_TStringList.Strings[y];

        // 檢查並設置 ColCount
        if y = 0 then
        begin
          Log_Memo.Lines.Add('每行逗號分隔項目數量 = ' + IntToStr(one_line_csv_TStringList.Count));
          // 調整 StringGrid1 的 ColCount (X方向列數)。+1 是為了增加一個表格編號列。
          StringGrid1.ColCount := one_line_csv_TStringList.Count + 1;
        end
        else
        begin
          if StringGrid1.ColCount <> (one_line_csv_TStringList.Count + 1) then
          begin
            Log_Memo.Lines.Add('本行逗號分隔項目數量與前一行不同!');
          end;
        end;

        // 填入 X 方向表格編號 (標題列) - 僅在第 y=0 行
        if y = 0 then
        begin
          for x := 0 to one_line_csv_TStringList.Count - 1 do
          begin
            StringGrid1.Cells[x + 1, y] := IntToStr(x + 1);
          end;
        end;

        // 填入 Y 方向表格編號 (最左邊列)
        StringGrid1.Cells[0, y + 1] := IntToStr(y + 1);

        // 填入內容
        for x := 0 to one_line_csv_TStringList.Count - 1 do
        begin
          StringGrid1.Cells[x + 1, y + 1] := one_line_csv_TStringList.Strings[x];
        end;
      end;
      // ----------------------------------------------------------------------

    finally
      // 釋放 TStringList
      all_csv_TStringList.Free;
      one_line_csv_TStringList.Free;
    end;
  end
  else
  begin
    // 使用者取消
    MessageDlg('使用者取消!', mtError, [mbOK], 0); // Delphi 中建議使用 MessageDlg
    Exit; // 使用 Exit 替代 return
  end;
end;
```
