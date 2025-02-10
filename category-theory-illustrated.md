# 0. intro

## abstract theory
집합론과 범주론이 다른 이론과 다른 것.
domain 밖에 적용 가능하다
그러나 시작할 자기 자신의 구체적인 domain은 없다

concrete theory는 구체적인 concept에서 컨셉을 따온다

# 1. set

## set :: collection of things(things는 아무거나 될 수 있음)
	set, subset, singleton set, empty set

## function :: relation between two sets.
	match each element of source to target.
// domain/codomain, input/output, argument type/return type, premise/conclusion
one-to-one
many-to-one 함수는 연산을 represent할 것이다: 대상의 모음을 기준에 따라 분류/분할하는 것, 그들이 가진 property에 따라.
정의역 모든 값들은 함수값 가져야.

"Q : How far are we from Net York?"는 함수이다. F : 모든 장소 -> 양의 실수
"Q : Who is my father?"
"Q : What is the target of this function?"

## identity function

"no matter what they represent" :: 추상화의 일면

어떤 집합 G이든, Id_G : G -> G 항등함수를 "정의할 수 있다"

```haskell
id :: a -> a
```

## functions and subsets

```haskell
image :: a -> a
image a = a
-- 유저가 집어넣는 값들이 결국 정의역이 되면서, 그 집합의 크기는 실제 공역의 부분집합일 것임.. 뇌피셜.
```

모든 집합에 대해서, 정의역이 부분집합(subset), 공역이 자기자신(superset)이며 같은 원소로 대응시키는 함수 (상, image)를 정의할 수 있다.

## functions and the empty set

```haskell
absurd :: Void -> a
```

> 공집합에서 임의의 집합으로 가는 함수가 존재하는가? Yes. 공집합은 모든 집합의 부분집합.

## functions and singleton sets

```haskell
unit :: a -> ()
unit _ = ()
-- 어떤 집합을 정의역으로 갖든, 싱글톤셋을 공역으로 하는 함수는 오직 한 개 뿐이다.
```

## sets and numbers

모든 numerical 연산들은 숫자타입에 대해 작용하는 함수로 표현될 수 있다.

## number functions

```haskell
square :: Int -> Int
square x = x * x
```

> 모든 수의 일반화는, 연산을 가능하게 하기 위해서 나타났어요. 마이너스 -> 음수, 나눗셈 -> 분수, 제곱근 -> 허수

지금까지 살펴본거는 T -> U인데, (+)나 (*)는 a -> a -> a이다. 이걸 (a, a) -> a 튜플로 바라보든, a -> (a -> a) 함수 반환 함수로 바라보든..

## sets and functions in programming.

집합은 프로그래밍에서 많이 사용됩니다. 특히 type(class라고 불리기도)로 구체화될 때.

## sets and types

집합과 타입이 동일하진 않지만, 타입을 집합으로 볼 수 있다.

Boolean, Char만 보더라도 어떤 원소를 담고 있는 집합을 상상할 수 있다.

프로그래밍에서 대부분은 복합 타입을 사용한다.

> Q: 프로그래밍에서 부분집합에 해당되는 것은?

## functions and methods/subroutine

오늘날 프로그래밍 언어에서의 함수들: 수학적이지 않음. side-effect 발생.

결국 런타임이 되어서야 결정되는 것들은 순수하지 않다..

순수 함수형 언어에서는 순수함수들로만 된다고 하네요~

[todo] continuation passing style(cps)를 배우자~

## functional composition

## composition of relationships

집합의 요소를 보여주던 internal diagram.

vs

집합의 요소를 보여주지 않고 함수만 보여주는 external diagram

## composition and external diagrams

> [깨달음] 모든 정의역이 대응되는 관계가 있어야 한다는 것은, 함수에 대해서만임.
> 사상에서는 그럴 필요가 없음. 그러니 내부에 연결이 없는 대상이 있어도, 내부를 생략하고 외부 다이어그램을 그릴 수 있는 것 같음..
> 물론 실제로 어떻게 대응하는지는 써놓지 않지만, 다이어그램일 뿐임. 이걸 구현하려면.. 곱셈테이블 같은 게 필요할지도..
> 대신 중요한 것은, 타겟과 소스가 겹치면 반드시 합성사상이 존재해야 한다는 것임.
> 만약 그게 성립 안하지 않을까 걱정이 된다? 그러면 그 녀석은 카테고리가 아니다.

f : x -> y, g : y -> z 두 번 걸쳐서 가나 h : x -> z 한 번에 가나 동치(equivalent)이다. 이런 다이어그램을 commuting diagram이라 부름.

## 그래서 카테고리 언제 배움???

범주론은 어찌보면 함수같은 것을에 대한 학문이다. 소스와 타겟이 있는, 그리고 (결합적으로) 합성할 수 있는, 교환 다이어그램으로 표현할 수 있는.

범주론을 정의없이 정의해보자~?

범주론은, 동등성(equality)의 개념을 동형성(isomorphism)의 개념으로 대체했을 때 나타나는 결과입니다.

## isomorphism

어찌됐든, 동형성을 설명하기 전에는 항상, 함수가 표현할 수 있는 관계 유형의 예에 대해 알아봐야됨. 전사, 단사, 전단사 <- 얘들

전단사 함수들을 동형사상이라고 부릅니다. 좀 더 다르게 말하자면 가역성을 가진 함수를 동형사상이라고 부름(의역). 전단사함수, one-to-one, 가역함수, 동형사상? <- 얘들 다 똑같은 놈들

만약에 어떤 두 집합 사이에 가역함수가 "존재하기만이라도 한다면" 두 집합을 동형이라고 부릅니다.

> 예를 들어 섭씨와 화씨는 서로 돌아갈 수 있는 가역함수가 있으므로, 둘은 동형사상입니다.

[정의] 두 집합 R, G에 대해서, 함수 f : G -> R과 역함수 g : R -> G가 일을 때, f . g = ID_R, g . f = ID_G를 만족하면, R ~= G이다.

## isomorphism and identity

복사버그주의 - 항등함수도 가역성을 띈다. 즉, 집합은 자기자신에게 동형이다.

즉, 동형사상의 개념은 동등성 개념을 포함함. 즉, 동등한 것들은 -> 동형임.

## isomorphism and composition

동형인 두 집합 A, B가 있을 때, A와 사상을 가진 C가 있으면, 방향에 따라 합성을 취해 B와 C 사이의 사상도 존재해야함.

## composing isomorphisms

두 개의 동형사상 합성하기 - A ~= B, B ~= C일 때, 이 사상들 합성하면, A ~= C인 동형사상이 튀어나옴.

g . f . f' . g' = id

(g . f) . (f' . g') == id <=> g . f ~= f' . g'

## isomorphisms between singleton sets

모든 싱글톤셋들은 동형임. {1}, {a}, ... 구분할 필요가 없다.

## equivalence relations and isomorphisms (동치관계와 동형사상)

같은 집합 -> 동형인 집합

## 수학에서의 동치 관계

- reflexity : 모든 집합 A에 대해, A = A
- transitivity : if A = B && B = C then, A = C
- symmetry : A = B <=> B = A

동치 관계의 가장 특징적인 속성은, 대칭성입니다.

## isomorphisms as equivalence relations (동형사상을 동치 관계로 바라보기)

- 반사성 A ~= A
- 추이성 A ~= B && B ~= C => A ~= C
- 대칭성 A ~= B <=> B ~= A

동형사상은 가역적이므로, 대칭적이다.

## 부록 - 소프트웨어 개발에서의 composition

엔지니어링의 영역으로 묘사되지만, composition의 원칙이 최대한 활용되지 않는 아이러니한 분야 - 소프트웨어 개발

monolithic design <-> microservice

실제 물리적 세계에서의 전기, 기계 부품들은 소비자가 부품을 자체적으로 생산할 여력이 없다는 제약 때문에

다재다능하고 함께 잘 작동하는 부품을 만들어야 함. (순수함수의 작동방식, 구성적)

하지만 소프트웨어 세계에서는 monolithic design이라는 지엽적, 하나만을 위한 개발 방식이 만연했다.

concatenative programming 유틸라이즈

## 내 요약

함수, 함수 관계 종류, 동형사상, 항등, 합성, 동형사상 합성, 동치 관계

[todo] 동치관계, 집합론에 대해서 더 알아보자.

# 2. category

## from sets to categories

## products

우리는 아직은 한 개의 인자를 받는 함수밖에 알아보지 않았기 때문에 +, -를 정의할 수 없다.

그러니, 곱타입에 대해 알아보자.

집합 A, B에 대해서 곱집합은 A × B = (a, b)로 표현된다.

카테시안곱은 각 속성(property에 대해 하나씩 두 가지 함수를 가지고 있어야 한다.

C -> A, C -> B, 이를 product의 projection이라고 함 (프로그래밍 분야에서는 "getters").

이는 이것의 구성값(constituent values)을 돌려받는 함수다.

(A × B) × C ~= A × (B × C)

[todo] 카테시안곱 연산이 함수적 합성처럼 결합적인 이유를 증명해보자. (동형과 관련...)

## products as objects

프로그래밍에서의 복합타입(records, structs) -> 카테시안곱, methods/subroutines -> 함수

Person { name: String, age: Int }; 그리고, getter

## using products to define numeric operation

(+) : Z × Z -> Z
(+) :: a -> a -> a

Z × Z의 원소 (4, 3)을 생각하보자.

여기서 4 * 3 = 12로 가는 함수도 있을 것이고, 왼쪽원소 4를 고르는 함수, 오른쪽원소 3을 고르는 함수도 있을 것이다.

## 집합의 관점에서 product 정의하기

product은 "순서"쌍의 집합이다. ordered pairs. (A × B != B × A)

순서쌍에는 단순히 원소뿐만이 아니라, 순서에 대한 '정보'도 포함되어 있다.

(x, y)

Norbert Wiener의 제안: { {A, {}}, {B} }
Felix Hausdorff의 제안: { {A, 1}, {B, 2} }
Kazimierz Kuratowski의 제안: { {A, B}, {B} }

## 함수의 관점에서 product 정의하기

방금 살펴본 방법은 저레벨 수준의 zoom in이었다.

시선을 밖으로 빼보자. external diagram과 함께

f1 : C -> A, f2 : C -> B인 C 중, 가장 단순한 C를 A와 B의 cartesian product라고 한다 (?)

여기서 단순하다는 것은 universal property라고 불리는 거에서 소수처럼 더이상 분해되지 않는 그런거..

imposter product들을 싹다 배제하여..

여기서 이 정의는 이 product와 동형인 것들을 배제하지 않았다.

우리가 universal property를 사용할 때, 동형사상을 동등성으로 취급한다.

프로그래밍의 예로 보자면, 페어를 구현하는 많은 방식이 있지만, 그것들이 같은 방식으로 동작하는 한, 서로 변환될 수 있다.

[내 생각 정리] 동형이라는 것은, 서로 대체될 수있는 것들임. 동형사상 === 가역사상.

## sums

father이면 parent이다. father이면 father + mother이다.

product의 쌍대, coproduct. 또는, sum.

co : converse

## 집합의 원소를 함수의 관점에서 정의하기.

집합론에서 모든 것은 집합과 원소의 관점에서 정의된다.

각 원소 x에 대해 싱글톤셋 1에서 x로 가는 함수는 정확히 한 개 있다.

그러니 서로 가역이며, 동형이다. 그래서 집합의 원소는 싱글톤셋에서 해당 원소로 가는 '함수'로 바라볼 수 있다.

## 싱글톤셋을 함수의 관점에서 정의하기.

싱글톤셋을 원소를 가지고 정의한다면 순환적인 정의가 되버리니 안된다.

싱글톤셋은 원소가 하나인 집합이니, 공역으로 취했을 때, 무조건 하나의 함수만 존재할 수 있게 된다.

그래서. for all X, (X -> 1) exist unique.인 집합 1을 싱글톤셋으로 정의한다.

terminal object

## 공집합을 함수의 관점에서 정의하기.

이전 챕터에서 어떤 집합이던지 공집합을 정의역으로 갖는 함수는 단 한 개 존재한다고 했다. 모든 집합에 대해서 성립하므로, 이 것을 공집합의 함수적 정의로 사용할 수 있다.

여기서 보면, 이건 싱글톤셋과 쌍대를 이룬다.

initial object

