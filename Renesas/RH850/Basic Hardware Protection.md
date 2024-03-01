## Mode and connection entry protection
- The 256bit SPID password authentication is needed to enter Serial Programming Mode.
- The 256bit OCDID password authentication is needed when On-chip Debug.
- The RHSIF ID authentication is needed to connect RHSIF Link Partner.

## Flash Protection by ID authentication
- 256bit Customer ID authentication is required for re-programming to User Area by each block.
- 256bit Customer ID authentication is required to read User/User Boot Area with debugger connected.
- 256bit Data Flash ID authentication is required for re-programming to Data Area
- 256bit Data Flash ID authentication is required to read Data Area with debugger connected
(Information of each flash area see [[Flash Memory]])

## Flash Protection by OTP
- User area on code flash supports OTP for each block.
- <span style="color:#ff0000">Flash extra area?</span> protection by OTP
- Dedicated configuration area supports OTP for each 4-bytes.
![[RH850_Basic_Hardware_Protection_IDCode.png]]

| name              | address                       | comment                                      |
| ----------------- | ----------------------------- | -------------------------------------------- |
| OCDIDn (n=0 to 7) | <SSAk_base> + 0x340 + 0x4 * n | ID code for Serial Programmer authentication |
Related option bytes: [[Option Byte#^30ed0e|S_OPBT5]]

# Security Functions in Debug Interfaces
- **Security level 1:**
	- OCD can be used after OCDID authentication.
- **Security level 2**
	- OCD cannot be used.
- With ICUMHA enabled, C&R authentication can be used for debugging.

| name              | address                       | comment                                             |
| ----------------- | ----------------------------- | --------------------------------------------------- |
| OCDIDn (n=0 to 7) | <SSAk_base> + 0x320 + 0x4 * n | ID code for On-chip debug connection authentication |
Related option bytes: [[Option Byte#^8cf87c|S_OPBT0]]
# Security Functions in Connection of RHSIF Link Partner
- **RHSIF ID authentication**
	- ID codes are checked for authenticity to protect RHSIF Link Partner connection.
	- With ICUMHA enabled, C&R authentication is added to security functions.
- **Access Window Setting**
	- User can set the access area from Link Partner that is configured the access window register. The access window register cannot set from Link Partner.

| name                | address                       | comment                                                  |
| ------------------- | ----------------------------- | -------------------------------------------------------- |
| RHSIFIDn (n=0 to 7) | <SSAk_base> + 0x3C0 + 0x4 * n | ID code for RHSIF Link Partner connection authentication |
Related option bytes: [[Option Byte#^0379da|S_OPBT6]]
# Code Flash/Data Area Protection
- Customer ID and Data Flash ID stored in SSA to protect Code Flash and Data Area
- User can unlock protection by inputting correct section password by software or external debugging tool
	- [ ] By SW, using CSAI API?
	- [ ] By external debugging tool, how?

| name                   | address                       | comment                                                                        |
| ---------------------- | ----------------------------- | ------------------------------------------------------------------------------ |
| CUSTOMERIDn (n=0 to 7) | <SSAk_base> + 0x360 + 0x4 * n | Customer ID A, ID code for code flash and configuration setting authentication |
Related option bytes: [[Option Byte#^9514cf|S_OPBT3]]

| name                    | address                       | comment                              |
| ----------------------- | ----------------------------- | ------------------------------------ |
| DATAFLASHIDn (n=0 to 7) | <SSAk_base> + 0x380 + 0x4 * n | ID code for data area authentication |
Related option bytes: [[Option Byte#^75fd6a|S_OPBT4]]


| name                    | address                       | comment                                              |
| ----------------------- | ----------------------------- | ---------------------------------------------------- |
| CUSTOMERIDBn (n=0 to 7) | <SSAk_base> + 0x3E0 + 0x4 * n | Customer ID B, ID code for code flash authentication |
| CUSTOMERIDCn (n=0 to 7) | <SSAk_base> + 0x400 + 0x4 * n | Customer ID C, ID code for code flash authentication |
