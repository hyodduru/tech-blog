---
title: '[테스트 주도 개발] 1-5 솔직히 말하자면, 1-6 돌아온 모두를 위한 평등'
date: '2023-06-15'
tags: ['테스트 주도 개발', 'Test-Driven Development', '화폐 예제']
draft: false
---

## 1장 - 5. 솔직히 말하자면

### 해결해야 할 문제

`$5 + 10CHF = $10(환율이 2:1일 경우)`

위의 문제를 바로 해결하려고 접근하기 보다는, 작은 단계에서부터 시작을 해보자.

Dollar 객체와 비슷하지만 달러 대신 프랑(Franc)이라는 객체를 만들어서 테스트를 해보자.

기존의 테스트를 복사한 후 수정해보면,

```java
public void testMultiplication(){
    Franc five = new Franc(5);
    assertEquals(new Franc(10), five.times(2));
    assertEquals(new Franc(15), five.times(3));
}
```

다시 한번 반복해서 기억하는 테스트 주기

> 1. 테스트 작성
> 2. 컴파일되게 하기
> 3. 실패하는지 확인하기 위해 실행
> 4. 실행하게 만듦
> 5. 중복 제거

처음 네 단계는 빠르게. 그리고 마지막 단계를 통해 적절한 시기에 적절한 설계를 하도록 하자.

```java
// Franc
class Franc {
    private int amount;

    Franc(int amount){
        this.amount = amount;
    }

    Franc times(int multiplier){
        return new Franc(amount * multiplier);
    }

    public boolean equals(Object object){
        Franc franc = (Franc) object;
        return amount == franc.amount;
    }
}
```

코드를 실행시키기까지의 단계가 짧았기 때문에 '컴파일되게 하기' 단계도 넘어갈 수 있었다. 중복이 엄청나게 많기 때문에 다음 테스트를 작성하기 전에 이것들을 제거해야함. equals()를 일반화하는 것부터 시작할 수 있다.

### 지금까지 한일 정리

- 큰 테스트를 공략할 수 없다. 그래서 진전을 나타낼 수 있는 자그마한 테스트를 만들었다.
- 뻔뻔스럽게도 중복을 만들고 조금 고쳐서 테스트를 작성했다.
- 설상가상으로 모델 코드까지 도매금으로 복사하고 수정해서 테스트를 통과했다.
- 중복이 사라지기 전에는 집에 가지 않겠다고 약속했다.

## 1장 - 6. 돌아온 '모두를 위한 '평등'

### 해결해야 하는 문제

중복 코드 제거하기!

### 시도해볼 수 있는 방법

우리가 만든 클래스 중 하나가 다른 클래스를 상속받게 하는 것

Money라는 클래스가 공통의 equals를 갖는다면 어떨까?

```java
class Money;

class Dollar extends Money{
    private int amount;
}
```

이제 amount 인스턴스 변수를 Money로 옮겨보자

```java
class Money {
    protected int amount
}

class Dollar extends Money{
}
```

이제 equals() 코드를 위로 올려보면,

```java
// Dollar
public boolean equals(Object object){
    Money dollar = (Money) object;
    return amount == dollar.amount;
}
```

임시 변수의 이름도 변경해보자 (원활한 의사소통을 위해)

```java
// Dollar
public boolean equals(Object object){
    Money money = (Money) money;
    return amount == dollar.amount;
}
```

이제 위의 메서드를 Dollar에서 Money로 옮길 수 있다.

이제 Franc.amount() 도 위와 동일한 방식으로 제거해주면 된다.

### 지금까지 한일 정리

- 공통된 코드를 첫 번째 클래스(Dollar)에서 상위 클래스(Money)로 단계적으로 옮김
- 두 번째 클래스(Franc)도 Money의 하위 클래스로 만듬
- 불필요한 구현을 제거하기 전에 두 equals()의 구현을 일치시킴
