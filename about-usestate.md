


 # react hook의 usestate가 어떻게 내부적으로 동작하는가?



그동안 깃헙에 올리지는 않았지만 알고리즘공부와 같이 react로  project를 만들던 도중에 내가 이것을 진짜 알고 쓰는 게 아니라 그냥 남의 코드 배껴오는 것에서  현타가 왔다. 그래서 usestate가 어떻게 동작 하는지에 대하여 알아 보았다.





앞으로 react 공식문서의 설명을 다시한번 곱씹어볼 예정이다. fe 개발자로 하엳금 편하게 만들어진 jsx등도 정확히 어떻게 동작하는지 공부하겠다


## usestate는 어떻게 동작하는가?

react hook의 usestate는 closure로 이루어저 있다. 

clousre는 c++의 private과 비슷하다고 이해하였는데 왜쓰이는가를 찾아보면 변수를 private하게 사용하여 변수충돌이 일어나는 것을 막게 하기 위해서 이다.

closure라는 개념으로도 하나의 readmefile이 생기므로 다음 시간에 다루도록 하겠다.

만약 이글이 이해가 되지 않는다면 js의 executional context와 clousre개념을 숙지하고 읽어보기를 바란다.
 
```
function useState(initialValue) {
  var _val = initialValue // _val은 useState에 의해 만들어진 지역 변수입니다.
  function state() {
    // state는 내부 함수이자 클로저입니다.
    return _val // state()는 부모 함수에 정의된 _val을 참조합니다.
  }
  function setState(newVal) {
    // 마찬가지
    _val = newVal // _val를 노출하지 않고 _val를 변경합니다.
  }
  return [state, setState] // 외부에서 사용하기 위해 함수들을 노출
}
var [foo, setFoo] = useState(0) // 배열 구조분해 사용
console.log(foo()) // 0 출력 - 위에서 넘긴 initialValue
setFoo(1) // useState의 스코프 내부에 있는 _val를 변경합니다.
console.log(foo()) // 1 출력 - 동일한 호출하지만 새로운 initialValue

```
위 예제에서 볼수 있드시 js의 동작원리에 의해(js는 c처럼 top-down방식으로 코드를 읽으면 안된다) 

1st.usestate, var [foo,setFoo]가 먼저 변수로 등록이되고 

2.console.log(foo)->setFoo(1)->console.log(foo)가 순차적으로 실행된다.

3.그러면 console.log에서 foo를 호출하게될 떄 main flow가 foo로 옮겨지게 된다.
[

4.https://velog.io/@tjdgus3160/React-Virtual-DOM)
