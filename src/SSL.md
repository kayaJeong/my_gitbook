# SSL 설치 & 환경설정 & letsencrypt 인증서 설치 


<br>

#### 0. /var/www/html/kayaho.ze.to 경로에  index.html을 생성

#### 1. mod_ssl 설치

```plaintext
yum update -y

yum install mod_ssl
```

#### 2. 설치 확인

```plaintext
ll /etc/httpd/modules/mod_ssl.so
```

#### 3. 아파치 서비스 재시작
```plaintext
service httpd restart
```

#### 4. 정상 구동 완료 시 443 포트 열렸는지 확인 

```plaintext
netstat -anlp | grep 443 
```

#### 5. vi /etc/httpd/conf/httpd.conf 에서 설정 

```plaintext

Listen 443

<VirtualHost *:80>  
    ServerName kayaho.ze.to
    DocumentRoot /var/www/html/kayaho.ze.to
    ServerAlias www.kayaho.ze.to
</VirtualHost>
```

#### 6. 아파치 재시작
```plaintext
apachctl restart 
```
<br>

# letsencrypt

#### 7. letsencrypt 인증서 설치 

Let's Encrypt 는 사용자에게 무료로 SSL/TLS 인증서를 발급해주는 비영리기관이며 한번 발급 받으면 90일간 사용이 가능하고 만료 30일 전에 갱신이 가능

- 설치에 5번 이상 실패하면 한 시간 동안 다시 시도할 수 없음

- no space left on device 오류


#### 8.  certbot 설치 

```plaintext
yum install certbot python2-certbot-apache

certbot --webroot --installer apache -w /var/www/html/kayaho.ze.to -d kayaho.ze.to
```

#### 9. 아래와 같은 내용이 추가적으로 생성되는지 확인   vi /etc/httpd/conf/httpd.conf 

```plaintext
<VirtualHost *:80>
    ServerName kayaho.ze.to
    DocumentRoot /var/www/html/kayaho.ze.to
    ServerAlias www.kayaho.ze.to
RewriteEngine on
RewriteCond %{SERVER_NAME} =kayaho.ze.to [OR]
RewriteCond %{SERVER_NAME} =www.kayaho.ze.to
RewriteRule ^ https://%{SERVER_NAME}%{REQUEST_URI} [END,NE,R=permanent]
</VirtualHost>
```

#### 10. kayaho.ze.to 도메인에 접속해 인증서가 발급되었는지 확인!!!



#### 11. 443에 대한 가상호스트 설정 및 인증서 파일 경로 지정




# conf.d/ssl.conf  ?
 