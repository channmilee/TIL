## **CLI**
1. 절대 경로 VS 상대 경로
1) 절대 경로: C:/Users/cmlee/Desktop
2) 상대 경로
   - . / : 현재 작업하고 있는 폴더
   - . . / : 현재 작업하고 있는 폴더의 상위 폴더
---
## **터미널 명령어**
1. 파일 생성 명령어 `touch`
2. 폴더 생성 명령어 `mkdir`
3. 현재 작업 중인 디렉터리의 폴더, 파일 목록을 보여주는 명령어 `ls`
- ls -a : all 옵션
4. 폴더/파일 삭제 명령어 `rm`
----
## **Git 명령어**
1. git init
   - 현재 작업 중인 디렉토리를 Git으로 관리한다는 명령어
   - `(master)` 표기되고 `.git` 숨김 폴더 생성
2. git status
   - Working Directory와 Staging Area에 있는 파일의 현재 상태를 알려주는 명령어
   - 상태
     - Untracked: Git이 관리하지 않는 파일
     - Tracked: Git이 관리하는 파일
       - Unmodified: 최신 상태
       - Modified: 수정 되었지만 아직 Staging Area에 반영하지 않은 상태
       - Staged: Staging Area에 올라간 상태
3. git add
   - Working Directory에 있는 파일을 Staging Area로 올리는 명령어
4. git commit
   ex) git commit -m '변경 사항'
5. git log -- oneline

**md 변경하면 저장하기**

## **Github**
1. git remote 
   - 등록
     - git remote add origin 주소
   - 조회
     - git remote -v
   - 삭제
     - git remote rm origin
2. 원격 저장소에 업로드
   - git add . 
   - 커밋 생성
   -  git push
     - git push origin master