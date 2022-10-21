### Reduxjs/toolkit 사용

- 여러 상태를 하나의 store에서 관리하게 되면 state의 불변성을 침범할 수 있기 때문에 reduxtoolkit을 사용해서 상태를 나누어 관리한다.
- 리덕스 툴킷을 사용하기 위해서는 먼저 리덕스 툴킷을 설치해야 한다.

```sh
npm install @reduxjs/toolkit
```

으로 설치하면 된다.

- 설치 후에 리덕스를 삭제해야한다. 리덕스툴킷에 이미 리덕스가 포함되어 있기 때문이다.

### createSlice

- createSlice를 임포트해서 새로운 슬라이스를 만들어준다.
- 이전에 만들었던 store를 이런식으로 변경할 수 있다.
- if체크를 안해도 되기 때문에 가독성이 좋아진다. 기존 state를 변경하지 않도록 내부적으로 immer 패키지를 사용하기 때문에 자동으로 원래의 상태를 복제하고 새로운 상태 객체를 복사한다.

```JavaScript

const counterSlice = createSlice({
  name: 'counter',
  initialState: initialCounterState,
  reducers: {
    increment(state) {
      state.counter++;
    },
    decrement(state) {
      state.counter--;
    },
    increase(state, action) {
      state.counter = state.counter + action.payload;
    },
  },
});

cosnt store = configureStore({
  reducer: {
    counter: counterSlice.reducer
  }});

export const counterActions = couterSlice.actions;
```

- 위와 같이 configureStore를 활용해서 리듀서를 등록해줄 수 있다. 여러개의 슬라이스가 있을 경우를 위해 reducer안에 또 다른 객체로 counter를 등록해주었다.
- 리덕스 툴킷을 사용하면 액션 객체를 conterSlice.actions.incremet()할 때 저절로 생성해주기 때문에 액션 타입 오탈자를 걱정할 필요가 없다.
- 아래와 같이 액션을 발송할 수 있다.

```JavaScript
import { counterActions } from '../store/counter-slice';

const Counter = () => {
  const dispatch = useDispatch();

  const incrementHandelr = () => {
    dispatch(counterActions.increment());
  };

  const increaseHandler = () => {
    dispatch(counterActions.increase(5));
    //인자로 값을 전달
  };

  const decrementHandler = () => {
    dispatch(counterActions.decrement());
  };
```
