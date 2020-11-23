- SageMaker와 Serverless 연결하기

![sageMaker](https://www.sagemaker-workshop-kr.com/images/apps/internet_facing_app/image14.png?width=%226.728391294838145in%22height=%223.4464293525809273in)

![dataflow](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F9907DC485C85C0C82A)

SageMaker는 쉽고 빠르게 구성, 학습하고 기계 학습 모델을 배포할 수 있도록 해주는 관리형 서비스!

SageMaker에서 Jupyter Notebook Instance를 생성하고 작업한 후, 모델을 배포해서 사용할 수 있다.

-> 사용하기 위해서는 (여기서는 마스크 썼는지 안 썼는지 판단, 예측) SageMaker에서 모델을 배포하고 SageMaker EndPoint를 만들어준 다음, 이 EndPoint를 람다와 연결해주어야 한다.

위의 그림에서는 User가 웹브라우저로 되어 있지만 우리 프로젝트에서는 IoT가 될 것

sageMaker는 사용하지 않을때 중지하거나 종료하는 것이 중요! 겁나 비쌈



궁금한 점) s3에 있는 마스크 이미지들을 sageMaker의 Jupyter Notebook에서 어떻게 가져와서 사용할 수 있지?

s3로 SageMaker 엔드포인트를 만들 수 있나? -> s3에 모델을 저장하고, 엔드포인트는 엔드포인트대로 따로 만드는 듯

