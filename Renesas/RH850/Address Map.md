| Area       | Comment                                                  | Start Address | End Address | Size |
| ---------- | -------------------------------------------------------- | ------------- | ----------- | ---- |
| Code Flash | Single Map Mode: Bank A<br><br>Double Map Mode: Bank A/B | 0x0000 0000   | 0x003F FFFF | 4MB  |
| Code Flash | Single Map Mode: Bank B<br><br>Double Map Mode: --       | 0x0040 0000   | 0x007F FFFF | 4MB  |
In single map mode, 
* Bank A(user area 0) is always located in 0x0-0x3FFFFF(4MB). 
* Bank B(user area 1) is always located in 0x400000-0x7FFFFF(4MB). 
* The 2 user areas are located continuously in code flash

In double map mode, 
* the valid area(user area 0, front side) is always located in 0x0-0x3FFFFF(4MB). 
* the invalid area(user area 1, back side) is always located in 0x2000000-0x23FFFFF(4MB).
* The 2 user areas are located not continuously in code flash
* The banks in valid area and invalid area(Bank A and Bank B) can be swapped by Option Byte setting. 
* The user program can be executed from banks in valid area while banks in invalid area are being programmed/erased.