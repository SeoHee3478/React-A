# [4장] 이벤트 핸들링

### 들어가기에 앞서
- 함수형 컴포넌트를 중심으로 설명합니다
- 컴포넌트, props, state 등의 기본 개념은 설명하지 않습니다

#### 목차
- 리액트의 이벤트 시스템
- 예제로 이벤트 핸들링 익히기

## [4-1] 리액트의 이벤트 시스템
리액트에서도 DOM 요소들과 상호 작용하는 이벤트를 적용할 수 있습니다

대표적으로는 `onClick` `onChange` `onSubmit` 등을 예로 들 수 있겠네요

### 이벤트를 사용할 때 주의 사항

- 이벤트 이름은 카멜 표기법으로 작성합니다
[ ( 카멜, 파스칼, 스네이크 코드 표기법 )](https://color-workroom.tistory.com/entry/%EC%B9%B4%EB%A9%9C-%ED%8C%8C%EC%8A%A4%EC%B9%BC-%EC%8A%A4%EB%84%A4%EC%9D%B4%ED%81%AC-%ED%91%9C%EA%B8%B0%EB%B2%95-camelCasePascalCasesnakecase)
```
올바른 사용법
<button onClick={someHandler}>이벤트</button>

잘못된 사용법(카멜케이스 미준수)
<button onclick={someHandler}이벤트</button>
```

- 이벤트에 실행할 자바스크립트 코드를 전달하는 것이 아니라 함수 형태의 값을 전달합니다
```
import {useState} from 'react'

const Say = () => {
  const [message, setMessage] = useState('');
  
  // 이벤트 핸들러 (화살표 함수 형태를 가지고있다)
  const onClickEnter = () => setMessage('안녕하세요!');
  const onClickLeave = () => setMessage('안녕히가세요!');
  
  return (
    <div>
     {/* 클릭 이벤트에 함수 객체를 전달한다 */}
     <button onClick={onClickEnter>입장</button>
     <button onClick={onClickLeave>퇴장</button>
    </div>
  );
}
```
위 코드처럼 onClick을 보면 실행할 코드를 넣은 것이 아니라 함수 형태의 객체를 전달해야합니다

- DOM 요소에만 이벤트를 설정할 수 있습니다
```
{/* DOM에 이벤트 설정 */}
<div onClick={someEvent}>안녕</div>

{/* 컴포넌트에는 이벤트를 설정할 수 없다 */}
{/* 단순히 onClick이라는 props를 전달하는 형태가 됨 */}
<MyComponent onClick={someEvent} />
```
