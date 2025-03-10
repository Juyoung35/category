# category
Let's learn about category theory!

link: https://www.youtube.com/playlist?list=PLZQAYIhEkMXHPq0bZekOp8oeGZPkqnNWi

출처: 이웋 youtube https://www.youtube.com/@antel588


# 1. Functor
```haskell
class MyFunctor f where
  lift :: (a -> b) -> f a -> f b
-- Functor은 이미 Data.Functor모듈에 정의되어 있다.
```
가 주어져 있으면, 타입 제너릭 F는 함자이다.

수학적으로는 두 가지 조건이 추가로 달라붙는다:

첫 번째는 항등성의 보존이다.
```haskell
id_t :: t -> t
id_ft :: f t -> f t
-- 위와 같을 때,
lift id_t = id_ft
```

두 번째는 합성관계의 보존이다.
```haskell
h = g . f
-- 위와 같을 때,
lift h = (lift g) . (lift f)
```

다음 `lift` 함수가 주어져 있으므로, `Maybe`를 함자로 사용할 수 있다.
```haskell
instance MyFunctor Maybe where
  lift f x = Just (f x)
  lift _ Nothing = Nothing
```

rust에서:
```rust
fn lift<T, U, F>(f: F) -> impl FnOnce(Option<T>) -> Option<U>
  where F: FnOnce(T) -> U,
{
    |f_t| {
        match f_t {
            Some(t) => Some(f(t)),
            None => None,
        }
    }
}
```

## 1.1. high-dimensional lift (다변수 함수)
`lift :: (a -> b) -> f a -> f b`에서 `(a -> b)`는 일변수 함수이다.

그렇다면 `((a, b) -> c)` 이변수 함수에 대해선 어떻게 처리해야할까?
```haskell
lift_2d :: ((a, b) -> c) -> (f a, f b) -> f (f c)
```

우선 이것보다 더 간단한 함수 형태를 만들어보자:
```haskell
-- 페어 (a, b) 중 왼쪽 a에 대해서만 f가 취해진 경우
class MyFunctor f => LiftE1 f where
  lift_e1 :: ((a, b) -> c) -> (f a, b) -> f c
  lift_e1 f (fx, y) = lift (\x -> f (x, y)) fx

-- (MyFunctor f =>)는 타입 파라미터 f에 MyFunctor가 구현되어 있어야 한다는 조건을 맥락에 추가하는 것이다.

-- \x, \y 같은건 closure 같은 것에서 쓰이는 매개변수이다.

-- 이번에는 class 선언부에 함수의 구체적인 정의까지 정의했기 때문에,
instance LiftE1 Maybe
-- 만 써줘도 Maybe타입에 lift_e1을 구현시킬 수 있다.
```
`lift (a -> b) -> f a -> f b`이다. 여기서 우리는 `(a -> b)`로서 `(a -> c)`를 제공할 것이다.

`lift_e1`에서는 `f a`는 있을지언정 `a`는 없다. 그래서 closure 형태로 `(\a -> f(a, b))`의 형태를 `(a -> c)`로 사용할 것이다.

이해를 위해 덧붙이자면, `f`가 `((a, b) -> c)`를 받는 인자이니 이는 가능하다.

`lift (\x -> f (x, y))`는 `f a -> f c`를 반환할 것이다.

`fx`가 `f a`이므로 여기의 후미에다가 `fx`를 넣어주면 `f c`를 반환할 것이고, 이것은 우리가 얻고자 하는 값이다.

```haskell
-- 페어 (a, b) 중 오른쪽 b에 대해서만 f가 취해진 경우
class MyFunctor f => LiftE2 f where
  lift_e2 :: ((a, b) -> c) -> (a, f b) -> f c
  lift_e2 f (x, fy) = lift (\y -> f (x, y)) fy
```

이제 이 두 함수의 합성을 통해 lift_2d를 정의할 수 있다.
```haskell
class MyFunctor f => Lift2d f where
  lift_2d :: ((a, b) -> c) -> (f a, f b) -> f (f c)
  lift_2d = lift_e1 . lift_e2

  -- lift_2d = lift_e2 . lift_e1으로 순서를 바꿔도 상관없다.
```

이게 가능한 이유는 `lift_e2 :: ((a, b) -> c) -> (a, f b) -> f c`에서 뒤쪽 `((a, f b) -> f c)`를 `lift_e1`의 `((a, b) -> c)`에 대응시켰기 때문이다.

그냥 `lift_e1`의 `b`를 `f b`로, `c`를 `f c`로 바꿨다고 생각하면 편하다.

그러면 `lift_e1 :: ((a, f b) -> f c) -> (f a, f b) -> f (f c)`가 될 것이고, 이것은 우리가 구하고자 했던 `lift_2d`의 형태이다.

`lift_e1`과 `lift_e2`를 통해 깨닫기 원하는 개념은 다변수함수를 처리할 때, 나머지 변수를 상수 취급하고 일변수함수처럼 처리하는 방식이다. 이 방법에 대해서는 다음 챕터에서 바로 더 자세히 설명하도록 하겠다.

이번에는 `lift_e1`과 `lift_e2` 없이 `lift`로만 바로 `lift_2d`를 정의하는 것을 알아보자:
```haskell
-- lift_2d :: ((a, b) -> c) -> (f a, f b) -> f (f c)
class MyFunctor f => Lift2d f where
  lift_2d f (fx, fy) = lift (
      \x -> lift (
          \y -> f (x, y)
        ) fy
    ) fx
```

rust에서:
```rust
fn lift_2d<T, U, V, F>(f: F) -> impl FnOnce(Option<T>, Option<U>) -> Option<Option<V>>
    where F: FnOnce(T, U) -> V,
{
    |ft, fu| {
        let g = |t| {
            let f_t = |u| {
                f(t, u)
            };
            lift::<U, V, _, G>(f_t)(fu)
        };
        lift::<T, Option<V>, _, G>(g)(ft)
    }
}
```

# 2. Applicative Functor

## 2.0. Monoidial Functor
Applicative Functor는 Functor와 Monad의 중간지점에 있는 개념이다.

이는 원래 Monoidal Functor라고 하는 것의 다른 이름일 뿐이다.

Monoidal Functor은 `()`(유닛 타입)과 `(a, b)`(2-tuple 타입)를 제공해야 한다.

또한 다음과 함수들을 제공해야 한다: `empty`, `unite`, `e1`, `e2`
```haskell
empty :: a -> ()          -- 빈 값 (unit)을 반환
unite :: (a, b) -> (a, b) -- n-tuple을 받아 2-tuple 형식을 반환
e1 :: (a, b) -> a         -- 주어진 2-tuple의 첫 번째 요소를 반환
e2 :: (a, b) -> b         -- 주어진 2-tuple의 두 번째 요소를 반환
```

이전 챕터에서 일변수에 대한 처리를 다변수로 확장할 때 사용하는 방법이 있다고 언급했었다.

사실 다변수함수의 인자 `(a, b)`, `(a, b, c)`는 각각 `a -> b`, `a -> b -> c`와 동일하다.

이런식으로 화살표를 연달아 붙여서, 다변수함수를 함수를 반환하는 일변수함수로 바꾸는 것을 currying이라고 한다.
```haskell
f :: T1 -> (T2 -> (T3 -> (... (T{n} -> U) ...))) -- 실제로 하스켈에서 이런 ...표기법은 지원하지 않는다. 이해를 돕기 위한 것.
f :: T1 -> T2 -> T3 -> ... -> T{n}               -- 위처럼 괄호를 따로 달아주지 않아도, Haskell은 자동으로 오른쪽부터 괄호를 붙이는 것으로 인식한다.
                                                 -- (right-to-left associativity)
```
이 커링된 함수는 실질적으로 n-variable 다변수 함수와 똑같다.

일반적인 프로그래밍 언어에서 이런 함수가 있다면 호출할 때 `f(t1)(t2)(t3)...(t{n})`과 같은 식으로 호출할 것이다.

하지만 Haskell에서는 `f t1 t2 t3 ... t{n}`과 같이 괄호 없이 호출할 수 있다.



이전에 Monoidal Functor의 조건으로 제시된 함수 `unite`를 다차원으로 확장해보자.

```haskell
unite_0 = empty
unite_1 = id
unite_{n} (t1, t2, t3, ..., t{n}) = unite (unite_{n-1} (t1, t2, t3, ..., t{n-1})) (t{n})

-- n인수 함수를 받아 이 n개의 값을 unite_{n}으로 합친 것을 인자로 받아 u를 반환하는 1인수 함수로 바꾸는 함수.
unite_arg_{n} :: ((t1, t2, t3, ..., t{n}) -> u) -> (((((t1, t2), t3), t4), ...), t{n}) -> u
unite_arg_0 _ = empty
unite_arg_1 = id
unite_arg_2 = id
unite_arg_3 f p = f (e1 (e1 p), e2 (e1 p), e2 p)
unite_arg_4 f p = f (e1 (e1 (e1 p)), e2 (e1 (e1 p)), e2 (e1 p), e2 1)
...

-- 서로 다른 구조를 가진 구조를 가진 두 중첩된 2-tuple 사이의 변환 함수
reassociate_r :: ((t, u), v) -> (t, (u, v))
reassociate_r p = unite(e1 (e1 p), unite(e2 (e1 p), e2 p))
reassociate_l :: (t, (u, v)) -> ((t, u), v)
reassociate_r p = unite(unite(e1 p, e1 (e2 p)), e2 (e2 p))
```

그렇다면 Monoidal Functor의 조건은 어떻게 되는 것일까?
```haskell
empty _ = ()
unite = id
e1 = fst
e2 = snd

unite_0 = empty
unite_1 = id
unite_2 = id
unite_3 (t1, t2, t3) = unite(unite_2(t1, t2), t3)
unite_4 (t1, t2, t3, t4) = unite(unite_3(t1, t2, t3), t4)

unite_arg_0 _ = empty
unite_arg_1 = id
unite_arg_2 = id
unite_arg_3 f p = f (e1 (e1 p), e2 (e1 p), e2 p)
unite_arg_4 f p = f (e1 (e1 (e1 p)), e2 (e1 (e1 p)), e2 (e1 p), e2 p)

reassociate_r :: ((a, b), c) -> (a, (b, c))
reassociate_r p = unite(e1 (e1 p), unite(e2 (e1 p), e2 p))
reassociate_l :: (a, (b, c)) -> ((a, b), c)
reassociate_l p = unite(unite(e1 p, e1 (e2 p)), e2 (e2 p))

-- 이 함수들은 구현의 한계로 인해 별도로 정의해도 쓰는 함수이다. 강의에선 등장하지 않는다.
-- 2-tuple의 좌측, 우측 각각에 대해서만 gather을 적용하는 것은 바로 적용되지 않기에 이 함수를 거친다.
apply_e1 :: (a -> c) -> (a, b) -> (c, b)
apply_e1 f = \p -> (f (e1 p), e2 p)
apply_e2 :: (b -> c) -> (a, b) -> (a, c)
apply_e2 f = \p -> (e1 p, f (e2 p))



class MyFunctor f => MonoidalFunctor f where
  pure :: a -> f ()                -- 값을 제너릭 타입으로 바꿔줌
  -- 강의에는 pure :: a -> f a로 나와있음. 아마 디폴트값을 의미하는 것일텐데
  -- instance 구현할 때 말고는 구현해줄 수가 없음.
  -- 여기서는, class 수준에서 구현하기 위해 () 타입을 써줬음.
  gather :: (f a, f b) -> f (a, b) -- 제너릭의 튜플을 튜플 제너릭으로 만들어 줌
  
  -- 조건 1. 결합성의 보존
  associativity :: ((f a, f b), f c) -> f (a, (b, c))
  -- associativity = gather . (apply_e2 gather) . reassociate_r
  -- associativity = (lift reassociate_r) . gather . (apply_e1 gather)
  -- 위의 두 합성함수의 결과값이 동일해야 한다.
  
  -- 조건 2. 항등원소의 보존
  identity :: (f t, ()) -> f t
  -- identity = e1
  -- identity = (lift e1) . gather . (apply_e2 Main.pure)
  -- 위의 두 합성함수의 결과값이 동일해야 한다.
```

그렇다면 Monoidal Functor는 왜 있는 것일까?

다인수 함수의 자연스러운 `lift`를 위해서이다.

```haskell
lift_2d :: ((a, b) -> c) -> (f a, f b) -> f (f c)
lift_3d :: ((a, b, c) -> d) -> (f a, f b, f c) -> f (f (f d))
```

`lift`의 차원확장에서는 반환 타입에 제너릭이 인수 개수만큼 중첩이 걸리므로 자연스럽지 않다.

```haskell
lift_0 :: (() -> a) -> () -> f a
lift_2 :: ((a, b) -> c) -> (f a, f b) -> f c
lift_3 :: ((a, b, c) -> d) -> (f a, f b, f c) -> f d
```

이런식으로 하나의 제너릭만 걸려있는 것이 더 자연스럽다.

이번엔 제너릭 타입 튜플을 제너릭 타입 한 개로 합쳐주는 함수를 만들어보자.

```haskell
m_unite_{n} :: (f t1, f t2, f t3, ..., f t{n}) -> f (((((t1, t2), t3), t4), ...), t{n})
m_unite_0 :: () -> f ()
m_unite_0 = Main.pure . empty
m_unite_1 :: (f t) -> f t
m_unite_1 = id
m_unite_{n} :: (f t1, f t2, f t3, ..., f t{n}) -> f (t1, t2, t3, ..., t{n})
m_unite_{n} (f1, f2, f3, ..., f{n}) = gather (unite(m_unite_{n-1} (f1, f2, f3, ..., f{n-1}), f{n}))
```

이제 지금까지 만든 함수들을 이용해 자연스러운 `lift_{n}` 함수를 만들어보자.

```haskell
-- lift :: (a -> b) -> f a -> f b
-- unite_arg_{n} :: ((t1, t2, t3, ..., t{n}) -> u) -> (((((t1, t2), t3), t4), ...), t{n}) -> u
-- m_unite_{n} :: (f t1, f t2, f t3, ..., f t{n}) -> f (t1, t2, t3, ..., t{n})
lift_0 :: (t -> u) -> v -> f ()
lift_0 f a = lift (unite_arg_0 f) (m_unite_0 a)
lift_{n} :: ((t1, t2, t3, ..., t{n}) -> u) -> (f t1, f t2, f t3, ..., f t{n}) -> f u
lift_{n} f (t1, t2, t3, ..., t{n}) = lift (unite_arg_{n} f) (m_unite_{n} (t1, t2, t3, ..., t{n})

-- m_unite_{n}과 lift_{n}은 동치
m_unite_{n} = lift_{n} unite_{n}
-- pure와 lift_0은 동치
pure a = lift_0 (\b -> a) ()
-- gather와 lift_2는 동치
gather p = m_unite_2(e1 p, e2 p)
```

> 대충 말하자면, Monoidal Functor은 `lift_{n}`이 정의되어 있는 Functor의 가장 큰 부분군이다.

3개 이상의 인수를 2-tuple로 바꾸는 방법은 하나가 아니다.

그래서 `unite_{n}` 함수, 그리고 `lift_{n}` 함수까지 다양한 방법으로 정의될 수 있다.

이런 불상사를 방지하기 위해, 결합성 보존과 항등원소 보존에 대한 조건이 있다고 볼 수 있다.

우리가 정의한 `unite_{n}`은 n-tuple이 지원되지 않는 언어라 가정하고 중첩된 2-tuple 구조를 썼지만,

언어에서 n-tuple을 지원한다면

```haskell
unite_{n} :: t1 -> t2 -> t3 -> ... -> t{n} -> (t1, t2, t3, ..., t{n})
unite_arg_{n} :: t1 -> t2 -> t3 -> ... -> t{n} -> u -> (t1, t2, t3, ..., t{n}) -> u
m_unite_{n} :: f t1 -> f t2 -> f t3 -> ... -> f t{n} -> f (t1, t2, t3, ..., t{n})
```

다음과 같이 쓸 수 있을 것이다.

만약 언어에서 n인수 함수와 n-튜플을 욕하는 함수의 구분이 아예 없고 호환된다면, `unite_{n}`, `unite_arg_{n}`은 항등함수이다.

함수형 프로그래밍언어에서 제너릭이 걸린 함수 타입의 값을 제너릭 타입 간의 함수로 바꿔주는 함수도 유용하다:
```haskell
apply :: f (t -> u) -> f t -> f u
```

이 개념이 바로 Applicative Functor라는 변형된 개념을 낳게 되었다.

## 2.1. Applicative Functor

```haskell
currying_{n} :: t1 -> t2 -> t3 -> ... -> t{n} -> u -> (t1, t2, t3, ..., t{n}) -> u
currying_{n} t1 t2 t3 ... t{n} u = (\tt -> u)
uncurrying_{n} :: (t1, t2, t3, ..., t{n}) -> u -> t1 -> t2 -> t3 -> ... -> t{n} -> u
uncurrying_{n} (t1, t2, t3, ..., t{n}) u = \t1 -> \t2 -> \t3 -> ... -> \t{n} -> u
```

이제 `apply` 함수를 정의해보자.
```
apply_{n} :: f (t1 -> t2 -> t3 -> ... -> t{n} -> u) -> f t1 -> f t2 -> f t3 -> ... -> f t{n} -> f u
apply_0 = id
apply_1 = apply
apply_{n} ff = apply_{n-1} . (apply ff)

lift_{n}c :: t1 -> t2 -> t3 -> ... -> t{n} -> u -> f t1 -> f t2 -> f t3 -> ... -> f t{n} -> u
lift_{n}c t1 t2 t3 ... t{n} u = apply_{n} (pure u)
lift_{n}c t1 t2 t3 ... t{n} u ft1 = apply_{n-1} (lift u ft1) -- n >= 1일 때

-- apply와 lift_2c는 동치
-- lift_2와 lift_2c도 동치 (apply와도)
apply = lift_2c id -- id :: (a -> b) -> a -> b
```

이렇게 보면 튜플을 받는 `gather`보다 유연하게 currying되어있는 'apply'를 쓰는 게 더 좋아보인다.

이래서 탄생한 게 Applicative Functor이다.

```haskell
class Functor f => MyApplicativeFunctor f where
  pure :: a -> f a
  apply :: f (a -> b) -> f a -> f b

  mf[mt] = apply(mf)(mt)
  mf[mt1]...[mt{n}] = apply_{n}(mf)(mt1)...(mt{n})

  -- 조건 1 identity
  apply (pure id_t) = id_ft

  -- 조건 2 composition
  pure(compose)[mf][mg][mv] = mf[mg[mv]]
  compose :: (a -> b) -> (b -> c) -> (c -> d)
  compose f g = f . g

  -- 조건 3 homomorphism
  pure(f) [pure(t)] = pure(f(t))

  -- 조건 4 interchange
  mf[pure(t)] = pure(/g -> g(t)) [mf]
  f(t) = (\g -> g(t)) (f)

  -- 조건 5
  apply (pure(f)) = lift(f)
