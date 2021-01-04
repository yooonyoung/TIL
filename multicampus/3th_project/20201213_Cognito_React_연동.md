<h2>AWS Cognito와 Amplify로 로그인 기능 구현하기 (React)</h2>

React로 관리자 페이지를 구현하면서, 관리자 로그인 기능을 위해 AWS Cognito를 사용하기로 결정했다. 생각보다 간단했던 과정! Cognito 사용자 풀을 생성한 후 React에서 Amplify UI로 로그인 기능을 쉽게 구현해볼 것이다.

><h4>Cognito 사용자 풀 생성하기</h4>

<b>AWS Cognito 사용자 풀 관리 - 생성</b>

![이미지 3](https://user-images.githubusercontent.com/30336831/102007539-4a2b1880-3d6d-11eb-995d-b6cf76513187.png)

왼쪽 탭에 <b>속성</b> 클릭

![이미지 4](https://user-images.githubusercontent.com/30336831/102007627-0edd1980-3d6e-11eb-9553-dc722a8fb4bf.png)

이메일과 아이디로 로그인할 수 있도록 다음과 같이 설정!

![이미지 5](https://user-images.githubusercontent.com/30336831/102007724-dc7fec00-3d6e-11eb-8ef7-19fb6f318507.png)

모두 기본값으로 넘어가고 아래 페이지가 나오면 <b>앱 클라이언트 추가</b>

![이미지 6](https://user-images.githubusercontent.com/30336831/102007750-12bd6b80-3d6f-11eb-9314-2c3af9860d67.png)

<b>클라이언트 보안키 생성</b> 체크 해제

![이미지 7](https://user-images.githubusercontent.com/30336831/102007784-47c9be00-3d6f-11eb-9cd0-90e4d4d01ba5.png)

그대로 앱 클라이언트를 생성해주고 다음 단계로 넘어간다.

![이미지 8](https://user-images.githubusercontent.com/30336831/102007816-7778c600-3d6f-11eb-96ba-6f8bc4d01b69.png)

<b>풀 생성</b>!

![이미지 9](https://user-images.githubusercontent.com/30336831/102007838-a131ed00-3d6f-11eb-8eb2-ee407ed55765.png)

리액트와 연동하기 위해서는 사용자 풀 생성 후 나온 풀 ID와

![이미지 10](https://user-images.githubusercontent.com/30336831/102007913-4220a800-3d70-11eb-9652-fd219be1a48a.png)

<b>앱 클라이언트</b> 탭에서 볼 수 있는 앱 클라이언트 ID가 필요하다.

![이미지 11](https://user-images.githubusercontent.com/30336831/102007951-88760700-3d70-11eb-9bc9-de4e22f81361.png)

나는 관리자 페이지이기 때문에 사용자를 하나만 생성해줄 것이다.

<b>사용자 및 그룹 - 사용자 생성</b>

![이미지 14](https://user-images.githubusercontent.com/30336831/102008388-48645380-3d73-11eb-8e51-07bb01b0fbc7.png)

원하는 값을 적어주고 사용자를 생성한다.

![이미지 16](https://user-images.githubusercontent.com/30336831/102008435-bc9ef700-3d73-11eb-9cab-c90e3b3e9190.png)



> <h4>Amplify로 로그인 페이지 구현하기</h4>

리액트에서 Amplify UI를 사용하려면 다음과 같은 디펜던시 설치가 필요하다. Amplify가 제공하는 리액트 컴포넌트와 Amplify를 설치해준다.

```
$ npm install aws-amplify @aws-amplify/ui-react
$ npm install aws-amplify
```

원하는 위치에 awsconfig.js 파일을 생성해 아까 필요하다고 했던 풀 ID와 앱 클라이언트 ID를 다음과 같이 적어준다.

```
// awsconfig.js

export default {
  Auth: {
    region: "YOUR-REGION",
    userPoolId: "YOUR-POOL-ID",
    userPoolWebClientId: "YOUR-CLIENT-ID"
  }
}
```

로그인을 적용해보자.

```
import React from "react";
import Amplify from "aws-amplify";
import { AmplifyAuthenticator } from "@aws-amplify/ui-react";
import awsconfig from "./awsconfig";

Amplify.configure(awsconfig);

export default () => {
  return (
    <AmplifyAuthenticator>
      <div>
        ONLY LOGGED IN USERS CAN SEE THIS
      </div>
    </AmplifyAuthenticator>
  );
};
```

AmplifyAuthenticator 태그 안에 있는 컴포넌트는 로그인한 사람만 볼 수 있도록 해준다. 

나는 로그인을 한 경우에만 관리자 대시보드를 볼 수 있도록 하고 싶었기 때문에 Admin 파일의 렌더링을 AmplifyAuthenticator로 감싸주었다.

이렇게만 하면 예쁜 UI로 로그인 페이지가 구현된다.

![이미지 12](https://user-images.githubusercontent.com/30336831/102008247-9036ab00-3d72-11eb-974b-611e062b0981.png)

나는 Username 로그인이 아니라 이메일 로그인이고, 관리자이므로 따로 회원가입을 할 수 없도록 하고 싶었기 때문에 약간의 커스터마이징을 진행했다. -> 나중에 username 로그인으로 변경했음

```
<AmplifyAuthenticator>
        <AmplifySignIn
          slot="sign-in"
          headerText="KF99 Dashboard Sign In"
          usernameAlias="email"
          style={{
            display: "flex",
            justifyContent: "center",
          }}
        >
          <div slot="secondary-footer-content"></div>
        </AmplifySignIn>
        ...
        Admin Dashboard
        ...
</AmplifyAuthenticator>
```

또 버튼 색상이 맘에 들지 않아 css도 바꿔주었다.

```
:root {
    --amplify-primary-color: #6A7CA2;
    --amplify-primary-tint: #7d8fb6;
    --amplify-primary-shade: #55627e;
}
```

![이미지 13](https://user-images.githubusercontent.com/30336831/102008338-fb807d00-3d72-11eb-945e-07836221f915.png)

짠!

하지만 이렇게 속성값만 바꿔서는 템플릿을 통째로 바꾸거나 버튼 모양 등을 변경할 수 없어서 조금 더 복잡한 방법으로 전체 템플릿을 수정해야 할 것 같다. 방법 찾는 중!