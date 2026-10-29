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
+ 2.2 修改「ForwardModelingSettingsDefaultJson_Memo」的「Lines」:
```
{
"SimulateForTimeSeriesSettings_Version":"v20251031a",
"SimulateForTimeSeriesSettings_Author":"HsiupoYeh",
"InputFile01_MeshVTK_FileName_Readme":"不論Windows或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"InputFile01_MeshVTK_FileName":"Input_ERTMaker_SimulateForTimeSeries/XP1_SyntheticModel.vtk",
"InputFile02_MeshBCMarkersJSON_FileName_Readme":"不論Windows或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"InputFile02_MeshBCMarkersJSON_FileName":"Input_ERTMaker_SimulateForTimeSeries/XP1_SyntheticModelBCMarkers.json",
"InputFile03_OHM_FileName_Readme":"不論Windows或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"InputFile03_OHM_FileName":"Input_ERTMaker_SimulateForTimeSeries/XP1_SyntheticModel.ohm",
"InputFile04_CurrentMode_FileName_Readme":"不論Windows或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"InputFile04_CurrentMode_FileName":"Input_ERTMaker_SimulateForTimeSeries/Configs/v299_S64NCurrentMode/Current Mode.csv",
"MeshPNG_DPI_Readme":"圖片輸出的DPI，建議:100。",
"MeshPNG_DPI":100,
"MeshPNG_Width_Readme":"圖片輸出的寬度，建議:1280。",
"MeshPNG_Width":1280,
"MeshPNG_Height_Readme":"圖片輸出的寬度，建議:720。",
"MeshPNG_Height":720,
"MeshPNG_Title_Readme":"圖片的標題。",
"MeshPNG_Title":"XP1 Synthetic Model",
"MeshPNG_ColorBarResistivityMin_Readme":"電阻率色階下限，線性數值，必為正數。建議:1。",
"MeshPNG_ColorBarResistivityMin":1,
"MeshPNG_ColorBarResistivityMax_Readme":"電阻率色階下限，線性數值，必為正數。建議:10000。",
"MeshPNG_ColorBarResistivityMax":10000,
"OutputFolderPath_Readme":"不論Windows或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。務必要使用「/」在結尾，表示資料夾。",
"OutputFolderPath":"Output_ERTMaker_SimulateForTimeSeries/",
"Output_MainFileName_Readme":"會產生很多檔案，這些檔案的主要檔名。",
"Output_MainFileName":"XP1_SyntheticModel",
"Output_PNG_Enable_Readme":"是否啟用輸出PNG。若要啟用請填入字串Yes，若不啟用請填入字串No。輸出PNG每張圖約耗時",
"Output_PNG_Enable":"Yes",
"ElectrodeIndexAB_TxVoltageMax_Readme":"AB電極的發射器最大電壓，單位[V]",
"ElectrodeIndexAB_TxVoltageMax":400,
"ElectrodeIndexAB_TxCurrentMax_Readme":"AB電極的發射器最大電流，單位[A]",
"ElectrodeIndexAB_TxCurrentMax":0.5,
"ElectrodeIndexAB_Resistance_Readme":"AB電極的接地電阻，單位[Ohm]",
"ElectrodeIndexAB_Resistance":1000
}

```
+ 2.3 修改「TimeSeriesProcessingSettingsDefaultJson_Memo」的「Lines」:
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
+ 2.4 修改「InversionModelingSettingsDefaultJson_Memo」的「Lines」:
```
{
"Inversion2D_Version":"v20261029a",
"Inversion2D_Author":"HsiupoYeh",
"InputFile01_Ohm_FileName_Readme":"不論Windows或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"InputFile01_Ohm_FileName":"Input_ERTMaker_Inversion2D/XP2_R01_L01_I01_CalData.dat",
"Mesh_Setting01_AutoMesh_Enable_Readme":"逆推參數-使用自動三角網格。若要啟用請填入字串Yes，若不啟用請填入字串No。若啟用則使用自動產生三角網格，不會讀取VTK檔案與JSON檔案。",
"Mesh_Setting01_AutoMesh_Enable":"Yes",
"Mesh_Setting02_AutoMesh_quality_Readme":"逆推參數-設定自動三角網格的網格品質。控制不要讓三角形太扁長。預設值:32，建議填入32.0~34.0之間的浮點數。此設定只有啟用自動網格才會有效。",
"Mesh_Setting02_AutoMesh_quality":33,
"Mesh_Setting03_AutoMesh_paraDepth_Readme":"逆推參數-設定自動三角網格的研究區域內最大深度。預設值:-1，單位:[m]，設定小於0的數字會自動設定為測線電極X方向長度的0.4倍。此設定只有啟用自動網格才會有效。",
"Mesh_Setting03_AutoMesh_paraDepth":-1,
"Mesh_Setting04_AutoMesh_boundary_Readme":"逆推參數-設定自動三角網格的外邊界倍數。預設值:-1，建議填入4~10之間的整數，設定小於4的數字仍會強制設定為4。此設定只有啟用自動網格才會有效。",
"Mesh_Setting04_AutoMesh_boundary":-1,
"Mesh_Setting05_AutoMesh_paraMaxCellSize_Readme":"逆推參數-設定自動三角網格的網格面積上限。單位:[m^2]。預設值:0，表示自動決定。此設定只有啟用自動網格才會有效。",
"Mesh_Setting05_AutoMesh_paraMaxCellSize":0,
"Mesh_Setting06_AutoMesh_addNodes_Readme":"逆推參數-設定自動三角網格的網格地表電極之間的節點數量。預設值:1。限制填入正整數。此值填「1」時無效果，改由paraDX決定增加的方式；填其他正整數則線性均勻增加兩個電極之間的節點數量。此設定只有啟用自動網格才會有效。",
"Mesh_Setting06_AutoMesh_addNodes":1,
"Mesh_Setting07_AutoMesh_paraDX_Readme":"逆推參數-設定自動三角網格的網格地表電極之間的節點數量。預設值:1。限制填入浮點數。僅有addNodes小於等於1的時候有效。此值大於等於0.5時，等於在兩個電極正中間增加一個節點。小於0.5的時候，在每個電極的左右兩邊各增加一個節點，距離是電極間距乘以此浮點數。此設定只有啟用自動網格才會有效。",
"Mesh_Setting07_AutoMesh_paraDX":1,
"InputFile02_MeshVTK_FileName_Readme":"不論Windows或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"InputFile02_MeshVTK_FileName":"Input_ERTMaker_Inversion2D/InputFile02_MeshVTK_FileName.vtk",
"InputFile03_MeshBCMarkersJSON_FileName_Readme":"不論Windows或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"InputFile03_MeshBCMarkersJSON_FileName":"Input_ERTMaker_Inversion2D/InputFile03_MeshBCMarkersJSON_FileName.json",
"Data_Setting00_RecalculateDataMode_Readme":"逆推參數-重新計算觀測資料方式。預設值:「r」。可填選項:「r」、「rhoa」。",
"Data_Setting00_RecalculateDataMode":"r",
"Data_Setting01_Relative_Error_Percentage_Readme":"逆推參數-資料預估相對誤差百分比，單位:[%]。用於建構目標函數，野外蒐集的資料品質好的推薦使用3[%]，若品質不好則考慮增加。",
"Data_Setting01_Relative_Error_Percentage":3,
"Data_Setting02_GeometricFactorForInversion_Readme":"逆推參數-逆推內部採用的幾何因子計算。若要使用網格綁定的數值解(K_Mesh)請填入字串「K_Mesh」。若要使用解析解公式(K0_Mesh)請填入字串「K0_Mesh」，建議僅有水平地層可以填「K0_Mesh」。",
"Data_Setting02_GeometricFactorForInversion":"K_Mesh",
"Data_Setting03_GeometricFactorForRecalculateData_Readme":"逆推參數-觀測資料轉換視電阻率採用的理想幾何因子計算。若要使用自訂理想網格計算數值解(K_Ideal)請填入字串「Ideal_Mesh」，此時後方要填入網格設定。若要使用前面的網格綁定的數值解(K_Mesh)請填入字串「K_Mesh」。若要使用解析解公式(K0_Mesh)請填入字串「K0_Mesh」，建議僅有水平地層可以填「K0_Mesh」。",
"Data_Setting03_GeometricFactorForRecalculateData":"K_Mesh",
"Ideal_Mesh_Setting01_AutoMesh_quality_Readme":"理想幾何因子計算網格-設定三角網格的網格品質。控制不要讓三角形太扁長。預設值:32，建議填入32.0~34.0之間的浮點數。此設定只有啟用自動網格才會有效。",
"Ideal_Mesh_Setting01_AutoMesh_quality":33,
"Ideal_Mesh_Setting02_AutoMesh_paraDepth_Readme":"理想幾何因子計算網格-設定三角網格的研究區域內最大深度。預設值:-1，單位:[m]，設定小於0的數字會自動設定為測線電極X方向長度的0.4倍。此設定只有啟用自動網格才會有效。",
"Ideal_Mesh_Setting02_AutoMesh_paraDepth":-1,
"Ideal_Mesh_Setting03_AutoMesh_boundary_Readme":"理想幾何因子計算網格-設定三角網格的外邊界倍數。預設值:-1，建議填入4~10之間的整數，設定小於4的數字仍會強制設定為4。此設定只有啟用自動網格才會有效。",
"Ideal_Mesh_Setting03_AutoMesh_boundary":-1,
"Ideal_Mesh_Setting04_AutoMesh_paraMaxCellSize_Readme":"理想幾何因子計算網格-設定三角網格的網格面積上限。單位:[m^2]。預設值:0，表示自動決定。此設定只有啟用自動網格才會有效。",
"Ideal_Mesh_Setting04_AutoMesh_paraMaxCellSize":0,
"Ideal_Mesh_Setting05_AutoMesh_addNodes_Readme":"理想幾何因子計算網格-設定三角網格的網格地表電極之間的節點數量。預設值:1。限制填入正整數。此值填「1」時無效果，改由paraDX決定增加的方式；填其他正整數則線性均勻增加兩個電極之間的節點數量。此設定只有啟用自動網格才會有效。",
"Ideal_Mesh_Setting05_AutoMesh_addNodes":1,
"Ideal_Mesh_Setting06_AutoMesh_paraDX_Readme":"理想幾何因子計算網格-設定三角網格的網格地表電極之間的節點數量。預設值:1。限制填入浮點數。僅有addNodes小於等於1的時候有效。此值大於等於0.5時，等於在兩個電極正中間增加一個節點。小於0.5的時候，在每個電極的左右兩邊各增加一個節點，距離是電極間距乘以此浮點數。此設定只有啟用自動網格才會有效。",
"Ideal_Mesh_Setting06_AutoMesh_paraDX":1,
"Data_Setting03_RemoveLowCurrentData_LowerThan_A_Readme":"逆推參數-移除過低電流觀測資料，單位:[A]。建議值:0.01。限制填大於等於0的數字，填「0」表示不移除。注意，全部刪除資料會造成後續異常錯誤。請考量正確的儀器電流情形。",
"Data_Setting03_RemoveLowCurrentData_LowerThan_A":0,
"Data_Setting04_RemoveHighCurrentData_HigherThan_A_Readme":"逆推參數-移除過大電流觀測資料，單位:[A]。建議值:0.5。限制填大於0的數字。注意，全部刪除資料會造成後續異常錯誤。請考量正確的儀器電流情形。",
"Data_Setting04_RemoveHighCurrentData_HigherThan_A":10.0,
"Data_Setting05_RemoveLowVoltageData_LowerThan_V_Readme":"逆推參數-移除過低電壓觀測資料，單位:[V]。建議值:0.01。限制填大於等於0的數字，填「0」表示不移除。注意，全部刪除資料會造成後續異常錯誤。請考量正確的儀器電流情形。",
"Data_Setting05_RemoveLowVoltageData_LowerThan_V":0,
"Data_Setting06_RemoveHighVoltageData_HigherThan_V_Readme":"逆推參數-移除過大電壓觀測資料，單位:[V]。建議值:400。限制填大於0的數字。注意，全部刪除資料會造成後續異常錯誤。請考量正確的儀器電流情形。",
"Data_Setting06_RemoveHighVoltageData_HigherThan_V":999999,
"Data_Setting07_RemoveLowAppResData_LowerThan_OhmM_Readme":"逆推參數-移除過低視電阻率觀測資料，單位:[Ohm-m]。建議值:0.01。限制填大於等於0的數字，填「0」表示不移除。注意，全部刪除資料會造成後續異常錯誤。請考量正確的儀器電流情形。",
"Data_Setting07_RemoveLowAppResData_LowerThan_OhmM":0,
"Data_Setting08_RemoveHighAppResData_HigherThan_OhmM_Readme":"逆推參數-移除過大視電阻率觀測資料，單位:[Ohm-m]。建議值:100000。限制填大於0的數字。注意，全部刪除資料會造成後續異常錯誤。請考量正確的儀器電流情形。",
"Data_Setting08_RemoveHighAppResData_HigherThan_OhmM":999999,
"Data_Setting09_RemoveBadElectrodeData_List_Readme":"逆推參數-移除損毀電極觀測資料。建議值:[]。限制填正整數。注意，全部刪除資料會造成後續異常錯誤。請考量正確的儀器電流情形。",
"Data_Setting09_RemoveBadElectrodeData_List":[],
"Data_Setting10_RemoveLowMeshEffectData_LowerThan_Percent_Readme":"逆推參數-移除過低網格效應觀測資料，單位:[%]。建議值:99。限制填浮點數。注意，全部刪除資料會造成後續異常錯誤。請考量正確的儀器電流情形。",
"Data_Setting10_RemoveLowMeshEffectData_LowerThan_Percent":-999999,
"Data_Setting11_RemoveHighMeshEffectData_HigherThan_Percent_Readme":"逆推參數-移除過高網格效應觀測資料，單位:[%]。建議值:101。限制浮點數。注意，全部刪除資料會造成後續異常錯誤。請考量正確的儀器電流情形。",
"Data_Setting11_RemoveHighMeshEffectData_HigherThan_Percent":999999,
"Data_Setting12_UseFakeData_Enable_Readme":"逆推參數-使用假的觀測資料。建議值:「No」。若要啟用(把觀測資料變成全部都是1[Ohm-m])請填入字串Yes，若不啟用(使用實際資料)請填入字串No。",
"Data_Setting12_UseFakeData_Enable":"No",
"Data_Setting13_UseFakeData_rhoa_Readme":"逆推參數-使用假的觀測資料值(視電阻率值)，單位:[Ohm-m]。建議值:「100」，限制填正數。",
"Data_Setting13_UseFakeData_rhoa":100,
"OutputFile01_GeometricFactor_K_CrossPlot_PNG_Enable_Readme":"幾何因子分析圖。若要啟用請填入字串Yes，若不啟用請填入字串No。不啟用可節省計算時間。",
"OutputFile01_GeometricFactor_K_CrossPlot_PNG_Enable":"Yes",
"Inversion_Setting00_getOptimizeLambda_Enable_Readme":"查找最佳lambda。若要啟用請填入字串Yes，若不啟用請填入字串No。啟用的話只會跑一次迭代，用L曲線法找出第一次迭代的最佳lambda。",
"Inversion_Setting00_getOptimizeLambda_Enable":"No",
"Inversion_Setting00_getOptimizeLambda_StartingLambda_Readme":"查找最佳lambda的起始值。將固定逐點測試0.8",
"Inversion_Setting00_getOptimizeLambda_StartingLambda":10,
"Inversion_Setting01_Remove_Data_Times_Readme":"逆推參數-刪除擬合不佳的觀測資料次數，單位:[次]。建議值3，若不刪除任何資料，填「0」。",
"Inversion_Setting01_Remove_Data_Times":1,
"Inversion_Setting02_Remove_Data_Percentage_Readme":"逆推參數-刪除擬合不佳的觀測資料百分比，單位:[%]。建議值5[%]。限制填入1-99的正整數，請注意刪除過多可能導致程式錯誤。",
"Inversion_Setting02_Remove_Data_Percentage":5,
"Inversion_Setting03_Lambda_Readme":"逆推參數-權重參數Lambda，單位:[無]。建議值20。更相信資料就將lambda減小。lambda調大通常造成模型更加平滑。lambda太小模型將由資料主宰有過擬合的風險；lambda太大模型將過度平滑無法反映細節。",
"Inversion_Setting03_Lambda":[20],
"Inversion_Setting04_maxIter_Readme":"逆推參數-最大迭代次數，單位:[次]。建議值20。",
"Inversion_Setting04_maxIter":20,
"Inversion_Setting05_startModel_Readme":"逆推參數-起始模型，單位:[Ohm-m]。建議值:「-1」，限制填正數或「-1」或「-2」或「-3」。填「-1」表示使用資料的中位數製作均質起始模型。若填正數則用該值製作均質起始模型。填「-2」表示使用全網格MeshVTK內的電阻率值。填「-3」表示使用研究區域網格StartModelVTK內的電阻率值。",
"Inversion_Setting05_startModel":-1,
"InputFile05_StudyAreaStartModelVTK_FileName_Readme":"不論Windows或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"InputFile05_StudyAreaStartModelVTK_FileName":"Input_ERTMaker_Inversion2D/InputFile05_StudyAreaStartModelVTK_FileName.vtk",
"Inversion_Setting06_verbose_Enable_Readme":"逆推參數-詳細運行資訊。若要啟用請填入字串Yes，若不啟用請填入字串No。",
"Inversion_Setting06_verbose_Enable":"Yes",
"Inversion_Setting07_limitModelCellMinValue_Readme":"逆推參數-模型電阻率下限。預設值:「0」，限制填正數或「0」。填「0」表示不限制。",
"Inversion_Setting07_limitModelCellMinValue":0,
"Inversion_Setting08_limitModelCellMaxValue_Readme":"逆推參數-模型電阻率上限。預設值:「0」，限制填正數或「0」。填「0」表示不限制。",
"Inversion_Setting08_limitModelCellMaxValue":0,
"Inversion_Setting09_SR_Enable_Readme":"逆推參數-奇異值移除技術(SR技術)。若要啟用請填入字串Yes，若不啟用請填入字串No。預設值:「Yes」",
"Inversion_Setting09_SR_Enable":"Yes",
"OutputMainFolderPath_Readme":"不論Windows或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。務必要使用「/」在結尾，表示資料夾。",
"OutputMainFolderPath":"Output_ERTMaker_Inversion2D/",
"Output_MainFileName_Readme":"會產生很多檔案，這些檔案的主要檔名。",
"Output_MainFileName":"XP2",
"Output_Inversion_ColorBarResistivityMin_Readme":"電阻率色階下限，線性數值，必為正數。建議:1。",
"Output_Inversion_ColorBarResistivityMin":50,
"Output_Inversion_ColorBarResistivityMax_Readme":"電阻率色階上限，線性數值，必為正數。建議:10000。",
"Output_Inversion_ColorBarResistivityMax":2000,
"Output_Inversion_XMinMax_Enable_Readme":"繪製研究區域網格-使用自訂X範圍。若要啟用請填入字串Yes，若不啟用請填入字串No。預設值:「No」",
"Output_Inversion_XMinMax_Enable":"No",
"Output_Inversion_XMin_Readme":"繪製研究區域網格-使用X範圍下限。請填入網格範圍內的數字，必須小於XMax。",
"Output_Inversion_XMin":-40,
"Output_Inversion_XMax_Readme":"繪製研究區域網格-使用X範圍上限。請填入網格範圍內的數字，必須大於XMin。",
"Output_Inversion_XMax":1600,
"Output_Inversion_YMin_Readme":"繪製研究區域網格-使用Y範圍下限。請填入網格範圍內的數字，必須小於YMax。",
"Output_Inversion_YMin":-624,
"Output_Inversion_YMax_Readme":"繪製研究區域網格-使用Y範圍上限。請填入網格範圍內的數字，必須大於YMin。",
"Output_Inversion_YMax":64
}
```
+ 2.5 去修改2594行的程式碼:
  + 由這個:
  ```
      if not FileExists('ERTMaker_Inversion2D_v20260419a.cpython-312.pyc') then
  ```
  + 改成:
  ```
      if not FileExists('ERTMaker_Inversion2D_v20261029a.cpython-312.pyc') then
  ```
+ 2.6 去修改2608行的程式碼:
  + 由這個:
  ```
     InversionModeling_AsyncProcess.Parameters.Add('set PYTHONIOENCODING=utf-8 && .\PythonEnv\Python.exe -u ERTMaker_Inversion2D_v20260419a.cpython-312.pyc');
  ```
  + 改成:
  ```
     InversionModeling_AsyncProcess.Parameters.Add('set PYTHONIOENCODING=utf-8 && .\PythonEnv\Python.exe -u ERTMaker_Inversion2D_v20261029a.cpython-312.pyc');
  ```
+ 2.7 去修改4243行的程式碼:
  + 由這個:
  ```
    if not FileExists('ERTMaker_v299ScsvToUrf_v20260217a.cpython-312.pyc') then 
  ```
  + 改成:
  ```
    if not FileExists('ERTMaker_v299ScsvToUrf_v20261028a.cpython-312.pyc') then 
  ```
+ 2.8 去修改4258行的程式碼:
  + 由這個:
  ```
    TimeSeriesProcessing_AsyncProcess.Parameters.Add('.\PythonEnv\Python.exe -u ERTMaker_v299ScsvToUrf_v20260217a.cpython-312.pyc'); 
  ```
  + 改成:
  ```
    TimeSeriesProcessing_AsyncProcess.Parameters.Add('.\PythonEnv\Python.exe -u ERTMaker_v299ScsvToUrf_v20261028a.cpython-312.pyc'); 
  ```
+ 2.9 去修改2397行的程式碼:
  + 由這個:
  ```
      InversionModelingSettingsNowJson_Memo.Lines.Strings[77-1] := ('"Inversion_Setting03_Lamda":'+InversionModelingInvSettings_Lambda_Edit.Text+',');
  ```
  + 改成:
  ```
      InversionModelingSettingsNowJson_Memo.Lines.Strings[77-1] := ('"Inversion_Setting03_Lambda":'+InversionModelingInvSettings_Lambda_Edit.Text+',');
  ```
+ 2.10 去修改2567行的程式碼:
  + 由這個:
  ```
          InversionModelingSettingsNowJson_Memo.Lines.Strings[109-1] := ('"Output_Inversion_YMax":'+temp_str4+',');
  ```
  + 改成:
  ```
          InversionModelingSettingsNowJson_Memo.Lines.Strings[109-1] := ('"Output_Inversion_YMax":'+temp_str4);
  ```
+ 2.11 去修改4198行的程式碼:
  + 由這個:
  ```
      TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[83-1] := '"OutputFile24_PNG_Select_Enable":"Yes",';
  ```
  + 改成:
  ```
      TimeSeriesProcessingSettingsNowJson_Memo.Lines.Strings[83-1] := '"OutputFile24_PNG_Select_Enable":"Yes"';
  ```
+ 2.12 去修改1040行的程式碼:
  + 由這個:
  ```
      CreateMesh_AsyncProcess.Options:=[poUsePipes] + [poNoConsole]; 
  ```
  + 改成:
  ```
      CreateMesh_AsyncProcess.Options:=[poUsePipes, poStderrToOutPut, poNoConsole];
  ```
+ 2.13 去修改1524行的程式碼:
  + 由這個:
  ```
      ForwardModeling_AsyncProcess.Options:=[poUsePipes] + [poNoConsole]; 
  ```
  + 改成:
  ```
      ForwardModeling_AsyncProcess.Options:=[poUsePipes, poStderrToOutPut, poNoConsole];
  ```
+ 2.14 去修改4259行的程式碼:
  + 由這個:
  ```
    TimeSeriesProcessing_AsyncProcess.Options:=[poUsePipes] + [poNoConsole];    
  ```
  + 改成:
  ```
    TimeSeriesProcessing_AsyncProcess.Options:=[poUsePipes, poStderrToOutPut, poNoConsole];
  ```
+ 2.15 修改「InversionModelingOutputSettings_MainName_GroupBox」的「Hint」:
```
測線名稱。將作為檔案命名用途。
```
+ 2.16 修改「InversionModelingInvSettings_Lambda_GroupBox」的「Hint」:
```
逆推參數中的Lambda為「正規化參數(Regularization Parameter)」，或通俗地稱為「平滑係數(Smoothing Factor)」。其值越大表示目標函數中資料項影響降低、模型項影響增加，通常導致模型更平滑但資料擬合較差。反之，其值越小，模型主要受資料項影響，通常會產生更粗糙(更不平滑)的模型。
預設值=[20]。請填入陣列，限制陣列中元素僅可填入正數，例如:「[100,33.33,1]」。
依序選擇陣列中的Lambda進行逆推，當該Lambda的逆推滿足終止條件時，保留該模型作為起始模型接關使用下一個Lambda繼續逆推，直到所有Lambda都執行完畢並獲得最終模型為止。
```
+ 2.17 修改「InversionModelingInvSettings_startModel_GroupBox」的「Hint」:
```
起始模型，單位:[Ohm-m]。預設值=「-1」。限制填正數或「-1」或「-2」或「-3」。填正數則用該值製作均質起始模型。填「-1」表示使用資料的中位數製作均質起始模型。填「-2」表示使用前面已經載入的全網格MeshVTK內的電阻率值。填「-3」表示使用另外準備的研究區域網格StartModelVTK內的電阻率值，該檔案必須是「Input_ERTMaker_Inversion2D/InputFile05_StudyAreaStartModelVTK.vtk」。
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
**建議作業系統: Windows 10
**ERTMaker主版本號: v1.0
**ERTMaker圖形化介面版本號: v20261029a
**Python版本: 3.12.7 (64-bit)
**PyGimli版本: v1.5.4
***已知問題: 逆推運算的平行運算預設不使用所有核心運作(會保留兩個核心)。但在Intel第12代的CPU(i7-1260P)環境下設定失效。這可能會使作業系統極度忙碌，導致網路等功能無回應。這並非軟體設計問題，而是這版本的CPU調度邏輯不良。
***支援快捷鍵: 「Ctrl+方向鍵」來快速瀏覽歷次逆推結果。
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
