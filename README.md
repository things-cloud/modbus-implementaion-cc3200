## Platform: Windows
# Installation and settings of CC3200 and CCS:
1. Follow the installation and settings of CC3200 through this link: https://www.youtube.com/watch?v=xbh9I8waq5g
2. Go on project explorer-> file name and right click from mouse and go in properties. Include the file path in ARM compiler-> include options (#include search path)
```
"C:\TI\CC3200SDK_1.3.0\cc3200-sdk\netapps"
"C:\TI\ccsv6\tools\compiler\ti-cgt-arm_5.2.5\include"
"C:\TI\CC3200SDK_1.3.0\cc3200-sdk\netapps\json"
"C:\TI\CC3200SDK_1.3.0\cc3200-sdk\driverlib"
"C:\TI\CC3200SDK_1.3.0\cc3200-sdk\inc"
"C:\TI\CC3200SDK_1.3.0\cc3200-sdk\example\common"
"C:\TI\CC3200SDK_1.3.0\cc3200-sdk\simplelink"
"C:\TI\CC3200SDK_1.3.0\cc3200-sdk\simplelink\source"
"C:\TI\CC3200SDK_1.3.0\cc3200-sdk\simplelink_extlib\provisioninglib"
"C:\TI\CC3200SDK_1.3.0\cc3200-sdk\oslib"
"C:\TI\CC3200SDK_1.3.0\cc3200-sdk\netapps\http\client"
"C:\TI\CC3200SDK_1.3.0\cc3200-sdk\simplelink\include"
```

3. Include the file path in ARM linker-> file search 
->Include library file or command file:
```
"libc.a"
"C:\TI\CC3200SDK_1.3.0\cc3200-sdk\driverlib\ccs\Release\driverlib.a"
"C:\TI\CC3200SDK_1.3.0\cc3200-sdk\simplelink\ccs\NON_OS\simplelink.a"
"C:\TI\CC3200SDK_1.3.0\cc3200-sdk\oslib\ccs\free_rtos\free_rtos.a"
"C:\TI\CC3200SDK_1.3.0\cc3200-sdk\netapps\json\ccs\Release\json.a"
"D:\webclient\HTTPClientMinLib\webclient.a"
```

->Add <dir> to library search path
```
"${CG_TOOL_ROOT}/lib"
"C:\TI\CC3200SDK_1.3.0\cc3200-sdk\simplelink\include"
"${CC3200_SDK_ROOT}/netapps/json/ccs/Release/"
"C:\TI\CC3200SDK_1.3.0\cc3200-sdk\simplelink\ccs\OS"
"C:\TI\CC3200SDK_1.3.0\cc3200-sdk\netapps\http\client"
"C:\TI\CC3200SDK_1.3.0\cc3200-sdk\simplelink"
"C:\TI\CC3200SDK_1.3.0\cc3200-sdk\oslib\ccs\free_rtos"
"D:\webclient\HTTPClientMinLib"
```

4. Create the account in the “http://thingsio.thethingscloud.com”.


### Code modification: 
1. Firstly, change the host name and url respectively in main.c.
```
const char *soft_layer = "baas.thethingscloud.com"
#define CREATEA_SESSION_URI              "/api/sessions"
```
2. Change the user name and password in json format.
```
#define LOGIN_DATA                      "{\n\"email\":\"…………\",\n\"password\":\"……………\"\n}"
```
3. Change the json data format like:
Change the device ID and slave_id.
```
{ "data": {}, "device_id": "integer", "dts": "date-time", "slave_id": "integer" }
```
Example:
```
#define POST_HEADER                     "{"
#define POST_timestamp                           "\n\"dts\":"
#define POST_DEVICEID                          ",\n\"device_id\":201426,"
#define POST_SLAVE                              "\n\"slave_id\":"

#define POST_HEADER1                    "\n\"data\":"
#define POST_BEGIN                              "{"
//
//
//#define POST_CHUNK_1                            "\n\"device_type\":"
//#define POST_CHUNK_2                            "\n\"e_today\":"
//#define POST_CHUNK_3                            "\n\"e_total\":"
//#define POST_CHUNK_4                            "\n\"h_total\":"
//#define POST_CHUNK_5                            "\n\"running_status\":"
//#define POST_CHUNK_25                            "\n\"qac\":"
//
#define POST_END                                "\n}"
#define POST_TAIL1                              "\n}"
```
4. Change the address and number of slave because here, we are using Modbus protocol. You can remove Modbus when you are using any sensor.
```
const int Adress[5] = {10001, 10002, 10004, 10006 ,10012}
```
5. Change slave name according to the address
```
const char*name_list[12] = {"device_type","e_today","e_total", "h_total" , "running_status"}
```
6. construct_post_buffer()
1. Modify the case according to number of slaves :
2. start_val = 0;
              end_val = 12;
              break;
3. Change buffer location according to json data:
Example: 
```
strcpy(pcBufLocation,POST_HEADER);
pcBufLocation += strlen(POST_HEADER);
strcpy(pcBufLocation,POST_HEADER1);
pcBufLocation += strlen(POST_HEADER1);
```
7. In  HTTPLoginMethod and HTTPPostMethod , please make sure that content type should be “application/json”.
8. Go in project explorer->your_file->/example/common->common.h
Change SSID_NAME, SECURITY_KEY and SECURITY_TYPE = “SL_SEC_TYPE_WPA” or WPA2
9. Plugin the CC3200 through USB cable. Check in device manager the port name and number.
10. Check the pin configuration and shorting of pins in CC3200.
11. Go in debug option and debug it.
12. Open the Hercules or tera-term application to see the output/procedure of the CC3200.
13. Go in serial and set the Name: according to your port number, Baud rate: 19200, Data size: 8 and Parity: none and open it.
14. Go in CCS and play the code.
15. You can see all the response from the server, login details, connection with the wi-fi etc. in Hercules or tera-term.
