# R2MS_Lite_ERTMaker 開發筆記
+ 作者: HsiupoYeh  
+ 更新日期: 2026-10-29
+ 從 **R2MS_Lite_ERTMaker_v1.0(v20260427a).md** 開始，修正功能。

## 開發環境  
+ 使用「Win10_64_CHT_Lazarus4.2_v20250915a.ova」。**匯入為「Win10_Lazarus4.2_R2MS_Lite_ERTMaker_v20251105a.ova」。**
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
+ 需要使用外部python程式
  + 使用Lite_PyGimli_Env (pygimli==1.5.4)
    + https://github.com/cgrgncu/pyGimli_dev/releases/tag/Lite_PyGimli_Env
  + 將目錄規定為以下結構
  ```
  ...\R2MS_Lite_ERTMaker.exe
  ...\ERTMaker_CreateAndModifyMesh_v20260112a.cpython-312.pyc
  ...\ERTMaker_Inversion2D_v20261029a.cpython-312.pyc
  ...\ERTMaker_SimulateForTimeSeries_v20251031b.cpython-312.pyc
  ...\ERTMaker_SimulateForTimeSeries_v20251101b.cpython-312.pyc
  ...\ERTMaker_v299ScsvToUrf_v20261028a.cpython-312.pyc
  ...\Input_ERTMaker_CreateAndModifyMesh\
  ...\Input_ERTMaker_SimulateForTimeSeries\
  ...\Input_ERTMaker_SimulateForTimeSeries\Configs\
  ...\Input_ERTMaker_SimulateForTimeSeries\Configs\... (許多放電檔案)
  ...\Input_ERTMaker_v299ScsvToUrf\
  ...\Input_ERTMaker_Inversion2D\
  ...\PythonEnv\
  ...\PythonEnv\python.exe
  ...\PythonEnv\... (許多其他python所需檔案)
  ```
  + 準備VC檔案，沒有安裝過散發套件的電腦會需要。
  ```
  ...\PythonEnv\vcruntime140.dll
  ...\PythonEnv\vcruntime140_1.dll
  ```

## 開發紀錄(寫功能部分)
### 版本
+ 1.1選Form，去編輯FormCreate，修改部分:
```pascal
procedure TForm1.FormCreate(Sender: TObject);
begin
  //--------------------------------------------------------------------------
  // 在這裡對全域變數進行初始化
  version_str := 'v1.0(v20261029a)';
  Current_Folder_Path := ExtractFilePath(Application.ExeName);
  //--------------------------------------------------------------------------
  //--------------------------------------------------------------------------
  // 表單標題追加版本
  Form1.Caption := Form1.Caption + ' ' + version_str;
  //--------------------------------------------------------------------------
end;        
```
### 功能 

+ 2.1 修改「CreateMeshSettingsDefaultJson_Memo」的「Lines」:
```
{
"CreateAndModifyMeshSettings_Version":"v20251031a",
"CreateAndModifyMeshSettings_Author":"HsiupoYeh",
"TempFile01_BasicMeshPNG_FileName_Readme":"不論Windows或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"TempFile01_BasicMeshPNG_FileName":"Temp/CreateMesh_01_BasicMesh.png",
"TempFile02_BasicMeshVTK_FileName_Readme":"不論Windows或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"TempFile02_BasicMeshVTK_FileName":"Temp/CreateMesh_01_BasicMesh.vtk",
"TempFile03_BasicMeshPNG_FileName_Readme":"不論Windows或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"TempFile03_BasicMeshPNG_FileName":"Temp/CreateMesh_02_BasicMeshSetMarker1.png",
"TempFile04_BasicMeshVTK_FileName_Readme":"不論Windows或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"TempFile04_BasicMeshVTK_FileName":"Temp/CreateMesh_02_BasicMeshSetMarker1.vtk",
"TempFile05_BasicMeshPNG_FileName_Readme":"不論Windows或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"TempFile05_BasicMeshPNG_FileName":"Temp/CreateMesh_03_BasicMeshSetMarker2.png",
"TempFile06_BasicMeshVTK_FileName_Readme":"不論Windows或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"TempFile06_BasicMeshVTK_FileName":"Temp/CreateMesh_03_BasicMeshSetMarker2.vtk",
"TempFile07_BasicMeshPNG_FileName_Readme":"不論Windows或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"TempFile07_BasicMeshPNG_FileName":"Temp/CreateMesh_04_BasicMeshSetBCMarker1.png",
"TempFile08_BasicMeshVTK_FileName_Readme":"不論Windows或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"TempFile08_BasicMeshVTK_FileName":"Temp/CreateMesh_04_BasicMeshSetBCMarker1.vtk",
"TempFile09_BasicMeshPNG_FileName_Readme":"不論Windows或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"TempFile09_BasicMeshPNG_FileName":"Temp/CreateMesh_05_BasicMeshSetBCMarker2.png",
"TempFile10_BasicMeshVTK_FileName_Readme":"不論Windows或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"TempFile10_BasicMeshVTK_FileName":"Temp/CreateMesh_05_BasicMeshSetBCMarker2.vtk",
"TempFile11_BasicMeshPNG_FileName_Readme":"不論Windows或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"TempFile11_BasicMeshPNG_FileName":"Temp/CreateMesh_06_BasicMeshSetTopo.png",
"TempFile12_BasicMeshPNG_FileName_Readme":"不論Windows或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"TempFile12_BasicMeshPNG_FileName":"Temp/CreateMesh_07_BasicMeshSetTopoRes.png",
"TempFile13_BasicMeshVTK_FileName_Readme":"不論Windows或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"TempFile13_BasicMeshVTK_FileName":"Temp/CreateMesh_07_BasicMeshSetTopoRes.vtk",
"OutputFile01_MeshBCMarkersJSON_FileName_Readme":"不論Windows或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"OutputFile01_MeshBCMarkersJSON_FileName":"Output_ERTMaker_CreateAndModifyMesh/Demo_SyntheticModelBCMarkers.json",
"OutputFile02_MeshBCMarkersVTK_FileName_Readme":"不論Windows或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"OutputFile02_MeshBCMarkersVTK_FileName":"Output_ERTMaker_CreateAndModifyMesh/Demo_SyntheticModel.vtk",
"OutputFile03_GEO_FileName_Readme":"不論Windows或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"OutputFile03_GEO_FileName":"Output_ERTMaker_CreateAndModifyMesh/Demo_SyntheticModel.geo",
"OutputFile04_TRN_FileName_Readme":"不論Windows或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"OutputFile04_TRN_FileName":"Output_ERTMaker_CreateAndModifyMesh/Demo_SyntheticModel.trn",
"OutputFile05_OHM_FileName_Readme":"不論Windows或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"OutputFile05_OHM_FileName":"Output_ERTMaker_CreateAndModifyMesh/Demo_SyntheticModel.ohm",
"OutputFile06_BasicMeshPNG_FileName_Readme":"不論Windows或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"OutputFile06_BasicMeshPNG_FileName":"Output_ERTMaker_CreateAndModifyMesh/Demo_SyntheticModel.png",
"OutputFile06_BasicMeshPNG_DPI_Readme":"圖片輸出的DPI，建議:100。",
"OutputFile06_BasicMeshPNG_DPI":100,
"OutputFile06_BasicMeshPNG_Width_Readme":"圖片輸出的寬度，建議:1280。",
"OutputFile06_BasicMeshPNG_Width":1280,
"OutputFile06_BasicMeshPNG_Height_Readme":"圖片輸出的寬度，建議:720。",
"OutputFile06_BasicMeshPNG_Height":720,
"OutputFile06_BasicMeshPNG_Title_Readme":"圖片的標題。",
"OutputFile06_BasicMeshPNG_Title":"XP1 Synthetic Model",
"OutputFile06_BasicMeshPNG_ColorBarResistivityMin_Readme":"電阻率色階下限，線性數值，必為正數。建議:1。",
"OutputFile06_BasicMeshPNG_ColorBarResistivityMin":1,
"OutputFile06_BasicMeshPNG_ColorBarResistivityMax_Readme":"電阻率色階下限，線性數值，必為正數。建議:10000。",
"OutputFile06_BasicMeshPNG_ColorBarResistivityMax":10000,
"StudyAreaMesh_GridStyle_Readme":"研究區域的網格類型。目前只支援「Rectangle」，就是指以矩形為基礎的網格類型。",
"StudyAreaMesh_GridStyle":"Rectangle",
"StudyAreaMesh_RectangleGrid_LayerCount_Readme":"研究區域的網格層數。就是指Z方向的地層數量。此值必須是正整數。",
"StudyAreaMesh_RectangleGrid_LayerCount":30,
"StudyAreaMesh_RectangleGrid_LayerThicknessFacfor_Readme":"研究區域的地層厚度控制因子。此值必須是正數。1表示厚度不變化，0.9表示厚度變小為原本的90%，1.1表示厚度變大為原本的110%。",
"StudyAreaMesh_RectangleGrid_LayerThicknessFacfor":1,
"StudyAreaMesh_RectangleGrid_FirstLayerThickness_Readme":"研究區域的第一層厚度。單位:[m]。此值必須是正數。",
"StudyAreaMesh_RectangleGrid_FirstLayerThickness":1,
"StudyAreaMesh_GridSurfaceNode_Readme":"逐個點在X方向上應為遞增的數字，不可填入null；在Z方向上填入對應高程，允許部分缺值(填入null)，將進行內插；Electrode_Index從1開始依序增加。若設為0或負值表示該位置不設置電極",
"StudyAreaMesh_GridSurfaceNode_Readme2":"X[m],Z[m],Electrode_Index",
"StudyAreaMesh_GridSurfaceNode":[
],
"PaddingMesh_GridStyle_Readme":"附加區域的網格類型。目前只支援「Rectangle」，就是指以矩形為基礎的網格類型。",
"PaddingMesh_GridStyle":"Rectangle",
"PaddingMesh_RectangleGrid_LeftPaddingLayerCount_Readme":"附加區域左側要追加的網格層數。就是指-X方向追加的地層數量。此值必須是0或是正整數。",
"PaddingMesh_RectangleGrid_LeftPaddingLayerCount":5,
"PaddingMesh_RectangleGrid_LeftPaddingLayerWidthFacfor_Readme":"附加區域左側的地層寬度控制因子。此值必須是正數。1表示寬度不變化，0.9表示寬度變小為原本的90%，1.1表示寬度變大為原本的110%。",
"PaddingMesh_RectangleGrid_LeftPaddingLayerWidthFacfor":1.2,
"PaddingMesh_RectangleGrid_FirstLeftPaddingLayerWidth_Readme":"附加區域左側的第一層寬度。單位:[m]。此值小於等於0的時候，表示自動使用最靠近的一層搭配厚度因子計算出第一層寬度，否則直接使用指定數值為第一層寬度。",
"PaddingMesh_RectangleGrid_FirstLeftPaddingLayerWidth":-1,
"PaddingMesh_RectangleGrid_RightPaddingLayerCount_Readme":"附加區域右側要追加的網格層數。就是指+X方向追加的地層數量。此值必須是0或是正整數。",
"PaddingMesh_RectangleGrid_RightPaddingLayerCount":5,
"PaddingMesh_RectangleGrid_RightPaddingLayerWidthFacfor_Readme":"附加區域右側的地層寬度控制因子。此值必須是正數。1表示寬度不變化，0.9表示寬度變小為原本的90%，1.1表示寬度變大為原本的110%。",
"PaddingMesh_RectangleGrid_RightPaddingLayerWidthFacfor":1.2,
"PaddingMesh_RectangleGrid_FirstRightPaddingLayerWidth_Readme":"附加區域右側的第一層寬度。單位:[m]。此值小於等於0的時候，表示自動使用最靠近的一層搭配厚度因子計算出第一層寬度，否則直接使用指定數值為第一層寬度。",
"PaddingMesh_RectangleGrid_FirstRightPaddingLayerWidth":-1,
"PaddingMesh_RectangleGrid_BottomPaddingLayerCount_Readme":"附加區域下方的網格層數。是指Z方向的地層數量。此值必須是0或正整數。",
"PaddingMesh_RectangleGrid_BottomPaddingLayerCount":5,
"PaddingMesh_RectangleGrid_BottomPaddingLayerThicknessFacfor_Readme":"附加區域右側的地層寬度控制因子。此值必須是正數。1表示寬度不變化，0.9表示寬度變小為原本的90%，1.1表示寬度變大為原本的110%。",
"PaddingMesh_RectangleGrid_BottomPaddingLayerThicknessFacfor":1.2,
"PaddingMesh_RectangleGrid_FirstBottomPaddingLayerThickness_Readme":"附加區域的第一層厚度。單位:[m]。此值小於等於0的時候，表示自動使用最靠近的一層搭配厚度因子計算出第一層厚度，否則直接使用指定數值為第一層厚度。",
"PaddingMesh_RectangleGrid_FirstBottomPaddingLayerThickness":-1,
"ModifyMeshResistivity_Readme":"依序填入多則修改範圍，將依照規則順序修改模型。",
"ModifyMeshResistivity_Readme2":"x_min[m],x_max[m],z_min[m],z_max[m],New_Resistivity[ohm-m]",
"ModifyMeshResistivity":[
]
}
```


+ 2.1 修改「TimeSeriesProcessingSettingsDefaultJson_Memo」的「Lines」:
```
{
"v299ScsvToUrf_Version":"v20261028a",
"v299ScsvToUrf_Author":"HsiupoYeh",
"InputFile01_Geo_FileName_Readme":"不論Windows或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"InputFile01_Geo_FileName":"Input_ERTMaker_v299ScsvToUrf/Demo_SyntheticModel.geo",
"InputFile02_Ohm_FileName_Readme":"不論Windows或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"InputFile02_Ohm_FileName":"Input_ERTMaker_v299ScsvToUrf/Demo_SyntheticModel.ohm",
"InputFile03_v299Scsv_FileName_Readme":"不論Windows或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"InputFile03_v299Scsv_FileName":"Input_ERTMaker_v299ScsvToUrf/Demo_SyntheticModel_CurrentFlowLinesAB_0001.v299S.csv",
"InputFile04_Keep_ABMN_FileName_Readme":"不論Windows或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"InputFile04_Keep_ABMN_FileName":"Input_ERTMaker_v299ScsvToUrf/Demo_SyntheticModel.abmn",
"OutputFile01_Urf_All_FileName_Readme":"不論Windows或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"OutputFile01_Urf_All_FileName":"Output_ERTMaker_v299ScsvToUrf/Demo_SyntheticModel.v299S.urf",
"OutputFile02_Urf_All_SNR_FileName_Readme":"不論Windows或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"OutputFile02_Urf_All_SNR_FileName":"Output_ERTMaker_v299ScsvToUrf/Demo_SyntheticModel.v299S.SNR.urf",
"OutputFile03_Ohm_All_FileName_Readme":"不論Windows或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"OutputFile03_Ohm_All_FileName":"Output_ERTMaker_v299ScsvToUrf/Demo_SyntheticModel.v299S.ohm",
"OutputFile04_PNG_All_MainFileName_Readme":"不論Windows或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"OutputFile04_PNG_All_MainFileName":"Output_ERTMaker_v299ScsvToUrf/Demo_SyntheticModel.v299S",
"OutputFile04_PNG_All_Enable_Readme":"是否啟用輸出PNG。若要啟用請填入字串Yes，若不啟用請填入字串No。輸出PNG每張圖約耗時?",
"OutputFile04_PNG_All_Enable":"No",
"OutputFile05_Urf_WS_FileName_Readme":"不論Windows或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"OutputFile05_Urf_WS_FileName":"Output_ERTMaker_v299ScsvToUrf/Demo_SyntheticModel.v299S.WS.urf",
"OutputFile06_Urf_WS_SNR_FileName_Readme":"不論Windows或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"OutputFile06_Urf_WS_SNR_FileName":"Output_ERTMaker_v299ScsvToUrf/Demo_SyntheticModel.v299S.WS.SNR.urf",
"OutputFile07_Ohm_WS_FileName_Readme":"不論Windows或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"OutputFile07_Ohm_WS_FileName":"Output_ERTMaker_v299ScsvToUrf/Demo_SyntheticModel.v299S.WS.ohm",
"OutputFile08_PNG_WS_MainFileName_Readme":"不論Windows或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"OutputFile08_PNG_WS_MainFileName":"Output_ERTMaker_v299ScsvToUrf/Demo_SyntheticModel.v299S.WS",
"OutputFile08_PNG_WS_Enable_Readme":"是否啟用輸出PNG。若要啟用請填入字串Yes，若不啟用請填入字串No。輸出PNG每張圖約耗時?",
"OutputFile08_PNG_WS_Enable":"No",
"OutputFile09_Urf_MGD_FileName_Readme":"不論Windows或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"OutputFile09_Urf_MGD_FileName":"Output_ERTMaker_v299ScsvToUrf/Demo_SyntheticModel.v299S.MGD.urf",
"OutputFile10_Urf_MGD_SNR_FileName_Readme":"不論Windows或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"OutputFile10_Urf_MGD_SNR_FileName":"Output_ERTMaker_v299ScsvToUrf/Demo_SyntheticModel.v299S.MGD.SNR.urf",
"OutputFile11_Ohm_MGD_FileName_Readme":"不論Windows或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"OutputFile11_Ohm_MGD_FileName":"Output_ERTMaker_v299ScsvToUrf/Demo_SyntheticModel.v299S.MGD.ohm",
"OutputFile12_PNG_MGD_MainFileName_Readme":"不論Windows或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"OutputFile12_PNG_MGD_MainFileName":"Output_ERTMaker_v299ScsvToUrf/Demo_SyntheticModel.v299S.MGD",
"OutputFile12_PNG_MGD_Enable_Readme":"是否啟用輸出PNG。若要啟用請填入字串Yes，若不啟用請填入字串No。輸出PNG每張圖約耗時?",
"OutputFile12_PNG_MGD_Enable":"No",
"OutputFile13_Urf_MPR_FileName_Readme":"不論Windows或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"OutputFile13_Urf_MPR_FileName":"Output_ERTMaker_v299ScsvToUrf/Demo_SyntheticModel.v299S.MPR.urf",
"OutputFile14_Urf_MPR_SNR_FileName_Readme":"不論Windows或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"OutputFile14_Urf_MPR_SNR_FileName":"Output_ERTMaker_v299ScsvToUrf/Demo_SyntheticModel.v299S.MPR.SNR.urf",
"OutputFile15_Ohm_MPR_FileName_Readme":"不論Windows或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"OutputFile15_Ohm_MPR_FileName":"Output_ERTMaker_v299ScsvToUrf/Demo_SyntheticModel.v299S.MPR.ohm",
"OutputFile16_PNG_MPR_MainFileName_Readme":"不論Windows或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"OutputFile16_PNG_MPR_MainFileName":"Output_ERTMaker_v299ScsvToUrf/Demo_SyntheticModel.v299S.MPR",
"OutputFile16_PNG_MPR_Enable_Readme":"是否啟用輸出PNG。若要啟用請填入字串Yes，若不啟用請填入字串No。輸出PNG每張圖約耗時?",
"OutputFile16_PNG_MPR_Enable":"No",
"OutputFile17_Urf_GD_FileName_Readme":"不論Windows或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"OutputFile17_Urf_GD_FileName":"Output_ERTMaker_v299ScsvToUrf/Demo_SyntheticModel.v299S.GD.urf",
"OutputFile18_Urf_GD_SNR_FileName_Readme":"不論Windows或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"OutputFile18_Urf_GD_SNR_FileName":"Output_ERTMaker_v299ScsvToUrf/Demo_SyntheticModel.v299S.GD.SNR.urf",
"OutputFile19_Ohm_GD_FileName_Readme":"不論Windows或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"OutputFile19_Ohm_GD_FileName":"Output_ERTMaker_v299ScsvToUrf/Demo_SyntheticModel.v299S.GD.ohm",
"OutputFile20_PNG_GD_MainFileName_Readme":"不論Windows或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"OutputFile20_PNG_GD_MainFileName":"Output_ERTMaker_v299ScsvToUrf/Demo_SyntheticModel.v299S.GD",
"OutputFile20_PNG_GD_Enable_Readme":"是否啟用輸出PNG。若要啟用請填入字串Yes，若不啟用請填入字串No。輸出PNG每張圖約耗時?",
"OutputFile20_PNG_GD_Enable":"No",
"Select_From_Readme":"請填入字串「All」、「WS」、「MGD」、「MPR」或「GD」。",
"Select_From":"All",
"Select_Keep_A_index_Readme":"請填入陣列。例如：「[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64]」",
"Select_Keep_A_index":[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64],
"Select_Keep_B_index_Readme":"請填入陣列。例如：「[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64]」",
"Select_Keep_B_index":[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64],
"Select_Keep_M_index_Readme":"請填入陣列。例如：「[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64]」",
"Select_Keep_M_index":[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64],
"Select_Keep_N_index_Readme":"請填入陣列。例如：「[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64]」",
"Select_Keep_N_index":[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64],
"Select_Keep_ABMN_Enable_Readme":"是否啟用ABMN檔案清單篩選。若要啟用請填入字串Yes，若不啟用請填入字串No。",
"Select_Keep_ABMN_Enable":"No",
"OutputFile21_Urf_Select_FileName_Readme":"不論Windows或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"OutputFile21_Urf_Select_FileName":"Output_ERTMaker_v299ScsvToUrf/Demo_SyntheticModel.v299S.Select.urf",
"OutputFile22_Urf_Select_SNR_FileName_Readme":"不論Windows或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"OutputFile22_Urf_Select_SNR_FileName":"Output_ERTMaker_v299ScsvToUrf/Demo_SyntheticModel.v299S.Select.SNR.urf",
"OutputFile23_Ohm_Select_FileName_Readme":"不論Windows或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"OutputFile23_Ohm_Select_FileName":"Output_ERTMaker_v299ScsvToUrf/Demo_SyntheticModel.v299S.Select.ohm",
"OutputFile24_PNG_Select_MainFileName_Readme":"不論Windows或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"OutputFile24_PNG_Select_MainFileName":"Output_ERTMaker_v299ScsvToUrf/Demo_SyntheticModel.v299S.Select",
"OutputFile24_PNG_Select_Enable_Readme":"是否啟用輸出PNG。若要啟用請填入字串Yes，若不啟用請填入字串No。輸出PNG每張圖約耗時?",
"OutputFile24_PNG_Select_Enable":"No"
}
```
+ 2.2 修改「InversionModelingSettingsDefaultJson_Memo」的「Lines」:
```
procedure TForm1.InversionResultPreviewShowBoth_ToolButtonClick(Sender: TObject
  );
begin
  InversionResultPreviewShowImage_ToolButton.Down:=False;
  InversionResultPreviewImage_GroupBox.Visible:=True;
  InversionResultPreviewImage_GroupBox.AnchorSideRight.Control := InversionResultPreview_Panel;
  InversionResultPreviewImage_GroupBox.AnchorSideRight.Side := asrLeft;
  InversionResultPreviewShowGrid_ToolButton.Down:=False;
  InversionResultPreviewGrid_GroupBox.Visible:=True;
  InversionResultPreviewGrid_GroupBox.AnchorSideLeft.Control := InversionResultPreview_Panel;
  InversionResultPreviewGrid_GroupBox.AnchorSideLeft.Side := asrRight;
  InversionResultPreviewShowBoth_ToolButton.Down:=True;
  Form1.FormResize(Form1);
end;
```
+ 2.3 去修改2594行的程式碼:
  + 由這個:
  ```
      if not FileExists('ERTMaker_Inversion2D_v20260419a.cpython-312.pyc') then
  ```
  + 改成:
  ```
      if not FileExists('ERTMaker_Inversion2D_v20261029a.cpython-312.pyc') then
  ```
+ 2.4 去修改2608行的程式碼:
  + 由這個:
  ```
     InversionModeling_AsyncProcess.Parameters.Add('set PYTHONIOENCODING=utf-8 && .\PythonEnv\Python.exe -u ERTMaker_Inversion2D_v20260419a.cpython-312.pyc');
  ```
  + 改成:
  ```
     InversionModeling_AsyncProcess.Parameters.Add('set PYTHONIOENCODING=utf-8 && .\PythonEnv\Python.exe -u ERTMaker_Inversion2D_v20261029a.cpython-312.pyc');
  ```
+ 2.5 去修改4243行的程式碼:
  + 由這個:
  ```
    if not FileExists('ERTMaker_v299ScsvToUrf_v20260217a.cpython-312.pyc') then 
  ```
  + 改成:
  ```
    if not FileExists('ERTMaker_v299ScsvToUrf_v20261028a.cpython-312.pyc') then 
  ```
+ 2.6 去修改4258行的程式碼:
  + 由這個:
  ```
    TimeSeriesProcessing_AsyncProcess.Parameters.Add('.\PythonEnv\Python.exe -u ERTMaker_v299ScsvToUrf_v20260217a.cpython-312.pyc'); 
  ```
  + 改成:
  ```
    TimeSeriesProcessing_AsyncProcess.Parameters.Add('.\PythonEnv\Python.exe -u ERTMaker_v299ScsvToUrf_v20261028a.cpython-312.pyc'); 
  ```
+ 2.7 去修改2397行的程式碼:
  + 由這個:
  ```
      InversionModelingSettingsNowJson_Memo.Lines.Strings[77-1] := ('"Inversion_Setting03_Lamda":'+InversionModelingInvSettings_Lambda_Edit.Text+',');
  ```
  + 改成:
  ```
      InversionModelingSettingsNowJson_Memo.Lines.Strings[77-1] := ('"Inversion_Setting03_Lambda":'+InversionModelingInvSettings_Lambda_Edit.Text+',');
  ```
+ 2.8 去修改2567行的程式碼:
  + 由這個:
  ```
          InversionModelingSettingsNowJson_Memo.Lines.Strings[109-1] := ('"Output_Inversion_YMax":'+temp_str4+',');
  ```
  + 改成:
  ```
          InversionModelingSettingsNowJson_Memo.Lines.Strings[109-1] := ('"Output_Inversion_YMax":'+temp_str4);
  ```
+ 2.9 去修改4198行的程式碼:
  + 由這個:
  ```
      TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[83-1] := '"OutputFile24_PNG_Select_Enable":"Yes",';
  ```
  + 改成:
  ```
      TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[83-1] := '"OutputFile24_PNG_Select_Enable":"Yes"';
  ```
+ 2.10 修改「InversionModelingOutputSettings_MainName_GroupBox」的「Hint」:
```
測線名稱。將作為檔案命名用途。
```
+ 2.11 修改「InversionModelingInvSettings_Lambda_GroupBox」的「Hint」:
```
逆推參數中的Lambda為「正規化參數(Regularization Parameter)」，或通俗地稱為「平滑係數(Smoothing Factor)」。其值越大表示目標函數中資料項影響降低、模型項影響增加，通常導致模型更平滑但資料擬合較差。反之，其值越小，模型主要受資料項影響，通常會產生更粗糙(更不平滑)的模型。
預設值=[20]。請填入陣列，限制陣列中元素僅可填入正數，例如:「[100,33.33,1]」。
依序選擇陣列中的Lambda進行逆推，當該Lambda的逆推滿足終止條件時，保留該模型作為起始模型接關使用下一個Lambda繼續逆推，直到所有Lambda都執行完畢並獲得最終模型為止。
```

### 更新紀錄
+ 3.1 選「UpdateLog_Memo」，去編輯「Lines」
```
軟體名稱: R2MS_Lite_ERTMaker
作者: HsiupoYeh
--
v20261029a
修正錯字與錯誤描述。
更新Python外部程式版本及對應參數檔案內容(參數名稱修改)，並修補部分錯誤。
--
v20260427a
修正不同DPI圖片縮放錯誤的問題。
--
v20260423a
完整功能版 (包含順推模擬:建立模型網格、模擬時間序列蒐集、時間序列資料處理以及逆推模擬)
**建議作業系統: Windows 10
**ERTMaker主版本號: v1.0
**ERTMaker圖形化介面版本號: v20260423a
**Python版本: 3.12.7 (64-bit)
**PyGimli版本: v1.5.4
***已知問題: 逆推運算的平行運算預設不使用所有核心運作(會保留兩個核心)。但在Intel第12代的CPU(i7-1260P)環境下設定失效。這可能會使作業系統極度忙碌，導致網路等功能無回應。這並非軟體設計問題，而是這版本的CPU調度邏輯不良。
***支援快捷鍵: 「Ctrl+方向鍵」來快速瀏覽歷次逆推結果。
--
v20251105a
初版 (僅設計排版)
--
v20251105a
初版 (僅設計排版)
```
