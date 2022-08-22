# Apach 설정



<br>

### 1. /etc/httpd/conf/httpd.conf 


```plaintext
$ vi /etc/httpd/conf/httpd.conf
```


#### 1. ServerRoot : 
    - 아파치의 기본 루트 경로.
    - 절대 경로로 설정 
    - 디렉토리 경로 끝에 / 넣지 말 것
    - 웹 서버에 관련된 환경설정 파일, 에러, 로고 파일들이 존재하는 디렉토리 위치 지정

```plaintext
ServerRoot "/etc/httpd"
```

#### 2. Listen : 
    - 아파치가 실행되는 동안 수신할 포트 번호 지정
    - 아파치와 특정 IP 주소를 연결 or 아파치와 특정 포트들을 연결
    - 미 지정시 아파치 실행 안 됨
    - 443포트(SSL)는 httpd-ssl.conf 파일에서 따로 설정해야 함 


```plaintext
#Listen 12.34.56.78:80

Listen 80
```


#### 3. Include : 
    - LoadModule 라인에 DSO 로 구축된 모듈의 기능을 사용하기 위해서 선언되어야 함
    - http.conf 파일에 다른 설정 파일을 가져오기 위해서 Include 사용

```plaintext
Include conf.modules.d/*.conf
```

#### 4. User / Group : 
    - 웹 서버 실행할 때 소유권을 갖게되는 사용자와 그룹명 지정 

```plaintext
User apache
```


#### 5. ServerAdmin 
    - 서버 문제 발생시 관련 메일을 받는 메일 지정

```plaintext
ServerAdmin root@localhost
```

#### 6. ServerName
    - 서버의 도메인 이름
    - 서버의 도메인 네임과 포트 식별
    - 도메인이 없으면 IP 주소  

```plaintext
#ServerName www.example.com:80
```

#### 7. Directory
    - 전체 디렉토리에 대한 기본 옵션이나 권한 나타냄
    - 설정 항목
        - Options : CGI, Server Side Includes, 디렉터리의 심볼릭 링크 사용 여부 결정 
            - None : 접근 거부
            - All : 기본값. Multiviews 옵션 제외하고 모든 옵션 부여
            - Include : 서버측의 추가 정보 제공
            - Indexes : 
            - FollowSymLinks : 디렉토리 내 심볼릭 링크 사용 허가
            - ExecCGI : CGI 스크립트 실행 허가
            - IncludesNOEXEC : Server Side Includes 허가, 
            - MultiViews : 
        - AllowOverride : 사용자 인증 관련
            - None : 유저 인증 파일 사용 안 함, 아파치 서버는 이 파일 무시
            - ALL : httpd.conf 파일의 AccessFileName 지시자로 설정한 파일을 사용하고 관련 지시자를 사용 가능
            - AuthConfig :  AccessFileName 지시자에 명시한 파일에 대해 사용자 인증 지시자 사용 허락
            - FileInfo : AccessFileName 지시자로 설정한 파일에 대해 문서 유형을 제어하는 지시자 사용 허락 
            - Indexes : AccessFileName 지시자로 설정한 파일에 대해 디렉토리 Indexing을 제어하는 지시자 사용 허락
            - Limit : AccessFileName 지시자로 설정한 파일에 호스트 접근 제어하는 지시자 사용 허락
            - Options : AccessFileName 지시자에 명시한 파일에 대해 지시자 사용 허락 

        - Order : 해당 디렉터리에 접근하는 호스트 인증/ ip 주소 및 도메인에 대한 필터링 순서 설정
            - Allow(호스트 접근 허락) 
            - Deny(호스트 접근 거부) 
            - Allow from all(모든 호스트 접근 허락) 
            - Deny from all(모든 호스트 접근 거부)
        - Require : 접근 권한 허용 or 거부 


```plaintext
<Directory />
    AllowOverride none
    Require all denied
</Directory>
```


#### 8. DocumentRoot
    - 웹 문서가 위치하는 디렉토리 

```plaintext
DocumentRoot "/var/www/html"
```

#### 9. Directory "/var/www" ~ /Directory
    - 특정 디렉토리에 대한 기본 옵션이나 권한 등 설정 
    - 설정 항목 
    - /var/www 에 대하여 모든 접근을 허용


```plaintext

<Directory "/var/www">
    AllowOverride None
    Require all granted
</Directory>

```


#### 10. Directory "/var/www/html" ~ /Directory
    - Directoryindex를 가지고 있지 않은 디렉토리에 접근하려고 하는 경우 디렉토리의 내용
    - 아파치가 심볼릭 링크를 사용

```plaintext
<Directory "/var/www/html">
    Options Indexes FollowSymLinks
    AllowOverride None
    Require all granted
</Directory>
```

#### 11. IfModule dir_module
    - 모듈이 서버에 존재할 때만 적용
    - DirectoryIndex로 어떤 파일을 반환할 지 지정

```plaintext
<IfModule dir_module>
    DirectoryIndex index.html
</IfModule>
```

#### 12. Files ".ht*"
    - 파일 이름으로 범위 설정 
    - .ht*인 파일에 대해 모든 접근을 거부

```plaintext
<Files ".ht*">
    Require all denied
</Files>
```

#### 13. ErrorLog 
    - 웹 서버 에러 발생 시 에러에 관한 기록 파일 설정
    - <VirtualHost> 지시자에서 설정하지 않았을 경우 이곳에서 설정

```plaintext
ErrorLog "logs/error_log"
```

#### 14. LogLevel 
    - 로그 메시지의 크기 설정
    - 옵션 : debug, info, notice, warn, error, crit, alert, emerg 

```plaintext
LogLevel warn
```

#### 15. IfModule log_config_module
    - 로그 관련 형식 및 관련 파일 설정
    - LogFormat : 로그 형식 지정 
        - %h(호스트명), %l(리모트 로그 이름), %u(사용자 인증에 사용된 유저명), %t(시간), %r(요청한 내용의 첫번째 줄), %s(서버 상태), %b(전송량-바이트수), %{헤더}(요구된 헤더내용), %u(요구한 URL)
    - CustomLog : 웹 서버에 접근한 정보 기록하는 access_log 파일 위치 설정 
        - 옵션: combined(access, agent, referer 정보를 하나의 파일에 모두 저장) 
    - {Referer}i : Request에 포함된 Referer 값 

```plaintext

<IfModule log_config_module>
    LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\"" combined
    LogFormat "%h %l %u %t \"%r\" %>s %b" common

    <IfModule logio_module>
      LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\" %I %O" combinedio
    </IfModule>


    #CustomLog "logs/access_log" common
    CustomLog "logs/access_log" combined
</IfModule>

```


#### 16. IfModule alias_module 
    - CGI 스크립트 파일이 있는 경로 지정
    - alias_module이 존재할 때  /cgi-bin/foo 로 요청이 오면 /var/www/cgi-bin/foo 스크립트 실행

```plaintext
<IfModule alias_module>
    ScriptAlias /cgi-bin/ "/var/www/cgi-bin/"
</IfModule>
```

#### 17.  Directory "/var/www/cgi-bin"
    - /var/www/cgi-bin 디렉토리에 대하여 옵션을 제거, 모든 접근을 허용


```plaintext
<Directory "/var/www/cgi-bin">
    AllowOverride None
    Options None
    Require all granted
</Directory>
```



#### 18. IfModule mime_module
    - mime_module 존재 시 TypesConfig 미디어 타입 위치 설정 
    - AddType : 주어진 파일이름 확장자를 특정 콘텐츠 타입으로 매핑
    - AddOutputFilter : 주어진 파일이름 확장자를 출력 필터와 매핑 

```plaintext
<IfModule mime_module>
    
    TypesConfig /etc/mime.types
  
    AddType application/x-compress .Z
    AddType application/x-gzip .gz .tgz

    AddType text/html .shtml
    AddOutputFilter INCLUDES .shtml
</IfModule>
```



#### 19. AddDefaultCharset 
    - default characterset 인코딩을 UTF-8로 설정

```plaintext
AddDefaultCharset UTF-8
```

#### 20. IfModule mime_magic_module
    - magic file을 이용해 MIME 타입 결정
    - MIMEMagicFile : magic file의 경로 conf/magic로 지정

```plaintext
<IfModule mime_magic_module>
    MIMEMagicFile conf/magic
</IfModule>
```

#### 21. EnableSendfile 
    - httpd가 client에게 파일 컨텐츠를 전송할 때 커널로부터 sendfile 지원을 받을 것인지 여부를 설정


```plaintext
EnableSendfile on
```


#### 22. IfModule mod_http2.c
    - mod_http2.c : HTTP / 2 에 대한 지원 제공
    - Protocols directive : 서버 또는 virtualhost 에 대해 지원되는 프로토콜의 리스트를 특정

```plaintext
<IfModule mod_http2.c>
    Protocols h2 h2c http/1.1
</IfModule>
```



#### 23. IncludeOptional 
    - 설정파일 포함 
    - 파일 미존재 시 에러 발생하지 않음
    - conf.d/*.conf 파일을 포함 

```plaintext
IncludeOptional conf.d/*.conf
```














