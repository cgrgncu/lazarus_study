# SwitchArray 韌體研究

### 腳位定義
```
#define REF_PSU_POS 5 ->板子右邊10->線路圖H1_10:REF_PSU_POS->空的U3
#define READY_PIN_CURRENT_NEG 12 ->板子左邊13->線路圖H2_7:N_ready->接到U102的RCLK 
#define READY_PIN_CURRENT_POS 13 ->板子左邊15->線路圖H2_5:P_ready->接到U101的RCLK 
#define RESET 14 ->板子左邊12->線路圖H2_8:reset->接到(U101~U104)的SRCLR
#define OFF_PSU 17 ->板子右邊11->線路圖H1_9:根本沒接
#define REF_EXTERNAL 18 ->板子右邊9->線路圖H1_11:REF_EXTERNAL->有用的U1
#define REF_PSU_NEG 19 ->板子右邊9->線路圖H1_12:REF_PSU_NEG->空的U2
#define DATA 21 ->板子右邊6->線路圖H1_14:serial_data->接到(U101~U104)的SER
#define READY_PSU  22 -> 板子右邊3->線路圖H1_17:V_PSU_ready ->接到U103的RCLK    
#define PUSH_PIN 23  -> 板子右邊2->線路圖H1_18:clock ->接到(U101~U104)的SRCLK 
#define OFF_VOLTAGE 25 ->板子左邊9->線路圖H2_11:V_enable->接到U104的OE#
#define READY_VOLTAGE 26 ->板子左邊10->線路圖H2_10:V_ready ->接到U104的RCLK->到BNC的正
```

### 主要迴圈
```c
void loop() {

  while (Serial.available() > 0) {

    readCommand(Serial.read());
    connect_last_time=millis();
  }

  if(millis()-connect_last_time>300000)
  {
    if(millis()-connect_last_time<330000)
    {
      pinMode(READY_VOLTAGE, INPUT);
    }
    delay(1000);
  }

}
```
+ 基本上就是一值讀一次一個Byte，然後進去readCommand來解析命令做對應的工作。
+ 另外有一個大約5分鐘都沒有收到命令的條件下會觸發的奇怪休息命令並初始化READY_VOLTAGE。

### 清除SN74HC595DR內部位移暫存器（Shift Register）內容(等於在規劃狀態的初始化)。
```c
void resetSerial() {
  digitalWrite(RESET, 0); //觸發清除(SRCLR拉高)
  delayMicroseconds(pushPeriod); //等待
  digitalWrite(RESET, 1); // 恢復
}
```
+ 拉低SRCLR時會清空內部位移暫存器（Shift Register）內容(等於在規劃狀態的初始化)。
+ 清除後等待一下再把SRCLR拉高，後恢復正常狀態。
+ 注意: 內部位移暫存器（Shift Register）是規劃期的，並不影響IO硬體狀態。

### 用全部空的規劃，然後依序應用到硬體中。導致全部IO關閉。
```c
void clearEveryOutput() {
  resetSerial();// 內部位移暫存器（Shift Register）全部歸0
  digitalWrite(READY_PIN_CURRENT_POS, 1); //觸發鎖存(RCLK拉高)
  delayMicroseconds(pushPeriod); //等待
  digitalWrite(READY_PIN_CURRENT_POS, 0); // 恢復拉低
  delayMicroseconds(pushPeriod); //等待
  digitalWrite(READY_PIN_CURRENT_NEG, 1); //觸發鎖存(RCLK拉高)
  delayMicroseconds(pushPeriod); //等待
  digitalWrite(READY_PIN_CURRENT_NEG, 0); // 恢復拉低
  delayMicroseconds(pushPeriod); //等待
  digitalWrite(READY_VOLTAGE, 1); //觸發鎖存(RCLK拉高)
  delayMicroseconds(pushPeriod); //等待
  digitalWrite(READY_VOLTAGE, 0); // 恢復拉低
  delayMicroseconds(pushPeriod); //等待
  digitalWrite(READY_PSU, 1); //觸發鎖存(RCLK拉高)
  delayMicroseconds(pushPeriod); //等待
  digitalWrite(READY_PSU, 0); // 恢復拉低
  delayMicroseconds(switchTurnOffTime); //等待200us
}
```
+ 規劃的 內部位移暫存器（Shift Register）全部歸0，然後逐個觸發鎖存(RCLK拉高)及啟用(OE#拉低)，結果就使所有繼電器關閉。
+ 後面有等待時間，200us。這個等於規格書的最大值，很合理。


### 方法B的解析 (會觸發256?次BNC線的TRIGER)
```
case 'b':  
{
	//----------------------------
	// 這看起來bcount是完全沒用的變數...
	bcount++;  //DD
	//----------------------------
	//----------------------------
	// 這看起來就是預防性斷開繼電器
	resetREF();//其實就是digitalWrite(REF_EXTERNAL, 0);斷開參考電極
	//----------------------------
	//----------------------------
	// 這看起來就是要開始規劃狀態，使其暫存器內容初始化為00000000。
	resetSerial(); //利用SRCLR清空位移暫存器內容(等於在規劃狀態的初始化)。
	//----------------------------	
	//----------------------------
	// 這看起來就是把剛剛規劃的狀態全部00000000，鎖存之後等同於把第三排繼電器全部斷開。這優點就是不用慢慢推0推到最後一個IC。
	digitalWrite(READY_PSU, 1);//第三排繼電器電流用的RCLK拉高，觸發鎖存(等於規劃好的狀態被儲存)。因為OE#硬體拉低等於硬體啟用(等於鎖存之後立刻依照設定成為開路或斷路)
	delayMicroseconds(pushPeriod);//等待
	digitalWrite(READY_PSU, 0);//第三排繼電器電壓用的RCLK拉低，已經鎖存完就恢復低。
	delayMicroseconds(switchTurnOffTime);//等待
	//----------------------------
	//--------------------------------------------------------
	// 這看起來就是跑多次輪流量測，因為S命令這裡會是4次。因為A命令頻道數是64。所以最終會量測4*64=256點。順序會是全部的電壓先量完，然後全部的電流量完。
	for (int sample_i = 0; sample_i < sample_number; sample_i++) {  //sample_number = 4 跑四次            
		//--------------------------------------------------------
		// 大繼電器狀態已經固定不動，且電流已經在釋放。
		// 控制小繼電器去量測電壓頻道。通常單源情況下總頻道64電壓頻道62電流頻道2。這裡將量測62次
		// 原理是電表的DMM(+)目前是空接，DMM(-)目前也是空接。
		// 現在所有小繼電器才跟電表有關，初始情況下全部都是不通。
		// 1. 切換ControlBoard上的獨立小繼電器，使以下持續連接: 電表負端DMM(-)[線路圖上稱DMM_ref] <--> 參考電極[線路圖上稱REF_pin]。這時候電表接好一隻腳。
		// 2. 沒有控制RelayBoard的第三排繼電器，所以DMM(-)不會連接RelayBoard。
		// 3. 控制RelayBoard的第四排繼電器，只與DMM(+)有關。然後有需要的就使以下短暫連接並觸發量測，量完就斷開。: 電表正端DMM(+)[線路圖上稱DMM_ref] <--> 普通電極。這時候電表接好兩隻腳，進行量測。
		// 測完電表的DMM(+)目前是空接，DMM(-)目前是接參考電極。
		// 4. 切換ControlBoard上的獨立小繼電器，斷開連接
		// 最終恢復原狀。電表的DMM(+)目前是空接，DMM(-)目前是空接。
		//--
		//----------------------------
		/ 這邊看起來就是連接DMM(-)到參考電極
		digitalWrite(REF_EXTERNAL, 1);//連接到參考電極
		delayMicroseconds(switchTurnOnTime);//等他接好
		//----------------------------
		//----------------------------
		// 這邊看起來就是規劃狀態，先初始化為全0，然後推一個1進去。之後只推0。所以只會有一個繼電器會被連接。
		resetSerial(); //利用SRCLR清空位移暫存器內容(等於在規劃狀態的初始化)。		
		digitalWrite(DATA, 1);//拉高SER，這是想要被推進去的資料。若高，等下就是要推進去1。
		delayMicroseconds(pushPeriod);//等待
		digitalWrite(PUSH_PIN, 1);//拉高SRCLK，這時候推進去(等於在規劃狀態)
		delayMicroseconds(pushPeriod);//等待
		digitalWrite(PUSH_PIN, 0);//拉低SRCLK，已經推完就恢復低。
		digitalWrite(DATA, 0);//拉低SER，恢復低
		//----------------------------
		//----------------------------
		// 這邊看起來就是維持連接DMM(-)的情況下，找到要量電壓的逐個去連接DMM(+)，並量測電壓，量完就斷開DMM(+)。
		for (int ii = 0; ii < channel_count; ii++) { // channel_count=64，這裡一次只會開一個IO
			//--
			if (current_mode[ii] == 0) {//要量測電壓，以我的狀況，會有62個0，表示會請DMM量62次。
				//--
				digitalWrite(READY_VOLTAGE, 1);//第四排繼電器電壓用的RCLK拉高，觸發鎖存(等於規劃好的狀態被儲存)，目前OE#拉高，硬體還不會啟用。
				delayMicroseconds(pushPeriod);//等待
				digitalWrite(READY_VOLTAGE, 0);//第四排繼電器電壓用的RCLK拉低，已經鎖存完就恢復低。只有這個PIN硬體設計上這個做完會造成一個BNC脈衝。DMM收到脈衝後會等0.1ms才工作然後會量測並平均接下來100us內的資料。
				//--
				digitalWrite(OFF_VOLTAGE, 0);//第四排繼電器電壓用的OE#拉低，硬體啟用(等於讓硬體依照儲存的狀態工作，依照設定成為開路或斷路)
				delayMicroseconds(scan_turn_on_time);//等待，這裡共等了0.164ms，
				digitalWrite(OFF_VOLTAGE, 1);//第四排繼電器電壓用的OE#拉高，硬體停用(等於讓硬體一定回到斷路)
				delayMicroseconds(switchTurnOffTime);//等待
				//--
			} else {
				//--
				delayMicroseconds(pushPeriod);//等待
				//--
			}
			//--
			digitalWrite(PUSH_PIN, 1);//拉高SRCLK，這時候推進去(等於在規劃狀態)
			delayMicroseconds(pushPeriod);//等待
			digitalWrite(PUSH_PIN, 0);//拉低SRCLK，已經推完就恢復低。
			//--
		}
		//----------------------------
		// 這邊看起來就是斷開DMM(-)到參考電極
		resetREF();//其實就是digitalWrite(REF_EXTERNAL, 0);斷開參考電極
		delayMicroseconds(switchTurnOffTime);//等待
		//--------------------------------------------------------

		//--------------------------------------------------------
		// 大繼電器狀態已經固定不動，且電流已經在釋放。
		// 控制小繼電器去量測電流頻道。通常單源情況下總頻道64電壓頻道62電流頻道2。這裡將量測2次
		// 原理是電表的DMM(+)目前是空接，DMM(-)目前也是空接。
		// 現在所有小繼電器才跟電表有關，初始情況下全部都是不通。
		// 1. 沒有控制ControlBoard上的獨立小繼電器，所以DMM(-)不會連接參考電極。
		// 2. 控制RelayBoard的第三排繼電器，只與DMM(-)有關。使以下持續連接: 電表負端DMM(-)[線路圖上稱DMM_ref] <--> 量測電流用的檢流電阻A側。這時候電表接好一隻腳。
		// 3. 控制RelayBoard的第四排繼電器，只與DMM(+)有關。然後有需要的就使以下短暫連接並觸發量測，量完就斷開。: 電表正端DMM(+)[線路圖上稱DMM_ref] <--> 量測電流用的檢流電阻B側。這時候電表接好兩隻腳，進行量測。
		// 測完電表的DMM(+)目前是空接，DMM(-)目前是接量測電流用的檢流電阻A側。
		// 4. 控制RelayBoard的第三排繼電器，只與DMM(-)有關。斷開所有連接。
		// 最終恢復原狀。電表的DMM(+)目前是空接，DMM(-)目前是空接。
		//--
		//----------------------------
		// 這邊看起來就是先維持連接DMM(-)的情況下，再找到要量電壓的逐個去連接DMM(+)，並量測電壓，量完就斷開DMM(+)，再斷開DMM(-)。
		for (int ii = 0; ii < channel_count; ii++) { // channel_count=64，這裡一次只會開一個IO 
			//--
			if (current_mode[ii] != 0) {//要量測電流，以我的狀況，會有2個非0，表示會請DMM量2次。
				//----------------------------
				// 這邊看起來就是規劃狀態，先初始化為全0，然後推一個1進去。之後只推0。所以只會有一個繼電器會被連接。
				resetSerial();//利用SRCLR清空位移暫存器內容(等於在規劃狀態的初始化)。
				digitalWrite(DATA, 1);//拉高SER，這是想要被推進去的資料。若高，等下就是要推進去1。
				delayMicroseconds(pushPeriod);//等待
				digitalWrite(PUSH_PIN, 1);//拉高SRCLK，這時候推進去(等於在規劃狀態)
				delayMicroseconds(pushPeriod);//等待
				digitalWrite(PUSH_PIN, 0);//拉低SRCLK，已經推完就恢復低。
				digitalWrite(DATA, 0);//拉低SER，恢復低
				//----------------------------
				//----------------------------
				// 這邊看起來是因為前面清空後推了一個1，繼續推0讓1被擠到想要的位置(就是規劃)
				for (int i = 0; i < ii; i++) {//就是算出來要讓1推到哪的時候，規劃對應的IO被切換，這裡只是規劃。
					digitalWrite(PUSH_PIN, 1);//拉高SRCLK，這時候推進去(等於在規劃狀態)，因為SER是LOW，所以只會推0進去。這時候永遠只有一個1。
					delayMicroseconds(pushPeriod);//等待
					digitalWrite(PUSH_PIN, 0);//拉低SRCLK，已經推完就恢復低。
					delayMicroseconds(pushPeriod);//等待
				}
				//----------------------------
				//----------------------------
				// 這邊看起來是把規劃好的狀態生效，使DMM(-)連接到檢流電阻A側。
				digitalWrite(READY_PSU, 1);//第三排繼電器電流用的RCLK拉高，觸發鎖存(等於規劃好的狀態被儲存)。因為OE#硬體拉低等於硬體啟用(等於鎖存之後立刻依照設定成為開路或斷路)
				delayMicroseconds(pushPeriod);//等待
				digitalWrite(READY_PSU, 0);//第三排繼電器電壓用的RCLK拉低，已經鎖存完就恢復低。
				delayMicroseconds(switchTurnOnTime);//等待  
				//----------------------------
				//----------------------------
				// 這邊看起來是維持DMM(-)連接到檢流電阻A側的情況下，再使DMM(+)連接到檢流電阻B側，並量測，測完就斷開DMM(+)。
				digitalWrite(READY_VOLTAGE, 1);//第四排繼電器電壓用的RCLK拉高，觸發鎖存(等於規劃好的狀態被儲存)，目前OE#拉高，硬體還不會啟用。
				delayMicroseconds(pushPeriod);//等待
				digitalWrite(READY_VOLTAGE, 0);//第四排繼電器電壓用的RCLK拉低，已經鎖存完就恢復低。只有這個PIN硬體設計上這個做完會造成一個BNC脈衝。DMM收到脈衝後會等0.1ms才工作然後會量測並平均接下來100us內的資料。
				digitalWrite(OFF_VOLTAGE, 0);//第四排繼電器電壓用的OE#拉低，硬體啟用(等於讓硬體依照儲存的狀態工作，依照設定成為開路或斷路)
				delayMicroseconds(scan_turn_on_time);//等待，這裡共等了0.164ms，
				digitalWrite(OFF_VOLTAGE, 1);//第四排繼電器電壓用的OE#拉高，硬體停用(等於讓硬體一定回到斷路)
				//----------------------------
				//----------------------------
				// 這邊看起來是把DMM(-)斷開
				resetSerial();//利用SRCLR清空位移暫存器內容(等於在規劃狀態的初始化)。              
				digitalWrite(READY_PSU, 1);//第三排繼電器電流用的RCLK拉高，觸發鎖存(等於規劃好的狀態被儲存)。因為OE#硬體拉低等於硬體啟用(等於鎖存之後立刻依照設定成為開路或斷路)  
				delayMicroseconds(pushPeriod);//等待
				digitalWrite(READY_PSU, 0);//第三排繼電器電壓用的RCLK拉低，已經鎖存完就恢復低。
				delayMicroseconds(switchTurnOffTime);//等待 
				//----------------------------
			}
			//--
		}
		//--------------------------------------------------------
	}
	//--------------------------------------------------------
  
  break;
}
```
