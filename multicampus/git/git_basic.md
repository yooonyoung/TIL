- 인터페이스

  서로 다른 객체를 연결해주는 접점

  ex) 컴퓨터 본체와 모니터 연결해주는 인터페이스 : HDMI

  ​	  사용자와 컴퓨터의 인터페이스 : 마우스, 컴퓨터

  

- API(Application Programming Interface, 응용 프로그램 프로그래밍 인터페이스)

  소프트웨어의 접점, 개발자에게 의미가 있는 접점, 다른 시스템 간의 커뮤니케이션 방식

  보내는 사람, 받는 사람, 우편번호만 알면 우체국이 무슨 일을 하는지 알 필요가 없는 것처럼  API를 사용할 때는 엔드포인트만 알면 된다. 

  서로 정한 약속만 지켜서 보내면 됨. url, key, value 등을 맞춰서 보내면 되는 것

  웹 API : 특정 서버와 다른 서버간의 대화 방식



- 인사이트 얻을 수 있는 사이트

  [Coursera][https://www.coursera.org/]
  
  [Edx - CS50][https://www.edx.org/?g_acctid=926-195-8061&g_campaign=new-gs-row-brand-core-bmm&g_campaignid=1666032422&g_adgroupid=68314562630&g_adid=321751434846&g_keyword=%2Bedx&g_keywordid=kwd-313851179758&g_network=g?utm_source=adwords&utm_campaign=1666032422&utm_medium=68314562630&utm_term=+edx&gclid=Cj0KCQjwvvj5BRDkARIsAGD9vlLqVmR80CIbl4KUmjtp0uaywtdFSGc8xCfjWkZ_TbFngH0Jg1k8NxAaAv-rEALw_wcB]
  
  [Udacity][https://www.udacity.com/]



```
$ pip freeze 
$ pip freeze > packages
$ pip install -r packages
# 하지만 packages라는 이름보다는 requirements.txt로 정해져있다
# 이름 바꾸기 - move 명령어지만 같은 위치에서 실행하게 되면 이름을 바꿔준다
$ mv packages requirements.txt

# requirements.txt에 있는 모듈 모두 설치하기
$pip install -r requirements.txt 

# 가상환경 생성 - venv 라는 이름의 폴더로
$python -m venv venv 
# global python을 가리키는 것이 아니라 따로 만들어진 파이썬 가상환경을 가리키게 하기
$source venv/Scripte/activate 
# venv에 있는 딱 두개 pip,setuptools만 나온다.
# 순수 파이썬이기 때문에 가상환경 만들 때마다 django도 설치해주어야 한다. 퓨어한 파이썬이어서!
$pip list
```

- Semantic  : 의미를 담고 있는 태그 . semver

![Image](C:\Users\i\AppData\Local\Temp\Image.png)



```
# 깃 이름/이메일 바꾸기(컴퓨터 계정 당 일회성으로 깃 설치할때 하는 것. 뒤에 붙는 이름)
$ git config --global user.name 'young'
$ git config --global user.email 'dbsydde@gmail.com'

# 글로벌은 저걸로 쓰고 이 폴더에서만 이걸로 서명할래요!(init 한다음 할 수 있음)
$ git config --local user.name 'young2' 

$ git init 
# .git이라는 폴더가 생긴 것을 볼 수 있음
# git init은 현재 폴더에 .git폴더를 만드는 것이다.
$ ls -a 
# 깃 그냥 삭제하려면 
$ rm -rf .git
```

- Project에도 마크다운 파일을 넣어주는 것이 국룰이다. Readme.markdown넣어주면 github에서 맨 처음 페이지에 뜬다 (Typora 활용)
- master가 붙어있는 깃 폴더는 repo라고 부른다.
- 

