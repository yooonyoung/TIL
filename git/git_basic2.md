> Git Basic
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

> Git Branch
>

```
# 브랜치 만들기
$ git branch feature/board
$ git branch (확인)
$ git checkout feature/board (board 브랜치로 스위칭, 구버전)
$ git switch feature/board (최신버전)
```

git은 커밋을 해주어야 누구인지 안다.브랜치를 나눠서 어떤 브랜치에서 파일을 만들어도 커밋을 하지 않으면 모든 브랜치에 존재하게 됨-> 하지만 add나 commit을 안했기 때문에 그냥 방관중commit을 하고 나면 마스터 브랜치에서 안보이게 된다. 만들었다, 수정했다 이런거는 깃 입장에서 남 이야기! 커밋을 해야 주시하게 된다.

```
# 할일을 끝낸 브랜치 지우기
$ git branch -d feature/board

# 브랜치를 만들면서 체크아웃하기
git checkout -b feature/authg
```

두 브랜치 (또는 브랜치 하나와 마스터) 에서 작업하다가 merge를 할 경우merge한 브랜치는 둘이 합친 결과물을 가리키게 되고다른 브랜치는 merge를 실행한 브랜치가 아니기 때문에 그대로 마지막 작업물에서 남아있음.

```
# 마지막 커밋과 지금 파일 상태를 비교해서 어디가 다른지 말해줌
$ git diff HEAD
```

브랜치 만들었으면 그냥 오리진에 푸시해주면 오리진에 브랜치 만들어진다.



> Contribution

  contribution을 하기 위해서는 clone이 아니라 fork를 해야 한다. (github에서 오른쪽 상단에 있는 fork)
