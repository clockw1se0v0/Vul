## **<font style="color:rgb(15, 17, 21);">Vulnerability Description</font>**

<font style="color:rgb(15, 17, 21);">There is a stored XSS vulnerability in the leave management module of the Student Manager system. When a low-privilege user submits a malicious payload, an administrator clicking to view it may lead to the compromise of the administrator account.</font>

## **<font style="color:rgb(15, 17, 21);">Vulnerability Analysis</font>**
In the `addLeave()`  method of `src/main/java/com/wdd/studentmanager/controller/LeaveController.java`, the lack of input filtering for user-supplied content results in this XSS vulnerability.

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/42371411/1770016315585-7a3b12a0-80f3-480e-9ccc-8ccef1ad8f5e.png)

## **<font style="color:rgb(15, 17, 21);">Proof of Concept</font>**

1. <font style="color:rgb(15, 17, 21);">Log in as a student account.</font>
2. <font style="color:rgb(15, 17, 21);">In the leave management section, add a new leave request and insert the following payload into the "Reason for Leave" field:</font>

```plain
<a href='javascript:alert(document.cookie)'>test</a>
```

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/42371411/1770015639855-2f3c7af6-92e6-4f41-8020-2d742ab6ac43.png)

3. <font style="color:rgb(15, 17, 21);">Log in as an administrator account (admin:123456).</font>
4. <font style="color:rgb(15, 17, 21);">Navigate to the leave management module and click to view the leave application submitted by the student account.</font>
5. <font style="color:rgb(15, 17, 21);">The XSS payload is successfully triggered.</font>

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/42371411/1770015754093-d820db5f-ea24-4937-8c4d-de52579eff3d.png)

