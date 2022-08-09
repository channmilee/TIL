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
   - git add . :????
4. git commit
   ex) git commit -m '변경 사항'
5. git log -- oneline
6. git clone <원격 저장소 주소>
   - 원격 저장소를 통째로 복제해서 **다운로드**
   - git clone을 통해 생성된 로컬 저장소는 `git init`, `git remote add`가 이미 수행되어 있음
   - 바탕화면 같은 상위 폴더에서 실시
   - (master) 뜨면 안됨
      - master 없애는 법: rm-r .git/
7. git pull 
   ex) git pull origin master
   - 원격 저장소의 변경사항을 가져와서 로컬 저장소에 **업데이트** 하는 명령어
8. 

**md 변경하면 저장하기**

---
## **Github**
1. git remote 
   - 등록
     - git remote add origin 주소
   - 조회
     - git remote -v
   - 삭제
     - git remote rm origin
2. 원격 저장소에 업로드
   - git add
   - git commit -m '변경 사항'
   - git push origin master

## **Branch**
- 여러 갈래로 작업 공간을 나누어 독립적으로 작업할 수 있도록 도와주는 Git의 도구

### > Branch 명령어
```bash
- 브랜치 목록 확인
   $ git branch

- 원격 저장소의 브랜치 목록 확인
   $ git branch -r

- 새로운 브랜치 생성
   $ git branch <브랜치 이름>

- 특정 커밋 기준으로 브랜치 생성
   $ git branch <브랜치 이름> <커밋ID>

- 특정 브런치 삭제
   $ git branch -d <브랜치 이름>
```
### > git switch
``` bash
- 다른 브랜치로 이동
   $ git switch <다른 브랜치 이름>

- 브랜치 생성과 동시에 이동
   $ git switch -c <브랜치 이름>

- 특정 커밋 기준으로 브랜치 생성과 동시에 이동
   $ git switch -c <브랜치 이름> <커밋ID>
   
