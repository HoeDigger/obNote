U2A8:

|             | Code Flash Memory | Data Flash Memory |                        |
| ----------- | ----------------- | ----------------- | ---------------------- |
| User Area   | User Boot Area    | Data Area         | Exclusively for ICUMHA |
| 2* 4 MBytes | 2 * 64 KBytes     | 2 * 128 KBytes    | 64 KBytes              |

U2A8 incorporates 8MBytes code flash allowing high-speed accesses and has 320KBytes data flash that is available for storing EEPROM data.

# Code Flash
* Program Unit: **512B**
* Erase Unit: In each User Area, 
	* **16KB** for 8 blocks(block0-block7), 0x0-0x1FFFF(128KB in total), 0x4000 for each
	* **64KB** for remaining blocks(block8-block69), 0x20000-0x3FFFFF(3968KB in total), 0x10000 for each

# Data Flash
## Data Area
* Two data areas: 128KB + 128KB and 64KB for ICUMHA exclusively (0xFF200000-0xFF24FFFF, 320KB in total)
	* Data Area 0: 0xFF200000-0xFF21FFFF, 128KB
	* Data Area 1: 0xFF220000-0xFF23FFFF, 128KB
	* Data Area 2: 0xFF240000-0xFF24FFFF, 64KB
* Program Unit: 4, 8, 16, 32, 64, 128 B
* Erase Unit: N * 4KB (N=1,2,3...)(within one data area)

## Hardware Property Area
^023cfa

HPA to configure settings of U2A8 in data flash memory.

| Name                                                     | Abbreviation | Comment                                                                                                                  | Address                             | Size                                       |
| -------------------------------------------------------- | ------------ | ------------------------------------------------------------------------------------------------------------------------ | ----------------------------------- | ------------------------------------------ |
| Extended Data Area                                       | EDA          | store any data to use by user software (2KB)                                                                             | 0xFF320000                          | 0x800(2KB)                                 |
| Configuration Setting Area                               | CSA          | store system configuration parameters, such as flash option byte, reset vector, software configuration option byte, etc. | 0xFF320800                          | 0x1000(2*2KB)                              |
| Security Setting Area                                    | SSA          | store security parameters, such as ID codes, security setting flags, etc.                                                | 0xFF321800                          | 0x1000(2*2KB)                              |
| Block Protection Area for FPSYS0                         | BPA          | store code flash protection settings, like OTP flag, etc.                                                                | 0xFF322800                          | 0x1000(2*2KB)                              |
| Erase Counter Area for User Area 0&1, User Boot Area 0&1 | ECA          | store the erase counter                                                                                                  | 0xFF325000                          | 0x3000(6*2KB)                              |
| Switch Area and TAG Area                                 | SA, TA       | to update CSA, SSA, BPA in an atomic and robust way                                                                      | SA: 0xFF373800<br>TA:<br>0xFF374800 | SA: <br>0x1000(2*2KB)<br>TA:<br>0x800(2KB) |


