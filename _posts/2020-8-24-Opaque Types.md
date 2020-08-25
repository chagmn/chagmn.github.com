---
title : "Opaque Types"
date : 2020-08-24
categories : Swift5
---



# Opaque Types (불분명한 타입)

불분명한 반환 타입의 함수나 메소드는 반환하는 값의 타입 정보를 감춘다. 대신에 함수의 반환 타입은 분명한 타입을 제공하는데 반환 값은 그것이 제공하는 프로토콜 약관에 묘사되어있다. 타입 정보를 감추는 것은 반환 값의 기본 타입은 비공개로 유지 될 수 있기 때문에 모듈을 호출하는 코드와 모듈 사이의 경계에서 유용하다. 타입이 프로토콜 타입인 값을 반환하는 것과 다르게 불분명한 타입은 타입ID를 유지한다. 컴파일러는 타입 정보에 액세스 할 수 있지만 모듈의 클라이언트는 그렇지 않다.



## The Problem That Opaque Types Solve (불분명한 타입이 해결해야 할 문제)

예를 들어, ASCII 미술 모양을 그리는 모듈을 작성한다고 제안한다. ASCII 미술 모양의 기본 특징은 모양을 대표하는 문자열을 반환하는 `draw()`함수다. 이 함수는 `Shape`프로토콜 요구사항에서 볼 수 있다.

```swift
protocol Shape {
  func draw() -> String
}

struct Triangle: Shape {
  var size: Int
  func draw() -> String {
    var result = [String]()
    var length in 1...size {
      result.append(String(repeating: "*", count: length))
    }
    return result.joined(separator: "\n")
  }
}
let smallTriangle = Triangle(size: 3)
print(smallTriangle.draw())
// *
// **
// ***
```

아래 코드에서 보여주는 것 처럼 제네릭을 사용해 수직으로 모양 뒤집기같은 연산자를 구현할 수 있다. 그러나 이 접근 방식에는 중요한 제한이 있다: 뒤집힌 결과를 생성하는데 사용된 정확한 제네틱을 타입을 노출시킨다.

```swift
struct FlippedShape<T: Shape>: Shape {
	var shape: T
	func draw() -> String {
		let lines = shape.draw().split(separator: "\n")
		return lines.reversed().joined(separator: "\n")
	}
}
let flippedTriangle = FlippedShape(shape: smallTriangle)
print(flippedTriangle.draw())
// ***
// **
// *
```

아래 코드처럼 두 모양을 수직적으로 합치는 `JoinedShape<T: Shape, U: Shape>` 구조체를 정의하는 접근방식은 뒤집힌 삼각형을 다른 삼각형과 결합하여 `JoinedShape<FlippedShape<Triangle>, Triangle>`과 같은 타입을 생성한다.

```swift
struct JoinedShape<T: Shape, U: Shape>: Shape {
  var top: T
  var bottom: U
  func draw() -> String {
    return top.draw() + "\n" + bottom.draw()
  }
}
let joinedTriangles = JoinedShape(top: smallTriangle, bottom: flippedTriangle)
print(joinedTriangles.draw())
// *
// **
// ***
// **
// *
```

모양 만드는 것에 대해 자세한 정보 노출하면 전체 반환 타입을 명시해야하므로 ASCII 아트 모듈의 공개 인터페이스에 포함되지 않는 타입이 유출 될 수 있다. 모듈안에 코드는 다양한 방법으로 같은 모양을 만들 수 있고, 모양에 사용되는 모듈 밖의 다른 코드는 변환 목록에 대한 세부 정보를 고려할 필요가 없다. `JoinedShape`와 `FlippedShape` 와 같은 감싸여진 타입은 모듈 사용자들에게 중요하지 않고 표시되지 않는다. 모듈의 공용 인터페이스는 모양을 합치고 뒤집기와 같은 작업으로 구성되고 이 작업들은 또 다른 `Shape`값을 반환한다.



## Returning an Opaque Type (불분명한 타입 반환 )

불분명한 타입은 제네릭 타입의 반대라고 생각할 수 있다. 제네릭 타입을 사용하면 함수를 호출하는 코드에서 해당 함수의 매개변수 타입을 선택하고 함수 구현에서 추상화 된 방식으로 값을 반환할 수 있다. 예를 들어, 뒤따르는 코드에서 함수는 호출자에 의존한 타입을 반환한다:

```swift
func max<T>(_ x: T, _ y: T) -> T where T: Comparable { ... }
```

`max(_:_:)`는 `x`, `y`의 값을 선택하고 이 값들의 타입을 `T`라는 분명한 타입으로 결정한다. 호출하는 코드는 `Comparable`프로토콜을 준수하는 어떠한 타입으로 사용할 수 있다. 함수 안에 코드는 일반적인 방법으로 기술되어서 호출자가 제공하는 어떤 타입이든지 조작이 가능하다. `max(_:_:)`의 구현은 모든 `Comparable`타입이 공유하는 기능만을 사용한다.<br>

불분명한 반환 타입을 가진 함수의 경우 이러한 역할이 반전된다. 불분명한 타입을 사용하면 함수 구현에서 함수를 호출하는 코드에서 추상화된 방식으로 반환되는 값의 타입을 선택할 수 있다. 예를 들어, 다음 예제에서의 함수가 해당 모양의 기본 타입을 노출하지 않고 사다리꼴을 반환한다.

```swift
struct Square: Shape {
  var size: Int
  func draw() -> String {
    let line = String(repeating: "*", count: size)
    let result = Array<String>(repeating: line, count: size)
    return result.joined(separator: "\n")
  }
}

func makeTrapezoid() -> some Shape {
  let top = Triangle(size: 2)
  let middle = Square(size: 2)
  let bottom = FlippedShape(shape: top)
  let trapezoid = JoinedShape(
  	top: top,
  	bottom: JoinedShape(top: middle, bottom: bottom)
  )
  return trapezoid
}
let trapezoid = makeTrapezoid()
print(trapezoid.draw())
// *
// **
// **
// **
// **
// *
```

이 예제에서 `makeTrapezoid()`함수는 `some Shape` 타입을 반환하고, 결과적으로 함수는 특정 구체적인 타입을 지정하지 않고 `Shape`프로토콜을 준수하는 특정 타입의 값을 반환한다. `makeTrapezoid()` 이러한 방법으로 작성하면 공용 인터페이스의 일부에서 모양이 만들어지는 특정 타입을 만들지 않고도 공용 인터페이스의 기본 측면(반환되는 값은 모양)을 표현할 수 있다. 이 구현에서 두개의 삼각형과 하나의 사각형이 사용되지만 함수는 반환 타입을 바꾸지 않고 다른 다양한 방법으로 사다리꼴을 그리는 것을 재작성할 수 있다.<br>

이 예제는 불분명한 반환 타입이 제네릭 타입의 반대와 비슷한 방식을 강조한다. `makeTrapezoid()` 안에 코드는 호출하는 코드가 제네릭 함수에 대해 수행하는 것처럼 해당 타입이 `Shape`프로토콜을 따르는 한 필요하는 어떤 타입이든지 반환할 수 있다. 함수를 호출하는 코드는 제네릭 함수 구현처럼 일반적인 방법으로 작성되야 해서 `makeTrapezoid()`에서 반환되는 어떤 `Shape`값과 동작할 수 있다. <br>

불분명한 반환 타입을 제네릭과 결합할 수 있다. 아래 코드에서 함수는 `Shape`프로토콜을 따르는 어떤 타입의 값을 반환한다.

```swift
func flip<T: Shape>(_ shape: T) -> some Shape {
  return FlippedShape(shape: shape)
}
func join<T: Shape, U: Shape>(_ top: T, _ bottom: U) -> some Shape {
  JoinedShape(top: top, bottom: bottom)
}

let opaqueJoinedTriangles = join (smallTriangle, flip(smallTriangle))
print(opaqueJoinedTriangles.draw())
// *
// **
// ***
// **
// *
```

이 예제에서 `opaqueJoinedTriangles`의 값은 앞에 나온 `joinedTriangles`와 비슷하다. 그러나 예제에서 값과 달리 `flip(_:)`과 `join(_:_:)`은 제네릭 모양 작업이 반환하는 기본 타입을 불분명한 반환 타입으로 래핑해 해당 타입이 표시되지 않도록 한다. 두 함수는 제네릭이다. 왜냐하면 그들이 의존하는 타입이 제네릭이고 함수에 대한 타입 매개변수가 `FlippedShape`와 `JoinedShape`에 필요한 타입 정보를 전달하기 때문이다.<br>

만약 다양한 위치로부터 반환되는 불분명한 반환 타입이 쓰인 함수라면, 가능한 반환 값 모두 반드시 같은 타입이여야 한다. 제네릭 함수에서 반환 타입은 함수의 제네릭 타입 매개변수로 사용할 수 있지만, 단 하나의 타입이여야 한다. 예를 들어, 다음은 정사각형에 대한 특수 사례를 포함하는 잘못된 버전의 모양 뒤집는 함수다.

```swift
func invalidFlip<T: Shape>(_ shape: T) -> some Shape {
  if shape is Square {
    return shape // 에러 발생 : 반환 타입이 매칭되지 않음
  }
  return FlippedShape(shape: shape) // 에러 발생 : 반환 타입이 매칭되지 않음
}
```

만약 이 함수를 `Square`와 함께 호출한다면, `Square`를 반환하고 그렇지 않으면 `FlippedShape`를 반환할 것 이다. 이는 한 타입의 값만 반환해야 한다는 요구사항을 위반하고 `invalidFlip(_:)`코드를 유효하지 않게 만든다. `invalidFlip(_:)`을 수정하는 한 가지 방법은 사각형의 특수 사례를 `FlippedShape`구현으로 이동하는 것 이다. 이렇게하면 이 함수가 항상 `FlippedShape`값을 반환 할 수 있다.

```swift
struct FlippedShape<T: Shape>: Shape {
  var shape: T
  func draw() -> String {
    if shape is Square {
      return shape.draw()
    }
    let lines = shape.draw().split(separator: "\n")
    return lines.reversed().joined(separator: "\n")
  }
}
```

항상 하나의 타입을 반환해야한다는 요구사항은 불분명한 반환 타입에서 제네릭 사용을 막는 것이 아니다. 다음은 반환하는 값의 기본 타입에 타입 매개변수를 통합하는 함수의 예제이다:

```swift
func `repeat`<T: Shape> (shape: T, count: Int) -> some Collection {
  return Array<T>(repeating: shape, count: count)
}
```

이러한 경우, 반환 값의 기본 타입은 `T`에 따라 달라진다. 모양이 무엇이든지 `repeat(shape:count:)` 모양의 배열을 만들고 반환한다. 그렇지만, 반환 값은 항상 `[T]`와 같은 기본 타입을 가져서 불분명한 반환 타입이 있는 함수의 요구사항은 반드시 단 하나의 타입의 값을 반환해야 한다는 것 이다.



## Differences Between Opaque Types and Protocol Types (불분명한 타입과 프로토콜 타입의 차이점)

불분명한 타입을 반환하는 것은 프로토콜 타입을 함수의 반환 타입으로 사용하는 것과 매우 유사한 것 처럼 보이지만 이 2개의 반환 타입은 타입ID를 유지하는지 여부가 다르다. 불분명한 타입은 하나의 특정 타입을 참조하지만 함수 호출자는 어떤 타입인지 볼 수 없다. 프로토콜 타입은 프로토콜을 따르는 어떤 타입이든지 참조할 수 있다. 일반적으로 말하자면, 프로토콜 타입은 그들이 저장하는 값의 기본 타입에 대해 더 유연하고 불분명한 타입은 기본 타입에 대해 더 강한 보증을 만드는 것을 권장한다.<br>

예를 들면, 불분명한 반환 타입 대신에 타입을 반환하는 프로토콜 타입을 사용하는 `flip(_:)`의 버전이 있다:

```swift
func protoFlip<T: Shape>(_ shape: T) -> Shape {
  return FlippedShape(shape: shape)
}
```

`protoFlip(_:)`의 버전은 `flip(_:)`과 같은 몸체를 가졌고 항상 같은 타입의 값을 반환한다. `flip(_:)`과 다르게 `protoFlip(_:)`의 반환 값은 항상 같은 타입일 필요는 없고 그것은 `Shape`프로토콜을 따르기만 하면 된다. 달리 말하면 `protoFlip(_:)`은 `flip(_:)`보다 호출자와 더 느슨한 API 연결을 만든다. 여러 타입의 값을 반환하는 유연성을 보유한다:

```swift
func protoFlip<T: Shape>(_ shape: T) -> Shape {
  if shape is Square {
    return shape
  }
  return FlippedShape(shape: shape)
}
```

코드의 수정된 버전은 어떤 모양이 전달되느냐에 따라 `Square` 인스턴스나 `FlippedShape`인스턴스를 반환한다. 이 함수에 의해 반환되는 두 뒤집힌 모양은 아마 완전히 다른 타입이다. 이 함수의 다른 유효한 버전은 같은 모양의 다수의 인스턴스를 뒤집을 때 다른 타입의 값을 반환할 수 있다. `protoFlip(_:)`의 덜 구체적인 반환 타입 정보는 타입 정보에 의존하는 많은 작업을 반환된 값에서 사용할 수 없음을 의미한다. 예를 들어, 함수에 의해 반환되는 결과를 `==`연산자를 사용해 비교하는 것은 불가능하다.

```swift
let protoFlippedTriangle = protoFlip(smallTriangle)
let sameThing = protoFlip(smallTriangle)
protoFlippedTriangle == sameThing   // 에러발생
```

예시에서 마지막 줄의 에러는 몇몇의 이유로 발생한다. 당면한 문제는 `Shape`는 프로토콜 요구사항의 부분으로 `==`연산자를 포함하고 있지 않다. 만약 이것을 추가하려 한다면, 다음 문제는 `==`연산자는 왼쪽 및 오른쪽 인수의 타입을 알아야한다는 것이다. 이러한 종류의 연산자는 보통 프로토콜을 선택하는 분명한 타입과 일치하는 `Self`타입의 인수는 사용하지만 프로토콜에 `Self`요구사항을 추가하면 프로토콜을 타입으로 사용할 때 발생하는 타입 삭제가 허용되지 않는다. <br>

함수의 반환 타입으로써 프로토콜 타입을 사용하면 프로토콜을 따르는 어떠한 타입을 반환하는데 좀 더 유연함을 줄 것이다. 그러나 유연성의 대가는 반환된 값에 대해 일부 작업을 수행 할 수 없다는 것이다. 예제는 어떻게 `==`연산자가 사용할 수 없고 이는 프로토콜 타입을 사용함으로써 보존되지 않는 특정 타입 정보에 따라 다르다. <br>

이러한 접근의 또 다른 문제는 모양 변환이 중첩되지 않는다는 것이다. 삼각형을 뒤집은 결과는 `Shape`타입의 값이고 `protoFlip(_:)`함수는 `Shape` 프로토콜을 따르는 몇몇 타입의 인수를 가져온다. 그러나 프로토콜 타입의 값은 프로토콜을 따르지 않는다. `protoFlip(_:)`의 반환된 값은 `Shape`를 따르지 않는다. 여러 변형을 적용하는 `protoFlip(protoFlip(smallTriangle))` 같은 코드가 의미하는 것은 변환된 모양이 `protoFlip(_:)`에 대해 유효한 인수가 아니기 때문에 유효하지 않다는 것 이다. <br>

반면에, 불분명한 타입은 기본 타입의 ID를 유지한다. Swift는 프로토콜 타입이 반환 값으로 사용할 수 없는 곳에 불분명한 반환 값을 사용할 수 있도록 연관된 타입을 추론할 수 있다. 예를 들어, 제네릭에서 `Container`프로토콜의 버전이 있다

```swift
protocol Container {
  associatedtype Item
  var count: Int { get }
  subscript(i: Int) -> Item { get }
}
extension Array: Container { }
```

`Container`를 함수의 반환 타입으로 사용할 수 없다. 그 이유는 프로토콜이 연관된 타입을 갖고 있기 때문이다. 또한 제네릭 반환 타입의 제약조건으로 사용할 수 있다. 함수 본문 외부에 제네릭 유형이 필요한 것을 추론하기에 충분한 정보가 없기 때문이다.

```swift
// 에러 : 연관된 타입이 있는 프로토콜은 반환 타입으로 사용할 수 없음
func makeProtocolContainer<T>(item: T) -> Container {
  return [item]
}

// 에러 : C 추론에 충분한 정보가 없음
func makeProtocolContainer<T, C: Container> (item: T) -> C {
  return [item]
}
```

`some Container`를 반환 타입으로 불분명한 타입을 사용하면 원하는 API 접근을 표현한다. 함수는 container를 반환하지만 container의 타입 지정은 거부한다.

```swift
func makeOpaqueContainer<T>(item: T) -> some Container {
  return [item]
}
let opaqueContainer = makeOpaqueContainer(item: 12)
let twelve = opaqueContainer[0]
print(type(of: twelve))
// "Int" 출력
```

`twelve`타입은 `Int`로 추론되며 이는 타입 추론이 불분명한 타입과 같이 동작한다는 사실을 보여준다. `makeOpaqueContainer(item:)`의 구현에서 불분명한 container의 기본 타입은 `[T]`다.

이러한 경우, `T`는 `Int`여서 반환 값은 정수 배열이고 `Item`의 연관 타입은 `Int`로 추론된다. `Container` 서브스크립트는 `Item`을 반환하며 `twelve`의 타입이 또한 `Int`라고 추론되는 것을 의미한다.



> 이 문서는 https://docs.swift.org/swift-book/LanguageGuide/OpaqueTypes.html 를 참조하여 제작되었습니다. 오역이 있을 수도 있으니 발견하시면 꼭 제보해주시면 감사하겠습니다.

