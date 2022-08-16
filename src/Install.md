# yum 을 이용해 epel, Apache2.4, Resin4, MySQL5.7, PHP5.4, OpenJDK8 설치

### 1. yum 리포지토리 설치 확인 

#### 1-1. 설치되어 있는 yum 리포지토리 조회 

```plaintext
yum repolist all 
```

#### 1-2. yum-config-manager 명령을 사용해 리포지토리 추가

```plaintext
sudo yum-config-manager --add-repo https://www.example.com/repository.repo
```

#### 1-3. /etc/yum.repos.d  에 yum 리포지토리 활성화 및 epel 설치 

```plaintext

cat /etc/os-release

sudo amazon-linux-extras install epel -y

```

#### 1-4. httpd설치

```plaintext
sudo yum -y install httpd
```

#### 1-5. 실행 및 상태 확인 

```plaintext
sudo systemctl starthttpd

sudo systemctl status httpd
```

### 2. MySQL 설치 및 실행

```plaintext 
sudo rpm --import https://repo.mysql.com/RPM-GPG-KEY-mysql-2022

sudo yum localinstall -y https://dev.mysql.com/get/mysql57-community-release-el7-11.noarch.rpm

sudo yum install -y mysql-community-server

sudo systemctl enable mysqld
sudo systemctl start mysqld

mysql --version
sudo netstat -plntu 
sudo systemctl restart mysqld
```

### 3. PHP 설치 

```plaintext
yum list php

sudo yum -y install php.x86_64

```

### 4. JDK 설치 및 환경변수 설정 

```plaintext

yum list | grep jdk

sudo yum -y install java-1.8.0-openjdk.x86_64

sudo yum -y install java-1.8.0-openjdk-devel.x86_64

```

```plaintext
환경변수 설정

which java

readlink -f /usr/bin/java

-------------------------
/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.332.b09-1.amzn2.0.2.x86_64/jre/bin/java
-------------------------


sudo vi /etc/profile

--------------------------
export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.332.b09-1.amzn2.0.2.x86_64/jre/bin/java
export PATH=$PATH:$JAVA_HOME/bin
export CLASSPATH=$JAVA_HOME/jre/lib:$JAVA_HOME/lib/tools.jar

:wq
----------------------------

source /etc/profile

echo $JAVA_HOME
```



### 5. resin 4.0.63 설치 


```plaintext
rpm --import http://caucho.com/download/rpm/RPM-GPG-KEY-caucho

yum install https://caucho.com/download/rpm-6.8/4.0.63/x86_64/resin-4.0.63-1.x86_64.rpm
```









-----------------------------------------------------------------------------------


```plaintext
다운로드. 패키지 설치
wget "https://caucho.com/download/resin-4.0.66.tar.gz"


tar파일 풀기 및 설치 
tar xzvf resin-4.0.66.tar.gz

cd resin-4.0.66

configure에서 no acceptable C compiler found in $PATH 오류
컴파일러를 찾지못해 생기는 에러 -> 
rpm -qa|grep gcc
#yum -y install gcc 

./configure --prefix=/data/resin-4.0.66

make && make install

```


### 6. 확인
```plaintext
yum repolist //현재 틍록된 레포리스트 출력
yum list installed //설치된 전체 패키지 

```



##### No package available 오류

```plaintext
yum clean all & yum clean metadata
```

##### packages excluded due to repository priority protections 오류

```plaintext
/etc/yum/pluginconf.d/priorities.conf 파일의 내용을

enabled = 0 으로 수정
```

##### you need to be root to perform this command 오류
```plaintext

su - 계정으로 변경

authentication failure 오류

su - 입력 시 해당 오류 발생
초기 비밀번호 설정 안되어 있어 오류 발생 

sudo passwd root 

```



참고..

https://aws.amazon.com/ko/premiumsupport/knowledge-center/ec2-enable-epel/

https://www.caucho.com/resin-4.0/admin/starting-resin.xtp

