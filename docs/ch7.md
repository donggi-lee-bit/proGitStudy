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
- 
