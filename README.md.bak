# 麻醉监测系统项目文档

## 概述

### 环境

* Ubuntu20.04
* ROS neotic
* Python3.8
* Qt 5.14.2
* MySQL 8.0

### 操作流程

​	以下部分是快速部署的流程，用于非开发模式下的应用

* 克隆仓库

  ``` shell
  git clone --recursive https://github.com/Gastrointestinal-anesthesia/Monitor-project.git
  
  ```
* 在Monitor-project目录下运行install.sh

  ``` shell
  ./install.sh
  ```
* 此时可在应用程序菜单中找到Anesthesia Monitor，点击即可运行
* 也可以在Monitor-project目录下直接运行GUIMonitor.sh启动
  ``` shell
  ./GUIMonitor.sh
  ```
  
## 迈瑞BeneVision N15监测仪部分
### 监测仪总述

​	医疗设备统一采用Healt*h L*evel Seven（HL7）协议，该协议属于数据传输7层模型中的最高一层。HL7的具体协议约定格式可以参照[HL7官网](http://www.hl7.org/)。对于具体的设备，HL7中的条目应当查询设备的HL7手册，迈瑞监测仪可参考文档eGateway_HL7参考手册_V3.0_CH。

​	监测仪与计算机采用TCP/IP协议，在同一局域网下，监测仪连接至计算机并进行数据传输。在监测仪右下角“主菜单”中向右滑动，找到并点击“维护”选项，输入维护密码“888888”，进入维护页面。点击右上角的向右箭头，找到网络设置栏，在左侧找到HL7设置，将服务器地址设置为计算机的IP地址，输入一个合理的端口地址，打开参数发送，关闭波形发送，发送间隔调至最小（10s）。

### 数据传输与解算部分

​	为方便调试，TCP/IP采用Python的socket方案，数据解算利用Python的hl7parser包。

​	HL7数据中存在大量无效数据，从中筛选出有效的内容如下表所示

| 描述      | 心率（心电或指尖氧）                        | DIAP               | SYSP               | 指尖血氧             | 脑电双频指数               |
| --------- | ------------------------------------------- | ------------------ | ------------------ | -------------------- | -------------------------- |
| HL7标识符 | MDC_ECG_HEART_RATE或MDC_PULS_OXIM_PULS_RATE | MDC_PRESS_CUFF_DIA | MDC_PRESS_CUFF_SYS | MDC_PULS_OXIM_SAT_O2 | MNDRY_EEG_BISPECTRAL_INDEX |
| 数据范围  | 自然数                                      | 自然数             | 自然数             | 0-100                | 0-100                      |
| 数据类型  | int16                                       | int16              | int16              | int8                 | int8                       |

​	**备注**

* 每个包内可能存在部分项无有效数据，此时用-1表示
* 帧头的0x0B不能被hl7parser识别，故需手动去除
* 每个包的大小不确定，目前已知长度大于1024
* 由于发送频率很低（0.1Hz），将FIFO大小设置为1

### 数据传输部分

​	为方便后续MATLAB等工具的拓展，采用ROS实现数据的传输。对于每个监测器，存在唯一的ROS node，负责将message发送到唯一对应的ROS topic。同时，ROS的消息机制也提供了丰富的调试工具。

​	每个publisher对象会创建一个名为anesthesia_publisherx的ROS node，其中x为一个数字，自动生成，代表publisher的编号。anesthesia_publisherx会在名为/anesthesia_infox的ROS topic发布消息，消息的格式由Anesthesia.msg定义。

### 使用方法

​	详见[说明文档](https://github.com/Gastrointestinal-anesthesia/DataReceiver#readme)

## 图形界面与数据库部分

### 使用方法

​	详见[说明文档](https://github.com/Gastrointestinal-anesthesia/Qt-interface#readme)

## 语音识别部分
