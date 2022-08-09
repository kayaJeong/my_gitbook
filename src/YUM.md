# YUM 사용법
<br>

### 1. YUM 이란??!
- Yellowdog Updater Modified 명령어는 CentOS에서 패키지 관리를 하는 패키지 관리 도구 
- /etc/yum.conf에 설정 내용,  /etc/yum.repos.d/ 디렉토리에 저장된 저장소 파일에 지정된 서버로부터 업데이트된 패키지들을 검사하고 다운로드하여 설치함
- 설치, 업데이트, 삭제 시 의존성을 검사하여 관련된 패키지를 설치, 업데이트, 삭제를 함께 진행함 

### 2. YUM 명령어 

```plaintext
yum 옵션 명령 패키지명
```


1. 설치
```plaintext
yum install 패키지명
yum -y install 패키지명     : yes/no 안 묻고 넘어감
yum localinstall rpm패키지.rpm    
yum install --nogpgcheck rpm패키지.rpm      : CentOS7에서 인증되지 않는 rpm 패키지 설치 안되면 GPG키 인증 생략하고 설치
```

2. 업데이트 가능한 패키지 목록 확인
```plaintext
yum check-update
```

3. 업데이트 : 설치 되어 있는 패키지를 업데이트 하고 싶은 경우
```plaintext
yum update 패키지명
yum update       : 업데이트 가능한 모든 패키지를 업데이트함
```

4. 패키지 정보 확인 : 패키지 요약 정보 
```plaintext
yum info 패키지명
```

5. 패키지 리스트 확인 
```plaintext
yum list          : 저장소 서버 모든 패키지 목록 확인
yum list | grep 패키지명      : 원하는 패키지 목록만 확인 
yum list updates       : 설치된 패키지 중 업데이트 된 목록만 확인
yum list available     : 설치 가능한 패키지 리스트
yum list installed     : 설치된 패키지 리스트 
```

6. 삭제 : 설치된 패키지 삭제 - 의존성 패키지도 자동 삭제 
```plaintext
yum remove 패키지명 
```

7. 저장소 목록 지우기 : 다운로드한 패키지 목록 지우기
```plaintext
yum clean all    
```

### 3. Yum  Repository
- Yum을 통해 Package 설치 시 활동화된 Yum Repositor에서 다운로드해서 설치해 Package가 Repository에 있어야함 
- Defaulf Repository 외 EPEL(Extra Package for Enterprise Linux), REMI 등 다른 Repository추가 해 원하는 Package 및 최신 Package 설치 가능 


### 4. Yum Repository 확인

1. 활성화된 Yum Repository 확인
```plaintext
yum repolist    
```

2. Yum Repository 위치
```plaintext
cd /etc/yum.repos.d/
```

3. Yum Repository 추가

```plaintext
vi /etc/yum.repos.d/레포지토리이름.repo  

<필드설명>
name=저장소 표시 이름
baseurl=저장소 주소
enabled=활성화 여부 (0 or 1)
gpgcheck=gpg 서명키 사용 여부 (0 or 1)
gpgkey=서명키 사용 시 서명키 주소
```

4. Yum Repository 삭제

```plaintext

<레포지토리삭제>
rm -rf /etc/yum.repos.d/레포지토리이름.repo

<캐시파일 삭제>
rm -rf /var/cache/yum/x86_64

<헤더, 패지키, 메타데이터 삭제>
yum clean headers
yum clean packages
yum clean metadata

```


----------------------------------------------------------
참고

https://kim-dragon.tistory.com/232

