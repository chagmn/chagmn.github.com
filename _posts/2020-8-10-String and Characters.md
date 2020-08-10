---
title: "String and Characters"
date: 2020-8-10
categories: Swift5
---

# String and Characters (문자열과 문자)

문자열은 `"Hello, world"`나 `"albatross"`와 같이 문자들의 집합이다. Swift 문자열들은 `String`자료형을 대표한다. `String`의 내용들은 `Character`값의 컬렉션을 포함해서 다양한 방법으로 접근 할 수 있다. <br>

Swift의 `String`과 `Character` 자료형은 코드에서 텍스트를 가지고 작업할 때 빠르고, 유니코드 호환 방식을 제공한다. 문자열 생성 및 조작을 위한 구문은 가볍고 읽기 쉬우며 C언어와 유사한 문자열 리터럴 구문을 사용한다. 문자열 연결은 두개의 문자열을 `+`연산자로 결합해서 간단하고 Swift에서 다른 값과 같이 문자열 가변성은 변수와 상수 사이에서 결정된다. 또한, 문자열 보간이라 하는 프로세스에서 문자열을 사용해 상수, 변수, 리터럴, 식을 긴 문자열에 삽입 할 수 있다. 이것은 표현하고, 저장하고, 출력하기 위한 커스텀 문자열 값을 생성하는 것을 쉽게 해준다. <br>

이러한 간단한 구문에도 불구하고, Swift의 `String` 자료형은 빠르고, 현대적 문자열 구현이다. 모든 문자열은 인코딩-독립된 유니코드 문자들로 구성되었고 다양한 유니코드 표현으로 이러한 문자들을 접근하는 것을 지원한다.

## String Literals (문자열 리터럴)
코드 내에 사전 정의 된 문자열 값을 문자열 리터럴로 포함 할 수 있다. 문자열 리터럴은 큰따옴표(`"`)로 묶인 일련의 문자다.<br>
상수나 변수에 초기값으로써 문자열 리터럴을 사용한다:

`let someString = "Some string literal value"`

Swift는 문자열 리터럴 값으로 초기화되기 때문에 `someString` 상수에 대해 `String`유형을 유추한다.

### Multiline String Literals (다중 문자열 리터럴)
만약 여러 줄에 걸친 문자열이 필요하다면 3개의 큰따옴표로 묶인 일련의 문자인 여러 줄 문자열 리터럴을 사용한다.

```
let quotation = """
The White Rabbit put on his spectacles.  "Where shall I begin,
please your Majesty?" he asked.

"Begin at the beginning," the King said gravely, "and go on
till you come to the end; then stop."
"""
```

다중 문자열 리터럴은 열고 닫는 따옴표 사이의 모든 줄들을 포함한다. 문자열은 여는 따옴표(`"""`) 후에 나오는 첫번째 줄에서 시작해서 닫는 따옴표 전까지의 줄에서 끝난다. 이는, 아래의 문자열은 줄 바꿈으로 시작하거나 끝나지 않는다.

```
let singleLineString = "These are the same."
let multilineString = """
These are the same.
"""
```

다중 문자열 리터럴안의 줄바꿈이 포함된 소스코드에서 줄 바꿈 또한 문자열의 값으로 나타날 수 있다. 줄 바꿈을 사용해 소스 코드를 읽기 쉽게 만들고 싶지만 줄 바꿈이 문자열 값의 일부가 되지 않도록 하려면 해당 줄 긑에 백 슬래시(`\`)를 작성해야 한다.

```
let softWrappedQuotation = """
The White Rabbit put on his spectacles.  "Where shall I begin, \
please your Majesty?" he asked.

"Begin at the beginning," the King said gravely, "and go on \
till you come to the end; then stop."
"""
```

줄 바꿈으로 시작하거나 끝나는 다중 문자열 리터럴을 만드려면, 첫 줄이나 마지막 줄을 공백으로 작성해야 한다.

```
let lineBreaks = """

This string starts with a line break.
It also ends with a line break.

"""
```

주변 코드와 일치하게 하기 위해 다중 문자열은 들여 쓰기 할 수 있다. 닫는 따옴표(`"""`) 앞의 내용 외에 줄 시작 부분에 공백을 쓰면 해당 공백이 포함된다.
![](https://docs.swift.org/swift-book/_images/multilineStringWhitespace_2x.png)

위의 예제에서 전체 다중 문자열이 들여쓰기 되었지만, 문자열에서 첫번째와 마지막 줄은 어떠한 공백없이 시작되었다. 가운데 줄은 닫는 따옴표보다 더 들여쓰기가 되어있어서 여분의 공백 4개와 함께 시작한다.

### Special Characters in String Literals (문자열 리터럴에서 특별한 문자들)
문자열 리터럴은 특별한 문다들을 포함 할 수 있다.

* 이스케이프 된 특수문자 `\0`(널 문자), `\\`(백슬래시), `\t`(수평 탭), `\n`(줄 바꿈), `\r`(캐리지 리턴), `\"`(큰 따옴표), `\'`(작은 따옴표)
* 임의의 유니코드 스칼라 값은 `\u{n}`로 쓰이고, `n` 자리는 16진수가 들어간다.

아래 코드는 이러한 특별 문자들의 4개 예시를 보여준다. `wiseWords` 상수는 2개의 이스케이프된 큰 따옴표가 포함되어 있다. `dollarSign`, `blackHeart`, `sparklingHear` 상수는 유니코드 스칼라 형식을 보여준다.

```
let wiseWords = "\"Imagination is more important than knowledge\" - Einstein"
// "Imagination is more important than knowledge" - Einstein
let dollarSign = "\u{24}"        // $,  Unicode scalar U+0024
let blackHeart = "\u{2665}"      // ♥,  Unicode scalar U+2665
let sparklingHeart = "\u{1F496}" // 💖, Unicode scalar U+1F496
``` 

다중 문자열 리터럴은 3개의 큰 따옴표(`"`)를 하나 대신에 사용하기 때문에 이스케이프 하지 않고 여러 줄 문자열 리터럴 내의 큰따옴표(`"`)를 포함 할 수 있다. 여러 줄 문자열에 문자(`"""`)를 포함하려면 적어도 하나의 따옴표를 이스케이프 해야한다. 예를 들면,

```
let threeDoubleQuotationMarks = """
Escaping the first quotation mark \"""
Escaping all three quotation marks \"\"\"
"""
```

### Extended String Delimiters (확장된 문자열 구분 기호)
확장 구분 기호 내에 문자열 리터럴을 배치해 효과를 호출하지 않고 문자열에 특수문자를 포함 할 수 있다. 문자열을 따옴표(`"`)안에 넣고 숫자 기호(`#`)로 묶을 수 있다. 예를 들어, 문자열 리터럴 `#"Line 1\nLine 2#`를 출력하면 두 줄에 걸쳐 출력되는 대신 줄 바꿈 이스케이프 문자(`\n`)가 출력된다. <br>

문자열 리터널에서 문자의 특수효과가 필요하다면, 이스케이프 문자(`\`)다음에 오는 문자열 안에 숫자 기호 수를 일치 시킨다. 예를 들어, 문자열이 `#"Line 1\\nLine 2"#`이고, 줄 바꿈을 하고싶으면 대신에 `#"Line 1\#nLinew 2"#`를 사용 할 수 있다. 유사하게, `###"Line1\###nLine2"###` 또한 줄을 바꾼다. <br>

확장 구분 기호를 사용해 만들어진 문자열 리터럴은 다중 문자열 리터럴이 될 수 있다. 확장 구분 기호를 사용해 여러 줄 문자열에 텍스트(`"""`)를 포함해 리터럴을 종료하는 기본 동작을 재정의 할 수 있다.

```
let threeMoreDoubleQuotationMarks = #"""
Here are three more double quotes: """
"""#
```

## Initializing an Empty String (빈 문자열 초기화)
긴 문자열을 만들기 위한 시작점으로 빈 문자열 값을 만들려면 빈 문자열 리터럴을 변수에 할당하거나 초기화 구문으로 새 문자열 인스턴스를 초기화한다.

```
var emptyString = ""  // 빈 문자열 리터럴
var anotherEmptyString = String()  //초기화 구문
// 두 문자열 모두 비었고 서로 동등
```

`String`값이 비었는지 찾기 위해서는 `isEmpty` 프로퍼티를 사용하면 된다.

```
if emptyString.isEmpty{
	print("Nothing to see here")
}
// Nothing to see here 출력
```

## String Mutablity (문자열 가변성)
특정 `String`을 변수(이 경우 수정할 수 있다.)  상수(이 경우 수정할 수 없다.)에 할당하여 특정 문자열을 수정(또는 변형) 할 수 있는지 여부를 나타낸다.

```
var variableString = "Horse"
variableString += " and carriage"
// variableString 는 Horse and carriage

let constantString = "Highlander"
constantString += " and another Highlander"
// 컴파일 에러를 일으킨다. 상수는 수정될 수 없기 때문
```

## Strings Are Value Types (문자열은 값 타입)
Swift의 `String`타입은 값 타입이다. 새로운 `String`값을 만드려고 하면, `String`값은 함수나 메소드에 전달되거나, 상수나 변수에 할당될 때 복사된다. 각각의 경우, 존재하는`String`값의 새로운 복사본이 생성되고, 원본이 아니라 새 복사본은 전달되거나 할당된다. <br>

Swift의 기본 복사 문자열 동작은 함수나 메소드가 `String`값을 전달할 때 출처에 관계없이 정확한 문자열 값을 소유하고 있음을 분명히 한다. 전달 된 문자열은 직접 수정하지 않는 한 수정되지 않는다.<Br>

배후에서, Swift의 컴파일러는 문자열 사용성을 최적화하여 절대적으로 필요한 경우에만 실제로 복사가 이루어 지도록 한다. 이것은 문자열을 값 타입으로 작업할 때 항상 최고의 퍼포먼스를 갖을 수 있다는 것을 의미한다.

## Working with Characters (문자 작업)
`for-in`루프를 사용하면 문자열을 반복해 `String`의 각각의 `Characters`값에 접근 할 수 있다. 

```
for character in "Dog!🐶" {
    print(character)
}
// D
// o
// g
// !
// 🐶
```

또는, `Character`타입 추론을 제공함으로써 단독 문자 문자열 리터럴로 부터 독립형 `Character`상수나 변수를 만들 수 있다.<br>

`let exclamationMark: Character = "!"`

`String`값은 초기화에 대한 인수로 `Character`값의 배열을 전달하여 생성 할 수 있다.

```
let catCharacters: [Character] = ["C", "a", "t", "!", "🐱"]
let catString = String(catCharacters)
print(catString)
// Cat!🐱 출력
```

## Concatenation Strings and Characters (문자열과 문자 연결)
문자열 값은 더하기 연산자(`+`)를 사용해 엽결하거나 합쳐 새로운 문자열 값을 만들 수 있다.

```
let string1 = "hello"
let string2 = " there"
var welcome = string1 + string2
// welecom 은 hello threre
```

또한, 존재하는 문자열 변수에 더하기 할당 연산자 (`+=`)를 사용해 문자열 값을 추가할 수 있다.

```
var instruction = "look over"
instruction += string2
// instruction 은 look over there
```

문자열 타입의 `append()`메소드를 사용해 문자열 값에 문자 값을 추가할 수 있다.

```
let exclamationMark: Character = "!"
welcome.append(exclamationMark)
// welcome은 hello there!
```

다중 문자열 리터럴을 사용해 긴 문자열의 줄을 구성하면, 문자열의 마지막 줄을 포함한 모든 줄의 문자열에 끝이 줄 바꿈이길 원한다. 예를 들어,

```
let badStart = """
one
two
"""
let end = """
three
"""
print(badStart + end)
// one
// twothree

let goodStart = """
one
two

"""
print(goodStart + end)
// one
// two
// three
``` 

위의 코드에서, `badStart`를 `end`와 연결하면 원하지 않는 두 줄의 문자열이 만들어 진다. `badStart`의 마지막 줄이 줄 바꿈으로 끝나지 않기 때문에 `end`의 첫번째 줄과 결합된다. 반면 `goodStart`는 줄 바꿈으로 끝나 예상한 것 처럼 `end`와 결합할 때 세 줄의 결과가 나타난다.

## String Interpolation (문자열 보간)
문자열 보간은 새로운 문자열 값을 문자열 리터럴 안에 그들의 값이 포함되어 섞인 상수, 변수, 리터럴, 식으로 부터 만드는 방법이다. 한 줄이나 여러 줄 문자열 리터럴에서 문자열 보간법을 사용할 수 있다. 각 문자열 리터럴 안에 넣는 항목은 백 슬래시(`\`)로 시작하는 한 쌍의 괄호로 둘러 싸여 있다.

```
let multiplier = 3
let message = "\(multiplier) times 2.5 is \(Double(multiplier) * 2.5)"
// message는 3 times 2.5 is 7.5 
```

위의 예제에서, `multiplier`값은 `\(multiplier)`라는 문자열 리터럴 안에 삽입된다. 실제 문자열을 생성하기 위해 문자열 보간을 평가할 때, `multiplier`의 실제 값으로 대체된다. <br>

`multiplier`의 값은 나중에 문자열에서 더 큰 표현의 한 부분이기도 하다. 표현식은 `Double(multiplier) * 2.5` 값을 계산하고 결과인`7.5`를 문자열 안에 삽입한다. 이 경우, 표현식이 문자열 리터럴 내에 포함되면 `\(Double(multiplier) * 2.5)` 로 작성된다.<br>

확장 된 문자열 구분 기호를 사용해 문자열 보간으로 처리 될 문자를 포함하지않는 문자열을 만들 수 있다.

`print(#"Write an interpolated string in Swift using \(multiplier)."#)
// Write an interpolated string in Swift using \(multiplier). 가 출력`

확장 구분 기호가 사용된 문자열 내에 문자열 보간을 사용하려면 백 슬래시 이후의 숫자 기호 수를 문자열의 시작과 끝에 있는 숫자 기호 수와 일치시킨다.

## Unicode (유니코드)
유니코드는 다른 쓰기 시스템에서 텍스트를 인코딩하고, 표현하고, 처리하기 위한 국제 표준이다. 표준 형식에서 어떠한 언어로 부터의 거의 어떠한 문자를 표현하는 것을 가능하게 하고 텍스트 파일이나 웹페이지같이 외부 소스로부터의 문자들을 읽고 쓰게 한다. Swift의 `String`과 `Character` 타입은 이 섹션에서 말하는 것처럼 완전히 유니코드 호환한다.<Br>

### Unicode Scala Values (유니코드 스칼라 값)
Swift의 원래 `String`형은 유니코드 스칼라 값으로 부터 만들어졌다. 유니코드 스칼라 값은  `LATIN SMALL LETTER A ("a")`경우 `U+0061`,  `FRONT-FACING BABY CHICK ("🐥")`경우 `U+1F425` 같이 문자나 수정자의 고유한 21비트 숫자이다.<br>

모든 21비트 유니코드 스칼라 값이 문자에 할당되는 것은 아니다. 일부 스칼라는 나중에 할당하거나 UTF-16 인코딩에 사용하기 위해 예약되어 있다. 문자에 할당된 스칼라 값은 위의 예시에서`LATIN SMALL LETTER A`와 `FRONT_FACING BABY CHICK`과 같이 이름을 갖고 있다.

### Extended Grapheme  (확장된 문자소 클러스터)
모든 Swift의 문자형 인스턴스는 하나의 확장된 문자소 클러스터를 표현한다. 확장된 문자소 클러스터는 하나 이상의 유니코드 스칼라 스퀀스로 (결합 된 경우) 사람이 읽을 수 있는 단일 문자를 생성한다. <br>

예시가 있다. 문자 `é`는 하나의 유니코드 스칼라 `é`(`LATIN SMALL LETTER E WITH ACUTE, or U+00E9`)를 표현한다. 그러나, 동일 문자는 표준 문자 `e` (`LATIN SMALL LETTER E, or U+0065`) 다음에 `COMBINING ACUTE ACCENT` 스칼라 (`U+0301`)가 오는 한 쌍의 스칼라로 표현 될 수 있다. `COMBINING ACUTE ACCENT` 스칼라는 앞에 있는 스칼라에 그래픽으로 적용되어 유니코드 인식 텍스트 렌더링 시스템에서 렌더링 될 때 `e`를 `é`로 바꾼다. <br>

두 경우에서, 문자`é`는 확장된 문자소 클러스터를 표현하는 단독 Swift문자 값으로 표현된다. 첫번째 경우, 클러스터는 하나의 스칼라를 포함하고 두번째 경우, 클러스터는 두개 스칼라를 포함한다.

```
let eAcute: Character = "\u{E9}"   // é
let combinedEAcute: Character = "\u{65}\u{301}"    // e 다음에 `
// eAcute는 é, combinedEAcute는 é
```

확장된 문자소 클러스터는 복잡한 스크립트 문자를 하나의 문자 값으로 표현하는 유연한 방법이다. 예를 들어, 한글의 음절은 미리 구성되거나 분해된 시퀀스로 표현할 수 있다. 이 두 표현은 모두 Swift에서 단일 문자 값으로 표현된다.

```
let precomposed: Character = "\u{D55C}"     // 한
let decomposed: Charcter = "\u{1112}\u{1161}\u{11AB}"   // ㅎ, ㅏ, ㄴ
precomposed는 한, decomposed는 한
```

확장된 문자소 클러스터를 사용하면 둘러쌓인 마크 스칼라(`COMBINING ENCLOSING CIRCLE, or U+20DD`)가 다른 유니코드 스칼라를 단일 문자 값의 일부로 묶을 수 있다.

```
let enclosedEAcute: Character = "\u{E9}\u{20DD}"
// enclosedEAcute는 é⃝
```

지역 구분 기호를 위한 유니코드 스칼라를 쌍으로 결합해 단일 문자 값을 만들 수 있다. 예를 들면, `REGIONAL INDICATOR SYMBOL LETTER U (U+1F1FA)`과 `REGIONAL INDICATOR SYMBOL LETTER S (U+1F1F8)`의 결합이 있다.

```
let regionalIndicatorForUS: Character = "\u{1F1FA}\u{1F1F8}"
// regionalIndicatorForUS는 🇺🇸
```

## Counting Characters (문자 수 세기)
문자열에서 문자 값의 수를 검색하기 위해서 문자열의 `count`프로퍼티를 사용해야 한다.

```
let unusualMenagerie = "Koala 🐨, Snail 🐌, Penguin 🐧, Dromedary 🐪"
print("unusualMenagerie has \(unusualMenagerie.count) characters")
// unusualMenagerie has 40 characters 출력
```

Swift의 문자 값에 대해 확장된 문자소 클러스터를 사용한다는 것은 문자열 연결과 수정은 항상 문자열의 문자 수의 영향을 주지 않는다는 것을 의미한다. <br>

예를 들어, 만약 새로운 문자열은 `cafe`라는 4개의 문자로 초기화 하고 문자열 뒤에 `COMBINING ACUTE ACCENT (U+0301)`를 추가하면 결과 문자열은 여전히 4개의 문자 수가 나오는데 4번째 문자가 `e`가 아니라 `é `이다.

```
var word = "cafe"
print("the number of characters in \(word) is \(word.count)")
// the number of characters in cafe is 4 출력

word += "\u{301}"    // ACUTE ACCENT, U+0301 결합

print("the number of characters in \(word) is \(word.count)")
// the number of characters in café is 4 출력
```

## Accesssing and Modifying a String
메소드와 프로퍼티 또는 서브 스크립트 구문을 사용해 문자열을 수정하거나 접근할 수 있다.

### String Indices (문자열 인덱스)
각 문자열값에는 연관된 인덱스 유형인 문자열이 있다. 인덱스는 문자열에서 각 문자의 위치에 대응한다.<br>

위에 언급한 것 처럼, 문자마다 저장하기 위해 다른 양의 메모리가 필요하다. 그래서 문자가 특정 위치에 있는 문자를 확인하려면 위해서 문자열의 시작 또는 끝에서 각 유니코드 스칼라를 반복해야만 한다. 이러한 이유로, Swift 문자열은 정수 값으로 인덱스를 생성 할 수 없다. <br>

`startIndex`프로퍼티를 사용해 문자열의 첫번째 위치에 접근 할 수 있다. `endIndex`프로퍼티를 사용하면 문자열의 마지막 위치 다음에 접근 할 수 있다. 결과적으로 `endIndex`프로퍼티는 문자열 서브 스크립트에 대한 유효한 인수가 아니다. 문자열이 비었다면, `startIndex`와 `endIndex`는 같다.<br>

`String`의 `index(before :)`및 `index(after :)`메서드를 사용해 주어진 인덱스 앞뒤의 인덱스에 접근한다. 주어진 인덱스에서 멀리 떨어진 인덱스에 접근하려면 이러한 메소드를 여러번 호출하는 대신에 `index(_:offsetBy:)`메서드를 사용할 수 있다.<br>

특정 문자열 인덱스의 문자에 접근하기 위해 서브 스크립트 구문을 사용할 수 있다.

```
let greeting = "Guten Tag!"
greeting[greeting.startIndex]
// G
greeting[greeting.index(before: greeting.endIndex)]
// !
greeting[greeting.index(after: greeting.startIndex)]
// u
let index = greeting.index(greeting.startIndex, offsetBy: 7)
greeting[index]
// a
```

문자열 범위 밖의 인덱스에 접근하려고 하거나 문자열 범위의 밖의 인덱스의 문자에 접근하려고 하면 런타임 에러가 작동한다.

```
greeting[greeting.endIndex]  // 에러
greeting.index(after: greeting.endIndex)  // 에러
```

`indices`프로퍼티를 사용해 문자열의 각각 문자의 모든 인덱스에 접근할 수 있다.

```
for index in greeting.indices {
    print("\(greeting[index]) ", terminator: "")
}
// G u t e n   T a g !  출력
```

### Inserting and Removing (삽입 삭제)
문자열의 특정 인덱스에 문자를 삽입하려면, `insert(_:at:)`메소드를 사용하고 또 문자열의 특정 인덱스에 또 다른 문자열의 내용을 삽입 할 땐 `insert(contentsOf:at:)`메소드를 사용한다.

```
var welcome = "hello"
welcome.insert("!", at: welcome.endIndex)
// welcome은 hello!

welcome.insert(contentsOf: " there", at: welcome.index(before: welcome.endIndex))
// welcome은 hello there!
```

문자열의 특정 인덱스에 문자를 삭제하려면 `remove(at:)`메소드를 사용하고 특정 범위의 부분문자열을 제거하려면 `removeSubrange(_:)`메소드를 사용한다.

```
welcome.remove(at: welcome.index(before: welcome.endIndex))
// welcome는 hello there

let range = welcome.index(welcome.endIndex, offsetBy: -6)..<welcome.endIndex
welcome.removeSubrange(range)
// welcome는 hello
```

## Substring (부분문자열)
예를 들어 서브 스크립트나 `prefix(_:)`와 같은 메소드를 사용해 문자열로부터 부분문자열을 얻을때, 결과는 문자열이 아니라 부분문자열의 인스턴스다. Swift에서 부분문자열은 문자열과 대부분 같은 메서드를 갖고 있다. 이것은 부분문자열을 문자열과 동일한 방법으로 작업 할 수 있다는 뜻이다. 그러나, 문자열과는 다르게 문자열에 대한 작업을 수행하는 동안 짧은 시간 동안만 부분문자열을 사용한다. 더 긴 시간동안 결과를 저장할 준비가 되어있을 때, 부분문자열을 문자열 인스턴스로 변경할 수 있다. 예를 들어:

```
let greeting = "Hello, world!"
let index = greeting.firstIndex(of: ",") ?? greeting.endIndex
let beginning = greeting[..<index]
// beginning는 Hello

// 더 긴 시간 저장하기 위해 결과를 문자열로 변환
let newString = String(beginning)
```

문자열과 같이, 각 부분문자열은 부분문자열을 구성하는 문자가 저장되는 메모리 영역이 있다. 문자열과 부분문자열 사이의 차이점은 성능 최적화를 위해 부분문자열은 원래 문자열을 저장하기 위해 사용한 메모리의 부분이나 또 다른 부분문자열을 저장하기 위해 사용된 메모리의 부분을 재사용 할 수 있다는 것이다. (문자열은 유사한 최적화 방법을 갖고있지만, 만약 두개의 문자열이 메모리를 공유한다면 같은 것이다.) 이 성능 최적화는 문자열이나 부분문자열을 수정하기 전까지 메모리 복사 성능 비용을 지불할 필요가 없다는 것을 의미한다. 위에 언급한 것 처럼, 부분문자열은 장기 저장에 적합하지 않다. 원래 문자열의 저장소를 재사용하기 때문에 부분문자열이 사용되는 한 전체 원본 문자열을 메모리에 보관해야 한다. <br>

위의 예시에서, `greeting`은 문자열이고 문자열을 구성하는 문자가 저장되는 메모리 영역이 있음을 의미한다. 왜냐하면 `beginning`은 `greeing`의 부분문자열이고 `greeing`이 사용한 메모리를 재사용한다. 반면 부분문자열로부터 만들어 졌을때 스스로의 저장소를 갖고있어서 `newString`은 문자열이다. 아래 그림은 이들의 관계를 보여준다:

![](https://docs.swift.org/swift-book/_images/stringSubstring_2x.png)

## Comparing Strings (문자열 비교)
Swift는 문자열 값을 비교하는데 3가지 방법을 제공한다: string and character equality, prefix equality, suffix equality.

### String and Character Equality
String and character equality는 같음 연산자(`==`)와 다름 연산자로 검사한다.

```
let quotation = "We're a lot alike, you and I."
let sameQuotation = "We're a lot alike, you and I."
if quotation == sameQuotation {
    print("These two strings are considered equal")
}
// These two strings are considered equal 출력
```

두 문자열 값(또는 두 문자 값)은 확장된 문자소 클러스터가 정규적으로 동일하면 같다고 판단된다. 확장된 문자소 클러스터는 언어적 의미와 외관이 같지만 다른 유니코드 스칼라로 구성되어 있어도 정규적으로 동일하다. <br>

예를 들어, ` LATIN SMALL LETTER E WITH ACUTE (U+00E9)`는 `LATIN SMALL LETTER E (U+0065)` 다음에 `COMBINING ACUTE ACCENT (U+0301)`와 정규적으로 동일하다. 두 확장된 문자소 클러스터는 문자 `é `를 표현하는 유효한 방법이고 그래서 정규적으로 동일하다 판단된다.

```
// "Voulez-vous un café?"<- LATIN SMALL LETTER E WITH ACUTE 사용
let eAcuteQuestion = "Voulez-vous un caf\u{E9}?"

// "Voulez-vous un café?"<- LATIN SMALL LETTER E and COMBINING ACUTE ACCENT 를 사용
let combinedEAcuteQuestion = "Voulez-vous un caf\u{65}\u{301}?"

if eAcuteQuestion == combinedEAcuteQuestion {
    print("These two strings are considered equal")
}
// These two strings are considered equal 출력
```

반대로, 영어에서 사용되는 `LATIN CAPITAL LETTER A (U+0041, or "A")`는 러시아어에서 사용되는 `CYRILLIC CAPITAL LETTER A (U+0410, or "А")`와 동일하지 않다. 문자들은 비슷하게 보이지만 같은 언어적 의미를 갖고 있지 않다.

```
let latinCapitalLetterA: Character = "\u{41}"

let cyrillicCapitalLetterA: Character = "\u{0410}"

if latinCapitalLetterA != cyrillicCapitalLetterA {
    print("These two characters are not equivalent.")
}
// These two characters are not equivalent. 출력
```

### Prefix and Suffix Equality
문자열에 특정 문자열 접두사나 접미사가 있는지 확인하려면 `String`의 `hasPrefix(_:)`나 `hasSuffix(_:)`메서드를 호출한다. 둘 다 `String`타입의 단일 인수를 사용하고 `Boolean`값을 반환한다. <br>

아래 예제는 셰익스피어의 로미오와 줄리엣의 처음 두 막에서 장면 위치를 나타내는 문자열 배열을 고려한다.

```
let romeoAndJuliet = [
    "Act 1 Scene 1: Verona, A public place",
    "Act 1 Scene 2: Capulet's mansion",
    "Act 1 Scene 3: A room in Capulet's mansion",
    "Act 1 Scene 4: A street outside Capulet's mansion",
    "Act 1 Scene 5: The Great Hall in Capulet's mansion",
    "Act 2 Scene 1: Outside Capulet's mansion",
    "Act 2 Scene 2: Capulet's orchard",
    "Act 2 Scene 3: Outside Friar Lawrence's cell",
    "Act 2 Scene 4: A street in Verona",
    "Act 2 Scene 5: Capulet's mansion",
    "Act 2 Scene 6: Friar Lawrence's cell"
]
```

`hasPrefix(_:)`메서드를 `romeoAndJuliet`배열과 함께 사용해 연극의 Act 1에서의 장면 수를 셀 수 있다.

```
var act1SceneCount = 0
for scene in romeoAndJuliet {
    if scene.hasPrefix("Act 1 ") {
        act1SceneCount += 1
    }
}
print("There are \(act1SceneCount) scenes in Act 1")
// There are 5 scenes in Act 1 출력
```

유사하게, `hasSuffix(_:)`메서드를 사용해 Capulet's mansion과 Friar Lawrence's cell 내부 또는 주변에서 발생하는 장면의 수를 계산한다:

```
var mansionCount = 0
var cellCount = 0
for scene in romeoAndJuliet {
    if scene.hasSuffix("Capulet's mansion") {
        mansionCount += 1
    } else if scene.hasSuffix("Friar Lawrence's cell") {
        cellCount += 1
    }
}
print("\(mansionCount) mansion scenes; \(cellCount) cell scenes")
// 6 mansion scenes; 2 cell scenes 출력
```

## Unicode Representations of Strings (문자열의 유니코드 표현)
유니코드 문자열이 텍스트 파일이나 다른 저장소에 작성될 때, 해당 문자열의 유니코드 스칼라들은 여러 유니코드로 정의된 인코딩 형식 중 하나로 인코딩된다. 각 양식는 코드 단위로 알려진 작은 청크로 문자열을 인코딩한다. UTF-8 인코딩 양식 (문자열을 8비트 코드 단위로 인코딩 한)과 UTF-16 인코딩 양식(문자열을 16비트 코드 단위로 인코딩 한) 그리고 UTF-32 인코딩 양식(문자열을 32비트 코드 단위로 인코딩 한)을 포함한다.<br>

Swift는 문자열 유니코드 표현 접근에 몇개의 다양한 방식을 제공한다. 문자열을 `for-in`문으로 반복하여 유니코드 확장된 문자소 클러스터로 각각의 문자 값에 접근 할 수 있다. 이 과정은 `Working with Characters`에 나와있다.<br>

또는, 3개의 다른 유니코드 호환 표현 중 하나의`String`값에 접근한다:

* UTF-8 코드 단위 모음 (문자열의 `utf8`프로퍼티로 접근)
* UTF-16 코드 단위 모음 (문자열의 `utf16`프로퍼티로 접근)
* 문자열의 UTF-32 인코딩 형식에 해당하는 21비트 유니코드 스칼라 값 모음 (문자열의 `unicodeScalars`프로퍼티로 접근)

아래 예제는 문자 `D`,`o`,`g`,`!!` (`DOUBLE EXCLAMATION MARK, or Unicode scalar U+203C`)과 🐶문자 (`DOG FACE, or Unicode scalar U+1F436`)로 구성된 다음 문자열의 다른 표현을 보여준다.

`let dogString = "Dog‼🐶"`

### UTF-8 Representation (UTF-8 표현)
`utf8`프로퍼티를 반복해 `String`의 UTF-8 표현에 접근 할 수 있다. 이 프로퍼티는 문자열의 UTF-8 표현에서 각 바이트에 대해 하나씩 부호없는 8비트(`UInt8`)값의 모음인 `String.UTF8View` 형태이다.<br>

![](https://docs.swift.org/swift-book/_images/UTF8_2x.png)

```
for codeUnit in dogString.utf8 {
    print("\(codeUnit) ", terminator: "")
}
print("")
// 68 111 103 226 128 188 240 159 144 182 출력
```
위의 예제에서, 첫번째 3개 10진수 `codeUnit`값 (`68, 111, 103`)은 UTF-8 표현이 ASC|| 표현과 비슷한  `D`,`o`,`g`를 나타낸다. 다음 3개의 10진수 `codeUnit`값 (`226, 128, 188`)은 ` DOUBLE EXCLAMATION MARK `문자의 3바이트 UTF-8 표현이다. 마지막 4개 `codeUnit`값 (`240, 159, 144, 182`)은 `DOG FACE`문자의 4바이트 UTF-8표현이다.

### UTF-16 Representation (UTF-16 표현)
`utf16`프로퍼티를 반복해 `String`의 UTF-16 표현에 접근 할 수 있다. 이 프로퍼티는 문자열의 UTF-16 표현에서 각 바이트에 대해 하나씩 부호없는 16비트(`UInt16`)값의 모음인 `String.UTF16View` 형태이다.<br>

![](https://docs.swift.org/swift-book/_images/UTF16_2x.png)
```
for codeUnit in dogString.utf16 {
    print("\(codeUnit) ", terminator: "")
}
print("")
// 68 111 103 8252 55357 56374 출력
```

다시, 첫번째 3개 `codeUnit`값 (`68, 111, 103`)은 문자열의 UTF-8 표현과 같은 값을 갖는 UTF-16 코드 단위인 `D`, `o`, `g` 문자이다.<br>

네번째 `codeUnit` 값 (`8252`)는 `DOUBLE EXCLAMATION MARK`문자에 대한 유니코드 스칼라 `U+203C`를 나타내는 16진수 값 `203C`에 해당하는 10진수이다.<br>

다섯번째, 여섯번째 `codeUnit`값인 (`55357, 56374`)는 `DOG FACE`문자의 UTF-16 서러게이트(surrogate) 쌍 표현이다. 이 값들은 `U+D83D`(10진수 `55357`)의 high-surrogate 값이고 `U+DC36`(10진수 `56374`)의 low-surrogate 값이다.

### Unicode Scalar Representation (유니코드 스칼라 표현)
`unicodeScalars`프로퍼티를 반복적으로 수행해 `String`값의 유니코드 스칼라 표현에 접근 할 수 있다. 이 프로퍼티는 `UnicodeScalar`타입의 값의 컬렉션인 `UnicodeScalarView`타입이다.<br>

각 `UnicodeScalar`에는 `UInt32`값 내에 표시된 스칼라의 21비트 값을 반환하는 `value` 프로퍼티다.<br>

![](https://docs.swift.org/swift-book/_images/UnicodeScalar_2x.png)

```
for scalar in dogString.unicodeScalars {
    print("\(scalar.value) ", terminator: "")
}
print("")
// 68 111 103 8252 128054 출력
```

첫번째 3개 `UnicodeScalr`값 (`68, 111, 103`)에 대한 `value` 프로퍼티는 다시 한번 `D`,`o`,`g`를 나타낸다.<br>

네번째 `codeUnit` 의 값 (`8252`)는 다시 `DOUBLE EXCLAMATION MARK ` 문자에 대한 유니코드 스칼라 `U + 203C`를 나타내는 16진수 값 `203C`에 해당하는 10진수이다. <br>

다섯번째와 마지막 `UnicodeScalar`,`128054`의 `value`프로퍼티는 `DOG FACE`문자에 대한 유니코드 스칼라 `U+1F436`을 나타내는 16진수 값 `1F436`에 해당하는 10진수이다.<br>

`value`프로퍼티를 쿼리하는 대신, 각 `UnicodeScalar` 값은 문자 보간과 같이새로운 `String`값을 만드는데 사용 될 수 있다.

```
for scalar in dogString.unicodeScalars {
    print("\(scalar) ")
}
// D
// o
// g
// ‼
// 🐶
```
