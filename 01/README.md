# 실전 리액트 프로그래밍

## 1장 리액트 프로젝트 시작하기

### 1.1 리액트란 무엇인가

리액트는 페이스북에서 개발하고 관리하는 UI 라이브러리다.

리액트 팀에서는 리액트의 진입 장벽을 낮추기 위해 create-react-app . 을 만들었다.

리액트와 같은 프론트엔드 라이브러리 혹은 프레임워크를 사용하는 이유는 무엇일까? 

→ UI를 자동으로 업데이트해 준다는 점 → UI = render(state)



리액트의 장점 

1. 가상 돔(virtual dom)을 통해서 UI를 자동으로 업데이트한다.
   - 가상 돔 덕분에 불필요한 UI 업데이트는 줄고, 성능은 좋아진다.
2. 리액트는 함수형 프로그래밍을 적극적으로 활용한다는 특징이 있음
   - render 함수는 순수 함수로 작성한다.
     - render 함수는 순수 함수여야 하므로 인수 state가 변하지 않으면 항상 같은 값을 반환해야 함
   - state은 불변 함수로 관리한다.
   - 컴포넌트의 상태값을 수정할 때는 기존 값을 변경하는 게 아니라 새로운 객체를 생성해야 한다.
   - 이 두 조건을 따르면 렌더링 성능을 크게 향상 시킬 수 있다.

### 1.2 리액트 개발 환경 직접 구축하기

하나의 웹 애플리케이션을 만들기 위해서는 테스트 시스템, 빌드 시스템, 라우팅 시스템 등 UI 외에도 신경 써야 할 부분이 많다.

#### 1.2.1 Hello World 페이지 만들기

리액트로 웹 애플리케이션을 제작할 때는 다양한 외부 패키지를 활영하는게 일반적이다.

1. https://unpkg.com/react@16/umd/react.development.js

2. https://unpkg.com/react@16/umd/react.production.min.js

3. https://unpkg.com/react-dom@16/umd/react-dom.development.js

4. https://unpkg.com/react-dom@16/umd/react-dom.production.min.js

1, 3은 개발 환경에서 사용되는 파일 : 개발시 도움이 되는 에러 메시지를 확인할 수 있음

2, 4는 배포 환경에서 사용되는 파일  



1, 2 파일은 플랫폼 구분 없이 공통으로 사용되는 리액트의 핵심 기능 따라서 리액트 네이티브에서도 사용된다.

3, 4는 웹에서만 사용되는 파일이다.



**1.2.1 **은 여기 까지만 설명함 그냥 create-react-app . 으로 만드는게 편함

#### createElement 이해하기

createElement 함수의 구조는 다음과 같음

React.createElement(component, props, ...children) => ReactElement

- 첫 번째 매개변수 component는 일반적으로 문자열이나 리액트 컴포넌트다. component의 인수가 문자열이면 HTML 태그에 해당하는 돔 요소가 생성됨 ex) 문자열 p를 입력하면 HTML p 태그가 생성된다.
- 두 번째 매개변수 props는 컴포넌트가 사용하는 데이터를 나타낸다. 돔 요소의 경우 style, className 등의 데이터가 사용될 수 있다.
- 세 번째 매개변수 children은 해당 컴포넌트가 감싸고 있는 내부의 컴포넌트를 가리킨다. div 태그가 두 개의 p 태그를 감싸고 있는 경우에 다음과 같이 작성할 수 있음

```html
<div>
  <p>hello</p>
  <p>world</p>  
</div>
1.  일반적인 HTML 코드다.


createElement(
  'div',
  null,
  createElement('p', null, 'hello'),
  createElement('p', null, 'world'),
)
2. createElement 함수를 사용해서 작성했다. 대부분의 리액트 개발자는 createElement를 직접 작성하지 않는다. 일반적으로 바벨(babel)의 도움을 받아서 JSX 문법을 사용한다. createElement 함수보다는 JSX 문법으로 작성하는 리액트 코드가 훨씬 가독성이 좋기 때문이다. 바벨을 통한 JSX 문법의 사용은 잠시 후 설명한다.
```



**여러 개의 돔 요소에 렌더링하기**

리액트가 돔 요소의 한 곳에만 렌더링 할 수 있는 것은 아니다. 

**코드 1-4** simple.html

```html
<html>
  <body>
    <h2>안녕하세여. 이 프로젝트가 마음에 드시면 좋아요 버튼을 눌러 주세요.</h2>
    <div id="react-root1"></div>  ===============
    <!-- ... -->                                |
    <div id="react-root2"></div>                | 1
    <!-- ... -->                                |
    <div id="react-root3"></div>  ===============  
    <script src="react.development.js"></script>
    <script src="react-dom.development.js"></script>
    <script src="simple2.js"></script>    2
  </body>  
</html>
```

1. 기존의 react-root 돔 요소를 지우고 세 개의 돔 요소를 만듦

   - 각 요소 사이에 다른 코드가 있다는 사실에 주목하자. 다른 코드가 없다면 HTML 에서는 하나의 요소만 만들고 리액트 코드에서 여러 개의 버튼을 구성하는 게 낫다.

2. 새로 만들 자바스크립트 파일 이름으로 변경했다.

   

**코드 1-5** simple2.js

```HTML
// ... 1
ReactDOM.render(
  React.createElement(LikeButton),
  document.querySelector('#react-root1'),
);
ReactDOM.render(
  React.createElement(LikeButton),
  document.querySelector('#react-root2'),
);
ReactDOM.render(
  React.createElement(LikeButton),
  document.querySelector('#react-root3'),
);
```

1. LikeButton 컴포넌트는 수정하지 않는다. 미리 만들어 놓은 세 개의 돔 요소에 LikeButton 컴포넌트를 렌더링한다. simple2.html 파일을 브라우저에서 열어보면 세 개의 **좋아요** 버튼을 확인할 수 있다.

#### 1.2.2 바벨 사용해 보기

**바벨(babel)**은 자바스크립트 코드를 변환해 주는 컴파일러다. 바벨을 사용하면 최신 자바스크립트 문법을 지원하지 않는 환경에서도 최신 문법을 사용할 수 있다.

코드에서 주석을 제거하거나 코드를 압축하는 용도로 사용될 수 있다.

리액트에서는 JSX 문법을 사용하기 위해 바벨을 사용한다. 바벨이 JSX 문법으로 작성된 코드를 createElement 함수를 호출하는 코드로 변환해준다.

```
JSX 문법 알아보기
JSX는 HTML 에서 태그를 사용하는 방식과 유사하다. createElement 함수를 사용해서 작성하는 것보다는 JSX 문법을 사용하는 게 간결하고 가독성도 좋다. HTML 태그와의 가장 큰 차이는 속성값을 작성하는 방법에 있다.

1. HTML에서 돔 요소에 CSS 클래스 이름을 부여할 때 class 키워드를 사용했다면, JSX에서는 className 키워드를 사용한다. 이는 class라는 이름이 자바스크립트의 class 키워드와 같기 때문이다.
2. Title은 리액트 컴포넌트다. JSX에서는 돔 요소와 리액트 컴포넌트를 같이 사용할 수 있다. Title 컴포넌트는 text, width라는 두 개의 속성값을 입력받는다. width처럼 문자열 리터럴이 아닌 속성값은 중괄호를 사용해서 입력한다. Title 컴포넌트는 두 개의 속성값을 이용해서 어떤 돔 요소로 치환될지 결정한다.
3. 이벤트 처리 함수는 브라우저마다 다르게 동작할 수 있기 때문에 리액트와 같은 라이브러리를 사용하지 않을 때는 주의해야 한다. 다행히 리액트에서는 이벤트 처리 함수를 호출할 때 브라우저에 상관없이 통일된 이벤트 객체를 전달해 준다.
4. HTML에서 돔 요소에 직접 스타일을 적용하는 것과 같이 JSX에서도 스타일을 적용할 수 있다. 다만 자바스크립트에서는 속성 이름에 대시(-)로 연결되는 이름을 사용하기 힘들기 때문에 카멜 케이스(camelCase)를 이용한다.
```

JSX 문법은 자바스크립트 표준이 아니기 때문에 그대로 실행하면 에러가 발생한다. 바벨을 이용해서 JSX 문법으로 작성된 파일을 createElement 함수로 작성된 파일로 변환해야 한다.

파일을 변환하기 위해서는 다음 패키지를 설치해야 한다.

**npm install @babel/core @babel/cli @babel/preset-react**

@babel/cli에는 커맨드 라인에서 바벨을 실행할 수 있는 바이너리 파일이 들어 있다.

@babel/preset-react에는 JSX로 작성된 코드를 createElement 함수를 이용한 코드로 변환해 주는 바벨 플러그인이 들어 있다.

```
바벨 플러그인과 프리셋
바벨은 자바스크립트 파일을 입력으로 받아서 또 다른 자바스크립트 파일을 출력으로 준다. 이렇게 자바스크립트 파일을 변환해 주는 작업은 플러그인 단위로 이루어진다. 두 번의 변환이 필요하다면 두 개의 플러그인을 사용한다. 하나의 목적을 위해 여러개의 플러그인이 필요할 수 있는데, 이러한 플러그인의 집합을 프리셋이라고 한다.
예를 들어, 바벨에서는 자바스크립트 코드를 압축하는 플러그인을 모아 놓은 babel-preset-minify 프리셋을 제공한다. @babel/preset-react는 리액트 애플리케이션을 만들 때 필요한 플러그인을 모아 놓은 프리셋이다.
```

설치된 패키지를 이용해서 자바스크립트 파일을 변환해 보자

**npx babel --watch src --out-dir, --preset @babel/preset-react**

npx 명령어는 외부 패키지에 포함된 실행 파일을 실행할 때 사용된다. 

외부 패키지의 실행 파일은 **./node_module/.bin** 밑에 저장된다. 따라서 npx babel은 **./node_module/.bin/babel**을 입력하는 것과 비슷하다. 오래된 npm 버전에서는 npx 명령어가 동작하지 않으므로, 최신 버전의 npm을 설치하거나 **./node_module/.bin/babel**을 입력하자

위 명령어를 실행하면 src 폴더에 있는 모든 자바스크립트 파일을 @babel/preset-react 프리셋을 이용해서 변환 후 현재 폴더에 같은 이름의 자바스크립트 파일을 수정할 때마다 자동으로 변환 후 저장한다.



#### 1.2.3 웹팩의 기본 개념 이해하기

**웹팩(webpack)**은 자바스크립트로 만든 프로그램을 배포하기 좋은 형태로 묶어 주는 도구이다. 이전에는 자바스크립트의 파일이 적었기에 상관 없었지만 지금의 웹 사이트의 페이지는 한 페이지에도 자바스크립트 파일이 수십 또는 수백 개 필요했기 때문에 더는 기존 방식이 통하지 않는다.

기존의 방식으로는 계속 늘어나는 자바스크립트 파일을 관리하기가 힘들다. 파일 간의 의존성 때문에 선언되는 순서를 신경 써야 하기 때문이다. 그리고 뒤에 선언된 자바스크립트 파일이 앞에 선언된 파일에서 생성한 전역 변수를 덮어쓰는 위험도 존재한다.

```
자바스크립트의 모듈 시스템
자바스크립트에는 ES6부터 모듈 시스템이 언어 차원에서 지원된다. 현재 모든 최신 브라우저에서는 ES6의 모듈 시스템을 지원한다. 하지만 예전 버전의 브라우저에서는 모듈 시스템을 사용할 수 없다. 또한 상당히 많은 수의 오픈 소스가 ES6 모듈로 작성되지 않았다는 것도 큰 걸림돌이다.
ES6가 나오기 이전부터 자바스크립트의 모듈 시스템을 요구하는 개발자가 많았다. 그래서 등장한 대표적인 자바스크립트 모듈 시스템이 commonJS이다. node.js가 commonJS 표준을 따르면서 commonJS가 널리 퍼지기 시작했다. 현재 많은 수의 오픈소스가 commonJS 모듈 시스템으로 구현되어 있다.
```

웹팩은 ESM(ES6의 모듈 시스템)과 commonJS를 모두 지원한다. 이들 모듈 시스템을 이용해서 코드를 작성하고 웹팩을 실행하면 예전 버전의 브라우저에서도 동작하는 자바스크립트 코드를 만들 수 있다. 웹팩을 실행하면 보통은 하나의 자바스크립트 파일이 만들어지는데, 원한다면 여러 개의 파일로 분할할 수도 있다. 우리가 할 일은 웹팩이 만들어 준 자바스크립트 파일을 HTML의 script 태그에 포함시키는 것이다.



**ESM 문법 익히기**
ESM 문법을 익히기 위해 모듈을 내보내고 가져오는 코드를 작성해 보자. 다음 코드는 세 파일의 내용을 보여 준다. file1.js 파일은 코드를 내보내는 쪽이고 file2.js, file3.js 파일은 코드를 사용하는 쪽이다.

```
// file1.js 파일
export default function func1() {} 1
export function func2() {}         2
export const variable1 =123;       2
export let variable2 = 'hello'     2

// file2.js 파일
import myFunc1, { func2, variable1, variable2} from './file1.js'; 3

// file3.js 파일
import { func2 as myFunc2 } from './file1.js'; 4

1,2 코드를 내보낼 때는 export 키워드를 사용한다.
3 코드를 사용하는 쪽에서는 import, from 키워드를 사용한다.
1 default 키워드는 한 파일에서 한 번만 사용할 수 있다.
3 default 키워드로 내보내진 코드는 괄호 없이 가져올 수 있고, 이름은 원하는 대로 정할 수 있다.
1번 코드에서 내보낸 func1 함수는 3번 코드에서 myFunc1이라는 이름으로 가져왔다.
3 default 키워드 없이 내보내진 코드는 괄호를 사용해서 가져온다.가져올 때는 내보낼 때 사용된 이름 그대로 가져와야 한다.
4 원한다면 as 키워드를 이용해서 이름을 변경해서 사용할 수 있다.
```



#### 1.2.4 웹팩 사용해 보기

웹팩을 사용해서 리액트의 두 파일을 자바스크립트의 모듈 시스템으로 포함 시켜 보자. webpack-test라는 폴더를 만들고 그 폴더에서 다음 명령어를 실행한다.

**npm init -y**

명령어를 실행하면 package.json 파일이 만들어진다. simple1.html 파일을 복사해서 webpack-test 폴더 밑에 index.html 파일을 만들고, index.html에 있는 simple1.js 문자열을 dist/main.js로 변경하자. 그 다음 react.development.js, react-dom.development.js 파일을 포함하고 있는 script 태그를 지운다. 이 두 리액트 파일은 모듈 시스템을 이용해서 main.js 파일에 포함될 예정이다. webpack-test 폴더 밑에 src 폴더를 만들자. src 폴더 밑에 내용이 없는 index.js, Button.js 파일을 만든다.

이제 필요한 외부 패키지를 설치해 보자.

**npm install webpack webpack-cli react react-dom**

웹팩과 함께 리액트 패키지도 설치했다. 

~~(아 그냥 create-react-app으로 하는게 편하겠다. 구조만 알고 바벨과 웹팩의 용도만 알면 될 듯 싶다.)~~

### 1.3 create-react-app . 으로 시작하기

create-react-app을 이용하면 기존 기능을 개선하거나 새로운 기능을 추가했을 대 피키지 버전만 올리면 된다. 현재 자바스크립트 생태계는 춘추전국 시대라 할 만하다. 지금 이순간에도 새로운 패키지가 엄청나게 쏟아져 나온다. 기존에 가장 좋은 방법이라 여겨졌던 것이 새로운 방법으로 대체되는 경우도 있다. 그만큼 우리에게는 하나의 문제를 해결하기 위한 다양한 선택지가 있고, 이들 가운데 하나를 선택해야 하는 상황을 자주 맞딱뜨리게 된다. 여러 선택지 가운데 하나를 고르기 위해서는 각각의 장단점을 공부해야 하므로 시간이 많이 든다. create-react-app을 사용하면 이 시간을 좀 더 아낄 수 있다.



#### 1.3.1 create-react-app 사용해 보기

**npx create-react-app cra-test** 

create-react-app 패키지가 설치되어 있지 않더라도 npx가 자동으로 가져와서 실행한다. 만약 npm 버전이 낮아서 실행이 안 된다면 다음과 같이 입력한다.

**npm install -g create-react-app**

**create-react-app cra-test**

실행 후에는 cra-test 폴더가 생성되고 그 안에 몇 개의 폴더와 파일이 들어 있다. 거두절미하고 웹 페이지를 띄워 보자

**cd cra--test**

**npm start**

빌드가 끝나면 자동으로 브라우저에서 새 탭이 열리고 렌더링된 페이지를 볼 수 있다. 이렇게 자동으로 브라우저를 띄워 주는 소소한 기능이 create-react-app의 매력 포인트 중 하나다.

이 상태로 App.js 파일에서 Learn React 부분을 다른 텍스트로 수정해 보자. 브라우저의 화면이 자동으로 업데이트되는 것을 확인할 수 있다. 이번에는 App.css 파일에서 .App-link의 색상을 red로 변경해 보자. 마찬가지로 변경된 색상을 바로 확인할 수 있다. 이는 **HMR**이라는 이름의 기능 던분인데, npm start 실행 시 create-react-app이 로컬 서버를 띄워 주기 때문에 가능한 일이다. **참고로 npm start는 개발 모드에서 동작하므로 배포할 때 사용하면 안 된다.**

create-react-app에서 자동으로 생성한 파일을 살펴보자. 다음은 자동으로 생성된 cra-test 폴더의 내부 구조이다.

![image-20200922211858743](C:\Users\USER\AppData\Roaming\Typora\typora-user-images\image-20200922211858743.png)

index.html, indes.js 파일은 빌드 시 예약된 파일 이름이므로 지우면 안 된다. index.html, index.js, package.json 파일을 제외한 나머지 파일은 데모 앱을 위한 파일이기 때문에 마음대로 수정하거나 삭제해도 괜찮다. **index.js로 부터 연결된 모든 자바스크립트 파일과 CSS 파일은 src 폴더 밑에 있어야 한다.** src 폴더 바깥에 있는 파일을 import 키워드를 이용해서 가져오려고 하면 실패한다.

index.html에서 참조하는 파일은 public 폴더 밑에 있어야 한다. public 폴더 밑에 있는 자바스크립트 파일이나 CSS 파일을 link나 script 태그를 이용해서 index.html에 포함시킬 수 있다. 하지만 특별한 이유가 없다면 index.html에 직접 연결하는 것보다는 src 폴더 밑에서 import 키워드를 사용해서 포함시키는 게 좋다. 그래야 자바스크립트 파일이나 CSS 파일의 경우 빌드 시 자동으로 압축된다.

이미지 파일이나 폰트 파일도 마찬가지로 src 폴더 밑에서 import 키워드를 사용해서 포함시키는게 좋다. 웹팩에서 해시값을 이용해서 url을 생성해 주기 때문에 파일의 내용이 변경되지 않으면 브라우저 캐싱 효과를 볼 수 있다. 파일을 참조하는 경우 외에도 index.html의 내용을 직접 수정해도 괜찮다. 대표적으로 title 태그에서 제목을 직접 입력하는 경우를 예로 들 수 있다. 그런데 제목을 페이지별로 다르게 줘야 한다면 문제가 될 수 있다. 만약 사내에서만 쓰는 웹사이트라면 react-helmet과 같은 패키지를 사용하면 된다. 반대로 일반 사용자를 대상으로 하는 웹사이트라서 검색 엔진 최적화를 해야 한다면 문제는 복잡해진다. **검색 엔진 최적화가 중요하다면** create-react-app 보다는 서버사이드 렌더링에 특화된 넥스트(next.js)를 사용하는게 좋다.

serviceWorker.js 파일에는 PWA와 관련된 코드가 들어 있다. PWA는 오프라인에서도 잘 동작하는 웹 애플리케이션을 만들기 위한 기술이다. create-react-app으로 프로젝트를 생성하면 PWA 기능은 기본적으로 꺼져 있는 상태다. PWA 기능을 원한다면 index.js 파일에  **serviceWorker.register();** 코드를 넣으면 된다.



#### 1.3.2 주요 명령어 알아보기

pacakage.json 파일을 열어 보면 네 가지 npm 스크립트 명령어를 확인할 수 있다.

**개발 모드로 실행하기**

npm start는 개발 모드로 프로그램을 실행하는 명령어이다. 개발 모드로 실행하면 HMR이 동작하기 때문에 코드를 수정하면 화면에 즉시 반영된다. **HMR**이 없다면 코드를 수정하고브라우저에서 수동으로 새로고침을 해야 하므로 번거롭다. 개발모드에서 코드에 에러가 있을 때는 브라우저에 에러 메시지가 출력된다.

에러 메시지가 출력된 영역을 클릭하면 에러가 발생한 파일이 에디터에서 열린다. 이는 create-react-app의 섬세함을 느낄 수 있는 기능이다.

때에 따라 API 호출을 위해서 https로 실행해야 할 수도 있다. https 환경을 직접 구축하란 여간 귀찮은 일이 아닌데, 고맙게도 create-react-app에서는 https로 실행하는 옵션을 제공한다.

- 맥: HTTPS=true npm start
- 윈도우: HTTPS=true && npm start

이 명령어를 실행하면 자체 서명된 인증서와 함께 https 사이트로 접속한다. 자체 서명된 인증서이기 때문에 안전하지 않다는 경고 문구가 뜨지만 무시하고 진행하면 된다.



**빌드하기**

**npm run build** 명령어는 배포 환경에서 사용할 파일을 만들어 준다. 빌드 후 생성된 자바스크립트 파일과 CSS 파일을 열어 보면 사람이 읽기 힘든 형식으로 압축된 것을 확인 할 수 있다. 이렇게 생성된 정적 파일을 웹 서버를 통해서 사용자가 내려받을 수 있게 하면 된다. 로컬에서 웹 서버를 띄워서 확인해 보자

**npx serve -s build**

serve 패키지는 노드(node.js) 환경에서 동작하는 웹 서버 애플리케이션이다. 정적 파일을 서비스할 때 간단하게 사용하기 좋다.

build/static 폴더 밑에 생성된 파일의 이름에 해시값이 포함되어 있다. 파일의 내용이 변경되지 않으면 해시값은 항상 같다. 새로 빌드를 하더라도 변경되지 않은 파일은 브라우저에 캐싱되어 있는 파일이 사용된다. 따라서 재방문의 경우 빠르게 페이지가 렌더링되는 효과를 볼 수 있다.

자바스크립트 파일에서 import 키워드를 이용해서 가져온 CSS 파일은 다음 경로에 저장된다.

**build/static/css/main.{해시값}.chunk.css**

여러 개의  CSS 파일을 임포트하더라도 모두 앞의 파일에 저장된다. 

자바스크립트 파일에서 import 키워드를 이용해서 가져온 폰트, 이미지 등의 리소스 파일은 build/static/media 폴더 밑에 저장된다. 이미지 파일의 크기가 10킬로바이트 보다 작은 경우에는 별도의 파일로 생성되지 않고 data url 형식으로 자바스크립트 파일에 포함된다. 파일의 크기가 작다면 한 번의 요청으로 처리하는 게 효율적이기 때문이다.

이미지 파일으 크기가 10킬로바이트보다 작은 파일과 큰 파일을 하나씩 준비해서 src 폴더 밑에 저장해 보자 그리고 App.js 파일에서 두 개의 이미지 파일을 불러오는 코드를 작성해 보자. 

**코드 1-13** App.js

```
// ...
import smallImage from './small.jpeg'; 1
import bigImage from './big.jpeg';     1

function App() {
  return (
   <div className="App">
     <img src={bigImage} alt="big" />      2
     <img src={smallImage} alt="small" />  2
// ... 
```



1 일반적인 자바스크립트 모듈처럼 이미지 파일을 자바스크립트로 가져왔다.

2 bigImage, smallImage 변수는 해당 이미지의 경로를 나타내는 문자열이다.

지금까지 작업한 내용을 빌드해 보자. media 폴더에는 big.{해시값}.jpeg 파일이 생성된다. 그러나 small.{해시값}.jpeg 파일은 생성되지 않는다. small.jpeg 파일의 내용은 어디에 있을까? main.{해시값}.jpeg 파일에서 data:image를 키워드로 검색해 보면 이미지 파일이 문자열 형태로 자바스크립트 파일에 포함되어 있다는 사실을 확인할 수 있다.



**테스트 코드 실행하기**

**npm test**를 입력하면 테스트 코드가 실행된다.

- _ _test _ _ 폴더 밑에 있는 모든 자바스크립트 파일
- 파일 이름이 .test.js로 끝나는  파일
- 파일 이름이 .spec.js로 끝나는 파일



**설정 파일 추출하기**

**npm run eject**를 실행하면 숨겨져 있는 create-react-app의 내부 설정 파일이 밖으로 노출된다. 이 기능을 사용하면 바벨이나 웹팩의 설정을 변경할 수 있다.

- 방법 1 : react-scripts 프로젝트를 포크(fork)해서 나만의 스크립트를 만든다.

  - 자유도가 높기 때문에 원하는 부분을 얼마든지 수정할 수 있다. 이렇게 수정된 내용을 여러 프로젝트에서 공통으로 사용할 수 있다는 장점도 있다.

- 방법 2: react-app-rewired 패키지를 사용한다.

  - 자유도는 낮지만 비교적 쉽게 설정을 변경할 수 있다는 장점이 있다.

- 두 가지 방법 모두 create-react-app의 이후 버전에 변경된 내용을 쉽게 적용할 수 없다는 단점이 있다.

  

**npm run eject**를 실행하면 숨겨져 있는 create-react-app의 내부 설정 파일이 밖으로 노출된다. 이 기능을 사용하면 바벨이나 웹팩의 설정을 변경할 수 있다.

- 방법 1 : react-scripts 프로젝트를 포크(fork)해서 나만의 스크립트를 만든다.

  - 자유도가 높기 때문에 원하는 부분을 얼마든지 수정할 수 있다. 이렇게 수정된 내용을 여러 프로젝트에서 공통으로 사용할 수 있다는 장점도 있다.

- 방법 2: react-app-rewired 패키지를 사용한다.

  - 자유도는 낮지만 비교적 쉽게 설정을 변경할 수 있다는 장점이 있다.

- 두 가지 방법 모두 create-react-app의 이후 버전에 변경된 내용을 쉽게 적용할 수 없다는 단점이 있다.

  

#### 1.3.3 자바스크립트 지원 범위

create-react-app에서는 ES6의 모든 기능을 지원한다.

정적 타입을 사용할 것을 추천한다. 현재로서는 타입스크립트가 가장 괜찮은 선택이라고 생각한다.



#### 1.3.4 코드 분할하기

코드 분할을 이용하면 사용자에게 필요한 양의 코드만 내려 줄 수 있다.

코드 분할을 사용하지 않으면 전체 코드를 한 번에 내려 주기 때문에 첫 페이지가 뜨는 시간이 오래 걸린다.

코드를 분할하는 한 가지 방법은 이전에 언급했던 **동적 임포트**를 이용하는 것이다.



**코드 1-20 Todo.js**  

```
import React from 'react';

export function Todo({title}){
  return <div>{title}</div>
}
```

TodoList.js 파일을 생성해서 Todo 컴포넌트를 이용하는 TodoList 컴포넌트 코드를 입력해 보자.



**코드 1-21 TodoList.js**  

```
import React, {useState} from 'react';

export default function TodoList() {
  const [todos, setTodos] = useState([]);                                   1
  const onClick = () => {                                                   2
    import('./Todo.js').then(({Todo}) = {                                   3
      const position = todos.length + 1;
      const newTodo = <Todo key={position} title{'할 일 ${position}'} />;    4
      setTodos([...todos, newTodo]);
    });
  };
  return (
    <div>
      <button onClick={onClick}>할 일 추가</button>
      {todos}                                                               5
    </div>
    );
}
```



1. 할 일 목록을 관리할 상태값을 정의했다.
2. `할 일 추가` 버튼을 클릭하면 호출되는 이벤트 처리 함수다.
3. `onClick` 함수가 호출되면 비동기로 Todo 모듈을 가져온다. 
   1. 동적 임포트는 프로미스를 반환하기 때문에 `then` 메서드를 이용해서 이후 동작을 정의할 수 있다.
4. 비동기로 가져온 Todo 컴포넌트를 이용해서 새로운 할 일을 만든다.
5. 상태값에 저장된 할 일 목록을 모두 출력한다.



**코드 1-22 App.js 파일에서 TodoList 컴포넌트 사용하기**

```
// ...
import TodoList from './TodoList'

function App() {
  return (
    <div className="App">
      <TodoList />
	  // ...
```

#### 1.3.5 환경 변수 사용하기

한경 변수는 개발, 테스트, 배포 환경별로 다른 값을 적용할 때 유용하다.

전달된 환경 변수는 코드에서 **process.env.{환경 변수 이름}**으로 접근할 수 있다.



**NODE_ENV 환경 변수 이용하기**

- npm start로 실행하면 development
- npm test로 실행하면 test
- npm run build로 실행하면 production



**기타 환경 변수 이용하기**

**NODE_ENV** 환경 변수 외에 다른 환경 변수는 REACT_APP_ 접두사를 붙여야 한다.

따라서 코드에서는 process.env.REACT_APP_ 형태로 접근할 수 있다.



셀에서 입력하는 방법

- 맥 : REACT_APP_API_URL-api.myapp.com npm start
- 윈도우 : "REACT_APP_API_URL-api.myapp.com npm start" && npm start



### 1.4 CSS 작성 방법 결정하기

전통적인 방법 : CSS를 별도의 파일로 작성하고 HTML의 link 태그를 이용해서 사용자에게 전달하는 것

순수 CSS 문법은 코드를 재사용하기가 힘들기 때문에 Sass를 이용하기도 한다. Sass에는 변수와 믹스인 개념이 있어서 중복 코드를 많이 줄일 수 있다.

리액트로 프로그래밍 할 때는 컴포넌트를 중심으로 생각하는 게 좋다. UI는 컴포넌트의 조합으로 표현되며, 컴포넌트 하나를 잘 만들어서 여러 곳에 재사용하는 게 좋다. 그렇게 하기 위해서 각 컴포넌트는 서로 간의 의존성 최소화하면서 내부적으로는 응집도를 높요야 한다.

응집도가 높은 컴포논트를 작성하기 위해 CSS 코드로 컴포넌트 내부에서 관리하는게 좋다.

컴포넌트 내부에서 CSS 코드를 관리하는 방법으로 **css-module, css-in-js**



#### 1.4.1 일반적인 CSS 파일로 작성하기

src 폴더 밑에 Button1.js, Button1.css 파일을 만들고 다음 코드를 입력한다.

**코드 1-28 Button 컴포넌트**

```
// Button1.css 파일                                                        1
.big {
  width: 100px;
}
.small {
  width: 50px
}
.button {
  height:30px;
  background-color: #aaaaaa;
}

// Button1.js 파일
import React from 'react';
import './Button1.css';                                                    2

function Button({ size }) {
  if (size === 'big') {
    return <button className="button big">큰 버튼</button>;                 3
  } else {
    return <button className="button small">작은 버튼</button>;
  }
}

export default Button;
```

1. Button1.css 파일은 일반적인 CSS 파일이다.
2. CSS 파일도 ESM 문법을 이용해서 자바스크립트로 가져올 수 있다.
3. Button1.css 파일에 정의된 CSS 클래스명을 입력한다.

같은 방식으로 Box 컴포넌트를 만들어서 스타일을 적용해 보자. Box1.js, Box1.css



**코드 1-29 Box 컴포넌트**

```
// Box1.css 파일
.big {
  width: 200px;
}
.small{
  width: 100px;
}
.box{
  height: 50px;
  background-color: #aaaaaa;
}

// Box1.js 파일
import React from 'react';
import './Box1.css';

function Box({ size }) {
 if (size === 'big') {
   return <div className="box big">큰 박스</div>;
 } else {
   return <div className="box small">작은 박스</div>;
 }
}
export default Box;
```

앞의 코드는 버튼 파일의 내용과 크게 다르지 않다. 이제 Button, Box 컴포넌트를 사용하여 App.js 파일을 다음과 같이 수정해 보자.

**코드 1-30 App.js 파일에서 Button, Box 컴포넌트를 가져와서 사용하기**

```
import React from 'react';
import Button from './Button1';
import Box from './Box1';

export default function App() {
  return (
    <div>
      <Button size="big" />
      <Button size="small" />
      <Box size="big" />      
      <Box size="small" />            
    </div>
  );
};
```



일반적인 CSS 파일에서는 클래스명이 충동할 수 있다.



#### 1.4.2 css-module로 작성하기

css-module을 사용하면 일반적인 css 파일에서 클래스명이 충돌할 수 있는 단점을 극복할 수 있다.

css-module은 간결한 클래스명을 이용해서 컴포넌트 단위로 스타일을 적용할 땨 좋다.

create-react-app에서는 css 파일 이름을 다음과 같이 작성하면 css-module이 된다.

**`{이름}.module.css`**

**코드 1-32 css-module로 작성된 Button 컴포넌트**

```
import React from 'react';
import style from './Button2.module.css';

function Button({ size }) {
  if (size === 'big') {
    return <button className={'${style.button} ${style.big}}'>큰 버튼</button>;
  } else {
    return ( 
      <button className={'${style.button} ${style.small}}'>작은 버튼</button>;
   );
  }
}
export default Button;
console.log(style);
```



#### 1.4.3 Sass로 작성하기

Sass는 CSS와 비슷하지만 별도의 문법을 이용해서 생산성이 높은 스타일 코드를 작성할 수 있게 도와준다.

Sass 문법에 있는 변수, 믹스인 등의 개념을 이용하면 스타일 코드를 재사용할 수 있다.

다음은 Sass 문법으로 작성된 간단한 코드다.

**코드 1-37 Sass로 작성된 스타일 코드**

```
$sizeNormal: 100px;              1

.box {
  width: $sizeNormal;            2
  height: 80px;
}

.button {
  width: $sizeNormal;            3
  height: 50px;
}

```

1. 일반적인 프로그래밍 언어처럼 변수를 정의할 수 있다.
2. 3 변수를 사용하면 중복을 없앨 수 있다.

Sass 문법으로 작성한 파일은 별도의 빌드 단계를 거쳐서 CSS 파일로 변환된다. create-react-app에서 Sass를 사용하고 싶다면 다음 패키지를 설치하자

**npm install node-sass**

node-sass 패키지는 Sass를 CSS로 빌드할 때 사용된다. create-react-app에는 Sass를 위한 빌드 시스템이 구축되어 있다. 자바스크립트에서 scss 확장자를 가지는 파일을 불러오면 자동으로 Sass 파일이 CSS 파일로 컴파일된다.

먼저 공통으로 사용되는 코드를 관리할 shared.scss 파일을 만든 다음 다음 내용을 입력하자

**코드 1-38 shared.scss**

```
$infoColor: #aaaaaa;
```



Sass를 이용해서 이전에 작성한 버튼 컴포넌트에 스타일을 적용해보자. 



#### 1.4.4 css-in-js로 작성하기

css-in-js는 리액트의 인기에 힘입어 비교적 최근에 떠오르고 있는 방법이다. 이름에서 알 수 있듯이 CSS 코드를 자바스크립트 파일 안에서 작성한다. CSS 코드가 자바스크립트 안에서 관리되기 때문에 공통되는 CSS 코드를 변수로 관리할 수 있다. 또한 동적으로 CSS 코드를 작성하기도 쉽다.

css-in-js를 지원하는 패키지가 많이 나왔고, 문법도 다양하다. 개발자 개개인이 자바스크립트와 CSS 모두를 작성할 줄 안다면 css-in-js는 좋은 선택이 될 수 있다. 그러나 CSS만 담당하는 마크업 개발팀이 별도로 있는 회사라면 css-in-js를 도입하기가 힘들 수 있다.

**npm install styled-components**



**코드 1-41 동적 스타일이 적용되지 않은 Box4.js**

```
import React from 'react';
import styled from 'styled-components';

const BoxCommon = styled.div`                             1
  height: 50px;
  background-color: #aaaaaa;
`;
const BoxBig = styled.div`                                2
  width: 200px;
`;
const BoxSmall = styled.div`
  width: 100px;
`;

function Box({ size }) {
  if (size ==== 'big') {
    return <BoxBig>큰 박스</BoxBig>;                       3
  } else {
    return <BoxSmall>작은 박스</BoxSamll>;
  }
}

export default Box;
```

1. 공통 CSS 코드를 담고 있는 styled-components 컴포넌트를 만들었다.
2. 마치 클래스 상속처럼 이전에 만든 **BoxCommon** 컴포넌트를 확장해서 새로운 styled-components 컴포넌트를 만들 수 있다.
3. styled-components 컴포넌트는 일반적인 리액트 컴포넌트처럼 사용될 수 있다. styled-components 컴포넌트를 만들 때 사용된 문법이 생소해 보일 수 있다. 이는 ES6에 추가된 태그된 템플릿 리터럴  문법이다. 

App.js 파일에서 Box4.js 파일을 가져오도록 수정하고 npm start를 실행하면 의도한 대로 동작하는 것을 확인할 수 있다. CSS 파일 없이 자바스크립트 파일만으로 스타일을 작성하는 데 성공한 것이다.

**코드 1-42 동적 스타일이 적용된 Box4.js**

```
import React from 'react';
import styled from 'styled-components';

const BoxCommon = styled.div`                             
  width: ${props => (props.isBig ? 200 : 100)}px;                   1
  height: 50px;
  background-color: #aaaaaa;
`;

function Box({ size }) {
  const isBig = size === 'big';
  const label = isBig ? '큰 박스' : '작은 박스';
  return <BoxCommon isBig={isBig}>{label}</BoxCommon>;              2
}

export default Box;
```

1. 템플릿 리터럴에서 표현식을 사용하면 컴포넌트의 속성값을 매개변수로 갖는 함수를 작성할 수 있다. 동적으로 스타일을 변경하기 때문에 styled-components 컴포넌트는 하나로 충분하다.
2. isBig 속성값은 styled-components 컴포넌트의 표현식에서 사용된다.



### 1.5 단일 페이지 애플리케이션 만들기

리액트 애플리케이션의 페이지 전환은 **단일 페이지 애플리케이션(single page application, SPA)** 방식으로 개발하는 것이 정석이다.

단일 페이지 애플리케이션은 초기 요청 시 서버에서 첫 페이지를 처리하고 이후의 라우팅은 클라이언트에서 처리하는 웹 애플리케이션이다. 전동적인 방식의 웹 페이지는 페이지를 전환할 때마다 렌더링 결과를 서버에서 받기 때문에 화면이 깜빡이는 단점이 있었다. 단일 페이지 애플리케이션은 페이지 전환에 의한 렌더링을 클라이언트에서 처리하기 때문에 마치 네이티브 애플리케이션처럼 자연스럽게 동작한다.



먼저 단일 페이지 애플리케이션을 구현하기 위해 필요한 **브라우저 히스토리 API**를 알아보자. 그런 다음 브라우저 히스토리 API를 기반으로 구현된 react-router-dom 패키지를 이용해서 간단한 단일 페이지 애플리케이션을 만들어 보자.

이번 절에서도 create-react-app을 기반으로 설명한다. 우선 router-test라는 이름으로 프로젝트를 생성해 보자.

**npx create-react-app router-test**



#### 1.5.1 브라우저 히스토리 API

**SPA** 구현이 가능하려면 다음 두 가지 기능이 필요하다.

- 자바스크립트에서 브라우저로 페이지 전환 요청을 보낼 수 있다. 단, 브라우저는 서버로 요청을 보내지 않아야 한다.
- 브라우저의 뒤로 가기와 같은 사용자의 페이지 전환 요청을 자바스크립트에서 처리할 수 있다. 이때도 브라우저는 서버로 요청을 보내지 않아야 한다.

이러한 조건을 만족하는 브라우저 API는 **pushState, replaceState** 함수와 **popstate** 이벤트이다.

API 이름에서 알 수 있듯이 브라우저에는 히스토리에 state를 저장하는 스택(stack)이 존재한다.

브라우저 히스토리 API의 사용법을 확인해 보기 위해 App.js 파일을 다음과 같이 수정해 보자.

**코드 1-43 브라우저 히스토리 API의 동작을 확인하는 코드** 

```
import React, { useEffect } from 'react';

export default function App() {
  useEffect(() => { 3
    window.onpopstate = function(event) { 2
      console.log(`location: ${document.location}, state: ${event.state}`);
    };
  }, []);
  return (
   <div>
     <button onClick={() => window.history.pushState('v1','','/page1')}> 1
       page1
     </button>
     <button onClick={() => window.history.pushState('v2','','/page2')}> 1
       page2
     </button>
   </div>
  );
}
```

1. page1 버튼과 page2 버튼을 번갈아 가며 눌러 보자. 브라우저 주소창의 url이 /page1과 /page2로 번갈아 변경되는 것을 확인할 수 있다. 이때 서버로 요청이 가지 않고 화면도 변하지 않는다. 단지 스텍에 state가 쌓일 뿐이다.
2. 코드에서 등록한 onpopstate 함수도 호출되지 않는다.
3. **useEffect 함수**는 이벤트 핸들러를 등록하거나 API를 호출하는 등의 부수 효과를 처리할 때 사용하는 훅이다. 여기서는 컴포넌트가 마운트된 후에 popstate 이벤트 핸들러를 등록하는 용도로 사용했다.

이번에는 브라우저의 뒤로 가기 버튼을 눌러 보자. onpopstate 함수가 호출되는 것을 확인할 수 있다. 계속해서 뒤로 가기를 누르면 스택이 비워질 때까지 onpopstate 함수가 호출되다가 최초에 접속했던 페이지로 돌아간다.

처음에 언급했던 두 가지 기능이 pushState 함수와 popstate 이벤트로 모두 구현됐다. replaceState 함수는 pushState와 거의 같지만 스택에 state를 쌓지 않고 가장 최신의 state를 대체한다. 이렇게 pushState, replaceState 함수와 popState 이벤트만 있으면 클라이언트에서 라우팅 처리가 되는 단일 페이지 애플리케이션을 만들 수 있다.

브라우저 히스토리 API를 이용해서 간단한 단일 페이지 애플리케이션을 만들어보자. App.js 파일에 다음 코드를 입력한다.



**코드 1-44 브라우저 히스토리 API로 직접 작성한 단일 페이지 애플리케이션**

```
import React, { useEffect, useState } from 'react';

export default function App() {
  const [pageName, setPageName] = useState('');                         1
  useEffect(() => {
    window.onpopstate = event => {
     setPageName(event.state);                                          2
    };
  }, []);
  function onClick1() {                                                 3
    const pageName = 'page1';
    window.history.pushState(pageName, '', './page1');
    setPageName(pageName);
  }
  function onClick2() {                                                 3
    const pageName = 'page2';
    window.history.pushState(pageName, '', './page2');
    setPageName(pageName);
  }
  return (
    <div>
      <button onClick={onClick1}>page1</button>
      <button onClick={onClick2}>page2</button>
      {!pageName && <Home />}                                           4
      {pageName === 'page1' && <Page1 />}                               5
      {pageName === 'page2' && <Page2 />}                               5
    </div>    
  );
}

function Home() {
  return <h2>여기는 홈페이지입니다. 원하는 페이지 버튼을 클릭하세요.</h2>;
}
function Page1() {
  return <h2>여기는 Page1입니다.</h2>;
}
function Page2() {
  return <h2>여기는 Page2입니다.</h2>;
}
```



1. 현재 페이지 정보를 pageName 상태값으로 관리한다. 
2. popstete 이벤트가 발생하면 페이지를 전환한다는 의미로 pageName 상태값을 수정한다. 브라우저 히스토리 state를 페이지 이름으로 사용하고 있다.
3. 페이지 버튼을 클릭했을 때 호출되는 이벤트 처리 함수다.
4. 페이지 버튼을 누르기 전에는 Home 컴포넌트가 렌더링된다.
5. 첫 번째 페이지 버튼을 클릭하면 Page1 컴포넌트가 렌더링되고, 두 번째 페이지 버튼을 클릭하면 Page2 컴포넌트가 렌더링된다.

페이지 버튼을 클릭하면 브라우저 주소창의 내용이 변경되고, 브라우저의 뒤로 가기 버튼을 클릭해도 의도한 대로 잘 동작하는 것을 확인할 수 있다.

#### 1.5.2 react-router-dom 사용하기

브라우저 히스토리 API를 이용해서 페이지 라우팅 처리를 직접 구현할 수도 있지만 신경 써야 할 부분이 많다. 이럴 때 도움이 되는 것이 react-router-dom으로, 리액트로 단일 페이지 애플리케이션을 만들 때 많이 사용된다. react-router-dom 패키지도 내부적으로 브라우저 히스토리 API를 사용한다.

**npm install react-router-dom**

react-router는 웹뿐만 아니라 리액트 네이티브도 지원한다.

react-router-dom을 사용해서 단일 페이지 애플리케이션을 만들어 보자, 먼저 App.js 파일에 다음 코드를 입력한다.

**코드 1-45 react-router-dom으로 작성한 단일 페이지 애플리케이션**

```
import React from 'react';
import { BrowserRouter, Route, Link } from 'react-router-dom';
import Rooms from './Rooms';                                            1

export default function App() {
  return (
    <BrowserRouter>                                                     2
      <div style={{ padding: 20, border: '5px solid gray' }}>
        <Link to="/">홈</Link>                                          3
        <br />
        <Link to="/photo">사진</Link>                                    3
        <br />
        <Link to="/romms">방 소개</Link>                                 3
        <br />
        <Route exact path="/" componenet={Home} />                      4
        <Route path="/photo" componenet={Photo} />                      4
        <Route path="/rooms" componenet={Rooms} />                      4
       </div>
    </BrowserRouter>
  );
}

function Home({ match }) {
  return <h2>이곳은 홈페이지입니다.</h2>;
}
function Photo({ match }) {
  return <h2>여기서 사진을 감상하세요.</h2>;
}
```

1. Rooms 컴포넌트는 별도의 파일로 구현할 예정이다.
2. react-router-dom을 사용하기 위해서는 전체를 BrowserRouter 컴포넌트로 감싸야 한다.
3. 버튼을 통해서 페이지를 전환할 때는 react-router-dom에서 제공하는 Link 컴포넌트를 사용한다. to 속성값은 이동할 주소를 나타낸다.
4. react-router-dom의 Route 컴포넌트를 이용해서 각 페이지를 정의한다. 현재 주소가 path 속성값으로 시작하면 component 속성값이 가리키는 컴포넌트를 렌더링한다. 
   1. 예를 들어, localhost:3030/photo/abc를 입력했을 때 주소가 /photo으로 시작하므로 Photo 컴포넌트가 렌더링되지 않는다. 이는 슬래시(/)단위로 비교를 하기 때문이다. 

exact 속성값을 입력하지 않았다면 Home 컴포넌트는 항상 렌더링 된다.

**코드 1-47 Room.js**

```
import React from 'react';
import { Route, Link } from 'react-router-dom';

function Rooms({ match }) {                                            1
 return (
   <div>
     <h2>여기는 방을 소개하는 페이지입니다.</h2>
     <Link to={`${match.url}/blueRoom`}>파란 방입니다.</Link>             2
     <br />
     <Link to={`${match.url}/greenRoom`}>초록 방입니다.</Link>
     <br />
     <Link to={`${match.url}/:roomId`} component={Room} />              3
     <Route
       exact
       path={match.url}
       render={() => <h3>방을 선택해 주세요.</h3>}
      />     
   </div>
 );
}

export default Rooms;

function Room({ match }) {
  return <h2>{`${match.params.roomId} 방을 선택하셨습니다.`}</h2>;          4
}
```

Rooms 컴포넌트 내부에는 또다시 라우팅을 처리하는 코드가 들어 있다.

1. Route를 통해서 렌더링되는 컴포넌트는 match라는 속성값을 사용할 수 있다.
2. match.url은 Route 컴포넌트의 path 속성값과 같다. 따라서 Rooms 컴포넌트의 match.url은 /rooms과 같다.
3. Route 컴포넌트의 path 속성값에서 콜론을 사용하면 파라미터를 나타낼 수 있다.
4. 추출된 파라미터는 match.params.{파라미터 이름} 형식으로 사용될 수 있다.