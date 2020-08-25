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
  
- QuerySet Replace 해서 새로운 임시 컬럼 만들어 사용하기

```
  # title 컬럼의 값들을 공백 제거해서 fixed_title이라는 컬럼으로 만들기
  movielist_replace = Movie.objects.annotate(
      fixed_title=Replace('title', Value(' '), Value('')), )
```

- 2개 이상의 QuerySet 합치기

```
# q1, q2, q3, q4 네 개의 쿼리셋을 하나로 합쳐 merge_query에 저장
merge_query = q1.union(q2, q3, q4)
# 또는 리스트로 만들어 병합할 수도 있다
q1 = list(q1)
q2 = list(q2)
result = q1 + q2
```

