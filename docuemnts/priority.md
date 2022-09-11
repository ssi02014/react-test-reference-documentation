# 💻 쿼리 우선순위(priority)

- [RTL - priority 공식 문서](https://testing-library.com/docs/queries/about/#priority)

<br />

## Queries Accessible to Everyone

- 모두가 액세스 할 수 있는 쿼리를 사용하는게 좋다. 화면을 쳐다보고 있는 사람이든, 스크린 리더 등의 보조 기술을 사용하고 있는 사람에게건 말이다.

<br />

### 1. getByRole

- `접근성 트리`에 노출된 모든 요소를 가져오는데 사용할 수 있다. 옵션을 사용해서 액세스 가능한 이름(name)으로 반환된 요소를 필터링 할 수 잇다. `가장 선호되는 쿼리이다.👍`

```html
<button>Button</button>
```

```js
import { render, screen } from "@testing-library/react";

render(<MyComponent />);
const button = screen.getByRole("button", {
  name: "Button",
});
```

<br />

### 2. getByLabelText

- `Form` 필드에 사용하기 정말 좋은 쿼리이다. 웹 사이트의 Form을 탐색할 때 `label text`를 사용해서 요소를 찾는다.

```html
<label for="username-input">Username</label><input id="username-input" />
```

```js
import { render, screen } from "@testing-library/react";

render(<MyComponent />);
const inputNode = screen.getByLabelText("Username");
```

<br />

### 3. getByPlaceholderText

- `placeholder 값`으로 input 또는 textarea를 찾는다.

```html
<input placeholder="Username" />
```

```js
import { render, screen } from "@testing-library/react";

render(<MyComponent />);
const inputNode = screen.getByPlaceholderText("Username");
```

<br />

### 4. getByText

- `Form 외부`에서 `텍스트`를 통해 요소를 찾을 때 좋은 방법이다.

```html
<a href="/about">About ℹ️</a>
```

```js
import { render, screen } from "@testing-library/react";

render(<MyComponent />);
const aboutAnchorNode = screen.getByText(/about/i);
```

<br />

### 5. getByDisplayValue

- Form 요소(`input`, `textarea`, `select` 등)의 `현재 값`을 갖는 요소를 찾을 때 좋은 방법이다.

```js
document.getElementById("lastName").value = "Norris";
```

```js
import { render, screen } from "@testing-library/react";

render(<MyComponent />);
const lastNameInput = screen.getByDisplayValue("Norris");
```

<br />

## Semantic Queries

- Semantic Queries 쿼리들은 다소 선호되지 않는데, 그 이유는 브라우저와 보조 기술 사이의 `일관성이 다소 떨어지기 때문`이다.
- 테스트는 사용자들이 소프트웨어를 사용하는 방식을 모방해야 한다는 점을 잊지말자. 그리고 이러한 속성들이 표시되는 방식이 일관되지 못하다면 사용자들이 소프트웨어와 상호작용 하는 것과 동일한 방식으로 테스트가 진행되고 있는지를 알 수 없을 것이다.

<br />

### 1. getByAltText

- 요소가 `alt`를 지원하는 요소(img, area, input)인 경우 이를 사용해서 해당 요소를 찾을 수 있다.

```html
<img alt="Incredibles 2 Poster" src="/incredibles-2.png" />
```

```js
import { render, screen } from "@testing-library/react";

render(<MyComponent />);
const incrediblesPosterImg = screen.getByAltText(/incredibles.*? poster/i);
```

<br />

### 2. getByTitle

- title은 스크린 리더와 일관되지 않으며, 시력이 있는 사용자에게는 기본적으로 표시되지 않는다.

```html
<span title="Delete" id="2"></span>
```

```js
import { render, screen } from "@testing-library/react";

render(<MyComponent />);
const deleteElement = screen.getByTitle("Delete");
```

<br />

## Test IDs

- `최후의 수단`이다. 사용자들이 test id와 상호작용할 일은 절대 없기 때문이다.

<br />

### 1. getByTestId

- 사용자가 보거나 들을 수 없으므로 role이나 text로 일치시킬 수 없거나 의미가 없는 경우(예: 텍스트가 동적임)에만 권장

```html
<div data-testid="custom-element" />
```

```js
import { render, screen } from "@testing-library/react";

render(<MyComponent />);
const element = screen.getByTestId("custom-element");
```

<br />
