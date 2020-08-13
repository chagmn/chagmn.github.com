# Subscripts (서브스크립트)

클래스, 구조체, 열거형에서 스크립트를 정의해 사용할 수 있다. 서브스크립트는 콜렉션, 리스트, 시퀀스 등 집합의 특정 멤버 요소에 간단하게 접근할 수 있는 문법이다. 서브스크립트를 이용하면 추가적인 메서드 없이 특정 값을 할당하거나 가져올 수 있다. 예를들면, 배열 인스턴스의 특정 요소는 `someArray[index]`문법으로, 딕셔너리 인스턴스의 특정 요소는 `someDictionary[key]`로 접근할 수 있다. 하나의 타입에 여러 서브스크립트를 정의할 수 있고 오버로드도 가능하다. 뿐만아니라 단일 인자 값을 넘어, 필요에 따라 복수 인자 값을 사용할 수도 있다.

## Subscript Syntax (서브스크립트 문법)

서브스크립트 선언 문법은 인스턴스 메서드와 계산된 프로퍼티를 선언하는 것과 비슷하다. 인스턴스 메서드와 다른 점은, 서브스크립트는 읽고-쓰기나 읽기 전용만 가능하다는 점이다. 정의는 계산된 프로퍼티 방식과 같이 `setter`, `getter`방식을 따른다.

```swift
subscript(index: Int) -> Int {
	get {
		// 반환 값
	}
	set(newValue){
		// set 액션
	}
}
```

서브스크립트의 `set`에 대한 인자 값을 따로 지정하지 않으면 기본 값으로 `newValue`를 사용한다. 읽기 전용으로 선언하려면 `get`, `set`을 지우고 따로 지정하지 않으면 `get`으로 동작해서 읽기 전용으로 선언된다.

```swift
subscript(index: Int) -> Int {
	// 반환 값
}
```

다음은 읽기 전용으로 선언한 서브스크립트 예시이다.

```swift
struct TimesTable {
	let multiplier: Int
	subscript(index: Int) -> Int {
		return multiplier * index
	}
}
let threeTimesTable = TimesTable(multiplier: 3)
print("six times three is \(threeTimesTable[6])")
// six times three is 18 출력
```

`TimesTable`구조체의 `multiplier`를 3으로 설정하고 `threeTimesTable[6]`에서 6번째 값, 3에서 6을 곱한 값을 출력한다.



## Subscript Usage (서브스크립트 사용)

딕셔너리에서 서브스크립트 사용의 예시이다.

```swift
var numberOfLegs = ["spider": 8, "ant": 6, "cat": 4]
numberOfLegs["bird"] = 2
```

`numberOfLegs`값은 타입 추론에 의해 `[String: Int]`형을 갖는다. `numberOfLegs["bird"] = 2`는 사전형 변수 `numberOfLegs`에 키로 bird, 값은 2를 넣으라는 서브스크립트 문법이다.



## Subscript Options (서브스크립트 옵션)

서브스크립트는 입력 인자의 숫자에 제한이 없고, 입력 인자의 타입과 반환 타입의 제한도 없다. 다만 in-out인자 나 기본 인자값을 제공할 수는 없다. 서브스크립트는 오버로딩도 허용한다. 그래서 인자형, 반환형에 따라 원하는 수 만큼의 서브스크립트를 선언할 수 있다. 다음은 서브스크립트를 이용해 다차원 행열을 선언하고 접근하는 예시 이다.

```swift
struct Matrix {
    let rows: Int, columns: Int
    var grid: [Double]
    init(rows: Int, columns: Int) {
        self.rows = rows
        self.columns = columns
        grid = Array(repeating: 0.0, count: rows * columns)
    }
    func indexIsValid(row: Int, column: Int) -> Bool {
        return row >= 0 && row < rows && column >= 0 && column < columns
    }
    subscript(row: Int, column: Int) -> Double {
        get {
            assert(indexIsValid(row: row, column: column), "Index out of range")
            return grid[(row * columns) + column]
        }
        set {
            assert(indexIsValid(row: row, column: column), "Index out of range")
            grid[(row * columns) + column] = newValue
        }
    }
}
```

위 코드에서 `subscript(row: Int, column: Int) -> Double`과 같이 row, column 2개의 인자를 받고, Double형을 반환하는 서브스크립트를 선언했다. get, set 각각에 `indexIsValid`메서드를 사용해서 유효한 인덱스가 아닌 경우 프로그램이 종료되도록 `assert`를 호출했다. 선언한 서브스크립트 문법을 이용해 `var matrix = Matrix(rows: 2, columns: 2)` 2x2 행렬을 선언했다.

![](https://docs.swift.org/swift-book/_images/subscriptMatrix01_2x.png)

grid 배열은 서브스크립트에 의해 위 사진과 같이 row와 column을 갖는 행렬도 동작한다. 행렬에 서브스크립트를 이용해 row와 column에 값을 저장할 수 있다.

```swift
matrix[0, 1] = 1.5
matrix[1, 0] = 3.2
```

값을 저장한 결과 행렬은 다음과 같은 모습이 된다.

![](https://gblobscdn.gitbook.com/assets%2F-LKLx6PA5iF3Uq2IzQsf%2F-LKMNVThMs-6OciUjtgW%2F-LKMNoEZxJZygWqCLkZp%2FBE27D61E-5730-4155-B132-ED1A14038787.png?alt=media&token=52c94ff7-b995-4a97-b1bf-3415e91b7ada)

행렬의 입출력시 row, column의 범위가 적절한지 아래의 코드로 확인한다.

```swift
func indexIsValid(row: Int, column: Int) -> Bool {
	return row >= 0 && row < rows && column >= 0 && column < columns
}
```

만약 적절한 범를 벗어나면 assert가 실행된다.

```swift
let someValue = matrix[2, 2]
// [2, 2]가 사용할 수 있는 행렬의 범위를 벗어나므로 assert가 실행
```



## Type Subscripts (타입 서브스크립트)

위의 설명대로 인스턴스 서브스크립트는 특정 타입의 인스턴스에서 호출하는 서브스크립트다. 타입 스스로 호출되는 서브스크립트를 정의할 수 있다. 이러한 서브스크립트 종류를 타입 서브스크립트라고 부른다. `subscript`키워드 전에 `static`키워드를 적어서 타입 스크립트라고 명시한다. 클래스들은 대신 `class`키워드를 사용해 하위 클래스가 해당 서브스크립트의 상위 클래스 구현을 재정의 할 수 있다. 아래 예시가 타입 서브스크립트를 정의하고 호출하는지 나타낸다.

## enum Planet: Int {
    case mercury = 1, venus, earth, mars, jupiter, saturn, uranus, neptune
    static subscript(n: Int) -> Planet {
        return Planet(rawValue: n)!
    }
}
let mars = Planet[4]
print(mars)

