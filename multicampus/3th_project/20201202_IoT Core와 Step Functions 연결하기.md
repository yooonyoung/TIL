<h2>IoT Core로 메시지 수신 시 Step Functions 호출</h2>

 출처 : https://docs.aws.amazon.com/ko_kr/iot/latest/developerguide/iot-lambda-rule.html

IoT MQTT 통신 방식은 잘 모르지만 어쨌든 라즈베리파이에서 AWS로 메시지가 수신되었을 때 Step Functions가 호출되도록 구성해보았다.

<h4>IoT Core 사물 생성</h4>

![이미지 11](https://user-images.githubusercontent.com/30336831/100857876-96a26880-34d0-11eb-851a-e0afa4d95aa1.png)

먼저 정책을 만들어줘야 한다.

<b>보안 - 정책 - 정책 생성</b>

![이미지 12](https://user-images.githubusercontent.com/30336831/100858082-dc5f3100-34d0-11eb-8eee-385b830f1fd4.png)

정책 이름과 필요한 작업을 입력해준다.

리소스 ARN은 일단 *로 해준다. 사물을 생성해야 ARN이 나오기 때문에 나중에 바꿔줄 예정!

허용 선택해주고 정책을 생성한다.

![이미지 6](https://user-images.githubusercontent.com/30336831/100856869-4d9de480-34cf-11eb-9713-fff2f36e01c3.png)

사물을 생성해보자.

<b>관리 - 사물 - 단일 사물 생성</b> 클릭

![이미지 7](https://user-images.githubusercontent.com/30336831/100857317-e7fe2800-34cf-11eb-93c7-16586de25d27.png)

이름만 적어주고 <b>다음</b> 클릭한다.

![이미지 8](https://user-images.githubusercontent.com/30336831/100857506-167c0300-34d0-11eb-82c6-bb861acb9f07.png)

![이미지 10](https://user-images.githubusercontent.com/30336831/100857652-47f4ce80-34d0-11eb-8ac4-0401ad23888a.png)

인증서를 모두 다운로드해주고 루트 CA도 다운로드한 후, <b>활성화</b>를 해준다. <b>정책 연결</b>!

그럼 사물이 생성된다.



<h4>Step Functions 호출하는 규칙 생성하기</h4>

![이미지 13](https://user-images.githubusercontent.com/30336831/100858418-4b3c8a00-34d1-11eb-8ee2-a1fb0344e633.png)

<b>동작 - 규칙 - 규칙 생성</b> 클릭

![이미지 14](https://user-images.githubusercontent.com/30336831/100858796-caca5900-34d1-11eb-9057-14e98ee18d75.png)

규칙 쿼리는 다음과 같이 설정한다.

`SELECT * FROM 'kf99/topic' WHERE temperature > 30`

`kf99/topic`은 IoT 디바이스가 주제를 게시하는 곳이고 WHERE 절은 규칙 이벤트? 가 발생하는 조건이다. 이번 프로젝트에서는 온도 데이터를 받기 때문에, 온도가 30도 이상인 경우에만 감지하도록 조건절을 추가해 보았다.

![이미지 15](https://user-images.githubusercontent.com/30336831/100859049-1a108980-34d2-11eb-92e9-f642aa783930.png)

<b>작업 추가</b>를 클릭한다.

![이미지 16](https://user-images.githubusercontent.com/30336831/100859141-3ca2a280-34d2-11eb-94b1-41d43721753f.png)

나오는 항목 중 <b>Step Functions 상태 시스템 실행 시작</b>을 선택한다.

![이미지 17](https://user-images.githubusercontent.com/30336831/100859358-7bd0f380-34d2-11eb-843c-377fcfa92d26.png)

생성해놓은 Step Functions 상태 머신을 선택한다.

실행 이름 접두사는 뭔지 모르겠어서 일단 저렇게 해 놓았다.

역할은 새 역할을 생성해도 되고, 이미 만들어놓은 게 있다면 선택하면 된다.

<b>작업 추가</b> 클릭

![이미지 18](https://user-images.githubusercontent.com/30336831/100859536-ae7aec00-34d2-11eb-86c0-5ce1ca8f3ed3.png)

다음은 오류 작업의 <b>작업 추가</b>를 선택한다.

![이미지 19](https://user-images.githubusercontent.com/30336831/100859657-d5d1b900-34d2-11eb-8d60-15a850859a57.png)

에러가 났을 때 메시지로 수신되도록 <b>메시지를 AWS IoT 주제에 재게시</b>를 선택한다.

![이미지 21](https://user-images.githubusercontent.com/30336831/100860245-98b9f680-34d3-11eb-84b6-bd18874c0c7d.png)

에러메시지를 게시할 주제를 정해주고, 마찬가지로 역할을 생성하거나 이미 있다면 역할을 선택해준다.

서비스 품질은 IoT 팀원 분과 상의해야 할 것 같다.

![이미지 23](https://user-images.githubusercontent.com/30336831/100860639-1a118900-34d4-11eb-9a70-1a0312cbc7bd.png)

테스트를 위해서 <b>테스트</b> 탭으로 이동한다.

에러 메시지를 구독하기 위해 구독 주제에 `kf99/error`를 입력했다.

<b>주제 구독</b> 클릭

![이미지 24](https://user-images.githubusercontent.com/30336831/100860763-462d0a00-34d4-11eb-92df-119ef020357a.png)

주제를 게시하는 부분에는 `kf99/topic`을 입력했다.

<b>주제 게시</b>를 클릭하면, 메시지가 수신될 때 Step Functions가 호출될 것이다.

테스트를 위해서는 라즈베리파이를 연결해야 해서 오늘은 여기까지!