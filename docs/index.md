Bun은 Javascript, Typescript  앱의 다용도 도구모음(이하 툴킷)입니다.  `bun`이라는 단일 실행파일로 구성되어있습니다.

Node.js의 빠른 런타임위해 만들어진 편리한 대체품인 _Bun runtime_은 `bun`의 Bun의 핵심구성품입니다. Zig으로 작성되었으며, JavaScriptCore가 Bun을 보이지 않는 곳에서 작동시켜 극적으로 시작 시간과 메모리 사용량을 줄여줍니다.

```bash
$ bun run index.tsx # TS 와 JSX도 바로 사용이 가능합니다.
```

`bun` 명령줄(이하 커맨드라인) 툴은 현재 나와있는 어떠한 툴과 기존의 Node.js 프로젝트를 바꾸지 않거나, 살짝 바꿔 실행하는 툴보다  빠른 테스트 실행, 스크립트 실행, Node.js 호환 패키지 관리자 역시 포함되어있습니다.

```bash
$ bun run start                  # 'start' 스크립트 실행
$ bun install <pkg>              # 패키지 설치
$ bun build ./index.tsx          # 브라우저를 위한 압축
$ bun test                       # tests 실행
$ bunx cowsay "Hello, world!"    # 패키지 실행
```

{% callout type="note" %}
**Bun은 아직도 개발 단계에 있습니다.** 개발 작업 속도를 올리고 싶거나, 서버가 포함되지 않은 기능과 같이 제약적 자원 환경에서 보다 단순한 코드를 실행할 때 사용해주세요. Node.js와 이미 존재하고 있는 다양한 프레임워크와의 호환을 위해서 열심히 개발 중에 있습니다. 추가적인 배포를 빠르게 알고 싶다면 [Discord](https://bun.sh/discord)에 가입하거나 [GitHub repository](https://github.com/oven-sh/bun)를 확인해주세요.
{% /callout %}

다음의 링크를 통해서 Bun를 시작하거나, Bun에 대해서 더 자세히 알고 싶다면 계속해서 읽으시면 됩니다.

{% block className="gap-2 grid grid-flow-row grid-cols-1 md:grid-cols-2" %}
{% arrowbutton href="/docs/installation" text="Bun 설치 방법" /%}
{% arrowbutton href="/docs/quickstart" text="간단설치 방법" /%}
{% arrowbutton href="/docs/cli/install" text="package 설치 방법" /%}
{% arrowbutton href="/docs/templates" text="project 템플릿 사용" /%}
{% arrowbutton href="/docs/bundler" text="production을 위한 Bun 코드" /%}
{% arrowbutton href="/docs/api/http" text="HTTP 서버 만들기" /%}
{% arrowbutton href="/docs/api/websockets" text="Websocket 서버 만들기" /%}
{% arrowbutton href="/docs/api/file-io" text="파일 읽기 와 쓰기" /%}
{% arrowbutton href="/docs/api/sqlite" text="SQLite 쿼리문 실행" /%}
{% arrowbutton href="/docs/cli/test" text="테스트 실행과 작성법" /%}
{% /block %}

## 런타임이란 무엇인가요?

Javascript 혹은 ECMAScript는 프로그래밍 언어에 대한 규격일 뿐입니다. 누구나 올바른 형식의 JavaScript 프로그램을 실행해주는 JavaScript 엔진을 제작할 수 있습니다. 현재 가장유명한 엔진으로는 오픈소스인 구글에서 제작한 V8과 마찬가지로 오픈소스인 애플에서 제작한 JavaScriptCore가 있습니다.

### 브라우저

거의 모든 JavaScript 프로그램은 혼자서 작동되지 않습니다. JavaScript는 유용한 작업들을 하기 위해서는 JavaScript의 환경이 아닌 또 따른 무언가들이 많이 필요합니다. 여기서 _runtimes_ 이 사용되게 됩니다. Javascript가  부가 기능을 추가적인 API들의 구현으로 통해 실행할 수 있게 됩니다. 예시를 들자면, 브라우저는 오직 웹에서 사용되는 API인 전역변수 `window` 오브젝트가 구현되어있는 런타임을 제공합니다. 브라우저로 실행된 모든 JavaScript code는 이 API를 사용하여 참여적이고 다양한 기능을 만들 수 있습니다.

<!-- JavaScript runtime that exposes  JavaScript engines are designed to run "vanilla" JavaScript programs, but it's often JavaScript _runtimes_ use an engine internally to execute the code and implement additional APIs that are then made available to executed programs.
JavaScript was [initially designed](https://en.wikipedia.org/wiki/JavaScript) as a language to run in web browsers to implement interactivity and dynamic behavior in web pages. Browsers are the first JavaScript runtimes. JavaScript programs that are executed in browsers have access to a set of Web-specific global APIs on the `window` object. -->

### Node.js

비슷하게, Node.js는 서버처럼 브라우저가 사용되지 않는 환경에서의 런타임이라고 생각하면됩니다.  Node.js에 의해 실행된 JavaSciprt 프로그램은 `Buffer`,`process`,`__dirname`와 같은 전역변수와  파일 읽기 쓰기(`node:fs`)와 네트워크 같이 OS레벨에서 실행되는 명령 위해 내장되어있는 모듈(`node:net`, `node:http`)에 접근할 수 있습니다. 또한 Node.js는 CommonJS 기반 모듈과 자바스크립트의 네이티브 모듈 시스템을 미리 업데이트하는 해상도 알고리즘이 구현되어 있습니다.

<!-- Bun.js prefers Web API compatibility instead of designing new APIs when possible. Bun.js also implements some Node.js APIs. -->

Bun은 빠르고, 가볍고, 더 현대적인 Node.js의 대체품으로 디자인되었습니다.

<!-- ## Why a new runtime?

Bun is designed as a faster, leaner, more modern replacement for Node.js. Node.js is burdened by ingrained performance issues, backwards compatibility concerns, and slow development velocity—inevitable issues for a project of its age and magnitude. -->

## Design goals

Bun은 현재의 JavaScript 에코시스템을 염두에 두고 처음부터 설계되었습니다.

- **속도**. Bun은 [Node.js보다 4배 빠르게](https://twitter.com/jarredsumner/status/1499225725492076544) 처리됩니다.(직접 해보세요!)
- **TypeScript와 JSX 지원**. Bun의 트랜스파일러는 실행하기 전에 JavaScript 코드로 변경하여 `.jsx`,`.ts` 와 `.tsx` 파일을 바로 실행할 수 있습니다.
- **ESM 과 CommonJS 호환**. 요즘 시장은 ES 모듈(ESM)를 사용하는 추세입니다. 하지만 npm에 있는 백 만개의 패키지는 아직도 CommonJS를 필요로 합니다. Bun 역시 ESM를 권장하고 있지만, CommonJS 역시 지원하고 있습니다.
- **웹 표준 APIs**. `fetch`, `WebSocket`, `ReadableStream`과 같은 Bun은 표준 웹 API 구현하였습니다. Bun은 Apple이 Safari를 위해 개발한 JavaScriptCore를 기반으로 실행되기에 [`Headers`](https://developer.mozilla.org/en-US/docs/Web/API/Headers) and [`URL`](https://developer.mozilla.org/en-US/docs/Web/API/URL) 와 같은 몇 몇의 API는 바로 직접적으로  [Safari's implementation](https://github.com/oven-sh/bun/blob/HEAD/src/bun.js/bindings/webcore/JSFetchHeaders.cpp)에 사용할 수 있습니다.
- **Node.js 호환성**. 추가적으로 Bun은 노드 스타일 모듈안도 지원합니다. Bun은 내장되어있는 Node.js의 전역변수(`process`,`Buffer`)와 모듈(`path`,`fs`,`http`, 등등...) 완벽한 호환성을 지원하는 것이 목표입니다. 이 목표는 현재까지 진행 중인 사항이며, 완료되지 않습니다. 현재의 상황을 보고 싶다면 [다음 페이지](https://bun.sh/docs/runtime/nodejs-apis)를 참조해주세요.


Bun을 단순히 런타임이라고 생각하면 안됩니다. 저희의 Bun를 JavaScript/TypeScript로 앱을 제작하고, 패키지 관리자를 포함하고, 트랜스파일러, 번들러, 스크립트 실행, 테스터 외에 더 많은 것을 포함한 종합적인 인프라 툴킷으로 만들 것이라는 궁극적인 목표를 가지고 있습니다.

<!-- - tsconfig.json `"paths"` is natively supported, along with `"exports"` in package.json
- `fs`, `path`, and `process` from Node.js are partially implemented
- Web APIs like [`fetch`](https://developer.mozilla.org/en-US/docs/Web/API/fetch), [`Response`](https://developer.mozilla.org/en-US/docs/Web/API/Response), [`URL`](https://developer.mozilla.org/en-US/docs/Web/API/URL) and more are built-in
- [`HTMLRewriter`](https://developers.cloudflare.com/workers/runtime-apis/html-rewriter/) makes it easy to transform HTML in Bun.js
- `.env` files automatically load into `process.env` and `Bun.env`
- top level await -->
