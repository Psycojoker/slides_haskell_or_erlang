# Haskell ou Erlang<br>comment choisir son nerdisme ?

---

# Disclamer

- peut d´expérience pratique, surtout du théorique
- overview (y a beaucoup trop à montrer)
- surtout les concepts
- j'ai surtout fait de l'Haskell dernièrement

---

# Haskell

---

# Haskell

- langage de programmation fonctionnel
- ml style
- pure, sans exceptions, YOLO style
- typage statique fort
- inférence de type
- compilé
- mais a un shell (limité)
- relativement bonne performances
- "lazy" (outer most evaluation)
- très très solide
- hardcore

---

# Haskell aurait du rester un projet de recherche mais ...

---

# Cet homme est arrivé avec...les MONADS !

![philip](philip.png)

---

![simon](simon.jpg)

---

# Hello world!

    !haskell
    main = putStrLn "Hello World!"

"Dafuq?!"

(Sacrilège !)

---

# Let's start with functions

Petit jeu, ce code:

    !haskell
    a = 1

fait quoi ?

---

# Clef de lecture 1 ! <br> « Les mathématiciens sont des branleurs »

---

# Définition de fonction

    !haskell
    a = 1

Est l'équivalent de:

    !python
    def a():
        return 1

De même:

    !haskell
    a b = b + 1

Est l'équivalent de:

    !python
    def a(b):
        return b + 1

Et s'appel (rappel: les mathématiciens sont des branleurs):

    !haskell
    a 42

(En python:)

    !python
    a(42)

---

# Typage

Sans arguments:

    !haskell
    function :: Int
    function = 1

2 arguments:

    !haskell
    plus_1 :: Int -> Int
    plus_1 a = a + 1

MOAR:

    !haskell
    pluche :: Int -> Int -> Int
    pluche a b = a + b

Etc ...

---

# Fonction infixes

    !haskell
    a +%%+ b = a + b

On peut appeler une fonction infixe comme une fonction normalle:

    !haskell
    (+) 1 2

Et l'inverse:

    !haskell
    1 `div` 2

---

# Opérateurs && types:

## Opérateurs

- \+ * / ** // == /= && ||

## Types:

- Bool, Int, Integer, Char, [Char], String, Tuple etc...

## Demo GHCI

---

# Lists

    !haskell
    [1,2,3,4]
    -- est l'équivalent de
    1:2:3:4:[]

Mais ... on s'en branle ?

---

# Et non ! Car ... pattern matching!

    !haskell
    add_1 :: Int -> Int
    add_1 1 = 2
    add_1 2 = 3
    add_1 a = a + 1

Sur des listes (ZOMG typage générique !):

    !haskell
    head :: [a] -> a
    head (x:xs) = x

    tail :: [a] -> [a]
    tail (x:xs) = xs

(Les () ici sont pour la **priorité** d'évaluation)

    !haskell
    len :: [a] -> Int
    len [] = 0
    len (x:xs) = 1 + len xs

Le **_** est un catchall (marque aussi le "rien a foutre de cet argument, bp style"):

    !haskell
    est_1 :: Int -> Bool
    est_1 1 = True
    est_1 _ = False

---

# Autres constructions

Listes compréhensions (syntaxe pourrie):

    !haskell
    [x*2 | x <- [1..10], x < 42]

Opérateur ternaire:

    !haskell
    if a > 10 then "pouet" else "plop"

Guards:

    !haskell
    est_1 :: Int -> Bool
    est_1 a
      | a == 1    = True
      | a == 2    = False
      | otherwise = False

Case:

    !haskell
    est_1 :: Int -> Bool
    est_1 a = case a of 1 -> True
                        2 -> False
                        3 -> False
                        _ -> False

---

# Autres constructions 2

Where (are you?):

    !haskell
    plus_42_fois_3 :: Int -> Int
    plus_42_fois_3 a = a + 42_fois_3
        where 42_fois_3 = 42 * 3

Let (it be):

    !haskell
    plus_42_fois_3 :: Int -> Int
    plus_42_fois_3 a =
        let 42_fois_3 = 42 * 3
        in a + 42_fois_3


Lambdas:

    !haskell
    \a b -> a + b

Le $ (branleur style):

    !haskell
    add_1 (add_1 (add_1 10))
    -- est l'équivalent de
    add_1 $ add_1 $ add_1 10

---

# Currification

Haha ! Je vous ai menti !

    !haskell
    add :: Int -> Int -> Int
    add a b = a + b

Tout ça est de l'haskell valide:

    !haskell
    add
    add 1
    add 1 2

Ceci aussi:

    !haskell
    add :: Int -> Int -> Int
    add a = (a +)

Aussi:

    !haskell
    add :: Int -> Int -> Int
    add = (+)

---

# High order functions

Map:

    !haskell
    map (+ 1) [1..10]

Filter:

    !haskell
    filter even [1..10]

Foldl (fold left, le reduce):

    !haskell
    foldl (+) 0 [1..10]

    sum = foldl (+)

(Y a aussi foldr, foldl1, foldr1)

takeWhile:

    takeWhile (<100) [1..]
    -- ZOMG INFINITY

---

# Clef de lecture 2<br>« la composition de fonctions sauvera le monde »

---

# Composition

En math (beurk):

    !haskell
    (f o g)(x) == f(g(x))

En Haskell:

    !haskell
    add_2 = add_1 . add_1
    - ==
    add_2 a = add_1 $ add_1 a

But du jeu: jouer aux légos. Super puissant (vraiment) et permet une très grande réutilisabilité.

Pour la blague, le type de (.):

    !haskell
    (.) :: (b -> c) -> (a -> b) -> a -> c

---

# Data

    !haskell
    data Bool = False | True deriving (show)

    data Shape = Circle Float Float Float | Rectangle Float Float Float Float

    data Maybe a = Nothing | Just a

    getContent :: Maybe a -> a
    getContent Just a = a

    data Person = Person { firstName :: String
                         , lastName :: String
                         , age :: Int
                         , height :: Float
                         , phoneNumber :: String
                         , flavor :: String
                         } deriving (Show)

    age :: Person -> Int

    type String = [Char]

    -- y a aussi les types classes

---

# IO

Hello World!

    !haskell
    main = putStrLn "Pouet"

Deux print ?

    !haskell
    main = putStrLn "Pouet"
           putStrLn "Pouet"

KABOOM!

La version pour humain:

    !haskell
    main = do
        putStrLn "Pouet"
        putStrLn "Pouet"

Read:

    !haskell
    main = do
        toi <- getLine
        putStrLn $ "Salut " ++ toi

---

# Vous voulez les monads ?<br><small>(run for your life!)</small>

---

# Erlang

---

# Erlang

- fonctionnel non pure
- ml style (mais du pipi de chat à côté de haskell)
- 20 ans en avance sur tout le monde sur la concurrence
- syntaxe de merde
- dynamique fort
- bytecode
- a aussi un shell (bien naze mais utile)
- vm de fou
- orienté fiabilité
- hot code loading
- assez pourri hors de son scop (avis perso)
- Disclamer: j'ai quasi pas codé avec

---


