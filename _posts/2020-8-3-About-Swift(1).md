---
title: "About Swift5.2 (Part.1)"
date: 2020-8-3
categories: iOS&Swift
---
## About Swift 5.2 (Part.1)

Apple에서 공개한 The Swift Programming Language 책의 Basics 부분을 참고하여 작성

### Constants and Variables (상수와 변수)

상수 = 한번 설정하면 바꿀 수 없음. ⇒ let <br>
 `let temp = 10`<br>
변수 = 나중에 다른 값으로 설정하는 것이 가능. ⇒ var<br>
 `var temp = 10`<br>
한 줄에 여러 개의 상수와 변수를 선언 하려면, 쉼표로 구분하면 된다.<br>
`var x = 0, y = 1, z = 2`<br>

### Type Annotations (타입 보조 설명)

그럼 상수, 변수는 알겠는데 타입은 어떻게 나타내야 하는가?<br>
이때 사용해야 하는 것이 바로 타입 보조 설명이다.<br>
`var temp : String` <br>
→ 이 문장의 뜻은 "String형의 변수 temp를 생성한다."<br>
`var x, y, z : Double`<br>
이런 식으로 여러 개의 변수에 대해 타입 보조 설명을 설정 할 수 있다.

### Naming Constants and Variables (상수와 변수 이름 짓기)

상수, 변수의 이름은 유니코드 문자를 포함해 거의 어떤 문자라도 가질 수 있다.
기본적으로, 공백, 수학 기호, 화살표, 사용자 영역 유니코드 크기 값, 선 그리기, 상자 그리기 문자를 갖을 수 없다. 숫자로 시작하는 것도 안되지만, 이름 내의 다른 곳에서 숫자를 포함할 수는 있다.<br>

일단 한번 상수, 변수를 정해진 타입으로 선언하면, 같은 이름으로 다시 선언하거나, 다른 타입의 값을 저장하도록 바꿀 수 없다. 상수를 변수로 바꾸거나 변수를 상수로 바꾸는 것도 안된다.

### Print Constants and Variables (상수와 변수 출력하기)

현재 상수, 변수의 값은 `print(_:separator:terminator:)` 함수로 출력할 수 있다.<br>
```
var temp = "hi!"
print(temp)
// "hi!" 가 출력된다.
```
`separator` 과 `terminator` 매개 변수는 기본 설정 값을 갖고 있으며, 함수를 호출할 때 생략할 수 있다. 기본적으로, 이 함수는 출력하는 줄에 '줄 나눔' 을 추가하여 끝낸다. 값을 '줄 나눔' 없이 출력 하려면, 빈 문자열을 terminator 로 전달하면 된다.<br>

Swift는 문자열 보간법을 사용해 더 긴 문자열 속에 자리 표시 용도로 상수나 변수의 이름을 포함한 후, 이를 해당 상수, 변수의 현재 값으로 대체하도록 Swift에게 알린다. 이는 이름을 괄호로 감싼 후, 시작 괄호 앞에 '\' 을 적어 주면 된다.<br>
```
print("This is example : value of temp = \(temp)")
// This is example : value of temp = hi! 가 출력된다.
```

### Comments (주석)

주석은 코드 내에 실행되지 않는 텍스트를 넣어서, 설명이나 기억을 되살리는 용도로 사용한다.
C언어와 비슷하게 주석이 구성되어 있다. 먼저 한 줄 짜리 주석은 `//` 로 시작한다.<br>
`// 이것은 주석이다.`<br>
여러 줄 짜리 주석은 `/* 부터 */` 까지다.<br>
`/* 이것은`<br>
`주석이다 */`<br>
그 외 Swift만의 특별한 점은 여러 줄 짜리 주석은 다른 여러 줄 짜리 주석을 중첩할 수 있다는 것이다.<br>
```
/* 이것은 첫번째 주석
/* 이것은 두번째 주석 */
세번째 주석입니다 */
```

### Semicolons (세미콜론)

Swift는 코드 문장 뒤에 세미콜론(`;`)을 작성할 필요가 없지만, 원하면 사용해도 된다.
여러 개의 분리된 문장을 한 줄에 쓰고 싶으면 세미콜론을 필수적으로 사용해야 한다.<br>
```
let dog = "bark!!"; print(dog)
// bark!! 가 출력됩니다.
```

### Type Safety and Type Inference (타입 안전 장치와 타입 추론 장치)

Swift는 타입-안전 언어이다. 타입-안전 언어를 사용하면 코드에서 사용할 값의 타입을 명확히 알 수 있다. 코드 일부에서 `String` 을 요구하는데, 실수로 `Int` 를 전달하는 일은 일어나지 않는다.
Swift는 타입에 안전하므로, 코드를 컴파일할 때 '타입 검사' 를 수행하고 맞지 않는 타입이 있으면 에러로 표시한다. 이를 통해 개발 과정에서 최대한 빨리 에러를 잡아내고 고칠 수 있다.<br>

'타입 검사' 는 다른 타입의 값을 섞어 쓰는 에러를 피할 수 있도록 해준다. 하지만, 이것이 상수와 변수를 선언할 때마다 모든 타입을 일일이 지정해야 한다는 것을 의미하는 것은 아니다. 값의 타입이 필요한 곳에서 이를 지정하지 않은 경우, Swift는 '타입 추론 장치' 를 사용해서 알맞은 타입을 알아낼 수 있다. 
'타입 추론 장치' 를 통해 컴파일러는 코드를 컴파일할 때, 제공한 값을 검토해서, 특정 표현식의 타입을 자동으로 파악할 수 있다.<br>

'타입 추론 장치' 가 더 유용한 순간은 상수나 변수를 선언할 때 기본 설정 값이 있는 경우다. 이는 상수, 변수를 선언하는 시점에서 글자 값을 할당하는 것으로 이루어 진다.<br>

```
let value = 20 //value 의 타입을 Int형으로 추론
let value2 = 2.0 //value2 의 타입을 Double형으로 추론
```

### Type Aliases (타입 별명)

타입 별명은 기존 타입에 대한 대체 이름을 정의하는 것이다. 타입 별명을 정의할 때는 `typealias` 키워드를 사용한다.
타입 별명은 기존 타입을 문맥에 더 알맞은 이름으로 참조하고 싶을 때 유용하며, 가령 외부 소스에서 크기가 지정된 데이터와 작업할 때는 아래와 같이 할 수 있다.<br>
```
typealias Sample = UInt16
var minValue = Sample.min
// minValue 는 이제 0 이다.
```
`Sample`을 `UInt16`의 별명으로 정의했다. 이것은 별명이여서, `Sample.min`을 호출하는 것은 실제로 `UInt16.min`을 호출하는 것이며, 이것은 `minValue`변수에 기본 설정 값으로 `0`을 제공한다.<br>

### Tuples (튜플, 짝)

튜플은 여러 개의 값을 그룹지어 단일한 하나의 복합 값으로 만들어 준다. 튜플 안의 값은 어떤 타입이어도 상관 없고 서로 같은 타입이 아니어도 된다. <br>
```
let error = (404, "Not Found")
// error의 타입은 (Int, String) 이며, 값은 (404, "Not Found") 와 같다
```
튜플을 만들 때는 타입의 순서 같은 것은 아무 상관이 없으며, 서로 다른 타입을 원하는 만큼 많이 집어 넣어도 상관 없다. 타입이 많아도 다 튜플을 만들 수 있으며, 진짜로 원하는 데로 아무 순서로 만들어도 상관없다.<br>

튜플의 내용물은 별도의 상수나 변수로 분해할 수 있으며, 평소처럼 접근할 수도 있다.<br>
```
let (Code, Message) = error
print("The status code is \(Code)")
// The status code is 404 를 출력
print("The status message is \(Message)")
// The status message is Not Found 를 출력
```
튜플 값 중에서 일부만 필요한 경우, 튜플을 분해할 때 '밑줄`_`' 을 써서 그 일부분을 무시할 수 있다.<br>
```
let (Code, _) = error
print("The status code is \(Code)")
// The status code is 404 를 출력
```
다른 방법으로, 인덱스 번호를 사용해 튜플의 개별 원소 값에 접근할 수도 있다.<br>
```
print("The status code is \(error.0)")
// The status code is 404 를 출력
print("The status message is \(error.1)")
// The status message is Not Found 를 출력
튜플을 정의할 때 튜플에 있는 개별 원소에 이름을 부여할 수도 있다.
let 200Status = (code: 200, description: "OK")
이렇게 튜플의 원소에 이름을 부여했으면, 그 원소의 값에 접근할 때 원소의 이름을 사용할 수 있다.
print("The status code is \(200Status.code)")
// The status code is 200 을 출력
```
