```rust
#![feature(associated_type_defaults)]
#![feature(impl_trait_in_fn_trait_return)]
#![feature(never_type)]
#![feature(unboxed_closures)]
#![allow(dead_code)]
#![feature(tuple_trait)]

struct Void(!);
impl Void {
    fn absurd<T>(self: Void) -> T {
        self.0
    }
}

struct NonEmpty<'a, T>(T, &'a [T]);

fn r#const<T, U>(a: T) -> impl FnOnce(U) -> T {
    |_| a
}


fn compose<T, U, V, F, G>(f_uv: F) -> impl FnOnce(G) -> impl FnOnce(T) -> V
where
    F: Fn(U) -> V,
    G: Fn(T) -> U,
{
    |f_tu| {
        move |t| {
            f_uv(f_tu(t))
        }
    }
}

fn id<T>(t: T) -> T { t }

#[macro_export]
macro_rules! curry {
    ($f:expr, $first_arg:expr) => {
        $f($first_arg)
    };
    ($f:expr, $first_arg:expr, $($arg:expr),+) => {
        curry!($f($first_arg), $($arg),+)
    };
}


// trait Semigroup {
//     fn () {
        
//     }
// }

#[macro_export]
macro_rules! F {
    [$T:ident] => {
        Self::Wrapped<$T>
    };
}

// #[macro_export]
// macro_rules! func {
//     ($from:ident -> $to:ident) => {
//         impl FnOnce($from) -> $to
//     };
//     ($from:ident -> $($mult:ident)->+) => {
//         impl FnOnce($from) -> func!($($mult)->+)
//     };
// }

trait Functor {
    type Unwrapped;
    type Wrapped<T>: Functor<Unwrapped = T>;
    
    fn fmap<T, U>(f_ab: impl FnOnce(T) -> U) -> impl FnOnce(F![T]) -> F![U];
    
    fn lfmap<T, U>(t: T) -> impl FnOnce(F![U]) -> F![T] {
        curry!(compose, Self::fmap, r#const)(t)
    }
}

trait Applicative: Functor {
    fn pure<T>(a: T) -> F![T];
    
    fn chain<T, U>(ff_f_ab: Self::Wrapped<impl FnOnce(T) -> U>) -> impl FnOnce(F![T]) -> F![U] {
        Self::lift_a2(id)(ff_f_ab)
    }
    
    fn lift_a2<T, U, V, F>(f_abc: impl FnOnce(T) -> F) -> impl FnOnce(F![T]) -> impl FnOnce(F![U]) -> F![V]
    where
        F: FnOnce(U) -> V
    {
        |ff_t| Self::chain(curry!(Self::fmap, f_abc, ff_t))
    }
    
    fn rchain<T, U>(ff_a: F![T]) -> impl FnOnce(F![U]) -> F![U] {
        |ff_b| curry!(Self::chain, curry!(Self::lfmap, id, ff_a), ff_b)
    }
    
    fn lchain<T, U>(ff_a: F![T]) -> impl FnOnce(F![U]) -> F![T] {
        Self::lift_a2(r#const)(ff_a)
    }
    
    fn longchain<T, U, F>(ff_a: F![T]) -> impl FnOnce(Self::Wrapped<F>) -> F![U]
    where
        F: FnOnce(T) -> U,
    {
        Self::lift_a2(|a: T| {
            |f_bc: F| {
                f_bc(a)
            }
        })(ff_a)
    }
    
    fn lift_a<T, U>(f_ab: impl FnOnce(T) -> U) -> impl FnOnce(F![T]) -> F![U] {
        compose(Self::chain)(Self::pure)(f_ab)
    }
    
    fn lift_a3<T, U, V, W, F, G>(f_abcd: impl FnOnce(T) -> F) -> impl FnOnce(F![T]) -> impl FnOnce(F![U]) -> impl FnOnce(F![V]) -> F![W]
    where
        F: FnOnce(U) -> G,
        G: FnOnce(V) -> W,
    {
        |a| {
            |b| {
                |c| {
                    curry!(Self::chain, curry!(Self::lift_a2, f_abcd, a, b), c)
                }
            }
        }
    }
}

trait Monad: Applicative {
    fn mchain<T, U, F>(mm_a: F![T]) -> impl FnOnce(F) -> F![U]
    where
        F: FnOnce(T) -> F![U];
    
    fn rmchain<T, U, F>(mm_a: F![T]) -> impl FnOnce(F![U]) -> F![U] {
        |mm_b| curry!(Self::mchain, mm_a, |_| mm_b)
    }
    
    fn r#return<T>(a: T) -> F![T] {
        Self::pure(a)
    }
    
    fn lmchain<T, U, F>(f_amm_b: impl FnOnce(T) -> F![U]) -> impl FnOnce(F![T]) -> F![U] {
        |x| curry!(Self::mchain, x, f_amm_b)
    }
}
```
