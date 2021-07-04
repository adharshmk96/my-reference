

- Pure Functions
  - Same input gives same output
- Immutable data
  - Every variable is a constant
- No / Less side effects
  - Accessing data outside the function's scope
- Declarative
  - Define the output of functions rather than step by step algorithm
- Lazy Evaluation
  - Evaluate statements only if or when they're needed
- There are no Loops
  - No where, for loops we need to use reccurssion 

List functions
- map:   (a -> b) -> [a] -> [b]

String functions

- words:  String -> [String]
- read:   Read a => String -> a


Snippets

```
bmiTell :: (RealFloat a) => a -> a -> String  
bmiTell weight height  
    | bmi <= skinny = "You're underweight, you emo, you!"  
    | bmi <= normal = "You're supposedly normal. Pffft, I bet you're ugly!"  
    | bmi <= fat    = "You're fat! Lose some weight, fatty!"  
    | otherwise     = "You're a whale, congratulations!"  
    where bmi = weight / height ^ 2  
          skinny = 18.5  
          normal = 25.0  
          fat = 30.0  
```

3 ways to bind values in functions

```
something (x : xs) (y : ys) = x ++ y

something2 farr sarr = x ++ y
  where
    (x : _) = farr
    (y : _) = sarr

something3 farr sarr =
  let (x : _) = farr
      (y : _) = sarr
   in x ++ y
```

4 ways to do pattern matching (the if else evaluation)

```
-- Function names
patternM 0 = "It's zero"   
patternM 1 = "It's One"   
patternM x = "It's More than One"  

-- The case
patternMGC x = case x of 0 -> "It's zero"    
                         1 -> "It's One"
                         x -> "It's More than One"  

-- Guards
patternMG x
    | x == 0 = "It's zero"    
    | x == 1 = "It's One"
    | otherwise  = "It's More than One"

-- Where binding
patternMGW1 x = "well, "+ans x
    where
      ans 0 = "It's zero"   
      ans 1 = "It's One"   
      ans x = "It's More than One"  

-- Where binding 2
patternMGW2 x
    | isZero = "It's zero"    
    | isOne = "It's One"
    | otherwise  = "It's More than One"
    where 
        isZero = x == 0
        isOne = x == 1
```

ways to do list comprehention
```
[ x+1 | x <- [1..10], x < 5 ]

[ ans | x <- [1..10], let ans = x + 1 ]

```