## AWS에서 DevOps 시작하기

#### DevOps란?

- 소프트웨어 개발과 정보 기술 운영을 결합한 문화 철학, 사례 및 도구의 조합

- 빠르게 피드백을 바탕으로 개선할 수 있게 만드는 새로운 개발 접근 방법

- 기업은 새 애플리케이션 기능과 개선된 서비스를 더욱 빠른 속도로 고객에게 전달할 수 있다

- 혁신적인 DevOps를 위해서는

  오퍼레이션 모델 -> 아키텍처 패턴 -> 소프트웨어 배포 -> 오퍼레이션 모델.. 형식으로 모든 단계가 서로 아주 긴밀히 연관되어 같이 발전을 이루어야 한다.



#### 작은 단위로 서비스를 나누는 원칙
- 유닛을 가능한 한 작게 (Primitives)
- 데이터 도메인 생성
- 스케일링(확장) 단위 기반으로 분리 (기능이나 컴포넌트 단위로 나누는 것보다는 스케일 단위로 나누는 것이 좋다) 
- 각각의 서비스를 별도의 운영
- APIs (contracts)를 사용한 서비스간 통신

만약 대규모 개발 환경에서, 우리 개발 팀이 다른 서비스에 미치는 영향을 고려해서 개발해야 한다면 DevOps가 아닐 수 있다.



#### 투-피자팀 조직
- 12명-15명의 팀을 말한다.

- 서비스를 직접 운영 (개발, 보안, 운영, 버그 테스팅, 서비스 문서화, 배포/전개, 운영 등을 모두 DevOps 팀이 한다) 

- 즉, 조직을 재구성해야 함.

- 다양한 일들을 그 팀이 다 해야하기 때문에 자동화되어있는 프로세스를 따라서 하지 않으면 다 감당할 수 밖에 없게 된다.

- 사회적 제약 최소화 (Conway의 법칙)

- 결정을 내리는 자율성

- 개발스테이지에서 모두 자동화해서 빠르게 개발하고 검증, 테스팅 단계까지 자동화되어 프로덕션 레벨에서의 검증과 데이터를 뽑아내는 것이 중요

  
  
#### DevOps 파이프라인

- 개발단계

  - CodeCommit, Developer workstation, Developer AWS account, Code review..

  - 개발과 배포, 테스트에서 빠른 속도가 중요

- 중간단계

  - 철저한 검증이 가장 중요

  - 디펜던시 체크, 패키징, 빌드 등을 위한 검증 도구 필요

- 프로덕션

  실제 프로덕션 레벨에서 사용 가능한지 테스트. 아래의 세 단계를 수행 -> 성공 시에만 승격, 테스트 실패 시 롤백, 베이킹 실패 시 중지

  - 베타
    - 환경만 분리하여 가혹한 테스트. 파괴적 또는 기타 테스트 실행

  - 감마
    - ''프로덕션''과 같은 환경에서 테스트. 완전한 엔드 투 엔드 통합 테스트 실행

  - 프로덕션
    - 웨이브로 배포하고 각 웨이브 시간을 '베이킹'할 시간을 준다



#### CI/CD를 위한 AWS 개발자 도구

- 소스 (AWS CodeCommit) -> 빌드 (AWS CodeBuild) -> 테스트 (AWS CodeBuild + Third Party) -> 배포 (AWS CodeDeploy) -> 모니터 (AWS X-Ray, Amazon CloudWatch)
- 이 전체 파이프라인을 AWS CodePipeline을 통해 모니터링하고 관리할 수 있다.

#### 소프트웨어 딜리버리의 발전
  >- 많은 서비스에 대한 릴리스 프로세스를 어떻게 관리합니까?
  - 모범 사례를 어떻게 만들고 적용합니까?
  - Lambda 애플리케이션을 작성하고 디버깅하려면 어떻게 해야 합니까?
  - 분산 아키텍처에서 임시 리소스를 모니터링하려면 어떻게 해야 합니까?

- AWS Cloud Development Kit
  - 한 눈에 보이고 통제하고 관리할 수 있는 방법
  - 프레임워크를 사용하듯이 이 키트를 이용해서 데브옵스를 구현할 수 있다.
  - Infrastructure as a Code로 배포까지도 관리할 수 있다.

  IaC를 이용해서 데브옵스를 간단하게 할 수 있다. 블루-그린 배포도 굉장히 쉬워진다?



#### 대규모 DevOps 운영 과제

- DevOps 파이프라인에서 발생하는 데이터 볼륨이 크고 단계별로 서로 이질적인 패턴을 가짐
- 문제가 발생하면 사용자는 데이터 소스와 도구를 수동으로 상호 연결하는 데 많은 시간과 노력을 소비하게 됨
- 새로운 서비스가 채택됨에 따라 새로운 경보 및 모니터링 절차를 구성하려면 심층적인 DevOps / CloudOps 전문 지식이 필요
- 여러 도구의 경보 및 알림으로 인해 경보 피로가 발생하고 가장 중요한 문제를 식별하지 못함

#### DevOps Guru

- 애플리케이션 가용성을 향상시키는 ML 기반 클라우드 운영 서비스
- 머신러닝 기반 서비스로, 개발자와 운영자가 문제를 자동으로 감지하여 애플리케이션 가용성을 개선, 다운타임을 줄일 수 있음

- SageMaker, 쿠버네티스의 쿠버플로우 등을 데브옵스 레벨로 전개시키는 것보다 훨씬 손쉽게 할 수 있다.
- 데브옵스 파이프라인에 집어넣을 수 있고 운영 문제를 자동으로 감지해준다.
- 어플리케이션을 개선하고 빌드하는 데만 집중하면 되고, 데브옵스 오퍼레이션 자체를 향상시키는 접근은 데브옵스 그루를 이용해서 할 수 있다.