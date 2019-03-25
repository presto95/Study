https://learngitbranching.js.org](https://learngitbranching.js.org)

# git 기본

## git 커밋 소개

### commit

- git 저장소에 디렉토리에 있는 모든 파일에 대한 스냅샷을 기록
- 저장소의 이전 버전과 다음 버전의 변경내역*delta*를 저장함
  - 대부분의 커밋이 그 커밋 위의 부모를 가리킴

`git commit` : 커밋하기

## git에서 브랜치 쓰기

### branch

- 하나의 커밋과 그 부모 커밋들을 포함하는 작업 내역
- 특정 커밋에 대한 참조에 지나지 않음
  - 브랜치를 서둘러, 많이 만들기
  - 작은 단위로 잘게 나누어 만들기

`git branch [브랜치_이름]` : 브랜치 만들기

`git checkout [브랜치_이름]` : 명시한 브랜치로 이동하기

## git에서 브랜치 합치기

### merge

- 두 개의 별도의 브랜치를 합침
- **두 개의 부모를 가리키는** 특별한 커밋을 만들어냄
  - 한 부모의 모든 작업 내역과 나머지 부모의 모든 작업, 그리고 그 두 부모의 모든 부모들의 작업내역을 포함

`git merge [합칠_브랜치_이름]` : 두 브랜치를 합하고 현재 브랜치의 위치를 새로운 커밋으로 옮김

## rebase의 기본

- 커밋들을 **모아서** 복사한 후 다른 곳에 떨궈놓기
- 커밋들의 흐름을 보기 좋게 한 줄로 만들 수 있음
- 부모 커밋 가리키는 브랜치에서 자식 커밋을 가리키는 브랜치로 rebase하는 경우 두 브랜치가 모두 자식 커밋을 가리키게 됨

`git rebase [다른_브랜치_이름]` : 현재 브랜치를 복사한 커밋을  `다른_브랜치_이름`을 부모로 하여 위치시킴

`git rebase [기준_브랜치_이름] [다른_브랜치_이름]` : 기준 브랜치 아래에 다른 브랜치를 떨궈놓음

# 다음 단계로

## HEAD 분리하기

### HEAD

- 현재 체크아웃된 커밋 == 현재 작업중인 커밋
- 항상 작업 트리의 가장 최근 커밋을 가리킴
  - 작업 트리에 변화를 주는 명령어는 대부분 HEAD를 변경하는 것으로 시작함
- 일반적으로 HEAD는 브랜치의 이름을 가리키고 있음 (브랜치를 가리킴, 참조함)
- HEAD 분리하기 : HEAD를 브랜치 대신 커밋에 붙임
  - `git checkout [커밋 해시값]`으로 HEAD를 브랜치 대신 커밋 노드를 가리키게 함

## 상대 참조 (^)

- 커밋 해시는 `git log`를 통하여 확인할 수 있으나 귀찮을 수 있음. 이를 해결하기 위해 상대적인 위치를 활용할 수 있음
  - `^` : 한 번에 한 커밋 위로 움직임
  - `~<num>` : 한 번에 여러 커밋 위로 움직임

`git checkout master^^` : master 브랜치가 가리키고 있는 커밋의 두 단계 위로 HEAD를 체크아웃

`git checkout HEAD^` : HEAD가 가리키고 있는 커밋의 한 단계 위로 HEAD를 체크아웃

## 상대 참조 (~)

- `f` 옵션 : 브랜치를 특정 커밋에 직접적으로 재지정할 수 있음
  - `git branch -f master HEAD~3` : master 브랜치를 HEAD에서 세 단계 거슬러 올라간 커밋으로 강제 체크아웃

`git checkout HEAD~4` : HEAD가 가리키는 커밋으로부터 네 단계 거슬러 올라가 HEAD를 체크아웃

## git에서 작업 되돌리기

### reset

- 브랜치가 예전의 커밋을 가리키도록 이동시킴
  - 마치 애초에 커밋하지 않은 것처럼 예전 커밋으로 브랜치를 옮김
  - 히스토리를 고쳐쓴다 : 원격 브랜치에서 쓰기 힘듬
- 옵션
  - `--mixed` : 기본
  - `--hard` : 하드. 현재 커밋을 날리고 돌아감
  - `--soft` : 소프트. 현재 커밋을 스테이징된 상태로 하여 돌아감

`git reset HEAD^` : 현재 커밋의 한 단계 위로 돌아감.

### revert

- 변경분을 되돌리고 그 내용을 다른 사람과 공유하기 위한 방법
- 현재 커밋의 자식에 변경내용이 기록됨

`git revert HEAD^` : 현재 커밋 한 단계 위의 커밋 내용을 되돌려 새로운 커밋을 만들어냄

# 코드 이리저리 옮기기

## cherry-pick 소개

- 원하는 커밋과 그 해시값이 무엇인지 알 때 매우 유용함

`git cherry-pick <commit1> <commit2> <...>` : HEAD 위에 있는 일련의 커밋들에 대한 복사본을 만들어 순서대로 HEAD에 떨궈줌

## 인터랙티브 리베이스 소개

- 원하는 커밋을 모르는 경우 활용할 수 있음
  - rebase할 일련의 커밋을 검토할 수 있음

`git rebase -i`로 인터랙티브 리베이스

- 적용할 커밋들의 순서를 바꿀 수 있음
- 원하지 않는 커밋을 뺄 수 있음 (pick)
- squash하여 커밋을 합칠 수 있음

`git rebase -i HEAD~4` : HEAD로부터 네 단계 위에 있는 커밋에, 인터랙티브하게 설정한 커밋을 붙이거나 수정하거나 할 수 있음

# 종합선물세트

## 딱 한 개의 커밋만 가져오기

- cherry-pick 또는 인터랙티브 리베이스로 구현 가능

## 커밋 가지고 놀기

- 인터랙티브 리베이스로 커밋의 순서를 바꿀 수 있음
- 정정할 커밋이 바로 직전에 있으면 `git commit --amend`로 간단히 수정 가능
- 물론 cherry-pick 사용하여도 됨

## git 태그

- 브랜치는 이동하기 쉬움. 작업의 완료, 진행에 따라 이리저리 이동하며 서로 다른 커밋을 참조하게 됨. 쉽게 변하며 임시적임. 항상 바뀜.
- 작업 이력의 중요한 지점(주요 릴리즈 / 주요 브랜치 병합 등)들에 영구적인 표시를 하고 싶을 때 태그 사용
  - 특정 커밋들을 영구적인 마일스톤으로 표시함
  - 커밋들이 추가적으로 생성되어도 절대 움직이지 않음
    - 태그를 체크아웃한 후 그 태그에서 어떠한 작업을 완료할 수 없음

`git tag v1 [커밋]` : `커밋해시`에 v1이라는 태그를 만듦. `커밋`을 지정하지 않으면 HEAD에 태그를 붙임

## git describe

- 가장 가까운 태그에 비해 상대적으로 어디에 위치해 있는지 묘사
  - 커밋 이력에서 앞뒤로 여러 커밋을 이동하고 나서 현재 어디에 있는지 다시 인지하는데 도움을 

`git describe <ref>` : `ref`에는 커밋을 의미하는 그 어떤 것이든 사용할 수 있음. 명시하지 않으면 HEAD.

- `<tag>_<numCommits>_g<hash>`
  - tag : 가장 가까운 부모 태그
  - numCommits : 그 태그가 몇 커밋 멀리 있는지 나타냄
  - hash : 묘사하고 있는 커밋의 해시를 나타냄

# 고급 문제

## 9천번이 넘는 rebase

- rebase는 공통된 커밋 이외의 커밋들을 모아서 다른 커밋에 떨궈놓는 것

## 다수의 부모

- merge된 커밋은 다수의 부모를 가지고 있어 상대 참조로 되돌아갈 때 어떤 부모를 선택할지 예측할 수 없음
  - 일반적으로 병합된 커밋에서 첫 부모를 따라감
  - `git checkout master^2`와 같이 명시하면 두 번째 부모에 체크아웃
  - `git checkout HEAD~^2~2` 
    - 한 단계 거슬러 올라가 체크아웃
    - 두 번째 부모로 거슬러 올라가 체크카웃
    - 두 단계 거슬러 올라가 체크아웃
  - `git branch bugWork -f HEAD~^2~` : 현재 커밋에서 `bugWork` 브랜치 생성 후 한 단계 거슬러 올라간 후 두 번째 부모로 거슬러 올라간 후 한 단계 거슬러 올라간 곳에 `bugWork` 브랜치 위치시킴

## 브랜치 스파게티

- cherry-pick / rebase 잘 활용하기
  - rebase가 좀더 우아하긴 하지만 어렵다

# git 원격 저장소

## clone 소개

- git remote : 또 하나의 컴퓨터에 있는 우리의 저장소의 복사본일 뿐. 인터넷을 통하여 이 또 하나의 컴퓨터와 커밋을 주고받는 등의 대화가 가능함
  - 백업의 역할
  - 다른 사람과 함께 코딩할 수 있음
  - 일반적으로 Github 사용

### clone

- 원격 저장소의 복사본을 로컬에 생성

`git clone [주소]` : 현재 디렉토리에 원격 저장소의 복사본 생성

## remote branch

- 로컬 저장소에 원격 브랜치가 생성됨
  - 원격 저장소의 상태를 반영 : 로컬에서의 작업과 비교 가능
  - 분리된 HEAD 모드로 이동 : 원격 브랜치에서 직접 작업할 수 없음. 원격 브랜치는 원격 저장소가 갱신될 때만 갱신됨
- `<remote name>/<branch name>` : `origin/master` : 원격 저장소 이름 origin, 브랜치 이름 master
  - origin은 clone시 기본으로 지정되는 원격 저장소 이름

## git fetch

- 원격 저장소에서 데이터 가져오기
  - 원격 저장소에는 있지만 로컬에는 없는 커밋을 다운로드
  - 로컬의 원격 브랜치가 가리키는 곳을 갱신함
  - **로컬의 상태는 전혀 갱신하지 않음**
- 원격 저장소와 작업하여 상태가 변하면 원격 브랜치 또한 그 변경을 반영함
- 로컬에서 나타내는 원격 저장소의 상태를 실제 원격 저장소의 상태와 동기화함

`git fetch`

## git pull

- fetch + merge = pull
  - fetch 후 내려받은 브랜치를 병합하는 과정의 단축

`git pull`

## git push

- 로컬의 변경 사항을 원격 저장소에 업로드함. 원격 저장소는 새로운 커밋을 합쳐 갱신함. 로컬의 원격 브랜치를 갱신함

`git push`

## 엇갈린 히스토리

- 히스토리가 엇갈리면 push하기 전 원격 저장소의 최신 상태를 합치도록 강제함
- `git pull --rebase` : fetch + rebase
- `git pull` : fetch + merge

# 고급 git 원격 저장소

## 원격 작업과 merge하기

### merge vs. rebase

- rebase
  - 장점 : 커밋 트리를 한 줄에 깔끔하게 정리함
  - 단점 : 커밋 트리의 이력을 수정함
- merge
  - 장점 : 이력 보존이 가능
  - 단점 : 커밋 트리를 보기가 좋지 않음

## 원격 저장소 추적하기

- 로컬의 master 브랜치와 원격 브랜치인 origin/master 브랜치는 서로 관계가 있음
  - pull 작업 도중 커밋은 origin/master에 내려받아지고 master 브랜치로 merge됨
  - push 작업 도중 master 브랜치의 작업은 원격의 master 브랜치로 push
- master 브랜치는 origin/master 브랜치를 추적하도록 설정되어 있음
  - local branch 'master' set to track remote branch 'origin/master'
- `git checkout -b [브랜치_이름] [원격_브랜치_이름]` : 지정한 원격 브랜치를 참조하여 새로운 브랜치를 생성하여 체크아웃
- `git branch -u [원격_브랜치_이름] [브랜치_이름]` : 지정한 브랜치가 원격 브랜치를 추적하게 함

## git push의 인자들

`git push <remote> <place>` : `git remote origin master` : 지정한 원격 저장소의 브랜치에 로컬의 같은 이름의 브랜치를 푸시

`git push origin <source>:<destination>` : origin 원격 저장소의 destination에 로컬의 source 브랜치를 푸시

- `git push origin foo^:master` : origin 원격 저장소의 master 브랜치에 로컬의 foo의 한 단계 이전 커밋을 푸시

## fetch의 인자들

`git fetch origin foo` : origin 원격 저장소의 foo 브랜치에서 로컬에 없는 커밋들을 로컬의 origin/foo 브랜치 아래에 추가

`git fetch origin foo~1:bar` : origin 원격 저장소의 foo 브랜치의 한 단계 이전 커밋을 로컬의 bar 브랜치 아래에 추가

## source가 없다

`git push origin :side` : 원격 저장소에 있는 side 브랜치와 로컬에 있는 origin/side 원격 브랜치를 없앰

`git fetch origin :bugFix` : 로컬에 bugFix 브랜치를 만듦

## pull 인자들

`git pull origin foo` = `git fetch origin foo; git merge origin/foo`

`git pull origin bar~1:bugFix` = `git fetch origin bar~1:bugFix; git merge bugFix`