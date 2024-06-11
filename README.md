# 서민석 201930112

## 6월 11일 강의
### Containment와 Specialization을 같이 사용하기
* Containment를 위해서 props.children을 사용하고, Specialization을 위해 직접 정의한 props를 사용하면 됨
* Dialog를 사용하는 SignUpDialong는 Specializtion을 위해 props인 title, message에 값을 넣어 주고 있고, 입력 받기 위해 input과 button을 사용한다

### 상속에 대해서
* 합성과 대비되는 개념으로 상속이 있음
* 자식 클래스는 부모 클래스가 가진 변수나 함수 등의 속성을 모두 갖게 되는 개념
* 리액트에서는 상속보다는 합성을 통해 새로운 컴포넌트를 생성

### 컨텍스트란 무엇인가?
* 기존의 일반적인 리액트에서는 데이터가 컴포넌트의 props를 통해 부모에서 자식으로 단방향으로 전달되었습니다.
* 컨텍스트는 리액트 컴포넌트들 사이에서 데이터를 기존의 props를 통해 전달하는 방식 대신 '컴포넌트 트리를 통해 곧바로 컴포너트에 전달하는 새로운 방식'을 제공
* 어떤 컴포너늩라도 쉽게 데이터에 접근할 수 있다
* 컨텍스트를 사용하면 일일이 props로 전달할 필요 없이 데이터를 필요로 하는 컴포넌트에 곧바로 데이터를 줄 수 있다.

### 언제 컨텍스트를 사용해야 할까?
* 여러 컴포넌트에서 자주 필요로 하는 데이터는 로그인 엽, 로그인 정보, UI테마, 현재 선택된 언어등이 있음
* props를 통해 데이터를 전달하는 기존 방식은 실제 데이터를 필요로 하는 컴포넌트까지의 깊이가 깊어질 수록 복잡해짐
* 반복적인 코드를 계속해서 작성해 주어야 하기 때문에 비효율적이고 가독성이 떨어짐
* 컨텍스트를 사용하면 이러한 방식을 깔끔하게 개선 가능

### 컨텍스트를 사용하기 전에 고려할 점
* 컨텍스트는 다른 레벨의 많은 컴포넌트가 특정 데이터를 필요로 하는 경우에 주로 사용
* 무조건 컨텍스트를 사용하는 것이 좋은 것은 아님
* 컴포넌트와 컨텍스트가 연동되면 재사용성이 떨어지기 때문
* 다른 레벨의 많은 컴포넌트가 데이터를 필요로 하는 경우가 아니면 props를 통해 데이터를 전달하는 컴포넌트 합성 방법이 더 적합
* 어떤 경우에는 하나의 데이터에 다양한 레벨에 있는 중첩된 컴포넌트들의 접근이 필요할 수 있다
* 이런 경우면 컨텍스트가 유리
* 컨텍스트는 해당 데이터와 데이터의 변경사항을 모두 하위 컴포넌트들에게 broadcast해주기 때문
* 컨텍스트를 사용하기에 적합한 데이터의 대표적인 예로는 '지역정보', 'UI테마 그리고 '캐싱된 데이터'등이 있음

### 컨텍스트 API
#### React.createContext
* 컨텍스트를 생성하기 위한 함수
* 파라메타에는 기본 값을 넣어주면 됨
* 하위 컴포넌트는 가장 가까운 상위 레벨의 Provider로 부터 컨텍스트를 받게 되지만, 만일 Provider를 찾을 수 없다면 설정한 기본 값을 사용하게 됨

#### Context.Provider
* Context.Provider 컴포넌트로 하위 컴포넌트들을 감싸주면 모든 하위 컴포넌트들이 해당 컨텍스트의 데이터에 접근할 수 있게 됨
* Provider 컴포넌트에는 value라는 prop이 있고, 이것은 Provider컴포넌트 하위에 있는 컴포넌트에게 전달됨
* 하위 컴포넌트를 consumer컴포넌트라고 부름

#### Context.Consumer
* 함수형 컴포넌트에서 Context.Consumer를 사용하여 컨텍스트를 구독할 수 있음
* 컴포넌트의 자식으로 함수가 올 수 있는데 이것을 function as a child라고 부름
* Context.Consumer로 감싸주면 자식으로 들어간 함수가 현재 컨텍스트의 value를 받아서 리액트 노드로 리턴
* 함수로 전달되는 value는 Provider의 value prop과 동일

####  Context.displayName
* 컨텍스트 객체는 displayName이라는 문자열 속성을 갖는다
* 크롬의 리액트 개발자 도구에서는 컨텍스트의 Provider나 Consumer를 표시할 때 displayName을 함께 표시

### 여러 개의 컨텍스트 사용하기
* 여러 개의 컨텍스트를 동시에 사용하려면 Context.Provider를 중첩해서 사용
* 두 개 또는 그 이상의 컨텍스트 값이 자주 함께 사용될 경우 모든 값을 한 번에 제공해 주는 별도의 render prop 컴포넌트를 직접 만드는 것을 고려하는 것이 좋음

### useContext
* 함수형 컴포넌트에서 컨텍스트를 사용하기 위해 컴포넌트를 매번 Consumer 컴포넌트로 감싸주는 것 보다 더 좋은 방법이 있다
* use Context()훅은 React.createContext() 함수 호출로 생성된 컨텍스트 객체를 인자로 받아서 현재 컨텍스트의 값을 리넡
* 이 방법도 가장 가까운 상위 Provider로 부터 컨텍스트의 값을 받아옴
* 만일 값이 변경되면 useContext() 훅을 사용하는 컴포넌트가 재 렌더링 됨
* 또한 useContext() 훅을 사용할 때에는 파라미터로 컨텍스트 객체를 넣어줘야 한다는 것을 기억해야 함

## 6월 5일 강의
### Shared State
* Shared state는 state의 공유를 의미
* 같은 부모 컴포넌트의 state를 자식 컴포넌트가 공유해서 사용하는 것

* 상위 컴포넌트인 Calculator에서 온도와 단위를 state로 갖는다
* 두 개의 하위 컴포넌트는 각각 섭씨와 화씨로 변환된 온도와 단위, 그리고 온도를 업데이트하기 위한 함수를 props로 갖고 있음
* 모든 컴포넌트가 state를 갖지 않고, 상위 컴포넌트로 올려서 공유하면 리액트를 더욱 간결하고 효율적으로 개발 가능

### 합성에대해
* 합성은 여러 개의 컴포넌트를 합쳐서 새로운 컴포넌트를 만드는 것
### Containment
* 특정 컴포넌트가 하위 컴포넌트를 포함하는 형태의 합성 방법
* 컴포넌트에 따라서는 어떤 자식 엘리먼트가 들어올 지 미리 예상할 수 없는 경우가 있음
* 범용적인 박스 역할을 하는 Sidebar 혹은 Dialog와 같은 컴포넌트에서 특히 자주 볼 수 있음
* 컴포넌트에서는 children prop을 사용하여 자식 엘리먼트를 출력에 그대로 전달하는 것이 좋다
* children prop은 컴포넌트의 props에 기본적으로 들어있는 children 속성을 사용

* 리액트에서는 props.children을 통해 하위 컴포넌트를 하나로 모아서 제공
* 만일 여러개의 children 집합이 필요할 경우는 별도로 props를 정의해서 각각 원하는 컴포넌트를 넣어줌

### Specialization (특수화, 전문화)
* 웰컴 다이얼로그는 다이얼로그의 특별한 케이스
* 범용적인 개념을 구별이 되게 구체화하는 것을 특수화라고 함
* 객체지향 언어에서는 상속을 사용하여 특수화를 구현
* 리액트에서는 합성을 사용하여 특수화를 구현


## 5월 29일 강의
### select 태그
* select 태그도 taxtarea와 동일합니다.

```js
<select>
  <option value="apple">사과</option>
  <option value="banana">바나나</option>
  <option value="grape">포도</option>
  <option value="watermelon">수박</option>
<select>
```

### File input 태그
* File input 태그는 그 값이 읽기 전용이기 때문에 리액트에서 비제어 컴포넌트가 됩니다.

```js
<input type="file" />
```

### Input Null Value
* 제어 컴포넌트에 value prop을 정해진 값을 넣으면 코드를 수정하지 않는 한 입력 값을 바꿀 수 없습니다.
* 만약 value prop은 넣되 자유롭게 입력할 수 있게 만들고 싶다면 값이 undefined 또는 null을 넣어주면 됩니다.
```js
ReactDOM.render(<input value="hi" />, rootNode);

setTimeout(function() {
  ReactDOM.render(<input value={null} />, rootNode);
}, 1000);
```


## 5월 22일 강의
### 리스트
* 목록
* 같은 아이템을 순서대로 모아 놓은 것
* 배열 사용

### 키
* 모두 각자 공유
* 각 객체나 아이템을 구분할수 있는 고유한 값
* 유일값

### 여러 컴포넌트 코드
* 앱 함수를 사용

### 폼
* 양식
* 사용자로부터 입력을 받기 위해 사용하는 것

### 제어 컴포넌트
* 사용자가 입력한 값에 접근하고 제어할 수 있도록 해주는 컴포넌트
* state를 사용하여 관리



## 5월 8일 강의
### Arguments 전달하기
* 함수를 정의할 때는 파라미터 혹은 매개변수, 함수를 사용할 때는 아귀먼트 혹은 인수라고 부른다
* 이벤트 핸들러에 매개변수를 전달해야 하는 경우도 많다
* event라는 매개변수는 리액트의 이벤트 객체를 의미
[Toggle]()

<!-- <button onCick={(event) => this.deleteItem(id, event)}>삭제하기</button> -->

### 인라인 조건
* 필요한 곳에 조건문을 직접 넣어 사용하는 방법
1. 인라인 if
* if문을 직접 사용하지 않고, 동일한 효과를 내기 위해 && 논리 연산자를 사용
* &&는 and연산자로 모든 조건이 참일 때만 참
* 첫 번째 조건이 거짓이면 두 번째 조건은 판단할 필요가 없음
* 판단만 하지 않는 것이고 결과 값은 그대로 리턴

2. 인라인 if-else
* 삼항 연산자를 사용합니다. 조건문 ? 참일 경우 : 거짓일 경우
* 문자열이나 엘리먼트를 넣어서 사용할 수 도 있음

## 5월 1일 강의
### 훅의 규칙
* 첫 번째 규칙은 무조건 최상위 레벨에서만 호출해야한다
* 반복문이나 조건문 또는 중첩된 함수들 안에서 훅을 호출하면 안됩니다.
* 이 규칙에 따라서 훅은 컴포넌트가 렌더링 될 때마다 같은 순서로 호출 되어야합니다.
* 페이지 230의 코드는 조건에 따라 호출 되기 때문에 잘못된 코드입니다.
* 두 번째 규칙은 함수형 컴포넌트에서만 훅을 호출
* 일반 자바스크립트 함수에서 훅을 호출하면 안됨
* 훅은 함수형 컴포넌트 혹은 직접 만든 커스텀 훅에서만 호출할 수 있다.

### 나만의 훅 만들기
* 필요 하다면 직접 훅을 만들어 쓸 수도 있다. 이것을 커스텀 훅이라고 함
* 한 가지 주의할 점은 일반 컴포넌트와 마찬가지로 다른 훅을 호출하는 것은 무조건 커스텀 훅의 최상위 레벨에서만 해야함
* 이름은 use로 시작하도록 합니다. 그렇지 않으면 다른 훅을 불러올 수 없습니다.
* useUserStatus()훅의 목적은 사용자의 온라인/오프라인 상태를 구독하는 것
* 파라메터로 userid를 받아, 사용자의 온라인 상태를 반환하고 있습니다.  
[useUserStatus 참조]()

### DOM, React에서 onClick 차이점 
1. 이벤트 이름이 onclick에서 onClick으로 변경
2. 전달하려는 함수는 문자열에서 함수 그대로 전달


## 4월 17일 강의
### 훅이란 무엇인가?
* 클래스형 컴포넌트에서는 생성자에서 state를 정의하고, setState() 함수를 통해 state를 업데이트함
* 예전에 사용하던 함수형 컴포넌트는 별도로 state를 정의하거나, 컴포넌트의 생명주기에 맞춰서 어떤 코드가 실행 되도록 할 수 없었다
* 함수형 컴포넌트에서도 state나 생명주기 함수의 기능을 사용하게 해주기 위해 추가된 기능이 바로 훅
* 함수형 컴포넌트도 훅을 사용하여 클래스형 컴포넌트의 기능을 모두 동일하게 구현 할 수 있게 되었습니다.
* Hook이란 state와 생명주기 기능에 갈고리를 걸어 원하는 시점에 정해진 함수를 실행 되도록 만든 함수를 의미
* 훅의 이름은 모두 use로 시작
* 사용자 정의 훅(custom hook)을 만들 수 있으며, 이 경우에 이름은 자유롭게 할 수 있으나 use로 시작할 것을 권장

### useState
* useState는 함수형 컴포넌트에서 state를 사용하기 위한 Hook
* 다음 예제는 버튼을 클릭 할 때 마다 카운트가 증가하는 함수형 컴포넌트
* 하지만 증가는 시킬 수 있지만 증가 할 때 마다 재렌더링은 일어나지 않는다
* 이럴 때 state를 사용해야 하지만 하숨형에느 없기 때문에 useState()를 사용

### useEffect
* useState와 함께 가장 많이 사용하는 Hook
* 이 함수는 사이드 이펙트를 수행하기 위한 것
* 영어로 side effect는 부작용을 의미합니다. 일반적으로 프로그래밍에서 사이트 이펙트는 개발자가 의도하지 않은 코드가 실행되면서 버그가 발생하는 것
* 하지만 리액트에서는 효과 또는 영향을 뜻하는 effect의 의미에 가깝습니다 
* 예를 들면 서버에서 데이터를 받아오거나 수동으로 DOM을 변경하는 등의 작업을 의미합니다
* 이 작업을 이펙트라고 부르는 이유는 이 작업들이 다른 컴포넌트에 영향을 미칠 수 있으며, 렌더링 중에는 작업이 완료될 수 없기 때문입니다. 렌더링이 끝난 이후에 실행되어야 하는 작업들입니다.
* 클래스 컴포넌트의 생명주기 함수와 같은 기능을 하나로 통합한 기능을 제공합니다.
* (저자는 useEffect가 아니라 effect에 가깝다고 설명하고 있지만, 이것은 부작용의 의미를 잘 못 해석해서 생긴 오해이다. 부작용의 부를 不로 생각했기 때문입니다.)
* (Side effect는 副作用으로 원래의 용도 혹은 목적의 효과외에, 부수적으로 다른 효과가 있는 것을 뜻하는 것입니다.)
* 결국 sideEffect는 렌더링 외에 실행해야 하는 부수적인 코드를 말함
* 예를 들면 네트워크 리퀘스트, DOM 수동 조작, 로깅 등은 정리가 필요 없는 경우들 입니다.
* useEffect()함수는 다음과 같이 사용
* 첫 번째 파라미터는 이펙트 함수가 들어가고, 두 번째 파라미터로는 의존성 배열이 들어갑니다.
* 의존성 배열은 이펙트가 의존하고 있는 배열로, 배열 안에 있는 변수 중에 하나라도 값이 변경되었을 대 이펙트 함수가 실행됩니다.
* 이펙트 함수는 처음 컴포넌트가 렌더링 된 이후, 그리고 재 렌더링 이후에 실행됩니다.
* 만약 이펙트 함수가 마운트와 언마운트 될 때만 한 번씩 실행 되게 하고 싶으면 빈 배열을 넣으면 됩니다.이 경우 props나 state에 있는 어떤 값에도 의존하지 않기 때문에 여러 번 실행되지 않습니다

### useMemo
* useMemo()훅은 Memoized value를 리턴하는 훅입니다.
* 이전 계산 값을 갖고 있기 때문에 연산량이 많은 작업의 반복을 피할 수 있습니다.
* 이 훅은 렌더링이 일어나는 동안 실행됩니다.
* 따라서 렌더링이 일어나는 동안 실행돼서는 안될 작업을 넣으면 안됩니다.
* 예를 들면 useEffect에서 실행되어야 할 사이드 이팩트 같은 것입니다. 
* 의존성 배열을 넣지 않을 경우, 렌더링이 일어날 때 마다 매번 함수가 실행 됩니다.
* 따라서 의존성 배열을 넣지 않는 것은 의미가 없습니다.
* 만약 빈 배열을 넣게 되면 컴포넌트 마운트 시에만 함수가 실행됩니다

### useCallback
* useCallback() 훅은 useMemo()와 유사한 역할을 합니다.
* 차이점은 값이 아닌 함수를 반환한다는 점입니다.
* 의존성 배열을 파라미터로 받는 것은 useMemo와 동일합니다.
* 파라미터로 받은 함수를 콜백 함수라고 합니다.

### use Ref
* useRef() 훅은 레퍼런스를 사용하기 위한 훅
* 레퍼런스란 특정 컴포넌트에 접근 할 수 있는 객체를 의미
* useRef() 훅은 바로 이 레퍼런스 객체를 반환
* 레퍼넌스 객체에는 .current라는 속성이 있는데, 이것은 현재 참조하고 있는 엘리먼트를 의미
* 반환된 레퍼런스 객체는 컴포넌트의 라이프타임 전체에 걸쳐서 유지
* 즉, 컴포넌트가 마운트 해제 전까지는 계속 유지된다는 의미

## 4월 3일 강의

3. Props 사용법
* JSX 에서는 key-value쌍으로 props를 구성합니다.
1. Profile컴포넌트로 name, introduction, viewCount Props를 전달한다.
2. 이때 전달되는 props는 다음과 같은 자바스크립트 객체입니다.
* JSX에서는 중괄호를 사용하면 JS코드를 넣을 수 있다고 배웠습니다.
* JSX를 사용하지 않는 경우 props의 전달 방법은 createElement()함수를 사용하는 것 입니다.
* createElement()함수의 두번째 매개변수가 바로 props입니다.

### 컴포넌트 만들기
1. 컴포넌트의 종류
* 리액트 초기 버전을 사용할 때는 클래스형 컴포넌트를 주로 사용했습니다.
* 이후 Hook이라는 개념이 나오면서 최근에는 함수형 컴포넌트를 주로 사용합니다.
* 예전에 작성된 코드나 문서들이 클래스형 컴포넌트를 사용하고 있기 때문에,
* 클래스형 컴포넌트와 컴포넌트의 생명주기에 관해서도 공부해 두어야 합니다.

2. 함수형 컴포넌트
<!-- function Welcome(props) {
  return <h1>안녕, {props.name}</h1>;
} -->
* Welcome컴포넌트는 props를 받아, 받은 props중 name키의 값을 "안녕", 뒤에 넣어 반환합니다.

3. 클래스형 컴포넌트
Welcome 컴포넌트는 React.

4. 컴포넌트 이름 짓기
* 이름은 항상 대문자로 시작
* 리액트는 소문자로 시작하는 컴포넌트를 DOM태그로 인식하기 때문
* 컴포넌트 파일 이름과 컴포넌트 이름은 같게 한다

5. 컴포넌트의 렌더링
* 렌더링의 과정은 다음 코드와 같습니다.

<!-- function Welcome(props) {
  return <h1>안녕, {props.name}</h1>;
}

const element = <Welcome name="인제" />;
reactDOM.render(
  element,
  document.getElementById('root')
); -->

### 컴포넌트 합성
* 컴포넌트 합성은 여러 개의 컴포넌트를 합쳐서 하나의 컴포넌트를 만드는 것입니다.
* 리액트에서는 컴포너트 안에 또 다른 컴포넌트를 사용할 수 있기 때문에, 복잡한 화면을 여러개의 컴포넌트로 나누어 구현할 수 있음
* props의 값을 다르게 해서 Welcome 컴포넌트를 여러 번 사용함

### 컴포넌트 추출
* 복잡한 컴포넌트를 쪼개서 여러 개의 컴포넌트로 나눌 수도 있습니다.
* 큰 컴포넌트에서 일부를 추출해서 새로운 컴포너트를 만드는 것입니다.
* 실무에서는 처음부터 1개의 컴포넌트에 하나의 기능만 사용하도록 설계하는 것이 좋습니다.

### state
1. state란?
 * State는 리액트 컴포넌트의 상태를 의미
 * 상태의 의미는 정상인지 비정상인지가 아니라 컴포넌트의 데이터를 의미
 * 정확히는 컴포넌트의 변경 가능한 데이터를 의미
 * State가 변하면 다시 렌더링이 되기 때문에 렌더링과 관련된 값만 state에 포함시켜야 합니다

2. state의 특징
 * 리액트만의 특별한 형태가 아닌 단지 자바스크립트 객체일 뿐입니다.
* 함수형에서는 useState()라는 함수를 사용
* state는 변경은 가능하다고 했지만 직접 수정해서는 안됨
* 불가능하다고 생각하는 것이 좋음
* state를 변경하고자 할 때는 setstate()함수를 사용

### 생명주기에 대해 알아보기
* 생명주기는 컴포넌트의 생성 시점, 사용 시점, 종료 시점을 나타냄
* constructor가 실행 되면서 컴포넌트가 생성
* 생성 직후 componentDidMount()함수가 호출
* 컴포넌트가 소멸하기 전까지 여러 번 렌더링함
* 렌더링은 props, setState(), forceUpdate()에 의해 상태가 변경되면 이루어짐
* 렌더링이 끝나면 componentDinUpdate()함수가 호출
* 마지막으로 컴포넌트가 언마운트 되면 componentWillUnmount()함수가 호출

## 3월 27일 강의

### JSX란?
* JSX란 내부적으로 XML/HTML 코드를 자바스크립트로 변환합니다.
* React가 createElement함수를 사용하여 자동으로 자바스크립트로 변환해 줍니다.
* 만일 JSX로 작업할 경우 직접 createElement함수를 사용해야 합니다.
* 앞으로 설명하는 코드를 보면 알 수 있지만 결국 JSX는 가독성을 높여주는 역할을 합니다.

### JSX장점
* 코드 간결
* 가독성 향상
* Injection Attack이라는 불리는 해킹방법을 방어함으로써 보안에 강함

### JSX 사용법
* 모든 자바스크립트 문법을 지원
* 자바스크립트 문법에 XML과 HTML을 섞어서 사용
* 만일 html이나 xml에 자바스크립트 코드를 사용하고 싶으면 {}괄호를 사용
* 태그의 속성값을 넣고 싶을 때는  
  큰따옴표 사이에 문자열을 넣거나 중괄호 사이에 자바스크립트 코드를 넣으면 됨

### 엘리먼트에 대해 알아보기

1. 엘리먼트의 정의
* 엘리먼트는 리액트 앱을 구성하는 요소를 의미
* 공식 페이지에는 "엘리멘트는 리액트 앱의 가장 작은 빌딩 블록들"이라고 설명
* 웹사이트의 경우는 DOM 엘리먼트이며, HTML요소를 의미

### 리액트 엘리먼트와 DOM엘리먼트의 차이
* 리액트 엘리먼트는 Virtual DOM형태를 취하고 있다.
* DOM 엘리먼트는 페이즤 모든 정보를 갖고 있어 무겁습니다.
* 반면 리액트 엘리먼트는 변화한 부분만 갖고 있어 가볍습니다.

2. 엘리먼트의 생김새
* 리액트 엘리먼트는 자바스크립트 객체의 형태로 존재합니다.
* 컴포넌트(Button등), 속성(color 등) 및 내부의 모든 children을 포함하는 일반 JS객체입니다.
* 이 객체는 마음대로 변경할 수 없는 불변성을 갖고 있습니다.

3. 엘리먼트의 특징
* 리액트 엘리먼트의 가장 큰 특징은 불변성
* 한 번 생성된 엘리먼트의 내용을 마음대로 바꿀 수 없다

4. 엘리먼트 랜더링하기
* Root DOM node -> HTML코드는 id값이 root인 div태그로 단순하지만 리액트에 필수로 들어가는 아주 중요한 코드
* 엘리먼트를 렌더링하기 위해서는 다음과 같은 코드 필요  
-> ReactDom.render

### 컴포넌트에 대해 알아보기
* 리액트는 컴포넌트 기반의 구조
* 컴포넌트 구조라는 것은 작은 컴포넌트가 모여 큰 컴포넌트를 구성하고, 다시 이런 컴포넌트들이 모여서 전체 페이지를 구성한다는 것을 의미
* 컴포넌트 재사용이 가능하기 때문에 전체 코드의 양을 줄일 수 있어 개발 시간과 유지 보수 비용도 줄일 수 있습니다.
* 컴포넌트는 자바스크립트 함수처럼 입력과 출력이 있다는 면에서는 유사
* 입력은 Props가 담당하고, 출력은 리액트 엘리먼트의 형태로 출력
* 엘리먼트를 필요한 만큼 만들어 사용한다는 면에서는 객체 지향의 개념과 비슷합니다.

### Props
* props는 prop(property: 속성, 특성)의 준말
* props가 바로 컴포넌트의 속성
* 컴포넌트에 어떤 속성, props를 넣느냐에 따라서 속성이 다른 엘리먼트가 출력
* props는 컴포넌트에 전달 할 다양한 정보를 담고 있는 자바스크립트 객체
* 에어비앤비의 예도 마찬가지

2. Props의 특징
* 읽기 전용, 변경할 수 없다는 의미
* 속성이 다른 엘리먼트를 생성하려면 새로운 props를 컴포넌트에 전달

Pure 함수 VS Impure 함수
* Pure함수는 인수로 받은 정보가 함수 내부에서도 변하지 않는 함수
* Impure함수는 인수로 받은 정보가 함수 내부에서 변하는 함수



## 3월 20일 강의

### 리액트는 무엇인가?
1. 리액트의 정의  
사용자 인터페이스를 만들기 위한 자바스크립트 라이브러리
* 2023 - 웹 및 앱 유저 인터페이스를 위한 라이브러리

2. 다양한 자바스크립트 UI 프레임워크: [Stack Overflow trends](https://insights.stackoverflow.com/trends?tags=r%2Cstatistics)

3. 리액트 개념 정리
* 복잡한 사이트를 쉽고 빠르게 만들고, 관리하기 위해 만들어진 것이 바로 리액트입니다.
* 다른 표현으로는 SPA를 쉽고 빠르게 만들 수 있도록 해주는 도구라고 생각하면 됩니다.

### 리액트의 장점
1. 빠른 업데이트와 렌더링 속도
* 이것을 가능하게 하는 것이 바로 Virtual DOM입니다.
* DOM(Document Object Model)이란 XML, HTML 문서의 각 항목을 계층으로 표현하여 생성, 변형, 삭제할 수 있도록 돕는 인터페이스입니다. 이것은 W3C의 표준입니다.
* Virtual DOM은 DOM 조작이 비효율적인 이유로 속도가 느리기 때문에 고안된 방법입니다.
* DOM은 동기식, Virtual DOM은 비동기식 방법으로 렌더링을 합니다.

2. 컴포넌트 기반 구조
* 리액트의 모든 페이지는 컴포넌트로 구성됩니다.
* 하나의 컴포넌트는 다른 여러 개의 컴포넌트의 조합으로 구성할 수 있습니다.
* 그래서 리액트로 개발을 하다 보면 레고 블록을 조립하는 것처럼 컴포넌트를 조합해서 웹사이트를 개발하게 됩니다.
* 아래 그림은 에어비앤비 사이트 화면의 컴포넌트 구조입니다. 재사용성이 뛰어납니다.

3. 재사용성
* 반복적인 작업을 줄여주기 때문에 생산성을 높여 줍니다.
* 또한 유지보수가 용이합니다.
* 재사용이 가능 하려면 해당 모듈의 의존성이 없어야 합니다.

4. 든든한 지원군
* 메타(구 페이스북)에서 오픈소스 프로젝트로 관리하고 있어 계속 발전하고 있습니다.

5. 활발한 지식 공유 & 커뮤니티  

6. 모바일 앱 개발가능
* 리액트 네이티브라는 모바일 환경 UI프레임워크를 사용하면 크로스 플랫폼(cross-platform) 모바일 앱을 개발할 수 있습니다.

### 리액트의 단점
1. 방대한 학습량
* 자바스크립트를 공부한 경우 빠르게 학습할 수 있습니다.

2. 높은 상태 관리 복잡도
* state, component life cycle 등의 개념이 있지만 그리 어렵지 않습니다.

## 3월 13일 강의

### HTML 살펴보기
1. HTML이란 무엇인가?  
[HTML](https://namu.wiki/w/HTML)

2. 웹사이트의 뼈대를 구성하는 태그들

3. SPA(Single Page Application)  
[SPA](https://www.startupcode.kr/company/blog/archives/11)

### CSS란 무엇인가?
[CSS](https://namu.wiki/w/CSS)

### 자바스크립트
1. 자바스크립트란 무엇인가?
* var - 중복 선언 가능, 재할당 가능
* let - 중복 선언 불가능, 재할당 가능
* const - 중복 선언 불가능, 재할당 불가능
* Array type - 배열
* Object type
2. ES6 (ECMAScript6)  
   [표준 ECMA-262]() (ECMA스크립트는 ECMA-262에 의해 표준화된 언어의 이름)
3. 자바스크립트의 자료형
4. 자바스크립트의 연산자
5. 자바스크립트 함수
* function statement 형: 일반적 함수의 형태
* Arrow function expression 형: 화살표 함수