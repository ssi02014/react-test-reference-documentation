# 💻 Jest Matchers

## 🧑‍💻 Jest 단언(assert)

- `단언(assert)`는 테스트의 통과 여부를 결정한다. 따라서 테스트 함수의 핵심 부분이다.

```js
expect(linkElement).toBeInTheDocument();
```

- 단언(assert)은 Jest 전역 메서드인 `expect()` 메서드로 시작한다. expect를 이용해서 다양한 항목의 유효성을 검사할 수 있는 여러 `matcher`에 액세스할 수 있다.
- Jest는 `matcher`를 사용하여 다양한 방식으로 값을 테스트할 수 있습니다.
- 위 예제에서 `toBeInTheDocument()`는 Jest DOM에서 사용하는 `매처(matcher)`이다.
- matcher에는 종종 인수가 있는데 위 예제에서 toBeInTheDocument는 인수를 가지지 않는다.
- toBe 같은 경우가 인수를 넣어서 테스트를 진행하는 matcher이다.

<br />

## 🧑‍💻 기본 Jest Matchers

### toBe

- toBe는 숫자나 문자열을 검증하고자 할 때 사용하는 Matcher이다.

```js
const fn = {
  add: (num1, num2) => num1 + num2,
};

test("3 더하기 5는 8입니다.", () => {
  expect(fn.add(3, 5)).toBe(8);
});
```

<br />

### toEqual, toStrictEqual

- toEqual은 toBe와 비슷하지만 다르다. 숫자와 문자열을 검증할 때는 둘은 동일하게 동작하지만 객체를 검증할 때는 다르게 동작한다.
- toEqual은 재귀적으로 객체를 돌면서 각 value가 같은지 확인한다.
- toStrictEqual은 toEqual보다 더 엄격하게 검증한다.

```js
const fn = {
  makeUser: (name, age) => ({ name, age }),
};

test("이름과 나이를 받아서 객체를 반환해줍니다.", () => {
  expect(fn.makeUser("Loki", 1050)).toEqual({ name: "Loki", age: 1050 });
});
```

<br />

### toBeNull, toBeUndefined, toBeDefined

- toBeNull은 오직 null과 같은지를 확인한다 (=== null)
- toBeUndefined는 오직 undefined와 같은지를 확인한다 (=== undefined)
- toBeDefined는 undefined가 아닌지를 확인한다. 즉, toBeUndefined 반대 개념

```js
test("null은 null입니다.", () => {
  expect(null).toBeNull();
});

test("undefined는 undefined입니다.", () => {
  expect(undefined).toBeUndefined();
});

test("undefined의 반대는 defined입니다.", () => {
  expect("Thor").toBeDefined();
});
```

<br />

### toBeTruthy, toBeFalsy

- toBeTruthy는 if문이 true라고 받아들일 값인지 검증하는 Matcher이다.
- toBeFalsy는 if문이 false라고 받아들일 값인지 검증하는 Matcher이다.
- 참고로 null, 빈 문자열, 0은 모두 false라고 판단한다. 반대로 0이 아닌 숫자, 문자열, 빈 배열 등은 true라고 판단한다.

```js
const fn = {
  add: (num1, num2) => num1 + num2,
};

test("0은 false입니다.", () => {
  expect(fn.add(1, -1)).toBeFalsy();
});

test("비어있지 않은 문자열은 true입니다.", () => {
  expect(fn.add("디즈니 플러스", "런칭 언제 할까요..")).toBeTruthy();
});
```

<br />

### toBeGreaterThan, toBeGreaterThanOrEqual, toBeLessThan, toBeLessThanOrEqual

- toBeGreaterThan는 A가 B보다 초과하는지 검증하는 Matcher이다. (A > B)
- toBeGreaterThanOrEqual는 A가 B보다 이상인지 검증하는 Matcher이다. (A >= B)
- toBeLessThan는 A가 B보다 미만인지 검증하는 Matcher이다.(A < B)
- toBeLessThanOrEqual는 A가 B보다 이하인지 검증하는 Matcher이다.(A <= B)

```js
test("message는 10글자 이하여야 합니다.", () => {
  const message = "디즈니 플러스 런칭";
  expect(message.length).toBeLessThanOrEqual(10);
});
```

<br />

### toBeCloseTo

- 자바스크립트에서 0.1 더하기 0.2는 0.3이 아니다. 이는 몇몇 프로그래밍 언어에서 발생하는 현상으로 이진법으로 변환하는 과정에서 무한 소수가 발생하기 때문이다.
- toBeCloseTo는 `근사치`인지 검증하는 Matcher이다.

```js
const fn = {
  add: (num1, num2) => num1 + num2,
};

test("0.1 더하기 0.2는 0.3입니다.", () => {
  expect(fn.add(0.1, 0.2)).toBe(0.3);
}); // 실패

test("0.1 더하기 0.2는 0.3입니다.", () => {
  expect(fn.add(0.1, 0.2)).toBeCloseTo(0.3);
}); // 성공
```

<br />

### toMatch

- toMatch는 `정규식 표현`을 사용하여 문자열을 검증하는 Matcher이다.

```js
test("Thor: Love and Thunder에는 t가 있나요?", () => {
  expect("Thor: Love and Thunder").toMatch(/t/i);
});
```

<br />

### toContain

- toContain는 `배열 내에 요소가 포함`되어 있는지 검증하는 Matcher이다.

```js
test("user list에 Thor가 있나요?", () => {
  const user = "Thor";
  const userList = ["Thor", "Loki"];
  expect(userList).toContain(user);
});
```

<br />

### toThrow

- toThrow는 `에러 발생 여부`를 검증하는 Matcher이다. 특징으로 인수가 따로 존재하지 않으면 어떤 종류의 에러든 발생하면 통과한다.
- 특정 에러인지 확인하고 싶다면 toThrow의 인수로 에러의 종류를 전달하면 된다.
- 아래 코드는 단순히 에러 발생 여부만을 검증하는 테스트 코드이다.

```js
const fn = {
  throwErr: () => {
    throw new Error();
  },
};

test("에러가 발생하나요?", () => {
  expect(() => fn.throwErr()).toThrow();
});
```

<br />

- 아래 코드는 에러에 인수를 전달하여 특정 에러를 검증하는 테스트 코드이다.

```js
const fn = {
  throwErr: () => {
    throw new Error("type2");
  },
};

const fn = require("../fn");

test("에러가 발생하나요?", () => {
  expect(() => fn.throwErr()).toThrow("type2");
});
```

<br />

## 🧑‍💻 Jest DOM Matchers

- (추가 예정)
