---
layout: post
title: IoT Thinkspeak
author: [chen]
category: [Lecture]
tags: [jekyll, ai]
---
---
## IoT Thinkspeak.com
### 應用功能說明


### 系統方塊圖
![](https://github.com/hjgyjg123/MCU-project/blob/main/images/ESP32_thingspeak_DHT11%E7%B3%BB%E7%B5%B1%E6%96%B9%E5%A1%8A%E5%9C%96.jpg?raw=true)

### ESP32_thingspeak_DHT11程式碼
![](https://github.com/hjgyjg123/MCU-project/blob/main/images/ESP32_thingspeak_DHT11%E7%A8%8B%E5%BC%8F%E7%A2%BC1.jpg?raw=true)
![](https://github.com/hjgyjg123/MCU-project/blob/main/images/ESP32_thingspeak_DHT11%E7%A8%8B%E5%BC%8F%E7%A2%BC2.jpg?raw=true)
![](https://github.com/hjgyjg123/MCU-project/blob/main/images/ESP32_thingspeak_DHT11%E7%A8%8B%E5%BC%8F%E7%A2%BC3.jpg?raw=true)

**ESP32_thingspeak_DHT11程式碼功能**

透過sensor DHT11讀取溫濕度，並將其數值透過程式碼呈現在thingspeak.com網頁
1. 定義所使用的DHT11溫濕度感測器的引腳和類型，include必要的函式庫
2. 設置Wi-Fi連接所需的SSID和密碼，並建立了與Thingspeak服務器的連接，透過API
3. 在setup() 函式中，確認是否連接到網路
4. 在loop() 函式中，通過WiFiClient創建TCP連接到Thingspeak網站
5. 構建要發送的URL，包括Thingspeak的API和溫度、濕度數據
6. 使用client.print()方法將HTTP GET請求發送到Thingspeak服務器
7. 每10秒一次，讀取數據

### 結果展示
picture; web-share" allowfullscreen></iframe>
![](https://github.com/yitingo41o/MCU-project/blob/522f3a066c45cff48a41b750a0e304239a4e7f71/images/%E8%9E%A2%E5%B9%95%E6%93%B7%E5%8F%96%E7%95%AB%E9%9D%A2%20(1).png?raw=true)
---
