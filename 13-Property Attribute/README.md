# 내부 슬롯 & 내부 메서드

[내부 슬롯 & 내부 메서드 Detail](https://medium.com/jspoint/what-are-internal-slots-and-internal-methods-in-javascript-f2f0f6b38de)

- `내부 슬롯 & 내부 메서드` 는 자바스크립트 엔진에서 실제로 동작은 하지만 `개발자가 직접 접근할 수 있도록 외부로 공개된 객체의 프로퍼티는 아니다.`
- 즉, 자바스크립트 엔진의 `내부 로직` 이므로 자바스크립트로 직접 접근하거나 호출할 수 있는 방법을 제공하진 않는다.
- 단, 일부 내부 슬롯 & 메서드에 대해서는 접근할 수 있는 수단을 제공한다.
  - 예를 들어 `[[Prototype]]` 내부 슬롯은 `__proto__` 를 통해 간접 접근 가능

<br />
<br />

# 프로퍼티 어트리뷰트 & 디스크립터 객체

### Javascript Property Atrribute

> `Property Attribute` : 자바스크립트 엔진은 프로퍼티를 생성할 때 `프로퍼티 상태` 를 기본값으로 자동 정의한다.

- 프로퍼티 상태
  - `프로퍼티의 값( value )`
  - `값의 갱신 가능 여부( writable )`
  - `열거 가능 여부( enumerable )`
  - `정의 가능 여부( configurable )`
- 프로퍼티 어트리뷰트에 직접 접근할 수는 없고, `간접적으로 확인` 은 할 수 있다.
  ```jsx
  Object.getOwnPropertyDescriptor(객체, 프로퍼티 키)
  ```
  - 객체 : `객체의 참조(Reference)` 를 전달
  - 프로퍼티 키 : `문자열 or 심벌`을 전달

<br />

### Javascript Property Descriptor Object

> `Property Descriptor Object` : 프로퍼티 어트리뷰트 정보를 제공하는 객체

- 존재하지 않는 프로퍼티나 상속받은 프로퍼티에 대한 프로퍼티 디스크립터를 요구하면 → `undefined 반환`
- 기본적으로 하나의 프로퍼티에 대해 프로퍼티 디스크립터 객체를 반환
  - ES8 에 `모든 프로퍼티의 프로퍼티 어트리뷰트 정보`를 제공하는 프로퍼티 디스크립터 객체를 반환할 수 있게 되었다.
  ```jsx
  Object.getOwnPropertyDescriptors(객체);
  ```

<br />
<br />

# 프로퍼티

> 프로퍼티는 `데이터 프로퍼티` 와 `접근자 프로퍼티` 로 구분할 수 있다.

### 데이터 프로퍼티

> `데이터 프로퍼티(data property)` : `(key : value)` 로 구성된 일반적인 프로퍼티

- 데이터 프로퍼티 어트리뷰트는 자바스크립트 엔진이 `프로퍼티를 생성할 때 기본값으로 자동 정의된다.`

| 프로퍼티 어트리뷰트 | 프로퍼티 디스크립터 객체의 프로퍼티 | 설명                                                     |
| ------------------- | ----------------------------------- | -------------------------------------------------------- |
| [[Value]]           | value                               | + 프로퍼티 키를 통해 프로퍼티 값에 접근하면 반환 되는 값 |
| [[Writable]]        | writable                            | + 프로퍼티 값 변경 가능 여부 → boolean                   |
| [[Enumerable]]      | enumerable                          | + 프로퍼티의 열거 가능 여부 → boolean                    |
| [[Configurable]]    | configurable                        | + 프로퍼티의 재정의 가능 여부 → boolean                  |

- 프로퍼티 생성 시
  - `[[ Value ]]` 값은 `프로퍼티 값`으로 초기화
  - `[[ Writable ]], [[ Enumerable ]], [[ Configurable ]]` 는 모두 `true` 로 초기화
  - 프로퍼티를 `동적 생성` 해도 마찬가지로 적용

```jsx
const person = {
  name: "WI",
};

// age 프로퍼티 동적 생성
person.age = 100;

console.log(Object.getOwnPropertyDescriptors(person));

/*
{
  name: { value: 'WI', writable: true, enumerable: true, configurable: true },
  age: { value: 100, writable: true, enumerable: true, configurable: true }
}
 */
```

<br />

### 접근자 프로퍼티

> `접근자 프로퍼티(accessor property)` : 자체적으로 값을 갖지 않고 다른 데이터 프로퍼티의 값을 `읽거나 저장할 때` 호출되는 `접근자 함수(accessor function)` 로 구성된 프로퍼티

| 프로퍼티 어트리뷰트 | 프로퍼티 디스크립터 객체의 프로퍼티 | 설명                                                                     |
| ------------------- | ----------------------------------- | ------------------------------------------------------------------------ |
| [[Get]]             | get                                 | 데이터 프로퍼티의 값을 읽을 때 호출되는 접근자 함수 → getter 함수 호출   |
| [[Set]]             | set                                 | 데이터 프로퍼티의 값을 저장할 때 호출되는 접근자 함수 → setter 함수 호출 |
| [[Enumerable]]      | enumerable                          | 데이터 프로퍼티의 [[Enumerable]] 과 상동                                 |
| [[Configurable]]    | configurable                        | 데이터 프로퍼티의 [[Configurable]] 과 상동                               |

- 접근자 함수는 `getter / setter 함수` 라고도 한다.
  - `getter 와 setter` 는 모두 정의하거나 하나만 정의할 수도 있고, 아예 정의 안할 수도 있다.

```jsx
const person = {
  firstName: "Youngmin",
  lastName: "WI",

  get fullName() {
    return `${this.firstName} ${this.lastName}`;
  },

  set fullName(name) {
    [this.firstName, this.lastName] = name.split(" ");
  },
};

// 데이터 프로퍼티를 통한 프로퍼티 값의 참조
console.log(person.firstName + " " + person.lastName); // Youngmin WI

// 접근자 프로퍼티를 통한 프로퍼티 값의 저장 👉 setter 함수 호출
person.fullName = "Youngmaaan WIE";
console.log(person); // { firstName: 'Youngmaaan', lastName: 'WIE', fullName: [Getter/Setter] }

// 접근자 프로퍼티를 통한 프로퍼티 값의 참조 👉 getter 함수 호출
console.log(person.fullName); // Youngmaaan WIE

// 데이터 프로퍼티는 [[Value]], [[Writable]], [[Enumerable]], [[Configurable]] 프로퍼티 어트리뷰트를 갖는다.
// 현 코드에서 "firstName" 프로퍼티는 "데이터 프로퍼티"다.
let descriptor = Object.getOwnPropertyDescriptor(person, "firstName");
console.log(descriptor);
/*
{
  value: 'Youngmaaan',
  writable: true,
  enumerable: true,
  configurable: true
} 
*/

// 접근자 프로퍼티는 [[Get]], [[Set]], [[Enumerable]], [[Configurable]] 프로퍼티 어트리뷰트를 갖는다.
// 현 코드에서 "fullName" 프로퍼티는 "접근자 프로퍼티"다.
descriptor = Object.getOwnPropertyDescriptor(person, "fullName");
console.log(descriptor);
/*
{
  get: [Function: get fullName],
  set: [Function: set fullName],
  enumerable: true,
  configurable: true
}
*/
```

<br />
<br />

# 프로퍼티 정의

> `프로퍼티 정의` : 새로운 프로퍼티를 추가하면서 프로퍼티 어트리뷰트를 `명시적으로 정의` 하거나, 기존 프로퍼티의 프로퍼티 어트리뷰트를 `재정의` 하는 것을 의미

```jsx
// 객체에 프로퍼티 하나 정의
Object.defineProperty(객체, 추가할 프로퍼티, {
  value: 값,
  writable: boolean,
  enumerable: boolean,
  configurable: boolean
})

// 객체에 프로퍼티 여러 개 정의
Object.defineProperties(객체, {
  데이터 프로퍼티 1: {
		value: 값,
	  writable: boolean,
	  enumerable: boolean,
	  configurable: boolean
  },
	데이터 프로퍼티 2: {
		value: 값,
	  writable: boolean,
	  enumerable: boolean,
	  configurable: boolean
  },
  ...
  접근자 프로퍼티 1: {
	  get() { ... },
		set() { ... },
	  enumerable: boolean,
	  configurable: boolean
  },
	접근자 프로퍼티 2: {
		get() { ... },
		set() { ... },
	  enumerable: boolean,
	  configurable: boolean
	}
})
```

- 각 프로퍼티에 대해 모든 프로퍼티 어트리뷰트를 설정할 필요는 없다.
  - 설정하지 않을 경우 default 로 설정되는 값들이 있다.

| 프로퍼티 디스크립터 객체의 프로퍼티 | 대응하는 프로퍼티 어트리뷰트 | 생략했을 경우 Default Value |
| ----------------------------------- | ---------------------------- | --------------------------- |
| value                               | [[Value]]                    | undefined                   |
| get                                 | [[Value]]                    | undefined                   |
| set                                 | [[Value]]                    | undefined                   |
| writable                            | [[Value]]                    | false                       |
| enumerable                          | [[Value]]                    | false                       |
| configurable                        | [[Value]]                    | false                       |

<br />
<br />

# 객체 변경 방지

- 객체는 기본적으로 `변경 가능한 값(mutable value)` 라고 했다.
- 즉, 재할당 없이 직접 변경할 수 있음을 의미한다.
  - 프로퍼티를 추가, 삭제, 값 갱신 모두 할 수 있다.
  - `object.defineProperty` or `object.definePropertie` 메서드를 사용하여 프로퍼티 어트리뷰트를 재정의할 수도 있다.

> 하지만, 객체를 `변경하지 말아야할 경우가 존재`하고, 이를 지원하는 메서드를 제공한다.

- 각 메서드마다 제한하는 강도가 다르다.
- `strict mode` 에서 허용하지 않는 프로퍼티 제어 동작에 대해서는 `에러를 발생시킴`
- `strict mode 가 아닌 경우` 에서는 해당 명령을 `무시한다.`

| 구분           | 메서드                   | 프로퍼티 추가 | 프로퍼티 삭제 | 프로퍼티 값 읽기 | 프로퍼티 값 쓰기 | 프로퍼티 어트리뷰트 재정의 |
| -------------- | ------------------------ | ------------- | ------------- | ---------------- | ---------------- | -------------------------- |
| 객체 확장 금지 | Object.preventExtensions | X             | O             | O                | O                | O                          |
| 객체 밀봉      | Object.isSealed          | X             | X             | O                | O                | X                          |
| 객체 동결      | Object.freeze            | X             | X             | O                | X                | X                          |

<br />

### 객체 확장 금지

```jsx
Object.preventExtensions(객체);

// 확장 가능한 객체인지 판단 여부 메서드 -> boolean
Object.isExtensible(객체);
```

<br />

### 객체 밀봉

```jsx
Object.seal(객체);

// 밀봉된 객체인지 판단 여부 메서드 -> boolean
Object.isSealed(객체);
```

<br />

### 객체 동결

```jsx
Object.freeze(객체);

// 동결된 객체인지 판단 여부 메서드 -> boolean
Object.isFrozen(객체);
```

<br />
<br />

# 불변 객체

- 앞서 확인한 `객체 변경 방지` 메서드들은 모두 `얕은(Shallow) 변경 방지` 로 `직속 프로퍼티만 변경이 방지된다.`
- 즉, `중접 객체까지는 영향을 줄 수 없다.`
- 따라서, 한번의 `Object.freeze 메서드` 로 객체를 동결하여도 중첩 객체까지 동결할 수는 없다.

> 이를 해결하기 위해서는 `객체를 값으로 갖는 모든 프로퍼티` 에 `재귀(recursive)` 로 `Object.freeze 메서드를 적용` 한다.

```jsx
// 💩 중첩 객체가 있는 상황에서 얕은 객체 변경 방지(Shallow)
const person = {
  name: "WI",
  age: 100,
  address: {
    city: "Incheon",
  },
};

Object.freeze(person);
console.log(Object.isFrozen(person)); // true
console.log(Object.isFrozen(person.address)); // false << 🔎 중첩된 address 프로퍼티에 대해서는 freeze 되지 않았다.

person.address.city = "Seoul";
console.log(person); // { name: 'WI', age: 100, address: { city: 'Seoul' } } << 🔎 프로퍼티 값이 갱신되었다.

// 👍 깊은 객체 변경 방지(Deep)
function deepFreezen(target) {
  if (target && typeof target === "object" && !Object.isFrozen(target)) {
    // 일단 현재 들어온 객체를 동결시키고
    Object.freeze(target);

    // 내부에 있는 다른 프로퍼티 중, 중첩 객체들을 고려한 deepFrozen 함수 재귀실행
    // Object.keys 메서드 = 객체 자신의 키들에 대해 열거가능한(enumerable) 형태의 배열을 반환
    Object.keys(target).forEach((key) => deepFreezen(target[key]));
  }
}

const person = {
  name: "WI",
  age: 100,
  address: {
    city: "Incheon",
  },
};

deepFreezen(person);

console.log(Object.isFrozen(person)); // true
console.log(Object.isFrozen(person.address)); // true << 🔎 깊은 객체 변경 방지를 통해, 중접된 객체에 대해서도 동결되었다.

person.address.city = "Seoul";
console.log(person); // { name: 'WI', age: 100, address: { city: 'Incheon' } } << 🔎 덕분에 중접 객체의 프로퍼티 값도 갱신되지 않게 되었다.
```
