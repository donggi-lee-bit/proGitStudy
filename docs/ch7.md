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