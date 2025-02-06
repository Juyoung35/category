# Part 1

## 1. 합성의 본질

인간의 본질 합성.

```haskell
(.) :: (b -> c) -> (a -> b) -> (a -> c)
(.) f g = (\a -> g (f a))
-- [associativity] h . (g . f) == (h . g) . f == h . g . f
id :: a -> a
id x = x
-- [identity]
--     f . id == f
--     id . f == f
```

## 2. 타입과 함수

[todo] 하스켈의 타입 시스템 우회 백도어 `unsafe.Coerce` 나중에 알아보기.

### 단위 테스트

> 단위 테스트가 강력한 타입을 대체할 수 있을까?
> 테스트는 항상 결정론적 프로세스라기보다는 확률론적 프로세스입니다. 테스트는 증명을 대신할 수 없습니다.

```haskell
f :: a -> a
f = undefined
-- _|_(bottom type)의 반환
```

[todo] Set이 아닌 Hask 범주 알아보기.

### 수학적 모델의 필요성

[todo] 언어의 semantics을 설명하는 도구로, operational semantics이 있음. 형식화된 이상 인터프리터 정의. "abstract machine"

연산 의미론을 사용해 프로그램에 대한 것을 증명하는 것을 매우 어려움.

-> 이에 대한 대안으로, 표기 의미론(denotational semantics)이 있음. 모든 프로그래밍 합성에 수학적 해석이 제공됨.

```haskell
-- 표기 의미론 예시
fact n = product [1..n]
```

> 표기 의미론은 동작 의미론 작업에 적합하지 않게 보였지만, 모나드를 이용해 구현 가능.

### 순수 함수와 비순수 함수

### 타입의 예시

```haskell
absurd :: Void -> a
-- ex falso sequitur quodlibet : 잘못된 전제에서 오는 모든 명제는 참이다.
-- 호출할 수 없는 함수, 반환 타입에서 다형성인 함수
```

```haskell
unit :: a -> ()
unit _ = ()
-- 매개변수에서 다형성인 함수
```

## 3. 크고 작은 범주

### 객체가 없는 범주

### 단순 그래프

그래프에 의해 생성된 '자유 범주'라고 함.

그러니까 그냥 free, 말그대로 무슨 의미는 없고 걍 만든 범주이다.. 정도로 해석하자.

### 순서
