# SwitchArray 韌體研究

### 腳位定義
```
#define REF_PSU_POS 5 ->ESP32板子簡介圖右邊10->線路圖H1_10:REF_PSU_POS->空的U3
#define READY_PIN_CURRENT_NEG 12 ->ESP32板子簡介圖左邊13->線路圖H2_7:N_ready->
#define READY_PIN_CURRENT_POS 13 ->ESP32板子簡介圖左邊15->線路圖H2_5:P_ready->
#define RESET 14 ->ESP32板子簡介圖左邊12->線路圖H2_8:reset->接到(U101~U104)的SRCLR
#define OFF_PSU 17 ->ESP32板子簡介圖右邊11->線路圖H1_9:根本沒接
#define REF_EXTERNAL 18 ->ESP32板子簡介圖右邊9->線路圖H1_11:REF_EXTERNAL->有用的U1
#define REF_PSU_NEG 19 ->ESP32板子簡介圖右邊9->線路圖H1_12:REF_PSU_NEG->空的U2
#define DATA 21 ->ESP32板子簡介圖右邊6->線路圖H1_14:serial_data->接到(U101~U104)的SER
#define READY_PSU  22 -> ESP32板子簡介圖右邊3->線路圖H1_17:V_PSU_ready ->接到U103的RCLK    
#define PUSH_PIN 23  -> ESP32板子簡介圖右邊2->線路圖H1_18:clock ->接到(U101~U104)的SRCLK                                                                                                                               
#define OFF_VOLTAGE 25 ->ESP32板子簡介圖左邊9->線路圖H2_11:V_enable->接到U104的OE#
#define READY_VOLTAGE 26 ->ESP32板子簡介圖左邊10->線路圖H2_10:V_ready ->接到U104的RCLK
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
+ 
