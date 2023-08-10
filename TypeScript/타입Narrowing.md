# 타입Narrowing

- [x] 타입 narrowing의 정의 및 중요성
- [x] 타입 추론의 기본 개념
- [x] 타입 narrowing 없이 발생할 수 있는 문제점
- [x] Type Guards 소개와 typeof
- [x] Truthiness checking
- [x] Equality narrowing
- [x] The in operator narrowing
- [x] instanceof narrowing
- [x] Assignments
- [x] Control flow analysis
- [x] Using type predicates
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

## instanceof narrowing

---

</br>

- 자바스크립트에는 값이 다른 값의 인스턴스인지 확인하는 연산자가 존재한다. 다음의 예시를 보며 이해해보자.

```ts
function logValue(x: Date | string) {
  if (x instanceof Date) {
    console.log(x.toUTCString()); // x는 Date
  } else {
    console.log(x.toUpperCase()); // x는 string
  }
}

x instanceof Foo는 x의 프로토타입 체인에 Foo.prototype이 포함되어있는지 확인한다.
즉 new 로 구성할 수 있는 대부분의 값에 유용하게 사용할 수 있으며, instanceof도 타입가드이며, 타입narrowing에 사용된다.
instanceof는 주로 클래스와 관련된 언어에서 자주 사용되는 것을 의미하므로, 그 목적에 맞게 사용하자.
```

</br>

## Assignments

---

</br>

- 타입스크립트는 변수에 값을 할당할 때, 할당의 오른쪽 부분을 살펴보고 왼쪽 부분을 적절하게 좁혀간다. 
- 다음의 예시를 보며 이해해보자

```ts
let x = Math.random() < 0.5 ? 10 : "hello world!"; // x는 문자열 혹은 숫자 타입
-> let x: string | number 추론
x = 1; // 숫자 1을 할당하면, x는 숫자 타입

console.log(x); // x의 타입은 숫자
-> let x: number
x = "goodbye!"; // 문자열 "goodbye!"를 할당하면, x는 문자열 타입
 
console.log(x); // x의 타입은 문자열
-> let x: string

x = true; // error Type 'boolean' is not assignable to type 'string | number'.
console.log(x);
-> let x: string | number 이기 때문
```

- 첫번째 항당 후에, x의 타입이 숫자로 바뀌었어도, x에 문자열을 할당할 수 있다. 즉 선언된 타입 시작할 때의 타입이 string | number 이기 때문이며, 할당 가능성은 항상 선언된 타입과 비교하여 확인된다.

</br>

## Control flow analysis

---

</br>

- 타입스크립트에서 특정 분기 내에서 어떻게 좁혀져가는지 배웠다. 하지만, 변수를 따라가며, if, while 문 등에서 타입 가드를 찾는 것보다 더 복잡한 과정이 발생하고 있다.
- 다음의 예시를 보면

```ts
function padLeft(padding: number | string, input: string) {
  if (typeof padding === "number") {
    return " ".repeat(padding) + input;
  }
  return padding + input;
}
padding 이 number 타입일 경우, return padding + input;에 도달할 수 없음을 알 수 있다.
즉 return padding + input;의 리턴 타입이 (number | string)에서 string으로 좁혀짐을 알 수 있다.
```

- 코드에서 도달 가능성을 기반으로 분석한 것을 제어 흐름 분석이라 하며, 타입 가드와 할당을 만날 때 흐름 분석을 사용해 타입을 좁혀간다.
- 변수가 분석될 때 제어의 흐름은 분기하고 다시 합쳐지며, 각각의 변수는 지점마다 다른 타입을 가질 수 있음을 알 수 있다.
- 다음의 코드를 보며 이해해보자.

```ts
function example() {
  let x: string | number | boolean;
  x = Math.random() < 0.5;
  console.log(x); // let x: boolean
  if (Math.random() < 0.5) {
    x = "hello";
    console.log(x); // let x: string
  } else {
    x = 100;
    console.log(x); // let x: number
  }
  return x; // let x: string | number
}
```

</br>

## Using type predicates

---

</br>

- 지금까지는 자바스크립트의 구조를 사용해서 타입을 좁혀왔지만, 때로는 코드 전반에서 타입의 변화를 직접 제어할 필요가 있을 수 있다.
- 사용자 정의 타입 가드를 정의하려면 반환 타입이 타입 예측인 함수를 정의하면 된다.
- 다음의 코드를 봐보자

```ts
function isFish(pet: Fish | Bird): pet is Fish {
  return (pet as Fish).swim !== undefined;
}

여기서 리턴 타입인 pet is Fish는 타입 예측이다. 예측의 형태는 parameterName is Type의 형태를 취하며, parameterName은 함수 서명의 매개변수 이름이어야한다.
isFish에 매개변수와 함께 호출될 때, 타입스크립트는 원래 타입이 호환되는 경우 해당 변수를 특정한 타입으로 좁힌다.

let pet = getSmallPet();

if (isFish(pet)) {
  pet.swim();
} else {
  pet.fly();
}

직접 정의한 타입 가드로 타입 스크립트는 if문의 분기에서 pet이 fish임을 알고 있고, else에서는 Fish가 아니므로, Bird임을 알 수 있다. 

또한 타입을 기준으로 필터링 하는 경우도 사용이 가능하다.
const zoo: (Fish | Bird)[] = [getSmallPet(), getSmallPet(), getSmallPet()];
const underWater1: Fish[] = zoo.filter(isFish);
// 또는 동등하게
const underWater2: Fish[] = zoo.filter(isFish) as Fish[];

// 더 복잡한 예제의 경우 예측이 반복될 필요가 있을 수 있습니다
const underWater3: Fish[] = zoo.filter((pet): pet is Fish => {
  if (pet.name === "sharkey") return false;
  return isFish(pet);
});

타입 가드 isFish를 사용해서, Fish | Bird 배열을 필터링하고, Fish 배열을 얻는 코드이다.
```

- 또한 클래스 내에서 this를 기반으로한 타입 가드를 정의할 수 있다.

```ts
class FileSystemObject {
  isFile(): this is FileRep {
    return this instanceof FileRep;
  }
  isDirectory(): this is Directory {
    return this instanceof Directory;
  }
  isNetworked(): this is Networked & this {
    return this.networked;
  }
  constructor(public path: string, private networked: boolean) {}
}
 
class FileRep extends FileSystemObject {
  constructor(path: string, public content: string) {
    super(path, false);
  }
}
 
class Directory extends FileSystemObject {
  children: FileSystemObject[];
}
 
interface Networked {
  host: string;
}
 
const fso: FileSystemObject = new FileRep("foo/bar.txt", "foo");
 
if (fso.isFile()) {
  fso.content;
  
const fso: FileRep
} else if (fso.isDirectory()) {
  fso.children;
  
const fso: Directory
} else if (fso.isNetworked()) {
  fso.host;
  
const fso: Networked & FileSystemObject
}
```

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