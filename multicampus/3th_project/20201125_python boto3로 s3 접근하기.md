> <h2>AWS S3 with Python boto3</h2>



​	python boto3 라이브러리를 이용해 S3에 접근하기 위해서는 IAM 사용자 권한이 필요하다.

- IAM 계정 추가

  ![이미지 2](https://user-images.githubusercontent.com/30336831/100188978-ba910780-2f2e-11eb-9f71-9ea7911b3791.png)

  

  AmazonS3FullAccess 권한 추가

  ![이미지 3](https://user-images.githubusercontent.com/30336831/100189131-05128400-2f2f-11eb-9867-297608b4c9ad.png)

  

  완료한 후 생성되는 <b>비밀 액세스 키</b>는 다운로드받아 잘 보관한다. (또는 기록해두기)

  액세스 키 ID, 비밀 액세스 키는 python boto3에서 S3에 접근할 경우 id, password로 사용된다.

  

  boto3와 aws cli 설치

  ![이미지 4](https://user-images.githubusercontent.com/30336831/100189506-c7fac180-2f2f-11eb-98df-f835b9df057b.png)

  

  ![이미지 5](https://user-images.githubusercontent.com/30336831/100189579-e52f9000-2f2f-11eb-833c-ccecdd1636a1.png)

  

  `aws configure` 명령어로 인증 정보 설정

  ![이미지 6](https://user-images.githubusercontent.com/30336831/100189755-42c3dc80-2f30-11eb-9a42-0f61e2950dfe.png)

  

  이렇게 설정해주고 나면 다음과 같이 boto3로 S3에 접근할 수 있다.

  ```
  import boto3
  
  # S3 Client 생성
  s3 = boto3.resource('s3')
  
  # S3 Bucket 생성 
  bucket_name = 'test-bucket'
  s3.create_bucket(Bucket=bucket_name , CreateBucketConfiguration={'LocationConstraint': 'ap-northeast-2'})
  
  # S3 Bucket 에 파일 업로드 
  data = open('test.png', 'rb')
  s3.Bucket(bucket_name).put_object(Key='test.png', Body=data)
  ```

  

  

