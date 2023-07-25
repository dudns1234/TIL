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