---
layout: post
title: ESP32 IoT webserver & client
author: [yiting]
category: [Lecture]
tags: [jekyll, ai]
---
---
## ESP32 慣性元件 Inertial Measurement Unit (IMU)

### 系統方塊圖
![](https://github.com/hjgyjg123/MCU-project/blob/main/images/MPU6050_DMP6%E7%B3%BB%E7%B5%B1%E6%96%B9%E5%A1%8A%E5%9C%96.jpg?raw=true)
### MPU6050_DMP6_Teapot程式碼
![](https://github.com/hjgyjg123/MCU-project/blob/main/images/MPU6050_DMP6%E7%A8%8B%E5%BC%8F%E7%A2%BC1.jpg?raw=true)
![](https://github.com/hjgyjg123/MCU-project/blob/main/images/MPU6050_DMP6%E7%A8%8B%E5%BC%8F%E7%A2%BC2.jpg?raw=true)
![](https://github.com/hjgyjg123/MCU-project/blob/main/images/MPU6050_DMP6%E7%A8%8B%E5%BC%8F%E7%A2%BC3.jpg?raw=true)
![](https://github.com/hjgyjg123/MCU-project/blob/main/images/MPU6050_DMP6%E7%A8%8B%E5%BC%8F%E7%A2%BC4.jpg?raw=true)
**MPU6050_DMP6_Teapot程式碼功能**
1. 通過include相應的函式庫（I2Cdev.h 和 MPU6050_6Axis_MotionApps_V6_12.h）來設置 MPU6050 模組
2. 初始化 I2C 通信和串口通信
3. 測試 MPU6050 模組的連接狀態
4. 初始化 DMP（Digital Motion Processor）並校準 MPU6050 模組的加速度計和陀螺儀
5. 啟用 DMP 功能，並設置中斷監聽以檢測新的數據包
6. 在主循環（loop）中讀取 MPU6050 的數據包，根據需要處理數據，例如顯示四元數、歐拉角、加速度等
7. 根據需求，將數據以不同的格式輸出，例如 InvenSense Teapot demo 格式，並通過 LED 燈的閃爍來指示模組的活動狀態

### MPU Plane程式碼
![](https://github.com/hjgyjg123/MCU-project/blob/main/images/MPUplane1.png?raw=true)
![](https://github.com/hjgyjg123/MCU-project/blob/main/images/MPUplane2.png?raw=true)
![](https://github.com/hjgyjg123/MCU-project/blob/main/images/MPUplane3.png?raw=true)
![](https://github.com/hjgyjg123/MCU-project/blob/main/images/MPUplane4.png?raw=true)
**MPU Plane程式碼功能**
通過MPU6050 DMP接收從傳感器獲取的姿態數據，並將其應用於3D飛機模型的模擬，以實現模型根據物體的姿態而旋轉
1. include必要的函式庫與變數宣告
2. 在setup()函數中，設置視窗大小、燈光和圖形渲染參數，並初始化串口連接
3. 在draw()函數中，定義了畫面的渲染邏輯
 - 設置背景顏色
 - 將3D場景的原點移到視窗中心
 - 根據從MPU6050接收到的四元數數據，將3D模型進行旋轉
 - 繪製3D飛機模型
4. serialEvent()函數是一個串口事件處理函數，當從串口接收到數據時被調用
 - 解析從MPU6050接收到的數據包
 - 提取四元數數據和相關的姿態角度數據
 - 將四元數轉換為歐拉角和偏航-俯仰-翻滾角度
 - 輸出數據以進行調試
### 結果展示
<iframe width="329" height="584" src="https://www.youtube.com/embed/MXfHoLkgoW4" title="20230511 211713" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

---

### 2.Kalman Filter
### 系統方塊圖
![](https://github.com/hjgyjg123/MCU-project/blob/main/images/MPU6050_KalmanFliter%E7%B3%BB%E7%B5%B1%E6%96%B9%E5%A1%8A%E5%9C%96.jpg?raw=true)
### MPU6050_KalmanFliter程式碼
![](https://github.com/hjgyjg123/MCU-project/blob/main/images/MPU6050_KalmanFliter%E7%A8%8B%E5%BC%8F%E7%A2%BC1.jpg?raw=true)
![](https://github.com/hjgyjg123/MCU-project/blob/main/images/MPU6050_KalmanFliter%E7%A8%8B%E5%BC%8F%E7%A2%BC2.jpg?raw=true)
![](https://github.com/hjgyjg123/MCU-project/blob/main/images/MPU6050_KalmanFliter%E7%A8%8B%E5%BC%8F%E7%A2%BC3.jpg?raw=true)
![](https://github.com/hjgyjg123/MCU-project/blob/main/images/MPU6050_KalmanFliter%E7%A8%8B%E5%BC%8F%E7%A2%BC4.jpg?raw=true)
**MPU6050_KalmanFliter程式碼功能**
1. include必要的函式庫與變數宣告
2. 在 setup() 函式中，初始化串口通信和 I2C 通信，並設置 MPU6050 感測器，獲取加速度計和陀螺儀的數據，並通過數據計算出初始角度
3. 在 loop() 函式中，負責持續更新感測器數據並計算物體的傾斜角度，使用加速度計和陀螺儀數據進行角度計算，並應用 Kalman 和 Complementary 兩種濾波器來獲得較為準確和穩定的角度估計值
4. 在 Serial.print() 函式中，負責數據輸出，通過串口輸出計算得到的角度數據，可以在監控式窗看到其計算結果
### 結果展示
<iframe width="329" height="584" src="https://www.youtube.com/embed/DpYj8SMo1Fc" title="20230511 212420" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

---

