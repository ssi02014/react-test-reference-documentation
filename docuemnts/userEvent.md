# 💻 userEvent

## 📄 userEvent 이벤트 함수 종류

1. [click](#click)
2. [dbClick](#dbClick)
3. [type](#type)
4. [keyboard](#keyboard)
5. [upload](#upload)
6. [clear](#clear)
7. [selectOptions](#selectoptions)
8. [deselectOptions](#deselectOptions)
9. [tab](#tab)
10. [hover](#hover)
11. [unhover](#unhover)
12. [paste](#paste)

<br />

## userEvent

- userEvent 공식 문서
  - [userEvent 공식 Github](https://github.com/testing-library/user-event)
  - [RTL - useEvent API 공식 문서](https://testing-library.com/docs/ecosystem-user-event)

<br />

- userEvent를 프로젝트에 적용하려면 별도의 설치가 필요하다.

```
npm install --save-dev @testing-library/user-event @testing-library/dom
또는
yarn add -D @testing-library/user-event @testing-library/dom
```

- 설치 후에는 다음과 같이 userEvent를 import하고 사용하면 된다.

```js
import { render, screen } from "@testing-library/react";
import userEvent from "@testing-library/user-event";

test("버튼을 클릭하면 배경색이 빨간색으로 변경한다.", () => {
  render(<MyComponent />);
  const button = screen.getByRole("button", {
    name: "button",
  });

  // fireEvent.click(colorButton); 기존 fireEvent 코드
  userEvent.click(button);
  expect(colorButton).toHaveStyle({ backgroundColor: "red" });
});
```

<br />

## userEvent 이벤트 함수 종류

### click

- `click(element, eventInit, options)`
- `click`은 클릭 이벤트를 시뮬레이션 할 수 있다.

```js
import userEvent from "@testing-library/user-event";

test("click", () => {
  render(<MyComponent />);
  const checkBox = screen.getByRole("checkbox");

  userEvent.click(checkBox);
  expect(checkBox).toBeChecked();
});
```

<br />

### dbClick

- `dblClick(element, eventInit, options)`
- `dbClick`은 더블 클릭 이벤트를 시뮬레이션 할 수 있다.

```js
import userEvent from "@testing-library/user-event";

test("click", () => {
  render(<MyComponent />);
  const checkBox = screen.getByRole("checkbox");

  userEvent.dbClick(checkBox);
  expect(checkBox).not.toBeChecked();
});
```

<br />

### type

- `type(element, text, [options])`
- `type`은 input 또는 textarea에 text 입력을 시뮬레이션 할 수 있다.

```js
import userEvent from "@testing-library/user-event";

test("click", () => {
  render(<MyComponent />);
  const input = screen.getByRole("textbox");

  userEvent.type(input, "Hello,{enter}World!");
  expect(checkBox).toHaveValue("Hello,\nWorld");
});
```

- 위 예제에서 사용한 `{enter}`는 type에서 사용하는 특별한 문자인데 각 문자들의 종류와 의미는 [userEvent - type](https://testing-library.com/docs/ecosystem-user-event/#typeelement-text-options)를 참고하자.

<br />

- 기본적으로 type은 기존 텍스트에 추가된다. 하지만, 텍스트를 기존 텍스트 앞에 추가하려면 요소의 `선택 범위를 재설정`하면 되는데, 이때 `initialSelectionStart`와 `initialSelectionEnd` 옵션을 제공하면 된다.
- 참고로 `HTMLInputElement.setSelectionRange()`는 현재 텍스트 선택의 시작 및 끝 위치를 설정하는 메서드이다.

```js
import userEvent from "@testing-library/user-event";

test("click", () => {
  render(<input defaultValue="World!" />);
  const input = screen.getByRole("textbox");

  input.setSelectionRange(0, 0);

  userEvent.type(input, "Hello, ", {
    initialSelectionStart: 0,
    initialSelectionEnd: 0,
  });
  expect(input).toHaveValue("Hello, World!");
});
```

- `<input type="time" />`은 다음과 같이 제공한다.

```js
test("types into the input", () => {
  render(
    <>
      <label for="time">Enter a time</label>
      <input type="time" id="time" />
    </>
  );
  const input = screen.getByLabelText(/enter a time/i);

  userEvent.type(input, "13:58");
  expect(input.value).toBe("13:58");
});
```

<br />

### keyboard

- `keyboard(text, options)`
- keyboard는 키보드 이벤트를 시뮬레이션한다. 이는 userEvent.type과 유사하지만 선택 범위를 클릭하거나 변경하지는 않는다.

<br />

- 인쇄 가능한 각 문자

```js
userEvent.keyboard("foo"); // translates to: f, o, o
```

- `{`, `[`와 같은 괄호는 특수문자로 사용되며 이중화하여 참조할 수 있다.

```js
userEvent.keyboard("{{a[["); // translates to: {, a, [
```

- KeyboardEvent.key

```js
userEvent.keyboard("{Shift}{f}{o}{o}"); // translates to: Shift, f, o, o
```

- KeyboardEvent.code

```js
userEvent.keyboard("[ShiftLeft][KeyF][KeyO][KeyO]"); // translates to: Shift, f, o, o
```

- 설명자 끝에 `>`를 추가하여 누른 상태를 `유지`하고, 설명자의 시작 부분에 `/`를 추가하여 키를 `해제`할 수 있습니다.

```js
userEvent.keyboard("{Shift>}A{/Shift}"); // translates to: Shift(down), A, Shift(up)
```

- userEvent.keyboard는 같이 누르는 것도 지원한다. 예를 들어 `왼쪽 컨트롤` 키를 누른 상태에서 `a` 누르기

```js
const keyboardState = userEvent.keyboard("[ControlLeft>]"); // keydown [ControlLeft]
userEvent.keyboard("a", { keyboardState }); // press [KeyA] with active ctrlKey
```

<br />
