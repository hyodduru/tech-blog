---
title: '[테스트 주도 개발] 1-9 우리가 사는 시간'
date: '2023-06-20'
tags: ['테스트 주도 개발', 'Test-Driven Development', '화폐 예제']
draft: false
---

## 1장 - 9. 우리가 사는 시간

### 해결해야 할 문제

- times 중복 제거하기

우선 Money 객체에 currency 메서드를 선언하자

```js
class Money {
  constructor(amount) {
    this.amount = amount
    this.currency = currency
  }

  dollar = () => {
    return new Dollar()
  }

  currency = () => {
    return this.currency
  }
}
```

그리고 두 하위 클래스에서 이를 구현하자

```js
class Franc {
  currency = () => {
    return 'CHF'
  }
}

class Dollar {
  currency = () => {
    return 'USD'
  }
}
```

우린 두 클래스를 모두 포함할 수 있는 동일한 구현을 원하기 때문에, 통화를 인스턴스 변수에 저장하고, 메서드에서는 그냥 그걸 반환하게 만들 수 있을 것 같다.

```js
class Franc {
  constructor(amount) {
    this.amount = amount
    this.currency = 'CHF'
  }

  currency = () => {
    return this.currency
  }
}

class Dollar {
  constructor(amount) {
    this.amount = amount
    this.currency = 'USD'
  }

  currency = () => {
    return this.currency
  }
}
```

이렇게 되면 두 currency()가 동일하므로 변수 선언과 currency 둘 다 상위 클래스로 옮길 수가 있다.

```js
class Money {
  constructor(amount, currency) {
    this.amount = amount
    this.currency = currency
  }

  currency = () => {
    return this.currency
  }
}
```

```js
class Money {
  constructor(amount) {
    this.amount = amount
    this.currency = currency
  }

  dollar() {
    return new Dollar(this.amount, 'USD')
  }

  franc() {
    return new Franc(this.amount, 'CHF')
  }

  currency = () => {
    return this.currency
  }
}
```

상위 클래스인 Money로 부터 상속받아서 currency를 쓸 수 있다.

```js
class Dollar extends Money {
  constructor(amount, currency) {
    super(amount, currency)
  }
}
```

### 지금까지 한 일 정리

- 다른 부분들을 호출자(팩토리 메서드)로 옮김으로써 두 생성자를 일치시켰다.
- times()가 팩처리 메서드를 사용하도록 만들기 위해 리팩토링을 잠시 중단했다.
- 비슷한 리팩토링(Franc에 했던 일을 Dollar에도 적용)을 한번의 큰 단계로 처리했다.
- 동일한 생성자들을 상위 클래스로 옮겼다.
