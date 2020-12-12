<h2>Django EC2에 배포하기 - Gunicorn, Nginx 연결</h2>

Django는 Nginx 웹 서버와 직접적으로 통신할 수 없어 중간에서 Gunicorn server를 설치해주어야 한다. runserver는 개발용이고 배포용이 아니기 때문에 안정적인 배포를 위해 Gunicorn 서버를 Django와 연결해야 한다.

또 사용자의 브라우저를 통한 요청은 웹 서버가 받아서 처리해야 하고 Gunicorn이 받아 처리하는 것은 적절하지 않기 때문에 Nginx 웹 서버가 필요하다. 따라서 Gunicorn 서버와 Nginx를 연결해 배포를 해 볼 것이다.

<h4>Gunicorn 설치 및 시작하기</h4>

<b>(EC2 서버)</b>venv 가상환경에서 Gunicorn을 설치한다.

```
$ pip install Django gunicorn
```

```
(프로젝트 루트 폴더에서)
$ mkdir run
$ sudo chown ubuntu:www-data run
```

![이미지 5](https://user-images.githubusercontent.com/30336831/101238525-373c9680-3724-11eb-8b0d-b61c4ce6d9cc.png)

Gunicorn으로 서버를 구동해본다.

```
$ gunicorn --bind 0.0.0.0:8888 장고프로젝트이름.wsgi:application
```

브라우저에서 http://IP주소:8888/ 로 접속해 잘 출력되는지 확인한다.

가상환경에서 빠져나온다.

```
$ deactivate
```

system에 Gunicorn을 등록하기 위해 아래 파일을 생성해준다.

```
$ sudo vi /etc/systemd/system/gunicorn.service
```

```
[Unit]
Description=gunicorn daemon
After=network.target

[Service]
User=ubuntu
Group=www-data
WorkingDirectory=/home/ubuntu/DjangoServer
ExecStart=/home/ubuntu/DjangoServer/venv_for_django/bin/gunicorn \
        --workers 1 \
        --bind unix:/home/ubuntu/DjangoServer/run/gunicorn.sock \
        Project.wsgi:application

[Install]
WantedBy=multi-user.target
```

WorkingDirectory는 <b>프로젝트 루트 폴더</b>, ExecStart 앞부분은 <b>가상환경 안의 gunicorn 경로</b>, 뒷부분은 아까 생성한 <b>run 디렉토리 경로</b>이다.

workers 옵션은 <b>프로세스의 갯수</b>이다. 여러 개의 프로세스로 나눠서 병렬로 처리할 수 있게 되는데, 나는 Django에서 전역 변수를 쓸 일이 있어서 단일 프로세스로 돌리려고 1로 지정했다.

저장한 후 데몬을 구동시키고, 제대로 실행되었는지 확인한다.

```
$ sudo systemctl start gunicorn
$ sudo systemctl enable gunicorn
$ sudo systemctl status gunicorn
```

![이미지 3](https://user-images.githubusercontent.com/30336831/101236667-7e238f80-3716-11eb-917f-9e2f971990e6.png)

 <h4>Nginx 설치하기</h4>

다음 명령어로 Nginx를 설치한다.

```
$ sudo apt-get update
$ sudo apt-get install nginx
```

service로 nginx를 시작하고 상태를 확인해본다.

```
$ service nginx restart 
$ service nginx status
```

![이미지 4](https://user-images.githubusercontent.com/30336831/101236759-086bf380-3717-11eb-9268-a86d9844274d.png)

<h4>Nginx 설정 추가하기</h4>

```
$ sudo vi /etc/nginx/sites-available/프로젝트이름
```

```
server {
        listen 80;
        server_name 3.35.178.102;

        location = /favicon.ico { access_log off; log_not_found off; }

        location /static/ {
                root /home/ubuntu/DjangoServer;
        }

        location / {
                include proxy_params;
                proxy_pass http://unix:/home/ubuntu/DjangoServer/run/gunicorn.sock;
        }
}
```

Nginx 설정파일은 `/etc/nginx/sites-enabled` 경로의 파일을 보고 있으므로 링크를 걸어준다.

```
$ sudo ln -s /etc/nginx/sites-available/django_test /etc/nginx/sites-enabled
```

만약 이미 존재해서 안된다고 하면 <b>f 옵션</b>을 걸어준다.

```
$ sudo ln -sf /etc/nginx/sites-available/django_test /etc/nginx/sites-enabled
```

문법 검사 후 Nginx를 다시 시작해준다.

```
$ sudo nginx -t
$ sudo systemctl restart nginx
```

그럼 된다!!!!!!

http://IP주소 로 들어가서 잘 출력되는지 확인한다.

참조한 글에서는 방화벽을 열어주어야 한다고 하는데 나는 하지 않아도 잘 되었다.

혹시 안되면 참고

```
$ sudo ufw delete allow 8000
$ sudo ufw allow 'Nginx Full'
```



<h4>static 파일 연결하기</h4>

브라우저에서 Django의 admin 페이지를 들어가보면 페이지를 꾸며주는 정적 파일이 로드되지 못한 것을 볼 수 있다. 이것은 static 파일 경로가 nginx에서 설정되어있지 않기 때문이다. 

먼저 Django의 <b>settings.py</b> 파일 제일 아래에 있는 STATIC_URL 밑에 다음과 같이 추가해준다.

```
STATIC_ROOT = os.path.join(BASE_DIR, 'static')
```

이렇게 만들고 <b>collectstatic 명령어</b>를 사용하면 STATIC_ROOT 경로에 각 앱의 static 파일들이 모아지게 된다.

EC2에서 다음 명령어를 실행해준다.

```
$ python manage.py collectstatic

132 static files copied to '/home/ubuntu/DjangoServer/static'.
```

이렇게 하면 static 폴더 밑에 admin 폴더 아래로 static 파일들이 모아진 것을 볼 수 있다.

```
$ sudo vi /etc/nginx/sites-available/프로젝트이름
```

```
server {
        listen 80;
        server_name 3.35.178.102;

        location = /favicon.ico { access_log off; log_not_found off; }

        location /static/ {
                root /home/ubuntu/DjangoServer;
        }

        location / {
                include proxy_params;
                proxy_pass http://unix:/home/ubuntu/DjangoServer/run/gunicorn.sock;
        }
}
```

가운데의 /static/ 부분을 추가해준다.

다시 링크를 걸어주고 nginx를 restart 하면 끝!

```
$ sudo ln -sf /etc/nginx/sites-available/django_test /etc/nginx/sites-enabled
$ sudo nginx -t
$ sudo systemctl restart nginx
```

