# VirtualHost 



<br>

### 1. VirtualHost 
- 한 서버에 여러 개의 서비스를 호스팅할 때 사용
    

#### 2. 설정 방법(2가지) 
-  /etc/httpd/conf.d 로 이동 후 vhosts.conf 파일에 정의 
-  /etc/httpd/conf/httpd.conf 파일에  VirtualHost 태그를 넣어 직접 정의 
    
#### 2-1.  /etc/httpd/conf/httpd.conf 에 VirtualHost 태그 삽입 

```plaintext
<VirtualHost *:80>

  ServerName vhosttest.com
  DocumentRoot /var/www/html/vhosttest.com

  <Directory /var/www/html/vhosttest.com>
    AllowOverride All
  </Directory>

</VirtualHost>

```


#### 2-2. 지정한 DocumentRoot 경로 이동 -> 디렉토리 생성 -> html 파일 생성(/var/www/html/vhosttest.com/index.html) -> 아파치 재시작

```plaintext

cd /var/www/html/

mkdir vhosttest.com

vi index.html 

service httpd restart 

```

#### 2-3. 로컬에서 호스트 파일 수정(C:\Windows\System32\drivers\etc\hosts)
- 하위 내용 추가

```plaintext
aws퍼블릭IPv4주소 vhosttest.com
```


#### 2-4. 로컬 host파일의 aws퍼블릭IPv4주소 or vhosttest.com 를 입력하면 index.html 뜬다! 
![버추얼호스팅](/img/virtualhosting.PNG)

