# R2MS_Lite_ERTMaker 開發筆記
+ 作者: HsiupoYeh  
+ 更新日期: 2025-11-06
+ 從 **R2MS_Lite_ERTMaker_v1.0(v20251105a).md** 開始，繼續更新基本功能。

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
  ...\PythonEnv\
  ...\PythonEnv\python.exe
  ...\PythonPrograms\... (相關的python程式)
  ```
  + 準備VC檔案，沒有安裝過散發套件的電腦會需要。
  ```
  ...\PythonEnv\vcruntime140.dll
  ...\PythonEnv\vcruntime140_1.dll
  ```

## 開發紀錄(寫功能部分)
### 版本
+ 1.1選選Form，去編輯FormCreate，修改部分:
```pascal
procedure TForm1.FormCreate(Sender: TObject);
begin
  //--------------------------------------------------------------------------
  // 在這裡對全域變數進行初始化
  version_str := 'v1.0(v20251106a)';
  Current_Folder_Path := ExtractFilePath(Application.ExeName);
  //--------------------------------------------------------------------------
  //--------------------------------------------------------------------------
  // 表單標題追加版本
  Form1.Caption := Form1.Caption + ' ' + version_str;
  //--------------------------------------------------------------------------
end;      
```

### 寫功能
+ 1.1 去修改「CreateMeshSettingsDefaultJson_Memo」的「Lines」為以下文字。
```
{
"CreateAndModifyMeshSettings_Version":"v20251031a",
"CreateAndModifyMeshSettings_Author":"HsiupoYeh",
"TempFile01_BasicMeshPNG_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"TempFile01_BasicMeshPNG_FileName":"Temp/CreateMesh_01_BasicMesh.png",
"TempFile02_BasicMeshVTK_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"TempFile02_BasicMeshVTK_FileName":"Temp/CreateMesh_01_BasicMesh.vtk",
"TempFile03_BasicMeshPNG_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"TempFile03_BasicMeshPNG_FileName":"Temp/CreateMesh_02_BasicMeshSetMarker1.png",
"TempFile04_BasicMeshVTK_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"TempFile04_BasicMeshVTK_FileName":"Temp/CreateMesh_02_BasicMeshSetMarker1.vtk",
"TempFile05_BasicMeshPNG_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"TempFile05_BasicMeshPNG_FileName":"Temp/CreateMesh_03_BasicMeshSetMarker2.png",
"TempFile06_BasicMeshVTK_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"TempFile06_BasicMeshVTK_FileName":"Temp/CreateMesh_03_BasicMeshSetMarker2.vtk",
"TempFile07_BasicMeshPNG_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"TempFile07_BasicMeshPNG_FileName":"Temp/CreateMesh_04_BasicMeshSetBCMarker1.png",
"TempFile08_BasicMeshVTK_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"TempFile08_BasicMeshVTK_FileName":"Temp/CreateMesh_04_BasicMeshSetBCMarker1.vtk",
"TempFile09_BasicMeshPNG_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"TempFile09_BasicMeshPNG_FileName":"Temp/CreateMesh_05_BasicMeshSetBCMarker2.png",
"TempFile10_BasicMeshVTK_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"TempFile10_BasicMeshVTK_FileName":"Temp/CreateMesh_05_BasicMeshSetBCMarker2.vtk",
"TempFile11_BasicMeshPNG_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"TempFile11_BasicMeshPNG_FileName":"Temp/CreateMesh_06_BasicMeshSetTopo.png",
"TempFile12_BasicMeshPNG_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"TempFile12_BasicMeshPNG_FileName":"Temp/CreateMesh_07_BasicMeshSetTopoRes.png",
"TempFile13_BasicMeshVTK_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"TempFile13_BasicMeshVTK_FileName":"Temp/CreateMesh_07_BasicMeshSetTopoRes.vtk",
"OutputFile01_MeshBCMarkersJSON_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"OutputFile01_MeshBCMarkersJSON_FileName":"Output_ERTMaker_CreateAndModifyMesh/Demo_SyntheticModelBCMarkers.json",
"OutputFile02_MeshBCMarkersVTK_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"OutputFile02_MeshBCMarkersVTK_FileName":"Output_ERTMaker_CreateAndModifyMesh/Demo_SyntheticModel.vtk",
"OutputFile03_GEO_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"OutputFile03_GEO_FileName":"Output_ERTMaker_CreateAndModifyMesh/Demo_SyntheticModel.geo",
"OutputFile04_TRN_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"OutputFile04_TRN_FileName":"Output_ERTMaker_CreateAndModifyMesh/Demo_SyntheticModel.trn",
"OutputFile05_OHM_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
"OutputFile05_OHM_FileName":"Output_ERTMaker_CreateAndModifyMesh/Demo_SyntheticModel.ohm",
"OutputFile06_BasicMeshPNG_FileName_Readme":"不論Winodws或Linux或MAC作業系統一律使用「/」描述檔案路徑與資料夾路徑。",
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
+ 1.2
