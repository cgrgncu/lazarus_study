# README

### 特色
+ https://wiki.freepascal.org/TImage


### 範例

```pascal
  Image1.Picture.Bitmap.Width := 1000;
  Image1.Picture.Bitmap.Height := 200;
  Image1.Picture.Bitmap.Canvas.Brush.Color := clRed;
  Image1.Picture.Bitmap.Canvas.FillRect(0, 0, Image1.Picture.Bitmap.Width, Image1.Picture.Bitmap.Height);
  Image1.Picture.Bitmap.Canvas.TextOut(10, 10, '這是詳細的繪圖步驟!');
  Image1.Picture.SaveToFile('44.png');
```
