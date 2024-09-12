# [Git] CLI 기본 설정 및 사용법

Git 구조는 아래 글을 확인한다.

<br />

## Git CLI 사용법

git bash나 cmd에서 명령어를 통해 git을 조작할 수 있다.


### ✅ 명령어 구조

```
git [명령어] [-옵션]
```


#### 예시

```bash
git add
git config
git commit
```

git bash나 cmd에서 이러한 명령을 사용할 수 있고, 뒤 옵션을 어떻게 주는지에 따라 다른 동작을 수행한다.

<br />

### ✅ 사용자 설정

기본적으로 깃 설치 후 사용자명, 사용자 이메일을 설정하도록 Git 공식 홈페이지에서도 가이드하고 있다. 해당 정보는 commit 이력을 남길 때 함께 저장되는 정보이며 관련 명령어로 `config` 를 사용한다.

- `git config [key]` : 저장된 내역 출력
- `git config  [key] [value]` : 새로운 값 설정

```bash
# 전역적으로 사용할 사용자명 설정
$ git config --global user.name "yourname"

# 전역적으로 사용할 사용자 이메일 설정
$ git config --global user.email "your@email"

# 설정 내역 확인
$ git config user.name
yourname
$ git config user.email
your@email
```

<br />

## Git 실습

### ✅ Git 초기화

이제 버전을 관리할 프로젝트에 Git 을 적용해보자. 프로젝트 폴더로 이동 후 아래 명령어를 입력해 깃 저장소를 생성한다.

```bash
$ cd git-start

$ git init
Initialized empty Git repository in C:/hr/workspace/SideProjects/git-start/.git/
```

<br />

깃을 초기화하고 나면 명령줄에 ‘master’ 라는 기본 브랜치가 설정되고, `ls -al` 명령어로 폴더 내부를 확인해보면 .git 이라는 폴더가 생성된 것을 확인할 수 있다.

```bash
$ ls -al
total 4
drwxr-xr-x 1 USER 197121 0 Apr  3 16:25 ./
drwxr-xr-x 1 USER 197121 0 Apr  3 16:24 ../
drwxr-xr-x 1 USER 197121 0 Apr  3 16:25 .git/
```

> [!WARNING]
> .git 폴더를 삭제하게 되면 모든 변경 이력이 소실되고 일반 폴더가 된다

<br />

### ✅ Git 상태 확인

현재 깃의 상태를 확인해보자. 현재 설정된 브랜치나 commit 내역 등을 확인할 수 있다.

```bash
$ git status
On branch master 

No commits yet

nothing to commit (create/copy files and use "git add" to track) 
```

출력된 내용에서 3가지를 확인할 수 있다.

- master 브랜치에서 작업하고 있음
- 아직 커밋된 내역은 없음
- `git add` 명령어를 통해 track(staging area 에 추가) 할 수 있다고 알려줌

<br />

### ✅ Git 추가

프로젝트에 테스트용 파일을 작성한 후 현재 상태를 확인해보자.

```bash
$ git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        sample.txt

nothing added to commit but untracked files present (use "git add" to track)
```

출력된 내역엔 3가지 정보가 포함되어 있다.

- 아직 commit된 것은 없다
- untracked 파일이 1개 있다
- commit할 것은 없지만 `git add`라는 명령어로 track 할 수 있다

<br />

이제 아래 명령을 통해 staging area에 해당 파일을 추가해보자

```bash
$ git add sample.txt
```

<br />

위 명령에서 `sample.txt` 대신 `.`을 사용하면 폴더 내부에 전체 파일을 추가할 수 있다.

add 까지 한 후 현재 git 상태를 체크해보면 변경된 내역이 반영되어 있다.


```bash
$ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   sample.txt
```

<br />

### ✅ Git Commit

이제 변경에 저장된 내역을 commit 해보자. 커밋 시 반드시 메세지를 적어줘야 하므로 `-m` 옵션 뒤에 저장할 메세지를 입력한다.

```bash
$ git commit -m "my first commit"
[master (root-commit) ed7e3b0] my first commit
 1 file changed, 1 insertion(+)
 create mode 100644 sample.txt
```

<br />

다시 현재 상태를 조회화면 작업 영역이 깨끗해 진 것을 알 수 있다.

```bash
$ git status
On branch master
nothing to commit, working tree clean
```

<br />

이제 commit 이력을 조회해보자. 이전에 설정한 사용자 정보가 커밋 이력에 사용되었다는 것을 체크할 수 있고, 그 외 commit 번호나 메세지, 시간 등도 함께 저장된다.

```bash
$ git log
commit ed7e3b012f8a728266b8b428656408f43a1ba1ea (HEAD -> master)
Author: hyeri <cos850@inzent.com>
Date:   Wed Apr 3 17:09:15 2024 +0900

    my first commit
```

<br />

---

참고

[www.youtube.com/watch?v=Z9dvM7qgN9s](https://www.youtube.com/watch?v=Z9dvM7qgN9s)