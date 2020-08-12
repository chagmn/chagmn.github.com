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

서브 스크립트 구문을 사용할 때, 선택한 인덱스는 유효해야 한다. 예를 들어, `shoppingList[shoppingList.count] = "Salt"`라 적을 때, 배열 끝에 항목을 추가하려고 하면 런타임 에러가 발생한다.<br>

또한 서브 스크립트 구문을 사용해 한번에 값의 범위를 바꿀 수 있다. 심지어 대체 값의 집합의 길이가 대체하려는 범위와 다른경우에도 가능하다. 아래 예제에서 `"Chocolate Spread", "Cheese", "Butter"`를 `"Banana", "Apples"`로 바꾼다:

```swift
shoppingList[4...6] = ["Bananas", "Apples"]
// shoppingList는 6개 항목을 갖고 있다
```

배열에서 특정 인덱스에 항목을 삽입하고 싶을 때, 배열의 `insert(_:at:)`메소드를 호출한다.

```swift
shoppingList.insert("Maple Syrup", at: 0)
// shoppingList에 7개 항목 존재
// "Maple Syrup"이 첫번째 항목
```

0번 인덱스를 가리키는 shopping list의 맨 첫번째에 `"Maple Syrup"`이라는 값의 새로운 항목을 삽입하기 위해서 `insert(_:at:)`메서드를 호출했다.<br>

유사하게, 배열로부터 `remove(at:)`메서드를 사용해 아이템을 삭제 할 수 있다. 이 메서드는 특정 인덱스에 위치한 항목을 제거하고 제거한 항목을 반환한다.

```swift
let mapleSyrup = shoppingList.remove(at: 0)
// 인덱스 0에 해당하는 항목 삭제
// shoppingList에는 6개 항목 존재, Maple Syrup은 없음
// mapleSyrup상수는 "Maple Syrup"을 갖음
```

항목이 제거되면 배열에는 어떠한 공간도 존재하지 않아서 인덱스 0의 값은 다시 한 번 `"Six eggs"`와 같다.

```swift
firstItem = shoppingList[0]
// firstItem는 "Six eggs"
```

배열의 마지막 항목을 제거하려면 `remove(at:)`메서드보다 `removeLast()`메서드를 사용해 배열의 `count`프로퍼티를 쿼리 할 필요 없도록 한다. `remove(at:)`메서드처럼 `removeLast()`도 제거된 항목을 반환한다.

```swift
let apples = shoppingList.removeLast()
// 배열의 마지막 항목은 제거
// shoppingList는 5개 항목, Apples는 없음
// 상수 apples는 제거된 "Apples"를 갖음
```



### Iterating Over an Array (전체 배열 반복)

`for-in`루프를 사용해 배열의 전체 값을 반복 할 수 있다.

```swift
for item in shoppingList {
    print(item)
}
// Six eggs
// Milk
// Flour
// Baking Powder
// Bananas
```

각 항목의 정수 인덱스와 해당 값이 필요한 경우 `enumerated()`메서드를 사용해 전체 배열을 반복한다. 배열에서 각 항목에 대해 `enumerated()`메서드는 정수와 항목으로 구성된 튜플을 반환한다. 정수는 0에서 부터 시작되며 각 아이템마다 1씩 증가한다. 전체 배열을 세게되면, 이 정수들은 각 항목의 인덱스와 일치한다. 튜플은 반복의 일부로써 일시적인 상수나 변수로 분해 할 수 있다.

```swift
for (index, value) in shoppingList.enumerated() {
    print("Item \(index + 1): \(value)")
}
// Item 1: Six eggs
// Item 2: Milk
// Item 3: Flour
// Item 4: Baking Powder
// Item 5: Bananas
```



## Sets (집합)

Set은 순서가 없는 같은 타입의 고유한 값을 저장하는 컬렉션이다. 항목 순서가 중요하지 않거나 항목이 한 번만 표시되도록 해야하는 경우 배열 대신 집합을 사용한다.<br>



### Hash Value for Set Types (집합 유형을 위한 해시 값)

타입은 집합에 저장되기 위해 해시가능(정수 해시 값을 제공하는) 타입이어야 한다. 즉, 타입은 자체에 대한 해시 값을 계산하는 방법을 제공해야 한다. 해시 값은 동일하게 비교되는 모든 개체에 대해 동일한 `Int`값으로, `a==b`이면, `a.hashValue == b.hashValue`이다.<br>
`String`, `Int`, `Double`, `Bool`같은 Swift의 모든 타입은 기본으로부터 해시가능이고 집합 값 타입이나 딕셔너리 키 타입으로 사용할 수 있다. 연결된 값이 없는 열거형 케이스 값도 기본적으로 해시가능 할 수 있다.<br>

### Set Type Syntax (집합 타입 구문)

Swift 집합의 타입은 `Set<Element>`로 작성된다. 여기서 `Element`는 집합이 저장할 수 있는 타입이다. 배열과 달리 집합에는 동등한 단축 형식이 없다.



### Creating and Initializing an Empty Set (빈 집합 생성 및 초기화)

초기화 구문을 사용해 확실한 타입의 집합을 만들 수 있다.

```swift
var letters = Set<Character>()
print("letters is of type Set<Character> with \(letters.count) items.")
// letters is of type Set<Character> with 0 items. 출력
```

또한, 문맥이 이미 타입 정보를 제공하는 함수 인자나 이미 정의된 상수나 변수에서 빈 배열 리터럴과 함께 빈 집합을 만들 수 있다.

```swift
letters.insert("a")
// letters는 문자타입의 1개 값을 갖음
letters = []
// letters는 빈 집합이지만, Set<Character> 임
```



### Creating a Set with an Array Literal (배열 리터럴을 사용해 집합 생성

하나 이상의 값을 집합 컬렉션으로 쓰는 간단한 방법으로 배열 리터럴을 사용해 집합을 최기화 할 수 있다.<br>
아래 예제는 `String`값을 저장하는 `favoriteGenres`라 불리는 집합을 생성했다.

```swift
var favoriteGenres: Set<String> = ["Rock", "Classical", "Hip hop"]
// favoriteGenres는 3개의 초기값으로 초기화
```

`favoriteGenres`변수는 `Set<String>`으로 쓰이고 "`String`값의 집합"이라고 정의되어있다. 이 특정 집합은 `String`타입의 값을 갖고 `String`값만 저장을 허락한다. `favoriteGenres`집합은 배열 리터럴로 적힌 3개의 문자열 변수(`"Rock", "Classical", "Hip hop"`)로 초기화했다.<br>

집합 타입은 배열 리터럴만으로부터 추론될 수 없다. 그래서 집합 타입은 반드시 명확한 선언이 있어야 한다. 그러나 Swift 타입 추론때문에 만약  오직 하나의 타입의 값을 포함하는 배열 리터럴을 가지고 초기화를 했다면 집합 요소의 타입을 적을 필요 없다. `favoriteGenres`를 짧은 형태로 초기화문을 작성했다:

```swift
var favoriteGenres: Set = ["Rock", "Classical", "Hip hop"]
```

배열 리터럴의 모든 값은 같은 타입이기 때문에, Swift는 `Set<String`이 `favoriteGenres`변수에 사용할 정확한 타입이라고 추론 할 수 있다.



### Accessing and Modifying a Set (집합에 접근하고 수정)

메소드나 프로퍼티들을 통해 집합에 접근하거나 수정 할 수 있다.

집합의 아이템의 수를 찾아낼 때, 읽기만 가능한 `count`프로퍼티를 사용한다.

```swift
print("I have \(favoriteGenres.count) favorite music genres.")
// I have 3 favorite music genres. 출력
```

`isEmpty`프로퍼티를 사용하면 `count`프로퍼티가 0과 동일할 때를 짧게 나타낼 수 있다.

```swift
if favoriteGenres.isEmpty {
    print("As far as music goes, I'm not picky.")
} else {
    print("I have particular music preferences.")
}
// I have particular music preferences. 출력
```

집합의 `insert(_:)`메서드를 사용해 집합에 새로운 항목을 추가 할 수 있다.

```swift
favoriteGenres.insert("Jazz")
// favoriteGenres는 4개 항목이 존재
```

집합 안의 항목을 제거하고 제거한 항목을 반환하거나 집합이 갖고 있지 않으면 `nil`을 반환하는 `remove(_:)`메소드를 호출해 집합으로 부터 항목 제거 할 수 있다. 또한, 모든 항목을 제거할 때는 `removeAll()`메서드를 호출한다.

```swift
if let removedGenre = favoriteGenres.remove("Rock") {
    print("\(removedGenre)? I'm over it.")
} else {
    print("I never much cared for that.")
}
// Rock? I'm over it. 출력
```

집합이 특정 항목을 포함하고 있는지 확인할 때, `contains(_:)`메서드를 사용한다.

```swift
if favoriteGenres.contains("Funk") {
    print("I get up on the good foot.")
} else {
    print("It's too funky in here.")
}
// It's too funky in here. 출력
```



### Iterating Over a Set (집합 전체 반복)

`for-in`루프를 사용해 집합의 전체 값을 반복 할 수 있다.

```swift
for genre in favoriteGenres {
    print("\(genre)")
}
// Classical
// Jazz
// Hip hop
```

Swift의 집합 타입은 순서를 정의하지 않는다. 집합의 특정 순서의 값을 반복하려면 집합의 요소를 `<`연산자를 사용해 정렬된 배열로 반환하는 `sorted()`메서드를 사용한다.

```swift
for genre in favoriteGenres.sorted() {
    print("\(genre)")
}
// Classical
// Hip hop
// Jazz
```



## Performing Set Operations (집합 연산 수행)

두 집합을 결합하거나, 두 집합이 공통으로 갖는 값을 결정하거나, 두 집합이 동일한 값을 모두 포함하는지 아니면 일부를 포함하는지, 전혀 포함하지 않는지 결정하는 등의 기본적인 집합 작업을 효율적으로 수행 할 수 있다.



### Fundamental Set Operations (기본적인 집합 연산)

아래 사진은 색칠된 구역으로 표시되는 다양한 집합 연산의 결과와 함께 `a`, `b` 두 집합을 묘사한다.<br>

![](https://docs.swift.org/swift-book/_images/setVennDiagram_2x.png)

* `intersection(_:)`메서드를 사용해 두 집합의 공통 된 값으로 새 집합을 생성한다.
* `symmetricDifference(_:)`메서드를 사용해 두 집합 중 하나의 값을 사용해 새로운 집합을 생성한다.
* `union(_:)`메서드를 사용해 두 집합의 모든 값으로 새로운 집합을 생성한다.
* `subtracting(_:)`메서드를 사용해 특정 집합의 값을 갖지 않는 집합을 생성한다.

```swift
let oddDigits: Set = [1, 3, 5, 7, 9]
let evenDigits: Set = [0, 2, 4, 6, 8]
let singleDigitPrimeNumbers: Set = [2, 3, 5, 7]

oddDigits.union(evenDigits).sorted()
// [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
oddDigits.intersection(evenDigits).sorted()
// []
oddDigits.subtracting(singleDigitPrimeNumbers).sorted()
// [1, 9]
oddDigits.symmetricDifference(singleDigitPrimeNumbers).sorted()
// [1, 2, 9]
```



### Set Membership and Equality (집합 멤버와 동등)

아래 그림은 집합간에 공유되는 요소를 나타내는 중첩 영역이 있는 3개 집합 `a`, `b`, `c`를 보여준다. 집합`a`는 집합`b`의 모든 요소를 포함하고 있기 때문에 집합`a`는 집합`b`의 `superset`이다. 반대로, 집합 `b`는 집합 `a`의 `subset`이다. 집합 `b`와 집합 `c`는 공통 요소를 공유하지 않기 때문에 `disjoint` 이다.

![](https://docs.swift.org/swift-book/_images/setEulerDiagram_2x.png)

* 동등 연산자(`==`)를 사용해 두 집합이 모두 같은 값을 갖는지 확인한다.
* `isSubset(of:)`메서드를 사용해 집합의 모든 값이 특정 집합에 포함되는지 확인한다.
* `isSuperset(of:)`메서드를 사용해 특정 집합의 모든 값이 집합에 포함되는지 확인한다.
* `isStrictSubset(of:_)`나 `isStrictSuperset(of:)`메서드를 사용해 집합이 subset인지 superset인지 확인하지만, 특정 집합과 동일한지는 확인 못한다.
* `isDisjoint(with:)`메서드를 사용해 두 집합이 공통으로 갖는 값이 없는지 확인한다.

```swift
let houseAnimals: Set = ["🐶", "🐱"]
let farmAnimals: Set = ["🐮", "🐔", "🐑", "🐶", "🐱"]
let cityAnimals: Set = ["🐦", "🐭"]

houseAnimals.isSubset(of: farmAnimals)
// true
farmAnimals.isSuperset(of: houseAnimals)
// true
farmAnimals.isDisjoint(with: cityAnimals)
// true
```



## Dictionaries

Dictionary는 순서 없이 컬렉션에서 같은 타입의 키와 같은 타입의 사이의 관계를 저장한다. 각 값은 딕셔너리에서 값을 식별하는 행동을 하는 유일한 키와 관련되어 있다. 배열에서의 항목과 다르게, 딕셔너리의 항목은 특별한 순서를 갖지 않는다. 식별자르 기반으로한 값을 찾을 때, 딕셔너리가 필요하며 현실의 사전에서 특정 단어의 정의를 찾을 때와 동일한 방법이다.<br>



### Dictionary Type Shorthand Syntax (딕셔너리 타입 단축 구문)

Swift 딕셔너리 타입은 `Dictionary<Key, Value>`로 작성되며, `Key`에는 딕셔너리 키처럼 사용되는 값의 타입이 들어가고, `Value`에는 키들을 위해 저장되는 딕셔너리의 값의 타입이 들어간다.<br>

딕셔너리 타입을 짧게 `[Key: Value]`로 작성할 수 있다. 비록 두 형태가 기능적으로 동일하지만, 짧은 형태가 선호되고, 이 가이드에서도 딕셔너리 타입을 표현하는데 사용된다.<br>



### Creating an Empty Dictionary (빈 딕셔너리 생성)

배열처럼, 초기화 구문을 사용해 확실한 타입의 빈 딕셔너리를 생성할 수 있다.

```swift
var namesOfIntegers = [Int: String]()
// namesOfIntegers는 빈 [Int: String]타입의 딕셔너리
```

`[Int: String]`타입의 빈 딕셔너리를 생성하고 사람이 읽을 수 있는 정수 값을 저장한다.   키는 `Int`형이고, 값은 `String`형이다.<br>

문맥이 이미 타입 정보를 제공하면, `[:]`의 빈 딕셔너리 리터럴을 사용해 빈 딕셔너리를 생성할 수 있다.

```swift
namesOfIntegers[16] = "sixteen"
// namesOfInteger는 1개의 키-값 쌍을 갖음
namesOfIntegers = [:]
// namesOfIntegers는 [Int: String]형의 빈 딕셔너리
```



### Creating a  Dictionary with a Dictionary Literal (딕셔너리 리터럴로 딕셔너리 생성하기)

앞에 본 배열 리터럴과 구문이 유사한 딕셔너리 리터럴로 딕셔너리를 초기화할 수 있다. 딕셔너리 리터럴은 딕셔너리 컬렉션으로써 하나 이상의 키-값 쌍을 작성하는 단순 방법이다.<br>

키-값 쌍은 키와 값의 결합이다. 딕셔너리 리터럴에서 키와 값은 키-값 쌍이 콜론으로 분리된 것이다. 키-값 쌍은 리스트로 작성되며 콤마로 구분되고 대괄호로 감쌓였다.<br>

[key 1: value 1, key 2: value 2, key 3: value 3]

아래 예는 공항의 이름을 저장하는 딕셔너리를 생성했다. 딕셔너리에서 키는 3개 문자의 국제 운송 연합 코드이고 값은 공항 이름이다.

```swift
var airports: [String: String] = ["YYZ": "Toronto Pearson", "DUB": "Dublin"]
```

`airports`딕셔너리는 "딕셔너리는 `String`타입의 키와 `String`타입의 값을 갖는다"라는 표현의 `[String: String]`타입을 갖는다고 명시되어 있다.<br>

`airports`딕셔너리는 2개의 키-값 쌍을 갖는 딕셔너리 리터럴로 초기화 되어 있다. 첫 번째 쌍은 키가 `"YYZ"`이고, 값은 `"Toronto Person"` 이다. 두 번째 쌍은 키가 `"DUB"`이고, 값은 `"Dublin"`이다.<br>

두 `String: String`쌍을 갖는 딕셔너리 리터럴이다. 이 키-값 타입은 `airpots`변수의 명시와 일치한다. 그래서 딕셔너리 리터럴 할당은 `airports`딕셔너리를 두 초기 항목과 초기화하는 방법으로 허가한다.<br>

일관된 타입의 키-값을 갖는 딕셔너리 리터럴로 초기화되어 있다면 배열처럼 딕셔너리의 타입을 적지 않아도 된다. `airports`의 초기화는 짧은 형식으로 되어 있다.

```swift
var airports = ["YYZ": "Toronto Pearson", "DUB": "Dublin"]
```

리터럴 안의 모든 키는 같은 타입으로 되어있고 마찬가지로 모든 값도 같은 타입으로 되었어 Swift는 `airports`딕셔너리에 `[String: String]`이 정확한 타입이라는 것을 추론 할 수 있다.



### Accessing and Modifying a Dictionary (딕셔너리에 접근 및 수정)

메소드나 프로퍼티 또는 서브 스크립트 구문을 사용해 딕셔너리에 접근하거나 수정할 수 있다.<br>

배열처럼 딕셔너리의 항목의 수를 찾기 위해서는 `count`프로퍼티를 사용한다.

```swift
print("The airports dictionary contains \(airports.count) items.")
// The airports dictionary contains 2 items. 출력
```

`isEmpty`프로퍼티를 사용해 `count`프로퍼티가 0과 동일한 것을 간편하게 찾아낼 수 있다.

```swift
if airports.isEmpty {
    print("The airports dictionary is empty.")
} else {
    print("The airports dictionary is not empty.")
}
// Prints "The airports dictionary is not empty. 출력
```

서브 스크립트 구문을 사용해 딕셔너리에 새로운 항목을 추가할 수 있다. 서브 스크립트 인데스로써 적당한 타입의 새로운 키를 사용하고 적당한 타입의 새 값을 할당한다.

```swift
airports["LHR"] = "London"
// airports 딕셔너리는 3개의 항목을 포함
```

서브 스크립트 구문을 사용해 특정 키의 값을 바꿀 수 있다.

```swift
airports["LHR"] = "London Heathrow"
// 키 "LHR"의 값은 "London Heathrow" 로 바뀜
```

서브 스크립트의 대안으로 딕셔너리의 `updateValue(_:forKey:)`메소드를 사용해 특정 키의 값을 바꾸거나 설정 할 수 있다. 위의 서브 스크립트 처럼, `updateValue(_:forKey:)`메서드는 키가 없는 경우 값을 설정하고, 키가 있는 경우 값을 변경한다. 그러나 서브 스크립트와는 다르게, `updateValue(_:forKey:)`메서드는 업데이트가 수행된 후 이전 값을 반환한다. 이를 통해 업데이트가 이루어졌는지의 여부를 확인할 수 있다.<br>

`updateValue(_:forKey:)`메서드는 딕셔너리의 값 타입의 옵셔널한 값을 반환한다. 예를 들어, `String`값을 저장하는 딕셔너리에서 메서드는 `String?`이나 `옵셔널 String`의 값을 반환한다. 이 옵셔널한 값은 업데이트 전에 존재했던 값이나 값이 없었으면 `nil`값을 포함한다:

```swift
if let oldValue = airports.updateValue("Dublin Airport", forKey: "DUB") {
    print("The old value for DUB was \(oldValue).")
}
// The old value for DUB was Dublin. 출력
```

서브 스크립트 구문을 사용해 해당 키에 `nil`값을 할당해 딕셔너리에서 키-값 쌍을 제거할 수 있다.

```swift
airports["APL"] = "Apple International"
// "Apple International"값은  APL키의 진짜 값이 아니여서 삭제
airports["APL"] = nil
// APL은 딕셔너리에서 삭제
```

또한, 딕셔너리에서 키-값 쌍을 제거하기 위해 `removeValue(forKey:)`메서드를 사용할 수 있따. 이 메서드는 존재하면 키-값 쌍을 제거하고 제거된 값을 반환하거나 값이 존재하지 않으면 `nil`을 반환한다.

```swift
if let removedValue = airports.removeValue(forKey: "DUB") {
    print("The removed airport's name is \(removedValue).")
} else {
    print("The airports dictionary does not contain a value for DUB.")
}
// The removed airport's name is Dublin Airport. 출력
```



### Iterating Over a Dictionary (딕셔너리 전체 반복)

`for-in`루프를 사용해 딕셔너리 전체를 반복할 수 있다. 딕셔너리 각 항목은 `(key, value)`튜플로 반환되고 튜플의 멤버를 일시적 상수나 변수로 분해할 수 있다.

```swift
for (airportCode, airportName) in airports {
    print("\(airportCode): \(airportName)")
}
// LHR: London Heathrow
// YYZ: Toronto Pearson
```

키 및 값 속성에 접근하여 키 또는 값의 반복 가능한 컬렉션을 검색 할 수도 있다.

```swift
for airportCode in airports.keys {
    print("Airport code: \(airportCode)")
}
// Airport code: LHR
// Airport code: YYZ

for airportName in airports.values {
    print("Airport name: \(airportName)")
}
// Airport name: London Heathrow
// Airport name: Toronto Pearson
```

Array 인스턴스를 사용하는 API와 함께 딕셔너리의 키 및 값을 사용하는 경우 `keys` 또는 `values`프로퍼티로 새 배열을 초기화 한다.

```swift
let airportCodes = [String](airports.keys)
// airportCodes is ["LHR", "YYZ"]

let airportNames = [String](airports.values)
// airportNames is ["London Heathrow", "Toronto Pearson"]
```

Swift의 딕셔너리 타입은 순서를 정의하지 않는다. 특정 순서로 딕셔너리의 키나 값을 반복하려면 `sorted()`메서드를 `keys`나 `values`프로퍼티를 사용한다.

