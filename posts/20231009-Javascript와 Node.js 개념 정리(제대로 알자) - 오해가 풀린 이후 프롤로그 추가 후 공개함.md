# 프롤로그

본 글은 최근 시니어 개발자와 커뮤니케이션 과정에서 발생한 내용을 수정한 글이다. 결론만 말하자면, 그가 말한 React.js는 Next.js를 말한 것이었으며, 필자는 React.js 자체를 말한 것이었으므로 서로 다른 주제로 대화를 하고 있었다는 사실이 밝혀졌다. 이 대화 내용을 주고 받은 시점은 그가 팀에 합류한 지 약 1주일 정도 되는 날이었다. 그렇기에 그가 말하는 것과 필자가 말하는 것이 서로 다를 수 있다는 사실은 충분히 받아들여진다. 팔자는 프론트엔드 개발자가 아니라 최근 프론트엔드 트렌드는 잘 모른다. 해당 시니어는 요즘 대부분 프론트엔드 개발자가 Next.js를 React.js라고 한다고 했으며, 필자는 그 말을 듣자마자 그가 말한 내용이 모두 이해가 되었다. 이 일이 있고난 후 약 1주가 지났고, 이 이슈는 하나의 히스토리가 되었다. 수정 전의 내용을 수정하려고 하였으나, 이 또한 하나의 기록으로 남기기 위해 그대로 보존한 후 프롤로그만 작성하여 공개로 전환하였다.

# 들어가며

최근 어떤 시니어 개발자로부터 아래와 같이 괴상한 말을 들은 적이 있다.

> 오해가 풀린 이후(그가 말한 React.js가 Next.js라는 사실을 알고 난 후), 아래 내용 중 두 번째 내용을 제외하고는 반박할 여지 없이 당연하다는 생각이 든다. S3로 배포할 것이었으면 Next.js 대신에 React.js를 사용했을 것이며, React.js로는 Socket 서버를 열 수 없기 때문이다.

1.  React로 개발한 웹 애플리케이션을 배포할 때 S3로 배포하지 말고 별도의 서버 애플리케이션을 두어 배포해야 한다.
2.  React 웹 애플리케이션도 S3에 로깅해야 한다.
3.  React도 NodeJS이며 서버이므로 Socket 서버를 열 수 있으며, Chrome에서 동작하는 Javascript도 NodeJS이므로 Node.js에서 사용 가능한 모듈은 React에서도 사용할 수 있다(예 : Redis, MongoDB 커넥션 등).

이 말들에 반박을 하고 싶었지만 Javascript와 Node.js의 차이점부터 설명해야 논리를 풀어갈 수 있을 것이라 판단하여 조용히 자리를 피했다. 여전히 이 말들을 떠올리면 어이없음과 함께 분노가 치밀어 오른다. 이와 관련된 내용을 수차례 작성하고 삭제했으나, 감정이 너무 섞여있다고 판단하여 최대한 감정을 걷어내고 작성하였다.

# 1\. 개요

## 1.1. Javascript

> Javascript에 대한 역사와 등장 배경에 대해서는 [다른 글](https://choewy.tistory.com/145)에 정리해두었다.

Javascrip는 HTML에 종속되어 있는 스크립트 언어이며, HTML 조작과 변경을 위해 사용된다. Javascript를 해석하는 주체는 웹 브라우저(구체적으로 말하면 웹 브라우저에 탑재된 엔진)이며, 대표적으로는 2008년에 구글에서 출시한 Chrome에 탑재된 V8 엔진이 있다. 정리하자면, Javascript는 웹 브라우저에서 HTML이라는 문서를 다루는 용도로 사용되는 언어라고 할 수 있다. 따라서, 웹 브라우저에서 실행되는 Javascript는 운영체제 또는 파일 시스템에 직접적인 접근이 불가능하다.

# 1.2. Node.js

Node.js는 Chrome V8 Javascript 엔진으로 빌드된 Javascript 런타임이다.

> Node.js 공식 사이트에서도 위와 같이 설명되어 있다.

Node.js가 등장하기 이전까지의 Javascript는 웹 페이지 요소 접근이라는 단 한 가지 용도로만 사용되었다. 즉, 웹 브라우저 없이는 동작할 수 없었으며, 클라이언트 개발을 위한 용도로 국한되어 있었다. 2005년, Google Maps의 등장으로 Javascript의 가능성이 확인되었고, 이를 통해 웹 애플리케이션을 구축하려는 시도와 활용이 많아짐에 따라 Javascript 엔진에 대한 수요도 증가하였다. 2008년 9월, 구글이 발표한 C++ 기반의 오픈소스화 된 V8엔진이 베타버전으로 등장하였고, 어떤 Javascript 엔진보다도 우수한 속도를 보여주었으며, 지속적으로 성능을 향상시켰다. V8 엔진이 강력해지자, 2009년 1월부터 Javascript를 웹 브라우저가 아닌 곳에서도 사용할 수 있는 표준에 대한 수요가 높아짐에 따라 CommonJS 표준이 발표되었다. Ryan Dahl은 CommonJS 표준, 이벤트 구동 서버에 대한 지식, V8 엔진을 이용한 non-blocking, event-driven I/O 패러다임에서 작업할 수 있도록 Node.js를 만들었다.

> 이를 한 줄로 요약하자면, Google Maps와 Chrome에 탑재된 V8 엔진이 등장한 이후 Javascript의 수요가 증가함에 따라 웹 브라우저 없이 컴퓨터 내에서 다양한 용도로 작업하기 위해 만들어진 것이 Node.js이다.

# 1.3. 개념 정리

결국 브라우저에서 동작하는 Javascript와 Node.js에서 동작하는 Javascript는 Javascript라는 공통된 언어이나, 실행 환경에 따라 언어 자체의 목적과 기능이 크게 달라진다.

웹 브라우저에서 동작하는 Javascript는 웹 페이지의 구성 요소에 접근하는 기능에만 제한되어 있다. 반면, Node.js에서 동작하는 Javascript는 서버 및 응용 프로그램 개발 환경을 제공하는 것이 주된 목적이며, 이 둘의 기능적 범위는 서로 배타적이라고 할 수 있다. 아래 코드는 이를 증명하기에 가장 적합한 코드가 아닐까 싶다.

```
/** @description browser에서의 this */
console.log(this); // Window

/** @description node.js에서의 this */
console.log(this); // {}
```

웹 브라우저에 동작하는 Javascript에서 this를 출력하면 Window 객체가 출력되나, Node.js에서 동작하는 Javascript에서 this를 출력하면 빈 Object가 출력된다. 이 말은 같은 Javascript 임에도 불구하고 서로 다른 기능 범위 내에서 동작한다는 것을 나타낸다고 할 수 있다. 예를 들어, 브라우저에는 DOM에 접근할 수 있으나, Node.js에서는 DOM에 접근할 수 없다. 반대로, Node.js에서는 MySQL에 연결하거나, FileSystem을 사용할 수 있으나, 웹 브라우저에서는 불가능하다.

> Javascript와 Node.js를 구분하고 비교하는 것을 자주 볼 수 있으나, 이는 부정확한 비교 방식이 아닐까라는 생각이 든다. 개념을 정확히 알고 있다면야 상관없겠으나, 이와 같이 비교를 한다면 Javascript와 Node.js는 서로 다른 것으로 인식되지 않겠는가? 따라서, 브라우저에서 동작하는 Javascript인지, Node.js에서 동작하는 Javascript인지를 구분하는 편이 더 정확하지 않을까라는 생각을 해본다.

# 2\. 궤변 정리

## 2.1. React는 서버 애플리케이션으로 배포해야 한다?

이 말은 듣고 일단 가장 먼저 떠오른 말은 굳이?였다. 과거에야 프론트엔드, 백엔드 구분 없이 서버에서 웹 페이지의 구성요소를 제공하고, 웹 브라우저가 Javascript를 해석하여 렌더링을 했기에 그럴 수 있겠지만, SPA라는 개념이 등장한 이후에는 렌더링 전에 웹 애플리케이션에 필요한 정적 리소스를 처음 한 번 다운로드 후 페이지 갱신에 필요한 데이터를 서버로부터 받아와서 재렌더링하지 않는가? 재렌더링하는 경우가 빈번한 경우 웹 브라우저의 성능 저하를 야기하거나, 최초 렌더링 속도가 SSR에 비해 다소 느리다는 단점이 존재하겠으나, 필요한 부분만 서버로부터 데이터를 받아온 후 렌더링하는 것이 서버가 웹 페이지의 모든 구성요소를 제작 후 브라우저가 렌더링하는 과정보다 효율적이지 않은가?

이어서 추가로 React를 서버로 배포할 때 어떤 방식으로 배포했는가에 대한 질문을 했는데, Node.js로 배포한다고 하였다. 이 대답을 듣고 추가로 express로 배포했는지, serve 모듈로 배포했는지 등을 여쭤보았으나, React를 빌드 후 `npm run start`를 하면 Node.js로 배포된다는 대답을 들었다. `npm run start` 스크립트를 어떻게 작성하느냐에 따라 동작 방식이 달라질 수 있겠으나, CRA를 통해 기본적으로 생성된 스크립트의 경우 build되지 않은 개발 환경의 React 웹 애플리케이션을 실행시킨다. 이와 같이 배포하면 build할 필요가 없을 뿐더러, 압축되지 않았으므로 build된 정적 파일에 비해 용량이 높다는 단점이 있다. 서버가 필요하다면 React를 배포할 서버 애플리케이션을 직접 구현하는 것보다는 차라리 Next.js로 개발 후 배포하는 편이 더 낫지 않겠는가? 초기 스타트업에서 React를 사용하는 사례가 많은 이유 중 하나가 개발부터 배포가 쉽기 때문이라고 생각한다. 그 이유는 서버 트래픽 관리 및 운영 비용 등을 절감시킬 수 있을 뿐더러, CDN(하다못해 Github Pages)을 통해 빠르게 배포할 수 있기 때문이지 않는가?

## 2.2. React도 S3에 직접 로깅해야 한다?

필요하다면 S3에 로깅할 수 있겠으나, 이런 사례는 들어본 적이 없다. 왜 S3에 로깅해야 하는가? 접속지가 불분명한 웹 브라우저에서 S3에 로깅할 수 있으려면 권한 등의 다소 민감한 정보를 public으로 허용해야 하지 않겠는가? 또한, S3에 로깅한다면 Bucket에 Object 단위로 로깅이 되지 않겠는가? 차라리 별도의 로깅용 서버로 API 요청을 하는 편이 더 낫다고 생각한다. 서버가 있으면 파일에 접근이 가능하므로 파일로 쪼개서 Batch를 구성하여 S3에 적재하는 편이 클라이언트에서 S3에 직접 접근하는 것보다 보안적, 비용적인 측면으로 더 낫지 않을까 생각한다. 그런데 위 방법을 떠올리다보면 서버 운영 비용, 구축 비용 등을 고려하게 된다. 결국 로깅용 서버 1대를 배포해야하고, 서버 로직을 처리해줄 애플리케이션을 개발해야 하기 때문이다. 결국, 시간적 비용, 인적 자원이 추가로 드는 셈이다. 그럴 바에는 차라리 일정 비용을 지불하고 Sentry와 같은 로깅 및 모니터링 서비스를 활용하는 편이 효율적이지 않겠는가?

## 2.3. Node.js에서 사용 가능한 모듈을 React에서도 사용할 수 있다?

React에서 Redis에 직접적으로 연결할 수 있는가? 불가능하다. 왜? 브라우저에서 동작하는 Javascript는 Node.js에서 동작하는 Javascript가 아니기 때문이다. 근거를 기록하기 위해 직접 테스트 해보았다.

```
import { useEffect } from 'react';

import IoRedis from 'ioredis';

export const App = () => {
  useEffect(() => {
    const redis = new IoRedis({
      host: 'localhost',
      port: 6380,
      db: 0,
      lazyConnect: true,
    });

    redis.connect();
  }, []);

  return <div></div>;
};
```

결과는 너무 당연하게도 오류가 발생한다. 오류의 내용으로는 utils, stream, crypto, dns, net, tls, url, buffer 등의 모듈을 IoRedis 모듈 및 해당 모듈의 의존성으로부터 찾을 수 없다는 것이다. 앞서 언급하였는데, 웹 브라우저에서 동작하는 Javascript는 HTML 요소를 다루기 위해 동작한다. 그 말인 즉슨, tls, dns, stream 등 서버 사이드에서만 동작하는 기능은 사용할 수 없다는 것이다.

# 마치며

이 일이 떠오를수록 분노가 치밀어 오르는 이유는 그가 이 말을 주니어 개발자한테 설명했었고, 자신이 위와 같은 경험을 직접 해보았다는 거짓된 정보를 전달했기 했기 때문이다. 실제 코드를 보여달라고 요청했으나, 코드도 없었을 뿐더러 다른 코드를 보여주었는데, 이와는 전혀 다른 정상적인 코드였다. 연차가 높은 개발자라고 할 지라도 위와 같은 괴상한 말들을 내뱉을 수 있다. 경험이 적을수록 실습이 동반된 공부를 게을리하지 않으면 무엇이 참인지 분별할 수 없기에 꾸준히 공부해야만 실력있는 개발자로 거듭날 수 있으리라 생각한다. 그리고, 연차가 올라감에 따라 자신의 경험만을 믿고 나태해지면 안 되고, 자신이 알고 있는 내용이 참인지 거짓인지 다시 복기하는 시간이 반드시 필요할 것 같다. 필자는 프로그래밍 뿐만 아니라 운동이나, 악기를 배울 때에도 기초를 매우 중요하게 생각한다. 기반이 허술하면 언젠가는 쉽게 무너지기 때문이다. 무엇을 하든지 기초가 잘 잡혀있어야 그 위에 많은 것들을 쌓아올릴 수 있는 것은 변함없는 것 같다.

# 참고자료

- [Introduction to Node.js](https://nodejs.dev/en/learn)
- [ECMAScript 2015 (ES6) and beyond](https://nodejs.org/en/docs/es6)
- [Node.js Wiki](https://ko.wikipedia.org/wiki/Node.js)

# 같이 읽으면 좋은 글

- [설레발 주도 개발 - Hype Driven Development](https://lazygyu.net/blog/hype_driven_development)
- [더 이상 배우려 하지 않는 개발자 - How Developers Stop Learning: Rise of the Expert Beginner](https://daedtech.com/how-developers-stop-learning-rise-of-the-expert-beginner/)
- [소프트웨어 집단의 부패 - How Software Groups Rot: Legacy of the Expert Beginner](https://daedtech.com/how-software-groups-rot-legacy-of-the-expert-beginner/)
