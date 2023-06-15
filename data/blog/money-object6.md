---
title: '[테스트 주도 개발] 1-7 사과와 오렌지, 1-8 객체 만들기'
date: '2023-06-16'
tags: ['테스트 주도 개발', 'Test-Driven Development', '화폐 예제']
draft: false
---

## 1장 - 7. 사과와 오렌지

### 해결해야 할 문제

- Franc과 Dollar 비교하기

두 객체의 클래스를 비교함으로써 테스트를 성공시켜보기
(오직 금액과 클래스가 서로 동일할 때만 두 Money가 서로 같은 것)

```java
// Money
public boolean equals(Object object){
    Money money = (Money) object;
    return amount == money.amount && getClasses().equals(money.getClass());
}
```

### 지금까지 한일 정리

- 완벽하진 않지만 그럭저럭 봐줄 만한 방법 (getClass())으로 테스트를 통과하게 만들었다.
- 더 많은 동기가 있기 전에는 더 많은 설계를 도입하지 않기로 했다.

## 1장 - 8. 객체 만들기

### 해결해야 할 문제

Dollar / Franc 중복

두 times() 구현 코드가 거의 똑같다.

```java
// Franc
Franc times(int multiplier){
   return new Franc(amount * multiplier);
}

// Dollar
Dollar times(int multiplier){
    return new Dollar(amount * multiplier);
}
```

양쪽 모두 Money를 반환하게 만들면 더 비슷하게 만들 수 있다.

```java
// Franc
Money times(int multiplier){
   return new Franc(amount * multiplier);
}

// Dollar
Money times(int multiplier){
    return new Dollar(amount * multiplier);
}
```

Money의 두 하위 클래스가 그다지 많은 일을 하지 않아서 제거해버리고 싶은데, 한번에 그렇게 큰 단계를 밟는 것은 TDD를 효과적으로 보여주기에 적절치 않다.

-> 하위 클래스에 대한 직접적인 참조를 줄임으로써 하위 클래스를 제거하기 위한 방향으로 가보자

> 이 때 Money에 Dollar를 반환하는 팩터리 메서드(factory method)를 도입할 수 있다.

<strong> 지금부터는 자바스크립트 문법으로 직접 바꾸어서 정리해보려고 한다(!)</strong>

아무튼 Dollar를 반환하는 Money 객체를 만들어보면 아래와 같다.

```js
class Money {
  constructor(amount) {
    this.amount = amount
  }
  dollar = () => {
    return new Dollar()
  }
}
```

이렇게 되면, 어떤 클라이언트 코드도 Dollar라는 이름의 하위 클래스가 있다는 사실을 알지 못한다.

하위 클래스의 존재를 테스트에서 분리(decoupling)함으로써 어떤 모델 코드에도 영향을 주지 않고 상속 구조를 마음대로 변경할 수 있게 됐다.

### 지금까지 한일 정리

- 동일한 메서드(times)의 두 변이형 메서드 서명부를 통일시킴으로써 중복 제거를 향해 한 단계 더 전진했다.
- 최소한 메서드 선언부만이라도 공통 상위 클래스(superclass)로 옮겼다.
- 팩토리 메서드를 도입하여 테스트 코드에서 콘크리트 하위 클래스 존재 사실을 분리했다.
- 하위 클래스가 사라지면 몇몇 테스트는 불필요한 여분의 것이 된다는 것을 인식했다.
