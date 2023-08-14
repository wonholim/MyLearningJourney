# Mixin

- [x] Mixin이란
- [x] Mixin은 어떻게 작동하는가?
- [x] Constrained Mixins
- [x] Alternative Pattern
- [ ] Constraints
- [ ] 참고자료

---

<br/><br/>

## Mixin이란

---

<br/>

- 객체 지향 프로그래밍에서, 재사용 가능한 부품을 만들어서 사용하면, 확장성을 높일 수 있다.
- 이와 같이 Mixin은 여러 클래스간에 메서드를 공유하기 위한 방법 중 하나이다.
- Mixin을 사용하면 여러 객체 간에 코드를 재사용할 수 있게 해준다.
- 즉, 특정 기능을 모듈화하여 여러 클래스에서 혼합할 수 있도록 해준다.

<br/>

## Mixin은 어떻게 작동하는가?

---

<br/>

- 클래스를 혼합하여 더 유여하고 재사용가능한 코드 패턴을 제공한다.
- 클래스가 다른 클래스로 부터 상속을 받는 대신, 모듈화된 기능이나 메서드를 가져와 하나의 클래스로 결합한다.
- `Mixin 패턴은 클래스를 확장하기 위해, 클래스 상속과 제네릭에 의존하며, 타입스크립트는 클래스 표현식 패턴을 이용한다.`

- 먼저, Mixin을 적용할 클래스를 생성한다.

```ts
class Sprite {
  name = "";
  x = 0;
  y = 0;
 
  constructor(name: string) {
    this.name = name;
  }
}

기본 클래스를 확장하고 클래스 표현식을 반활할 타입과 팩토리 함수가 필요하다.

다른 클래스에서 확장할 타입이 필요한데, 주로, 책임이 전달되는 타입이 클래스임을 선언한다.
type Constructor = new (...args: any[]) => {};

이 Mixin은 스케일 속성을 추가하며, 캡슐화된 private 속성을 사용해서, 변경할 수 있는 getter와 setter가 있다.
function Scale<TBase extends Constructor>(Base: TBase) {
  return class Scaling extends Base {
    mixin은 원래 private/protected 속성을 선언할 수 없지만,
    ES2020 이후는 private 필드를 사용할 수 있다.
    _scale = 1;
    
    setScale(scale: number) {
      this._scale = scale;
    }
 
    get scale(): number {
      return this._scale;
    }
  };
}

이후 Mixin이 적용된 클래스 나태나는 클래스를 생성할 수 있다.

Sprite 클래스에서, Mixin Scale 적용자와 새로운 클래스를 생성한다.
// Scaling 클래스는 ()안에 있는 클래스를 베이스로 상속을 받는다.
const EightBitSprite = Scale(Sprite);
 
const flappySprite = new EightBitSprite("Bird");
flappySprite.setScale(0.8);
console.log(flappySprite.scale);
```

<br/>

## Constrained Mixins

---

<br/>

- Mixin을 이용해서 제한된 클래스 타입만을 이용해 리턴 시켜줄 수 있다.
- 이전 코드에서 생성자 타입에 제네릭 인자를 받아들일 수 있도록 수정한다.

```ts
// 이전 생성자 타입
type Constructor = new (...args: any[]) => {};
// 제네릭을 사용해서, 제약을 걸어줄 예정
type GConstructor<T = {}> = new (...args: any[]) => T;

이렇게 된다면, 제약된 베이스 클래스와 함께 작동하는 클래스를 생성할 수 있다.

type Positionable = GConstructor<{ setPos: (x: number, y: number) => void }>;
type Spritable = GConstructor<Sprite>;
type Loggable = GConstructor<{ print: () => void }>;

이후, 특정 베이스를 기반으로 작동하는 Mixin을 만들 수 있게된다.

function Jumpable<TBase extends Positionable>(Base: TBase) {
  return class Jumpable extends Base {
    jump() {
      // 이 믹스인은 setPos가 정의된 베이스 클래스를 전달받아야만 작동한다.
      // Positionable 제약 때문이다.
      this.setPos(0, 20);
    }
  };
}
```

<br/>

## Alternative Pattern

---

<br/>

- Mixin문서에서 과거 문서에는 런타임과 타입 계층을 따로 생성한 뒤, 마지막에 병합하는 방식으로 Mixin을 작성하는 방법을 추천했다.

```ts
// 각 믹스인은 전통적인 ES 클래스이다.
class Jumpable {
  jump() {}
}

class Duckable {
  duck() {}
}

// 베이스를 클래스합니다.
class Sprite {
  x = 0;
  y = 0;
}

// 그런 다음 인터페이스를 생성하여 기대되는 믹스인과 베이스의 동일한 이름을 병합한다.
interface Sprite extends Jumpable, Duckable {}
// JS 런타임에서 믹스인을 베이스 클래스에 적용한다.
applyMixins(Sprite, [Jumpable, Duckable]);

let player = new Sprite();
player.jump();
console.log(player.x, player.y);

// 이 코드는 코드베이스의 어느 곳에서나 사용할 수 있는 함수를 작성하는 것이다.
// 클래스들을 가져와 적용한다.
declare function applyMixins(derivedCtor: any, constructors: any[]): void;
```

- 하지만, 이 패턴의 가장 큰 문제는 제네릭을 사용한 Mixin보다 까다롭다는 것이다.
- 제네릭 사용을 권장한다.

<br/>

## Constraints

---

<br/>

<br/>

## 참고자료

---

<br/>

<br/>
