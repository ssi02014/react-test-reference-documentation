# 💻 RTL, Jest DOM ESLint Setting

## ESLint

- CRA(create-react-app)를 하면 ESLint는 자동으로 설치되어 있다. 그외 구성만 진행하면 되는데, 가장 먼저 ESLint 플러그인을 설치하자.
  <br />

  - eslint-plugin-testing-library: Testing Library로 테스트 코드를 작성할 때 모범사례를 따르고 일반적인 실수를 예상하는 ESLint 플러그인
  - eslint-plugin-jest-dom: jest-dom으로 테스트 코드를 작성할 때 모범사례를 따르고 일반적인 실수를 예상하는 ESLint 플러그인

<br />

```
npm install eslint-plugin-testing-library eslint-plugin-jest-dom --save-dev
또는
yarn add -D eslint-plugin-testing-library eslint-plugin-jest-dom
```

<br />

- 그리고 create-react-app을 통해 package.json에 기본적으로 ESLint 설정 부분들을 자체 파일로 분리한다.
- package.json에서 eslintConifg을 전부 제거한다.

<br />

```json
// package.json
{
  // 아래 내용 제거
  "eslintConfig": {
    "extends": ["react-app", "react-app/jest"]
  }
}
```

<br />

- 그리고 새로운 `.eslintrc` 파일을 생성 후 아래와 같이 코드를 작성한다.
- `.eslintrc` 파일은 src와 같은 depth에 생성한다.

<img width="308" alt="스크린샷 2022-07-24 오후 7 29 39" src="https://user-images.githubusercontent.com/64779472/180643011-99c47303-bc84-4cc1-8afa-da371f730fcd.png">

<br />

```js
module.exports = {
  env: {
    browser: true,
    es2021: true,
    node: true,
  },
  plugins: ["testing-library", "jest-dom"],
  extends: [
    "react-app",
    "react-app/jest",
    "plugin:testing-library/react",
    "plugin:jest-dom/recommended",
  ],
};
```

<br />

- 작성하면 App.test.js파일에서 다음 코드에서 에러가 발생한다. DOM 속성에대한 단언대신 `toHaveTextContent`를 사용하라는 에러 메세지이다.

```js
// App.test.js
expect(colorButton.textContent).toBe("Change to Medium Violet Red");
/* 
  Use toHaveTextContent instead of asserting on DOM node attributes
  (eslintjest-dom/prefer-to-have-text-content)
  */
```

- 위 에서 메시지는 아래와 같이 수정하면 해결되며, 실제로 테스트를 실행해봐도 테스트가 통과하는 것을 확인할 수 있다.

```js
// App.test.js
expect(colorButton).toHaveTextContent("Change to Medium Violet Red");
```

<br />
