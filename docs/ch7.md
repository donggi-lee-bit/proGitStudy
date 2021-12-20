# Git 도구

## Git stash, clean
- git stash가 어떻게 돌아가는걸까
- 워킹 디렉토리를 비우고 싶을 때는 git clean -f -d 하게 되면 강제로 진행시키는 force 옵션 때문에 무조건 진행하게된다. git clean -d -n n옵션을 이용하여 어떤 일이 일어날지 미리 보여주는 옵션을 사용하여 불상사를 방지할 수 있다.
- git clean 하게 되면 untracked 파일까지 다 지우기 때문에 신중해야한다.
    - .gitignore에서 추적하지 않겠다고 명시한 파일들은 clean으로 지워지지 않는다고 한다. (지우고 싶다면 -x 옵션 사용)
- git 명령어를 대화형으로 하고 싶다면 git add -i (interactive) 옵션을 사용

### Git GPG (확인된 사람에게만 커밋 받기)
- 확인된 사람에게만 커밋을 받고 싶으면 GPG를 이용한다.
    - gpg 설정 : gpg —list-keys
    - 키가 없으면 키 설정 : gpg —gen-key
    - 개인 키가 있다면 : git config —global user.signingkey 0A46826A
    - 설정하게되면 tag 와 commit에 서명할 때 등록한 키를 이용하게 된다

### Git Tag 확인하기
- ```git tag -v [tag-name]```을 이용해 태그에 서명한 사람이 그 사람이 맞는지 확인한다.
    - 확인 작업을 하려면 서명한 사람의 GPG 공개키를 키 관리 시스템에 등록해 두어야 한다.
    - 공개 키가 없으면 ```error: could not verify the tag``` 에러 발생

### Commit에 서명하기
- commit에 서명하고 싶으면 : ```git commit -s``` 옵션 사용
- 서명 확인하고 싶으면 : ```git log --show-signature```

### Merge와 pull 에 GPG 서명 이용
- ```git merge --verify-signatrues non-verify``` 옵션을 이용해 Merge할 커밋 중 서명하지 않았거나 신뢰할 수 없는 사람이 서명한 커밋이 있으면 Merge 되지 않는다.

## 함수 검색

### Git grep
- grep 명령을 이용하여 커밋 트리의 내용이나 워킹 디렉터리의 내용을 문자열이나 정규 표현식을 이용해 쉽게 찾을 수 있다.
    - 대상을 지정하지 않으면 워킹 디렉터리의 파일에서 찾는다
    - ```-n``` 옵션을 추가하면 찾을 문자열이 위치한 라인 번호도 같이 출력한다. ```git grep -n [method_name]```

- 더 다양한 옵션
    - 매칭되는 라인이 있는 함수나 메서드를 찾고 싶을 때 : ```-p```
    - 여러 단어가 한 라인에 동시에 나타나는 줄 찾기 : ```--and```
    - ```--break``` ```--heading```

### Git log 검색
- 어떤 변수가 언제 추가되거나 변경됐는지 찾아볼 수 있다.
    - ```git log -s[method_name]``` : 해당 문자열이 추가된 커밋과 없어진 커밋만 검색할 수 있다
    - 더 세세한 조건으로 검색하고 싶으면 ```-G``` 옵션으로 정규표현식을 써서 검색
---
- 라인 히스토리 검색
    - ```git log -L :git_defalte_bound:zlib.c``` : zlib.c 파일에 있는 git_deflate_bound 함수의 모든 변경사항을 보여준다. 함수에서 일어난 모든 히스토리를 함수가 처음 만들어진 때부터 Patch를 나열하여 보여준다.

## git 히스토리 단장하기(?)

### 마지막 커밋 수정
- ```git commit --amend``` : 마지막 커밋 메세지를 열어준다. 여기서 메시지를 바꾸고 편집기를 닫으면 바뀐 메시지로 커밋이 수정된다.
    - 커밋하고 난 후 새로 만든 파일이나 수정한 파일을 최근 커밋에 넣을 수도 있다.
        1. 파일을 수정하고 git add 명령으로 staging area에 넣거나 git rm 으로 파일을 삭제한다
        2. 그리고 ```git commit --amend``` 명령으로 커밋하면 된다. 현 staging area의 내용을 이용하여 수정하게된다.
        3. 이 때 push를 했다면 commit은 수정하면 안된다.
    
### 커밋 메시지 여러개 수정하기
- 하나하나 수정하는 게 아니라 어느시점부터 HEAD까지의 커밋을 한 번에 rebase한다.
    - ```git rebase -i``` 대화형 모드로 rebase 한다. 어떤 시점부터 HEAD까지 rebase할 것인지 인자로 넘기면 된다.
        - 마지막 커밋 메시지 세 개를 수정한다고 하면, ```git rebase -i HEAD~3``` 혹은 ```git rebase -i HEAD~2^``` 해서 넘긴다.
    - 이 때도 이미 ```push``` 하였다면 commit을 수정하지 않는 게 좋다. 다른 개발자들에게 혼란을 줄 수 있음.

### commit 합치기
- ```git rebase -i``` 대화형 모드로 rebase할 때, ```squash``` 를 입력하면 git은 해당 커밋과 바로 이전 커밋을 합치고 커밋 메시지도 merge하게 된다.

### commit 분리하기 (내용을 잘 이해하지 못함)
-

## Git Reset
git reset의 역할을 살펴보자

### HEAD 브랜치 이동
- ```HEAD```는 계속 현재 브랜치를 가리키고 있고, 현재 브랜치가 가리키는 커밋을 바꾼다.
- reset 명령은 가장 최근의 ```git commit``` 명령을 되돌린다. ```git commit``` 명령을 실행하면 git은 새로운 커밋을 생성하고 HEAD가 가리키는 브랜치가 새로운 커밋을 가리키도록 업데이트한다. ```reset``` 명령 뒤에 ```HEAD~```를 주면 index나 워킹 디렉터리는 그대로이고 브랜치가 가리키는 커밋만 이전으로 되돌린다. index를 업데이트한 뒤 ```git commit```을 하면 ```git commit --amend``` 명령의 결과와 같아진다

### Index 업데이트 (--mixed)

- ```reset``` 명령을 실행할 때 아무 옵션도 주지 않으면 기본적으로 ```--mixed``` 옵션으로 동작한다.
    - ```--mixed``` 옵션이 동작하면 가리키는 대상을 가장 최근의 commit으로 되돌리게된다. 그러고 나서 staging area를 비우기까지 한다. git commit 명령도 되돌리고 git add 명령까지 되돌리는 것이다.


### 워킹 디렉터리 업데이트 (--hard)
- ```--hard``` 옵션을 사용하면 워킹 디렉터리의 파일까지 강제로 덮어쓴다. ```--hard``` 옵션으로 덮은 내용은 되돌리는 것이 불가능하다.
    - 만약 이전 버전을 git commit이 보관하고 있다면 reflog를 이용하여 복원할 수 있지만 commit하지 않았다면 데이터는 복원할 수 없다.

## Git Checkout
- ```reset --hard```와 달리 ```checkout``` 명령은 워킹 디렉터리를 안전하게 다룬다. 워킹 디렉터리에서 Merge 작업을 한 번 시도해보고 변경하지 않은 파일만 업데이트한다.

- ```reset``` 명령은 ```HEAD```가 가리키는 브랜치를 움직이지만, ```checkout``` 명령은 ```HEAD``` 자체를 다른 브랜치로 옮긴다.
    - 현재 브랜치가 develop 이라는 브랜치일 때, ```git checkout master``` 명령을 실행하면 develop 브랜치가 가리키는 커밋은 바뀌지 않고 ```HEAD```가 master 브랜치를 가리키도록 업데이트한다.
- ***reset 명령은 HEAD가 가리키는 브랜치의 포인터를 옮겼고, checkout 명령은 HEAD 자체를 옮겼다.***

## Git Merge
- 오랫동안 합치지 않은 두 브랜치를 한 번에 Merge하면 거대한 충돌이 발생한다. 조그마한 충돌을 자주 겪고 풀어나감으로써 브랜치를 최신으로 유지하는 게 낫다.
- Merge 하기 전에 워킹 디렉터리를 깔끔하게 정리하는 것이 좋다.
- 작업하던 게 있다면 임시 branch에 커밋하거나 stash해둔다. 작업 중인 파일을 저장하지 않은 채로 Merge한다면 작업했던 내용을 잃을 수 있다.

### Merge 취소하기
- ```git merge --abort``` 명령으로 간단히 Merge 하기 전으로 되돌릴 수 있다.

### 공백 무시하기
- Merge가 공백 때문에 충돌이 날 때도 있다 그럴 땐 ```git merge --abort```로 merge를 취소하고, ```git merge -Xignore-space-change``` 혹은 ```git merge -Xignore-all-space``` 옵션을 주어 다시 merge한다. ```all space```는 모든 공백을 무시하고, ```space change```는 뭉쳐 있는 공백을 하나로 취급하게한다.

### 수동으로 merge하기 (아직 내용 이해 못함)

