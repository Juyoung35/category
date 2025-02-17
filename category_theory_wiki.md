# 1. Key Concepts

## 1.1. Category

### 1.1.1. Abstract Category(Category)

대상과 사상은 abstract entities of **any** kind가 될 수 있다.
![category](https://upload.wikimedia.org/wikipedia/commons/thumb/f/ff/Category_SVG.svg/150px-Category_SVG.svg.png)

> 이 카테고리는 boldface 3으로 표기된다.

잘 알려진 카테고리

| name | objects            | morphisms          |
| ---  | ---                | ---                |
| Set  | sets               | set functions      |
| Ring | rings              | ring homomorphisms |
| Top  | topological spaces | continuou maps     |

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

## 1.2. Adjoint Functors
