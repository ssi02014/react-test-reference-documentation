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

- (추가 예정)

<br />
