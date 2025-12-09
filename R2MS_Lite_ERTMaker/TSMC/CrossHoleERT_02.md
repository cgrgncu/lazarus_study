# 台積電 井下跨孔ERT調查#02

### 地下井位資訊
| 地下井名稱        | 經度        | 緯度        | TM2_TWD97_E | TM2_TWD97_N | 高程[m] | 預訂井深[m] | 修正井深[m] | 電極實際深度[m]     |
| :--------------- | :---------- | :---------- | :---------- | :---------- | :----- | :---------- | :--------- | :----------------- |
| **E1-1(E4-480)** | 120.6100123 | 24.21678003 | 210388.567  | 2679086.532 | 205.80 | 83          | 83         | 19,22,25,...,79,82 |
| **E1-2(E1-500)** | 120.6101813 | 24.21674638 | 210405.7261 | 2679082.758 | 202.80 | 80          | 80         | 16,19,22,...,76,79 |
| **E1-3(E1-520)** | 120.610314  | 24.21671158 | 210419.19   | 2679078.865 | 201.80 | 79          | 79         | 15,18,21,...,75,78 |
| **E2-1**         | 120.6103417 | 24.21684685 | 210422.047  | 2679093.839 | 201.80 | 79          | 79         | 15,18,21,...,75,78 |
| **E1-4(E2-320)** | 120.6085001 | 24.21740046 | 210235.165  | 2679155.675 | 225    | 90          | 90         | 26,29,32,...,86,89 |
| **E1-5(E1-320)** | 120.608512  | 24.21725642 | 210236.332  | 2679139.719 | 225    | 90          | 90         | 26,29,32,...,86,89 |

### 下邊坡四口井: E1-1(E4-480)、 E1-2(E1-500)、 E1-3(E1-520)、 E2-1
+ E1-1(E4-480): 簡化模型中，地表位置訂為 X=0,Z=205。Z為高程，往上為正。
+ E1-2(E1-500): 簡化模型中，地表位置訂為 X=20,Z=202。Z為高程，往上為正。
+ E1-3(E1-520): 簡化模型中，地表位置訂為 X=40,Z=201。Z為高程，往上為正。
+ E2-1: 簡化模型中，地表位置訂為 X=60,Z=201。Z為高程，往上為正。

### ERTMaker設定 S003 (下邊坡四口井: E1-1(E4-480)、 E1-2(E1-500)、 E1-3(E1-520)、 E2-1)
1. 名稱: `E1-1_E2-1_S003`
2. 色階: `1,10000`
3. 內網格: `100,1,1`
4. 左網格: `5,1.2,-1`
5. 右網格: `5,1.2,-1`
6. 下網格: `5,1.2,-1`
7. 地表節點:
```
-5,205,0
-4,205,0
-3,205,0
-2,205,0
-1,205,0
0,205,1
1,205,2
2,205,3
3,205,4
4,205,5
5,205,0
6,205,0
7,205,0
8,205,0
9,205,0
10,null,0
11,null,0
12,null,0
13,null,0
14,null,0
15,null,0
16,null,0
17,null,0
18,null,0
19,null,0
20,202,0
21,202,0
22,202,0
23,202,0
24,202,0
25,202,0
26,202,0
27,202,0
28,202,0
29,202,0
30,null,0
31,null,0
32,null,0
33,null,0
34,null,0
35,null,0
36,null,0
37,null,0
38,null,0
39,null,0
40,201,0
41,201,0
42,201,0
43,201,0
44,201,0
45,201,0
46,201,0
47,201,0
48,201,0
49,201,0
50,201,0
51,201,0
52,201,0
53,201,0
54,201,0
55,201,0
56,201,0
57,201,0
58,201,0
59,201,0
60,201,0
61,201,0
62,201,0
63,201,0
64,201,0
65,201,0
```
8. 電阻率規則:
```
-9999, 9999, -9999, 9999, 100
15, 25, 140, 145, 10
50, 60, 140, 145, 1000
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
21
#x z
0 186
0 183
0 180
0 177
0 174
20 186
20 183
20 180
20 177
20 174
40 186
40 183
40 180
40 177
40 174
60 186
60 183
60 180
60 177
60 174
60 171
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
10. custom02 CurrentMode
+ A= 1 ~ 5, B= 6 ~ 10
+ A= 6 ~ 10, B= 11 ~ 15
+ A= 1 ~ 5, B= 16 ~ 20
+ A= 6 ~ 10, B= 16 ~ 20
+ A= 11 ~ 15, B= 16 ~ 20
```
clear;clc;close all   
%--
% 找出需要的放電AB
EarthImagerCmdFile.AB_index.Data=[
];
% E1-1 vs E1-2
for i=1:5
    for j=6:10
        EarthImagerCmdFile.AB_index.Data=[EarthImagerCmdFile.AB_index.Data;[i,j]];
    end
end
% E1-2 vs E1-3
for i=6:10
    for j=11:15
        EarthImagerCmdFile.AB_index.Data=[EarthImagerCmdFile.AB_index.Data;[i,j]];
    end
end
% E1-1 vs E2-1
for i=1:5
    for j=16:20
        EarthImagerCmdFile.AB_index.Data=[EarthImagerCmdFile.AB_index.Data;[i,j]];
    end
end
% E1-2 vs E2-1
for i=6:10
    for j=16:20
        EarthImagerCmdFile.AB_index.Data=[EarthImagerCmdFile.AB_index.Data;[i,j]];
    end
end
% E1-3 vs E2-1
for i=11:15
    for j=16:20
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


### ERTMaker設定 S006 (下邊坡四口井: E1-1(E4-480)、 E1-2(E1-500)、 E1-3(E1-520)、 E2-1)
1. 名稱: `E1-1_E2-1_S006`
2. 色階: `1,10000`
3. 內網格: `100,1,1`
4. 左網格: `5,1.2,-1`
5. 右網格: `5,1.2,-1`
6. 下網格: `5,1.2,-1`
7. 地表節點:
```
-5,205,0
-4,205,0
-3,205,0
-2,205,0
-1,205,0
0,205,1
1,205,2
2,205,3
3,205,4
4,205,5
5,205,0
6,205,0
7,205,0
8,205,0
9,205,0
10,null,0
11,null,0
12,null,0
13,null,0
14,null,0
15,null,0
16,null,0
17,null,0
18,null,0
19,null,0
20,202,0
21,202,0
22,202,0
23,202,0
24,202,0
25,202,0
26,202,0
27,202,0
28,202,0
29,202,0
30,null,0
31,null,0
32,null,0
33,null,0
34,null,0
35,null,0
36,null,0
37,null,0
38,null,0
39,null,0
40,201,0
41,201,0
42,201,0
43,201,0
44,201,0
45,201,0
46,201,0
47,201,0
48,201,0
49,201,0
50,201,0
51,201,0
52,201,0
53,201,0
54,201,0
55,201,0
56,201,0
57,201,0
58,201,0
59,201,0
60,201,0
61,201,0
62,201,0
63,201,0
64,201,0
65,201,0
```
8. 電阻率規則:
```
-9999, 9999, -9999, 9999, 100
15, 25, 140, 145, 10
50, 60, 140, 145, 1000
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
65
#x z
0 171
0 168
0 165
0 162
0 159
0 156
0 153
0 150
0 147
0 144
0 141
0 138
0 135
0 132
0 129
0 126
20 171
20 168
20 165
20 162
20 159
20 156
20 153
20 150
20 147
20 144
20 141
20 138
20 135
20 132
20 129
20 126
40 171
40 168
40 165
40 162
40 159
40 156
40 153
40 150
40 147
40 144
40 141
40 138
40 135
40 132
40 129
40 126
60 171
60 168
60 165
60 162
60 159
60 156
60 153
60 150
60 147
60 144
60 141
60 138
60 135
60 132
60 129
60 126
60 123
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
10. custom03 CurrentMode
+ A= 1 ~ 16, B= 17 ~ 32
+ A= 17 ~ 32, B= 33 ~ 48
+ A= 1 ~ 16, B= 49 ~ 64
+ A= 17 ~ 32, B= 49 ~ 64
+ A= 33 ~ 48, B= 49 ~ 64
```
clear;clc;close all   
%--
% 找出需要的放電AB
EarthImagerCmdFile.AB_index.Data=[
];
% E1-1 vs E1-2
for i=1:16
    for j=17:32
        EarthImagerCmdFile.AB_index.Data=[EarthImagerCmdFile.AB_index.Data;[i,j]];
    end
end
% E1-2 vs E1-3
for i=17:32
    for j=33:48
        EarthImagerCmdFile.AB_index.Data=[EarthImagerCmdFile.AB_index.Data;[i,j]];
    end
end
% E1-1 vs E2-1
for i=1:16
    for j=49:64
        EarthImagerCmdFile.AB_index.Data=[EarthImagerCmdFile.AB_index.Data;[i,j]];
    end
end
% E1-2 vs E2-1
for i=17:32
    for j=49:64
        EarthImagerCmdFile.AB_index.Data=[EarthImagerCmdFile.AB_index.Data;[i,j]];
    end
end
% E1-3 vs E2-1
for i=33:48
    for j=49:64
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
