# IoT-Final-Project
Driver Health Monitoring Framework Model using oneM2M Standard

![image](https://github.com/CSIEProject/IoT-Final-Project/assets/155866585/7459bf09-ea5d-4b02-8be0-d8daca7b49ee)

System introduction:
1.
  MN-CSE以及IN-CSE中以Postman建立二AE: DRIVER_BIO_DATA (儲存driver生理指數)與INSTITUTION_CONTROL_DATA (儲存第三方機構下的判定) ，並各自在底下建置container DATA
  
2.
  MN-CSE在二container下以Postman建立subscription，nu指向各自在IN-CSE對應的container
  
3.
  以SendDriverBioData此APP作為生理指數sensor，輸入driver的heart rate (正常範圍60~100 bpm)與blood oxygen level(正常範圍>92%)會作為content instance透過NODE-RED MN-CSE flow chat中的[post]sensorData處理存入MN-CSE container DRIVER_BIO_DATA下的DATA
  
4.
  IN-CSE會透過訂閱收到MN-CSE中最新的資訊，並透過NODE-RED IN-CSE flow chart中的[post]DRIVER_BIO_DATA處理存入IN-CSE container DRIVER_BIO_DATA下的DATA
  
5.
  以GetDriverBioData作為終端應用，會透過NODE-RED IN-CSE flow chart中的[get]driverData獲取IN-CSE DRIVER_BIO_DATA中最新生理指數，只要其中一者生理指數不在正常範圍內，便會啟動Alarm，並將當前VehicleSpeed降速，即使生理指數未超出正常範圍，第三方機構亦可透過Institution Control啟動Alarm與降低VehicleSpeed
  
6.
  由Institution Control下的判定會透過NODE-RED MN-CSE flow chat中的[post]institutionData處理存入MN-CSE container INSTITUTION_CONTROL_DATA下的DATA， IN-CSE訂閱再透過NODE-RED IN-CSE flow chart中的[post]DRIVER_BIO_DATA處理存入IN-CSE container DRIVER_BIO_DATA下的DATA

How to operate:
1. 
  開啟IN-CSE、MN-CSE與NODE-RED，將附檔中Node-Red_IN-CSE.json與Node-Red_MN-CSE.json import進NODE-RED中並Deploy
  
2.
  開啟Postman，以附檔中Postman-Collection.json中之各項建立IN-CSE與MN-CSE中的AE、Container、Subscription
  
3.
  模擬器(e.g.,BlueStacks)中安裝SendDriverBioData.apk與GetDriverBioData.apk
  
4.
  以SendDriverBioData輸入driver生理數值，並透過GetDriverBioData得知判定，第三方機構可透過Institution Control自行決定是否啟動判定

