## AWS 서비스로 웹 애플리케이션 만들기

#### 웹 애플리케이션이란?

인터넷을 통해, 웹 브라우저에서 이용할 수 있는 응용 프로그램
사용자(클라이언트)가 웹 브라우저의 주소 창을 통해 서비스를 요청하면
서버 쪽에서 요청을 처리하여 결과를 사용자의 웹 브라우저에 보내주는 형식으로 작동한다.

### Amazon EC2 기반 웹 애플리케이션
#### 1. 아키텍처
![](https://images.velog.io/images/younge/post/02a21dba-a6db-43ae-b7f5-a6d8225ae9ca/image.png)
- **Amazon Route53**
높은 가용성과 뛰어난 확장성을 가진 DNS 웹 서비스
도메인 명을 관리, 등록할 때 사용한다.

- **CloudFront**
짧은 지연 시간과 빠른 전송 속도로 데이터, 애플리케이션 및 API를 사용자에게 안전하게 전송하는 서비스

- **S3**
웹 어플리케이션의 사진, 동영상 등의 정적인 컨텐츠를 처리한다.

- **Elastic Load Balancing (ELB)**
누가, 언제, 어떻게 요청하는지에 따라 각기 다른 내용이 반환되는 동적인 컨텐츠를 처리한다.

- **Auto Scaling Group**
애플리케이션의 트래픽을 탄력적으로 처리하기 위해 사용한다. 

- **Amazon RDS**
실질적인 데이터를 가지고 있는 데이터 티어. 서비스의 요구사항에 따라 관계형 데이터베이스인 RDS를 사용하거나 NoSQL 데이터베이스인 DynamoDB를 사용한다.
#### 2. EC2, RDS, S3
- **Amazon Elastic Compute Cloud(EC2)**
  - 크기 조정이 가능한 컴퓨팅 파워
  - 워크로드 목적에 따른 다양한 인스턴스 타입 제공
  - 보안 및 네트워크 구성과 관련 스토리지 관리 용이
  - 다양한 AMI(Amazon Machine Image) 제공
  - **Auto Scaling Group** 
    - Auto Scaling 기능이 실현되는 논리적인 그룹. 
    - Auto Scaling Group을 Elastic Load Balancer에 걸어주면 Load Balancer를 통해 유입되는 요청을 여러 EC2에 분산할 수 있고 안전성을 확보할 수 있다.
  
- **Amazon Relational Database Service(RDS)**
  - 관리형 관계형 데이터베이스 서비스
  - 다양한 데이터베이스 엔진 제공
  - 하드웨어 준비, DB 소프트웨어 설치 및 관리 작업 불필요
  - 다중 AZ를 통한 동기식 복제, 자동화된 백업, 스냅샷, 장애 조치
  - 다운타임 없이 컴퓨팅 및 스토리지 확장
  - 저장 및 전송 중 암호화 지원
  
- **Amazon Simple Storage Service(S3)**

  - 뛰어난 확장성, 데이터 가용성 및 보안과 성능을 제공하는 객체 스토리지 서비스
  - 높은 데이터 내구성
  - 비용 효율적인 스토리지 클래스
  - Static website content hosted
    - S3에 있는 파일을 인터넷에 배포하는 기능
    - 간단한 정적 웹사이트 호스팅은 S3에 하는 것이 좋다
    - 웹 서버의 구성과 관리에 대한 제어와 유연성이 필요한 엔터프라이즈 급 웹 호스팅은 Amazon EC2에 하는 것이 적절
    
### 컨테이너 기반 웹 애플리케이션
#### 1. AWS 오케스트레이션 툴
- **Amazon ECS**
Docker 컨테이너를 지원하는 컨테이너 관리 서비스
- **Amazon EKS**
쿠버네티스를 control plane 설치와 운영 필요 없이 사용할 수 있는 관리형 서비스

#### 2. 컴퓨팅 레이어
컨테이너를 구동시키기 위한 컴퓨팅 레이어. 
- **Amazon EC2**
- **Aws Fargate**
컨테이너에 적합한 서버리스 컴퓨팅 엔진

때에 따라서는 하나의 클러스터에 EC2, Fargate를 혼합하여 사용할 수도 있다.

#### 3. EKS 기반 웹 애플리케이션
![](https://images.velog.io/images/younge/post/827f26fd-2fd1-4052-b7c9-6ab93e36aeda/image.png)
- **Application Load Balancer**
사용자의 요청이 Route53, Cloudfront를 타고 Application Load Balancer로 들어온다. 해당 트래픽은 쿠버네티스 pod에 도달하여 요청을 처리한다.

- **EKS Worker Nodes**
EC2 기반 웹 애플리케이션에서의 EC2 인스턴스들과 같은 역할이다. 하나의 큰 EC2 웹서버가 서비스 단위로 쪼개지고 이것들이 pod들로 올라가는 것

- **Amazon DynamoDB**
데이터 티어 단은 규모에 상관없이 고성능을 제공하는 DynamoDB를 사용하는 경우가 많다.

- **Amazon Elastic Container Registry(ECR)**
컨테이너 레지스트리. 컨테이너 이미지를 관리한다.

- **Amazon CloudWatch**
애플리케이션 및 인프라를 모니터링하는 서비스. EKS를 구성하는 인프라 및 웹 애플리케이션을 구성하는 서비스들을 관리한다.

#### 4. EKS, ECR
- **Amazon Elastic Kubernetes Service(EKS)**
  - Kubernetes를 손쉽게 실행할 수 있는 관리형 서비스
  - 고가용성의 안전한 클러스터 제공
  - 패치 적용, 노드 프로비저닝 및 업데이트와 같은 주요 태스크 자동화 가능
  - Amazon ECR, ELB, AWS IAM< Amazon VPC와 같은 다양한 AWS 서비스들과 쉽게 연동 가능

- **Amazon Elastic Container Registry(ECR)**

  - 완전 관리형 컨테이너 레지스트리
  - 배포 워크플로우 간소화 - Amazon EKS, Amazon ECS, Amazon Lambda, AWS Fargate
  - 권한 제어 및 수명 주기 정책 규칙 설정
  - 이미지 취약성 스캐닝 기능 제공
  - 퍼블릭/프라이빗 레파지토리 및 퍼블릭 갤러리 기능
  - 교차 리전/교차 계정 복제

### 서버리스 웹 애플리케이션
- 서버리스는 서버가 없다는 것이 아니고, 서버에 대한 고민 없이 어플리케이션을 개발할 수 있다는 것이다.
- Lambda 및 Fargate와 같은 리소스에 국한된 것이 아니다.
- AWS 서버리스 서비스들은 관리형 서비스이다. 
- 따라서 스케일링을 위해 별도의 Rule을 관리하거나 모니터링할 필요가 없다. -> 서비스의 가용성과 장애 복구를 AWS가 관리한다는 의미
- 유휴 용량이 없어 비용 절감을 노릴 수 있고, 효율적으로 인프라를 사용할 수 있다.
- 컨테이너 보다 더 작은 함수로 관리되면서 각각 서비스들이 더 작은 단위로 쪼개진 상태

#### 1. 아키텍처
![](https://images.velog.io/images/younge/post/a106f367-aeb8-4eb1-b9ea-32acd94aff0f/image.png)
- **S3**
html, css, javascript, 이미지 같은 정적 컨텐츠를 S3에 업로드한다.

- **AWS Amplify**
Amazon CloudFront와 S3의 정적 웹 호스팅 역할을 대체할 수 있는 서비
스
- **Amazon API Gateway, AWS Lambda, Amazon DynamoDB**
동적 컨텐츠를 처리한다. 
- **AWS Cognito**
인증 관련 부분은 AWS Cognito를 사용한다.

#### 2. Lambda, DynamoDB

- **AWS Lambda**
  - 서버리스 컴퓨팅 서비스
  - 코드를 업로드하기만 하면 람다에서 높은 가용성으로 코드를 실행 및 확장하는 데 모든 것을 처리한다.
  - 이벤트에 대한 응답으로 코드를 실행 -> 비즈니스 로직, 다른 서비스들을 자동화하는 데에도 사용
  - 최대 10GB 메모리 -> 다중 스레드, 다중 프로세스 애플리케이션 더 빠르게 실행 가능
  - 컨테이너 이미지 지원
- **Amazon DynamoDB**
  - 관리형으로 제공하는 Key-Value NoSQL 데이터베이스 서비스
  - 어떤 규모에서든 빠른 10밀리초 미만의 응답 속도
  - 규모와 관계없이 일관된 성능
  - 용량에 맞게 테이블 자동 확장/축소
  - DynamoDB Stream 기능을 활성화하면 테이블 업데이트 작업에 대한 기록을 스트리밍으로 받을 수 있다.