# **WIRELESS TRANSMISSION WITH RADIO FREQUENCY**

## **Introduction**

The objective of this project was to send and receive data between MCU and MCU, computer and computer or MCU and computer with no wire. This project  focuses on setting modules and config MCU to comunicate between MCU and CC1101 using buffer to store data. 

## **Dependent**

	IDE: Code Composer Studio version 7.3.
	Library: TivaWare C Series 2.1.4.178.
	TivaC TM4C123GH6PM.
	RF UART CC1101 433Mhz HC-11.
		Supply voltage: 3.3-5 VDC.
		Transmission frequency: 433Mhz.
		Transmission distance: 0-200m.
		Communication: UART/TTL.
		Broadcast power: 1-10 dBm.
	
## **Structure**

![](https://sv1.uphinhnhanh.com/images/2018/06/20/image53571.png)


## **How to build, run the demo and run the tests**


### A. Config two RFs 
Connect CC1101 to USB-TTL (CP2102), make RF entering AT mode.

          |     C1101 |         | CP2102    | 
          |     SET   |   --    | GND       |  
          |     TX    |   --    | RX        | 
          |     RX    |   --    | TX        |

![](https://sv1.uphinhnhanh.com/images/2018/06/20/image7e812.png)


        
 Checking driver software in Device Manager (Window + X and press M), if missing, get driver at [here](https://www.silabs.com/products/development-tools/software/usb-to-uart-bridge-vcp-drivers).
        
 Open [Hercules](https://www.hw-group.com/software/hercules-setup-utility) and setting up as the picture below. Note: Serial name can get in Device Manager.

 ![](https://sv1.uphinhnhanh.com/images/2018/06/20/image8aaac.png)

 Two RF must have the same channel, address, channel and non must be 0.

 Using these command:
1.	AT : test the mode, exp: will reply OK if correct.
2.	AT+Axxx: change module address, from 000-255.
3.	AT+Bxxxx: change baud rate, can be set to:  2400, 4800, 9600, 115200, 19200, 38400, 57600. Default is 9600.
4.	AT+Cxxx: change channel, from 001 to 127. Default is 001 (suggest to use 1 ~ 100 for stable performance).
5.	AT+Px: set wireless power, x is from 1 to 8, base on distance. Default is 8
6.	AT+Rx: show all parameter setting.
   For more information, download [datasheet](https://github.com/PIFClub/TIVAC123_CC1101/tree/master/TIVA_CC1101_UART/~Docs).

### B.	Setting TivaC

![](https://sv1.uphinhnhanh.com/images/2018/06/20/imaged44be.png)



Download source code.
Creating new project and move necessary files because not matching location of Tivaware driverlib and compiler can make some issues.

 Connect TivaC and RF carefully and connect TivaC to computer.
## ## 
          |   TIVA C   |       |  MODULE CC1101 |
          |         3v3|-------|VCC             |
          |         GND|-------|GND             | 
          |         PB0|-------|TXD             |
          |         PB1|-------|RXD             |
 

If TivaC in send mode: uncomment this line.

			//#define TX

If TivaC in receive mode : comment this line.

			#define TX

Build, upload and run code.

Some available functions:

		UART_C1101_Write() : send data with length.

		UART_C1101_WriteCMD() : send data until ‘\0’ char.

		UART_C1101_Read() : read data with length.




## **Result**

Test with echo program:




     uint8_t GetData[50];

     While(1)
       {
          UART_C1101_Read(GetData,6);
          SysCtlDelay(SysCtlClockGet()/30);
          UART_C1101_WriteCMD(GetData);
          SysCtlDelay(SysCtlClockGet()/30);
       }

![](http://sv1.upsieutoc.com/2018/06/21/image53b76e652d4649e8.png)

Data is received and resent to terminal successfully.

If you failed, you could check some things below:

 •	Two RFs must have the same setting.

 •	Connections (Vcc, GND, Tx, Rx).

 •	Code has no bug and correct baudrate with RFs.

 •	Your lucky :D 


	

***
