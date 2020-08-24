- create user 했을 때 django.db.utils.IntegrityError: (1048, "Column 'last_login' cannot be null") 에러 해결 방법

```
# 0005 migrate 안 되어있는 것 확인
$ python manage.py showmigrations auth
auth
 [X] 0001_initial
 [ ] 0002_alter_permission_name_max_length
 [ ] 0003_alter_user_email_max_length
 [ ] 0004_alter_user_username_opts
 [ ] 0005_alter_user_last_login_null
 [ ] 0006_require_contenttypes_0002
 [ ] 0007_alter_validators_add_error_messages
 [ ] 0008_alter_user_username_max_length
 [ ] 0009_alter_user_last_name_max_length
 
$ python manage.py migrate auth 0005
```

- 유저가 권한이 없어서 데이터베이스에 접근하지 못할 때 해결 방법 (Access denied for user 'user_name'@'localhost' to database 'db_name')

```
$ grant all privileges on project_db.* to 'python'@'localhost';
```

- superuser로 admin 계정 로그인 시 pymysql.err.ProgrammingError: (1146, "Table 'project_db2.django_session' doesn't exist") 발생

  -> python manage.py migrate app_name 을 하면 장고 앱에서 모델을 통해 테이블만 생성해준다. 모델 수정했을 때 쓰는 명령

  -> django의 모든 테이블을 생성하고 init 하기 위해서는

  ```
  $ python manage.py migrate
  ```

  위의 맨 첫 번째 에러도 이것 때문이었다!
  
- AttributeError: Cannot use add() on a ManyToManyField which specifies an intermediary model. Use movieapp.WishList's Manager instead. 아직 해결 못함.