- 기존에 관리하고 있던 파일이나 폴더를 ignore 하는 방법

  기존의 git 관리를 받고 있던 파일이나 폴더를 .gitignore 파일에 작성하고 add > commit > push 해도 무시되지 않음 -> 기존에 가지고 있는 cached를 지워야 한다.

  ```
  ## 파일
  git rm --cached test.txt 
  
  ## 전체파일
  git rm --cached *.txt 
  
  ## 폴더
  git rm --cached test/ -r
  ```

  git rm --cache 명령어는 Staging Area에서 파일을 제거하고 working directory에는 파일을 유지하는 명령어

  위의 명령어를 실행하고 꼭 commit 까지 해주어야 함