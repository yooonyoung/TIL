<h2>Amazon S3 이벤트 응답으로 Step Functions 실행하기</h2>

S3에 이미지가 업로드됨을 감지해서 CloudWatch Events를 사용해 Step Functions를 실행하도록 구성해볼 것이다. Step Functions에서는 S3에 저장된 이미지를 ai 모델을 활용해 마스크 착용 유무를 판단하고, Amazon RDS에 통계 정보를 저장하는 로직이 기본적으로 포함될 예정이다.



<h4>Step Functions 생성하기</h4>

이미 생성해놨으니까 패스!



<h4>Amazon S3 버킷 만들기</h4>

이미지를 업로드할 S3 버킷을 생성한다.



<h4>CloudTrail 추적 생성하기</h4>

![이미지 6](https://user-images.githubusercontent.com/30336831/100575743-176e3280-3320-11eb-9d75-2951a5d8834a.png)

![이미지 7](https://user-images.githubusercontent.com/30336831/100575866-543a2980-3320-11eb-93e3-aab46f494402.png)

이름을 입력하고, 로그를 저장할 새 S3 버킷을 생성한다. ~~AWS KMS 별칭은 뭔지 모르겠다.....~~

버킷 이름에는 영어 소문자와 하이픈(-)만 가능한데 잘못 입력했다!

![이미지 9](https://user-images.githubusercontent.com/30336831/100576127-cad72700-3320-11eb-8ff3-139afd227487.png)

S3 이벤트를 등록해준다.



<h4>CloudWatch 이벤트 규칙 생성하기</h4>

CloudWatch 콘솔에서 이벤트 - 규칙 - 규칙 생성을 클릭한다.

![이미지 10](https://user-images.githubusercontent.com/30336831/100576674-f870a000-3321-11eb-86bd-9fc51d8dfb2b.png)

![이미지 11](https://user-images.githubusercontent.com/30336831/100576974-8e0c2f80-3322-11eb-9013-2946254cbebc.png)

위 이미지와 같이 이벤트를 구성해준다.

![이미지 12](https://user-images.githubusercontent.com/30336831/100577190-04a92d00-3323-11eb-9bb2-c4d77651e601.png)

대상은 위 이미지처럼 Step Functions 상태 시스템으로 설정해주고, 아까 만든 Step Functions 를 지정해준다. 여기서는 S3 이벤트가 발생하면 입력 없이 그냥 호출하지만, 입력 구성 탭에서 특정 입력을 지정해줄 수 있다. 

다음 세부 정보 구성 단계에서 이름과 설명을 지정해주고 규칙을 생성한다.



<h4>CloudWatch 규칙 테스트하기</h4>

파일을 S3 버킷에 업로드해 보면, AWS Step Functions 콘솔에서 실행 결과를 볼 수 있다.

![이미지 13](https://user-images.githubusercontent.com/30336831/100577651-01fb0780-3324-11eb-88a7-9cc77eb36eb5.png)
![이미지 14](https://user-images.githubusercontent.com/30336831/100577657-032c3480-3324-11eb-83be-c3bb84bb752f.png)

다른 입력 없이 S3 이벤트로 입력 구성을 해 놓으면 버킷 이름, 객체 이름 등의 정보가 입력으로 구성된다. 출력은 앞에서 했던 테스트 예제로, 제대로 출력되는 것을 볼 수 있다.