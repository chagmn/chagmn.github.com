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

비록,

## Sets



## Performing Set Operations



## Dictionaries





