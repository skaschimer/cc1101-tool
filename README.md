# cc1101-tool
RF tool based on CC1101 module and Arduino Pro Micro 8VMHz/3.3V. Allows using CLI to control CC1101 board over USB interface. Putty or any other serial terminal can be used. It has similar functionality to YardStick One but is cheaper and does not need specialized software. Allows for RF jamming and replay attacks as well. It has RAW  recording/replaying function which works exactly the same as in the Flipper Zero. Additional function is Radio Chat communicator

You simply connect your Arduino Pro Micro (Arduino Leonardo clone from Sparkfun) to USB port of your PC and launch Putty terminal to communicate with CC1101 module over USB Serial port ( /dev/ttyACM0 port in Linux, COMxx in Windows).

Also you may connect this device to Android OTG USB port in your smartphone for portable hacking and use USB Serial Terminal application with option CDC driver set to communicate with the device ( app : https://play.google.com/store/apps/details?id=de.kai_morich.serial_usb_terminal  ). When using Serial Terminal app on Android first go to the Settings in the app on your smartphone then upper side of the screen select "Send" and then set "Newline" as CR only. It is sending to many characters to the device and  extra character of Newline sometimes stops some of commands like "rxraw" for example.

Following commands are available :

    setmodulation <mode>         // set modulation mode. 0 = 2-FSK, 1 = GFSK, 2 = ASK/OOK, 3 = 4-FSK, 4 = MSK. 
    
    setmhz <frequency>           // Here you can set your basic frequency. default = 433.92).The cc1101 can: 300-348 MHZ, 387-464MHZ and 779-928MHZ.
    
    setdeviation <deviation>     // Set the Frequency deviation in kHz. Value from 1.58 to 380.85. Default is 47.60 kHz.
    
    setchannel <channel>         // Set the Channelnumber from 0 to 255. Default is cahnnel 0.
    
    setchsp <spacing>            // The channel spacing is multiplied by the channel number CHAN and added to the base frequency in kHz. Value from 25.39 to 405.45. Default is 199.95 kHz. 
    
    setrxbw <Receive bandwidh>   // Set the Receive Bandwidth in kHz. Value from 58.03 to 812.50. Default is 812.50 kHz.
    
    setdrate <datarate>          // Set the Data Rate in kBaud. Value from 0.02 to 1621.83. Default is 99.97 kBaud!
    
    setpa <power value>          // Set TxPower. The following settings are possible depending on the frequency band.  (-30  -20  -15  -10  -6    0    5    7    10   11   12) Default is max!
    
    setsyncmode  <sync mode>     // Combined sync-word qualifier mode. 0 = No preamble/sync. 1 = 16 sync word bits detected. 2 = 16/16 sync word bits detected. 3 = 30/32 sync word bits detected. 4 = No preamble/sync, carrier-sense above threshold. 5 = 15/16 + carrier-sense above threshold. 6 = 16/16 + carrier-sense above threshold. 7 = 30/32 + carrier-sense above threshold.
    
    setsyncword <LOW, HIGH>      // Set sync word. Must be the same for the transmitter and receiver. (Syncword high, Syncword low)
    
    setadrchk <address check>    // Controls address check configuration of received packages. 0 = No address check. 1 = Address check, no broadcast. 2 = Address check and 0 (0x00) broadcast. 3 = Address check and 0 (0x00) and 255 (0xFF) broadcast.
    
    setaddr <address>            // Address used for packet filtration. Optional broadcast addresses are 0 (0x00) and 255 (0xFF).

    setwhitedata <whitening>     // Turn data whitening on / off. 0 = Whitening off. 1 = Whitening on.
    
    setpktformat <pkt format>    // Format of RX and TX data. 0 = Normal mode, use FIFOs for RX and TX. 1 = Synchronous serial mode, Data in on GDO0 and data out on either of the GDOx pins. 2 = Random TX mode; sends random data using PN9 generator. Used for test. Works as normal mode, setting 0 (00), in RX. 3 = Asynchronous serial mode, Data in on GDO0 and data out on either of the GDOx pins.
    
    setlengthconfig <mode>       // 0 = Fixed packet length mode. 1 = Variable packet length mode. 2 = Infinite packet length mode. 3 = Reserved 
    
    setpacketlength <mode>       // Indicates the packet length when fixed packet length mode is enabled. If variable packet length mode is used, this value indicates the maximum packet length allowed.
    
    setcrc <mode>                // 1 = CRC calculation in TX and CRC check in RX enabled. 0 = CRC disabled for TX and RX.

    setcrcaf <mode>             // Enable automatic flush of RX FIFO when CRC is not OK. This requires that only one packet is in the RXIFIFO and that packet length is limited to the RX FIFO size.

    setdcfilteroff <mode>        // Disable digital DC blocking filter before demodulator. Only for data rates ≤ 250 kBaud The recommended IF frequency changes when the DC blocking is disabled. 1 = Disable (current optimized). 0 = Enable (better sensitivity).

    setmanchester <mode>         // Enables Manchester encoding/decoding. 0 = Disable. 1 = Enable.

    setfec <mode>                // Enable Forward Error Correction (FEC) with interleaving for packet payload (Only supported for fixed packet length mode. 0 = Disable. 1 = Enable.

    setpre <mode>                // Sets the minimum number of preamble bytes to be transmitted. Values: 0 : 2, 1 : 3, 2 : 4, 3 : 6, 4 : 8, 5 : 12, 6 : 16, 7 : 24

    setpqt <mode>                // Preamble quality estimator threshold. The preamble quality estimator increases an internal counter by one each time a bit is received that is different from the previous bit, and decreases the counter by 8 each time a bit is received that is the same as the last bit. A threshold of 4∙PQT for this counter is used to gate sync word detection. When PQT=0 a sync word is always accepted.

    setappendstatus <mode>       // When enabled, two status bytes will be appended to the payload of the packet. The status bytes contain RSSI and LQI values, as well as CRC OK.

    getrssi                      // Shows radio quality information about last received RF data frame.
    
    scan <start freq> <end freq> // Scan frequency range for the highest signal and display results

    rx                           // Enable or disable printing of received RF packets on serial terminal.

    tx  <hex-vals>               // Send the packet of max 60 bytes < hex values > hex values over RF 

    jam                          // Enable or disable continous jamming on selected band with selected modulation etc... 

    brute <usec> <nb-of-bits>    // Brute force garage gate with <number-of-bits> keyword where symbol length is <microseconds>

    rec                          // Enable or disable recording frames in the buffer.
    
    show                         // Show content of recording buffer
    
    add <hex-vals>               // Manually add single frame payload (max 60 hex values) to the buffer so it can be replayed
    
    flush                        // Clear the recording buffer

    play <N>                     // Replay 0 = all frames or N-th recorded frame

    rxraw <microseconds>         // Sniffs radio by sampling with <microsecond> interval and prints received bytes in hex

    addraw <hex-vals>            // Manually add chunks (max 60 hex values) to the buffer so they can be further replayed.

    recraw <microseconds>        // Recording RAW RF data with <microsecond> sampling interval
    
    showraw                      // Showing content of recording buffer in RAW format

    showbit                      // Showing content of recording buffer in RAW format as a stream of bits.

    playraw <microseconds>       // Replaying previously recorded RAW RF data with <microsecond> sampling interval

    save                         // Store recording buffer content in non-volatile memory
    
    load                         // Load the content from non-volatile memory to the recording buffer

    echo <mode>                  // Enable or disable Echo on serial terminal. 1 = enabled, 0 = disabled
    
    chat                         // switching device into chat mode 
    
    x                            // Stops activities like jamming/receiving/recording packets
    
    init                         // Restarts CC1101 board with default parameters 
 
The code uses SmartRC library (modified Electrohouse library by Little_S@tan) which allows to customize ALL transmission parameters in human readable format without using SmartRF studio from TI (CC1101 parameter customization tool). To use it please download following ZIP library from following github link https://github.com/LSatan/SmartRC-CC1101-Driver-Lib and attach it to the script in Arduino IDE.

Arduino Pro Micro board ( ATMEGA32U4 chip ) must support 3.3Volt VCC and 3.3V TTL logic because this is required by CC1101 board, otherwise you will fry CC1101 chip. Please follow this guide to setup your Arduino environment for Arduino Pro Micro board : https://learn.sparkfun.com/tutorials/pro-micro--fio-v3-hookup-guide/all

If you are having issues with uploading the code from Arduino IDE to the board, after pressing "Upload" in Arduino you have to immediatelly short GND+RST pins two times in few seconds. Then bootloader in Arduino Pro Micro will start (common issue) and upload will begin.

Connections to be made for ARDUINO PRO MICRO :

ARDUINO PRO MICRO 3.3V / 8MHz <-> CC1101 BOARD

DIGITAL PIN 3 ( PD0 / INT0 ) <-> CC1101 GDO0

DIGITAL PIN 9 ( PB5 ) <-> CC1101 GDO2

DIGITAL PIN 10 ( PB6 ) <-> CC1101 CSN / CS / SS

DIGITAL PIN 16 ( PB2 / MOSI ) <-> CC1101 MOSI / SI

DIGITAL PIN 14 ( PB3 / MISO ) <-> CC1101 MISO / SO

DIGITAL PIN 15 ( PB1 / SCK ) <-> CC1101 SCLK / CLK

VCC 3.3V  <-> CC1101 VCC

GND <-> CC1101 GND


----

If you want to use different Arduino Board, please change pin assignment in the beginning of the source code and adjust size of EEPROM/FLASH for storing recorded data  and size of SRAM memory

----

Example for ESP32 board :

#define RECORDINGBUFFERSIZE 4096   // Buffer for recording the frames

#define EPROMSIZE 512              // Size of EEPROM in your Arduino chip. For ESP32 it is Flash simulated 512 bytes only

// defining PINs set for ESP32 module

byte sck = 18;     //  GPIO 18

byte miso = 19;  //  GPIO 19

byte mosi = 23;  // GPIO

byte ss = 5;        // GPIO 5

int gdo0 = 2;     // GPIO 2

int gdo2 = 4;     // GPIO 4

----

Example for XIAO ESP32 C3 - ATTENTION ! This board may require some shielding of cables connected to GDO0 pin when using some cheapest CC1101 boards (green D-SUN f.ex.)  to properly work with RXRAW/RECRAW commands. It catches noise like "FF" in RXRAW/RECRAW. 

#define RECORDINGBUFFERSIZE 4096   // Buffer for recording the frames

#define EPROMSIZE 512              // Size of EEPROM in your Arduino chip. For ESP32 it is Flash simulated 512 bytes only

// defining PINs set for XIAO ESP32 C3

byte sck = 8;   // GPIO 8 

byte miso = 4;  // GPIO 4

byte mosi = 10; // GPIO 10

byte ss = 20;   // GPIO 20

int gdo0 = 21;  // GPIO 21

int gdo2 = 7;   // GPIO 7

----

Example for WEMOS D1 MINI module

#define RECORDINGBUFFERSIZE 4096   // Buffer for recording the frames

#define EPROMSIZE 4096              // Size of EEPROM in your Arduino chip. For  ESP8266 size is 4096

// defining PINs set for ESP8266 - WEMOS D1 MINI module

byte sck = 14;     // GPIO 14

byte miso = 12;  // GPIO 12

byte mosi = 13;  // GPIO 13

byte ss = 15;      // GPIO 15

int gdo0 = 5;     // GPIO 5

int gdo2 = 4;     // GPIO 4

----

Example for Arduino Nano board - ATTENTION ! I HAVE TESTED THIS BOARD AND IT REQUIRES TTL LOGIC COVERTER 5V<->3.3V TXS0108E ESPECIALLY FOR BOARD CC1101 : E07-M1101D, otherwise it does not work

#define RECORDINGBUFFERSIZE 1024   // Buffer for recording the frames

#define EPROMSIZE 1024             // Size of EEPROM in your Arduino chip. 

// defining PINs for Arduino NANO

byte sck = 13; // D13

byte miso = 12; // D12

byte mosi = 11; // D11

byte ss = 10; // D10

byte gdo0 = 6; // D6

byte gdo2 = 2; // D2

----

Example for Arduino Pro MINI board - ATTENTION ! I HAVE TESTED THIS BOARD AND IT REQUIRES TTL LOGIC COVERTER 5V<->3.3V TXS0108E ESPECIALLY FOR BOARD CC1101 : E07-M1101D, otherwise it does not work

#define RECORDINGBUFFERSIZE 1024   // Buffer for recording the frames

#define EPROMSIZE 1024             // Size of EEPROM in your Arduino chip. 


// defining PINs for Arduino PRO MINI

byte sck = 16; // D13

byte miso = 15; // D12

byte mosi = 14; // D11

byte ss = 13; // D10

byte gdo0 = 9; // D6

byte gdo2 = 5; // D2


----

Example for Raspberry Pi Pico / RP2040 board - ATTENTION ! I HAVE TESTED THIS BOARD AND IT USES 3.3V LOGIC 

#define RECORDINGBUFFERSIZE 4096   // Buffer for recording the frames

#define EPROMSIZE 512             // Size of EEPROM in your Arduino chip. 

// defining PINs for Raspberry Pi Pico 

// see pinout:  https://cdn-learn.adafruit.com/assets/assets/000/099/339/original/raspberry_pi_Pico-R3-Pinout-narrow.png

byte sck = 2;  

byte miso = 4;

byte mosi = 3;

byte ss = 5;

int gdo0 = 7;

int gdo2 = 6;


Attention ! There may be different SPI pins set for your Pico board :

byte miso = 16;

byte ss = 17; 

byte mosi = 19;

byte sck = 18;




--------------------------------------------------------------------------------------
First version of this project was presented in this video : https://youtu.be/iPVckkTjsd0
Using Universal Radio Hacker and my CC1101-tool is presented in following video : https://youtu.be/mdkEK_wmWJA
--------------------------------------------------------------------------------------


Change log :

08.06.2023 : optimized CLI 
- removed unnecessary parameters for commands RX, TX, JAM. 
- changed command JAMM to JAM.  
- optimized output of RX command - now will print directly hex values with no description when sniffer enabled.  
- corrected reaction for CR/LF when using with "Serial Terminal" application on USB OTG port on Android phones
- Added CHAT mode, if you have couple of these devices you may use it as and IRC like communicator on selected band/modulation/frequency/channel...


09.06.2023 : added RAW mode as in Flipper Zero 
- rxraw "interval microseconds", 
- recraw "interval usec", 
- playraw "interval usec", 
- showraw - for record & replay attacking. 
- buffer of 1536 bytes is used to store recording (in ATMEGA32U4, 1024 for Atmega Mega/Uno/Nano, 4096 or more for ESP32 boards). 
- after playing with RAW mode please  always enter "init" command to restart CC1101 chip. Don't worry about Low Memory warning during Arduino compilation it will work JUST FINE.. Enjoy :-)    



10.06.2023 : 
- added Arduino Mega/Nano/Uno version which requires TTL logic converter for 3.3V - TXS0108E. 
- added ESP32 version. 
- changed RECRAW <sampling interval> command to start recording RAW signal once something appears over the radio. 
- added command ADDRAW to enable manual composition of the signal in the buffer (by copying hex number chunks from Universal Radio Hacker tool for example). 
- added option SCAN <start freq> <end freq> to find a peak frequency for recording/jamming..

17.06.2023 : 
- added SAVE function to store recorded frames buffer into non-volatile EEPROM memory of the Arduino chip 
- added LOAD function to restore recorded frames from non-volatile memory and put them into recording buffer for replaying.
- added SHOWBIT command to display RAW data from the buffer as stream of bits.
- corrected ESP32 version which has problem with changing (char *) type to (byte *) due to different C++ compiler for ESP32 boards

18.06.2023 : 
- updated bit storage order in PLAYRAW, RXRAW, RECRAW commands to match the type used in Universal Radio Hacker tool : https://github.com/jopohl/urh

30.06.2023
- added debug message during CC1101 startup
- corrected EEPROM usage for ESP32 chip based Arduino - ESP32 has 512 bytes, ESP8266 has 4096 bytes
- added ESP32-WROOM version ( I have tested it succesfully with my own board )
- added ESP8266 - WEMOS D1 Mini version

08.07.2023
- corrected ESP8266 version, WDT watchdog restarts MOSTLY solved (this single core chip is heavy loaded with internal WiFI procedures and TCP IP stack). Code was successfuly tested on WEMOS D1 MINI clone and D-SUN CC1101 board. The advantage of using WEMOS D1 MINI  biggest size of FLASH simulated EEPROM for RF sequences storage.  ESP32 chips are dual core and my code runs better. 

13.07.2023
- changed default data rate for Packet Mode from 1.2Kbaud to 9.6Kbaud which removes problems with Watchdog Restart on ESP8266 board and improves stability

27.07.2023:
- rp2040 board added

18.08.2023:
- added command BRUTE <microseconds> <number of bits> for brute force attack on some DIP switches based garage gates. Sometimes the code hangs after executing full brute force cycle. Trying to find the root cause... Another bad news is that I have reached full FLASH capacity of ATMEGA32U4 so no more extensions are possible to the code for this chip. 

02.09.2023
- WIFI client mode for ESP8266 board added - there is a separate source code version wifi in the name. Before uploading the code you need to assign an IP address to the module , put correct default gateway as well as configure SSID of your WIFI router and the WIFI password in the code like below (defaults are for Android tethering access point). Wifi client mode can be used to extend widely the range between CC1101 device and you PC/smartphone which is used to control this board.  Beetween them you have external Access Point that will be "man in the middle" to extend the WIFI range...
- The ESP8266 board connects to your WIFI router/access point (you need 2nd mobile phone which will serve as an accesspoint for your own phone and ESP8266 board) and you do a TELNET to its IP address 192.168.43.100 from the other smartphone :
  
    IPAddress ip(192, 168, 43, 100);           // Local Static IP address

    IPAddress gateway(192, 168, 43, 1);        // Gateway IP address

    IPAddress subnet(255, 255, 255, 0);       // Subnet Mask

    const char ssid[] = "AndroidAP";          // Change to your Router SSID

    const char password[] = "password";      // Change to your Router Password
  
Example scenario for WIFI / telnet connection when ESP8266 is WIFI client :
   ESP8266 + CC1101 WIFI client at 192.168.43.100   <->   Smartphone #1 wifi tethering / wifi access point at 192.168.43.1    <->  Smartphone #2 WIFI client / Connectbot - telnet to 192.168.43.100.  
   ATTENTION ! When using WIFI versions I recommend to set ESP8266 CPU speed to 160 MHz : in arduino IDE - go to Tools, in dropdown list select "CPU Frequency 160MHz" instead of "CPU Frequency 80MHz". 

   

08.09.2023
- WIFI Access Point mode to ESP8266 board added - there is a separate source code version with wifi-ap in the name. Before uploading the code you can change an IP address to the module and  SSID for your ESP8266 board. Default is SSID "cc1101" and IP address for telnet "192.168.1.100". This is the simplest scenario, use Connectbot and Telnet protocol to connect to CC1101 board over TCP port 23. ATTENTION ! When using WIFI versions I recommend to set ESP8266 CPU speed to 160 MHz : in arduino IDE - go to Tools, in dropdown list select "CPU Frequency 160MHz" instead of "CPU Frequency 80MHz". 


22.11.2023
Corrected bug in showbit() function, all the thanks go to  jps1x2.


08.02.2024
Corrected bug in scan() function (not accepting MHz fractions in frequency range), all the thanks go to chris4soft 


  
Known Bugs : sometimes RX command does not work correctly after many big frames have been received (in packet mode, not in async mode). This may be due to some memory leak in SmartRC library. Still checking what is the reason. Keep attention to putting an argument <microseconds> to rxraw, playraw command - otherwise ESP8266 are restarting themselves when wifi in use (stack overflow).
    
    
