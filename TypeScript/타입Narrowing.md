# 타입Narrowing

- [x] 타입 narrowing의 정의 및 중요성
- [x] 타입 추론의 기본 개념
- [x] 타입 narrowing 없이 발생할 수 있는 문제점
- [ ] Type Guards 소개
- [ ] typeof, instanceof, 사용자 정의 타입 가드
- [ ] 다양한 narrowing 사용
- [ ] Discriminated Unions 사용
- [ ] 실제 코드에서 타입 narrowing이 어떻게 적용되는지
- [ ] 타입 narrowing의 장점과 단점
- [ ] 타입 narrowing을 지원하는 도구와 라이브러리

---

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

## 타입 narrowing 없이 발생할 수 있는 문제점

---

</br>

- 타입 narrowing이 없다면, 발생할 수 있는 문제점은 다음과 같다.

```ts
type Cat = { name: string; 냐용: () => string };
type Dog = { name: string; 멍: () => string };

function 짖어(animal: Cat | Dog) {
  return animal.냐옹();
}

let cat: Cat = { name: "고양이", 냐옹: () => "냐옹" };
짖어(cat);

간단한 예시에서, 매개변수 animal은 고양이 타입과 강아지 타입을 통과시켜준다.
강아지는 멍 프로퍼티를 가지고
고양이는 냐옹 프로퍼티를 가진다.
하지만 return의 경우, 고양이만 가지는 냐옹을 돌려주므로, 강아지 타입이 들어올 경우 에러가 발생한다.
자바스크립트에서는 이런 상황이 자주 반복되었는데, 타입스크립트에서는 이를 해결하기 위해 타입 가드를 사용한다.
```

</br>

## Type Guards 소개

---

</br>

</br>

## typeof, instanceof, 사용자 정의 타입 가드

---

</br>

</br>

## 다양한 narrowing 사용

---

</br>

</br>

## Discriminated Unions 사용

---

</br>

</br>

## 실제 코드에서 타입 narrowing이 어떻게 적용되는지

---

</br>

</br>


## 타입 narrowing의 장점과 단점

---

</br>

</br>


## 타입 narrowing을 지원하는 도구와 라이브러리

---

</br>

</br>


## 출처

> 