# CHAPTER 13 스위프트 프로퍼티 래퍼

## 13. 스위프트 프로퍼티 래퍼

* 프로퍼티 래퍼 형태
* class와 struct 구현부에 getter, setter, 연산 프로퍼티 코드의 중복을 줄이는 방법을 제공함

### 13.1 프로퍼티 래퍼 이해하기

* 프로퍼티 유용한 때
  * instance의 프로퍼티에 값을 할당하거나 접근할 때 값을 저장하거나 읽기 전,
  * 변환 작업이나 유효성 검사를 하는 경우
  * 연산 프로퍼티로 구현할 수 있는데, 유사한 패턴을 갖는 경우가 빈번하다.
* 연산 프로퍼티의 기능을 개별 클래스와 구조체와 분리할 수 있게 한다
* 앱 코드에서 재사용할 수 있다.

### 13.2 간단한 프로퍼티 래퍼 예제

* property wrapper
  * `@propertyWrapper` 지시자를 이용해 선언한다
  * 클래스나 구조체 안에 구현된다.
  * `wrappedValue` 프로퍼티를 가져야 한다.
  * init은 optional
* property wrapper는 재사용할 수 있다.

### 13.3 여러 변수와 타입 지원하기

* 필요에 따라 더 복잡한 프로퍼티 래퍼를 구현할 수 있다.
* 추가되는 값들은 프로퍼티 래퍼 이름 다음의 괄호 안에 둔다.
*   지정된 값으로 사용하도록 설계된 프로퍼티 래퍼의 형태는 다음과 같다.

    ```swift
    struct Demo {
      @MinMaxVal(min: 10, max: 160) var value: Int = 100
    }
    ```
* property wrapper의 parameter type으로 protocol이 올 수도 있다.
*   `MinMaxVal` 프로퍼티 래퍼의 목적에 따라 `Foundation` 프레임워크 내의 `Comparable` 프로토콜을 받자.

    * 이를 위해 Generic을 사용한다.

    ```swift
    @propertyWrapper
    struct MinMaxVal<V: Comparable> {
        var value: V
        var min: V
        var max: V

        init(wrappedValue: V, min: V, max: V) {
            value = wrappedValue
            self.min = min
            self.max = max
        }

        var wrappedValue: V {
            get { value }
            set {
                switch newValue {
                case max...:
                    value = max
                case ...min:
                    value = min
                default:
                    value = newValue
                }
            }
        }
    }
    ```

    * example of Comparable:
      * String, Int, Date, DateInterval, Character 등

### 13.4 요약

* 클래스 및 구조체 선언 내 코드 중복을 피하면서
* 앱 프로젝트의 코드를 통해
* 재사용되는 프로퍼티의 게터와 세터 구현체를 사용할 수 있게 함
* `@propertyWrapper` 지시자를 이용해 구조체 형태로 선언된다.
* 미리 정의된 프로퍼티 래퍼는 SwiftUI에서 광범위하게 사용됨
  * example of ... 추후 작성
