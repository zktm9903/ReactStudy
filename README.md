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

- 조건부 렌더링
  - 특정 조건에 따라 다른 결과물을 렌더링 하는 것을 의미합니다.
  - ```JSX
    import React from 'react';
    import Hello from './Hello';
    import Wrapper from './Wrapper';


    function App() {
      return (
        <Wrapper>
          <Hello name="react" color="red" isSpecial={true}/>
          <Hello color="pink" />
        </Wrapper>
      )
    }

    export default App;
    ```
  - ```JSX
    import React from 'react';

    function Hello({ color, name, isSpecial }) {
      return (
        <div style={{ color }}>
          {isSpecial && <b>*</b>}
          안녕하세요 {name}
        </div>
      );
    }

    Hello.defaultProps = {
      name: '이름없음'
    }

    export default Hello;
    ```
- useState
  - 상태관리 Hooks 기능 안에 있는 함수 중 하나
  - 카운터 예시
  - ```JSX
    import React, { useState } from 'react';

    function Counter() {
      const [number, setNumber] = useState(0);

      const onIncrease = () => {
        setNumber(prevNumber => prevNumber + 1);
      }

      const onDecrease = () => {
        setNumber(prevNumber => prevNumber - 1);
      }

      return (
        <div>
          <h1>{number}</h1>
          <button onClick={onIncrease}>+1</button>
          <button onClick={onDecrease}>-1</button>
        </div>
      );
    }

    export default Counter;
    ```
  - useState를 사용할 때에는 상태의 기본값을 파라미터로 넣어서 호출함
  - 첫번째 원소는 현재 상태, 두번째 원소는 Setter 함수임
- input 상태 관리
  - ```JSX
    import React from 'react';
    import InputSample from './InputSample';

    function App() {
      return (
        <InputSample />
      );
    }

    export default App;
    ```
  - ```JSX
    import React, { useState } from 'react';

    function InputSample() {
      const [text, setText] = useState('');

      const onChange = (e) => {
        setText(e.target.value);
      };

      const onReset = () => {
        setText('');
      };

      return (
        <div>
          <input onChange={onChange} value={text}  />
          <button onClick={onReset}>초기화</button>
          <div>
            <b>값: {text}</b>
          </div>
        </div>
      );
    }

    export default InputSample;
    ```
- 여러개의 input 상태 관리
  - ```JSX
    import React, { useState } from 'react';

    function InputSample() {
      const [inputs, setInputs] = useState({
        name: '',
        nickname: ''
      });

      const { name, nickname } = inputs; // 비구조화 할당을 통해 값 추출

      const onChange = (e) => {
      const { value, name } = e.target; // 우선 e.target 에서 name 과 value 를 추출
      setInputs({
        ...inputs, // 기존의 input 객체를 복사한 뒤
        [name]: value // name 키를 가진 값을 value 로 설정
      });
      };

      const onReset = () => {
        setInputs({
          name: '',
          nickname: '',
        })
      };


      return (
        <div>
          <input name="name" placeholder="이름" onChange={onChange} value={name} />
          <input name="nickname" placeholder="닉네임" onChange={onChange} value={nickname}/>
          <button onClick={onReset}>초기화</button>
          <div>
            <b>값: </b>
            {name} ({nickname})
          </div>
        </div>
      );
    }

    export default InputSample;
    ```

- useRef으로 특정 DOM 선택
  - 초기화 버튼을 클릭했을 때 이름 input에 포커스가 잡히도록 useRef를 사용한 예시
  - ```JSX
    import React, { useState, useRef } from 'react';

    function InputSample() {
      const [inputs, setInputs] = useState({
        name: '',
        nickname: ''
      });
      const nameInput = useRef();

      const { name, nickname } = inputs; // 비구조화 할당을 통해 값 추출

      const onChange = e => {
        const { value, name } = e.target; // 우선 e.target 에서 name 과 value 를 추출
        setInputs({
          ...inputs, // 기존의 input 객체를 복사한 뒤
          [name]: value // name 키를 가진 값을 value 로 설정
        });
      };

      const onReset = () => {
        setInputs({
          name: '',
          nickname: ''
        });
        nameInput.current.focus();
      };

      return (
        <div>
          <input
            name="name"
            placeholder="이름"
            onChange={onChange}
            value={name}
            ref={nameInput}
          />
          <input
            name="nickname"
            placeholder="닉네임"
            onChange={onChange}
            value={nickname}
          />
          <button onClick={onReset}>초기화</button>
          <div>
            <b>값: </b>
            {name} ({nickname})
          </div>
        </div>
      );
    }

    export default InputSample;
    ```
- 배열 렌더링
  - 예시
  - ```JSX
    import React from 'react';

    function User({ user }) {
      return (
        <div>
          <b>{user.username}</b> <span>({user.email})</span>
        </div>
      );
    }

    function UserList() {
      const users = [
        {
          id: 1,
          username: 'velopert',
          email: 'public.velopert@gmail.com'
        },
        {
          id: 2,
          username: 'tester',
          email: 'tester@example.com'
        },
        {
          id: 3,
          username: 'liz',
          email: 'liz@example.com'
        }
      ];

      return (
        <div>
          {users.map(user => (
          <User user={user} key={user.id} />
          ))}
        </div>
      );
    }

    export default UserList;
    ```
- key값도 같이 넘겨줘야 효율적인 렌더링이 가능
- 