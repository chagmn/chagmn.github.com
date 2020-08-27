---
title: "Memory Safety"
date: 2020-8-27
categories: Swift5

---



# Memory Safety (메모리 안정성)

기본으로 Swift는 코드에서 발생하는 안전하지 않은 행위를 막는따. 예를 들어, Swift 사용하기 전에 변수 초기화를 보장하 메모리가 할당 해제된 후에는 접근 불가능하며, 배열 인덱스에서 범위를 벗어난 오류가 있는지 확인한다.<br>

Swift는 메모리의 위치를 수정하는 코드가 해당 메모리에 대한 독점 액세스 권한을 갖도록 요구함으로써 동일한 메모리 구역에 다중 접속을 할 때 충돌하지 않도록 한다. Swift는 메모리를 자동적으로 관리하기 때문에, 대부분 메모리 접근에 대한 생각을 하지 않아도 된다. 그러나 잠재적인 충돌이 어디서 일어날 수 있는지 이해하는게 중요해서 메모리 접근할 때 충돌이 일어날 수 있는 코드 작성을 피할 수 있다. 만약 코드에서 충돌이 있다면 컴파일 타임이나 런타임 에러가 발생할 것 이다.<br>



## Understanding Conflicting Access to Memory (메모리에 대한 충돌 액세스 이해하기)

변수에 값을 할당하거나 함수에 인수가 전달 될 때 코드에서 메모리 접근하는 일이 발생한다. 예를 들어, 아래 코드에서 읽기 접근과 쓰기 접근 모두 포함하고 있다.<br>

```swift
// 메모리에 쓰기 접근
var one = 1

// 메모리에 읽기 접근
print("We're number \(one)!")
```

메모리 접근 충돌은 코드에서 다른 부분들이 같은 시간에 메모리의 같은 구역에 접근하려고 시도할 때 발생할 수 있다. 같은 시간에 메모리의 같은 구역에 다중 접속은 예측할 수 없고 일관성이 없는 행동을 만들 수 있다. Swift에서 몇 줄의 코드를 추가함으로써 값을 수정하는 몇가지 방법이 있어서 자체 수정 중에 값을 접근하는 시도는 가능하다.<br>

종이 한장에 적힌 예산을 수정하는 방법을 생각해보면 같은 문제를 볼 수 있다. 예산을 2단계로 수정하는 과정을 보자: 첫번째 항목의 이름과 가격을 추가하고 현재 리스트의 아이템을 반영해 총 양을 바꿀 것 이다. 수정 전과 후에 예산으로부터 정보를 읽을 수 있고 아래 숫자가 보여주는 것 처럼 정확한 답을 얻을 수 있다.

![](https://docs.swift.org/swift-book/_images/memory_shopping_2x.png)

예산에 새로운 아이템들을 추가할 동안 일시적으로 유효하지 않은 상태다. 왜냐하면 총 양이 새로 추가된 아이템들을 반영해 업데이트가 이루어지지 않았다. 아이템 추가 과정 동안 총 양을 읽는 것은 잘못된 정보를 줄 수 있다. <br>

이 예제 또한 충돌하는 메모리 액세스를 수정할 때 발생할 수 있는 문제점을 보여준다: 때로 서로 다른 답변을 생성하는 충돌을 해결하는 여러가지 방법이 있으며 어떤 답변이 올바른지 항상 명확하지는 않다. 이 예제에서, 원래 총 양이나 업데이트 된 총양을 원하는지에 따라 $5나 $320은 정확한 답변일 수 있다. 충돌하는 접근을 수정하기 전에 먼저 수행할 작업을 결정해야 한다.



### Characteristics of Memory Access (메모리 액세스 특징)

충돌하는 액세스의 문맥에서 고려해야 할 메모리 액세스의 3가지 특징이 있다: 액세스가 읽기나 쓰기인지, 액세스 기간, 액세스되는 동안 메모리의 위치. 특히, 충돌은 다음 조건을 모두 충족하는 두가지 접근이 있을 때 발생한다: 

* 적어도 하나 이상의 쓰기 액세스
* 메모리의 같은 위치에 액세스
* 기간이 겹침

읽기와 쓰기 액세스 사이의 차잉점은 보통 명확하다: 쓰기 액세스는 메모르의 위치를 바꾸지만 읽기 액세스는 그렇지 않다. 메모리의 위치는 액세스되는 항목을 가리킨다. 예를 들어, 변수, 상수나 프로퍼티가 있다. 메모리 액세스 기간은 순간적이거나 장기적이다. <br>

액세스가 시작된 후 종료되기 전에 다른 코드를 실행 할 수 없는 경우 액세스는 즉시 이루어진다. 성격상, 두 순간적인 액세스는 같은 시간에 일어날 수 없다. 대부분 메모르 접근은 순간적이다. 예를 들어, 아래 코드의 모든 읽고 쓰는 액세스는 순간적이다.

```swift
func oneMore(than number: Int) -> Int {
  return number + 1
}

var myNumber = 1
myNumber = oneMore(than: myNumber)
print(myNumber)
// "2" 출력
```

그러나 다른 코드의 실행에 걸쳐있는 장기적 액세스라 불리는 메모리 액세스에는 몇가지 방법이 있다. 순간 액세스와 장기 액세스 사이의 차이점은 장기 액세스가 시작된 후 종료되기 전에 다른 코드가 실행될 수 있다는 것이다. 이를 `overlap`이라 부른다. 장기 액세스는 다른 장기 액세스나 순간 액세스와 겹칠(overlap)수 있다.<br>
겹치는 액세스는 함수나 메소드에서 인-아웃 매개변수를 사용하거나 구조체의 메소드를 변화시킬 때 일시적으로 나타난다. 장기 액세스를 사용하는 Swift 코드의 특정 종류는 아래 섹션에서 다룬다.



## Conflicting Access to In-Out Parameters (인-아웃 매개변수에 대한 액세스 충돌)

함수는 모든 인-아웃 매개변수에 대해 장기 쓰기 액세스를 갖고 있다. 인-아웃 매개변수에 대한 쓰기 액세스는 모든 인-아웃 매개변수가 아닌 것들이 평가된 후에 시작되고 해당 함수 호출 기간 동안 지속된다. 만약 다중 인-아웃 매개변수가 있다면 쓰기 액세스는 매개변수가 나타나는 동일한 순서로 시작된다.<br>

장기 쓰기 액세스의 결과 중 하나는 범위 지정 규칙과 액세스 제어가 허용되더라도 인아웃으로 전달된 원래 변수에는 접근할 수 없다는 것이다. 원본에 대한 어떠한 접근은 충돌을 만든다. 예를 들어:

```swift
var stepSize = 1

func increment(_ number: inout Int) {
  number += stepSize
}

increment(&stepSize)
// 에러: stepSize에 대한 접근이 충돌
```

위의 코드에서 `stepSize`는 전역 변수고 보통 `increment(_:)`안에서부터 접근 가능하다. 그러나 `stepSize`에 읽기 액세스는 `number`에 쓰기 액세스와 겹친다. 아래 그림에서 보이는 것 처럼 `number`과 `stepSize`는 메모리의 같은 지역을 가리킨다. 읽기와 쓰기 액세스는 같은 메모리를 가리키고 그들은 겹치고(overlap) 충돌을 만든다.

![](https://docs.swift.org/swift-book/_images/memory_increment_2x.png)

이 충돌을 해결할 하나의 방법은 `stepSize`의 명시적 복사본을 만드는 것이다:

```swift
// 명시적 복사 생성
var copyOfStepSize = stepSize
increment(&copyOFStepSize)

// 원본을 업데이트
stepSize = copyOfStepSize
// StepSize 는 2가 됨
```

`increment(_:)`를 호출하기 전 `stepSize` 복사를 만들 때, `copyOfStepSize`의 값이 현재 stepsize로 부터 증가하는 것은 명확하다. 읽기 액세스는 쓰기 액세스가 시작하기 전에 끝나서 충돌이 일어나지 않는다.<br>



또 다른 인아웃 매개변수에 대한 장기 쓰기 액세스의 결과는 같은 함수의 다중 인아웃 매개변수에 인자로써 하나의 변수를 전달할 때 충돌이 발생한다. 예를 들어:

```swift
func balance(_ x: inout Int, _ y: inout Int) {
  let sum = x + y
  x = sum / 2
  y = sum - x
}
var playerOneScore = 42
var playerTwoScore = 30
balance(&playerOneScore, &playerTwoScore)  // 가능
balance(&playerOneScore, &playerOneScore)  // 에러: playerOneScore에 액세스 충돌
```

위의 `balance(_:_)` 함수는 두 매개변수를 수정해 총 값을 두 매개변수에 균등하게 나눈다. `playOneScore`와 `playerTwoScore`를 인자로써 호출하는 것은 충돌을 일으키지 않는다. 동시간에 두개의 쓰기 액세스가 겹치지만 메모리의 다른 지역에 액세스한다. 반면에 `playerOneScore`를 양쪽 매개변수로 전달하는 것은 충돌을 일으킨다. 두 개의 쓰기 액세스를 같은 시간에 메모리의 같은 구역에 접근하는 것을 수행하기 때문이다.



## Conflicting Access to self in Methods (메소드에서 자신에 대한 충돌)

구조체에서 변경 메소드는 메소드 호출 기간 동안 `self`에 대한 쓰기 액세스를 갖는다. 예를 들어, 각 플레이어가 데이미를 입을 때 감소하는 힐 양을 갖고 특수능력을 사용할 때 감소하는 에너지 양을 갖는 게임을 생각하자. <br>

```swift
struct Player {
  var name: String
  var health: Int
  var energy: Int
  
  static let maxHealth = 10
  mutating func restoreHealth() {
		health = Player.maxHealth
  }
}
```

위의 `restoreHealth()`메소드에서 `self`에 대한 액세스는 메소드 시작부분에서 시작하고 메소드가 반환하기 까지 지속된다. 이 경우, `Player`인스턴스의 속성에 겹치는 액세스를 할 수 있는 `restoreHealth()`내부에 다른 코드가 없다. 아래의 `shareHealth(with:)`메소드는 또다른 `Player` 인스턴스를 인아웃 매개변수로써 사용해 중복 액세스 가능성을 만든다.<br>

```swift
extension Player {
	mutating func shareHealth(with teammate: inout Player) {
		balance(&teammate.health, &health)
  }
}

var oscar = Player(name: "Oscar", health: 10, energy: 10)
var maria = Player(name: "Maria", health: 5, energy: 10)
oscar.shareHealth(with: &maria)  // 가능
```

위의 예제에서, Oscar's player가 Maria's player와 health를 공유하기 위해 `shareHealth(with:)`를 호출하는 것은 충돌을 일으키지 않는다. `oscar`에 대한 쓰기 액세스가 메소드 호출 동안 일어난다. 왜냐하면 `oscar`는 변화 메소드에서 `self`의 값이고 `maria`가 인아웃 매개변수로 써 전달되기 때문에 같은 기간에 `maira`에 대한 쓰기 액세스도 발생한다. 심지어 두 쓰기 액세스는 시간은 겹치지만 충돌은 일어나지 않는다.

![](https://docs.swift.org/swift-book/_images/memory_share_health_maria_2x.png)

그러나, `oscar`를 `shareHealth(with:)`의 인수로써 전달하면 충돌이 일어난다.

```swift
oscar.shareHealth(with: &oscar)	// 에러: oscar 접근 충돌
```

변화 메소드는 메소드 기간 동안 `self`에 대한 쓰기 액세스가 필요하고 인아웃 매개변수는 같은 기간 `teammate`에 대한 스기 액세스가 필요하다. 메소드 안에서, `self`와 `teammate`는 아래 그림에서 보여주는 것 처럼 메모리의 같은 구역을 가리킨다. 두 쓰기 액세스는 같은 메모리를 가리키고 겹치고 충돌을 만든다.

![](https://docs.swift.org/swift-book/_images/memory_share_health_oscar_2x.png)





## Conflicting Access to Properties (프로퍼티에 대한 충돌 액세스)

구조체, 튜플, 열거형과 같은 타입은 구조체의 속성, 튜플의 요소와 같은 개별 구성 값으로 구성된다. 왜냐하면 값 타입이기 때문에 값의 일부를 변경하면 전체 값이 변경된다. 즉, 속성 중 하나에 대해 읽거나 쓰는 액세스는 전체 값에 대해 읽거나 쓰는 액세스가 필요하다. 예를 들어, 튜플 요소에 대한 쓰기 액세스가 겹치면 충돌이 발생한다.<br>

```swift
var playerInformation = (health: 10, energy: 20)
balance(&playerInformation.health, &playerInformation.energy)
// 에러: playerInformation의 속성을 액세스할 때 충돌
```

위의 예제에서, 튜플의 요소에 대한 `balance(_:_:)` 호출은 충돌을 일으킨다. 왜냐하면 `playerInformation`에 대해 쓰기 액세스가 겹치기 때문이다. `playerInformation.health`와 `playerInformation.enerygy` 둘 다 인아웃 매개변수로써 전달된다. 그 뜻은 `balance(_:_:)`는 함수 호출 기간동안 그들에 대한 쓰기 액세스가 필요하다는 것이다. 두 경우에서, 튜플 요소에 대한 쓰기 액세스는 전체 튜플에 대해 쓰기 액세스가 필요하다. 이 의미는, 지속시간이 겹치는 `playerInformation`에 대한 두 쓰기 액세스가 겹치고 충돌의 원인이 된다.<br>
아래 예제는 전역 변수가 저장된 구조체의 속성에 대해 쓰기 액세스가 겹칠 때 같은 에러가 나타나는 것을 보여준다. <br>

```swift
var holly = Player(name: "Holly", health: 10, energy: 10)
balance(&holly.health, &holly.energy)		// 에러
```

대부분의 구조체의 속성에 대한 액세스는 안전하게 겹칠 수 있다. 예를 들어, 위의 예제에서 `holly`변수가 전역변수 대신 지역변수로 바꾸면 컴파일러는 구조체의 저장된 속성에 대한 겹치는 액세스가 안전하다고 증명할 수 있다.<br>

```swift
func someFunction(){
  var oscar = Player(name: "Oscar", health: 10, energy: 10)
  balance(&oscar.health, &oscar.energy)	// 가능
}
```

위의 예제에서 Oscar's health와 energy는 `balance(_:_:)`에 두 인아웃 매개변수로써 전달된다. 컴파일러는 두 저장된 속성이 어떠한 방법으로도 상호작용하지 않기 때문에 메모리 안정성이 보존된다고 증명할 수 있다. <br>
구조체의 속성에 대한 겹치는 액세스에 대한 제한은 메모리 안정성 보존을 위해 항상 필요한 것은 아니다. 메모리 안정성은 보증을 요구하지만 독점 액세스는 메모리 안전보다 더 엄격한 요구사항이다. 즉, 일부 코드는 메모리에 대한 독점 액세스를 위반하더라도 메모리 안전을 유지한다. Swift는 만약 컴파일러가 메모리에 대한 비독점 액세스가 여전히 안전함을 증멸할 수 있다면 이 메모리 안전 코드를 허용한다. 특히, 구조체 속성에 대한 겹치는 접근이 주어진 상태를 따른다면 안전하다는 것을 증명할 수 있다.

* 계산된 속성이나 클래스 속성이 아닌 인스턴스의 저장된 속성에만 액세스한다.
* 구조체는 전역 변수가 아니고 지역 변수다.
* 구조체는 어떠한 클로저에 의해 캡처되지 않거나 nonescaping closures에 의해서만 캡처된다.

만약 컴파잉얼가 액세스가 안전하다는 것을 증명하지 못하면 액세스를 허용하지 않는다.

