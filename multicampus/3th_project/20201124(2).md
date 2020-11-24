><h2>AWS IoT Greengrass - 2 </h2> 



- Greengrass 그룹 생성

  ![이미지 1](https://user-images.githubusercontent.com/30336831/100091049-71da3f80-2e97-11eb-9d3f-c14eabdd0067.png)

  <b>기본 생성 사용</b> 클릭

  <b>그룹 이름, 코어 이름</b> 지정

  <b>코어의 보안 리소스</b> 다운로드 받아서 IoT한테 전달해주어야 함

  루트 CA는 눌러서 <b>Amazon Root CA 1</b> 오른쪽 클릭 - 링크 저장 
  
  

- 정책 설정

  <b>Greengrass 그룹 MaskTest - 설정 - 그룹 역할</b>

  정책이 다음과 같이 AWSGreengrassResourceAccessRolePolicy, AWSGreengrassFullAccess 두 개가 있어야 함. 없으면 역할 연결해주고 IAM 서비스로 가서 편집

  ![이미지 8](https://user-images.githubusercontent.com/30336831/100091654-4015a880-2e98-11eb-94d3-a7855a28e702.png)

- 람다 함수 생성

  https://docs.aws.amazon.com/greengrass/latest/developerguide/what-is-gg.html#gg-core-sdk-download 에서 언어에 맞는 AWS IoT Greengrass Core SDK 다운로드 (나는 Python)

  examples는 샘플 코드고 greengrasssdk 폴더가 SDK이다.

  examples - HelloWorld 폴더 안으로 greengrasssdk 폴더를 복사한 다음 같이 압축.

  

  ![이미지 9](https://user-images.githubusercontent.com/30336831/100092195-04c7a980-2e99-11eb-8d03-44bd7e427220.png)

  

  압축한 zip 파일 람다에 업로드

  <b>Runtime settings</b> 다음과 같이 수정

  ![이미지 10](https://user-images.githubusercontent.com/30336831/100092346-3e001980-2e99-11eb-94a9-ad2bd88cdb33.png)

  

  <b>작업</b> - <b>새 버전 발행</b>

  ![이미지 13](https://user-images.githubusercontent.com/30336831/100092529-920afe00-2e99-11eb-9248-75af3efa8d08.png)

  

  <b>작업</b> - <b>별칭 생성</b>

  ![이미지 13](https://user-images.githubusercontent.com/30336831/100092670-cc749b00-2e99-11eb-8fcf-dd0105e70f84.png)

  ![이미지 15](https://user-images.githubusercontent.com/30336831/100092853-1493bd80-2e9a-11eb-928f-620c9609ecf4.png)



- Greengrass 그룹에 Lambda 추가

  ![이미지 16](https://user-images.githubusercontent.com/30336831/100093101-64728480-2e9a-11eb-80c6-1f389d374e46.png)

  MaskTest 람다 선택하고

  ![이미지 17](https://user-images.githubusercontent.com/30336831/100093308-b0252e00-2e9a-11eb-8fe4-6ba1a573bb58.png)

  

  추가한 람다 함수 - <b>구성 편집</b>

  ![이미지 18](https://user-images.githubusercontent.com/30336831/100093527-009c8b80-2e9b-11eb-8687-af312b612497.png)

  ![이미지 19](https://user-images.githubusercontent.com/30336831/100093622-20cc4a80-2e9b-11eb-9c02-6a89b96c8878.png)