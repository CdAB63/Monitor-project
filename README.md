# Anesthesia Monitoring System Project Documentation
## Overview
### Environment
- Ubuntu 20.04
- ROS Noetic
- Python 3.8
- Qt 5.14.2
- MySQL 8.0

## Operating Procedure
The following section describes the quick deployment process for non-development use.

## Clone the repository

```
shell
git clone --recursive https://github.com/Gastrointestinal-anesthesia/Monitor-project.git
```
Run install.sh in the Monitor-project directory

```
shell
./install.sh
```

At this point, you can find "Anesthesia Monitor" in the application menu and click it to run.

You can also run GUIMonitor.sh directly in the Monitor-project directory to start it.

```
shell
./GUIMonitor.sh
```

## Mindray BeneVision N15 Monitor Section
### Monitor Overview
Medical devices uniformly adopt the Health Level Seven (HL7) protocol, which belongs to the highest layer of the 7-layer data transmission model. The specific protocol format conventions of HL7 can be found on the HL7 official website. For specific devices, the HL7 entries should be looked up in the device's HL7 manual. For Mindray monitors, refer to the document eGateway_HL7 Reference Manual_V3.0_CH.

The monitor and computer use the TCP/IP protocol. Within the same local area network, the monitor connects to the computer and performs data transmission. On the monitor, swipe right in the "Main Menu" at the bottom right corner, find and click the "Maintenance" option, enter the maintenance password "888888", and enter the maintenance page. Click the right arrow in the upper right corner, find the network settings section, locate HL7 settings on the left side, set the server address to the computer's IP address, enter a reasonable port address, turn on parameter sending, turn off waveform sending, and adjust the sending interval to the minimum (10s).

## Data Transmission and Parsing Section
For ease of debugging, the TCP/IP implementation uses Python's socket approach, and data parsing uses the Python hl7parser package.

There is a large amount of invalid data in HL7 messages. The valid content filtered out is shown in the table below.

Description	Heart Rate (ECG or Finger Pulse Oximetry)	DIAP	SYSP	Finger Pulse Oximetry	Bispectral Index
HL7 Identifier	MDC_ECG_HEART_RATE or MDC_PULS_OXIM_PULS_RATE	MDC_PRESS_CUFF_DIA	MDC_PRESS_CUFF_SYS	MDC_PULS_OXIM_SAT_O2	MNDRY_EEG_BISPECTRAL_INDEX
Data Range	Natural numbers	Natural numbers	Natural numbers	0–100	0–100
Data Type	int16	int16	int16	int8	int8
Notes

Some items in each packet may have no valid data; in this case, they are represented by -1.

The frame header 0x0B cannot be recognized by hl7parser, so it must be manually removed.

The size of each packet is not fixed; the known length is greater than 1024.

Since the sending frequency is very low (0.1 Hz), the FIFO size is set to 1.

## Data Transmission Section
To facilitate subsequent expansion with tools such as MATLAB, ROS is used for data transmission. For each monitor, there is a unique ROS node responsible for sending messages to a uniquely corresponding ROS topic. At the same time, the ROS messaging mechanism also provides a rich set of debugging tools.

Each publisher object creates a ROS node named anesthesia_publisherx, where x is a number automatically generated to represent the publisher's index. anesthesia_publisherx publishes messages on the ROS topic named /anesthesia_infox, and the message format is defined by Anesthesia.msg.

## Usage Instructions
See the documentation for details.

## Graphical Interface and Database Section
### Usage Instructions
See the documentation for details.

## Speech Recognition Section
