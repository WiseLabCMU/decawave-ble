# Beluga (UWB-BLE)
## Decawave controller for nRF52 that uses BLE for discovery

##  Required Software:
  
    SEGGER Embedded Studio ( Tested on Windows with v4.22 and Ubuntu with v4.52b)
  
    Serial Monitor w/ write capability (Tested with Arduino Serial Monitor 1.8.9 & 1.8.12)

## Main Project Directory:

  Run the Following CD command once inside cloned repo:
 
    cd Beluga/Application

## How to Open Project in Segger:

    1.) Go to Project Directory mentioned above
    2.) Open the segger embedded studio project file (the one labeled EMPROJECT File)

## To Test Code:
    0.) Plug in Decawave Module
    1.) Open up segger project
    2.) Go to toolbar -> build -> first option (F7) (build ble_hrs_....)
    3.) Go to toolbar -> target -> download ble_app_hrs... (ctrl+T, L)
    4.) Open up serial monitor that allows you to send data (We like the Aduino IDE's Serial Monitor (Tested on Arduino IDE 1.8.12)
  
## Running the Code

###  After downloading and opening Arduino, follow these instructions
  
    NOTE: This must be done every time the decawave module is restarted/plugged in, flash memory not supported yet
    Run the Following Commands in this Order, other orders may lead to undefined behaviour
  
      1.) AT+ID <number>  (for now use id greater than 0)
      2.) AT+STARTBLE
        This start BLE broadcasting/recieving scheme, now the node is visible and can see other nodes with ble on as well
        Neighbor List Will Start Printing and Start to be populated with nearby nodes
      3.) AT+STARTUWB
        This starts UWB initiating/responding, now the node can be ranged with, and range to other nodes
        Neighbor List will now populate range fields
       
       
#### The AT+BOOTMODE command
    
    AT+BOOTMODE <mode>  Determines how the node should behave when reset/powered on
    <mode> = 0  -  Do nothing on startup, by default BLE and UWB off.
    <mode> = 1  -  Start BLE broadcasting/recieving on startup
    <mode> = 2  -  Start BOTH BLE and UWB on startup, full functionality.

    Defualt setting: 0
    
    NOTE: For BOOTMODEs 1 and 2, The AT+ID command must have been previously run, the last set ID will be used on startup. 

#### The AT+RATE command
    
    AT+RATE <freq>  Determines the frequency that the node send poll message
    <freq> = 0-100 
    
    Defualt setting: 100
    
    NOTE: When frequency is 0, the node is in listening mode. (It only receives message passively)

#### The AT+CHANNEL command
    
    AT+CHANNEL <channel>  Determines the UWB signal's channel
    Available <channel> options: 1, 2, 3, 4, 5, 7
    
    Defualt setting: 5

    NOTE: The corresponding centre frequency and bandwidth of each channel please reference DW1000 User Manual

#### The AT+TXPOWER command
    
    AT+TXPOWER <mode>  Determines the UWB transmitter power setting
    <mode> = 0  -  Default power supply
    <mode> = 1  -  Maximum power supply
    
    Defualt setting: 0

    NOTE: Increasing transmitter power supply can help UWB to maximum range, but the maximum power supply exceed restricted transmit power level regulation.

#### The AT+RESET command
    
    AT+RESET   Clear flash memory configuration
    This command will reset all user configuration including node's ID, BOOTMODE, RATE, CHANNEL, TXPOWER
    
    NOTE: The node should be re-configure follow the above instructions to avoid undefinded behavior.
    

#### LED Layout: 

   ![LED Layout](https://github.com/WiseLabCMU/decawave-ble/blob/master/images/decawaveLED.png?raw=true)
  
# ADDITIONAL NOTES/ To be Added: 
  
    1.) Neighbor List Eviction and Sorting in Place, but timing parameters are hardcoded, they need to be exposed in the AT command set
    2.) Right now Poll frequency is set by taking the AT+RATE <rate> command and picking rand(rate, rate+50) in ms. This should probably be a better function than just random
    3.) Some BLE softdevice calls are spread throughout different FreeRTOS tasks, there should be 1 task that makes calls to the softdevice that the other tasks send signals to.
    4.) Move all neighbor list editing operations to one task, currently they are also spread out.
    5.) There are a lot of extraneous functions left over from nordic example code. These should be deleted to clean up code.
    6.) Codebase should be refactored into standalone project that imports libraries from nrf sdk and decawave sdk. 
    7.) Create list of all available+planned AT commands
    8.) Make code more efficient - lots of inefficiencies with sorting, searching through neighbor list   
    
  
  

