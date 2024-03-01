	
| name       | comment                                  | address             | setting                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| ---------- | ---------------------------------------- | ------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| OPBT0 (PE) | Settings for Window Watchdog Timer(WDTB) | <CSAk_base> + 0x3A0 | **b31: OPWDRUNA**<br>Start mode of WDTBA<br>0: WDTBA software trigger start mode<br>1: WDTBA default start mode<br><br>**b30: OPWDWMSA**<br>Window open function mode of WDTBA<br>0: Window Size of Window Open Function is set by WDTBAWS\[1:0]. WDTBATIT outputs when the counter reaches 75% of the overflow setting defined by WDTBAMD.WDTBAOVF\[2:0]<br>1: Window size of window open function is set by WDTBAWOST. WDTBATIT outputs when WDTBAWIS matches WDT counter.<br><br>**b29-b27: OPWDOVFA\[2:0]**<br>Select the overflow interval time of WDTBA<br>000b: 2^9/WDTBTCKI<br>001b: 2^10/WDTBTCKI<br>010b: 2^11/WDTBTCKI<br>011b: 2^12/WDTBTCKI<br>100b: 2^13/WDTBTCKI<br>101b: 2^14/WDTBTCKI<br>110b: 2^15/WDTBTCKI<br>111b: 2^16/WDTBTCKI<br><br><br>**b26: OPWDINTA**<br>Enable/Disable a 75% interrupt request of WDTBA(WDTBATIT)<br>0: WDTBATIT disabled<br>1: WDTBATIT enabled<br><br>b25: Reserved<br>**b24-b23: OPWDWSA\[1:0]**<br>Select the window open period of WDTBA<br>00b: 25%<br>01b: 50%<br>10b: 75%<br>11b: 100%<br><br>b22-b19: Reserved<br>**b18: OPWVACA**<br>This bit specifies the trigger register for the generation of counter re-start triggers to keep the counter from overflowing.<br>0:  WDTBAWDTE (fixed)<br>1: WDTBAEVAC (variable)<br><br>b17: Reserved<br>**b16: OPWENA**<br>Enables/Disables WDTBA<br>0: WDTBA disabled<br>1: WDTBA enabled<br><br>**b15-b0: OPWDWOSTA**<br>Window oepn start register for setting the start timing of the window open. |

| name    | comment                   | address             | setting                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| ------- | ------------------------- | ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| S_OPBT0 | OCDID related option byte | <SSAk_base> + 0x300 | **b31: JPDBGIF_EN**<br>Security level 2 setting<br>1: Unset security level 2 (can use debug interface)<br>0: Set security level 2 (cannot use debug interface)<br><br>**b30: DRDY_EN**<br>Switching of the DRDY use for Nexus<br>1: Use DRDY<br>0: Not use DRDY (Use as GPIO)<br><br>b29-b25: Reserved<br>**b24: CPUBTMSK_EN**<br>PE0 boot mask enable (when the TRST=H mode only)<br>1: PE boot mask disable<br>0: PE boot mask enable<br><br>b23-b1: Reserved<br>**b0: OIDDIS**<br>OCD ID authentication disable<br>1: ID authentication is disabled (Cannot enter debug mode)<br>2: ID authentication is enabled |

^8cf87c

| name    | comment                           | address             | setting                                                                                                                                                                                                                                                                                                                                                                               |
| ------- | --------------------------------- | ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| S_OPBT3 | Customer ID A related option byte | <SSAk_base> + 0x30C | b31-b17: Reserved<br>**b16: CFRPF**<br>Code Flash Read Protection Flag, controlling section password authentication for **code flash** and **[[Flash Memory#^023cfa\|hardware property area]]]** read access at **debug mode**.<br>0: Customer ID A authentication for read access is needed<br>1: Customer ID A authentication for read access is not needed<br><br>b15-b0: Reserved |

^9514cf

| name    | comment                           | address             | setting                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| ------- | --------------------------------- | ------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| S_OPBT4 | Data Flash ID related option byte | <SSAk_base> + 0x310 | b31-b17: Reserved<br>**b16: DFRPF**<br>Data Flash Read Protection Flag, controlling section password authentication for **data area** read access at **debug mode**.<br>0: Data Flash ID authentication for read access is needed<br>1: Data Flash ID authentication for read access is not needed<br><br>b15-b1: Reserved<br>**b0: DPROT**<br>Data Flash Erase/Write Protection Flag, controlling section password authentication for **data area** erase/write access.<br>0: Data Flash ID authentication for erase/write access is needed<br>1: Data Flash ID authentication for erase/write access is not needed |

^75fd6a

| name    | comment                                  | address             | setting                                                                                                                                                                     |
| ------- | ---------------------------------------- | ------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| S_OPBT5 | Serial Programmer ID related option byte | <SSAk_base> + 0x314 | b31-b1: Reserved<br>**b0: SPD**<br>Serial Programmer Disable<br>0: All serial programmer commands is disabled by boot firmware<br>1: Serial programmer commands is enabled. |

^30ed0e

| name    | comment                      | address             | setting                                                                                                                                                                                                                                                 |
| ------- | ---------------------------- | ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| S_OPBT6 | RHSIF ID related option byte | <SSAk_base> + 0x318 | b31-b17: Reserved<br>**b16: HSIF_IDAUTH_NEED**<br>RHSIF Link Partner Authentication Setting<br>0: RHSIF ID authentication for Link Partner access is not needed<br>1: RHSIF ID authentication for Link Partner access is needed<br><br>b15-b0: Reserved |

^0379da

| name    | comment                                       | address             | setting                                                                                                                                                                                                                                                                                                                                                                                                                              |
| ------- | --------------------------------------------- | ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| S_OPBT7 | Debugging and calibration related option byte | <SSAk_base> + 0x31C | b31-b25: Reserved<br>**b24: DDRSTDIS**<br>Debugger Disconnection Reset control flag<br>0: debugger disconnection reset is enabled<br>1: debugger disconnection reset is disabled<br><br>b23-b17: Reserved<br>**b16: OVERLAYEN**<br>Overlay function disable/enable<br>0: Overlay function of CFU, GCFU is disabled<br>1: Overlay function of CFU, GCFU can be enabled<br><br>b15-b1: Reserved<br>**b0: AUDREN**<br>For U2A-EVA only. |

