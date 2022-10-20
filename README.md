# React-Study

- 리액트에 대해 공부한 내용을 정리했습니다.

### virtual DOM 작동원리

- 버츄얼 돔이란 돔을 가볍게 만든 자바스크립트 표현이라고 할 수 있고, 주로 React, Vue.js에 사용된다. 가상 돔은 실제로 스크린에 렌더링하는 것이 아니기 때문에 DOM을 직접 업데이트하는 것보다 상대적으로 빠르다.
- 가상 DOM은 실제 DOM에서 처리하는 방식이 아닌 virtual DOM과 메모리에서 미리 처리하고 저장한 후 실제 DOM과 동기화하는 프로그래밍 개념이다. 해당 DOM을 컴포넌트 단위로 쪼개서 HTML 컴포넌트 조립품처럼 다루는 개념이다.
- DOM을 반복적으로 직접 조작하면 브라우저가 렌더링을 자주하게 되므로 그만큼 PC자원을 많이 소모하게 되는데 이 문제를 해결하기 위한 기술이다.
- 버츄얼 돔은 뷰에 변화가 있다면 실제 DOM에 적용되기 전에 가상의 DOM에 먼저 적용시키고 최종적인 결과를 실제 DOM으로 전달해준다. 이로써 브라우저 내에서 발생하는 연산의 양을 줄이며 성능 개선을 할 수 있다.

### 클래스형 컴포넌트와 함수형 컴포넌트의 차이점

- 클래스형 컴포넌트는 state, lifecycle 관련 기능을 사용하며 메모리 자원을 함수형 컴포넌트보다 조금 더 사용한다. 임의 메서드를 정의할 수 있다. props값을 this.props로 불러온다.
- 함수형 컴포넌트는 state, lifecycle 관련 기능을 사용할 수 없지만 Hook을 통하여 해결할 수 있다. 메모리 자원을 클래스형 컴포넌트보다 덜 사용하며 컴포넌트 선언이 편하다는 특징이 있다. props를 불러올 필요 없이 바로 호출할 수 있다.
- 클래스형 컴포넌트는 로직과 상태를 컴포넌트 내에서 구현하기 때문에 상대적으로 복잡한 UI 로직을 갖고있고 함수형 컴포넌트는 state를 사용하지 않고 단순하게 데이터를 props로 받아서 UI에 뿌려준다. Hook을 필요한 곳에 사용하며 로직의 재사용이 가능하다는 장점이 있다.

### ReactJS의 라이프 사이클

- 라이프사이클은 컴포넌트가 페이지에 마운팅되기 전인 준비 과정부터 시작하여 언마운팅 되기까지의 전체적인 컴포넌트 수명 주기를 의미한다.
- 리액트의 클래스형 컴포넌트에서는 라이프사이클을 조정할 수 있는 다양한 라이프 사이클 메서드가 있으며, componentWillMount, componentWillUnMount, componentDidUpdate 등이 그러한 메서드이다. 이러한 메서드들을 통해 라이프사이클 흐름에 대해 개발자가 관여할 수 있으나 함수형 컴포넌트에서는 이러한 라이프사이클 메서드의 역할을 useEffect훅이 대신한다.
- mounting -> update -> unmounting -> error handling의 과정을 거치며 이 중 mounting은 constructor -> render -> componentDidMount()의 순서를 가진다. Update는 getDerivedStateFromProps -> shouldComponentUpdate -> render -> getSnapshotBeforeUpdate -> componentDidUpdate의 순서를 가진다. Unmounting은 componentWillUnmount의 과정이 있다.

### useEffect

- useEffect훅은 리액트 컴포넌트가 렌더링될 때마다 특정 작업을 실행할 수 있도록 해주는 hook이다. 컴포넌트가 마운트 됐을 때, 언마운트 되거나 업데이트 되었을 때 특정 작업을 처리할 수 있다.
- 첫 번째 인자로 실행할 콜백 함수를 넣고 두 번째 인자로 검사하고자 하는 특정 값이나 빈 배열을 넣는다. 빈 배열을 넣었을 때는 컴포넌트가 렌더링 될 때 1회만 실행된다.

### React를 사용할 때의 장점

1. Virtual DOM을 사용하여 효율성 향상

- 버추얼 돔은 DOM을 가상으로 표현한 것이다. 리액트 앱에서 데이터가 변경될 때마다 새로운 virtual DOM이 생성된다. virtual DOM을 만드는 것이 브라우저 내에서 UI를 렌더링하는 것보다 더욱 빠르다.

2. SEO 친화적이다

- 개발자가 다양한 검색 엔진에서 쉽게 탐색될 수 있는 사용자 인터페이스를 개발할 수 있도록 한다. SSR도 가능하기 때문에 앱의 SEO가 향상된다.

3. 재사용 가능한 컴포넌트

- 컴포넌트 기반 아키텍처를 사용한다. 컴포넌트는 독립적이며 재사용 가능한 코드의 조각들이다. 이러한 컴포넌트는 유사한 기능을 가진 다양한 애플리케이션에서 공유할 수 있다. 재사용 가능한 컴포넌트들은 개발 속도를 향상시켜준다.

### React에서 리렌더링을 방지하는 방법

- 클래스형 컴포넌트에서는 shouldComponentUpdate()의 반환값을 false로 주어서 리렌더링을 방지할 수 있다.
- 함수형 컴포넌트에서는 React.memo로 방지할 수 있다.
