### Redux란?

-앱 전반적인 부분에 걸쳐서 스테이트를 관리할 수 있게 해주는 시스템입니다.

### Redux가 필요한 State는?

(useState와 useReduce로 대체 가능합니다.)
-Cross-Component State: 여러 개의 컴포넌트가 하나의 스테이트를 공유할 때, props chain으로 state를 관리하고 싶지 않을 경우
-App-Wide State: 사용자 인증과 같이 앱 전반에 걸쳐서 스테이트 변동이 있을 때

Redux는 Context API와 비슷하게 여러 부분에 걸쳐 스테이트를 관리할 때 사용합니다.
다만, Context API를 사용하게 된다면 아래와 같이 가시성이 좋지 않고 복잡한 코드를 작성하게 될 수도 있습니다.
많은 컨텍스트와 프로바이더가 중첩되어 있는 경우를 피하고 하나의 컨텍스트가 지나치게 많은 상태를 관리하지 않도록 하기 위해 리덕스를 사용하는 경우가 많습니다.

![](https://velog.velcdn.com/images/dochi486/post/b55b319b-1c23-4266-a5cc-5677b0b2fb60/image.png)

![](https://velog.velcdn.com/images/dochi486/post/2d679fd2-1f8d-46a9-b1ec-35125e63f3d3/image.png)

또 다른 Redux를 사용해야하는 이유로는 성능과 관련된 점 입니다. Context API는 자주 변경되는 상태에 대해 최적화되어 있지 않으므로 성능 이슈가 발생할 수 있습니다. 규모가 작은 앱이라면 Context API를 사용해도 관계가 없지만, 프로젝트 규모가 크다면 리덕스를 사용하는 것이 좋습니다.

### 리덕스의 작동방식

![](https://velog.velcdn.com/images/dochi486/post/549d86c1-e7e9-47d4-89d1-2efac2312521/image.png)

- 리덕스는 하나의 Store를 가지고 모든 앱의 상태를 저장합니다. 다만 이 중앙 저장소 전체를 항상 관리할 필요가 없으므로 데이터를 저장해서 컴포넌트에서 쉽게 사용할 수 있습니다.
- 컴포넌트는 Store를 구독하여 필요한 데이터가 변경되었을 때 Store의 일부를 받아서 사용합니다.
- 중요한 점은 저장된 데이터를 절대로 컴포넌트에서 바로 조작하지 않는 것이며, Reducer 함수를 사용해서 Store의 데이터를 변경합니다. useReducer와는 다른 개념으로, Reducer함수는 입력을 받아 변환하고 새로운 출력과 결과를 반환합니다.
- 컴포넌트와 Reducer를 연결하는 것은 Action을 통해 가능합니다. 컴포넌트가 액션을 발송(dispatch)하여 리덕스가 액션을 리듀서로 보내서 리듀서가 새로운 데이터를 Store에 저장하게 됩니다.
- 새로운 데이터가 Store에 저장되면 구독 중이던 컴포넌트가 해당 데이터를 가지고 새롭게 렌더링하게 됩니다.

### 리듀서 함수

- 리듀서 함수는 항상 이전 스테이트와 발생한 액션 2개의 인자를 받으며 새로운 스테이트 객체를 반환합니다. 그렇기 때문에 리듀서 함수는 순수한 함수여야합니다. 순수한 함수란 동일한 입력을 했을 경우 항상 정확하게 같은 값을 반환하는 함수를 말합니다.
- 순수한 함수여야하기 때문에 http 요청이나 로컬 저장소에서 파일을 읽어오는 등의 행동을 하면 안됩니다!!
- 리듀서 함수는 리덕스가 제공하는 입력을 취하고 새로운 스테이트 객체를 반환하는 순수한 함수 여야합니다. 리듀서 함수가 반환하는 새로운 스테이트는 기존의 스테이트를 대체합니다.

```JavaScript
const store = redux.createStore(sampleReducer)
```

- Store는 리듀서 함수와 함께 사용해야 합니다.

### 본격적으로 React에서 Redux 사용하기

- React에서 Redux를 제대로 사용하기 위해서는 npm install redux만이 아니라 react-redux 패키지도 함께 설치해줘야 합니다.
- 이 패키지를 통해서 리액트가 리덕스 스토어와 리듀서에 간단하게 연결할 수 있습니다. 컴포넌트가 쉽게 구독을 할 수 있게 해줍니다.

```JavaScript
const sampleReducer = (state = { counter : 0 } , action) => {
 if(action.type === 'increment'){
     return {
  	counter : state.counter + 1,
  }
 }
	return state; //변하지 않은 스테이트를 반환
}

const store = createStore(sampleReducer);
```

위의 내용처럼 스토어에는 리듀서 함수를 가리키는 포인터를 인자로 넘겨줘야한다. 리듀서 함수의 초기 state는 초기 값을 지정해줄 수 있고 이는 객체 형태로 넘겨 줄 수 있다.
액션의 타입을 스트링으로 지정하여 리듀서 함수 안에서 디스패치된 액션의 종류에 따라 리턴할 값을 설정할 수 있다.

### configureStore

![](https://velog.velcdn.com/images/dochi486/post/f9f6c4c9-438e-4774-b893-6e5431b1b7cb/image.png)

createStore는 더 이상 사용하지 않는 것이 좋다는 메세지가 뜬다. 따라서 reduxjs/toolkit의 configureStore를 사용하여 스토어를 설정해주도록 한다. createStore와 달리 객체로 reducer를 넘겨주는 점이 다르며

```JavaScript
const store = configureStore({
	reducer: {
   	counter: counterReducer,
   	auth: authReducer,
 },
});
```

이런 식으로 사용할 수 있다.

### Store 제공하기

- 만든 store를 앱 전체에서 사용하기 위해서는 최상위 레벨의 컴포넌트를 Provider로 감싸줘야한다. 이 때 Provider의 props로 store를 넘겨준다. 꼭 최상위 레벨은 아니어도 되지만 다양한 범위에 걸쳐 리덕스 스토어를 사용하려면 최상위 레벨에서 Provider로 감싸주는 것이 좋다.

```JavaScript
import store from './store/index';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <Provider store={store}>
    <App />
  </Provider>
);
```

- 위와 같이 store를 임포트해서 프로바이더의 props로 전달해준다.

### useSelector 훅

- 생성한 스토어를 실제로 컴포넌트에서 접근하려면 useSelector 훅을 통해서 접근할 수 있다.
- 클래스 기반 컴포넌트에서는 connect 함수를 사용할 수도 있다.
- useSelector에 인자로 함수를 넘겨주면 리덕스가 실행한다. useSelector를 사용하면 자동으로 리덕스가 구독을 해주기 때문에 스토어가 바뀌면 자동으로 컴포넌트가 재 렌더링된다.

### useDispatch 훅을 통해 액션 디스패치

만들어둔 액션을 디스패치해서 스토어에 알림을 보내려면 useDispatch 훅을 통해 컴포넌트에서 액션을 발송할 수 있습니다. useDispatch는 함수를 반환하는데 이 함수를 실행하여 타입 프로퍼티가 있는 액션을 발생시킵니다. 이 타입은 스토어에서 정의한 것 중 하나를 발생시키면 되며, 정확히 입력하는 것이 중요합니다.

#### counter 예시

```JavaScript
const Counter = () => {
  const dispatch = useDispatch();

  const counter = useSelector((state) => state.counter);

  const incrementHandelr = () => {
    dispatch({type: 'increment'});
  };

  const decrementHandler = () => {
    dispatch({type: 'decrement'});
  };

  return (
    <main className={classes.counter}>
      <h1>Redux Counter</h1>
      {show && <div className={classes.value}>{counter}</div>}
      <div>
        <button onClick={incrementHandelr}>증가</button>
        <button onClick={decrementHandler}>감소</button>
      </div>
    </main>
  );
}
```

위와 같이 액션을 발생시킬 버튼에 함수를 연결해주면 등록해둔 리덕스 스토어의 액션을 실행할 수 있게된다.

#### store 예시

```JavaScript
 const counterReducer = (state = initialState, action) => {
   if (action.type === 'increment') {
     return {
       counter: state.counter + 1,
       showCounter: state.showCounter,
     };
   }

  if (action.type === 'decrement') {
    return {
      counter: state.counter - 1,
      showCounter: state.showCounter,
    };
  }

  return state;
 };
```

store에서 increment와 decrement 액션을 정의해주었기 때문에 위의 카운터가 작동하게 된다.

## payload를 통한 추가 작업

액션에 타입 말고 payload를 추가하여 액션의 동작을 다양화할 수 있다.
위의 store에 페이로드가 있는 액션 타입을 하나 더 정의해보겠다. 페이로드는 액션에 추가할 수 있는 부가적인 속성이다.

#### store.js

```JavaScript

   if (action.type === 'increase') {
     return {
       counter: state.counter + action.amount,
       showCounter: state.showCounter,
     };
   }

```

위 처럼 action의 amount 페이로드를 추가하여 해당하는 페이로드를 넘겨주면 그만큼의 수를 증가시킬 수 있다.

#### counter.js

```JavaScript

const increaseHandler = () => {
  dispatch({type: 'increase', amount: 5});
}

```

counter에 위와 같은 함수를 추가하고 버튼 onClick 이벤트와 연결 시키면 클릭할 때마다 5만큼 숫자를 증가시키는 카운터를 작성할 수 있다. 주의할 점은 store에서 정의한 페이로드의 이름과 디스패치 시킬 때 전달하는 페이로드의 이름이 같아야한다는 점이다. 그렇지 않으면 데이터를 추출할 수 없기 때문에 중요하다!!

## Redux를 사용할 때의 상태 관리

- 절대로 기존 state를 변경해서는 안된다!! 그 대신 새로운 state를 새로 만들어서 덮어써야한다. 객체와 배열이 참조 값이기 때문에 기존의 state를 변형한다. 이는 원치 않는 버그를 발생시킬 수 있는 원인이 되기 때문에 꼭 피해야한다.
- 각각의 액션이 실행될 때마다 새로운 state로 덮어쓰기 때문에 해당하는 액션에서 사용하지 않는 데이터여도 이전 state 값을 반환하도록 해주어야한다.
