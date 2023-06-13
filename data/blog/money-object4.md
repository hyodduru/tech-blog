---
title: '[테스트 주도 개발] 1-4 프라이버시'
date: '2023-06-14'
tags: ['테스트 주도 개발', 'Test-Driven Development', '화폐 예제']
draft: false
---

## 1장 - 4. 프라이버시

### 해결해야 할 문제

amount를 private로 만들기

#### 기존의 코드

```java
public void testMultiplication(){
    Dollar five = new Dollar(5);
    Dollar product = five.times(2);
    assertEquals(10, product.amount);
    product = five.times(3);
    assertEquals(15, product.amount);
}
```

단언들을 Dollar와 Dollar를 비교하는 것으로 재작성 할 수 있다.

```java
public void testMultiplication(){
    Dollar five = new Dollar(5);
    Dollar product = five.times(2);
    assertEquals(new Dollar(10), product);
    product = five.times(3);
    assertEquals(new Dollar(15), product);
}
```

이렇게 되면 임시 변수인 product는 더 이상 쓸모가 없게 된다.

인라인시켜보면 아래와 같다.

```java
public void testMultiplication(){
    Dollar five = new Dollar(5);
    assertEquals(new Dollar(10), five.times(2));
    assertEquals(new Dollar(15), five.times(3));
}
```

위의 방식으로 수정함으로써, 일련의 오퍼레이션이 아니라 참인 명제에 대한 단언들이므로 우리의 의도를 더 명확하게 이야기해준다.

테스트를 고치고 나니 Dollar의 amount 인스턴스 변수를 사용하는 코드는 Dollar 자신밖에 없게 되어, 변수를 private으로 변경할 수 있게 되었다.

```java
// Dollar
private int amount;
```

### 지금까지 한일 정리

- 오직 테스트를 향상시키기 위해서만 개발된 기능을 사용했다.
- 두 테스트가 동시에 실패하면 망한다는 점을 인식했다.
- 위험 요소가 있음에도 계속 진행했다.
- 테스트와 코드의 결합도를 낮추기 위해, 테스트하는 객체의 새 기능을 사용했다.
