# Today I Learned

> 230724 학습한 내용 정리
- markdown
    - #, ##, ### : 제목, 중제목, 소제목
    - *, ** : 이탈릭체, 볼드체
    - > : 인용
    - `` : python화
    - ---(하이픈3개) : 구분선
    - [이름](경로) : 경로 바로가기
    - ![이름](경로) : 사진 불러오기
    - | : 나누기?
---
- linux 명령어
    - `pwd` : 현재 작업중인 경로 출력
    - `ls` : 현재 폴더에 있는 파일, 폴더 출력
    - `ls -a` : 숨김처리된 파일, 폴더까지 출력
    - `cd` : 폴더이동
    - `cd ~` : 홈으로 이동
    - `mkdir` : 폴더 생성
    - `touch` : 파일 생성
    - `rm` : 파일 삭제(폴더 삭제 X)
    - `rm -r` : 폴더 및 파일 삭제
---
- git
    - **파일관리**
        - `git init` : 현재 폴더에 .git 폴더 생성
        - `git add .` : 모든 파일과 폴더 업로드
        - `git add git.md` : git.md 파일만 업로드
        - `git commit -m 'message'` : commit message 작성
        - `git push origin master` : 'master' branch를 원격저장소 'origin'으로 업로드
        
    - **설정**
        - `git status` : 현재 상태를 체크 (무엇이 수정되었는지)
        - `git config` : commit 했을 때 누가했는지에 대한 확인용
            - `git config --global user.email '이메일주소'`
            - `git config --global user.email` 를 통해 값 확인
            - `git config --global user.name '이름'`
            - `git config --global user.name` 를 통해 값 확인
        - `git remote`
            - `git remote add origin <remote url>` : remote url을 origin으로 부르겠다.
            - `git remove -v` : remote 주소와 별명 확인

> 230725 학습한 내용 정리
- 개인 site 만들기
    - 공유 템플릿과 깃허브를 사용하여 개인 도메인 생성
    - html code 수정하여 도메인 반영 확인
- github로 협업하기
    - `git clone <remote url>` : 원격 저장소에 있는 repository를 현재 폴더에 복제 (git bash 이용)
    - `git pull origin master` : 원격 저장소에 마지막 코드 상태를 다운로드 (vscode 이용)
- 충돌 테스트
    - 사용자들이 동시에 수정한 후에 push한 경우에 발생
    - A사용자 수정 선택, B사용자 수정 선택, 둘 다 선택
    - `git add .` -> `git commit -m "update"` -> `git push origin master` 진행
- branch
    - `git branch -c jyj` : jyj라는 branch 생성
    - `git branch` : branch 생성 확인
    - `git switch jyj` : master에서 jyj branch로 이동
- folk
    - 다른 사용자의 repository 협업을 위한 github내에서 수행

> 230726 학습한 내용 정리
- python 설치
- python intro
    - 변수
        - number
        - boolean
        - None
        - string (+ string interpolation)

    - 연산자
        - 산술연산자 ( +, -, *, /, **, //, %)
        - 비교연산자 ( >, <, >=, <=, ==, !=)
        - 논리연산자 (and, or, not)
        - 복합연산자 ( +=, -=, *=, /=, //=, %=, **=)
        - 기타연산자
            - concatenation : +
            - containment : in
            - identity : is, id
        - 우선순위
            - ()를 통해 그룹
            - **
            - 산술연산자(*, /)
            - 산술연산자(+, -)
            - 비교연산자, in, is
            - not
            - and
            - or
    - 형변환
        - 암시적 형변환
            - True : 1 이상
            - False : 0
        - 명시적 형변환
            - int() : string, float를 int로 변환
            - float() : string, int를 float로 변환
            - str() : int, float 등을 str로 변환
            - bool() : int, list 등을 boolean으로 변환
    - 시퀀스(sequence) 자료형
        - list
            - 선언 : 변수이름 = [value1, value2, value3 ...]
            - 접급 : 변수이름[index]
        - tuple
            - 선언 : 변수이름 = (value1, value2, value3)
            - 접급 : 변수이름[index]
            - 리스트와 유사하지만 수정 불가능(immutabel)하다.
        - range
            - range(n) : 0부터 n-1까지 범위
            - range(n, m) : n부터 m-1까지 범위
            - range(n, m, s) : n부터 m-1까지 +s만큼 증가하는 범위
        - string
        - 시퀀스에서 활용 가능한 연산/구조
            - indexing
            - slicing (+ k간격 )
            - in, not in
            - concatenation
            - "*"
            - len
            - min, max
            - .count()
    - 시퀀스 데이터가 아닌 자료구조
        - set
            - 수학에서 사용하는 집합과 동일하게 처리
            - 선언 : 변수이름 = {value1, value2, value3}
            - 차집합, 교집합, 합집합
            - 리스트 중복제거
        - Dictionary
            - 선언 : 변수이름 = {key1: value1, key2: value2, key3: value3 ...}
            - 접근 : 변수이름[Key]
            - dictionary는 key와 value가 쌍으로 이루어져있다.
            - key에는 immutable한 모든것을 사용가능 (불변값 : string, integer ...)
            - value에는 모든 데이터 가능 (list, dictionary도 가능)

- python_control_of_flow
    - 제어문
        - 조건문(if문)
            1. `if` 문은 반드시 일정한 참/거짓을 판단 할 수 있는 `조건식`과 함께 사용한다. (`if : <조건식> :`)      
                2-1) `<조건식>`이 참인 경우 `:`이후의 문장을 실행한다.   
                2-2) `<조건식>`이 거짓인 경우 `else:` 이후의 문장을 실행한다.
        - 조건문(elif문)
            ```
            if <조건식>:   
                if 조건이 참인경우   
            elif <조건식>:   
                elif 조건이 참인경우   

            else:   
                위의 조건식에 하나도 부합하지 않는경우 실행
            ```
        - 조건표현식
            ```
            true_value if <조건식> else false_value

            ```
