# 💻 리액트 테스트 참고 저장소

- Jeat, React Testing Library 등 리액트 테스트 관련 참고 저장소입니다.
- Jest, RTL 각각의 개념을 자세히 알아보기보다는 자주 확인하게되는 쿼리 우선순위, Screen Query, matchers 종류 등을 보기 쉽게 정리한 저장소임을 알립니다.
- (현재 진행중)

<br />

## 📄 목차

1. [React Testing Library](#react-testing-library)
2. [대표적인 테스트 유형](#테스트-유형)
3. [RTL과 접근성](#rtl과-접근성)
4. [쿼리 우선순위](#쿼리-우선순위)
5. [getByRole 역할의 종류](#역할의-종류)
6. [Screen Query](#screen-query)
7. [Jest Matchers](#jest-matchers)

- etc.
  - [React Testing Library, jest-dom ESLint Setting](https://github.com/ssi02014/React-Test-Documents-To-Reference/blob/master/docuemnts/eslint.md)

<br />
<br />

## React Testing Library

### React Testing Library 사용 이유

- RTL(React Testing Library)는 `테스트를 위한 가상 DOM을 생성`하고 생성한 DOM과 상호 작용하기 위한 `유틸리티`도 제공한다. 예를 들어, DOM에서 요소를 찾거나 클릭과 같이 요소와 상호 작용할 수 있다. 또한, 브라우저 없이 테스트도 가능하다.

<br />

### 주의 사항

- RTL을 처음 접할 때면 RTL을 Jest의 대안으로 혼동하는 경우가 더러 있다.
- 두 도구는 React 내에서 테스트를 진행할 때 같이 사용되기에 상호 보완 관계라고 볼 수 있다. (엄밀히 말하자면, RTL이 Jest를 포함하는 구조)
- 전반적으로 Jest를 통해 기능 테스트를 진행할 수는 있지만, React의 컴포넌트를 렌더링하고 테스트하기 위해서는 몇 가지 기능이 더 필요하다.
  - Jest - 자체적인 `test runner`와 `test util` 제공
  - RTL - `Jest + React 컴포넌트 test util` 제공

<br />

## 테스트 유형

- 유닛(Unit) 테스트

  - 보통 함수나 별개의 React 컴포넌트 코드의 한 `유닛 혹은 단위를 테스트`한다.
  - 유닛 테스트의 특징은 다른 코드의 유닛과 상호 작용하는 것은 테스트하지 않는다.

- 통합(Intergration) 테스트

  - 여러 유닛이 함께 동작하는 방식을 테스트해서 `유닛 간의 상호 작용`을 테스트하는 것이다. 예를 들어 컴포넌트 간의 상호 작용을 테스트 하거나 마이크로 서비스 간의 상호 작용을 테스트한다.

- 기능(functional) 테스트

  - 소프트웨어의 `특정 기능`을 테스트하는 것이다.
  - 헷갈릴 수 있는게 function은 프로그래밍 언어에서 입력값을 취하고 출력을 제공하는 소프트웨어 단위(Unit)의 함수를 의미할 수도 있고, `소프트웨어의 동작`을 의미할 수도 있는데 이 경우에는 특정 코드 함수가 아닌 소프트웨어 동작에 해당한다. 즉, 기능 테스트의 의미는 코드가 아닌 `동작`을 테스트하는 것이다.

- 인수(Acceptance) 테스트 혹은 End to End (E2E) 테스트

  - 실제 사용자 환경에서 사용자의 입장으로 테스트를 수행하는 것을 의미한다.
  - 실제 `브라우저`가 필요하고 애플리케이션이 연결된 `서버`가 필요하다. 보통 `Cypress`나 `Selenium`과 같은 도구가 필요하다. 참고로 이 테스트는 `RTL을 위해 설계된 테스트는 아니다.`

<br />

- ⭐️ RTL은 유닛 테스트보다 `기능 테스트를 권장`한다. 즉 실제 사용자 경험과 유사한 방식의 테스트를 작성할 것을 권장하는 것이다. 예를 들어 `<div>Hello World</div>`라는 코드가 있다면, RTL은 div 태그를 사용하는지보다 `Hello World` 메시지가 브라우저에 노출이 되는지 파악하는 것을 더 중요하다고 본다.
- 기능에 초점을 맞춘 테스트 방식은 신뢰도를 높임과 동시에 코드 리팩토링 시 테스트 코드 수정 빈도를 줄일 수 있습니다.

<br />

## RTL과 접근성

- RTL은 우리의 웹사이트가 `어떻게 사용되는지 최대한 가깝게` 테스트를 작성할 수 있도록 장려하는 메서드와 유틸리티를 제공한다.
- 테스트 할 때 실제 사용자가 쓰는 것처럼 해야 하는데 여기에 `스크린 리더`와 같은 `접근성 인터페이스`도 포함된다.
- 접근성을 준수하기위해 어떤 쿼리를 우선순위로 사용해야되는 지는 아래를 참고하자.

<br />

## 쿼리 우선순위

- 아래 문서를 통해 가상 DOM에서 요소를 찾을 때 어떤 쿼리를 우선 순위로 사용해야 하는지 참고하자.
- [쿼리 우선순위(priority)](https://github.com/ssi02014/React-Test-Documents-To-Reference/blob/master/docuemnts/priority.md)

<br />

## 역할의 종류

- [쿼리 우선순위(priority)](https://github.com/ssi02014/React-Test-Documents-To-Reference/blob/master/docuemnts/priority.md)를 봤으면 `getByRole`를 사용하는 방법 즉, `역할`을 통해 요소를 찾을 수 있고, 실제로 `스크린 리더에서 액세스 할 수 있다.` 따라서 접근성을 보장하기때문에 요소를 찾을때 `가장 선호되는 방법`이다.
- getByRole이 어떤 역할을 갖고 있는지 확인하려면 아래 문서를 확인하자.
- [getByRole 역할 종류](https://www.w3.org/TR/wai-aria/#role_definitions)

<br />

- role 속성을 사용해서 `div`처럼 모든 요소에 역할을 추가할 수 있다. 코드에는 단순히 `role=""`처럼 큰따옴표로 역할을 묶으면 된다.
- 일반적으로 스크린 리더에서 테스트 요소를 찾을 수 없으면, 그건 우리의 앱이 스크린 리더에 친화적이지 않은 거고 접근성에서 안좋다는 의미이다.

```html
<div role="textbox"></div>
```

```js
const textbox = screen.getByRole("textbox");

expect(textbox).toBeEmptyDOMElement();
```

<br />

## Screen Query

- Screen Query는 페이지에서 `요소를 찾기 위해` Testing Library가 제공하는 방법입니다.
- 아래 문서를 통해 Screen Query에대해 알아보자.
- [Screen Query](https://github.com/ssi02014/React-Test-Documents-To-Reference/blob/master/docuemnts/screen-query.md)

```html
<button type="submit" disabled>submit</button>
```

```js
import { screen } from "@testing-library/react";

// getByRole screen query 사용
const button = screen.getByRole("button", {
  name: "submit",
});

expect(button).toBeDisabled();
```

<br />

## Jest Matchers

- Jest는 `matcher`를 사용하여 다양한 방식으로 값을 테스트할 수 있습니다.
- 아래 문서를 통해 다양한 Jest의 Matcher를 알아보자
- [Jest Matchers](https://github.com/ssi02014/React-Test-Documents-To-Reference/blob/master/docuemnts/jest-matchers.md)

```html
<input type="text" />
```

```js
const input = screen.getByRole("textbox");

expect(input).toBeInTheDocument(); // toBeInTheDocument matcher 사용
```

<br />
