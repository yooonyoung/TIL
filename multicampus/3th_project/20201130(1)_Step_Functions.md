<h2>AWS Step Functions</h2>

> AWS Step Functions은 시각적 워크플로우를 사용해 분산 애플리케이션 및 마이크로서비스의 구성 요소를 손쉽게 조정하도록 해주는 웹 서비스이다. 각각 기능 또는 작업을 수행하는 개별 구성 요소를 사용하여 애플리케이션을 구축하면 애플리케이션을 빠르게 확장하거나 변경할 수 있다.

쉽게 말하면 AWS의 여러 컴퓨팅 자원들의 수행 순서를 설정할 수 있는 서비스이다. 이번 프로젝트에서는 하나의 람다에서 모든 작업을 처리하지 않고, 람다를 기능별로 분산시켜 순차적으로 실행되도록 하기 위해 사용할 예정이다.

> AWS Step Functions는 각 단계가 이전 단계의 출력으로 입력되는 단계로 구성된 워크 플로우를 설계하고 실행할 수 있도록 함으로써, 작업을 보다 쉽게 조정할 수 있게 하는 완전 관리형 서비스다.

<h4>State</h4>

각 단계는 State라 불린다. 이들은 각자의 Type으로 어떤 명령을 수행하는지 나타낸다.

- **Task** : 하나의 수행되는 작업을 의미한다. Lambda로 수행되는 작업일 수도 있고, Batch로 수행되는 작업일 수도 있다.
- **Choice** : 다음에 어떤 State로 넘어갈 지 분기를 설정할 수 있다. Inputpath의 값에 따라 서로 다른 로직을 수행해야 할 때 사용된다.
- **Pararell** : Branch들을 병렬적으로 실행할 때 사용된다. Branch에 두 State가 있다면, 이 State를 서로 독립적으로 병렬적으로 수행한다는 뜻이다.
- **Wait** : "Seconds"에 지정된 초만큼 기다린다(딜레이된다).
- **Fail** : 작업을 멈추고 이 작업을 실패로 표시한다.
- **Succeed** : 작업을 멈추고 이 작업을 성공으로 표시한다.
- **Pass** : 입력된 데이터를 그대로 출력한다.

<h4>실제 사용 예제</h4>

(여기서는 순차적으로 실행되도록 구성했지만, Choice State를 이용해 병렬 구성도 가능하다.)

```
import json

def lambda_handler(event, context): 
    if (event["name"] == "PSY"):
        return {"song" : "gangnam style"}

    else:
        return {"song" : "i dont know"}
```

```
import json

def lambda_handler(event, context):
    if (event["song"] == "gangnam style"):
        return {"lyrics" : "oh oh oh oh oh pan gangnam style"}
    else:
        return {"lyrics" : "i don't know"}
```

두 람다 함수를 생성하고, ARN을 얻을 수 있다.

AWS의 Step Functions 콘솔로 들어가서 상태 머신을 생성한다.

![이미지 4](https://user-images.githubusercontent.com/30336831/100574493-7c745900-331d-11eb-907c-77690b49a6c3.png)

```
{
  "Comment": "A Hello World example of the Amazon States Language using Pass states",
  "StartAt": "song",
  "States": {
    "song": {
      "Type": "Task",
      "Resource": "arn:aws:lambda:us-east-1:425701337758:function:stepTest1",
      "Next": "lyrics"
    },
    "lyrics": {
      "Type": "Task",
      "Resource": "arn:aws:lambda:us-east-1:425701337758:function:stepTest2",
      "End": true
    }
  }
}
```

stepTest1 람다 함수를 먼저 호출하고, 그 다음 stepTest2 람다 함수를 호출하는 step function 코드다. 내부적으로 입력값에 따라 다른 결과를 출력하게 될 것이다.

`{"name": "PSY"}`를 입력으로 주고 실행시켰을 때

![이미지 5](https://user-images.githubusercontent.com/30336831/100574573-aaf23400-331d-11eb-9cb8-e9cba95deaad.png)

다음과 같은 출력 결과가 나오면 성공이다.

이번 프로젝트에서는 S3에 이미지를 업로드했을 때 CloudTrail과 CloudWatch를 사용해 트리거를 등록, Step Functions이 호출되도록 구현해볼 예정이다.



위 예제 출처 : https://velog.io/@daeyoon/AWS-step-function

