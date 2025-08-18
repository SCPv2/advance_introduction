# SCPV2 Advance 실습 준비

## 수강자 실습 단말 환경

### 실습용 폴더 생성(Windows)
```
cmd
cd \
md scpv2advance
```
### Visual Studio Code 설치
```
https://code.visualstudio.com/
```
### Github Clone
```
git clone https://github.com/SCPv2/advance_iac.git          # IaC를 이용한 클라우드 자원 배포 자동화
git clone https://github.com/SCPv2/advance_networking.git   # VPC 및 네트워크 확장
git clone https://github.com/SCPv2/advance_ha.git           # 고가용성/고성능 3계층 아키텍처 구현
git clone https://github.com/SCPv2/advance_backupdr.git     # 백업 및 재해복구 구현
git clone https://github.com/SCPv2/advance_cloudnative.git  # 클라우드 네이티브 환경 구현
```

## 실습 준비(콘솔)

### Keypair를 만들고 다운로드 받은 Keypair(mykey.pem)을 C:\scpv2advance\keypair에 저장

```
Keypair명: mykey
```

### 인증키 만들기


### DNS 만들기

```
Public Domain Name 
등록할 도메인명 : 수강자 자유 선택 

Private DNS
Private DNS명 : cesvc

Hosted Zone(Private)
용도구분: Private
등록할 Hosted Zone명 : cesvc.net
등록인 이메일 : 수강자 이메일

Hosted Zone(Public)
용도구분: Public
등록할 Hosted Zone명 : 위에서 생성한 Public DNS 사용
```

### Object Storage 버킷 생성 및 Network Logging 로그 저장소 지정
```
버킷명 : celog
```

### Trail 생성
```
Trail명 : cetrail
대상리전 : 전체
저장버킷 리전 : kr-west1
저장 버킷 : celog
저장 형식 : JSON
```

### Putty 다운로드 및 키변환
```
https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html
```

