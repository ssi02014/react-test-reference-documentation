# 💻 Screen Query

## Screen Query

- screen query 관련한 공식 문서는 아래 2개를 참고한다.
  - [RTL - About Query 공식 문서](https://testing-library.com/docs/queries/about/)
  - [RTL - cheatsheet 공식 문서](https://testing-library.com/docs/react-testing-library/cheatsheet/)

<br />

- screen query는 기본적으로 다음과 같은 기본 포맷을 가진다.

<br />

```
command[All]ByQueryType
```

<br />

### command

- get
  - get은 어떠한 요소도 매칭되지 않으면 `에러를 발생`시킨다. 따라서, 찾고자 하는 요소가 DOM 내에 있을 것을 예상할 때 사용한다.
- query
  - query는 어떠한 요소도 매칭되지 않을 때 에러를 발생하는 것이 아닌 `null을 반환`한다. 따라서, 찾고자하는 요소가 DOM 내에 있지 않을 것을 예상할 때 사용한다.
- find
  - 찾고자하는 요소가 `비동기`적으로 나타날 경우를 예상한다.
  - find는 `get`과 `waitFor`의 조합이다. 즉, `Promise`를 반환하는 것을 제외하면 get과 동일하다.
  - waitFor 메서드를 감싸는 형태로 되어 있어서 에러를 던지지 않을 때까지 일정 시간동안 계속 검색을 재시도한다.

<br />

### [All]

- [All]은 포함을 시키거나 포함을 시키지 않는 부분인데 다음과 같다.
  - (exclude): 하나의 match만을 expect한다. ex) `getByRole`
  - (include): 하나 이상의 match를 expect하며, 배열을 반환한다. ex) `getAllByText`

<br />

### QueryType

- QueryType은 무엇으로 검색을 하는지를 의미하는데, 다음과 같다.

  - Role (most preferred): 코드의 `접근성`을 보장하기위해 `가장 선호`된다.
  - AltText (images): `이미지`를 찾기 위해 사용한다.
  - Text (display elements): 특정 역할이 없고 `비상호작용적인 디스플레이 요소`에 사용한다.
  - TestId: 최후의 선택, `data-testid` 속성으로 요소를 찾는다.
  - Form elements: Form 요소를 찾는 데에는 다양한 속성의 사용이 가능하다.
    - placeholderText : `placeholder 값`으로 input 또는 textarea를 찾는다.
    - LabelText: Form을 탐색할 때 `label text`를 사용해서 요소를 찾는다.
    - DisplayValue: Form 요소(`input`, `textarea`, `select` 등)의 `현재 값`을 갖는 요소를 찾을 때 좋은 방법이다.

- 위에 내용들을 참고해서 가장 적절한 방법으로 DOM에서 찾고자하는 요소를 찾아내면 된다. 또한, 위 내용이 우선 순위는 아니다 우선 순위는 아래 문서를 참고하자. 👍
  - [쿼리 우선순위](https://github.com/ssi02014/React-Test-Documents-To-Reference/blob/master/docuemnts/priority.md)

<br />

### 쿼리 비교 표

![스크린샷 2022-07-24 오전 1 24 59](https://user-images.githubusercontent.com/64779472/180613880-835f6266-0348-4ab3-9e14-3870dc5526c0.png)

- 위 표를 보면 getBy는 매칭되는게 없으면 `에러를 발생`하고, queryBy는 에러를 발생하지 않고 `null을 리턴`한다.
- 에러를 발생 여부와 어떤 값을 리턴하는지를 파악하고 적절히 사용하면 된다.

<br />

### 예제1 (get)

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

### 예제2 (query)

```html
<h1>타이틀</h1>
```

```js
import { screen } from "@testing-library/react";

// query
const button = screen.queryByRole("button", {
  name: "submit",
});

// get
const header = screen.getByRole("heading", {
  name: "타이틀",
});

expect(button).not.toBeInTheDocument();
expect(header).toBeInTheDocument();
```

### 예제3 (find)

```jsx
const App = () => {
  const [data, setData] = useState(null);

  const getApi = async () => {
    const res = await axios.get(/* ~~ */);
    setData(res.data);
  };

  useEffect(() => {
    getApi(); // 비동기 함수 호출
  }, []);
};
```
