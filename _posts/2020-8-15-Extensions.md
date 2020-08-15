---
title: "Extensions"
date: 2020-8-15
categories: Swift5

---

# Extensions (익스텐션)

익스텐션을 이용해 클래스, 구조체, 열거형, 프로토콜 타입에 기능을 추가할 수 있다. retroactive modeling으로 알려진 것과 같이 원본 코드를 몰라도 그 타입에 대한 기능을 확장할 수 있다. 익스텐션은 Objective-C의 카테고리와 유사하다. Swift에서 익스텐션을 이용해 다음을 할 수 있다.

* 계산된 인스턴스 프로퍼티와 계산된 타입 프로퍼티의 추가
* 인스턴스 메소드와 타입 메소드의 추가
* 새로운 이니셜라이저 제공
* 서브스크립트 정의
* 중첩 타입의 선언과 사용
* 특정 프로토콜을 따르는 타입 만들기



## Extension Syntax (익스텐션 구문)

익스텐션은 `extension`키워드를 사용해 선언한다.

```swift
extension SomeType{
  // SomeType에 추가 할 새로운 기능이 여기에 표시
}
```

하나의 익스텐션에서 현재 존재하는 타입에 한개 이상의 프로토콜을 따르도록 확장할 수 있다.

```swift
extension SomeType: SomeProtocol, AnotherProtocol{
  // 프로토콜 요구 사항의 구현은 여기에 표시
}
```

이런 방식으로 프로토콜을 구현하는 것은 `Adding Protocol Conformance with an Extension.`에 나와있다.<br>

하나의 익스테션은 `Extending a Generic Type`에 설명돼 있는 것처럼 `Generic`타입으로 확장하는데 사용할 수 있다. 또 `generic`타입에 조건적으로 기능을 추가 할 수 있는 것은 `Extensions with a Generic Where clause`에 나와있다.<br>



## Computed Properties (계산된 프로퍼티)

익스텐션을 이용해 존재하는 타입에 계산된 인스턴스 프로퍼티와 타입 프로퍼티를 추가할 수 있다. 아래 예제는 Swift의 built-in 타입인 `Double`에 5개의 계산된 인스턴스 프로퍼티를 추가하는 예제이다:

```swift
extension Double {
  var km: Double { return self 1_000.0 }
  var m: Double { return self }
  var cm: Double { return self / 100.0 }
  var mm: Double { return self / 1_000.0 }
  var ft: Double { return self / 3.28084 }
}
let oneInch = 25.4.mm
print("One inch is \(oneInch) meters")
// "One inch is 0.0254 meters"
let threeFeet = 3.ft
print("Three feet is \(threeFeet) meters")
// "Three feet is 0.914399970739201 meters"
```

주어진 Double값에 km, m, cm 등 단위를 붙여 미터로 변경하는 계산된 프로퍼티다. 단위변환은 m(미터)를 기준으로 한다. 이 프로퍼티들은 읽기 전용 계산된 프로퍼티이기 때문에 간결함을 위해 get을 붙이지 않았다.

```swift
let aMarathon = 42.km + 195.m
print("A marathon is 42195.0 meters long")
```

단위 변환 값이 Double 타입이기 때문에 각각의 변환 값의 연산도 가능하다.



## Initializers (이니셜라이저)

익스텐션을 이용해 존재하는 타입에 새로운 이니셜라이저를 추가할 수 있따. 이 방법으로 커스텀 타입의 이니셜라이저 파라미터를 넣을 수 있도록 변경하거나 원래 구현에서 포함하지 않는 초기화 정보를 추가할 수 있다.

익스텐션은 클래스에 새로운 편리한 이니셜라이저를 추가할 수는 있지만 지정된 이니셜라이저(`designated initializers`)나 디이니셜라이저(`deinitializers`)를 추가할 수는 없다. 지정된 이니셜라이저는 항상 반드시 오리지널 클래스의 구현에서 작성돼야 한다.

만약 익스텐션을 값 타입에 이니셜라이저를 추가하는데 사용하고, 그 값 타입이 모든 프로퍼티에 대해 기본값을 제공하고 커스텀 이니셜라이저를 정의하지 않았다면, 익스텐션에서 기본 이니셜라이저와 멤버쪽 이니셜라이저를 익스텐션에서 호출할 수 있다. 만약 값 타입의 오리지널 구현의 부분으로 이니셜라이저 코드를 작성했다면 그것은 이 경우에 해당하지 않는다.

만약 다른 모듈에 선언되어 있는 구조체에 이니셜라이저를 추가하는 익스텐션을 사용한다면 새로운 이니셜라이저는 모듈에 정의된 이니셜라이저를 호출하기 전까지 `self`에 접근할 수 없다.

```swift
struct Size {
  var width = 0.0, height = 0.0
}
struct Point {
  var x = 0.0, y = 0.0
}
struct Rect {
  var origin = Point()
  var size = Size()
}
```

위 예제에서는 `Size`와 `Point`구조체를 정의하고 그것을 사용하는 `Rect`구조체를 정의했습니다. `Rect`구조체에서 모든 프로퍼티의 기본 값ㅇ르 제공하기 때문에 `Rect`구조체는 기본 이니셜라이저와 멤버쪽 이니셜라이저를 자동으로 제공 받아 사용할 수 있다.

기본적으로 제공되는 이니셜라이저를 사용해 초기화를 한 예제이다.

```swift
let defaultRect = Rect()
let memverwiseRect = Rect(origin: Point(x: 2.0, y: 2.0),
                         size: Size(width: 5.0, height: 5.0))
```

`Rect`구조체를 추가적인 이니셜라이저를 제공하기 위해 확장 할 수 있다.

```swift
extension Rect {
  init(center: Point, size: Size) {
    let originX = center.x - (size.width / 2)
    let originY = center.y - (size.height / 2)
    self.init(origin: Point(x: originX, y: originY), size: size)
  }
}
```

`Rect`에서 확장한 이니셜라이저를 사용한 코드는 다음과 같이 사용할 수 있다.

```swift
let centerRect = Rect(center: Point(x: 4.0, y: 4.0),
                     size: Size(width: 3.0, height: 3.0))
// centerRect의 origin은 (2.5, 2.5), size는 (3.0, 3.0)
```





## Methods (메소드)

익스텐션을 이용해 존재하는 타입에 인스턴스 메소드나 타입 메소드를 추가할 수 있다. 아래 예제는 `Int`타입에 `repetitions`라는 인스턴스 메소드를 추가한 예제이다.

```swift
extension Int {
  func repetitions(task: () -> Void) {
		for _ in 0..<self {
      task()
    }
  }
}
```

`repetitions(task:)`메소드는 `()->Void`타입의 하나의 인자를 받고 파라미터와 반환 값이 없는 함수다.

```swift
3.repetitions {
	print("hello!")
}
// hello!
// hello!
// hello!
```

함수를 실행하면 함수 안의 `task`를 숫자 만큼 반복 실행한다.



## Mutating Instance Methods (변경 가능한 인스턴스 메소드)

익스텐션에서 추가된 인스턴스 메소드는 인스턴스 자신(self)을 변경할 수 있다. 구조체와 열거형 메소드 중 자기 자신(self)를 변경하는 인스턴스 메소드는 원본 구현의 `mutating `메소드와 같이 반드시 `mutating`으로 선언되어야 한다.

아래 코드는 새 `mutating`메소드를 추가하고 호출하는 예제이다.

```swift
extension Int {
  mutating func square() {
		self = self * self
  }
}
var someInt = 3
someInt.square()
// someInt는 9
```



## Subscripts (서브스크립트)

익스텐션을 이용해 존재하는 타입에 새로운 서브스크립트를 추가할 수 있다. 다음 예제는 Swift의 `build-in`타입에 `Integer`서브스크립트를 추가한 예제다. 서브스크립트 `[n]`은 숫자의 오른쪽에서부터 n번째 위치하는 정수를 반환한다.

* 123456789[0] returns 9
* 123456789[1] returns 8

```swift
extension Int {
	subscript(digitIndex: Int) -> Int {
		var decimalBase = 1
		for _ in 0..<digitIndex {
      decimalBase *= 10
    }
    return (self / decimalBase) % 10
    // 10 * n번째 수로 현재 수를 나눈 것의 나머지
    // 1이면 746381295 % 10 -> 5가 나머지
    // 2이면 746381295 % 10 -> 9가 나머지
	}
}
746381295[0]
// 5 반환
746381295[1]
// 9 반환
746381295[2]
// 2 반환
746381295[8]
// 7 반환
```

만약 `Int`값에서 요청한 값이 처리할 수 있는 자릿 수를 넘어가면 서브스크립트 구현에서 0을 반환한다.

```swift
746381295[9]
// 9로 처리할 수 있는 자릿 수를 넘어가면 0을 반환
```



## Nested Types (중첩 타입)

익스텐션을 이용해 존재하는 클래스, 구조체, 열거형에 중첩 타입을 추가할 수 있다.

```swift
extension Int {
  enum Kind {
    case negative, zero, positive
  }
  var kind: Kind {
		switch self {
      case 0:
      	return .zero
      case let x where x > 0:
      	return .positive
      default:
      	return .negative
    }
  }
}
```

위 예제는 `Int`에 중첩형 enum을 추가한 예제다. `Kind`라고 불리는 열거형은 `Int`를 음수, 0, 양수로 표현한다.

아래 예제는 새롭게 계산된 프로퍼티 `kind`를 이용해 특정 수가 음수, 0, 양수 중 어떤 것인지를 나타내는 예제다.

```swift
func printIntegerKinds(_ numbers: [Int]) {
  for number in numbers {
    switch number.kind {
      case .negative:
      	print("- ", terminator: "")
      case .zero:
      	print("0 ", terminator: "")
      case .positive:
      	print("+ ", terminator: "")
    }
  }
  print("")
}
printIntegerKinds([3, 19, -27, 0, -6, 0, 7])
// "+ + - 0 - 0 + " 출력
```

`printIntegerKinds(_:)`함수는 `Int`배열을 입력받아 각 `Int`가 음수, 0, 양수 어디에 속하는지 계산해서 그에 맞는 기호를 반환하는 함수다.

