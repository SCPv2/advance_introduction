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

## 실습 준비(콘솔)

- Keypair를 만들고 다운로드 받은 Keypair(mykey.pem)을 C:\scpv2advance\keypair에 저장

```
Keypair명: mykey
```

- 인증키 만들기


- DNS 만들기

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

