# 台積電 井下跨孔ERT調查#01

### 地下井位資訊
| 地下井名稱        | 經度        | 緯度        | TM2_TWD97_E | TM2_TWD97_N | 高程[m] | 預訂井深[m] | 修正井深[m] | 電極實際深度[m]     |
| :--------------- | :---------- | :---------- | :---------- | :---------- | :----- | :---------- | :--------- | :----------------- |
| **E1-1(E4-480)** | 120.6100123 | 24.21678003 | 210388.567  | 2679086.532 | 205.80 | 83          | 83         | 19,22,25,...,79,82 |
| **E1-2(E1-500)** | 120.6101813 | 24.21674638 | 210405.7261 | 2679082.758 | 202.80 | 80          | 80         | 16,19,22,...,76,79 |
| **E1-3(E1-520)** | 120.610314  | 24.21671158 | 210419.19   | 2679078.865 | 201.80 | 79          | 79         | 15,18,21,...,75,78 |
| **E2-1**         | 120.6103417 | 24.21684685 | 210422.047  | 2679093.839 | 201.80 | 79          | 79         | 15,18,21,...,75,78 |
| **E1-4(E2-320)** | 120.6085001 | 24.21740046 | 210235.165  | 2679155.675 | 225    | 90          | 90         | 26,29,32,...,86,89 |
| **E1-5(E1-320)** | 120.608512  | 24.21725642 | 210236.332  | 2679139.719 | 225    | 90          | 90         | 26,29,32,...,86,89 |

### 上邊坡兩口井: E1-4(E2-320)、 E1-5(E1-320)
+ E1-4(E2-320): 簡化模型中，地表位置訂為 X=0,Z=225。Z為高程，往上為正。
+ E1-5(E1-320): 簡化模型中，地表位置訂為 X=16,Z=225。Z為高程，往上為正。


### ERTMaker設定 (上邊坡兩口井: E1-4(E2-320)、 E1-5(E1-320))
1. 名稱: `E1-4_E1-5`
2. 色階: `1,10000`
3. 內網格: `100,1,1`
4. 左網格: `5,1.2,-1`
5. 右網格: `5,1.2,-1`
6. 下網格: `5,1.2,-1`
7. 地表節點:
```
-5,0,0
-4,0,0
-3,0,0
-2,0,0
-1,0,0
0,0,1
1,0,4
2,0,4
3,0,4
4,0,4
5,0,7
6,0,10
7,0,10
8,0,10
9,0,10
10,0,13
11,0,16
12,0,19
13,0,22
14,0,0
15,0,0
16,0,0
17,0,0
18,0,0
19,0,0
20,0,0
21,0,0
```
8. 電阻率規則:
```
-9999, 9999, -9999, 9999, 100
3, 13, -80, -70, 10
```
9. ohm
```
#============================================================
# Data Format: unified data format(*.ohm)
# REF: http://resistivity.net/bert/data_format.html
# Description: 
#     pyGIMLI 2D ERT OHM Data
# 注意事項:
#   HsiupoYeh 2024-04-29整理，適用pyGIMLI v.1.5.4
#   電極資料從編號1開始到電極數量。例如:電極數量為38，編號為1~38。
#   編號0表示位置無限遠，留給特殊陣列使用。
#   所有資料可以用空白或tab分隔。
#   註解用「#」
#============================================================

#------------------------------------------------------------
# 在2D的情況下，電極定義為「#x z」。
# x 單位公尺，側向位置向右為正。
# z 單位公尺，高程向上為正。
# 注意: 讀檔程式不聰明，電極數量寫在本區塊某一行，其下一行必須是「#x z」。
#       這個「#x z」不是註解，讀檔要解析...
#--
# 電極數量:
44
#x z
0 -26.0
0 -29.0
0 -32.0
0 -35.0
0 -38.0
0 -41.0
0 -44.0
0 -47.0
0 -50.0
0 -53.0
0 -56.0
0 -59.0
0 -62.0
0 -65.0
0 -68.0
0 -71.0
0 -74.0
0 -77.0
0 -80.0
0 -83.0
0 -86.0
0 -89.0
16 -26.0
16 -29.0
16 -32.0
16 -35.0
16 -38.0
16 -41.0
16 -44.0
16 -47.0
16 -50.0
16 -53.0
16 -56.0
16 -59.0
16 -62.0
16 -65.0
16 -68.0
16 -71.0
16 -74.0
16 -77.0
16 -80.0
16 -83.0
16 -86.0
16 -89.0
#------------------------------------------------------------

#------------------------------------------------------------
# 注意: 讀檔程式不聰明，觀測資料數量寫在本區塊某一行，其下一行必須是「#A B M N R」。
#       這個「#A B M N R」不是註解，讀檔要解析...
#       A,B,M,N均為電極編號，以上方區塊電極位置的順序當作編號，第一個電極為編號1。
#       R為AB電流與MN電壓比值(V_MN/I_AB)，單位「Ohm」。
# 觀測資料數量:
1
#A B M N R
1 4 2 3 100
#------------------------------------------------------------
```
10. custom01 CurrentMode
+ A= 1 ~ 22, B= 23 ~ 43 
```
clear;clc;close all        
%--
% 找出需要的放電AB
EarthImagerCmdFile.AB_index.Data=[
];
for i=1:22
    for j=23:43
    EarthImagerCmdFile.AB_index.Data=[EarthImagerCmdFile.AB_index.Data;[i,j]];
    end
end
% 寫成放電檔案
EarthImagerCmdFile.R2MS_Lite_CurrentModeCsv.Data=[];
for i=1:length(EarthImagerCmdFile.AB_index.Data(:,1))
    temp_A_index=EarthImagerCmdFile.AB_index.Data(i,1);
    temp_B_index=EarthImagerCmdFile.AB_index.Data(i,2);
    temp_second_1_CurrentMode=zeros(1,64);
    temp_second_1_CurrentMode(temp_A_index)=1;
    temp_second_1_CurrentMode(temp_B_index)=2;
    EarthImagerCmdFile.R2MS_Lite_CurrentModeCsv.Data=[EarthImagerCmdFile.R2MS_Lite_CurrentModeCsv.Data;temp_second_1_CurrentMode];
    temp_second_2_CurrentMode=zeros(1,64);
    temp_second_2_CurrentMode(temp_A_index)=2;
    temp_second_2_CurrentMode(temp_B_index)=1;
    EarthImagerCmdFile.R2MS_Lite_CurrentModeCsv.Data=[EarthImagerCmdFile.R2MS_Lite_CurrentModeCsv.Data;temp_second_2_CurrentMode];
    temp_second_3_CurrentMode=zeros(1,64);
    EarthImagerCmdFile.R2MS_Lite_CurrentModeCsv.Data=[EarthImagerCmdFile.R2MS_Lite_CurrentModeCsv.Data;temp_second_3_CurrentMode];
end
EarthImagerCmdFile.R2MS_Lite_CurrentModeCsv.Data=EarthImagerCmdFile.R2MS_Lite_CurrentModeCsv.Data';
%--------------------------------------------------------------------------
csvwrite('Current Mode.csv',EarthImagerCmdFile.R2MS_Lite_CurrentModeCsv.Data); 
```
