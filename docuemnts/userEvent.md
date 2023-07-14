# 💻 userEvent

## 📄 userEvent 이벤트 함수 종류

1. [click](#click)
2. [dbClick](#dbclick)
3. [tripleClick](#tripleclick)
4. [type](#type)
5. [clear](#clear)
6. [selectOptions](#selectoptions)
7. [deselectOptions](#deselectoptions)
8. [keyboard](#keyboard)
9. [upload](#upload)
10. [tab](#tab)
11. [hover/unhover](#hover-unhover)

<br />

## userEvent

- userEvent 공식 문서

  - [userEvent 공식 Github](https://github.com/testing-library/user-event)
  - [RTL - useEvent API 공식 문서](https://testing-library.com/docs/user-event/intro)

- userEvent를 프로젝트에 적용하려면 `별도의 설치`가 필요하다.

```
npm install --save-dev @testing-library/user-event
또는
yarn add -D @testing-library/user-event
```

<br />

## Setup

- userEvent v14부터는 `userEvent.setup()` 를 통해 userEvent의 `인스턴스`를 구성할 수 있도록 한다.
- 생성된 인스턴스의 메서드는 어떤 키를 눌렀는지와 같은 `하나의 입력 장치 상태`를 공유합니다. 이를 통해 실제 사용자가 설명한 상호 작용처럼 동작하는 여러 개의 연속적인 상호작용을 작성할 수 있다.
- setup에는 다양한 옵션이 들어갈 수 있다. 아래 문서를 참고하자.
- [user-event setup options](https://testing-library.com/docs/user-event/options)

```ts
setup(options?: Options): UserEvent
```

- 아래와 같이 test파일에서 userEvent를 import하고, `userEvent.setup()`을 통해 인스턴스를 생성 후 해당 인스턴스로 테스트를 진행한다.

```js
import { render, screen } from "@testing-library/react";
import userEvent from "@testing-library/user-event";

const event = userEvent.setup(); // default

test("버튼을 클릭하면 배경색이 빨간색으로 변경한다.", async () => {
  render(<MyComponent />);
  const button = screen.getByRole("button", {
    name: "button",
  });

  await event.click(button);
  expect(colorButton).toHaveStyle({ backgroundColor: "red" });
});
```

<br />

## userEvent 주요 이벤트 종류

### click

- `click`은 클릭 이벤트를 시뮬레이션 한다.

```ts
click(element: Element): Promise<void>
```

```js
import React from "react";
import { render, screen } from "@testing-library/react";
import userEvent from "@testing-library/user-event";

const event = userEvent.setup();

test("click", async () => {
  render(<input id="checkbox" type="checkbox" />);
  const checkBox = screen.getByRole("checkbox");

  await event.click(checkBox);
  expect(checkBox).toBeChecked();
});
```

<br />

### dbClick

- `dbClick`은 더블 클릭 이벤트를 시뮬레이션 한다.

```ts
dblClick(element: Element): Promise<void>
```

```js
import React from "react";
import { render, screen } from "@testing-library/react";
import userEvent from "@testing-library/user-event";

const event = userEvent.setup();

test("dbClick", async () => {
  render(<input id="checkbox" type="checkbox" />);
  const checkBox = screen.getByRole("checkbox");

  await event.dbClick(checkBox);
  expect(checkBox).not.toBeChecked();
});
```

<br />

### tripleClick

- `dbClick`은 트리플 클릭 이벤트를 시뮬레이션 한다.

```ts
tripleClick(element: Element): Promise<void>
```

```js
import React from "react";
import { render, screen } from "@testing-library/react";
import userEvent from "@testing-library/user-event";

const event = userEvent.setup();

test("tripleClick", async () => {
  render(<input id="checkbox" type="checkbox" />);
  const checkBox = screen.getByRole("checkbox");

  await event.tripleClick(checkBox);
  expect(checkBox).toBeChecked();
});
```

<br />

### type

- type은 `<input>` 또는 `<textarea>` 에 text 입력을 시뮬레이션 한다.

```ts
type(
  element: Element
  text: KeyboardInput
  options?: {
      skipClick?: boolean // 해당 값이 true이면 입력하기 전에 요소를 클릭한다.
      skipAutoClose?: boolean // 해당 값이 true이면 눌렀던 키를 모두 놓는다.
      initialSelectionStart?: number // initialSelectionStart을 통해 선택 영역을 설정한다.
      initialSelectionEnd?: number // initialSelectionEnd이 설정되어 있지 않으면 선택 영역이 접힌다.
  }
): Promise<void>
```

```js
import React from "react";
import { render, screen } from "@testing-library/react";
import userEvent from "@testing-library/user-event";

const event = userEvent.setup();

test("type", async () => {
  render(<input defaultValue="Hello," />);
  const input: HTMLInputElement = screen.getByRole("textbox");

  await event.type(input, " World!");

  expect(input).toHaveValue("Hello, World!");
});
```

- `type`은 항상 입력하기 전에 요소를 클릭합니다. 이 기능을 비활성화하려면 `skipClick` 옵션을 `true`로 설정하면 된다.

```tsx
await event.type(input, " World!", { skipClick = true });
```

<br />

- type은 여러 특수 문자들을 지원한다. 예를 들어 `{enter}`, `{space}`, `{esc}`, `{backspace}` 등등 이러한 특수문자들을 확인하고 싶다면 다음 문서를 참고하자.
- [type-special-characters](https://testing-library.com/docs/ecosystem-user-event/#special-characters)

<br />

- 기본적으로 type은 기존 텍스트에 추가된다. 하지만, 텍스트를 기존 텍스트 앞에 추가하려면 요소의 `선택 범위를 재설정`하면 되는데, 이때 `initialSelectionStart`와 `initialSelectionEnd` 옵션을 제공하면 된다.
- 참고로 `HTMLInputElement.setSelectionRange()`는 현재 텍스트 선택의 시작 및 끝 위치를 설정하는 메서드이다.

```js
import React from "react";
import { render, screen } from "@testing-library/react";
import userEvent from "@testing-library/user-event";

const event = userEvent.setup();

test("type", async () => {
  render(<input defaultValue="World!" />);
  const const input: HTMLInputElement = screen.getByRole("textbox");

  input.setSelectionRange(0, 0);

  await event.type(input, "Hello, ", {
    initialSelectionStart: 0,
    initialSelectionEnd: 0,
  });
  expect(input).toHaveValue("Hello, World!");
});
```

<br />

### clear

- clear는 `<input>` 또는 `<textarea>` 내부의 텍스트를 삭제를 시뮬레이셔 한다.

```ts
clear(element: Element): Promise<void>
```

```tsx
import React from "react";
import { render, screen } from "@testing-library/react";
import userEvent from "@testing-library/user-event";

const event = userEvent.setup();

test("clear", async () => {
  render(<input defaultValue="Hello, World!" />);

  const input: HTMLInputElement = screen.getByRole("textbox");

  await event.clear(input);
  expect(input).toHaveValue("");
});
```

<br />

### selectOptions

- selectOptions는 `<select>` 또는 `<select multiple>`의 지정된 `option`을 선택을 시뮬레이션 한다.

```ts
selectOptions(
  element: Element,
  values: HTMLElement | HTMLElement[] | string[] | string,
): Promise<void>
```

```tsx
import React from "react";
import { render, screen } from "@testing-library/react";
import userEvent from "@testing-library/user-event";

const event = userEvent.setup();

test("selectOptions", async () => {
  render(
    <select multiple>
      <option value="1">A</option>
      <option value="2">B</option>
      <option value="3">C</option>
    </select>
  );

  const select = screen.getByRole("listbox");

  await event.selectOptions(select, ["1", "3"]);

  expect(screen.getByRole("option", { name: "A" }).selected).toBe(true);
  expect(screen.getByRole("option", { name: "B" }).selected).toBe(false);
  expect(screen.getByRole("option", { name: "C" }).selected).toBe(true);
});
```

<br />

### deselectOptions

- deselectOptions `<select multiple>`의 지정된 `option`을 선택 항목에서 제거를 시뮬레이션 한다.

```ts
deselectOptions(
  element: Element,
  values: HTMLElement | HTMLElement[] | string[] | string,
): Promise<void>
```

```tsx
import React from "react";
import { render, screen } from "@testing-library/react";
import userEvent from "@testing-library/user-event";

const event = userEvent.setup();

test("deselectOptions", async () => {
  render(
    <select multiple>
      <option value="1">A</option>
      <option value="2">B</option>
      <option value="3">C</option>
    </select>
  );

  const select = screen.getByRole("listbox");

  await event.selectOptions(select, "2"); // 2 선택
  expect(screen.getByText("B").selected).toBe(true);

  await event.deselectOptions(select, "2"); // 2 선택 해제
  expect(screen.getByText("B").selected).toBe(false);

  // 또는 한번에 여러 동작을 수행할 수도 있다.
  // await event.deselectOptions(select, ['1', '2'])
});
```

<br />

### keyboard

```ts
keyboard(input: KeyboardInput): Promise<void>
```

- keyboard는 `키보드 이벤트`를 시뮬레이션 한다. 이는 userEvent.type과 유사하지만 `선택 범위를 클릭하거나 변경하지는 않는다.`

<br />

1. 인쇄 가능한 각 문자

```js
await event.keyboard("foo"); // translates to: f, o, o
```

2. `{`, `[`와 같은 괄호는 특수 문자로 사용되며, `이중화`하여 참조할 수 있다.

```js
await event.keyboard("{{a[["); // translates to: {, a, [
```

3. KeyboardEvent.key

- 참고로 아래와 같이 하면 어떤 키도 누른 상태로 유지되지 않습니다. 따라서 f를 누르기 전에 Shift 키가 해제된다.

```js
await event.keyboard("{Shift}{f}{o}{o}"); // translates to: Shift, f, o, o
```

4. KeyboardEvent.code

```js
await event.keyboard("[ShiftLeft][KeyF][KeyO][KeyO]"); // translates to: Shift, f, o, o
```

5. 설명자 끝에 `>`를 추가하여 누른 상태를 `유지`하고, 설명자의 시작 부분에 `/`를 추가하여 키를 `해제`할 수 있다.

```js
await event.keyboard("{Shift>}A{/Shift}"); // translates to: Shift(down), A, Shift(up)
```

<br />

### upload

- upload는 `<input>`에 file을 업로드 이벤트를 시뮬레이션 한다.
- 여러 파일을 업로드하려면 input 태그에 `multiple` 속성과 upload의 두 번째 인자인 file을 `배열`로 설정하면 된다. upload의 세 번째 인자를 사용하여 클릭 또는 변경 이벤트를 초기화 하는 것도 가능하다.

```ts
upload(
  element: HTMLElement,
  fileOrFiles: File | File[],
): Promise<void>
```

```js
import React from "react";
import { render, screen } from "@testing-library/react";
import userEvent from "@testing-library/user-event";

const event = userEvent.setup();

// 단일 파일 업로드
test("upload file", async () => {
  render(
    <div>
      <label htmlFor="file-uploader">Upload file:</label>
      <input id="file-uploader" type="file" />
    </div>
  );

  const file = new File(["hello"], "hello.png", { type: "image/png" });
  const input: HTMLInputElement = screen.getByLabelText(/upload file/i);

  await event.upload(input, file);

  expect(input.files?.[0]).toStrictEqual(file);
  expect(input.files?.item(0)).toStrictEqual(file);
  expect(input.files).toHaveLength(1);
});

// 멀티 파일 업로드
test("upload multiple files", async () => {
  render(
    <div>
      <label htmlFor="file-uploader">Upload file:</label>
      <input id="file-uploader" type="file" multiple /> {/* multiple */}
    </div>
  );
  // files 배열로 설정
  const files = [
    new File(["hello"], "hello.png", { type: "image/png" }),
    new File(["there"], "there.png", { type: "image/png" }),
  ];
  const input: HTMLInputElement = screen.getByLabelText(/upload file/i);

  await event.upload(input, files);

  expect(input.files).toHaveLength(2);
  expect(input.files?.[0]).toStrictEqual(files[0]);
  expect(input.files?.[1]).toStrictEqual(files[1]);
});
```

<br />

### tab

- tab은 브라우저와 동일한 방식으로 `document.activeElement`를 변경하는 탭 이벤트를 시뮬레이션 한다.

```ts
tab(options: {shift?: boolean}): Promise<void>
```

```tsx
import React from "react";
import { render, screen } from "@testing-library/react";
import userEvent from "@testing-library/user-event";

const event = userEvent.setup();

test("tab", async () => {
  render(
    <div>
      <input data-testid="element" type="checkbox" />
      <input data-testid="element" type="radio" />
      <input data-testid="element" type="number" />
    </div>
  );

  const [checkbox, radio, number] = screen.getAllByTestId("element");

  expect(document.body).toHaveFocus();

  await event.tab();

  expect(checkbox).toHaveFocus();

  await event.tab();

  expect(radio).toHaveFocus();

  await event.tab();

  expect(number).toHaveFocus();

  await event.tab();

  // 다시 본문으로 돌아온다
  expect(document.body).toHaveFocus();

  await event.tab();

  expect(checkbox).toHaveFocus();
});
```

<br />

### hover unhover

- hover는 요소 위로 마우스를 가져가는 `hover`를 시뮬레이션 한다.

```ts
hover(element: Element): Promise<void>
```

- unhover는 요소 밖으로 마우스를 빼는 `unhover`를 시뮬레이션 한다.

```ts
unhover(element: Element): Promise<void>
```

```tsx
import React, { useState } from "react";
import { render, screen } from "@testing-library/react";
import userEvent from "@testing-library/user-event";
import Tooltip from "../tooltip";

const event = userEvent.setup();

test("hover", async () => {
  const [isOpen, setIsOpen] = useState(true);
  const messageText = "Hello";

  render(
    <Tooltip messageText={messageText}>
      <TrashIcon aria-label="Delete" />
    </Tooltip>
  );

  await event.hover(screen.getByLabelText(/delete/i));
  expect(screen.getByText(messageText)).toBeInTheDocument();

  await event.unhover(screen.getByLabelText(/delete/i));
  expect(screen.queryByText(messageText)).not.toBeInTheDocument();
});
```

<br />
