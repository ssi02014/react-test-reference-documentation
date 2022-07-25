# 💻 Screen Query

## Screen Query

- screen query 관련한 공식 문서는 아래 2개를 참고한다.
  - [RTL - About Query 공식 문서](https://testing-library.com/docs/queries/about/)
  - [RTL - cheatsheet 공식 문서](https://testing-library.com/docs/react-testing-library/cheatsheet/)

<br />

- screen query는 기본적으로 다음과 같은 포맷을 가진다.

<br />

```
command[All]ByQueryType
```

<br />

### command

- get
  - 요소가 DOM 내에 있을 것을 예상한다.
- query
  - 요소가 DOM 내에 있지 않을 것을 예상한다.
- find
  - 요소가 `비동기`적으로 나타날 경우를 예상한다.
  - find는 `get`과 `waitFor`의 조합이다. 즉, `Promise`를 반환하는 것을 제외하면 get과 동일하다.
  - waitFor 메서드를 감싸는 형태로 되어 있어서 에러를 던지지 않을 때까지 일정 시간동안 계속 검색을 재시도한다.

<br />

### [All]

- [All]은 포함을 시키거나 포함을 시키지 않는 부분인데 다음과 같다.
  - (exclude): 하나의 match만을 expect한다. ex) `getByRole`
  - (include): 하나 이상의 match를 expect한다. (배열로 반환) ex) `getAllByText`

<br />

### QueryType

- QueryType은 무엇으로 검색을 하는지를 의미하는데, 다음과 같다.

  - Role (most preferred): 코드의 `접근성`을 보장하기위해 가장 선호된다.
  - AltText (images): `이미지`를 찾기 위해 사용한다.
  - Text (display elements): 특정 역할이 없고 `비상호작용적인 디스플레이 요소`에 사용한다.
  - TestId: 최후의 선택, `data-testid` 속성으로 요소를 찾는다.
  - Form elements: Form 요소를 찾는 데에는 다양한 속성의 사용이 가능하다.
    - placeholderText
    - LabelText
    - DisplayValue
  - 위에 내용들은 혼합해서 가장 적절한 방법으로 DOM에서 찾고자하는 요소를 찾아낼 수 있다.

```
getByRole
getAllByText
QueryAllByLabelText
```

- 참고로 어떤 쿼리를 사용해야 할지는 [쿼리 우선순위](https://github.com/ssi02014/React-Test-Documents-To-Reference/blob/master/docuemnts/priority.md)를 참고하자 👍

<br />

### 쿼리 비교 표

![스크린샷 2022-07-24 오전 1 24 59](https://user-images.githubusercontent.com/64779472/180613880-835f6266-0348-4ab3-9e14-3870dc5526c0.png)

- 위 표를 보면 getBy는 매치하는게 없으면 에러를 발생하고, queryBy는 에러를 발생하지 않고 null을 리턴한다. 이런식으로 표를 참고하면 된다.

<br />

### 예제

```html
<button type="submit" disabled>submit</button>
```

```js
import { screen } from "@testing-library/react";

const button = screen.getByRole("button", {
  name: "submit",
});
expect(button).toBeDisabled();
```
