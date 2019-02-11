#Ramda Test

```javascript 
// The original composed function
R.reduce((acc, x) => R.compose(R.flip(R.prepend)(acc), R.sum, R.map(R.add(1)))([x, ...acc]), [0])([13, 28]);

// Split for easier comment
R.reduce(
  (acc, x) => R.compose( // Where the reducer begins
    R.flip(R.prepend)
      (acc) // curried to R.flip result
  , R.sum
  , R.map(R.add(1)))
    ([x, ...acc])// curried to R.compose result
  , [0] // R.reduce list to iterate over
)
([13, 28]); // curried to R.reduce result
// The above returns [ 46, 15, 0 ]
```

The library Ramda provides a set of useful javascript functions following the functional programming paradigm. Some of them are just different approaches to native javascript functions.

Starting from the outside, we have `R.Reduce` which takes in 3 arguments, a `Reducer function` which will be used to iterate over the other two elements, an `accumulator` and a `list` to iterate over. `R.Reduce` will run the `reducer function` over each element of the list and store the result on the `accumulator` value. In this case the `accumulator` is `[0]` and the list is `[13,28]`. 


The `reducer function` itself also takes two arguments, an `accumulator` and a `value`, which is the current value of the list it it iterating on.  


This `reducer function` is actually a mix of other functions, returned from the `R.Compose`, which performs left to right function composition and returns a function. The resulting function is immediatly called with the arguments `([x, ...acc])` which are passed down ( in reverse ) from the `reducer function`. The `spread operator` on the arguments will turn `[0]` into simply `0` and on each other iteration, will take the array and turn it into individual elements.
What this composed function does is: Adds 1 to each element being iterated over, sums each element of the list together, uses that result as the second argument to 
```javascript
R.flip(R.prepend)(acc), /*argument*/
```
which in turn flips the arguments of the prepend function, which will place the result of the sum of each element of the array at the start of the array.

So we can assume that  `R.reduce` starts with `acc = 0`, and will iterate `13` and `28`


```javascript
// First run through compose
// acc = 0
// x = 13

R.compose(
    R.flip(R.prepend)
      (0) // curried to R.flip result
  , R.sum
  , R.map(R.add(1))
  )
    ([13, 0]) // The ...acc array is spread to the existing array
// Returns 15


// Second run through compose
// acc = 15
// x = 28
R.compose(
    R.flip(R.prepend)
      (15) // curried to R.flip result
  , R.sum
  , R.map(R.add(1))
  )
    ([28, 15, 0]) // The ...acc array is spread to the existing array
// Returns 46
```