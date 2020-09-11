<h2>Docker</h2>

도커 참조 : http://pyrasis.com/docker.html

가상머신은 guest OS가 다 설치되는 구조이기 때문에 많은 시간을 낭비하고 파일의 크기가 커진다. 도커는 OS가 필요하지 않기 때문에 훨씬 빠르게 환경을 기동할 수 있다.

- Docker 설치

  ```
  [vagrant@demo html]$ sudo su
  [root@demo html]# cd
  [root@demo ~]# yum install -y docker
  ```

- Docker 서비스 기동

  `[root@demo ~]# systemctl start docker.service`

- Docker Hub 로그인

  `[root@demo ~]# docker login`

- 이미지 획득 및 확인

  ```
  [root@demo ~]# docker pull docker.io/centos
  [root@demo ~]# docker image ls
  ```

- 태그를 지정해서 이미지 획득

  `[root@demo ~]# docker pull docker.io/centos:centos7`

- 컨테이너 실행 및 확인

  ```
  [root@demo ~]# docker run -t -d --name centos7 docker.io/centos:centos7
  [root@demo ~]# docker container ls	
  ```

- 컨테이너 내부에 명령어 실행

  ```
  # 컨테이너 내부에 설치되어 있는 CentOS 버전을 확인
  [root@demo ~]# docker exec centos7 cat /etc/redhat-release
  [root@demo ~]# docker exec -it centos7 /bin/bash
  # 실행 중인 컨테이너 내부로 진입
  [root@61260c982277 /]# cat /etc/redhat-release
  [root@61260c982277 /]# exit
  ```

- 컨테이너 정지 / 재기동 / 삭제

  ```
  [root@demo ~]# docker container stop centos7
  [root@demo ~]# docker container start centos7
  [root@demo ~]# docker container rm -f centos7
  # 이미지는 남아있음
  [root@demo ~]# docker image ls
  ```

- nginx 컨테이너 기동

  ```
  # 공식 배포 이미지는 docker.io를 생략 가능
  [root@demo ~]# docker pull nginx
  # 호스트의 8000번 포트와 컨테이너 내부의 80번 포트를 연결
  [root@demo ~]# docker container run -d -p 8000:80 --name nginx-latest nginx
  # localhost : Vagrant로 생성한 CentOS. nginx 컨테이너의 80번 포트로 매핑
  [root@demo ~]# curl http://localhost:8000
  ```

- Dockerfile을 이용해서 이미지를 생성

  - 컨테이너 이미지 내부로 전달할 파일을 생성

    ```
    [root@demo ~]# echo "Hello, Docker." > hello-docker.txt
    ```
  
  - Dockerfile을 생성 -> 이미지 생성에 사용
  
    ```
  [root@demo ~]# vi Dockerfile
    ```
    
    ```
    FROM docker.io/centos:latest        ⇐ 베이스 이미지를 지정
    ADD  hello-docker.txt /tmp          ⇐ 호스트에 있는 hello-docker.txt 파일을 컨테이너 이미지의 /tmp 아래로 복사
    RUN  yum install -y epel-release    ⇐ 컨테이너 이미지를 만들 때 실행
    CMD  [ "/bin/bash" ]                ⇐ 컨테이너가 실행될 때 실행할 명령어
    ```
    
  - Dockerfile을 이용해서 이미지를 생성 (이미지를 만드는 첫 번째 방법)
  
    ```
    # docker image build : Dockerfile을 이용해서 이미지를 생성
    # -t DOCKER_HUB_ID/IMAGE_NAME:TAG_NAME
    # . : Dockerfile위치 (현재 위치)
    [root@demo ~]# docker image build -t dbsydde/centos:1.0 .
    ```
  
  - 생성한 이미지를 이용해서 컨테이너를 실행
  
    ```
    # 컨테이너 ID 나옴
    [root@demo ~]# docker container run -td --name devops-book-1.0 dbsydde/centos:1.0
    fc844b694b6409149c0774450ff540654ca51c4ed218b8558b784dbe75e48c24
    ```
  
  - 컨테이너 내부로 진입
  
    ```
    [root@demo ~]# docker exec -it devops-book-1.0 /bin/bash
    # 이미지 생성 중 파일 복사되었는지 확인
    [root@aa9eab5ad4c1 /]# cat /tmp/hello-docker.txt	
    Hello, Docker.
    # epel 패키지 설치 여부를 확인
    [root@aa9eab5ad4c1 /]# rpm -qa | grep epel
    epel-release-8-8.el8.noarch\
    ```
  
  - 컨테이너 내용을 변경 후 변경된 내용을 이미지로 생성 (이미지를 만드는 두 번째 방법)
  
    ```
    # 컨테이너 내부에 nginx 설치
    [root@aa9eab5ad4c1 /]# yum install -y nginx
    [root@aa9eab5ad4c1 /]# exit
    ```
  
    ```
    # docker container commit : 컨테이너의 현재 상태를 이미지로 기록(생성)
    # devops-book-1.0 : 컨테이너 이름
    # dbsydde/centos:1.1 : 이미지 이름
    [root@demo ~]# docker container commit devops-book-1.0 dbsydde/centos:1.1
    sha256:cbea17b934c507cf2685e4cfdaeab55a3f84463d32bd90b4d7999d4cf5d1cbc1
    ```
  
  - 도커 허브에 이미지를 등록
  
    ```
    [root@demo ~]# docker image push dbsydde/centos:1.1
    ```
  
  - 도커 허브에 등록된 이미지를 이용해서 컨테이너를 실행
  
    ```
    [root@demo ~]# docker run --name myanjini_centos -dt -p 8888:80 myanjini/centos:1.1
    ```
  
  - 모든 컨테이너를 삭제
  
    ```
    [root@demo ~]# docker container rm -f $(docker container ls -a -q)
    ```
  
    
  
- Docker Compose

  - Docker Composer 설치

    ```
    [root@demo ~]# curl -L https://github.com/docker/compose/releases/download/1.8.0/docker-compose-`uname -s`-`uname -m` > /usr/bin/docker-compose
    [root@demo ~]# chmod +x /usr/bin/docker-compose
    [root@demo ~]# docker-compose --version
    docker-compose version 1.8.0, build f3628c7
    ```

  - docker-compose.yml 파일을 생성

    ```
    [root@demo ~]# vi docker-compose.yml
    ```

    ```
    db:
      image: docker.io/mysql               ⇒ 컨테이너 이미지
      ports:
        - "3306:3306"                      ⇒ 호스트 포트 : 컨테이너 내부 포트(서비스 포트)
      environment:                         ⇒ 컨테이너 내부의 환경 변수
        - MYSQL_ROOT_PASSWORD=password
    
    app:
      image: docker.io/tomcat
      ports:
        - "9090:8080"
    
    web:
      image: docker.io/nginx
      ports:
        - "9000:80"
    ```

  - 컨테이너 생성 및 확인

    ```
    [root@demo ~]# docker-compose up -d
    [root@demo ~]# docker container ls
    ```

  - 컨테이너 중지

    ```
    [root@demo ~]# docker-compose down
    ```

- Jenkins

  - JDK 설치

    ```
    [root@demo ~]# yum -y install java-1.8.0-openjdk java-1.8.0-openjdk-devel
    ```

  - Jenkins 설치

    ```
    [root@demo ~]# yum -y install http://mirrors.jenkins-ci.org/redhat-stable/jenkins-2.235.5-1.1.noarch.rpm
    ```

  - Jenkins 기동

    ```
    [root@demo ~]# systemctl start jenkins.service
    ```

  - Jenkins 접속

    호스트 PC에서 브라우저를 이용해서 http://192.168.33.10:8080/ 접속

    - 접속되지 않는 경우 방화벽에 서비스 포트 등록(허용)

      ```
      [root@demo ~]# systemctl start firewalld
      [root@demo ~]# firewall-cmd --zone=public --permanent --add-port=8080/tcp
      [root@demo ~]# firewall-cmd --reload
      ```

    - 접속에 필요한 패스워드 확인

      ```
      [root@demo ~]# cat /var/lib/jenkins/secrets/initialAdminPassword
      ```

      
