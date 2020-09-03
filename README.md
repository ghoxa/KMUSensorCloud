# KMUSensorCloud
KMU Sensor Cloud Helper   
This is a project called KMU Crowd Sensor Cloud Air&Home&Web. Copyright © [KOOKMINUNIVERSITY SENSOR CLOUD LAB](https://air.cs.kookmin.ac.kr/)      

**개요**   
공기질 센서(미세먼지, 온도, 습도 등)를 아두이노에 결합하여 ThingSpeak를 기반으로 수집하고 관리한다. 
이렇게 모인 데이터를 시각화하고 분석하여 여러가지 응용 앱을 만드는데 활용할 수 있으며, 데이터가 수년간 축적되면 기계학습을 통한 예측 기능을 추가하는 것이 가능하다. 
또한 센서 이외에 다른 장치를 부착하여 ThingSpeak, AWS 기반의 웹서버 프로그래밍을 통해 다양한 응용 및 확장이 가능하다. 
함께 모여 아두이노와 공기질 센서를 조립하고 테스트한 후, 각자 집에 설치하여 ThingSpeak를 통해 데이터를 수집하고 지도에 나타내는 것이 1차 목표이다.   

<br/>   

## 목차   
+ [1. 저자](#1-저자)   
+ [2. 목표](#2-목표)   
+ [3. 센서모듈 종류 및 소개](#3-센서모듈-종류-및-소개)   
  + [3.1 와이파이 모듈(ESP8266)](#31-와이파이-모듈esp8266)   
  + [3.2 미세먼지 센서(PMS7003m)](#32-미세먼지-센서pms7003m)   
  + [3.3 온도/습도/기압 센서(BME280)](#33-온도습도기압-센서bme280)   
+ [4. 아두이노 연결](#4-아두이노-연결)   
  + [4.1 아두이노 센서 연결](#41-아두이노-센서-연결)
  + [4.2 아두이노 설치](#42-아두이노-설치)   
  + [4.3 아두이노에 라이브러리 추가](#43-아두이노에-라이브러리-추가)   
  + [4.4 WiFi 통신 테스트](#44-wifi-통신-테스트)   
    + [4.4.1 포트 확인](#441-포트-확인)   
    + [4.4.2 업로드 및 AT command 실행](#442-업로드-및-at-command-실행)   
+ [5. ThingSpeak와 통신](#5-thingspeak와-통신)   
  + [5.1 ThingSpeak에 로그인](#51-thingspeak에-로그인)   
  + [5.2 ThingSpeak 데이터 시각화](#52-thingspeak-데이터-시각화)

<br/>   

## 1. 저자   
* 황선태 교수님(sthwang@kookmin.ac.kr)   
* 허대영 교수님(dyheo@kookmin.ac.kr)   
* 최규연 이사님(gychoi@cs.kookmin.ac.kr)   
* 김호준(hotem1234@naver.com)   
* 박세원(psw7347@gmail.com)   

<br/>   

## 2. 목표   
* 여러 공간의 대기질 센싱 데이터 수집,가시화, 분석, 예측   
* 첫째, ThingSpeak를 사용한 IoT 센서 플랫폼 구축 방법과 실습   
* 둘째, IoT Device와 ThingSpeak 연동과 실습    
* 셋째, 모바일 환경에서 접근하기 위한 웹 어플리케이션 구성   

<br/>   

## 3. 센서모듈 종류 및 소개   
### 3.1 와이파이 모듈(ESP8266)   
<img src="https://user-images.githubusercontent.com/63793178/92008373-0db94b00-ed82-11ea-9255-9d55aec0d4fc.jpeg" width="30%">     

**Technical Index**   
Parameter | Index
:------------ | :-------------
모듈명 | ESP8266 + ESP-01
동작명령 | UART AT Command
안테나 | On-Board Ceramic Antenna
통신방식 | 802.11 b/g/n 지원
통신속도<br/>(Baud Rate) | - 115200(Default) <br/> - 소프트웨어-시리얼: 57600bps ~ 2000000bps <br/> - 하드웨어-시리얼: 9600bps ~ 2000000bps
플래시 메모리 | 16M Byte
프로세서 스피드 | 80 ~ 160 Mhz
동작 전압 | 3.3V(300mA 이상)
크기 | 14.5 x 24.8 mm²   

_추가참조_:   

<br/>   

### 3.2 미세먼지 센서(PMS7003m)   
<img src="https://user-images.githubusercontent.com/63793178/92009149-0a728f00-ed83-11ea-8238-f80b9e00cb11.jpeg" width="25%">   

**Technical Index**   
Parameter | Index
:------------ | :------------  
모듈명 | PMS7003m
측정 범위 | 0.3 ~ 1.0, 1.0 ~ 2.5, 2.5 ~ 10 µm
유효 범위(PM 2.5) | 0 ~ 500 µm
최대 범위(PM 2.5) | ≥ 1000 µm
전압 | 5.0V
동작 온도 | -10 ~ +60 °C
동작 습도 | 0 ~ 99 %
크기 |  48 x 37 x 12 mm³

_추가참조_:   

<br/>   

### 3.3 온도/습도/기압 센서(BME280)   
<img src="https://user-images.githubusercontent.com/63793178/92009502-95ec2000-ed83-11ea-8f62-ba1e85fb941c.jpeg" width="20%">      

**Technical Index**   
Parameter | Index
:------------ | :------------  
모듈명 | BME280   
유효 범위 | 압력: 300 ~ 1100 hPa <br/> 온도: -40 ~ +85 °C
습도 센서 | 허용안정도: ±3% <br/> 샘플링 시간: 1 sec
기압 센서 | 감도오차: ±0.25%
전압 | 3.3 ~ 5.0 V  
전류 | 2mA
크기 | 2.5 x 2.5 x 0.93 mm³

_추가참조_: [BOSCH](https://www.bosch-sensortec.com/products/environmental-sensors/humidity-sensors-bme280/), [BME280 libraries](https://github.com/finitespace/BME280), [Specification](https://wiki.dfrobot.com/Gravity__I2C_BME280_Environmental_Sensor__Temperature,_Humidity,_Barometer__SKU__SEN0236)   

<br/>   

## 4. 아두이노 연결   
### 4.1 아두이노 센서 연결   

<img width="700" src ="https://user-images.githubusercontent.com/63793178/92118804-ede16000-ee31-11ea-8e05-4aa9e468d4ae.png">   

위의 그림과 같이 아두이노 보드와 센서 및 모듈을 각각 연결한다.   

[아두이노 센서 연결](https://air.cs.kookmin.ac.kr/디바이스/law-iot-ta2019) 에서 보다 자세하게 확인할 수 있다.    
또한 다음과 같이 아두이노 보드에 대한 기본 정보를 알고있으면 좋다.   

<img width = "500" src = "https://user-images.githubusercontent.com/63793178/92121794-a65cd300-ee35-11ea-9603-1be69fd95832.jpeg">   

그림과 같이 각 묶음별로 VCC는 VCC끼리 연결돼 있고, GND는 GND끼리 연결 돼 있다. 따라서 해당 묶음 내에서 어느 위치에 연결해도 무방하다.   
단, ***PIN*** 은 ***번호에 유의하여 연결***해야 한다.    

Parameter | Index
:------------ | :-------------
VCC | 5V 에 해당하는 (+)극 
GND | 0V 에 해당하는 기준전압   


모든 센서와 모듈을 연결 했다면, 아두이노와 컴퓨터를 다음 사진과 같이 케이블로 연결한다.    

<img width="300" src="https://user-images.githubusercontent.com/63793178/92117915-c2aa4100-ee30-11ea-9b4b-35bbdf728e4d.jpeg">      

<br/>   

### 4.2 아두이노 설치   
[https://www.arduino.cc](https://www.arduino.cc/en/Main/Software) 에서 ***SOFTWARE > DOWNLOADS*** 에 들어가, 운영체제에 맞게 설치를 한다.   

<img width="500" alt="스크린샷 2020-09-03 오후 9 40 44" src="https://user-images.githubusercontent.com/63793178/92116122-60e8d780-ee2e-11ea-8919-73051bf37a07.png" width="10%">   

<br/>   

### 4.3 아두이노에 라이브러리 추가   

아두이노가 모두 설치 되면, 아두이노 응용프로그램을 실행한다.   

<img width="500" alt="스크린샷 2020-09-03 오후 9 51 45" src="https://user-images.githubusercontent.com/63793178/92117352-0e101f80-ee30-11ea-8143-a442a4bcda48.png" width="10%">

다음과 같은 화면이 나오는데, 이 때, ***스케치 > 라이브러리 포함하기 > .ZIP 라이브러리 추가...*** 로 **KMUSensorCloud/library/** 의 zip 파일을 모두 추가한다. zip 파일은 다음과 같다.   

```   
Adafruit_BME280_Library_KMU-master.zip   

Adafruit_Sensor-master.zip   

PMS-master.zip   

SparkFun_ESP8266_AT_Arduino_Library_KMU-master.zip   
```   

<br/>   

### 4.4 WiFi 통신 테스트    
`wificonnect.ino`를 실행해, WiFi 통신을 점검한다.   
<br/>   

#### 4.4.1 포트 확인

<img width="500" alt="스크린샷 2020-09-03 오전 11 54 24" src="https://user-images.githubusercontent.com/63793178/92066252-6e757180-eddc-11ea-86e1-2b39e8d57fc1.png" width="10%">   

상단 메뉴 바의 ***툴 > 포트 > 시리얼 포트*** 에서, 시리얼 포트가 제대로 설정 돼 있는 지 확인한다.   

<br/>   

#### 4.4.2 업로드 및 AT command 실행   

<img width="500" alt="스크린샷 2020-09-03 오전 11 45 00" src="https://user-images.githubusercontent.com/63793178/92065879-78e33b80-eddb-11ea-82b0-c13cb6a40752.png" width="10%">

***업로드*** 를 클릭한 후, ***툴 > 시리얼 모니터*** 에서, 사진과 같이 ***Both NL & CR*** 로 설정해준 후, ***9600 보드레이트*** 인지 확인한다.   
확인이 됐다면, `AT` 를 입력한 후, `OK`가 출력되는지 확인한다. `OK`가 출력이 되면 정상이다.   

<br/>   

***3.1 와이파이 모듈(ESP8266)*** 에서 ESP-01 보드의 Default 통신 속도가 115200bps 임을 알 수 있다. 충분히 빠른속도로 가장 흔하게 사용되는 통신속도는 9600bps 이므로 속도를 영구적으로 변경해보자.   

<img width="500" alt="스크린샷 2020-09-03 오후 9 21 53" src="https://user-images.githubusercontent.com/63793178/92114288-8c1df780-ee2b-11ea-8e93-93526da61c7b.png" width="30%">   

모니터에 `AT+ UART_DEF=9600,8,1,0,0`을 입력한다.    
_참고로, AT + UART_DEF = (baudrate),(databits),(stopbits),(parity),(flow control) 를 의미한다._   

<br/>   

## 5. ThingSpeak와 통신   
### 5.1 ThingSpeak에 로그인   

[https://thingspeak.com](https://thingspeak.com) 에 접속해 로그인한다.   

<img width="700" src = "https://user-images.githubusercontent.com/63793178/92125626-31d86300-ee3a-11ea-96c8-a763b5d6a855.jpeg">   
<img width="700" src = "https://user-images.githubusercontent.com/63793178/92126047-add2ab00-ee3a-11ea-9246-4f975afaea89.jpeg">   

<br/>   

### 5.2 ThingSpeak 데이터 시각화   

<img width="700" src = "https://user-images.githubusercontent.com/63793178/92126165-ccd13d00-ee3a-11ea-8daf-46044ea809b6.jpeg">   

로그인 후  ***Channels > New Channel*** 을 클릭한다.   

<img width="700" src = "https://user-images.githubusercontent.com/63793178/92126223-dd81b300-ee3a-11ea-98d6-1f15644b6aec.jpeg">   
<img width="700" src = "https://user-images.githubusercontent.com/63793178/92126316-fe4a0880-ee3a-11ea-90f3-728e3cc390e6.jpeg">   
<img width="700" src = "https://user-images.githubusercontent.com/63793178/92126483-33eef180-ee3b-11ea-9adb-4feb09774d36.jpeg">   






