---
title: 테스트 주도 개발의 장점 
date: '2023-05-20'
tags: ['테스트 주도 개발', 'Test-Driven Development']
draft: false
summary: '켄트 벡의 테스트 주도 개발이라는 책으로 TDD에 대한 공부를 제대로 해보기로 했다. 회사에서 테크리드 분께서 추천해주신 책이다. 회사에서 TDD 방식의 테스트 자동화 환경에서 개발을 하고는 있지만, 제대로 된 사전 지식 없이 바로 실전에 투입되서 개발을 하고 있어서 내가 하고 있는게 명확히 무엇인지 개념이 바로잡혀있지 않고, 이렇게 테스트 코드를 짜는 것이 맞을까에 대한 확신이 부족하다. 개발을 할 때 깔끔한 코드를 작성하기 위해 늘 고민하는 개발자가 되기 위해 공부하기로 했다. 
'
---

켄트 벡의 테스트 주도 개발이라는 책으로 TDD에 대한 공부를 제대로 해보기로 했다. 회사에서 테크리드 분께서 추천해주신 책이다. 회사에서 TDD 방식의 테스트 자동화 환경에서 개발을 하고는 있지만, 제대로 된 사전 지식 없이 바로 실전에 투입되서 개발을 하고 있어서 내가 하고 있는게 명확히 무엇인지 개념이 바로잡혀있지 않고, 이렇게 테스트 코드를 짜는 것이 맞을까에 대한 확신이 부족하다. 개발을 할 때 깔끔한 코드를 작성하기 위해 늘 고민하는 개발자가 되기 위해 공부하기로 했다.

해당 포스트에서는 본문에 들어가기 전에 테스트 주도 개발에 대해 간략히 설명한 글들을 요약해놓은 글이다.

# 테스트 주도 개발의 장점

## 작동하는 깔끔한 코드(clean code that works)

## 깔끔한 코드가 훌룡한 목표인 이유?

- 예측 가능한 개발 방법
- 코드가 가르쳐주는 모든 교훈을 학습할 기회를 갖게 된다.
- 작성하는 동안 기분이 좋다.

깔끔한 코드를 얻기 위해서는 자동화된 테스트로 개발을 이어갈 수 있다.

## 테스트 코드의 2가지 규칙

- 오직 자동화된 테스트가 실패할 경우에만 새로운 코드를 작성
- 중복을 제거

## 위의 테스트 코드 규칙으로 인해 만들어지는 행동 패턴

- 매 결정사항에 대해 피드백을 제공하는 실행 가능한 코드를 기반으로 하는 유기적인 설계
- 직접 테스트 작성
- 개발 환경은 작은 변화에도 빠르게 반응할 수 있어야 한다.
- 테스트를 쉽게 만들려면 반드시 응집도는 높고 결합도는 낮은 컴포넌트들로 구성되게끔 설계해야함. (이게 무슨 말일까?)

## 이 책을 다 읽고 나면

- 단순하게 시작하고
- 자동화된 테스트를 만들고
- 새로운 설계 결정을 한 번에 하나씩 도입하기 위해 리팩토링을 할 준비가 될 것
