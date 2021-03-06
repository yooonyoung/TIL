## ConfigMap, Secret

애플리케이션을 개발하거나 배포할 때, 개발환경(dev)이나 상용환경(production) 등 환경에 따라 컨테이너 안쪽에서 변화하는 값들을 관리할 필요가 있다. 하지만 이 환경 변수 때문에 별도의 이미지를 생성해 관리하는 것은 불편하기도 하고 비용적으로도 낭비가 크다.

그래서 외부에서 값들을 바꿀 수 있도록 환경 변수나 설정값들을 변수로 관리해서 `Pod`가 생성될 때 넣어줄 수 있는데, 이것이 바로 `ConfigMap`과 `Secret`이다.

`ConfigMap`과 `Secret`을 사용하는 방법은 크게 세 가지가 있다.

### 1. Env (Literal)

환경변수로 상수를 넣는 방법으로, 가장 기본적인 방법이다. 공통적으로 적용되는 `ConfigMap`과 `Secret`의 특징을 설명해보겠다.

`ConfigMap`은 `Key`와 `Value`로 구성되어있다. `ConfigMap`을 정의해놓으면 파드를 생성할때 가져와서 환경변수에 `Key=Value` 형식으로 세팅할 수 있다.

`Secret`이 `ConfigMap`과 다른 점은 인증 키와 같은 보안적인 요소를 저장할 때 사용한다는 것이다.
`Secret`은 `base64` 인코딩을 해서 넣어야 하는데, 이 부분이 보안적인 요소인 것은 아니고 단순 규칙으로 생각하면 된다. 실제로 파드에 가져올 때는 자동으로 디코딩이 되어 환경변수에 원래 값으로 들어가게 된다.

`Secret`은 파일이 아니라 메모리에 저장되므로 보안적으로 유리하다.
한 `Secret`당 최대 1M까지 저장할 수 있는데, 메모리의 특성 상 `Secret`을 너무 많이 저장하게 되면 메모리 사용량이 늘어나 `Out Of Memory`와 같은 이슈가 생길 수 있다. 따라서 보안적으로 꼭 필요한 정보만 `Secret`에 저장해야 한다.

`ConfigMap`과 `Secret`의 키/밸류는 모두 `String`으로, 예를 들어 `boolean` 값을 넣기 위해서는 `'false'`로 적어주어야 한다.

### 2. Env (File)

첫 번째 방법처럼 각각의 값을 공유하는 것이 아니라, 설정을 파일 형태로 파드에 공유하는 방법이다.

파일을 이용해서 `ConfigMap`을 만들 때는 아래와 같이 `--from-file`을 이용해 파일명을 넘겨주면 된다.
파일 이름이 `Key`가 되고 파일 안의 내용은 `Value`가 되어 `ConfigMap`이 만들어진다.

```null
$ kubectl create configmap cm-file --from-file=./file.txt
$ kubectl create secret generic sec-file --from-file=./file.txt 
```

`Secret`의 경우 파일의 내용이 `base64`로 인코딩되기 때문에 이미 `base64`로 되어있다면 두 번 인코딩하게 되므로 주의해야 한다.

파일로 만든 `ConfigMap`을 파드에 한 번 주입하면 파일 데이터가 바뀌어도 파드에 영향이 가지 않고, 파드가 재생성되어야지만 변경이 반영된다.

### 3. Volume Mount (File)

디스크 볼륨으로 `ConfigMap`파일을 마운팅하는 방법이다.
파일을 `ConfigMap`에 담는거까지 두 번째 방식과 동일하다.

파드를 만들 때 컨테이너 안에 `Mount Path`를 적어주고 `ConfigMap`파일을 넣을 수 있다.

```null
mountPath: /mount
```

원본과 직접 연결해주는 방법이기 때문에 `ConfigMap` 파일의 내용이 바뀌게 되면 파드 컨테이너의 마운트 내용도 바뀌게 되므로 변경사항이 바로 반영된다.