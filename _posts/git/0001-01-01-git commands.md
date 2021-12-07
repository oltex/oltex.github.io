---
title: "깃 명령어(Git Commands)"
categories:
  - git
tags:
  - tag
---

`-`
옵션

### config
```
$ git config --global user.name "Your Name"
$ git config --global user.email you@example.com
```
깃의 구성에 name과 email을 입력한다.  
입력하지 않으면 commit을 할 수 없다.
```
git config --global --list
```
깃 구성 리스트 확인
```
git config --unset user.name
git config --unset user.email
```
깃의 구성에 저장돼 있던 name과 email을 삭제한다.

### init
```
git init
```
이 명령은 .git이라는 하위 디렉토리를 만든다. .git 디렉토리에는 저장소에 필요한 뼈대 파일(Skeleton)이 들어 있다. 이 명령만으로는 아직 프로젝트의 어떤 파일도 관리하지 않는다.
git이 파일을 관리하게 하려면 저장소에 파일을 추가하고 커밋해야 한다.

### clone
```
git clone "https://"
```
다른 프로젝트에 참여하려거나 Git 저장소를 복사하고 싶을 때 git clone 명령을 사용한다.
git clone 을 실행하면 프로젝트 히스토리를 전부 받아온다.
### add
```
git add "filename.exe"
```
작업 디렉토리 상의 변경 내용을 스테이징 영역(staging area)에 추가하기 위한 명령어이다.
```
git add .
```
현재 디렉토리의 모든 변경 내용을 스테이징 영역에 추가한다.
```
git add -a
```
작업 디렉토리의 모든 변경 내용을 스테이징 영역에 추가한다.
```
git add -p
```
각 변경사항을 터미널에서 직접 확인하면서 스테이징 영역에 추가하거나 제외할 수 있다.
### commit
```
git commit
```
작업 디렉토리의 변경 내용을 기록한다 vim 사용
### push

### status
```
git status
```
파일의 상태를 확인하려면 status 명령을 사용한다.
### pull
```
git pull origin master
```
원격 저장소 변경 사항(이력)을 받아옵니다.
다른 작업 환경이나 위치에서 작업할 때, 혹은 공동 작업에서 타인이 commit해서 이력이 변경되었을 경우 등의 경우가 있습니다.
따라서 pull을 통해서 가져온 후, 작업을 진행하는 것이 좋습니다.

### log
```
git log
```
깃의 로그를 보여준다.
```
git log -p
```
각각의 커밋과 커밋 사이의 소스상의 차이점을 보여준다.
```
git log "commit id"
```
커밋 아이디에 해당하는 커밋 이전의 로그만 보여준다.

### diff
```
git diff
```
commit 된 파일 상태와 현재 수정중인 파일 상태 비교
```
git diff "commithash".."commithash"
```
commit간의 상태 비교
