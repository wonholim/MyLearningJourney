# 타입Narrowing

- [x] 타입 narrowing의 정의 및 중요성
- [x] 타입 추론의 기본 개념
- [x] 타입 narrowing 없이 발생할 수 있는 문제점
- [x] Type Guards 소개와 typeof
- [x] Truthiness checking
- [x] Equality narrowing
- [x] The in operator narrowing
- [ ] instanceof narrowing
- [ ] Assignments
- [ ] Control flow analysis
- [ ] Using type predicates
- [ ] Assertion functions
- [ ] Discriminated unions
- [ ] The never type
- [ ] Exhaustiveness checking
- [ ] 실제 코드에서 타입 narrowing가 어떻게 적용되는지
- [ ] 타입 narrowing을 지원하는 도구와 라이브러리
- [ ] 출처 

---

</br></br>

## 타입 narrowing의 정의 및 중요성

---

</br>

- 타입 narrowing은 특정 변수의 가능한 타입 범위를 좁혀서, 구체적으로 어떤 타입인지, 변환하는 과정을 말한다.
- 컴파일러가 더 정확한 정보를 얻게되어 코드의 안정성과 정확성을 향상시킬 수 있다.
- 예를 들어, 변수의 타입이 `string | number` 인 경우 하나의 조건을 걸어 string, number로 좁힐 수 있다.
- 즉, 덜 정확한 타입에서 더 정확한 타입으로 변화하는 과정을 타입 narrowing라 한다.

</br>

## 타입 추론의 기본 개념

---

</br>

- 우선, 타입 narrowing을 하기위해선, 타입스크립트가 타입을 추론해나가는 과정에대해 알고있어야한다.
- 타입 추론이란, 타입스크립트가 코드를 해석해 나가는 동작을 의미하며, 가장 기본적인 타입 추론은 다음과 같다.

```ts
let x = 3;
다음과 같이 x에 대한 타입을 따로 지정하지 않아도, x의 타입은 number로 추론한다.
변수를 선언하거나, 초기화할 때 타입이 추론되며 이외에도 변수, 속성, 인자의 기본 값, 함수의 반환 값 등을 설정할 때 타입 추론이 일어난다.
```

- Best Common Type : 여러 표현식에서 타입을 추론할 때, 표현식들의 타입을 이용해 최적의 공통 타입을 추론한다.

```ts
let arr = [0, 1, null]; 일경우, 타입스크립트의 가장 일반적인 타입 알고리즘은 각 후보 타입을 고려해서, 모든 후보 타입들과 호환되는 타입을 결정한다.
let arr: (number | null)[] = [0, 1, null]; 과 같이 선언된다.
```

- 하지만, 꼭 모든 후보 타입을 가지는 슈퍼 타입이 아닌 경우도 존재한다.

```ts
동물원이라는 타입은 사자, 호랑이, 뱀의 슈퍼 타입이라고 가정했을 때 다음과 같은 경우, 동물원 타입을 추론하지 않는다.
let zoo = [new 사자(), new 호랑이(), new 뱀()];
-> let zoo: (사자 | 호랑이 | 뱀)[]의 타입을 가진다.
다음과 같은 경우, 동물원이라는 타입을 명시적으로 제공해야한다.
let zoo: 동물원[] = [new 사자(), new 호랑이(), new 뱀()];
-> let zoo: 동물원[]
```

- 최적 공통 타입이 발견되지 않는다면, 추론 결과는 유니온한 배열 타입을 가지게 된다.

- 타입 스크립트가 타입을 추론하는 또 하나의 방식은 바로, 문맥상으로 타입을 결정하는 것인데, 표현식의 타입이 위치에 의해 암시 될 때 발생한다.

```ts
window.onmousedown = function (mouseEvent) {
  console.log(mouseEvent.button); //<- OK
  console.log(mouseEvent.kangaroo); //<- Error!   
};

타입스크립트는 window.onmousedown 함수 타입을 사용해, 오른쪽에 할당된 함수 표현식의 타입을 추론했다.
mouseEvent는 button 프로퍼티가 존재하므로, mouseEvent 매개변수의 타입을 추론할 수 있지만, kangaroo는 포함하지 않으므로, 에러가 발생한다. 
```

- 중요한점은 함수가 컨텍스트 타입 위치에 있지 않을 때, `--noImplicitAny` 옵션을 사용하지 않는다면, 함수의 인수의 타입은 `any`로 암묵적으로 적용된다.

```ts
--noImplicitAny를 사용하지 않는 경우
const handler = function (uiEvent) { -> any타입
  console.log(uiEvent.button); //<- OK
};

또한 타입 정보를 제공해서, 컨텍스트 타입을 재정의 할 수 있다.
window.onscroll = function (uiEvent: any) { -> 타입 재정의
  console.log(uiEvent.button); //<- Now, no error is given 하지만, undefined를 제공 프로퍼티가 없으므로
};
```

</br>

## Type Guards 소개와 typeof

---

</br>

- 자바스크립트는 실행 시점에서, 가지는 값들의 매우 기본적인 타입 정보를 제공하는 typeof 연산자를 지원한다.
> `string`, `number`, `bigint`, `boolean`, `symbol`, `undefined`, `object`, `function`

- 보고나서, 이상한 점이 있을 것이다. 위의 목록에서 `null`이 빠져있음을 알 수 있다.
- 사실 typeof null을 하게되면, `object 타입`이라고 알려준다. 

- 다음의 예제를 한번 봐보자

```ts
function printAll(strs: string | string[] | null) {
  if (typeof strs === "object") {
    for (const s of strs) {
      //'strs' is possibly 'null'.
      console.log(s);
    }
  } else if (typeof strs === "string") {
    console.log(strs);
  } else {
    // do nothing
  }
}

null일 경우 else로 넘어가서 처리되는게 맞지만, 실제론 object 타입이므로 TypeError: null is not iterable이 출력이 되는 불상사가 생겨난다.
이는 자바스크립트의 오래된 오류로, 타입 스크립트에서는 이 오류를 인식하고, 예상하지 못한 오류를 방지하기 위해 타입을 더 정확하게 narrowing해준다.
```

</br>

## Truthiness checking

---

</br>

- 자바스크립트에서는 조건문, && 연산자, || 연산자, if문, 불린, ! 등에서 어떤 표현식도 사용할 수 있다.
- 예를 들어, if문의 조건이 항상 boolean 타입일 것으로 기대하지 않는 것처럼 말이다.

```ts
function getUsersOnlineMessage(numUsersOnline: number) {
  if (numUsersOnline) {
    return `There are ${numUsersOnline} online now!`;
  }
  return "Nobody's here. :(";
}
```

- 자바스크립트에서는 if와 같은 구조가 먼저 조건을 boolean으로 강제 변환(coerce)하여, 이해하고, 그 결과가 참인지 거짓인지에 따라 분기를 선택한다.
- 자바스크립트에서는 `truthy`와 `falsy`를 이용해, 조건문에서 진리 값을 평가한다.
- `falsy`는 거짓으로 평가되는 값으로, 조건문이나 논리 연산자에서 진리 값을 검사할 때, 자동으로 false로 변환된다.
> `0`, `NaN`, `""`, `0n` -> bigint의 0, `null`, `undefined`
- `truthy`는 참으로 평가되는 값으로, 조건문이나 논리 연산자에서 진리 값을 검사할 때, 자동으로 true로 변환된다.
>  falsy의 값을 제외한 모든 값

- 자바스크립트는 불린 타입 대신 Truthiness checking을 한다. 즉, 특정값이 True인지, False인지 판별하기 위함이다.
- 타입스크립트는 이런 런타임 동작을 기반으로 타입을 추론한다. !! 연산자를 사용할 경우, 타입스크립트는 더욱 구체적인 불린 리터럴 타입을 추론할 수 있다.

```ts
// both of these result in 'true'
Boolean("hello"); // type: boolean, value: true
!!"world"; // type: true,    value: true

Boolean으로 감싸는 경우 타입은 불린이고, 값은 True이다.
!! 연산자를 사용하는 경우, 타입은 true이고 값은 true이다.
조금 더 타입에 대해 구체적으로 추론이 한다는 점이 다르다.
```

- null 또는 undefined와 같은 값에 대한 보호로, 이러한 동작을 활용하는 것은 인기가 많다. 예를들면 다음과 같다.

```ts
function printAll(strs: string | string[] | null) {
  if (strs && typeof strs === "object") {
    for (const s of strs) {
      console.log(s);
    }
  } else if (typeof strs === "string") {
    console.log(strs);
  }
}

strs가 Truthiness checking을 하여, TypeError: null is not iterable이 발생하는 상황을 없앨 수 있다.
하지만 종종 원시 값에 대한 Truthiness checking에서 오류가 발생할 수 있다.

그러면 함수의 전체를 Truthiness checking로 감싸면 어떻게 될까?

function printAll(strs: string | string[] | null) {
  // !!!!!!!!!!!!!!!!
  //  DON'T DO THIS!
  //   KEEP READING
  // !!!!!!!!!!!!!!!!
  if (strs) {
    if (typeof strs === "object") {
      for (const s of strs) {
        console.log(s);
      }
    } else if (typeof strs === "string") {
      console.log(strs);
    }
  }
}

아쉽지만, ""인 빈 문자열이 통과하지 못하게 된다. 이 점을 주의하자.

function multiplyAll(values: number[] | undefined, factor: number): number[] | undefined {
  if (!values) {
    return values;
  } else {
    return values.map((x) => x * factor);
  }
}

위와 같이 !을 이용해, 연산을 뒤집어 줄 수 있다.
```

- 자바스크립트에서는 `truthy`와 `falsy`를 이용해, 조건문에서 진리 값을 평가한다.
- `falsy`는 거짓으로 평가되는 값으로, 조건문이나 논리 연산자에서 진리 값을 검사할 때, 자동으로 false로 변환된다.

- `truthy`는 참으로 평가되는 값으로, 조건문이나 논리 연산자에서 진리 값을 검사할 때, 자동으로 true로 변환된다.


</br>

## Equality narrowing

---

</br>

- 타입스크립트는 `===`, `!==`, `==`, `!=`와 같은 switch 문과 동등성 검사를 사용해 타입 narrowing을 한다.
- 코드를 살펴 보면 다음과 같다.

```ts
function example(x: string | number, y: string | boolean) {
  if (x === y) {
    // We can now call any 'string' method on 'x' or 'y'.
    x.toUpperCase(); // (method) String.toUpperCase(): string
    y.toLowerCase(); // (method) String.toLowerCase(): string
  } else {
    console.log(x); // (parameter) x: string | number
    console.log(y); // (parameter) y: string | boolean
  }
}

x와 y가 공통되는 타입은 string이므로, ===을 통해 서로 공통 된다면, string타입만 사용가능한 프로퍼티를 적용하고, 그 외에는 출력을 한다.
또한 변수가 아닌 특정 리터럴 값과의 비교도 작동한다.

Truthiness checking에서 사용 했던 printAll을 동등성 검사를 이용해서, 빈 문자열 문제를 해결할 수 잇다.

function printAll(strs: string | string[] | null) {
  if (strs !== null) {
    if (typeof strs === "object") {
      for (const s of strs) { // (parameter) strs: string[]
        console.log(s);
      }
    } else if (typeof strs === "string") {
      console.log(strs); // (parameter) strs: string
    }
  }
}
```

- 주의할 점은 `== null`을 사용하면 값이 정확히 `null`인지 확인하는게 아닌, `undefined`인지 확인한다는 점이다.
- `== undefined`또한 마찬가지로 `null`인지 추가적으로 확인하므로, 가능한 경우 엄격한 동등성 검사인 `===`과 `!==`을 사용해야한다.
- 다음의 예제를 보면, 쉽게 이해가 가능하다.

```ts
interface Container {
  value: number | null | undefined;
}
 
function multiplyValue(container: Container, factor: number) {
  // Remove both 'null' and 'undefined' from the type.
  if (container.value != null) {
    console.log(container.value); // (property) Container.value: number
 
    // Now we can safely multiply 'container.value'.
    container.value *= factor;
  }
}
```

</br>

## The in operator narrowing

---

</br>

- 자바스크립트에는 객체나 프로토타입 체인이 특정 이름의 속성을 가지고 있는지 확인하는 연산자가 존재한다. 바로 `in`이다.
- 타입스크립트에서는 타입 narrowing 방법으로 고려한다.
- 다음의 예제를 봐보자.

```js
type Fish = { swim: () => void };
type Bird = { fly: () => void };
 
function move(animal: Fish | Bird) {
  if ("swim" in animal) {
    return animal.swim();
  }
 
  return animal.fly();
}

if ("swim" in animal)에서 "swim"은 문자열 리터럴이고, animal은 Fish와 Bird 유니온 타입이다.
"swim"을 변동되는 값이라고 했을 때, animal안의 속성 값으로 타입을 좁힐 수 있다.
그러면 만약 수영과 비행을 둘다할 수 있는 인간이 추가되면 어떻게 나누게 될까?

type Fish = { swim: () => void };
type Bird = { fly: () => void };
type Human = { swim?: () => void; fly?: () => void };
 
function move(animal: Fish | Bird | Human) {
  if ("swim" in animal) {
    animal; // (parameter) animal: Fish | Human
  } else {
    animal; // (parameter) animal: Bird | Human
  }
}

이전에서 인간만 추가해보면, 인간은 모두 할수있기 때문에, 양쪽 선택지에 모두 추가된다.
이 점에 유의해서, 특정 타입에 대해, 어떤 속성이 선택적으로 존재할 수 있는지, in을 사용해서 유니온 타입의 멤버를 좁힐 수 있는지 잘 판단해야한다.
인간 타입의 경우, 타입을 정확하게 좁히기 어려울 수 있다.
```

</br>

## Assignments

---

</br>

</br>

## Type Guards 소개

---

</br>

</br>

## Control flow analysis

---

</br>

</br>

## Using type predicates

---

</br>

</br>

## Assertion functions

---

</br>

</br>

## Discriminated unions

---

</br>

</br>

## The never type

---

</br>

</br>

## Exhaustiveness checking

---

</br>

</br>

## 실제 코드에서 타입 narrowing가 어떻게 적용되는지

---

</br>

</br>


## 타입 narrowing을 지원하는 도구와 라이브러리

---

</br>

</br>


## 출처

> 