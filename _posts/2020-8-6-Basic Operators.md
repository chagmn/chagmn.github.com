---
title: "Basic Operators"
date: 2020-8-6
categories: Swift5
---

# Basic Operators (기본 연산자)
연산자는 값을 검사하고 바꾸고 결합하는데 사용하는 특별한 기호나 문자이다. 예를 들어 더하기 연산자 (+)는 `let i = 1 + 2`처럼 두 숫자를 더하거나, 논리적 AND 연산 (&&)은 `if enterdDoorCode && passedRetinaScan`처럼 두개의 부울 값을 결합하는데 사용한다.<br>

Swift는 C언어로부터 이미 우리가 알고있는 연산자들을 지원하고 흔한 코딩 에러를 제거하기 위해 몇 개의 능력을 향상 시켰다. 할당 연산자 (=)는 동등 연산자 (==) 대신에 잘못 사용되었을 경우를 방지하기 위해 값을 반환하지 않는다. 산술 연산자 (+, -, *, /, % 등등) 값의 오버플로를 검사하고 허락하지 않고, 숫자들을 저장하는 타입의 허용된 값의 범위보다 더 크거나 더 작은 숫자들과 작업을 할 때 발생할 수 있는 예기치 않은 결과를 발생하지 않도록 한다. <br>

Swift는 또한 값의 범위를 쉽게 표현하기 위한 `a..<b 와 a...b`와 같은 C언어에서는 보지 못한 범위 연산자를 제공한다.  <br>

## Terminology (술어)
연산자는 단항, 이항 또는 삼항이다: <br>

* 단항 연산자는 `-a`와 같이 하나의 목표를 두고 작동한다. 단항 접두 연산자는 `!b`와 같이 그들의 목표 전에 나타나고 단항 접미 연산자는  `c!`처럼 그들의 목표 뒤에 나타난다. <br>
* 이항 연산자는 `2 + 3` 처럼 두 개의 목표를 두고 작동하고 접사다. 왜냐하면 그들은 두 개의 목표 사이에 나타나야 하기 때문이다. <br>
* 삼항 연산자는 3개의 목표를 두고 작동한다. C언어와 같이, Swift는 오직 하나의 삼항 연산자를 갖고 있다. `a ? b : c`와 같은 삼항 조건 연산자이다.<br>

연산자들이 영향을 미치는 값들은 피연산자들이다. 표현식 `1 + 2`에서 `+`기호는 이항 연산자고 `1`과 `2` 두 개의 피연산자가 있다.

## Assignment Operator (할당 연산자)
할당 연산자 `a = b`는 `a`의 값을 `b`의 값으로 초기화하거나 업데이트 해준다.<br>

```
let b = 10
var a = 5
a = b
// a에는 10이 할당된다.
```

만약 할당에서 오른쪽이 다수의 값을 같는 튜플이라면, 요소들은 한번에 다수의 상수나 변수로 분해 될 수 있다. <br>

```
let (x, y) = (1, 2)
// x는 1이 할당, y는 2가 할당
```

할당 연산자는 C언어나 Obejective-C와는 다르게 Swift에서 할당 연산자는 스스로 값을 반환하지 않는다. 아래 예시는 유효하지 않다.<br>

```
if x = y {
	//  유효하지 않다. 왜냐하면 x = y 는 값을 반환하지 않기 때문.
}
```

이 특징은 동등 연산자(==)가 실제로 의도된 경우 할당 연산자(=)가 실수로 사용되는 것을 방지한다. `if x = y`가 유효하지 않다면, Swift는 코드에서 이러한 에러들을 피할 수 있도록 도와준다.

## Arithmetic Operators (산술 연산자)
Swift는 모든 숫자형에대해 4개의 표준 산술 연산자들을 제공한다. <br>

* 덧셈 (+)
* 뺄셈 (-)
* 곱셈 (*)
* 나눗셈 (/)

```
1 + 2   // 3
5 - 2   // 3
2 * 3   // 6
10.0 / 2.5   // 4.0
```

C언어나 Obejective-C의 산술 연산자와는 다르게, Swift 산술 연산자들은 기본적으로 값들이 오버플로우 되는 것을 허용하지 않는다. Swift의 오버플로우 연산자를 사용해 `a &+ b` 처럼 값에 오버플러우 행위 옵션을 줄 수가 있다. 자세한건 Overflow Operators에서 다룬다.  <br>

덧셈 연산자는 `String` 결합(연쇄)를 지원한다.<br>

`"hello" + "world"   // "hello world"`

##Remainder Operator (나머지 연산자)
`a % b` 같은 나머지 연산자는 `a`안에 `b`의 배수가 몇개인지 작동하고 나머지로 알려진 남은 값을 반환한다. <br>

Swift에서는 이렇게 사용된다:<br>
`9 % 4   // 1`

`a % b`의 답을 결정하기 위해서, `%`연산자는 아래 방정식을 따르고 그것의 출력 값으로 나머지를 반환한다. <br>

`a = (b x 배수) + 나머지`

배수에는 `a`안에 `b`의 배수 중 가장 큰 값이고 9와 4를 방정식에 대입하면: <br>
`9 = (4 x 2) + 1`

같은 방법으로 `a`가 음수 값이여도 나머지를 계산할 수 있다.<br>

`-9 = (4 x 2) + -1`

나머지는 -1이 된다.<br>

`b`는 음수 기호를 무시한다. 이 말은 `a % b`와 `a % -b`은 항상 같은 답을 준다는 말이다.<br>

##Unary Minus Operator (단항 뺄셈 연산자)
숫자 값은 단항 뺄셈 연산자로 알려진 접두사 `-`를 사용해 토글할 수 있다. <br>

```
let three = 3
let minusThree = -three    // minusThree는 -3
let plusThree = -minusThree    // plusThree는 3이거나 minus minus three
```
 
 단항 뺄셈 연산자(-)는 공백없이 작동하는 값 바로 앞에 쓰인다.<br>
 
##Unary Plus Operator (단항 덧셈 연산자)
단항 덧셈 연산자(+)는 간단하게 그것이 작동하면 어떠한 변화 없이 값을 반환한다. <br>

```
let minusSix = -6
let alsoMinusSix = +minusSix   // alsoMinusSix는 -6
``` 

비록 단항 덧셈 연산자가 실제로 아무것도 안하지만, 음수에 단항 뺄셈 연산자를 사용했을때 코드에서 양수에 대해 대칭을 제공할 때 사용할 수 있다.<br>

##Compound Assignment Operators (복합 할당 연산자)
C언어 처럼, Swift는 할당 연산자(=)와 다른 연산자가 결합한 복합 할당 연산자를 제공한다. 하나의 예가 덧셈 할당 연산자이다. (+=):<br>

```
var a = 1
a += 2
// a는 3
```

표현식 `a += 2`은 `a = a + 2`을 짧게 표현한 것이다. 효과적으로 더하고 할당은 동시에 양쪽의 작업을 수행하기 위해 하나의 연산자로 결합했다.

##Comparison Operators (비교 연산자)
Swift는 다음과 같은 비교 연산자들을 제공한다:<br>

*  같다 (`a == b`)
*  같지 않다 (`a != b`)
*  더 크다 (`a > b`)
*  더 작다 (`a < b`)
*  더 크거나 같다 (`a >= b`)
*  더 작거나 같다 (`a <= b`)

각 비교 연산자들은 문장이 true인지 아닌지를 가리키는 `bool`값을 반환한다.

```
1 == 1   // true
2 != 1   // true
2 > 1    // true
1 < 2    // true
1 >= 1   // true
2 <= 1   //false
```

비교 연산자는 종종 `if`문과 같은 조건문에 사용된다:

```
let name = "world"
if name == "world" {
	print("hello world")
} else {
	print("i'm soory \(name), but i don't recognize you")
}
// hello world 가 출력
```
 
 만약 값의 수가 같고 타입이 같다면 두개의 튜플들을 비교할 수 있다. 튜플들은 비교식이 두 값이 같지 않을 때 까지 한번에 하나의 값이 왼쪽에서 오른쪽으로 비교된다. 두 개의 값이 비교되고, 튜플 비교의 결과에 따라 비교식의 전체 결과가 결정된다. 만약 모든 요소가 같다면, 그땐 튜플 스스로 같다.
 
 ```
 (1, "zebra") < (2, "apple")   // true. 1은 2보다 작고, zebra와 apple은 비교 x
 (3, "apple") < (3, "bird")    // true. 3과 3은 같고, apple은 bird 보다 작다
 (4, "dog") == (4, "dog")      // true. 4와 4는 같고 dog고 서로 같다.
 ```
 
 위의 예시에서, 첫 행에서 왼쪽에서 오른쪽으로 비교하는 행동을 볼 수 있다. 왜냐하면 1은 2보다 작아서 튜플에서 다른 어떤 값과 상관없이 `(1, "zebra")`는 `(2, "apple")`보다 작다고 여겨진다. `"zebra`가 `"apple"`보다 작지 않다는 것은 중요하지 않다. 왜냐하면, 비교식은 이미 튜플의 첫번째 요소에 의해 결정되었기 때문이다. 그러나, 튜플의 첫번째 요소가 같은 경우, 그들의 두번째 요소를 비교한다. 이것은 두번째, 세번째 행에서 나타난다. <br>
 
 튜플은 연산자가 각각의 튜플에서 각각의 값에 적용 할 수있는 경우에만 지정된 연산자와 비교 할 수 있다. 예를 들어, 아래처럼, `(String, Int)`타입의 튜플은  `String`과 `Int`가 `<`연산자를 가지고 비교 할 수 있기 때문에 비교가 가능하다. 반면에, `(String, Bool)`타입의 두 개의 튜플은 `<`연산자가 `Bool`타입의 값을 비교 할 수 없기 때문에 비교 할 수 없다.
 
```
("blue", -1) < ("purple", 1)    // 가능
("blue", false) < ("purple", true)  //불가능. Boolean은 < 로 비교 못해서
```

##Ternary Conditional Operator (삼항 조건 연산자)
삼항 조건 연산자는 `질문 ? 답 1 : 답 2`같은 모양을 가진 3개 부분으로 이루어진 특별한 연산자이다. 이것은 `질문`이 참인지 거짓인지에 따라 두 표현식 중 하나를 평가하는 단축어다. 만약 `질문`이 참이면, `답 1`이고 그 값을 반환하지만 반대면 `답 2`로 평가되어 이 값을 반환한다. <br>

삼항 조건 연산자는 아래 코드를 짧게 했다: 

```
if 질문 {
	답 1
} else {
	답 2
}
```

테이블의 행의 높이를 계산하는 예시가 있다. 행의 헤더가 있는 경우 행 높이가 컨텐츠 높이보다 50 포인트 높아야하고 행에 헤더가 없으면 20 포인트 높아햐 한다.

```
let contentHeight = 40
let hasHeader = true
let rowHeight = contentHeight + (hasHeader ? 50 : 20)
// rowHeight는 90
```

위의 예시는 아래 코드를 짧게 표현한 것이다.

```
let contentHeight = 40
let hasHeader = true
let rowHeight: Int
if hasHeader {
	rowHeight = contentHeight + 50
} else {
	rowHeight = contentHight + 20
}
// rowHeight는 90
```

삼항 조건 연산자는 고려되는 두개의 식 중 하나를 결정하는데 효율적인 단축을 제공한다. 그러나, 삼항 조건 연산자는 주의해서 사용해야 한다. 만약 과다하게 사용되면 간결함은 코드를 읽는데 어려움을 줄 수 있다.  삼항 조건 연산자의 다수의 인스턴스들을 하나의 복합식으로 결합하는 것을 피해야 한다.

##Nil-Coalescing Operator(Nil 병합 연산자)
Nil 병합 연산자 `(a ?? b)`는 값을 갖으면 `a`를 풀거나  `a`가 `nil`이면 `b` 기본 값을 리턴한다. `a`는 항상 옵셔널 타입이여야 한다. `b`는 `a`에 저장된 것과 반드시 타입이 같아야 한다.<br>

Nil 병합 연산자는 아래 코드를 간단하게 했다:

`a != nil ? a! : b`

위 코드는 삼항 조건 연산자를 사용했고 감쌓여져있는 `a`가 `nil`이 아닐때, `a`의 값에 접근하기 위해 강제로 풀었다(`a!`). 반대의 경우 `b`를 반환했다. Nil 병합 연산자는 조건부 검사와 언 래핑을 간결하고 읽기 쉬운 형식으로 캡슐화보다 우아한 방법을 제공한다. <br>

아래 예시는 기본 색 이름과 옵셔널한 사용자가 정의한 색의 이름 중 선택하는데 Nil 병합 연산자를 사용했다.

```
let defaultColorName = "red"
var userDefinedColorName: String?    // nil이 기본값

var colorNameToUse = userDefinedColorName ?? defaultColorName
// userDefinedColorName 은 nil 이여서 colorNameToUse 은 defaultColorName의 기본값인 red 가 된다.
```

`userDefinedColorName `변수는 옵셔널 `String`으로 정의되었고, 기본값은 `nil`이다. 왜냐하면 `userDefinedColorName `은 옵셔널 타입이고, 값을 고려하는데  Nil 병합 연산자를 사용하기 때문이다. 위의 예시에서, 연산자는 `colorNameToUse `라 불리는 `String`변수의 초기 값을 결정하는데 사용된다. 왜냐하면 `userDefinedColorName `은 `nil`이고 `userDefinedColorName ?? defaultColorName`식에서 `defaultColorName `이나 `red`를 반환하기 때문이다. <br>

만약 `userDefinedColorName `에 `non-nil`값을 할당하고 Nil 병합 연산자를 다시 수행하면, `userDefinedColorName `안에 래핑된 값이 기본 값 대신에 사용된다:

```
userDefinedColorName = "green"
var colorNameToUse = userDefinedColorName ?? defaultColorName
// userDefinedColorName이 nil이 아니여서 colorNameToUse은 green 이다.
```

##Range Operators (범위 연산자)
Swift는 값의 범위를 표현하기 위한 단축어인 몇개의 범위 연산자를 포함하고 있다.

###Closed Range Operator (폐쇄 범위 연산자)
폐쇄 범위 연산자(`a...b`)는 `a`와 `b`값을 포함해서 `a`부터 `b`까지 수행하는 범위이다. `a`의 값은 반드시 `b`보다 크지 않아도 된다.<br>

폐쇄 범위 연산자는 범위 안의 모든 값을 통합하는데 `for-in`반복문 과같이 유용하다.

```
for index in 1...5{
	print("\(index) times 5 is \(index * 5)")
}
// 1 times 5 is 5
// 2 times 5 is 10
// 3 times 5 is 15
// 4 times 5 is 20
// 5 times 5 is 25
```

###Half-Open Range Operator (반 열린 범위 연산자)
반 열린 범위 연산자(`a..<b`)는 `a`부터`b`까지 수행을 하는데 `b`는 포함하지 않는다. 첫번째 값은 포함하지만 마지막 값은 포함하지 않는다 해서 반 열림 이라고 말한다. 폐쇄 범위 연산자 처럼 `a`값은 반드시 `b`보다 클 필요가 없다. 만약 `a`값이 `b`값과 같다면 범위는 비게 된다.<br>

반 열린 범위는 배열과 같이 0부터 시작하는 리스트를 작성할 때 유용하다. 여기서 리스트의 길이까지 계산 할 수 있다.

```
let names = ["Anna", "Alex", "Brian", "Jack"]
let count = names.count
for i in 0..<count {
    print("Person \(i + 1) is called \(names[i])")
}
// Person 1 is called Anna
// Person 2 is called Alex
// Person 3 is called Brian
// Person 4 is called Jack
```

배열은 4개의 아이템을 갖고있다. 그러나 `0..<count`는 3(배열에서의 마지막 아이템의 인덱스)까지 카운트했다. 왜냐하면 반 열린 범위이기 때문이다.

###One-Sided Ranges (단측 범위)
폐쇄 범위 연산자는 인덱스 2부터 배열의 끝까지 배열의 모든 요소를 포함하는 범위같은 한 방향으로 가능한 한 계속되는 범위의 모양이다. 이 경우, 범위 연산자의 한쪽에서 값을 생략할 수 있다. 이러한 종류의 범위를 단측범위라고 한다. 연산자가 한 방향의 값을 갖고 있기 때문이다.

```
for name in names[2...] {
    print(name)
}
// Brian
// Jack

for name in names[...2] {
    print(name)
}
// Anna
// Alex
// Brian
```

반 열린 연산자 또한 마지막 값만 적으면 한 방향 모양을 갖을 수 있다. 양 쪽 값을 포함할 때 처럼 마지막 값은 범위에 포함되지 않는다. 

```
for name in names[..<2] {
    print(name)
}
// Anna
// Alex
```

한 방향 범위는 첨자뿐만 아니라 다른 문맥에서 사용될 수 있다. 첫번째 값을 생략한 단측 범위를 병합할수 없다. 왜냐하면 병합을 시작해야하는 위치가 명확하지 않기 때문이다. 그러나, 마지막 값을 생략한 단측 범위는 사용할 수 있다. 왜냐하면 범위는 지속적으로 증가하므로 루프에 대해 명시적인 종료 조건을 추가해야 한다. 아래와 같이 단측 범위가 특정 값을 포함하는지 확인 할 수 있다.

```
let range = ...5
range.contains(7)   // false
range.contains(4)   // true
range.contains(-1)  // true
```

##Logical Operators (논리 연산자)
논리 연산자는 Boolean 논리 값인 `true`와 `false`를 수정하거나 결합한다. Swift는 C기반 언어들에서 볼 수 있는 3개의 기본적인 논리 연산자를 지원한다.

*  논리적 NOT (`!a`)
*  논리적 AND (`a && b`)
*  논리적 OR (`a || b`)

### Logical NOT Operator (논리적 NOT 연산자)
논리적 NOT 연산자 (`!a`)은 Boolean 값을 `true`는`false`로, `false`는`true`로 변환한다. <br>

논리적 NOT 연산자는 접두사 연산자고, 공백없이 작동하는 값 바로 나타난다. `not a`라고 읽고 다음처럼 나타난다.

```
let allowedEntry = false
if !allowedEntry {
	print("ACCESS DENIED")
}
// ACCESS DENIED 가 출력
```

`if !allowedEntry`는 "만약 입력이 허용되지 않는다면" 이라고 읽을 수 있다. 후속줄은 만약 "입력이 허용되지 않는다면"이 참일 경우만 실행되고 그것은 `allowedEntry`가 `false`라는 것이다. <br>

이 예제에서, Boolean 상수와 변수의 이름을 조심스럽게 선택하면 코드를 읽기 쉽고 간결하게 유지하면서 이중 부정이나 혼란스러운 논리문을 피할 수 있다.

###Logical AND Operator (논리적 AND 연산자)
논리적 AND 연산자(`a && b`)는 전체 표현식이 `true`이기 위해 두 값이 반드시 `true`인 논리적 표현식을 만든다.<br>

만약 어느 한쪽이 `false`이면 전체 표현식 또한 `false`이다. 사실, 만약 첫번째 변수가 `false`이면, 두번째 변수는 평가되지 않는다. 왜냐하면 전체 표현식을 `true`로 같게 하는것은 불가능하기 때문이다.<br>

이 예제는 두 `Bool`변수를 고려하고 오직 두 값이 `true`이면 접근을 허용한다:

```
let enteredDoorCode = true
let passedRetinaScan = false
if enteredDoorCode && passedRetinaScan {
    print("Welcome!")
} else {
    print("ACCESS DENIED")
}
// ACCESS DENIED 출력
```

###Logical OR Operator (논리적 OR 연산자)
논리적 OR 연산자 (`a || b`)는  두개 인접한 파이프 문자로 부터 만들어진 접사이다. 전체 표현식은 `true`이기 위해 두 값중 하나가 `true`일 때 이 논리적 표현식을 만들어 사용할 수 있다.<br>

논리적 AND 연산자와 같이 논리적 OR 연산자는 표현식을 고려하기 위해 단락 평가를 사용한다. 만약 논리적 OR 연산자의 왼쪽 표현식이 `true`이면, 오른쪽은 평가하지 않아도 된다. 왜냐하면 전체 표현식의 결과를 바꿀 수 없기 때문이다. <br>

아래 예시에서, 첫번째 `Bool` 값 (`hasDoorKey`) 가 `false`지만 두번째 값(`knowsOverridePassword`)가 `true`이다. 왜냐하면 하나의 값이 `true`이면 전체 표현식 또한 `true`이고 접근이 허용된다:

```
let hasDoorKey = false
let knowsOverridePassword = true
if hasDoorKey || knowsOverridePassword {
    print("Welcome!")
} else {
    print("ACCESS DENIED")
}
// Welcome! 출력
```

### Combining Logical Operators (결합 논리적 연산자)
다수의 논리 연산자들을 결합해 더 긴 복합식을 만들 수 있다.

```
if enteredDoorCode && passedRetinaScan || hasDoorKey || knowsOverridePassword {
    print("Welcome!")
} else {
    print("ACCESS DENIED")
}
// Welcome! 출력
```

이 예제는 다수의 `&&`와 `||`연산자를 사용해 더 긴 복합식을 만들었다.
그러나, `&&`와 `||`연산자들은 오직 두 값에 대해 동작해 실제로 3개의 작은 표현식이 같이 연결되어있는 것이다. 이 예제는 다음처럼 읽을 수 있다.<br>

만약 우리가 정확한 문 번호로 들어가고 망막스캔을 통과하거나, 유용한 문의 키가 있거나, 만약 우리가 비상 비밀번호를 알고있다면 접근이 허용될 것 이다.<br>

`enteredDoorCode`, `passedRetinaScan`, `hasDoorKey` 값을 기초로 한 첫번째 두 하위식이 `false`다. 하지만 긴급 비밀번호를 알고있어서 전체 복합식은 `true`로 평가된다.

###Explicit Parentheses (명시적 괄호)
복합식의 의미를 쉽게 읽기 위해서 괄호가 꼭 필요하지 않더라도 괄호를 포함하는 것이 유용할 수 있다. 위의 문 접근 예제를 보면, 복합식의 첫번째 부분에 괄호를 추가해 의도를 명확하게 하는 것이 유용하다:

```
if (enteredDoorCode && passedRetinaScan) || hasDoorKey ||knowsOverridePassword {
    print("Welcome!")
} else {
    print("ACCESS DENIED")
}
// Welcome! 출력
```

괄호를 사용하면 처음 두 값은 전체 논리에서 별도의 가능한 상태의 일부로 간주된다. 복합식의 출력은 변하지 않지만, 전체의 의도를 읽는 사람에게 명확하게 해준다. 가독성은 항사 간결성보다 선호된다. 의도를 명확하게 하는 데 도움이되는 곳에 괄호를 사용해라.

##참고 문서
* https://docs.swift.org/swift-book/LanguageGuide/BasicOperators.html
