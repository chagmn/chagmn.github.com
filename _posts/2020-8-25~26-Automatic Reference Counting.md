---
title: "Automatic Reference Counting"
date: 2020-8-25~26
categories: Swift5 
---



# Automatic Reference Counting (자동 참조 계수)

 Swift는 ARC를 사용해 앱의 메모리 사용을 추적하고 관리한다. 대부분 경우, Swift에서 메모리 관리는 "그냥 작동"하며 스스로 메모리 관리에 대해 생각할 필요가 없음을 의미한다. ARC는 인스턴스가 더이상 필요하지 않을 때 클래스 인스턴스에 의해 사용되는 메모리를 자동으로 정리한다.<br>

그러나 ARC 몇몇 경우에서 메모리 관리를 위해서 코드에서 부분 사이의 관계에 대한 더 많은 정보가 필요하다. 이 챕터에서는 이러한 상황을 묘사하고 앱의 ARC가 메모리의 모든 관리를 어떻게 가능하게 하는지 보여준다. <br>

참조 계수는 클래스의 인스턴스에 오직 제공된다. 구조체와 열거형은 참조 타입이 아닌 값 타입이고 참조에 의해 저장 및 전달되지 않는다.



## How ARC Works (어떻게 ARC가 작동하는가)

항상 새로운 클래스의 인스턴스를 만들 때 ARC는 해당 인스턴스에 대한 정보를 저장하기 위해 메모리 청크에 할당한다. 이 메모리는 인스턴스의 타입에 대한 정보와 인스턴스와 연관되는 저장되는 프로퍼티의 값을 함께 갖는다. <br>

추가로, 인스턴스가 더 이상 필요 없을 때, ARC는 인스턴스에 의해 사용된 메모리를 정리해서 메모리가 다른 목적으로 사용될 수 있게 한다. 이것은 클래스 인스터스가 더 이상 필요 없을 때 메모리에서 공간을 비워주는 것을 보장한다. <br>

그러나, 만약 ARC가 여전히 사용되는 인스턴스를 할당 해제하면, 인스턴스의 프로퍼티에 더이상 접근할 수 없거나 인스턴스의 메소드를 호출할 수 없다. 실제로 만약 인스턴스에 접근하려 하면, 앱은 대부분 충돌이 일어날 것 이다. <br>

인스턴스가 필요할 때까지 사라지지 않게 확실히 하려면 ARC 얼마나 많은 프로퍼티, 상수 그리고 변수가 현재 각 클래스 인스턴스에 참조되는지 추적해야 한다. ARC는 해당 인스턴스에 대한 적어도 하나 이상의 활성 참조가 존재하는한 인스턴스 할당을 취소하지 않을 것 이다.<br>

이것이 가능하게 만드려면 언제든지 클래스 인스턴스를 프로퍼티, 상수, 변수에 할당할 때마다 해당 프로퍼티, 변수, 상수는 인스턴스에 대한 강한 참조를 만든다. 이러한 참조는 "강한"참조라고 불린다. 왜냐하면 참조는 해당 인스턴스를 확고하게 유지하고 강력한 참조가 유지되는 한 할당 해제를 허용하지 않기 때문이다.



## ARC in Action (동작 중인 ARC)

어떻게 ARC가 작동하는지에 대한 예시가 있다. 이 예제는 상수 프로퍼티를 저장하기 `name`이 정의된  `Person`이라 불리는 간단한 클래스로 시작한다.

```swift
class Person {
  let name: String
  init(name: String){
    self.name = name
    print("\(name) is being initialized")
  }
  deinit{
    print("\(name) is being deinitialized")
  }
}
```

`Person`클래스는 인스턴스의 `name`프로퍼티와 초기화 되었다는 메세지 출력을 갖는 초기화가 있다. `Person`클래스는 또한 클래스 인스턴스가 할당 해제될때 메시지를 출력하는 초기화 해제자도 있다.<br>

다음 코드는 뒤의 코드의 새로운 `Person`인스턴스에 대한 여러 참조를 설정하는데 사용되는 `Person?`타입의 3개 변수를 정의한다. 왜냐하면 이 변수들은 옵셔널 타입(`Person?`)이고 `nil`값으로 자동적으로 초기화되고 `Person`인스턴스를 현재 참조하고 있지 않다.

```swift
var reference1: Person?
var reference2: Person?
var reference3: Person?
```

새로운 `Person`인스턴스를 만들 수 있고 이 3개 변수 중 하나에 할당할 수 있다.

```swift
reference1 = Person(name: "John Appleseed")
// "John Appleseed is being initialized" 출력
```

새 `Person`인스턴스는  `reference1`변수에 할당되고 `reference1`에서 `Person`인스턴스로 강한 참조가 이루어 진다. 적어도 하나의 강한 참조가 있기 때문에, ARC는 `Person`을 메모리에 보관하고 할당 해제하지 않을 수 있다. <br>

같은 `Person`인스턴스를 두 개 이상 변수에 할당한다면, 두개 이상의 강한참조가 해당 인스턴스에 대해 만들어 진다.

```swift
reference2 = reference1
reference3 = reference1
```

하나의 `Person`인스턴스에 3개의 강한 참조가 있다. <br>

만약 원래 참조를 포함한 두 개의 강한 레퍼런스를 두 변수에 `nil`을 할당해 깨트린다면, 하나의 강한 참조가 남고 `Person`인스턴스는 할당 해제되지 않는다.

```swift
reference1 = nil
reference2 = nil
```

ARC는 3번째와 마지막 강한 참조가 깨지기 전까지 `Person`인스턴스를 할당 해제하지 않을 것이다. 

```swift
reference3 = nil
// "John Appleseed is being deinitialized" 출력
```





## Strong Reference Cycles Between Class Instances (클래스 인스턴스 사이에서 강한 참조 주기)

위의 예제에서, ARC는 생성하는 새로운 `Person`인스턴스의 수를 추적할 수 있고 더 이상 필요 없을 때 `Person`를 할당 해제할 수 있었다.<br>
그러나, 클래스 인스턴스가 강력한 참조가 없는 지점에 도달하지 않는 코드를 작성할 수도 있다. 만약 2개의 클래스 인스턴스가 서로에 대한 강한 참조를 유지해 각 인스턴스가 다른 인스턴스를 유지하고 있을 때 일어날 수 있다. 이것은 강한 참조 주기(strong reference cycle) 이라고 알려져 있다. <br>

강한 참조 주기를 해결하기 위해서는 클래스 간의 관계를 strong 대신 weak이나 unowned 참조로 정의할 수 있다. <br>

어떻게 강한 참조 주기가 실수로 만들어 지는 예시가 있다. 이 예제는 아파트와 거주자를 표현한 `Person`과 `Apartment`라는 두 개의 클래스를 정의했다.

```swift
class Person {
  let name: String
  init(name: String) { self.name = name }
  var apartment: Apartment?
  deinit { print("\(name) is being deinitialized")}
}

class Apartment {
  let unit: String
  init(unit: String) { self.unit = unit }
  var tenant: Person?
  deinit { print("Apartment \(unit) is being deinitialized") }
}
```

모든 `Person`인스턴스는 `name`프로퍼티와 `nil`로 초기화된 옵셔널`apartment`프로퍼티를 갖고 있다.

사람은 항상 아파트를 갖고있지 않기 때문에 `apartment`프로퍼티는 옵셔널이다.<br>

유사하게 모든 `Apartment`인스턴스는 `unit`프로퍼티와 `nil`로 초기화된 옵셔널`tenant`속성을 갖고 있다. 거주자 속성은 아파트는 항상 거주자를 갖는게 아니여서 옵셔널이다.<br>

두 클래스 모두 초기화 해제자가 정의되어 있다. `Person`과 `Apartment` 인스턴스가 할달 해제될 때를 예상할 수 있게 한다.<br>

다음 코드는 `Apartment`와 `Person`인스턴스에 할당하는 `john`과 `unit4A`라 불리는 옵셔널 타입의 두 변수를 정의했다. 두 변수 선택사항이기 때문에 모두 `nil`값으로 초기화한다.

```swift
var john: Person?
var unit4A: Apartment?
```

`Person`인스턴스와 `Apartment`인스턴스를 만들고 새 인스턴스를 `john`과 `unit4A` 변수에 할당한다

```swift
john = Person(name: "John Appleseed")
unit4A = Apartment(unit: "4A")
```

두 인스턴스를 만들어지고 할당된 후에 강력한 참조가 어떻게 표시되는지 다음과 같다. `john`변수는 새 `Person`인스턴스에 강한 참조를 갖고있고 `unit4A`변수는 새 `Apartment`인스턴스에 강한 참조를 갖고 있다:

![](https://docs.swift.org/swift-book/_images/referenceCycle01_2x.png)

 두 인스턴스를 같이 연결할 수 있어서 person은 apartment를 갖을 수 있고 apartment는 tenant를 갖을 수 있다. 느낌표(`!`)는 `john`과 `unit4A`옵셔널 변수 내에 저장된 인스턴스를 풀고 액세스하는데 사용되므로 해당 인스턴스의 프로퍼티를 설정할 수 있다.

```swift
john!.apartment = unit4A
unit4A.tenant = john
```

어떻게 강한 참조가 두 인스턴스를 같이 연결한 후에 강한 참조가 표시되는 방식은 다음과 같다.

![](https://docs.swift.org/swift-book/_images/referenceCycle02_2x.png)

불행하게도, 두 인스턴스 연결은 사이에 강한 참조 주기를 만든다. `Person`인스턴스는 `Apartment`인스턴승에 강한 참조를 갖고 `Apartment`인스턴스는 `Person`인스턴스에 강한 참조를 갖는다. 그러므로, `john`과 `unit4A`변수가 갖는 강한 참조를 없앨때 참조수가 0으로 떨어지지 않고 인스턴스가 ARC에 의해 할당 해제되지 않는다.

```swift
john = nil
unit4A = nil
```

두 변수가 `nil`이 될 때, 초기화 해제자가 호출된다. 강한 참주 주기가 `Person`과 `Apartment`인스턴스가 앱에서 메모리 누수로 인해 할당 해제 됨으로부터 보호한다. <br>

다음은 `john` 및 `unit4A`변수를 `nil`로 설정 한 후의 강력한 참조의 모습이다.

![](https://docs.swift.org/swift-book/_images/referenceCycle03_2x.png)

`Person`인스턴스와 `Apartment`인스턴스 사이의 강한 참조는 남고 깨지지 않는다.





## Resolving Strong Reference Cycles Between Class Instances (클래스 인스턴스 사이에서 강한 참조 주기 해제)

Swift는 클래스 타입의 프로퍼티와 작업할때 강한 참조 주기를 해제하는 2가지 방법을 제공한다: 약한 참조(weak references)와 소유자가 없는 참조(unowned reference).<br>

약하고 소유자가 없는 참조를 사용하면 참조 주기에서 하나의 인스턴스가 강력한 유지없이 다른 인스턴스를 참조할 수 없다. 인스턴스들은 강한 참조 주기 생성 없이 각각을 참조할 수 있다.<br>

다른 인스턴스가 더 짧은 생명주기를 갖고 있을 때, 다른 인스턴스를 먼저 할당 해제 할 수 있는 경우 약한 참조를 사용한다. 위의 `Apartment`예제에서 아파트는 생명주기 중 어느 시점에 거주자가 없을 수 있는 것이 적절하므로 약한 참조는 이러한 경우 참조 주기를 끊는 적절한 방법이다. 반면에, 다른 인스턴스가 같은 생명 주기나 더 긴 생명주기를 가질때 소유자가 없는 참조를 사용한다.



### Weak References (약한 참조)

약한 참조는 참조하는 인스턴스를 강력하게 유지하지 않는 참조여서 ARC가 참조된 인스턴스를 처리하는 것을 중지하지 않는다. 이 행동은 참조가 강한 참조 주기의 부분이 되는 것을 막는다. 프로퍼티나 변수 명시전에 `weak`키워드를 적음으로써 약한 참조인 것을 가리킨다. <br>

약한 참조는 참조하는 인스턴스를 강력하게 유지하기 않기 때문에 약한 참조가 참조하는 동안 해당 인스턴스가 할당 해제 될 수 있다. 그러므로 ARC는 자동적으로 참조하는 인스턴스가 할당 해제되면 자동으로 약한 참조를 `nil`로 설정한다. 그리고 약한 참조는 실행 중에 그들의 값이 `nil`로 바꾸기 위해서는 허락이 필요하기 때문에 항상 상수보다는 옵셔널 타입의 변수로 명시해야한다.<br>

다른 옵셔널 값처럼 약한 참조에 값의 존재를 확인 할 수 있고 더이상 존재하지 않는 유효하지 않은 인스턴스에 대한 참조로 끝나지 않는다.<br>

아래 예제는 위의 `Person` 및 `Apartment` 예제와 동일하지만 한 가지 중요한 차이점이 있다. 이번에는 `Apartment` 타입의 `tenant`프로퍼티는 약한 참조로 선언되었다.

```swift
class Person {
  let name: String
  init(name: String) { self.name = name }
  var apartment: Apartment?
  deinit { print("\(name) is being deinitialized") }
}

class Apartment {
  let unit: String
  init(unit: String) { self.unit = unit }
  weak var tenant: Person?
  deinit { print("Apartment \(unit) is being deinitialized") }
}
```

두 변수(`john`, `unit4A`)로 부터 강한 참조와 두 인스턴스 사이의 연결은 전에 생성되었다.

```swift
var john: Person?
var unit4A: Apartment?

john = Person(name: "John Applesseed")
unit4A = Apartment(unit: "4A")

john!.apartment = unit4A
unit4A!.tenant = john
```

두 인스턴스를 같이 연결한 참조의 모습이다:

![](https://docs.swift.org/swift-book/_images/weakReference01_2x.png)

`Person`인스턴스는 `Apartment`인스턴스에 강한 참조를 갖고있지만, `Apartment`인스턴스는 `Person`d인스턴스에 대해 약한 참조를 갖고 있다. 이 뜻은 `john`변수를 `nil`로 저장함으로써 강한 참조를 깰 수 있고 `Person`인스턴스에 대한 더 이상 강한 참조가 없다는 것이다.

```swift
john = nil
// "John Appleseed is being deinitialized" 출력
```

`Person`인스턴스에 대해 더 이상 강한 참조가 없기 때문에 할당 해제하고 `tenant`프로퍼티는 `nil`로 설정된다.

![](https://docs.swift.org/swift-book/_images/weakReference02_2x.png)

`Apartment`인스턴스에 강한 참조는 `unit4A`변수로 부터 남아있다. 만약 이 강한 참조를 깨면, 더이상 `Apartment`인스턴스에 대한 강한 참조가 없다.

```swift 
unit4A = nil
// "Apartment 4A is being deinitialized" 출력
```

이것 또한 해제되서 `Apartment`인스턴스에 대해 더 이상 강한 참조는 없다.

![](https://docs.swift.org/swift-book/_images/weakReference03_2x.png)



### Unowned References (소유자가 없는 참조)

소유자가 없는 참조는 약한 참조와 다르게 참조 대상이 되는 인스턴스가 현재 참조하고 잇는 것과 같은 생명주기를 갖거나 더 긴 생명주기를 갖기 때문에 항상 참조에 그 값이 있다고 생각이 된다. 그래서 ARC는 소유자가 없는 참조에는 절대 `nil`을 할당하지 않는다. 다시 말하면 소유자가 없는 참조는 옵셔널 타입을 사용하지 않는다.<br>

예제를 통해 소유자가 없는 참조의 동작을 확인하면 `Customer`과 `CreditCard` 두 개의 클래스를 선언한다.

```swift
class Customer {
  let name: String
  var card: CreditCard?
  init(name: String) {
    self.name = name
  }
  deinit{ print("\(name) is being deinitialized") }
}

class CreditCard {
  let number: UInt64
  unowned let customer: Customer
  init(number: UInt64, customer: Customer) {
		self.number = number
    self.customer = customer
  }
  deinit { print("Card #\(number) is being deinitialized")}
}
```

여기서 `Customer`는 `card`변수로 `CreditCard`인스턴스를 참조하고 있고 `CreditCard`는 `customer`로 `Customer`인스턴스를 참조하고 있다. `customer`는 소유자가 없는 참조 `unowned`로 선언한다. 그 이유는 고객과 신용카드를 비교할 때 신용카드는 없더라도 사용자는 남아있을 것이기 때문이다. 다시 말하면 사용자는 항상 존재한다. 그래서 `CreditCard`에 `customer`를 `unowned`로 선언한다.<br>

이제 고객 변수 `john`을 옵셔널 타입으로 선언한다.

```swift
var john: Customer?
```

선언한 고객에 인스턴스를 생성하고 고객의 카드 변수에도 카드 인스턴스를 생성해 할당한다.

```swift
john = Customer(name: "John Appleseed")
john!.card = CreditCard(number: 1234_5678_9012_3456, customer: john!)
```

이 시점에서의 참조 상황을 그림으로 표현하면 다음과 같다. `john`이 `Customer`인스턴스를 참조하고 있고 `CreditCard`인스턴스도 `Customer`인스턴스를 참조하고 있지만 소유하지 않은 참조를 하고 있기 때문에 `Customer`인스턴스에 대한 참조 횟수는 1회가 된다.

![](https://gblobscdn.gitbook.com/assets%2F-LKLx6PA5iF3Uq2IzQsf%2F-LKMaRBboSBx7woa9c3T%2F-LKMboJ91iOKc8rnK1h4%2FunownedReference01_2x.png?alt=media&token=7912e0f3-ddef-4708-a492-3f9afe66aa55)

이 상황에서 `john`변수의 `Customer`인스턴스 참조를 끊으면 다음 그림과 같다.

![](https://gblobscdn.gitbook.com/assets%2F-LKLx6PA5iF3Uq2IzQsf%2F-LKMaRBboSBx7woa9c3T%2F-LKMbr4xrSDSZSAmqZlX%2FunownedReference02_2x.png?alt=media&token=1b8a32a2-aa65-4c96-9c8f-5ef4a455faf4)

그러면 더 이상 `Customer`인스턴스를 강하게 참조하고 있는 인스턴스가 없으므로 `Customer`인스턴스가 해제되고 인스턴스가 해제됨에 따라 `CreditCard`인스턴스를 참조하고 있는 개체도 사라져서 `CreditCard`인스턴스도 메모리에서 해제된다.

```swift
john = nil
// "John Appleseed is being deinitialized" 출력
// "Card #12345667890123456 is being deinitialized" 출력
```



### Unowned Optional References (미소유자 옵셔널 참조)

옵셔널 참조를 클래스에 미소유자로 표시할 수 있다. ARC 소유권 모델과 관련해 소유되지 않은 옵셔널 참조와 약한 참조 모두 동일한 문백에서 사용될 수 있다. 차이점은 미소유자 옵셔널 참조를 사용할 때 항상 유효한 객체를 참조하거나 `nil`로 설정되어 있는지 확인해야 한다. <br>

다음은 학교의 특정 부서에서 제공하는 과정을 추적하는 예제이다.

```swift
class Department {
  var name: String
  var courses: [Course]
  init(name: String) {
    self.name = name
    self.courses = []
  }
}

class Course {
  var name: String
  unowned var department: Department
  unowned var nextCourse: Course?
  init(name: String, in department: Department) {
    self.name = name
    self.department = department
    self.nextCourse = nil
  }
}
```

`Department`는 부서가 제공하는 각 경로에 대해 강한 참조를 유지한다. ARC 소유권 모델에서 부서는 그들만의 경로를 소유한다. `Course`는 하나는 부서 그리고 하나는 학생들이 가질 다음 경로를 갖는 2개의 미소유자 참조를 갖고 경로는 이러한 객체를 소유하지 않는다. 모든 경로는 부서의 부분으로 `department`프로터피는 옵셔널이 아니다. 그러나, `nextCourse`프로퍼티는 옵셔널이기 때문에 각 경로는 추천된 코스를 갖고있지 않는다.<br>

이 클래스들을 사용하는 예제가 있다.<br>

```swift
let department = Department(name: "Horticulture")

let intro = Course(name: "Survey of Plants", in: department)
let intermediate = Course(name: "Growing Common Herbs", in: department)
let advanced = Course(name: "Caring for Tropical Plants", in: department)

intro.nextCourse = intermediate
intermediate.nextCourse = advanced
department.courses = [intro, intermediate, advanced]
```

우의 코드에서 부서를 생성하고 3가지 경로를 생성했다. intro 와 intermediate 코드는 모두 `nextCourse`프로퍼티에 저장된 추천된 다음 코스를 제안하고, 이는 학생이 코스를 완료한 후 수강해야하는 코스에 대한 미소유자 옵셔널 참조를 유지한다. <br>

![](https://docs.swift.org/swift-book/_images/unownedOptionalReference_2x.png)

미소유자 옵셔널 참조는 감싸진 클래스의 인스턴스에 대한 강한 참조를 유지하지 않는다. 따라서, ARC가 인스턴스 할당을 해제하는 것을 방지하지 않는다. 소유되지 않은 옵셔널 참조가 `nil` 일 수 있다는 점을 제외하면 미소유자 참조가 ARC에서 수행하는 것과 동일하게 작동한다. <br>

옵셔널이 아닌 미소유자 참조와 마찬가지로 `nextCourse`가 항상 할당 취소되지 않은 과정을 참조하도록 할 책임이 있다. 예를 들면 `department.courses`에서 코스를 삭제할 때 다른 코스에 있을 수도 있는 모든 참조도 제거해야 한다.





## Strong Reference Cycles for Closures (클로저에 대한 강한 참조 주기)

위에서 어떻게 강한 참조 주기가 두 클래스 인스턴스 프로퍼티가 각각 강한 참조를 유지할때 생성될 수 있는지 보여줬다. 또한 어떻게 약한, 미소유자 참조를 사용해 강한 참조 주기를 부시는지 봤다.<br>

강한 참조 주기는 클래스 인스턴스의 프로퍼티에 클로저를 할당하고 해당 클로저의 본문이 인스턴스를 캡처하는 경우에 발생할 수 있다. 이 캡처는 클로저의 몸체가 `self.someProperty`같이 인스턴스에 접근하기 때문에 발생할 것 이다. 또는 클로저가 `self.someMethod()` 같이 인스턴스의 메소드를 호출하기 때문일 것 이다. 각 경우 모두 이러한 액세스로 인해 클로저가 자신을 "캡쳐"해 강력한 참조 주기를 생성한다.<br>

강한 참조 주기는 클래스와 같은 클로저가 참조 타입이기 때문에 발생한다. 프로퍼티에 클로저를 할당하려 할 때, 해당 클로저에 대한 참조를 할당할 것 이다. 본질적으로, 위에서의 두 강함 참조가 서로 를 유지하고 있는 것과 같은 문제다. 그러나 두 클래스 인스턴스가 아니라 이번에는 서로를 유지하는 클래스 인스턴스와 클로저다.

Swift는 이 문제에 대해 `closure capture list`라는 좋은 해결 방법을 갖고 있다. 그러나 클로저 캡쳐 리스트를 이용해 강한 참조 주기를 어떻게 해결하는지 배우기 전에 그러한 주기가 어떻게 발생할 수 있는지 이해하는 것이 유용하다.<br>

아래 예제는 `self`를 참조하는 클로저를 사용할 때 어떻게 강한 참조 주기를 생성할 수 있는지 보여준다. 이 예제는 HTML 문서안의 각각의 요소를 위한 간단한 모델을 제공하는 `HTMLElement`라는 클래스를 정의한 예다.

```swift
class HTMLElement {
  let name: String
  let text: String?
  
  lazy var asHTML: () -> String = {
    if let text = self.text {
      return "<\(self.name)>\(text)</\(self.name)>"
    } else {
      return "<\(self.name)/>"
    }
  }
  
  init(name: String, text: String? = nil) {
		self.name = name
    self.text = text
  }
  
  deinit {
    print("\(name) is being deinitialized")
  }
}
```

`HTMLElement`클래스는 제목요소인 `h1`과 같은 요소의 이름들을 가리키는 `name`프로퍼티를 정의한다.

`HTMLElement`는 해당 HTML요소 내에서 렌더링 될 텍스트를 나타내는 문자열로 설정할 수 있는 옵셔널 `text`프로퍼티를 정의한다.<br>

추가로 두 프로퍼티외에도 `HTMLElement`클래스는 `asHTML`이라는 lazy 속성을 정의한다. 이 속성은 `name`과 `text`를 HTML문자열 조각으로 결합하는 클로저를 참조한다. `asHTML`프로퍼티는 `() -> String`타입이거나 매개변수가 없고 String 값을 반환하는 함수다.<br>

기본적으로, `asHTML`프로퍼티는 HTML태그의 문자열 표현을 반환하는 클로저가 할당된다. 이 태그는 옵셔널 `text`값이 있는 경우 포함되고 `text`값이 없는 경우 텍스트 콘텐츠가 없다.<br>

`asHTML`속성은 약간 인스턴스 메소드같이 작명되고 사용된다. 그러나 `asHTML`은 인스턴스 메소드보다 클로저 속성이기 때문에 특정 HTML 요소에 대한 HTML 렌더링을 변경하려는 경우 `asHTML`속성의 기본값을 사용자 정의 클로저로 바꿀 수 있다.<br>

예를 들어, `asHTML`속성 표현이 빈 HTML 태그를 반환하는 것을 방지하기 위해 `text`속성이 `nil`인 경우 일부 텍스트를 기본값으로하는 클로저로 설정할 수 있다.

```swift
let heading = HTMLElement(name: "h1")
let defaultText = "some default text"
heading.asHTML = {
  return "<\(heading.name)>\(heading.text ?? defaultText)</\(heading.name)>"
}
print(heading.asHTML())
// "<h1>some default text</h1>" 출력
```

`HTMLElement`클래스는  `name`인수와 `text`인수(원하는 경우)를 사용해서 새 요소를 초기화하는 단일 이니셜라이저를 제공한다. <br>
`HTMLElement`클래스를 사용해 인스턴스를 생성하고 출력하는 방법을 보여준다:

```swift
var paragraph: HTMLElement? = HTMLElement(name: "p", text: "hello, world")
print(paragraph!.asHTML())
// "<p>hello, world</p>" 출력 
```

불행하게도 `HTMLElement`는 `HTMLElement`인스턴스와 기본 `asHTML`값을 위해 사용된 클로저 사이에 강한 참조 주기가 생성된다.

![](https://docs.swift.org/swift-book/_images/closureReferenceCycle01_2x.png)

인스턴스의 `asHTML`프로퍼티는 클로저와 강한 참조를 유지하고 있다. 그러나 클로저는 (`self.name`및 `self.text`를 참조하는 방법으로) 본문 내에서 `self`를 참조하기 때문에 클로저는 `self`를 캡처한다. 즉 `HTMLElement`인스턴스에 대한 강력한 참조를 다시 보유한다는 의미다. 강한 참조 주기는 두개 사이에서 생성된다.<br>

만약 `paragraph`변수를 `nil`로 설정하거나 `HTMLElement`인스턴스에 대한 강력한 참조를 중단하면 강력한 참조 주기로 인해 `HTMLElement`인스턴스와 해당 클로저가 할당 해제되지 않는다.

```swift
paragraph = nil
```

`HTMLElement`의 초기화 해제자의 메시지는 출력되지 않으며 `HTMLElement`인스턴스가 할당 해제되지 않았음을 보여준다.



## Resolving Strong Reference Cycles for Closures (클로저에 대한 강한 참조 주기 해결법)

클로저의 정의의 일부분으로써 캡쳐 리스트(caputre list)를 정의함으로써 클로저와 클래스 인스턴스 사이의 강한 참조 주기를 해결한다. 캡쳐 리스트는 클로저 몸체에서 한개 이상의 참조 타입을 캡쳐할때 사용되는 규칙들이 정의한다. 두 클래스 인스턴스 사이의 강한 참조 주기와 같이 각 캡쳐된 참조를 강한참조보다 약하거나 미소유 참조로 명시 할 수 있다. 약하거나 미소유의 적당한 선택은 코드의 다른 부분 사이의 관계에 의존된다.



### Defining a Capture List (캡쳐 리스트 정의)

캡쳐 리스트의 각 항목은 클래스 인스턴스(`self`)나 일부 값으로 초기화된 변수(`delegate = self.delegate`)에 대한 참조가 있는 `weak`이나 `unowned`키워드 쌍이다. 이 쌍은 쉼표(`,`)로 구분되고 대괄호(`[ ]`)안에 적힌다.<br>

클로저의 매개변수 리스트와 반환 타입 전에 캡쳐 리스트가 위치한다.

```swift
lazy var someClosure = {
  [unowned self, weak delegate = self.delegate]
  (index: Int, stringToProcess: String) -> String in
	// 클로저 본문은 여기에..
}
```

만약 클로저에서 적당한 매개변수 리스트와 반환타입이 문맥에서 추정되어서 존재하지 않을 경우 캡쳐 리스트는 클로저의 시작 부분에 위치하고 `in`키워드가 뒤에 적힌다.

```swift
lazy var someClosure = {
  [unowned self, weak delegate = self.delegate] in
  // 클로저 본문은 여기에...
}
```



### Weak and Unowned References (약하고 미소유 참조)

클로저와 캡처하는 인스턴스가 항상 각각을 참조하고 항상 같은 시간에 할당 해제 될 때, 클로저의 캡쳐를 미소유 참조로써 정의한다. <br>

반대로, 캡쳐된 참조가 미래에 어떤 지점에서 `nil`이 될 때 약한 참조로써 캡처가 정의된다. 약한 참조는 항상 옵셔널 타입이고 자동적으로 참조하는 인스턴스가 `nil`이 된다. 이것은 그들의 존재를 클로저의 본문안에서 체크할 수 있게 한다.<br>

미소유 참조는 위의 `HTMLElement`예제에 강한 참조 주기를 해결하기 위해 적당한 캡쳐 메소드를 사용할 수 있다:

```swift	
class HTMLElement {
  let name: String
  let text: String?
  
  lazy var asHTML: () -> String = {
    [unowned self] in
    if let text = self.text {
			return "<\(self.name)>\(text)</\(self.name)>"
    } else {
      return "<\(self.name) />"
    }
  }
  
  init(name: String, text: String? = nil) {
    self.name = name
    self.text = text
  }
  
  deinit {
    print("\(name) is being deinitialized")
  }
}
```

`HTMLElement`구현은 이전 구현과 같고 `asHTML` 클로저 안에 캡쳐 리스트가 추가 되었다. 이 경우 캡쳐 리스트는 "강한 참조보다 미소유 참조로써 자신을 캡쳐한다"는 의미의 `[unowned self]` 이다.

<br>

`HTMLElement`인스턴스의 생성과 출력은 전에는 다음과 같이 했다.

```swift
var paragraph: HTMLElement? = HTMLElement(name: "p", text: "hello, world")
print(paragraph!.asHTML())
// "<p>hello, world</p>"
```

캡처 리스트와 참조가 어떻게 보이는지 나타낸다:

![](https://docs.swift.org/swift-book/_images/closureReferenceCycle02_2x.png)

이번엔, 클로저에 의한 `self`캡쳐는 미소유 참조이며 캡처한 `HTMLElement`인스턴스를 강력하게 유지하지 않는다. 만약 `paragraph`변수의 강한 참조를 `nil`로 설정하면 `HTMLElement`인스턴스는 아래 예제처럼 초기화 해제 메시지와 함께 할당 해제된다.

```swift
paragraph = nil
// "p is being deinitialized"
```



