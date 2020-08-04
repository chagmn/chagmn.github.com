# About Swift (Part.2)
## Optional (옵셔널, 선택적 값 타입)
옵셔널 값이 없을 수도 있는 상황에서 사용한다. 옵셔널은 두 가지 가능성을 표현한다. 첫 번째는 값이 있어서 그 옵셔널이 감싸고 있는 값에 접근할 수 있는 경우이고, 두 번째는 값이 아예 있지 않은 경우가 있다.<br>

옵셔널을 사용해 값이 없는 상황에 대처하는 방법을 알아보려고 한다. Swift의 `Int`타입은 `String`값을 `Int`값으로 변환할 수 있는 초기자(Initializer) 를 갖고 있다. 하지만, 모든 문자열을 정수로 변환할 수 있는 것은 아니다. 문자열 `"123"`은 정수`123`으로 변환할 수 있지만, 문자열 `"hello~"`는 변환할 만한 확실한 수치 값을 갖고 있지 않다.<br>

```
let strNumber = "123"
let intNumber = Int(strNumber)
// intNumber 의 타입은 Int or Optional Int 라고 추론
```

초기자(Initializer) 가 추론을 실패할 수도 있기 때문에 `Int`가 아니라, `Optional Int`를 반환한다. 이것은 `Int?`라고 쓴다. 물음표가 바로 이 값이 옵셔널을 갖고 있음을 나타낸다. 다시 말하면 이것은 "어떤 `Int`값을 갖고 있거나, 아니면 갖고 있는 값이 없다" 라는 것을 의미한다.

## Nil
옵셔널 변수에 값이 없는 상태를 설정하려면 `nil`이라는 특수한 값을 할당하면 된다. 이는 C언어 `null`과 비슷하다.<br>

```
var value: Int? = 100
// value는 100이라는 실제 Int 값을 갖는다.
value = nil
// value는 이제 아무 값도 갖지 않는다.
```

옵셔널 변수를 정의할 때 기본 설정 값을 제공하지 않으면 , 그 변수는 자동으로 `nil`로 설정된다.<br>
```
var temp: String?
// temp는 자동으로 nil로 설정
```

## If Statements and Forced Unwrapping (If문과 강제 포장 풀기)
