<h2>REST API URL 규칙</h2>

> 프로젝트 멘토링 때, 멘토님께서 Django REST API를 성숙도 모델과 규칙에 맞게 수정해보면 훨씬 더 좋겠다고 말씀해주셨다. 그래서 먼저 URL 규칙에 대해서 정리해보려고 한다. 

​	참고 : [Rechardson의 API 성숙도 모델](https://martinfowler.com/articles/richardsonMaturityModel.html)



<h4>가장 기본</h4>

1. URI는 <b>정보의 자원을 표현</b>해야 한다.
2. 자원에 대한 <b>행위는 HTTP Method(GET, POST, PUT, DELETE)로 표현</b>한다.



<h4>RESTful한 URL</h4>

1. 소문자를 사용한다.

   ```
   http://restapi.example.com/users/comments
   ```
   
1. 언더바 대신 하이픈을 사용한다.
   
   가급적이면 하이픈도 사용하지 않는 것이 좋지만, 가독성을 위해 단어의 결합이 꼭 필요할 때만 사용한다.
   
   ```
   http://restapi.example.com/users/post-comments
   ```
   
1. 맨 끝에 슬래시를 포함하지 않는다.

   슬래시는 계층을 구분할 때만 쓰고, 맨 마지막에는 사용하지 않는다.

   ```
   http://restapi.example.com/users
   ```

1. 행위는 URL에 포함하지 않는다.

   행위는 URL 대신 method를 사용해 전달한다.

   ```
   DELETE http://restapi.example.com/users/posts (O)
   POST http://restapi.example.com/users/delete-post (X)
   ```

1. 전달하고자 하는 자원의 명사를 사용하되, 컨트롤 자원을 의미할 경우에만 예외적으로 동사를 사용한다.

   ```
   http://restapi.example.com/posts/duplicate
   ```









