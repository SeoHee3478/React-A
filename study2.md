# 2주차 React 스터디 정리

| 장  | 제목                |
| --- | ------------------- |
| 4장 | 이벤트 핸들링       |
| 5장 | ref.DOM 에 이름달기 |

---

## 4장: 이벤트 핸들링

### `HTML` vs `React` (`onClick`)

우선 이 코드를 먼저 보고 갑시다!

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width" />
    <title>Title</title>
  </head>
  <body>
    <button onclick="alert('알러트입니다')">Click</button>
  </body>
</html>
```

기존의 `HTML` 에서 `DOM` 요소에 접근을 할 때는 위의 코드와 같이 `" "` 사이에 자바스크립트 코드를 넣거나 혹은 같은 맥락의 `addEventListener` 를 통해 이벤트 핸들링을 하곤 했습니다.
`리액트` 에서의 이벤트핸들링은 비슷하면서도 살짝 다른데요,, 밑의 코드를 보겠습니다!

위의 `HTML` 코드와 다른 점을 느끼셨나요?
크게 두 가지 다른 점을 느낄 수 있습니다.

```javascript
import React, { useState } from "react";

const Greeting = () => {
  const [message, setMessage] = useState("");
  const onClickHi = () => {
    setMessage("하이염");
  };
  const onClickBye = () => {
    setMessage("빠이염");
  };
  return (
    <div>
      <button onClick={onClickHi}>Hi</button>
      <button onClick={onClickBye}>Bye</button>
      <h1>{message}</h1>
    </div>
  );
};

export default Greeting;
```

1. `camelCase`
   저번주에서도 다뤘던 내용이지만 리액트에서는 이벤트 이름, 자바스크립트에 이미 존재하는 단어들을 카멜 케이스로 작성합니다.
2. `{ }`
   기존 `HTML` 에서는 `" "` 에 자바스크립트 코드를 넣었지만 `리액트` 에서는 함수 형태의 객체를 전달합니다.

💢 **주의사항** 💢
`리액트` 에서는 `DOM` 요소에만 이벤트를 설정할 수 있습니다.

```javascript
<ExComponent onClick={doSomething} />
```

예를 들어 위와 같이 컴포넌트에 `onClick` 을 설정한다면 이는 이벤트가 아닌 그냥 `onClick` 이라는 이름의 `props` 를 전달하게 되는 것입니다.

---

### 간단한 `onChange` 이벤트 예제

```javascript
import React from "react";

const WriteMessage = () => {
  const handleChange = (event) => {
    console.log(event.target.value);
  };
  return (
    <input
      type="text"
      name="message"
      placeholder="Write Something ... 📝"
      onChange={handleChange}
    />
  );
};

export default WriteMessage;
```

<img src="https://user-images.githubusercontent.com/89551626/133533315-264d030a-7218-4b2b-af61-809a2aa64af0.gif">

이런 식으로 `input` 값의 `value` 를 콘솔에 찍어볼 수 있습니다.
`onChange` 의 파라미터인 `event` 객체는 `리액트` 의 `Systhetic Event` 로 우리가 기존에 알고 있던 네이티브 이벤트의 객체로 이루어져 있어 비슷하게 사용하면 됩니다.
하지만 `Systhetic Event` 가 네이티브 이벤트와 다른 점은 `Event Polling` 인데요, 이벤트가 끝나고 나면 이벤트가 초기화되므로 정보를 참조할 수 없습니다.
`Event Polling` 의 해결방법과 `Synthetic Event` 에 대한 설명은 [이 곳](https://junukim.dev/articles/React_Synthetic_Event/)에서 추가로 확인할 수 있습니다.

### `state` 에 `input` 값 담기

위의 코드에 `state` 를 이용하여 `value` 를 업데이트 시키고 이를 화면에 찍어보겠습니다.
`state 값` 을 지난주에 배운 `세터함수` 를 활용하여 변경시킬 수 있습니다.
`세터함수` 에 위에서 콘솔에 찍어봤던 `event.target.value` 를 넣어주면 되겠죠?
이후 `input` 의 `value` 에 `state 값` 을 전달해주고 아무 태그나 이용하여 `state 값` 을 찍어봅시다.

```javascript
import React, { useState } from "react";

const WriteMessage = () => {
  const [message, setMessage] = useState("");
  const handleChange = (event) => {
    setMessage(event.target.value);
  };
  return (
    <>
      <input
        type="text"
        name="message"
        placeholder="Write Something ... 📝"
        value={message}
        onChange={handleChange}
      />
      <h1>내가 쓴 메시지: {message}</h1>
    </>
  );
};

export default WriteMessage;
```

<img src="https://user-images.githubusercontent.com/89551626/133533498-c0775dda-6891-47c9-aa9d-fa7475f098e2.gif">

### 리셋 시키기

버튼을 누르면 `input` 의 `value` 를 리셋시켜 보겠습니다.
`세터함수` 를 이용하여 빈 문자열을 넣어주면 됩니다.
반복되는 코드가 중복되는 부분은 생략하였습니다.

```javascript
// 콜백함수 handleReset
const handleReset = () => {
  setMessage("");
};

// 버튼을 클릭하면 handleReset 실행
<button onClick={handleReset}>초기화</button>;
```

---

### 여러 개의 `input` 다루기

`input` 이 하나가 아니라 여러 개라면 어떻게 해야할까요?

```javascript
const [writer, setWriter] = useState("");
const [message, setMessage] = useState("");
```

와 같이 `input` 의 개수만큼 `useState` 를 활용해도 되지만 `input` 의 개수가 많아진다면 위에서 사용한 `event` 객체를 활용하는 방법이 더 효율적일 것입니다.

`state 값` 을 단일 값이 아닌 객체로 선언해봅시다.

```javascript
const [inputs, setInputs] = useState({
  writer: "",
  message: "",
});
```

이렇게 되면 inputs 이라는 배열에 `writer` 와 `message` 라는 `key` 를 가진 객체가 생성됩니다.
`writer` 와 `message` 를 자유롭게 쓰기 위해선 `Destructing (비구조화 할당)` 이 유용하겠죠?

```javascript
const { writer, message } = inputs;
```

이제 컴포넌트 내에서 자유롭게 `writer` 와 `message` 를 사용할 수 있습니다.

메시지 작성자인 `writer` field 를 하나 추가하고 리셋 함수인 `handleReset` 까지 수정해보겠습니다.

```javascript
// handleReset()
const handleReset = () => {
  setInputs({
    writer: "",
    message: "",
  });
};

// writer input field
<input
  type="text"
  name="writer"
  placeholder="What's your name ... 👩"
  value={writer}
  onChange={handleChange}
/>;
```

이제 `handleChange` 함수만 수정해주면 되는데요,
선행되어야 하는 자바스크립트 지식이 있습니다.

1. `Spread`
   지난 주 발표내용에도 있었고 예빈님이 추가설명까지 해주셨기 때문에 자세한 내용은 생략합니다.
   [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax) 혹은 [벨로퍼트님 자료](https://learnjs.vlpt.us/useful/07-spread-and-rest.html)를 참고해주세요!
2. `Object`
   [참고 링크](https://medium.com/@bretdoucette/understanding-this-setstate-name-value-a5ef7b4ea2b4)

> 4장 다 끝내고 잘라했는데 자스 개념 공부하다보니 자야될 시간이 돼서 잡니다,,,
> 추후 수정예정

<details>
<summary>전체 코드 (클릭하면 볼 수 있음)</summary>
<div markdown="1">

```javascript
import React, { useState } from "react";

const WriteMessage = () => {
  const [inputs, setInputs] = useState({
    writer: "",
    message: "",
  });

  // Destructing
  const { writer, message } = inputs;

  const handleChange = (event) => {
    const { name, value } = event.target;
    setInputs({
      ...inputs,
      [name]: value,
    });
  };

  const handleReset = () => {
    setInputs({
      writer: "",
      message: "",
    });
  };

  return (
    <>
      <input
        type="text"
        name="writer"
        placeholder="What's your name ... 👩"
        value={writer}
        onChange={handleChange}
      />
      <input
        type="text"
        name="message"
        placeholder="Write Something ... 📝"
        value={message}
        onChange={handleChange}
      />
      <button onClick={handleReset}>초기화</button>
      <h1>작성자:{writer}</h1>
      <h1>내용: {message}</h1>
    </>
  );
};

export default WriteMessage;
```

</div>
</details>
