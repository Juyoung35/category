# 1. Key Concepts

## 1.1. Category

### 1.1.1. Abstract Category(Category)

대상과 사상은 abstract entities of **any** kind가 될 수 있다.

![category](https://upload.wikimedia.org/wikipedia/commons/thumb/f/ff/Category_SVG.svg/150px-Category_SVG.svg.png)

> 이 카테고리는 볼드체 **3**으로 표기된다.

#### group-like structures

| group-like structures                  | total | associative | identity | divisible | commuative |
| ---                                    | ---   | ---         | ---      | ---       | ---        |
| partial magma                          | ❌ | ❌ | ❌ | ❌ | ❌ |
| semigroupoid                           | ❌ | ✔️ | ❌ | ❌ | ❌ |
| small category                         | ❌ | ✔️ | ✔️ | ❌ | ❌ |
| groupoid                               | ❌ | ✔️ | ✔️ | ✔️ | ❌ |
| commutative groupoid                   | ❌ | ✔️ | ✔️ | ✔️ | ✔️ |
| magma                                  | ✔️ | ❌ | ❌ | ❌ | ❌ |
| commutative magma                      | ✔️ | ❌ | ❌ | ❌ | ✔️ |
| quasigroup                             | ✔️ | ❌ | ❌ | ✔️ | ❌ |
| commutative quasigroup                 | ✔️ | ❌ | ❌ | ✔️ | ✔️ |
| unital magma                           | ✔️ | ❌ | ✔️ | ❌ | ❌ |
| commutative unital magma               | ✔️ | ❌ | ✔️ | ❌ | ✔️ |
| loop                                   | ✔️ | ❌ | ✔️ | ✔️ | ❌ |
| commutative loop                       | ✔️ | ❌ | ✔️ | ✔️ | ✔️ |
| semigroup                              | ✔️ | ✔️ | ❌ | ❌ | ❌ |
| commutative semigroup                  | ✔️ | ✔️ | ❌ | ❌ | ✔️ |
| associative quasigroup                 | ✔️ | ✔️ | ❌ | ✔️ | ❌ |
| commutative-and-associative quasigroup | ✔️ | ✔️ | ❌ | ✔️ | ✔️ |
| monoid                                 | ✔️ | ✔️ | ✔️ | ❌ | ❌ |
| commutative monoid                     | ✔️ | ✔️ | ✔️ | ❌ | ✔️ |
| group                                  | ✔️ | ✔️ | ✔️ | ✔️ | ❌ |
| abelian group                          | ✔️ | ✔️ | ✔️ | ✔️ | ✔️ |

#### 정의

범주 C는 다음으로 구성됨:

- 대상(objects)의 모임(class) ob(C)
- 사상(morphisms)의 모임 mor(C)
- domain/source 모임 함수 dom : mor(C) -> ob(C)
- codomain/target 모임 함수 cod: mor(C) -> ob(C)
- 모든 세 대상 a, b, c에 대하여 연산(operation) hom(a, b) × hom(b, c) -> hom(a, c)인 사상들의 존재(사상의 합성)

> hom(a, b) :: mor(C)에서 dom(f) = a, cod(f) = b를 만족하는 사상 f의 부분모임.
> f : a -> b로 적는다.
> a에서 b로 가는 모든 사상.의 hom-class

범주 C는 다음 공리들을 만족함:
- 결합법칙(associative law)
- 좌/우 항등법칙(left and right unit laws)

> 범주론에서 대상에 대한 참조 없이, 부분 이진 연산자 w/ additional properties를 사용해서 정의될 수 있다.
> 중요한건 사상이라는거

#### 작고 큰 범주(small and large categories)

ob(C)와 hom(C)가 모두 고유 모임이 아닌 집합이라면, 범주 C는 작다(small)라고 함.

locally small category : 모든 대상 a, b에 대해 hom-class hom(a, b)가 집합이면(이럴 때 homset이라 부름)

> 수학에서 중요한 많은 범주들은 small하지 않더라도 적어도 locally small하다.
> small category는 대수적 구조 모노이드와 비슷하게 보일 수 있다. 게다가 closure 속성을 요구하지 않는.

large category : 대수적 구조의 구조(structures)를 만드는 것에 쓰일 수 있다.

#### 다양한 카테고리

Set

Rel

allegories

discrete category: 사상은 오직 항등사상
* hom(X, X) = {$`id_X`$}
* hom(X, Y) = $`\emptyset`$

preordered set (P, $`\le`$) -> small category
* 대상 : members of P
* 사상 : x $`\le`$ y일 때, x -> y
* 만약 $`\le`$가 반대칭이라면 어느 두 대상이든 최대 한 개의 사상만 존재한다.
* 항등 사상과 사상의 합성 가능성은 반사성과 추이성으로 보장된다.

partially ordered set -> small category

equivalence relation -> small category

ordinal number(as ordered set) -> small category

monoid -> small category
* with single object x.

group -> category ?small
* single object
* every morphism is invertible (== isomorphism)

groupoid : 모든 사상이 동형 사상.
* groups, group actions, equivalence relations의 일반화.
* 두 개 이상의 대상 가질 수 있음. (group과의 차이점)

topological space -TBD

any directed graph -> generate small category
* 대상 : vertices
* 사상 : path(loop)
* free category라고 부름

Ord : concrete category
* 모든 전위 집합의 모임 w/ order-preserving 함수 (예를 들어, 단조 증가 함수)
* 대상 : 전위 집합(preordered sets)
* 사상 : order-preserving 함수
* concrete category : Set범주에 어떤 유형의 구조를 추가함으로서 얻어지는 것.

Grp : large category이자 concrete category
* 모든 군의 모임.
* 대상 : 군
* 사상 : 군 준동형 사상

Ab
* 모든 아벨군의 모임.
* Grp의 full subcategory.
* abelian category의 prototype(?)

| category | objects | morphisms |
| ---      | ---     | ---       |
| Set    | 집합(sets) | 함수(functions) |
| Rel    | 집합(sets) | 이항 관계(binary relations) |
| Ord    | 원순서 집합(preordered sets) | 단조증가함수(monotone-increasing functions) |
| Mon    | 모노이드(monoids) | 모노이드 준동형 사상(monoid homomorphisms) |
| Grp    | 군(groups) | 군 준동형 사상(group homomorphisms) |
| Grph   | 그래프(graphs) | 그래프 준동형 사상(graph homomorphisms) |
| Ring   | 환(rings) | 환 준동형 사상(ring homomorphisms) |
| Field  | 체(fields) | 체 준동형 사상(field homomorphisms) |
| R-Mod  | R-가군(R-modules, R = 환) | R-가군 준동형 사상(R-module homomorphisms) |
| $`Vect_K`$ | 체 K 위의 벡터 공간(vector spcaes over the field K) | K-선형 변환(K-linear maps) |
| Met    | 거리 공간(metric spaces) | 거리 변환(?short maps) |
| Meas   | 측도 공간(measure spaces) | 가측 함수(measurable functions)
| Top    | 위상 공간(topological spaces) | 연속 함수(continuous functions) |
| $`Man^p`$  | 매끄러운 다양체(smooth manifolds) | p-times 연속 미분가능 변환(?p-times continuously differentiable maps) |
| Cat    | small categories | functors |

#### 새 범주의 구성(construction)

##### Dual Category

범주 C의 쌍대 $`C^{op}`$

##### Product Categories

범주 C, D의 곱 범주 C × D.

#### 범주의 유형

사상 f : a -> b는 다음과 같이 불립니다:

- 단사 사상(monomorphism, monic): 왼쪽 소거 가능(left-cancellable)
  * f . g1 = f . g2 => g1 = g2
  * 전사 사상의 쌍대.
- 전사 사상(epimorphism, epic): 오른쪽 소거 가능(right-cancellable)
  * g1 . f = g2 . f => g1 = g2
  * 단사 사상의 쌍대.
- 전단사 사상(?bimorphism): 단사 + 전사 사상

---

- retraction: 좌역?이 있으면
  * retraction(split epimorphism): 어떤 사상의 left inverse.
  * 모든 retraction은 전사 사상.
  * section의 쌍대.
- section: 우역?이 있으면
  * section(split monomorphism): 어떤 사상의 right inverse.
  * 모든 section은 단사 사상.
  * retraction의 쌍대.
  
![section and retraction](https://upload.wikimedia.org/wikipedia/commons/thumb/8/8d/Section_retract.svg/150px-Section_retract.svg.png)

- 동형 사상(isomorphism): 역이 있으면

---

- 자기 사상(endomorphism): a = b이면
  * 대상 a의 자기 사상의 모임은 end(a)로 표기.
  * 
- 자기 동형 사상(automorphism)

## 1.2. Adjoint Functors
