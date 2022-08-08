# Swap 에 대해서

## Swap 이란 ?

AWS EC2 의 1G의 물리적 메모리는 별 작업이 없어도 금새 소모되기 때문에 

swap 이라는 일종의 가상메모리를 생성하여 부족한 메모리를 확보 가능

<br>

## 해결방안 

1. 파티션 설정 : 가상서버 환경 초기에 파티션 설정해야하지만 접근하기 쉽지 않음.

2. 파일 생성 : 사용하는 파티션에 스왑파일을 생성하는 방법



<br>

## Swap 세팅 방법


### 1. 미리 만들어 둔 EC2 서버에 접속
    - putty 실행 -> Session(저장해둔 세션 불러오기), SSH-Auth(인증을 위한 키파일 선택)  -> Open 


### 2. 스왑 파일을 사용하여 Amazon EC2 인스턴스에서 스왑 공간으로 사용할 메모리를 할당하기
#### 2-1. 스왑파일 생성
- dd: 파일 생성 bs: 블록크기 count: 블록 수 
- 지정한 블록크기는 인스턴스에서 사용 간으한 메모리보다 작아야 함 


```plaintext
sudo dd if=/dev/zero of=/swapfile bs=128M count=32
```


#### 2-2. 스왑 파일의 읽기 및 쓰기 권한 업데이트

```plaintext
sudo chmod 600 /swapfile

```

#### 2-3. Linux 스왑 영역 설정
```plaintext
sudo mkswap /swapfile
```

#### 2-4. 스왑 공간에 스왑 파일을 추가하여 스왑 파일을 즉시 사용할 수 있도록 설정

```plaintext
sudo swapon /swapfile

```

#### 2-5. 프로시저가 성공적인지 확인
```plaintext
sudo swapon -s
```

#### 2-6. 편집기에서 /etc/fstab 파일을 편집
```plaintext
sudo vi /etc/fstab
```


#### 2-6-1. 파일 끝에 추가하고 파일을 저장한 다음 종료
- 편집명령어 : i 
- 저장 후 종료 : :wq


```plaintext
/swapfile swap swap defaults 0 0
```



---------------------------------------
참고

http://www.macnorton.com/csLab/886323

https://dog-developers.tistory.com/43

https://aws.amazon.com/ko/premiumsupport/knowledge-center/ec2-memory-swap-file/




