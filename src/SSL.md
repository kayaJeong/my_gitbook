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


#### 3. 정상 구동 완료 시 443 포트 열렸는지 확인 

```plaintext
netstat -anlp | grep 443 
```

#### 4. vi /etc/httpd/conf/httpd.conf 에서 포트 설정 

```plaintext

Listen 443

<VirtualHost *:80>  
    ServerName kayaho.ze.to
    DocumentRoot /var/www/html/kayaho.ze.to
    ServerAlias www.kayaho.ze.to
</VirtualHost>
```

#### 5. 아파치 재시작
```plaintext
Systemctl restart httpd 
```
<br>

# letsencrypt

#### 6. letsencrypt 인증서 설치 

Let's Encrypt 는 사용자에게 무료로 SSL/TLS 인증서를 발급해주는 비영리기관이며 한번 발급 받으면 90일간 사용이 가능하고 만료 30일 전에 갱신이 가능

- 설치에 5번 이상 실패하면 한 시간 동안 다시 시도할 수 없음

- no space left on device 오류


#### 7.  certbot 설치 

```plaintext
yum install certbot python2-certbot-apache

certbot --webroot --installer apache -w /var/www/html/kayaho.ze.to -d kayaho.ze.to
```

#### 8. 아래와 같은 내용이 추가적으로 생성되는지 확인   vi /etc/httpd/conf/httpd.conf 

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

#### 9. kayaho.ze.to 도메인에 접속해 인증서가 발급되었는지 확인!!!



#### 10. 443에 대한 가상호스트 설정 및 mod_ssl 을 설치했을경우 /etc/httpd/conf.d/ssl.conf 파일이 자동생성

- vi /etc/httpd/conf/httpd-le-ssl.conf


```plaintext
<IfModule mod_ssl.c>
<VirtualHost *:443>
        ServerName kayaho.ze.to
        DocumentRoot /var/www/html/kayaho.ze.to
        ServerAlias www.kayaho.ze.to
SSLCertificateFile /etc/letsencrypt/live/kayaho.ze.to/fullchain.pem
SSLCertificateKeyFile /etc/letsencrypt/live/kayaho.ze.to/privkey.pem
Include /etc/letsencrypt/options-ssl-apache.conf
</VirtualHost>
</IfModule>
```




# conf.d/ssl.conf  ?  vi /etc/httpd/conf.d/ssl.conf

- mod_ssl 에서 사용하는 설정 

#### 1. SSLPassPhraseDialog 
- 아파치가 구동될 때 SSL이 적용된 Virtualhost에 대한 Certificate와 암호화된 Private Key 읽어 옴
- 암호화된 PrivateKey 복호화하기위해 pass Phrase 요구  
    - Builtin : 관리자가 직접 암호화된 Private Key에 대한 Pass Phrase 입력할 수 있는 설정 
    - exec:/path/to/program : 암호화된 Private Key에 대한 외부 프로그램 설정하고 이 프로그램을 통해 Pass Phrase를 제공받음

```plaintext
SSLPassPhraseDialog exec:/usr/libexec/httpd-ssl-pass-dialog

```

#### 2. SSLSessionCache 
- Session Cache의 저장타입 설정
- 병렬적인 요청 프로세스 속도 개선해주는 역할 
    - none : Global/inter-process Session Cache 사용 안 하는 기본 설정임. 속도 저하 발생
    - Dbm:/path/to/datafile : 서버 프로세스들에 대한 로컬 SSLeay memory cache 동기화 위해 ㄹ컬 디스크에 있는 DBM hashfile 사용. 속도 향상 발생. 이 사용법 권장함

#### 3. SSLSessionCacheTimeout
- SSLSessionCache와 SSLeay internal memory cache에 저장된 정보들에 대한 타임아웃 초단위로 지정함. 
- 보통 300초 이상으로 지정

```plaintext
SSLSessionCache         shmcb:/run/httpd/sslcache(512000)
SSLSessionCacheTimeout  300
```


#### 4. SSLCrpytoDevice
- 엑셀러레이터 활성화 시키는 옵션 

```plaintext
SSLCryptoDevice builtin
```


#### 5. VirtualHost 설정
- 디폴트 가상호스트 설정 : "/var/www/html" 경로로 상속받음 

```plaintext

<VirtualHost _default_:443>

    ErrorLog logs/ssl_error_log
    TransferLog logs/ssl_access_log
    LogLevel warn
    SSLEngine on
    SSLProtocol all -SSLv3
    SSLCipherSuite HIGH:MEDIUM:!aNULL:!MD5:!SEED:!IDEA
    SSLCertificateFile /etc/pki/tls/certs/localhost.crt

    <Files ~ "\.(cgi|shtml|phtml|php3?)$">
        SSLOptions +StdEnvVars
    </Files>
    <Directory "/var/www/cgi-bin">
        SSLOptions +StdEnvVars
    </Directory>


    BrowserMatch "MSIE [2-5]" \
            nokeepalive ssl-unclean-shutdown \
            downgrade-1.0 force-response-1.0
    CustomLog logs/ssl_request_log \
            "%t %h %{SSL_PROTOCOL}x %{SSL_CIPHER}x \"%r\" %b"
</VirtualHost>
```

#### 6. ErrorLog 
- 에러로그 저장할 경로 설정
- 로그파일들은 httpd.conf 과 상관없음

```plaintext
ErrorLog logs/ssl_error_log
```

#### 7. TransferLog 
- 전송로그 저장 경로 설정

```plaintext
TransferLog logs/ssl_access_log
```

#### 8. LogLevel warn
- 에러로그 레벨 설정 

```plaintext
LogLevel warn
```

#### 9. SSLEngine on
- SSL/TLS 엔진 사용 여부 설정

```plaintext
SSLEngine on
```

#### 10. SSLProtocol all -SSLv3
- 사용가능한 SSL/TLS 프로토콜 버전 설정
- SSLv3, TLSv1, TLSv1.1, TLSv1.2, TLSv1.3, all


```plaintext
 SSLProtocol all -SSLv3
```

#### 10. SSLCipherSuite HIGH:MEDIUM:!aNULL:!MD5:!SEED:!IDEA
- SSL handshake phase 단계에서 클라이언트와 협상할 수 있는 Cipher Suite 설정. 
- : 로 구분되는 SSLeay cipher specification 들로 이루어진 스트링을 값으로 지정
- ! : 리스트로부터 cipher를 완전히 제거. 추후 재포함 할 수 없음 
- none : 리스트로부터 cipher 포함 가능. 리스트를 현재 위치로 가져다 놓고 제거하더라도 다시 포함 가능


```plaintext
 SSLCipherSuite HIGH:MEDIUM:!aNULL:!MD5:!SEED:!IDEA
```

#### 11. SSLCertificateFile /etc/pki/tls/certs/localhost.crt
- 인증서 파일 설정
- 서버에 대한 PEM-encoding 되어진 개인키를 지정 

```plaintext
 SSLCertificateFile /etc/pki/tls/certs/localhost.crt
```


#### 12. SSLOptions
- SSL engine 에 대한 옵션 설정 가능
    - FakeBasicAuth: 클라이언트 X.509를 기본인증으로 변환. 표준 Auth/DBMAuth 방법을 엑세스 제어에 사용 가능. User name은 클라이언트 X.509 인증서의 한 줄 버전임. 사용자로부터 암호를 얻을 수 없고 사용자 파일에 접근할 때마다 비밀번호 필요 
    - ExportCertData: 두개의 추가환경변수(SSL_CLIENT_CERT, SSL_SERVER_CERT) 내보냄. 
    - +StdEnvVars  : 표준 SSL/TLS 관련된 SSL_ 모든 환경변수를 내보냄. 기본적으로 해당 옵션은 꺼져있음. CGI, SSL 요청에 대해서만 내보냄.
    - StrictRequire: "Satisfy any" 상황에서"SSLRequireSSL" 이나 SSLRequire이 적용된 경우에도 접근 거부 
    - OptRenegotiate:  SSL 지시문이 사용될 때 최적화된 SSL 연결 재협상 처리가 가능

- cgi, sthml, phtml, php3을 포함하는 파일에 대해서 SSLOptions 활성화 


```plaintext
 <Files ~ "\.(cgi|shtml|phtml|php3?)$">
        SSLOptions +StdEnvVars
    </Files>
```

#### 13. Directory ~ /Directory
- cgi-bin 디렉토리에 대해 SSLOptions 활성화 

```plaintext
   <Directory "/var/www/cgi-bin">
        SSLOptions +StdEnvVars
    </Directory>
```

#### 14. BrowserMatch
- 구형 IE에서 발생하는 오류 처리하기 위한 설정
- IE2-5 브라우저에 대해서 http 프로토콜을 이용한 지속적인 연결을 무시 
- request가 이후 버전일지라도 HTTP/1.0으로 처리 
- HTTP/1.0 요청하는 클라이언트 -> HTTP/1.0 응답 강제


```plaintext
 BrowserMatch "MSIE [2-5]" \
            nokeepalive ssl-unclean-shutdown \
            downgrade-1.0 force-response-1.0
```


#### 15. CustomLog
- 사용자 정의 SSL 로그 파일의 홈.
- 가상 호스트 기반의 오류없는 SSL 로그 파일을 압축


```plaintext
CustomLog logs/ssl_request_log \
            "%t %h %{SSL_PROTOCOL}x %{SSL_CIPHER}x \"%r\" %b"
```