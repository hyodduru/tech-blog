---
title: '[테스트 주도 개발] 1-3 모두를 위한 평등'
date: '2023-06-13'
tags: ['테스트 주도 개발', 'Test-Driven Development', '화폐 예제']
draft: false
summary: '삼각측량이란?

만약 라디오 신호를 두 수신국이 감지하고 있을 때, 수신국 사이의 거리가 알려져 있고 각 수신국이 신호의 방향을 알고 있다면 이 정보들만으로 충분히 신호의 거리와 방위를 알 수 있다. -> 예제가 두 개 이상 있어야만 코드를 일반화할 수 있다.
'
---

## 1장 - 3. 모두를 위한 평등 feat. 삼각측량 방법에 대하여

### 삼각측량이란?

만약 라디오 신호를 두 수신국이 감지하고 있을 때, 수신국 사이의 거리가 알려져 있고 각 수신국이 신호의 방향을 알고 있다면 이 정보들만으로 충분히 신호의 거리와 방위를 알 수 있다. -> <strong>예제가 두 개 이상 있어야만 코드를 일반화할 수 있다.</strong>

삼각측량으로 테스트를 통과시켜보기 위해 우선 두 개의 예제를 만들어보자.

```java
Dollar {
    public void testEquality(){
        assertTrue(new Dollar(5).equals(new Dollar(5)));
        assertTrue(new Dollar(5).equals(new Dollar(6)));
    }
}
```

이제 동치성(equality)를 일반화해보면 아래와 같다.

```java
Dollar {
    public boolean equals(Object object){
       Dollar dollar = (Dollar) object;
       return amount === dollar.amount;
    }
}
```

### 지금까지 한일 정리

- 우리의 디자인 패턴(값 객체)이 하나의 또 다른 오퍼레이션을 암시한다는 걸 알아챘다.
- 해당 오퍼레이션을 테스트했다.
- 해당 오퍼레이션을 간단히 구현했다.
- 곧장 리팩토링하는 대신 테스트를 조금 더 했다.
- 두 경우를 모두 수용할 수 있도록 리팩토링했다.
