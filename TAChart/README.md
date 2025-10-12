# TAChart 簡介


### 元件清單
+ TChart:
  + 右鍵按編輯建立一個新的Chart1LineSeries1
  + 放TRandomChartSource(以後改真的數據)
  + TRandomChartSource設定數量100，X最小0最大100，Y最小0最大100。
  + Chart1LineSeries1綁定TRandomChartSource這樣就會看到一堆線
  + Chart1LineSeries1找到SeriesColor設定紅色
  + Chart1LineSeries1找到Pointer>Brusg>Color變成紅色
  + Chart1LineSeries1找到Pointer>Pen>Style變成圓形
  + Chart1LineSeries1找到Pointer>Pen>Color變成黑色
  + 如果有需要，找個位置自己放上TChartNavScrollBar來控制範圍拖拉。
  + 設定圖表標題(Title)，並設為可見
  + 設定X軸標題(AxisList>Bottom>Title>Caption)，並設為可見
  + 設定Y軸標題(AxisList>Left>Title>Caption)，並設為可見
+ TListChartSource: 底層數據源 (Base Data Source) 元件
+ TSortedChartSource: 中介數據源 (Intermediary Data Source) 元件。
+ TRandomChartSource: 底層數據源 (Base Data Source) 元件
+ TUserDefinedChartSource: 底層數據源 (Base Data Source) 元件
+ TCalculatedChartSource: 中介數據源 (Intermediary Data Source) 元件。
+ TDbChartSource: 資料庫相關的底層數據源 (Base Data Source) 元件
+ TChartToolset: 控制器元件
  + 不要設定的話，內建有拖個框來放大，等效於ChartToolset1ZoomDragTool1然後修改Shift為`[ssLeft]`
  + ChartToolset1ZoomMouseWheelTool1通常設定ZoomFactor為`1.1`，就會有很好的使用體驗。
+ TChartAxisTransformations: 細節控制元件
+ TChartStyles: 細節控制元件
+ TChartLegendPanel: 獨立的客製化圖說元件
+ TChartNavScrollBar: 資料相關的捲動軸元件
+ TChartNavPanel : 現成的面板控制器元件
+ TIntervalChartSource: 中介數據源 (Intermediary Data Source) 元件。
+ TDateTimeIntervalChartSource: 時間日期相關的中介數據源 (Intermediary Data Source) 元件。
+ TChartListbox: 互動式UI與圖說控制元件，多選多。
+ TChartExtentLink: 圖表範圍連動元件
+ TChartLiveView: 仿示波器的圖表展示元件
+ TChartImageList: 自定義點符號繪製器。
+ TChartComboBox: 互動式UI與圖說控制元件，多選一。
