# AWS
> 인스턴스, 보안그룹, 탄력적IP

## 인스턴스
인스턴스는 컴퓨터와 같다.
AWS 어딘가에 Data Center가 있는데 그 일부를 할당해주는 것.
EC2 = AWS Data Center 컴퓨터
local(집컴퓨터) - ssh방식 -> EC2(AWS제공컴퓨터)
    - 연결방식 : ssh(보안연결)
    - 정보필요 : EC2 host 정보
    - 보안연결 : pem파일(인증서)

### 인스턴스 만들기
- 인스턴스 시작 버튼 클릭
- 이름 및 태그 : my-instance-1
- application and OS Images (Linux 기준)
    - Quick start → **Ubuntu**
    - Amazon Machine Image(AMI) : 22.04로 하기에는 DE하기엔 최신이기 때문에 버전 바꿔야함 
    그래서 Ubuntu Server 20.04 LTS (HVM), SSD Volume Type으로 선택!!!!!!!!!!!!!!
- 인스턴스 유형
    - 기본으로 t2.micro 선택되어있음. ⇒ MySQL, Linux 연습까지는 가능
    - r4까지해야 잘 돌아감 대신 비용이 꽤 있음. (고사양)
- 키 페어(로그인)
    - 인스턴스를 만들 때 마다 키 페어를 만들 필요는 없음. 기존에 사용했던 것 사용 O
    - 키 패어 생성 방법
        - 이름 : test_instance
        - 키 페어 유형 : RSA
        - 파일 : pem
- 보안그룹
- 스토리지 구성(ssd 용량)
    - 8기가
---
## 보안그룹
```
    EC2   
↙       ↘  
local    Caffe  
허용     비허용 or 허용  
```
### 생성 방법
1. 기본 세부 정보
    1. 보안 그룹 이름 : sg-test
    2. vpc : 네트워크 자원들 개인화 해주는것(신경쓰지않아도됨)
2. 인바운드 규칙(중요)
    1. ec2 입장에서 접속할 때 허용 규칙 (ex) 로컬에서 ec2로 접속할 때 허용 규칙
    2. **ec2 ← local : 인바운드 /** ec2 → local : 아웃바운드 
    3. 유형 : SSH (반드시 있어야함)
    4. 소스 : 내 IP (내가 사용중인 PC만 접속가능 ⇒ 노트북을 들고 카페가면 사용 못함)
        1. Anywhere-IPv4 : 어디서든지
    5. 설명 - 선택 사항 : 주석임
        1. SSH For Anywhere
    6. 규칙 추가
        1. 유형 : 사용자 지정 TCP
        2. 포트범위 : 3306
        3. 소스정보 : Anywhere-IPv4
        4. MySQL Port

## 탄력적 IP

- 인스턴스를 껐다키면 아이피가 바뀌는데 엘라스틱 IP를 사용하면 안바뀜
- 인스턴스 연결
- 인스턴스 선택하면 끝