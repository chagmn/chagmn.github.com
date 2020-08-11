---
title: "Collection Types"
date: 2020-8-11
categories: Swift5

---

# Collection Types (컬렉션 타입)

Swift는 값의 모음을 저장하기 위한 Array, Set, Dictionary 라고 알려진 3개의 기본 컬렉션 타입을 제공한다. Array는 정렬 된 값 모음이다. Set는 정렬 되지 않은 고유한 값의 모음이다. Dictionary는 키-값 관계를 같는 정렬되지 않은 모음이다.

![](https://docs.swift.org/swift-book/_images/CollectionTypes_intro_2x.png)

Swift에서 Array,Set,Dictionary는 항상 값 및 키 타입에 대해 항상 명확하다. 이것은 실수로 컬렉션에 잘못된 타입의 값을 넣을 수 없다는 것을 의미한다. 또한, 컬렉션에서 값의 타입을 검색하는 것을 신뢰 할 수 있다.<br>



## Mutability of Collections (컬렉션의 가변성)

Array, Set, Dictionary를 만들고 이를 변수를 할당하면 컬렉션은 변경 될 수 있다. 컬렉션을 만든 후 컬렉션에 아이템을 추가, 제거, 변경하여 컬렉션을 변경 할 수 있다는 것을 의미한다. Array, Set, Dictionary를 상수에 할당하면 컬렉션은 변경 할 수 없고 사이즈와 내용도 변경 할 수 없다.<br>



## Arrays (배열)

배열은 같은 타입의 값을 정렬된 리스트에 저장한다. 같은 값은 배열의 다른 위치에서 여러번 나타날 수 있다. <br>



### Array Type Shorthand Syntax (배열 타입 단축 구문)

Swift 배열 타입은 `Array<Element>`로 작성되며, `Element`에는 저장될 수 있는 배열의 값의 타입이 들어간다. 짧게 배열의 타입을 `[Element]`라고 작성 할 수도 있다. 비록 두 모양이 기능적으로 동일하지만, 단축 형태를 더 선호하고 이 가이드에서 배열 타입을 언급 할 떄 사용된다.



### Creating an Empty Array (빈 배열 생성)

초기화 구문을 사용해 확실한 타입의 빈 배열을 생성 할 수 있다.

```swift
var someInts = [Int]()
print("someInts is of type [Int] with \(someInts.count) items.")
// someInts is of type [Int] with 0 items. 출력
```

`someInts`변수의 타입이 초기화에 의해 `[Int]`라고 추론됬다. <br>



또는, 함수 인수나 준비된 타입의 상수, 변수처럼 문맥에서 이미 타입 정보를 제공하면 `[]`처럼 빈 배열 리터럴로 빈 배열을 생성 할 수 있다.

```swift
someInts.append(3)
// someInts는 Int형의 1개 값을 포함한다.
someInts = []
// someInts는 빈 배열이고, 여전히 타입은 [Int]다.
```



### Creating an Array with a Default Value (기본 값과 함께 배열 생성)

Swift의 `Array`타입은 동일한 기본 값으로 모든 배열 값을 갖는 정확한 크기의 배열을 만들기 위한 초기화를 제공한다. 이 초기화에 적절한 타입의 기본값(`repeating`)과 새 배열에서 해당 값이 반복되는 횟수(`count`)를 전달한다.

```swift
var threeDoubles = Array(repeating: 0.0, count: 3)
// threeDoubles 는 [Double]타입, [0.0, 0.0, 0.0]과 동일
```



### Creating an Array by Adding Two Arrays Together (두 배열을 합쳐서 새 배열 만들기)

호환 가능한 두 개의 존재하는 배열을 더하기 연산자(`+`)를 이용해 더해서 새로운 배열을 만들 수 있다. 새 배열의 타입은 함께 추가하는 두 배열의 타입에서 유추된다.

```swift
var anotherThreeDoubles = Array(repeating: 2.5, count: 3)
// anotherThreeDoubles는 [Double]형, [2.5, 2.5, 2.5]과 동일

var sixDouble = threeDoubles + anotherThreeDoubles
// sixDouble은 [Double]로 유추, [0.0, 0.0, 0.0, 2.5, 2.5, 2.5]과 동일
```



### Creating an Array with an Array Literal

배열을 배열 리터럴을 사용해 초기화 할 수 있다. 이는 하나 이상의 값을 배열 컬렉션으로 쓰는 간단한 방법이다. 배열 리터럴은 값의 리스트로써 작성되고 대괄호로 둘러 싸이고 쉼표로 나눠진다.

`[value 1, value 2, value 3]`

아래 예시는 `String`값을 저장하기 위한 `shoppingList`배열을 생성했다.

```swift
var shoppingList: [String] = ["Eggs", "Milk"]
// shoppingList는 2개 초기 항목으로 초기화 되었다.
```

`shoppingList`변수는 `[String]`으로 적혀있어 "문자열 값의 배열"으로 정의되어 있다. 특정 배열은 지정된 `String` 타입의 값을 갖고, `String`값만 저장하는 것을 허락하기 때문이다. `shoppingList`배열은 두 개의 문자열 리터럴로 적힌 문자열 값(`"Eggs"`와 `"Milk"`)로 초기화 되었다.<br>

이 경우, 배열 리터럴에는 두 개의 문자열 값을 포함하고 그 외는 없다. 이는 `shoppingList`변수의 선언 타입(문자열 값만 포함 하는 배열)과 일치해 두 개의 초기 아이템으로 `shoppingList`를 초기화하는 방법으로 배열 리터럴을 할당 할 수 있다.<br>



Swift의 타입 추론에 고맙게도, 같은 타입의 값을 포함하는 배열 리터럴로 배열을 초기화하면 배열의 타입을 적어주지 않아도 된다. `shoppingList`초기화는 더 짧은 형태로 쓸 수 있다:

```swift
var shoppingList = ["Eggs", "Milk"]
```

배열 리터럴 안의 모든 값은 같은 타입이고, Swift는 `[String]`이 `shoppingList`변수를 사용하기 위한 정확한 타입이라는 것을 추론 할 수 있다.



### Accessing and Modifying an Array (배열 접근 및 수정)

메서드나 프로퍼티, 서브 스크립트 구문을 사용해 배열에 접근하거나 수정 할 수 있다.<br>



배열 안에 아이템의 수를 찾고 싶을 때, 읽기만 가능한 `count`프로퍼티를 사용한다:

```Swift
print("The shopping list contains \(shoppingList.count) items.")
// The shopping list contains 2 items. 출력
```

`count`프로퍼티가 0임을 검사하는 단축어로 `isEmpty`프로퍼티를 사용 할 수 있다.

```swift
if shoppingList.isEmpty {
    print("The shopping list is empty.")
} else {
    print("The shopping list is not empty.")
}
// The shopping list is not empty. 출력
```

배열의 `append(_:)`메서드를 호출의 배열의 맨 끝에 새로운 항목을 추가 할 수 있다.

```swift
shoppingList.append("Flour")
// shoppingList에는 3개의 항목이 저장됨
```

또한, 하나 이상 호환 가능한 항목을 추가하려면 덧셈 할당 연산자(`+=`)를 사용한다:

```swift
shoppingList += ["Baking Powder"]
// shoppingList에는 4개 항목이 저장
shoppingList += ["Chocolate Spread", "Cheese", "Butter"]
// shoppingList에는 7개 항목이 저장

```

서브 스크립트 구문을 사용해 배열에서 값을 검색할 때, 배열 이름 바로 뒤에 나오는 대괄호 안에 검색하기 원하는 값의 인덱스를 넣어준다:

```swift
var firstItem = shoppingList[0]
// firstItem에 Eggs가 할당
```

주어진 인덱스의 존재하는 값을 변경할 때 서브 스크립크 구문을 사용한다:

```swift
shoppingList[0] = "Six eggs"
// "Eggs"에서 "Six eggs"로 변경
```



## Sets



## Performing Set Operations



## Dictionaries





