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
```

- 위와 같은 형식으로 바꿔줄 수 있다.
