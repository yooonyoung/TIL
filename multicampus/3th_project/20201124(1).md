> <h2>AWS IoT Greengrass - 1</h2>



- AWS IoT Greengrass를 쓰자

  라즈베리파이와 어떻게 통신할지 고민하던 도중 알게 된 aws 서비스이다.

  AWS IoT Greengrass를 쓰면, 클라우드의 기능을 로컬 디바이스로 확장시켜(옮겨서?) 사용할 수 있다.

  로컬 디바이스 내에서 람다를 쓸 수도 있는 것 같다.

  라즈베리파이에서 HTTP 통신도 가능하긴 하지만, 보안상 이게 더 좋다고 한다. 디바이스가 많을 경우 엄청나게 많은 센서들이 있는데 이 센서들이 모두 인터넷망으로 클라우드에 연결되면 네트워크 대역폭이 버티지도 못하고, 데이터 처리 시간도 길어진다. 그래서 <b>IoT 가까이에 있는 네트워크(엣지)</b>에서 1차적으로 데이터를 처리한 후 <b>엣지를 클라우드와 연결</b>해 DB 관련 처리를 하는 방식이다!

  그리고 무엇보다 중요한 사실은 <b>AI 모델을 로컬 디바이스 환경 내에서 사용</b>할 수 있다. 마스크를 썼는지 안 썼는지 거의 실시간처럼 빠르게 판단되어야 하는 상황에서 API Gateway와 Lambda까지 왔다갔다하면 조금 느려지지 않을까 걱정했었는데 너무 좋은 기능인 것 같다.

  

  ![흐름도](https://d2908q01vomqb2.cloudfront.net/f6e1126cedebf23e1463aee73f9df08783640400/2018/12/06/1_architecture.jpg)

  

  출처 : https://aws.amazon.com/ko/blogs/korea/machine-learning-at-the-edge-using-and-retraining-image-classification-models-with-aws-iot-greengrass-part-1/

  

  - 구현 순서

  1. 라즈베리파이에 Greengrass 그룹 배포 (설치 및 환경 구축)
  2. 람다 함수 생성 후 Greengrass 그룹의 람다로 추가
  3. 이미지 분류 모델 구축(SageMaker)
  4. Greengrass 그룹에 머신러닝 리소스 생성 (모델 선택해서)
  5. AWS IoT Greengrass Image Classification 커넥터 생성 (람다 함수가 모델에 접근 가능해짐)
  6. 캡쳐할 이미지를 저장할 로컬 디렉터리 구성(AWS, 라즈베리파이 둘 다)
  7. [테스트]를 사용해서 구독 구성. 이벤트 트리거는 여러 가지가 있음. response topic을 subscribe하고, request topic을 publish

