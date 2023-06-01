---
layout: post
title: 藍牙遙控機器人實作
author: [yiting_chiu]
category: [homework]
tags: [jekyll, ai]
---

This project is to implement a bluetooth remote controlled robotcar.


## 藍牙遙控機器人
![](https://github.com/rkuo2023/MCU-project/blob/main/images/ESP32_RoboCar.jpg?raw=true)

### 應用功能說明
1. Bluetooth remote control App 
2. two-wheel robocar


### 設計考量與相關技術
**系統設計考量：**<br>
1. 操作方式:藍牙遙控手機App
2. 移動方式:兩輪 
3. 供電方式:鋰電池 3.7V x2
4. 聯網方式:藍牙

**所需相關技術：**
1. MIT App Inventor 2 手機程式設計 
2. Arduino程式設計



**所需相關套件:**
![](https://image.ruten.com.tw/g2/8/d4/16/21440347657238_872.jpg)

### 系統方塊圖
![](https://github.com/yitingo41o/MCU-project/blob/main/images/%E6%96%B0%E5%A2%9E%E5%B0%91%E9%87%8F%E5%85%A7%E6%96%87.jpg?raw=true)

### 實作影片
<iframe width="473" height="841" src="https://www.youtube.com/embed/f2BniY4nAnU" title="手機藍牙遙控, 或WebUI 遙控(利用手機熱點WiFi連線)1" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

<iframe width="473" height="841" src="https://www.youtube.com/embed/3FaQAwD77h4" title="手機藍牙遙控, 或WebUI 遙控(利用手機熱點WiFi連線)2" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

### 程式碼
// PWM to DRV8833 dual H-bridge motor driver, PWM freq. = 1000 Hz
// ESP32 Webserver to receive commands to control RoboCar

#include <WiFi.h>
#include <WebServer.h>
#include <ESP32MotorControl.h> 

// DRV8833 pin connection
#define IN1pin 16  
#define IN2pin 17 
#define IN3pin 18 
#define IN4pin 19

#define motorR 0
#define motorL 1
#define FULLSPEED 100
#define HALFSPEED 50

ESP32MotorControl motor;


/* Set these to your desired credentials. */
const char *ssid = "Your_SSID";
const char *password = "Your_Password";

WebServer server(80); // Set web server port number to 80

const String HTTP_PAGE_HEAD = "<!DOCTYPE html><html lang=\"en\"><head><meta name=\"viewport\" content=\"width=device-width, initial-scale=1, user-scalable=no\"/><title>{v}</title>";
const String HTTP_PAGE_STYLE = "<style>.c{text-align: center;} div,input{padding:5px;font-size:1em;}  input{width:90%;}  body{text-align: center;font-family:verdana;} button{border:0;border-radius:0.6rem;background-color:#1fb3ec;color:#fdd;line-height:2.4rem;font-size:1.2rem;width:100%;} .q{float: right;width: 64px;text-align: right;} .button1 {background-color: #4CAF50;} .button2 {background-color: #008CBA;} .button3 {background-color: #f44336;} .button4 {background-color: #e7e7e7; color: black;} .button5 {background-color: #555555;} </style>";
const String HTTP_PAGE_SCRIPT = "<script>function c(l){document.getElementById('s').value=l.innerText||l.textContent;document.getElementById('p').focus();}</script>";
const String HTTP_PAGE_BODY= "</head><body><div style='text-align:left;display:inline-block;min-width:260px;'>";
const String HTTP_PAGE_FORM = "<form action=\"/cmd1\" method=\"get\"><button class=\"button1\">Forward</button></form></br><form action=\"/cmd2\" method=\"get\"><button class=\"button2\">Backward</button></form></br><form action=\"/cmd3\" method=\"get\"><button class=\"button3\">Right</button></form></br><form action=\"/cmd4\" method=\"get\"><button class=\"button4\">Left</button></form></br><form action=\"/cmd5\" method=\"get\"><button class=\"button5\">Stop</button></form></br></div>";
const String HTTP_WEBPAGE = HTTP_PAGE_HEAD + HTTP_PAGE_STYLE + HTTP_PAGE_SCRIPT + HTTP_PAGE_BODY + HTTP_PAGE_FORM;
const String HTTP_PAGE_END = "</div></body></html>";

// Current time
unsigned long currentTime = millis();
// Previous time
unsigned long previousTime = 0; 
// Define timeout time in milliseconds (example: 2000ms = 2s)
const long timeoutTime = 2000;

int speed = HALFSPEED;

void handleRoot() {
  String s  = HTTP_WEBPAGE; 
  s += HTTP_PAGE_END;
  server.send(200, "text/html", s);
}

void cmd1() {
  String s  = HTTP_WEBPAGE; 
  s += HTTP_PAGE_END;  
  server.send(200, "text/html", s);
  motor.motorForward(motorR, speed);  
  motor.motorForward(motorL, speed);
  Serial.println("Move Forward");   
}

void cmd2() {
  String s  = HTTP_WEBPAGE; 
  s += HTTP_PAGE_END;  
  server.send(200, "text/html", s);
  motor.motorReverse(motorR, speed);
  motor.motorReverse(motorL, speed);
  Serial.println("Move Backward");     
}

void cmd3() {
  String s  = HTTP_WEBPAGE; 
  s += HTTP_PAGE_END;  
  server.send(200, "text/html", s);
  motor.motorReverse(motorR, speed);  
  motor.motorForward(motorL, speed);
  Serial.println("Turn Right");    
}

void cmd4() {
  String s  = HTTP_WEBPAGE; 
  s += HTTP_PAGE_END;  
  server.send(200, "text/html", s);
  motor.motorForward(motorR, speed);
  motor.motorReverse(motorL, speed);
  Serial.println("Turn Left"); 
}

void cmd5() {
  String s  = HTTP_WEBPAGE; 
  s += HTTP_PAGE_END;  
  server.send(200, "text/html", s);
  motor.motorStop(motorR);
  motor.motorStop(motorL);
  Serial.println("Motor Stop");
}

void setup() {
  Serial.begin(115200);
  Serial.println("Motor Pins assigned...");
  motor.attachMotors(IN1pin, IN2pin, IN3pin, IN4pin);
  
  // Connect to Wi-Fi network with SSID and password
  Serial.print("Connecting to ");
  Serial.println(ssid);
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  // Print local IP address and start web server
  Serial.println("");
  Serial.println("WiFi connected.");
  Serial.println("IP address: ");
  Serial.println(WiFi.localIP());

  server.on("/", handleRoot);
  server.on("/cmd1", cmd1);
  server.on("/cmd2", cmd2);
  server.on("/cmd3", cmd3);
  server.on("/cmd4", cmd4);  
  server.on("/cmd5", cmd5);  

  Serial.println("HTTP server started");
  server.begin();

  motor.motorStop(motorR);
  motor.motorStop(motorL);
}

void loop() {
  server.handleClient();
}

建立Web伺服器和回應HTTP請求。在Web伺服器上設置了5個按鈕，分別代表向前、向後、向右、向左和停止馬達。當使用者按下其中一個按鈕時，Web伺服器會發送HTTP GET請求到ESP32上，然後ESP32會根據所接收到的命令來控制馬達的運動。
總體來說，這個程式碼實現了一個基本的ESP32控制機器人小車的系統，並提供了一個簡單的Web界面來控制它。
handleRoot（） 函數會產生一個HTML網頁並將其作為回應發送。其他幾個函數（cmd1，cmd2，cmd3，cmd4和cmd5）分別處理前進、後退、右轉、左轉和停止的命令。在這些函數中，它們會向馬達驅動器發送特定的指令，以控制機器人移動方向和速度。
然後再使用ESP8266控制兩個直流馬達，並透過網頁介面控制其移動方向和停止。
<br>
<br>

*This site was last updated {{ site.time | date: "%B %d, %Y" }}.*

