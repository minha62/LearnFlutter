# Variables

main 함수는 모든 Dart 프로그램의 entry point기 때문에 아주 중요

반드시 main 함수가 있어야 하는데, main 함수에서 내가 쓴 코드가 호출되기 때문

dart는 일부로 세미콜론(;)을 안 쓰는 경우가 있기 때문에 자동으로 ; 붙여주지 않음

<br>

## 변수 만드는 법

### 1. var

```dart
var name = 'sumin';
```

변수 타입 구체화할 필요 없음 - dart 컴파일러가 자동으로 추측해냄

변수 값 업데이트 가능하지만 타입이 동일해야 함

> 관습적으로 함수나 메소드 내부에 지역 변수 선언할 때 사용
> 

### 2. 명시적으로 타입 지정

```dart
String name = 'sumin';
```

> class에서 변수나 property 선언할 때 사용
> 

<br>

## Dynamic Type

Dart는 개발자친화적 (다른 언어들은 아주 엄격한 규칙을 가짐) - Dart는 때때로 규칙 벗어나도 되는 자유가 있음

⇒ Dynamic (여러가지 타입을 가질 수 있는 변수에 쓰는 키워드)

사용을 추천하진 않지만 때때로 필요함 (정말 필요할 때만 쓰기!)

```dart
var name; or dynamic name;
name = 'sumin';
name = 2;
name = true;
```

왜 필요?

- 변수가 어떤 타입일지 알기 어려운  경우 (특히 flutter나 json과 함께 작업할 때)
- 가끔 dynamic으로 살짝 돌아가는 게 유용한 경우

dynamic 변수로 뭔가 작업을 하고 싶다면 먼저 **타입을 확인**해줘야 한다.

```dart
void main() {
	dynamic name;
	// name. 메소드 별로X (타입 모르니까)
	if (name is String){
				// name. 이제 타입이 String인 걸 아니까 아주 많은 옵션을 자동완성해줌
	}
}
```
<br>

## Nullable Variables

**📌 dart의 변수는 기본적으로 non-nullable (null이 될 수 없음)**

### null safety

> 개발자가 null 값을 참조할 수 없도록 하는 것
> 

null 값을 참조하면 런타임 에러 발생

- 런타임 에러: 사용자가 우리의 앱을 사용하던 중에 뜨는 에러
- 런타임 에러는 매우 안 좋기 때문에 컴파일 전에 이 에러를 잡아내는 게 좋음

> null safety가 없는 경우
> 

```dart
bool isEmpty(String string) => string.length == 0;

main() {
	isEmpty(null);
}
```

String 대신 null을 isEmpty에 보냈기 때문에 `NoSuchMethodError`가 발생

null은 length 속성이 없기 때문에 에러 발생 ⇒ 이 에러는 사용자 기기에서 발생 (컴파일러가 못 잡는 에러)

**dart에서는 어떤 변수가 null이 될 수 있음을 정확히 표시해야 함 ⇒ `?`**

```dart
void main() {
	String? name = 'red';
	name = null;
	// name.length; // 위에서와 달리 length에 빨간줄이 생김 (null 일 수 있다고)
	if (name != null) {
		name.isNotEmpty; // 이 부분에서 컴파일러는 name이 null이 아님을 확실히 알 수 있음
	}
}
```

위 아래는 같은 기능

```dart
void main() {
	String? name = 'red';
	name = null;
	name?.isNotEmpty; // name값이 존재하는지 확인하고 이후의 연산 진행
}
```

<br>

## Final Variables

> var 대신 final로 변수를 만들면 해당 변수는 수정 불가
> 

JS나 TS의 const와 동일

```dart
void main() {
	final name = 'red';
	name = 'blue'; // 에러 발생 The final variable 'name' can only be set once.
}
```

final 뒤에 타입 넣어줘도 됨 `final String name` (필수X)

<br>

## Late Variables

> 초기 값 없이 변수를 선언할 수 있게 해줌
> 

final이나 var 앞에 붙일 수 있음

```dart
void main() {
	late final String name;
	// 필요한 데이터가 아직 없으니 late으로 선언해두고
	// API에서 데이터를 받아 변수에 넣어줌
	name = 'red';
}
```

실수를 막아준다는 장점

- 만약 late으로 선언해두고 아직 값을 넣지 않았다면, print(name);을 했을 때 dart가 막아줌

<br>

## Constant Variables

> compile-time constant를 만들어줌 (아주 중요!)
⇒ const는 compile-time에 알고 있는 값이어야 함 (앱스토어에 앱을 올리기 전에 알고있어야함)
> 

JS나 TS의 const와 다름

```dart
void main() {
	const name = 'red';
	name = 'blue'; // Error
	
	const api = some_func(); // Error
	final api = some_func();
	var api = some_func();

	const max_allowed_price = 120;
}
```
