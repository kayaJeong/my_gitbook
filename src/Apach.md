# Apach 설정



<br>

### 1. /etc/httpd/conf/httpd.conf 


```plaintext
$ vi /etc/httpd
$ vi /etc/httpd/conf/httpd.conf
```



```plaintext

1. ServerRoot : 
    - 아파치의 기본 루트 경로.
    - 절대 경로로 설정 
    - 디렉토리 경로 끝에 / 넣지 말 것
    - 웹 서버에 관련된 환경설정 파일, 에러, 로고 파일들이 존재하는 디렉토리 위치 지정

ServerRoot "/etc/httpd"

2. Listen : 
    - 아파치가 실행되는 동안 수신할 포트 번호 지정
    - 아파치와 특정 IP 주소를 연결 or 아파치와 특정 포트들을 연결
    - 미 지정시 아파치 실행 안 됨
    - 443포트(SSL)는 httpd-ssl.conf 파일에서 따로 설정해야 함 
#Listen 12.34.56.78:80


Listen 80


3. Include : 
    - LoadModule 라인에 DSO 로 구축된 모듈의 기능을 사용하기 위해서 선언되어야 함
    - http.conf 파일에 다른 설정 파일을 가져오기 위해서 Include 사용

# Example:
# LoadModule foo_module modules/mod_foo.so

Include conf.modules.d/*.conf


4. User / Group : 
    - 웹 서버 실행할 때 소유권을 갖게되는 사용자와 그룹명 지정 

User apache

```



<br>

### 2. VirtualHost 
    - 한 서버에 여러 개의 서비스를 호스팅할 때 사용




------------------------------------------------------------------------------

DSO (Dynamic Shared Objects) 
