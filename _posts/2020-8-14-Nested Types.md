---
title: "Nested Typea"
date: 2020-8-14
categories: Swift5

---



# Nested Types (중첩 타입)

열거형은 특정 구조체나 클래스의 기능을 처리하기 위해 많이 사용된다. 이와 비슷하게 특정 문맥에서 조금 더 복잡한 타입을 위해 사용할 수 있는 유틸리티 클래스나 구조체를 정의할 수 있다. Swift에서 이 기능을 위해 중첩타입을 지원한다. 열거형, 클래스, 구조체를 그 타입 안에서 다시 정의할 수 있다.



## Nested Types in Action (중첩 타입의 사용)

아래 코드는 블랙잭 게임에서 사용되는 카드를 모델링한 `BlackjackCard`구조체를 정의한 예시이다. `BlackjackCard`구조체는 `Suit`과 `Rank`라고 부르는 두 개의 중첩 열거 타입을 포함한다.<br>
블랙잭에서 Ace 카드는 1이나 11로 사용될 수 있다. 이 기능은 ``Rank`열거형의 중첩 타입인 구조체의 `Values`라는 프로퍼티로 표현되어 있다.

```swift
struct BlackjackCard {
  // 중첩 Suit 열거형
  enum Suit: Character {
    case spades = ♠", hearts = "♡", diamonds = "♢", clubs = "♣"
  }
  
  // 중첩 Rank 열거형
  enum Rank: Int {
    case two = 2, three, four, five, six, seven, eight, nine, ten
    case jack, queen, king, ace
    struct Values {
      let first: Int, secont: Int?
    }
    var values: Values {
      switch self {
        case .ace:
        	return Values(first: 1, second: 11)
        case .jack, .queen, .king:
        	return Values(first: 10, second: nil)
        default:
        	return Values(first: self.rawValue, second: nil)
      }
    }
  }
  
  // 프로퍼티와 메소드
  let rank: Rank, suit: Suit
  var description: String {
    var output = "suit is \(suit.rawValue),"
    output += " value is \(rank.values.first)"
    if let second = rank.values.second {
      output += " or \(second)"
    }
    return output
  }
}
```

Suit 열거 값은 카드에서 사용하는 4가지 모양을 표현한다. raw값은 그 모양의 기호를 나타낸다. Rank 열거값은 카드에서 사용 가능한 13가지 카드 등급을 표현한다. raw값은 그 등급의 값을 나타낸다. (raw Int값은 Jack, Queen, King, Ace 카드에서는 사용되지 않는다.)<br>

위에서 언급했던 것 같이, Rank값은 그 값 자체의 중첩 구조체인 Values라는 값을 정의한다. 이 구조는 대부분의 카드가 하나의 값만 갖지만 Ace카드는 두개를 갖는데 이것을 캡슐화 해준다. Values 구조체는 이것을 표현하기 위해

* first : Int 형
* second : Int? (옵셔널 Int)

를 정의한다.<br>

Rank는 또 계산된 프로퍼티를 정의해 사용한다. `Ace`, `Jack`, `Queen`, `King` 등 first와 second 값을 모두 갖는 카드는 그것에 맞게 값을 반환하고 나머지 보통 숫자에 대해서 first값만 존재하고 second값은 nil 값을 반환한다. <br>

`BlackjackCard` 는 `rank`와 `suit` 두개의 프로퍼티를 소유하고 `Description`이라 부르는 계산된 프로퍼티도 정의한다. `BlackjacrCard`는 커스텀 초기자가 없는 구조체이기 때문에 암시적인 멤버쪽 초기자를 갖는다. 그래서 아래 코드와 같이 `rank`와 `suit`를 인자로 받아 `BlackjackCard`를 초기화 할 수 있다.

```swift
let theAceOfSpades = BlackjacrCard(rank: .ace, suit: .spades)
print("theAceOfSpaces: \(theAceOfSpades.description)")
// "theAceOfSpades: suit is ♠, value is 1 or 11" 출력
```

`.ace`,`.spades`가 `BlackjackCard`안에 중첩 타입으로 선언돼 있기 때문에 타입 추론이 가능해 `BlackjackCard`초기화시 타입형의 전체 명시 없이 `.ace`, `.spades`로 사용할 수 있다.



## Reffering to Nested Types (중첩 타입 언급)

중첩 타입을 선언 밖에서 사용하려면 선언된 곳의 시작부터 끝까지 적어줘야 한다.

```swift
let heartsSymbol = BlackjackCard.Suit.hearts.rawValue
// heartsSymbol는 "♡" 이다. 
```

