> ###### Git Basic
```
# commit 취소하기
# vim 나오면 i 누르고 커밋 메시지 수정해서 ex) remove all dummy txt files, :wq 해서 저장
$ git commit --amend

# commerce.py 파일 상태를 Unstage로 변경
$ git restore --staged commerce.py(최신)
$ git reset HEAD commerce.py (구버전)

# MODIFIED 파일 되돌리기
# board.py 파일의 마지막 커밋 상태로 파일을 되돌려준다. (restore도 가능하지만 checkout으로 쓰자)
$ git checkout -- board.py
```


> ###### Git Branch

```
# 브랜치 만들기
$ git branch feature/board
$ git branch (확인)
$ git checkout feature/board (board 브랜치로 스위칭, 구버전)
$ git switch feature/board (최신버전)
```

git은 커밋을 해주어야 누구인지 안다. 

브랜치를 나눠서 어떤 브랜치에서 파일을 만들어도 커밋을 하지 않으면 모든 브랜치에 존재하게 됨-> 하지만 add나 commit을 안했기 때문에 그냥 방관중

commit을 하고 나면 마스터 브랜치에서 안보이게 된다. 만들었다, 수정했다 이런거는 깃 입장에서 남 이야기! 커밋을 해야 주시하게 된다.

```
# 할일을 끝낸 브랜치 지우기
$ git branch -d feature/board

# 브랜치를 만들면서 체크아웃하기
git checkout -b feature/authg
```

두 브랜치 (또는 브랜치 하나와 마스터) 에서 작업하다가 merge를 할 경우merge한 브랜치는 둘이 합친 결과물을 가리키게 되고다른 브랜치는 merge를 실행한 브랜치가 아니기 때문에 그대로 마지막 작업물에서 남아있다.

```
# 마지막 커밋과 지금 파일 상태를 비교해서 어디가 다른지 말해줌
$ git diff HEAD
```

브랜치 만들었으면 그냥 오리진에 푸시해주면 오리진에 브랜치 만들어진다.



> ###### Contribution

1. contribution을 하기 위해서는 clone이 아니라 fork를 해야 한다. (github에서 오른쪽 상단에 있는 fork)
2. fork한 내 repo를 clone해서 파일 수정
3. 똑같이 add, commit, push 한 뒤 github에서 Create Pull Request
4. master가 승인하면 Contribution 완료!



> ###### Django에서 협업하기

```
$ mkdir DJANGO_COLLABO
$ cd DJANGO_COLLABO/
$ python -m venv venv
$ source venv/Scripts/activate
$ python -m pip install --upgrade pip   
# 필요한 모듈 설치
$ pip install django django_extensions ipython   
$ pip freeze > requirements.txt
# 뒤에 점을 찍어주면 중복 폴더 생성을 방지하고 바로 그 위치에 프로젝트를 생성한다.
$ django-admin startproject django_collabo .  
# git init 전에 먼저!
$ touch .gitignore
```

.gitignore에 추가하고

터미널 git bash로 바꿔주기 경로 : C:\Program Files\Git\bin\bash.exe

```
$ git init
$ git add .
$ git commit -m 'init project'
$ git remote add origin https://github.com/yooonyoung/DJANGO_COLLABO.git
$ git push origin master
$ git checkout -b feature/store
$ django-admin startapp store
```

django_collabo/settings.py

```
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'store',
]
```

```
# 변경하고 난 후
$ git add .
$ git commit -m 'startapp store'
$ git push origin feature/store
```

pull request와 충돌 수정. 

기능 브랜치에서 모든 작업을 끝냈으면

```
# 브랜치 삭제
$ git branch -d feature/store
```

다음 기능 브랜치 생성... 개발.... 



초대를 받는 사람 B 협업 과정

```
$ git clone https://github.com/yooonyoung/DJANGO_COLLABO.git
.....
dir 만들어서 clone 하고 pull 받기.
venv 셋팅. 모듈 설치
$ git checkoout -b feature/board
$ django-admin startapp board
```

django_collabo/settings.py

```
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'board',
]
```

