# Git의 기초

## Git Tag
태그는 semantic versioning 시스템을 많이 사용한다. (v1.0.0, Major, minor, fix)

### Tag 조회
```
$ git tag
v0.1
v1.3
```
```git tag``` 명령어로 만들어진 tag를 확인할 수 있다

---

```
$ git tag -l 'v1.8.5*'
v1.8.5
v1.8.5-rc0
v1.8.5-rc1
v1.8.5-rc2
```
- list 옵션을 이용해 보고 싶은 태그만 검색할 수 있다.
- ```*``` 을 이용해 확인하고 싶은 버전을 쉽게 확인할 수 있다.

### Tag 붙이기
- Lightweight 태그와 Annotated 태그 두 종류가 있다
    - Lightweight tag : 이름 정보만 갖고 있다
    - Annotated tag : 상세한 정보를 가지고 있다
- HEAD가 가리키고 있는 commit에 tag가 달리게 된다. 따로 HEAD를 움직이지 않는다면 HEAD는 가장 최근 commit을 가리키고 있기 때문에 해당 commit에 tag를 달게된다.
---
### Lightweight Tag

```
$ git tag v1.4-lw
$ git tag
v0.1
v1.3
v1.4
v1.4-lw
v1.5
```
<br>
<img width="1013" alt="스크린샷 2021-12-14 오후 2 55 40" src="https://user-images.githubusercontent.com/73376468/145941385-4b3ab809-a2be-49b6-8892-f6153266dded.png">

### Annotated Tag
```
$ git tag -a v1.4 -m 'my version 1.4'
$ git tag
v0.1
v1.3
v1.4
```
<img width="1234" alt="스크린샷 2021-12-14 오후 2 55 54" src="https://user-images.githubusercontent.com/73376468/145941493-6a7cc6f7-fda3-4794-b829-496bc20d1248.png">

- ```-m``` 옵션을 이용해 태그를 저장할 때 메시지를 같이 추가할 수 있다.
- ```git show``` 명령으로 태그 정보와 커밋 정보를 모두 확인할 수 있다. (태그 만든 사람의 이름, 이메일, 날짜, 태그 메시지)

### 선택한 commit에 tag 하기
```
git tag -a v1.2 9fceb02
```
- annotated 옵션 뒤에 태그 이름을 적고 tag 할 commit의 해시코드를 적어준다

### Tag 공유하기
- git push 명령으로 리모트 서버에 태그를 전송하지 않는다. Tag 는 별도로 push 해주어야한다. ```git push origin [태그 이름]```
- 한 번에 태그를 여러 개 push 하고 싶다면 --tags 옵션을 추가하여 git push 명령어를 실행한다. ```git push origin --tags```
- 이제 누군가 저장소를 clone 하거나 pull 하면 모든 태그 정보도 함께 전송된다.

### Tag checkout
- tag 는 branch 와는 달리 가리키는 커밋을 바꿀 수 없기에 checkout 해서 사용 할 수 없다. tag 가 가리키는 특정 커밋을 기반으로 작업하고자 한다면 새로 브랜치를 생성한다.
```
git checkout -b version2 v2.0.0
Switched to a new branch 'version2'
```
<img width="607" alt="스크린샷 2021-12-14 오후 3 09 48" src="https://user-images.githubusercontent.com/73376468/145942806-6188ea03-3560-4b64-962d-acc214793896.png">

## Git Alias
- 자주 사용하는 명령어를 사용자 설정을 하여 명령을 간략하게 사용할 수 있다.

### Alias 만드는 법
```
$ git config --global alias.co checkout
$ git config --global alias.br branch
$ git config --global alias.ci commit
$ git config --global alias.st status
```
- config를 사용하여 명령의 alias를 만들 수 있다.
- commit 명령어를 ci로 대신할 수 있게되었다.
---
git 명령어뿐만 아니라 외부 명령어도 실행할 수 있다. !를 앞에 추가하면 외부 명령을 실행한다.
```
$ git config --global alias.visual '!gitk'
```