## 2.깃 명령어

```
$ git config --global user.name "Your Name"
$ git config --global user.email you@example.com
```

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

### commit

### push

### status
```
git status
```
파일의 상태를 확인하려면 보통 git status 명령을 사용한다.
### pull
```
git pull origin master
```
원격 저장소 변경 사항(이력)을 받아옵니다.
다른 작업 환경이나 위치에서 작업할 때, 혹은 공동 작업에서 타인이 commit해서 이력이 변경되었을 경우 등의 경우가 있습니다.
따라서 pull을 통해서 가져온 후, 작업을 진행하는 것이 좋습니다.
