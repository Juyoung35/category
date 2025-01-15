```haskell
import Data.Void
import Data.List.NonEmpty

data Void'
  deriving (Eq, Ord)
-- data NonEmpty a = a :| [a]
--   deriving (Eq, Ord)


-- logical reasoning tool of "ex falso quodlibet"
absurd' :: Void -> a
absurd' a = case a of {}

vacuous' :: Functor f => f Void -> f a
vacuous' = fmap absurd

-- >>> map (const 42) [0..3]
-- [42, 42, 42, 42]
const' :: a -> b -> a
const' x _ = x


(.#) :: (b -> c) -> (a -> b) -> a -> c
(.#) f g = \x -> f (g x)

-- foldr reduces the list using binary operator, from right to left
foldr' :: (a -> b -> b) -> b -> [a] -> b
foldr' k z = go where
  go []     = z
  go (y:ys) = k y (go ys)

-- join function remove one level of monadic structure
-- >>> join [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
-- [1, 2, 3, 4, 5, 6, 7, 8, 9]

-- join' :: (Monad m) => m (m a) -> m a
-- join' x = x >>= id


class Semigroup' a where
  -- [Associativity] x <> (y <> z) = (x <> y) <> z
  
  -- >>> Just [1, 2, 3] <> Just [4, 5, 6]
  -- Just [1, 2, 3, 4, 5, 6]
  (<>#) :: a -> a -> a
  a <># b = sconcat' (a :| [b])
  
  -- [Unit]           sconcat (pure x) = x
  -- [Multiplication] sconcat (join xss) = sconcat (fmap sconcat xss)
  sconcat' :: NonEmpty a -> a
  sconcat' (a :| as) = go a as where
    go b (c:cs) = b <># go c cs
    go b []     = b
  
  -- >>> stimes 4 [1]
  -- [1, 1, 1, 1]
  stimes' :: Integral b => b -> a -> a
  -- (impl omitted)



class Semigroup a => Monoid' a where
  -- [Right Identity] x <> mempty = x
  -- [Left Identity]  mempty <> x = x
  -- [Associativity] ..from Semigroup law
  mempty' :: a
  mempty' = mconcat' []
  
  mappend' :: a -> a -> a
  mappend' = (<>)
  
  -- [Concatenation]  mconcat = foldr <> mempty
  -- [Unit]           mconcat (pure x) = x
  -- [Multiplication] mconcat (join xss) = mconcat (fmap mconcat xss)
  -- [Subclass]       mconcat (toList xs) = sconcat xs
  mconcat' :: [a] -> a
  mconcat' = foldr mappend' mempty'
  
  
  
class Functor' f where
  -- [Identity]    fmap id = id
  -- [Composition] fmap (f . g) = fmap g . fmap g
  fmap' :: (a -> b) -> f a -> f b
  
  (<$#) :: a -> f b -> f a
  (<$#) = fmap' . const
  
  
class Functor f => Applicative' f where
  -- [Identity]     pure id <*> v = v
  -- [Composition]  pure (.) <*> u <*> v <*> w = u <*> (v <*> w)
  -- [Homomorphism] pure f <*> pure x = pure (f x)
  -- [Interchange]  <*> pure y = pure ($ y) <*>
  pure' :: a -> f a
  
  (<*>#) :: f (a -> b) -> f a -> f b
  (<*>#) = liftA2' id
  
  liftA2' :: (a -> b -> c) -> f a -> f b -> f c
  liftA2' f x = (<*>#) (fmap f x)
  
  (*>#) :: f a -> f b -> f b
  a1 *># a2 = (id <$ a1) <*># a2
  
  (<*#) :: f a -> f b -> f a
  (<*#) = liftA2' const
  
(<**>#) :: Applicative' f => f a -> f (a -> b) -> f b
(<**>#) = liftA2' (\a f -> f a)

liftA' :: Applicative' f => (a -> b) -> f a -> f b
liftA' = (<*>#) . pure'

liftA3' :: Applicative' f => (a -> b -> c -> d) -> f a -> f b -> f c -> f d
-- a -> b -> (c -> d)
-- f (c -> d) -> f c -> f d
liftA3' f a b c = (liftA2' f a b) <*># c



class Applicative m => Monad' m where
  -- [Left Identity]  return a >>= k = k a
  -- [Right Identity] m >>= return = m
  -- [Associativity]  m >>= (\\x -> k x >>= h) = (m >>= k) >>= h
  (>>=#) :: m a -> (a -> m b) -> m b
  
  -- \`@as >> bs@`\ can be understood as @do@ expression
  (>>#) :: m a -> m b -> m b
  m >># k = m >>=# \_ -> k
  
  return :: a -> m a
  return = pure
  
(=<<#) :: Monad' m => (a -> m b) -> m a -> m b
f =<<# x = x >>=# f
```
