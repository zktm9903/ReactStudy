# 리액트 정리

- 리엑트의 탄생
  - vanilla javascript로 상태가 바뀔 때마다 DOM을 업데이트하는 것은 번거롭고 비효율 적이다. 
  - 어떠한 상태가 바뀌었을때, 그 상태에 따라 DOM을 어떻게 업데이트 할 지 규칙을 정하는 것이 아니라, 아예 다 날려버리고 처음부터 모든걸 새로 만들어서 보여주겠다는 것이 리액트의 목적

- Virtual DOM
  - 상태가 바뀔 때마다 모든걸 다 날려버리고 모든걸 새로 만들게 된다면, 속도가 굉장히 느려질 것이기 때문에 브라우저에 보여지는 DOM 말고도 `Virtual DOM`을 따로 생성함
  - 가상의 DOM, JavaScript 객체이기 때문에 작동 성능이 훨씬 빠름
  - 상태가 업데이트 되면, 업데이트가 필요한 곳의 UI를 `Virtual DOM`을 통해서 렌더링함
  - 비교 알고리즘을 통하여 브라우저 DOM과 비교를 한 후 차이가 있는 곳을 감지하여 이를 실제 DOM에 패치시켜줌

- 컴포넌트
  - 컴포넌트 예시 (함수로 작성)
  - ```javascript
    import React from 'react';

    function Hello() {
        return <div>안녕하세요</div>
    }

    export default Hello;
    ```
  - 컴포넌트는 일종의 UI조각 
  - 쉽게 재사용 가능

- JSX
  - 리액트에서 생김새를 정의할 때, 사용하는 문법
  - 얼핏보면 HTML 같이 생겼지만 실제로는 JavaScript 이다.
  - ```JSX
    return <div>안녕하세요</div>;
    ```
  - 태그는 무조건 닫혀있어야 함
  - 두개 이상의 태그는 무조건 하나의 태그로 감싸져있어야 함
  - ```JSX
    function App() {
        return (
            <>
                <Hello />
                <div>안녕히계세요</div>
            </>
        );
    }
    ```
- props
  - props는 properties의 줄임말
  - 어떠한 값을 컴포넌트에게 전달해줘야 할 때, props를 사용
  - props 예시
  - ```JSX
    import React from 'react';

    function Hello(props) {
        return <div>안녕하세요 {props.name}</div>
    }

    export default Hello;
    ```
  - 구조 분해 & defaultProps로 기본값 설정
  - ```JSX
    import React from 'react';

    function Hello({ color, name }) {
        return <div style={{ color }}>안녕하세요 {name}</div>
    }

    Hello.defaultProps = {
        name: '이름없음'
    }

    export default Hello;
    ```
  - props.children
  - ```JSX
    function Wrapper({ children }) {
        const style = {
            border: '2px solid black',
            padding: '16px',
        };
    return (
        <div style={style}>
            {children}
        </div>
        )
    }
    ```

