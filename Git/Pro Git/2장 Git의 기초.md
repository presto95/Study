# 2장 Git의 기초

## Git 저장소 만들기 

### 기존 디렉토리를 Git 저장소로 만들기

> ```sh
> git init
> ```

원하는 디렉토리로 이동하고 `git init` 명령을 실행하면 해당 디렉토리에 `.git` 디렉토리가 만들어진다.

`.git` 디렉토리에는 저장소에 필요한 뼈대 파일이 들어 있으며, 이 명령만으로는 프로젝트의 어떠한 파일의 관리를 시작하지 않는다.

> ```sh
> git add [원하는_파일]
> git commit
> ```

`git add` 명령으로 untracked 상태인 파일을 스테이징하거나, modified 상태인 파일을 스테이징한다.

`git commit` 명령으로 커밋한다.

### 기존 저장소를 Clone하기

> ```sh
> git clone [url]
> ```

해당 `url` 에 있는 모든 프로젝트 히스토리를 받아온다.

원격 저장소의 레포지토리 이름을 가진 디렉토리를 로컬에 생성하고, 그 안에 `.git` 디렉토리를 생성한다.

`git clone [url] [원하는_디렉토리_이름]` 으로 원격 저장소의 레포지토리 이름이 아닌 원하는 이름의 디렉토리를 생성할 수 있다.

https 프로토콜, git 프로토콜, SSH 프로토콜 등을 사용할 수 있다.

### 수정하고 저장소에 저장하기

#### 워킹 디렉토리의 파일 상태

- tracked
  - 스냅샷에 포함되어 있는 상태의 파일
  - unmodified : 수정되지 않음
  - modified : 수정됨
  - staged : 스테이징됨
- untracked
  - 워킹 디렉토리에 파일이 있으나 스테이징 에이리어에 포함되지 않은 파일

처음 저장소를 clone하면 모든 파일은 tracked이면서 unmodified이다.

#### git에서 파일의 라이프사이클

- untracked부터 시작하여, `git add` 명령을 통해 스테이징하면서 해당 파일을 git이 추적하도록 한다.
- 스테이징된 파일을 커밋하면, unmodified 상태가 된다.
- unmodified 상태인 파일을 수정하면, modified 상태가 된다.
- modified 상태인 파일을 스테이징하면, staged 상태가 된다.
- unmodified 상태인 파일을 삭제하면, untracked 상태가 된다.

### 파일의 상태 확인하기

> ```sh
> git status
> ```

```sh
On branch master
Your branch is up-to-date with 'origin/master'.
nothing to commit, working directory clean
```

로컬의 마스터 브랜치에서, 원격 저장소 origin에 있는 master 브랜치와 비교했을 때 최신 상태이며, 커밋할 것이 없고, 워킹 디렉토리도 수정된 내역이 없다.

이 상태에서 새로운 파일을 생성하게 되면, 이 파일은 untracked 상태로 표시된다. 파일은 tracked 상태가 되기 전까지는 절대 파일을 커밋하지 않는다.

`git status` 명령을 실행했을 때 해당 파일은 `Untracked files` 에 표시된다.

### 파일을 새로 추적하기

> ```sh
> git add [파일명_또는_디렉토리명]
> ```

해당 파일을 tracked 상태로 만든다.

`git status` 명령을 실행했을 때 해당 파일은 `Changes to be committed` 에 표시된다.

인자로 디렉토리명을 명시했을 경우, 디렉토리에 있는 모든 파일을 재귀적으로 추적한다.

`add` 는 프로젝트에 파일을 추가한다기보다는, **다음 커밋에 추가한다**고 생각하는 것이 좋다.

### Modified 상태의 파일을 Stage 하기

이미 스테이징된 파일을 수정했을 경우, 해당 파일은 `Changes not staged for commit` 에 표시된다.

이를 스테이징하려면 `git add` 명령을 사용한다.

변경한 파일을 스테이징하고 또 수정을 한다면, 스테이징된 부분까지는 `Changes to be committed` 에 위치하고, 이후 수정 부분은 `Changes not staged for commit` 에 위치하게 된다. 이 시점에서 커밋하게 되면 이후 수정 부분은 커밋되지 않는다.

### 파일 상태를 짤막하게 확인하기

> ```sh
> git status -s
> git status --short
> ```

`git status` 의 내용을 간단하게 보여준다.

```sh
 M README
MM Rakefile
A  lib/git.rb
M  lib/simplegit.rb
?? LICENSE.txt
```

두 개의 영문자 중 왼쪽 부분은 스테이징 에이리어, 오른쪽 부분은 워킹 디렉토리의 상태를 나타낸다. A는 새로 생성한 파일, M은 수정한 파일을 의미한다. ??는 추적하지 않은 파일을 뜻한다.

오른쪽에서 왼쪽으로 스테이징.

### 파일 무시하기

> `.gitignore` 파일

`.gitignore` 파일을 프로젝트 시작 시점에 만들어 git 저장소에 커밋하고 싶지 않은 파일을 실수로 커밋하는 일을 방지하는 것이 좋다.

`.gitignore` 파일에 입력하는 패턴이 따르는 규칙:

- 아무것도 없는 라인, `#` 으로 시작하는 라인 무시
- 표준 Glob 패턴
  - `[abc]` : a, b, c 중 하나
  - `[0-9]` : 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 중 하나
  - `a/**/z`  : a/z, a/b/z, a/b/c/z 같은 디렉토리 표현
- `/` 로 시작 : 하위 디렉토리에 적용되지 않음
- 디렉토리는 `/` 를 끝에 사용하는 것으로 표현
- `!` 로 시작하는 패턴의 파일은 무시하지 않음

### Staged와 Unstaged 상태의 변경 내용 보기

> ```sh
> git diff
> ```

어떠한 내용이 변경되었는지 살피려면 `git diff` 명령을 사용한다.

어떤 파일이 변경되었는가 살피려면 `git status` 만으로도 충분하다.

워킹 디렉토리에 있는 수정 내용과 스테이징 에이리어에 있는 것을 비교한다. 어떠한 파일이 스테이징되었다면 해당 명령어는 아무것도 출력하지 않는다.

> ```sh
> git diff --staged
> git diff --cached
> ```

`git diff --staged` 또는 `git diff --cached` 를 사용하여 커밋한 내용과 스테이징 에이리어에 있는 것을 비교한다.

### 변경사항 커밋하기

> ```sh
> git commit
> ```

staged 상태인 파일들을 커밋한다.

해당 명령어를 통해 실행한 편집기에서 커밋 메세지를 작성한다.

> ```sh
> git commit -m "[커밋_메세지]"
> ```

`git commit -m` 명령어를 사용하여 인라인에서 커밋 메세지를 작성할 수 있다.

### Staging Area 생략하기

> ```sh
> git commit -a
> ```

커밋 명령을 실행할 때 `-a` 옵션을 추가하여 tracked 상태의 파일을 staged 상태로 변경한 후 커밋을 실행한다.

### 파일 삭제하기

> ```sh
> git rm [파일명]
> ```

tracked 상태의 파일을 삭제하기 위해 `git rm` 명령을 실행한다. 결과적으로 실제 파일도 삭제되나 커밋 기록에 남게 되므로 복구할 수 있다.

> ```sh
> git rm --cached [파일명]
> ```

스테이징 에이리어에서만 파일을 삭제하고 워킹 디렉토리에서는 파일을 삭제하지 않고 남겨둔다. git이 해당 파일을 추적하지 않게 하고 싶을 때 사용한다. 해당 파일을 untracked 상태로 만든다.

### 파일 이름 변경하기

> ```sh
> git mv [기존_파일명] [바꿀_파일명]
> ```

## 커밋 히스토리 조회하기

> ```sh
> git log
> ```

저장소의 커밋 히스토리를 시간순으로 보여준다. 각 커밋의 SHA-1 체크섬, 저자 이름, 커밋한 날짜, 커밋 메세지를 보여준다.

`git log` 명령은 매우 다양한 옵션을 지원한다.

- `-p` : 각 커밋의 diff 결과를 보여줌
- `-n` : 최근 n개의 커밋 히스토리만 보여줌 ex) `git log -2`
- `--stat` : 각 커밋의 통계 정보를 조회함
  - 수정된 파일 / 수정된 파일 수 / 추가 또는 삭제된 라인 수
- `--pretty`
  - `--pretty=oneline` : 각 커밋을 한 라인으로 보여줌
    - `=short`, `=full`, `=fuller` 옵션도 있음
    - 기본 : 저자 / 커밋 날짜
    - `=short` : 저자
    - `=full` : 저자 / 커밋한 사람
    - `=fuller` : 저자 / 작성 날짜 / 커밋한 사람 / 커밋한 날짜
  - `--pretty=format` : 특정 포맷으로 보여줌
    - 다양한 서식 문자를 사용하여 특정 포맷을 문자열로 넘겨줄 수 있음
    - ex) `git log --pretty=format:"%h - %an, %ar : %s"
      - %h : 짧은 길이 커밋 해시
      - %an : 저자 이름
      - %ar : 저자 상대적 시간
      - $s : 요약
- `--graph` : 그래프 모양의 아스키 아트 표현
- `--shortstat` / `--name-only` / `--name-status` / `--abbrev-commit` / `--relative-date` 등

### 조회 제한조건

- `--since` / `--after` : 명시한 날짜 이후의 커밋만 검색
  - `--since=2.weeks`
- `--until` / `--before` : 명시한 날짜 이전의 커밋만 검색
- `--author` / `--committer` / `--grep`

## 되돌리기

> ```sh
> git commit --amend
> ```

스테이징 에이리어를 사용하여 커밋한다.

마지막으로 커밋하고 나서 수정 내역이 없을 때 이 명령을 사용한다면, 커밋 메세지만 수정한다. 그렇지 않은 경우 기존 커밋에 스테이징 에이리어에 있는 변경 내역을 포함한다.

### 파일 상태를 Unstage로 변경하기

> ```sh
> git reset HEAD
> ```

현재 커밋 상태로 상태를 리셋한다.

### Modified 파일 되돌리기

> ```sh
> git checkout -- [파일명]
> ```

어떠한 파일을 수정하여 modified 상태가 된 후 해당 명령을 실행하면, 해당 커밋의 초기 상태로 해당 파일의 상태를 되돌린다.

이 경우 해당 파일에 대한 변경 내역이 모두 사라진다.

이 방법 보다는 stash나 branch를 활용하는 것이 낫다.

> git으로 커밋한 모든 것은 언제나 복원할 수 있으나, 커밋하지 않은 것은 절대 되돌릴 수 없다.

## 원격 저장소

협업하기 위해서는 원격 저장소를 관리할 줄 알아야 한다.

### 원격 저장소 확인하기

> ```sh
> git remote
> ```

현재 프로젝트에 등록된 원격 저장소의 단축 이름을 확인한다.

기본적으로 `origin` 이라는 이름으로 원격 저장소가 자동으로 등록된다.

> ```sh
> git remote -v
> ```

단축 이름과 URL을 함께 확인할 수 있다.

### 원격 저장소 추가하기

> ```sh
> git remote add [단축이름] [url]
> ```

`url` 에 `단축이름` 으로 원격 저장소를 추가한다.

### 원격 저장소를 Pull 하거나 Fetch 하기

> ```sh
> git fetch [원격_저장소_이름]
> ```

로컬에는 없지만 원격 저장소에 있는 데이터를 모두 가져온다. 하지만 자동으로 merge하지는 않는다.

> ```sh
> git pull
> ```

fetch와 merge를 동시에 수행한다.

### 원격 저장소에 Push하기

> ```sh
> git push [원격_저장소_이름] [원격_저장소_브랜치_이름]
> ```

`git push origin master` 명령으로 `origin` 원격 저장소의 `master` 브랜치에 push한다.

### 원격 저장소 살펴보기

> ```sh
> git remote show [원격_저장소_이름]
> ```

원격 저장소의 URL / 추적하는 브랜치를 보여준다. 

브랜치명을 생략하고 명령을 실행했을 때 어떤 브랜치가 어떤 브랜치로 push되는지 보여준다.

`git pull` 명령을 실행했을 때 자동으로 merge할 브랜치가 무엇인지 보여준다.

### 원격 저장소 이름을 바꾸거나 원격 저장소를 삭제하기

> ```sh
> git remote rename [기존_원격_저장소_이름] [새로운_원격_저장소_이름]
> ```

이 경우 로컬에서 관리하던 원격 저장소의 브랜치 이름도 변경된다는 것에 주의해야 한다.

> ```sh
> git remote remove [원격_저장소_이름]
> git remote rm [원격_저장소_이름]
> ```

## 태그

보통 릴리즈 시 사용한다.

### 태그 조회하기

> ```sh
> git tag
> ```

이미 만들어지 태그를 조회한다. 알파벳 순서로 보여주며, 순서는 중요하지 않다.

`git tag -l "v1.8.5*"` 와 같은 명령은 1.8.5 버전의 태그들만 검색하고 싶을 때 사용할 수 있다.

### 태그 붙이기

Lightweight 태그와 Annotated 태그, 두 개의 종류가 있다.

#### Lighweight 태그

특정 커밋에 대한 포인터일 뿐이다.

#### Annotated 태그

git 데이터베이스에 태그를 만든 사람의 이름, 이메일, 태그 생성 날짜, 태그 메세지가 함께 저장된다.

### Annotated 태그

> ```sh
> git tag -a [태그_이름]
> ```

`-a` 옵션을 사용하여 annotated 태그를 생성한다.

`-m` 옵션을 함께 붙여 태그 저장시 메세지를 함께 저장할 수 있다. 메세지를 입력하지 않으면 편집기를 열어 메세지를 입력하게 된다.

> ```sh
> git show [태그_이름]
> ```

태그 정보와 커밋 정보를 모두 확인할 수 있다.

### Lightweight 태그

> ```sh
> git tag [태그_이름]
> ```

기본적으로 파일에 커밋 체크섬을 저장하는 것일 뿐이다.

> ```sh
> git show [태그_이름]
> ```

태그에 대한 정보를 확인할 수 있으나, annotated 태그와는 다르게 별도의 태그 정보가 없으므로 단순히 커밋 정보만을 보여준다.

### 나중에 태그하기

> ```sh
> git tag -a [태그_이름] [커밋_체크섬]
> ```

특정 커밋 체크섬을 넘겨주어 이전 커밋에 태그할 수 있다.

### 태그 공유하기

> ```sh
> git push [원격_저장소_이름] [태그_이름]
> ```

일반적인 push로는 원격 저장소에 태그를 전송할 수 없으며, 위의 명령으로 별도로 push해야 한다.

> ```sh
> git push [원격_저장소_이름] --tags
> ```

한 번에 여러 개의 태그를 push하기 위해 사용한다.

### 태그를 Checkout 하기

> ```sh
> git checkout -b [새로운_브랜치_이름] [태그_이름]
> ```

해당 태그 이름이 가리키는 커밋을 가리키는 새로운 브랜치를 생성한다.

## Git Alias

타입 별칭을 만들어 git을 편리하게 사용할 수 있다.

> ```sh
> git config --global alias.st status
> git config --global alias.unstage 'reset HEAD --'
> ```

`st` 명령를 `git status` 명령으로, `unstage` 명령을 `git reset HEAD --` 명령으로 인식하게 해준다.