---
title: "About Swift5.2 (Part.2)"
date: 2020-8-4
categories: iOS&Swift
---

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

### Nil
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

### If Statements and Forced Unwrapping (If문과 강제 포장 풀기)
`If`문을 사용하면 옵셔널을 `nil`과 비교해 옵셔널이 값을 갖는지 확인할 수 있다. 이러한 비교는 `==`이나 `!=`를 사용해 수행할 수 있다.<br>

```
if intNumber != nil {
	print("This value is integer value.")
}
// This value is integer value. 를 출력
```

옵셔널이 값을 갖고 있다고 확실하는 경우, 옵셔널 이름 끝에 느낌표(`!`)를 추가해 그것의 실제 값에 접근할 수 있다. 느낌표는 "나는 이 옵셔널이 값을 갖고 있음을 정확히 알고 있으니깐 그걸을 이용하세요." 라는 의미를 갖고 있다. 이것을 옵셔널 값에 대한 '강제 포장 풀기(forced unwrapping)' 이라고 한다.

```
if intNumber != nil {
	print("This has an integer value of  \(intNumver!).")
}
// This has an interger value of 123. 을 출력
```

### Optional Binding (옵셔널 연결)
옵셔널 연결을 사용하면 옵셔널이 값을 갖는지 확인해, 갖는 경우 그 값을 임시 상수나 변수의 형태로 사용하게 할 수 있다. 옵셔널 연결을 `if`와 `while`문과 같이 사용하면, 옵셔널 안의 값을 검사하고 그 값을 상수나 변수로 추출하는 것을 단 한번의 동작으로 할 수 있다. <br>

if문에서 옵셔널 연결을 작성하는 방법은 다음과 같다.<br>

```
if let constanName = someOptional {
	statements
}
```

옵셔널에 있는 `possibleNumber` 예제는 강제 포장 풀기 (forced unwrapping) 대신 옵셔널 연결 (optional binding) 을 써서 다음과 같이 고칠 수 있다.<br>

```
if let actualNumber = Int(possibleNumber) {
	print("String \(possibleNumber) has an integer value of \(actualNumvber)")
} else {
	print("String \(possibleNumber) couldn't be converted to an interger")
}
// String 123 has an interger value of 123 을 출력
```

이 코드를 해석하면<br>
-> `Int(possibleNumber)`에서 반환환 옵셔널 `Int`가 값을 갖고 있으면 그 옵셔널이 갖고 있는 값을 새로운 상수 `actualNumber`에 저장해라.<br>

이 변환에 성공하면 `if`문의 첫 번째 분기에서 `actualNumber`상수를 사용하게 되고 이것은 옵셔널 속에 있던 값으로 이미 초기화 됐으므로, 값에 접근할 때 `!`를 사용할 필요가 없다. 여기서, `actualNumber`는 단순히 반환 결과를 출력하는데 사용되고 있다.<br>

옵셔널 연결에는 상수와 변수 모두 사용 가능하다. `if`문의 첫 번째 분기 내에서 `actualNumber`값을 조작하고 싶으면, `if var actualNumber`를 대신 사용해 옵셔널이 갖고 있는 값을 상수가 아닌 변수로 사용하게 끔 할 수도 있다.<br>
단일 `if`문에 쉼표로 분리하는 방법을 쓰면, 옵셔널 연결과 참 거짓 조건을 원하는 만큼 포함 시킬 수 있따. 옵셔널 연결 중 어떤 값이든 `nil`이거나 혹은 어떤 Boolean 조건이든 `false`로 계산된다면, 전체 `if`문의 조건은 `false`인 것으로 고려된다. 다음의 `if`문들은 서로 동등하다. <br>

```
if let firstNum = Int("4"), let secondNum = Int("42"), firstNum < secondNum && secondNum <100 {
	print("\(firstNum) < \(secondNum) < 100")
}
// 4 < 42 < 100 을 출력

if let firstNum = Int("4"){
	if let secondNum = Int("42"){
		if firstNum  < secondNum && seondNum < 100 {
			print("\(firstNum) < \(secondNum) < 100")
		}
	}
}
// 4 < 42 < 100 을 출력
```

`if`문 안의 옵셔널 연결에서 만든 상수와 변수는 `if`문의 본문 내에서만 사용 가능하다. 이와 달리, `guard`문에서 만든 상수와 변수는 `guard`문 이후의 코드 줄에서도 사용 가능한데, 이는 조기 탈출 구문(Early Exit) 에 추가로 설명하려 한다.

### Implicitly Unwrapped Optionals (암시적으로 포장이 풀리는 옵셔널)
옵셔널은 앞서 말한 것 처럼 상수나 변수가 값을 없을 수 있는 가능성을 나타낸다. 옵셔널은 `if`문을 사용해 값이 존재하는지를 검사할 수 있으며, 옵셔널 값에 접근하기 위해 옵셔널 연결을 사용해 값의 유무에 따른 conditionally unwrapped 를 할 수도 있다.<br>

프로그램의 구조상, 최초로 값을 설정한 후, 옵셔널은 항상 값을 갖는다는 것은 분명하다. 이와 같은 경우, 항상 값이 있다고 가정해도 안전하므로, 접근할 때마다 옵셔널 값을 검사하고 풀고할 필요가 없는 것이 유용할 것 이다.<br>

이러한 종류의 옵셔널을 'implicitly unwrapped optionals (암시적으로 포장이 풀리는 옵셔널)' 이라고 정의한다. 암시적으로 포장이 풀리는 옵셔널을 작성하려면 옵셔널로 만들고 싶은 타입 뒤에 물음표 (`Int?`) 대신에 느낌표 (`Int!`) 를 붙이면 된다. <br>

암시적으로 포장이 풀리는 옵셔널도 속을 보면 보통의 옵셔널이지만, 옵셔널이 아닌 값처럼 사용할 수 있으며, 접근할 때마다 옵셔널 값을 풀 필요도 없다. 다음 예제는 '옵셔널 문자열' 과 '암시적으로 포장이 풀리는 옵셔널 문자열' 에서 감싸고 있는 값을 명시적인 `string`으로 접근할 때의 동작이 어떻게 다른지 보여준다. <br>

```
let possibleStr: String? = "Optional String."
let forcedStr: String = possibleStr! // 느낌표 필요

let assumedStr: String! = "Implicitly unwrapped optional string."
let implicitStr: String = assumedStr // 느낌표 필요없음
```
암시적으로 포장이 풀리는 옵셔널은 사용될 때마다 자동으로 옵셔널이 풀리는 권한을 부여받았다고 생각할 수 있다. 이는 사용할 때마다 옵셔널 이름 뒤에 느낌표를 붙이는 것이 아니라, 선언할 때 옵셔널을 선언할 때 그 타입 뒤에 느낌표를 붙이는 것에 해당한다.<br>

암시적으로 포장이 풀리는 옵셔널은 여전히 보통의 옵셔널처럼 취급하는 것도 가능해 다음 처럼 값을 갖고 있는지 검사하는 것도 가능하다. <br>
```
if assumedStr != nil {
	print(assumedStr)
}
// Implicitly unwrapped optional string. 을 출력
```

옵셔널 연결과 암시적으로 포장이 풀리는 옵셔널을 같이 사용해, 단일한 구문으로 값을 검사하고 풀기까지 할 수도 있다. <br>
```
if let definiteStr = assumedStr {
	print(definiteStr)
}
// Implicitly unwrapped optional string. 을 출력
```

## Error Handling (에러 처리)
에러 처리를 사용하면 프로그램 실행 중에 마주치는 에러 조건들에 대응 할 수 있다.<br>

값의 있고 없음으로 함수의 성공과 실패라는 정보를 전달할 수 있는, 옵셔널과는 다르게, 에러 처리는 실패의 실제 원인을 확인할 수 있도록 해주며, 필요하다면, 그 에러를 프로그램의 다른 부분으로 전파할 수 있도록 해준다.<br>

하나의 함수는 에러 조건과 마주쳤을 때, 에러를 던진다(throws). 그 함수를 호출한 쪽에서 에러를 잡아내고(catch) 적절하게 응답할 수 있다. <br>

```
func canThrowAnError() throws {
	// 이 함수가 에러를 던질수도 있고 아닐 수도 있다.
}
```

함수가 에러를 던질 수 있다고 표시하려면 함수를 선언할 때 `throws`키워드를 포함시키면 된다. 에러를 던질 수 있는 함수를 호출할 때는, 표현식에 `try`키워드를 먼저 붙여야 한다.<br>

Swift는 `catch`절에서 처리될 때까지 에러를 자동으로 현재 범위 밖으로 전파한다.

```
do {
	try canThrowAnError()
	// 어떠한 에러도 던져지지 않았다.
} catch {
	// 어떤 에러가 던져 졌다.
}
```

`do`구문은 새로운 포함 범위를 만들어서, 에러가 하나 이상의 `catch`절로 전파되도록 한다. <br>

```
func makeSandwich( ) throws {
	//...
}

do {
	try makeSandwich()
	eatSandwich()
} catch SandwichError.outOfCleanDishes {
	washDishes()
} catch SandwichError.missingIngredients (let ingredients) {
	buyGroceries(ingredients)
}
```
이 예제에서, `makeSandwich()`함수는 남아있는 깨끗한 접시가 하나도 없거나 하나라도 재료가 빠진 경우 에러를 던질 것이다. `makeSandwich()`가 에러를 던질 수 있기 때문에, 이 함수 호출을 `try`표현식으로 감쌌다. `do`구문 속에 함수 호출을 감쌌기 때문에, 어떤 에러가 던져져도 이미 제공한 `catch`절로 전파될 것 이다. <br>

에러가 던져지지 않으면, `eatSandwich()`함수를 호출한다. 에러가 던져지고 이것이 `SandwichError.outOfCleanDishes` 경우이면, `washDishes( )`함수를 호출한다. 에러가 던져지고 이것이 `SandwichError.missingIngredients`경우이면, 그 때는 `catch`에 붙잡힌 `[String]`결합 값을 가지고 `buyGroceries(_:)`함수를 호출하게 된다.

## Assertions and Preconditions (단언과 선행 조건)
단언 (Assertions) 과 선행 조건 (Preconditions) 은 '실행 시간(Runtime) 에 하는 검사' 이다. 이를 사용하면 그 다음 코드를 실행하기 전에 필수 조건을 만족하고 있는지 확인할 수 있다. 단언이나 선행 조건에 있는 Boolean 조건이 `true`로 계산되면, 코드 실행이 평소처럼 계속된다. 조건이 `false`로 계산되면, 프로그램의 현재 상태가 무효한 것이다. 코드 실행은 중지되고, 앱을 종료한다.<br>

단언과 선행 조건을 사용하면 코딩 중에 만들게 된 가정과 코딩 중에 가졌던 기대값을 표현할 수 있고, 이들을 코드의 일부로 포함할 수 있다. 단언은 개발 중에 실수한 것과 잘못된 가정을 찾도록 도와주며, 선행 조건은 제품의 문제점을 미리 감지할 수 있도록 도와준다.<br>

실행 시간에 기대값을 검증하는 용도 외에도, 단언과 선행 조건은 코드 내에서 문서활를 할 수 있는 편리한 양식이기도 하다. 앞에 에러 처리에서 설명한 에러 조건과는 다르게, 단언과 선행 조건은 복구 가능하거나 예상 가능한 에러에 사용하는 것이 아니다. 실패한 단언이나 선행 조건은 프로그램의상태가 유효하지 않음을 나타내는 것이기 때문에, 실패한 단언을 잡아내는 방법이란 건 없다. <br>

단언과 선행 조건은 예기치 않게 무효한 조건이 발생하는 상황에 대한 코드 설계인 것이 아니다. 하지만, 유효한 데이터와 상태에 이를 적용하는 것은 무효한 상태가 발생했을 때 앱이 종료될 것임을 예측 가능하게 해주고, 문제를 더 쉽게 고칠 수 있도록 해준다. 무효한 상태가 감지되자마자 즉시 실행을 중지하는 것은 무효한 상태로 인한 피해를 줄이는 데도 도움이 된다.<br>

단언과 선행 조건의 차이점은 검사되는 시점에 있다. 단언은 오직 디버그 빌드시에만 검사되지만 선행 조건은 디버그와 제품 빌드시 모두에서 검사된다. 제품 빌드 시에는, 단언 내의 조건은 계산되지 않는다. 이것은 개발 과정에서 원하는 만큼 단언을 많이 사용할 수 있으며, 이 경우에도 제품의 성능에는 영향을 주지 않는다는 것을 의미한다.

### Debugging with Assertions (단언으로 디버깅하기)
단언을 작성하려면 Swift 표준 라이브러리에 있는 `asset(_:_:file:line:)`함수를 호출하면 된다. 이 함수에 `true`나 `false`로 계산될 표현식과 조건 결과가 `false`일 때 표시할 메시지를 전달한다.<br>

```
let age = -3
asset(age >= 0, "Person's age can't be less than zero.")
// 이 단언은 -3 이 >= 0 이 아니기 때문에 실패
```

위 예제에서, 코드 실행을 계속하려면 `age >= 0`이 `true`로 계산되거나, `age`값이 음수가 아니여야 한다. 만약 `age`의 값이 음수면, `age >= 0`에서 `false`로 계산되어서, 단언이 실패하고 응용 프로그램을 종료한다. <br>

단언의 메시지는 생략할 수 있다. 조건 자체를 글자 그대로 표시하려고 할 수도 있을 것이다. <br>
```
assert(age >= 0 )
```
코드가 이미 조건을 검사한 경우, `assertionFailure(_:file:line:)`함수를 사용해 단언이 실패했음을 나타낼 수 있다. 예를 들면 다음과 같다.<br>
```
if age > 10 {
	print("Can ride roller-coaster or the firt ferris wheel.")
} else if age >= 0 {
	print("Can ride the ferris wheel.")
} else {
	assertionFailure("Can't be less than zero.")
}
```

### Enforcing Preconditions (선행 조건 강제하기)
코드를 계속 실행하려면 분명히 참이여만 하지만, 잠재적으로 거짓이 될 수 있는 조건이 있을 때마다 선행 조건을 사용하기 바란다. 예를 들어, 선행 조건으로 첨자 연산이 범위를 벗어나지 않았는지 검사하거나 , 함수에 유효한 값이 전달되었는지 검사하도록 한다. <br>

선행 조건을 작성하려면 `precondition(_:_:file:line:)`함수를 호출하면 된다. 이 함수에 `true`나 `false`로 계산될 표현식과 조건 결과가 `false`일 때 표시할 메시지를 전달한다. <br>

```
// 첨자 연산의 구현 내부에서...
precondition( index > 0, "Index must be greater than zero.")
```

`preconditonFailure(_:file:line:)`함수 호출로도 실패가 발생했음을 나타낼 수 있다. 예를 들어, 모든 유효한 데이터들은 switch의 다른 cases에서 처리됐어야 함에도 , switch의 default case에 들어온 상황이 그런 것이다.<br>



## 정리 후..
> 여기까지 Swift문서 Basic부분을 정리했다. 몇 부분은 빠졌을 수도 있는데 자료형을 표현하는 부분이여서 제외를 했다. 
> 그래도 정리하면서 Swift의 특징이나 기본적인 부분에 대해서 많이 알 수 있었고 더 나아가 Swift로 작성할 나의 iOS 어플들을 좀더 클린한 코드로 작성할 수 있도록 Swift 버전이 업데이트 될 때마다 추가되는 부분은 정리하려고 한다. <br>
> 감사합니다.
