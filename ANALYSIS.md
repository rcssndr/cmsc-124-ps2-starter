# Vector and Scalar Evaluation

## Tracing
`clean_scores_vector()` and `clean_scores_scalar()`

## Questions
### Which operations in the vector version act on all elements at once?

### Where does the scalar loop make evaluation order visible?
- In `clean_scores_scalar()`, the evaluation order is strictly visible pass by pass in the `for` loop, enforced by `seq_along(scores)` to account for empty vector of 0 length, as it visits each element index sequentially. Unlike `clean_scores_vector()` where evaluation occurs across all elements at once, `clean_scores_scalar()` allows step-by-step transparency and slot-by-slot conditional branching (`if(is.na(scores[[i]]))`), making the exact line and moment of evaluation for each element explicitly visible. 

### Give one concrete input where careless recycling would change the answer.
- Draft: if adding a short vector to a longer vector with the intention of adding to just the first, say, 2 elements without accounting for the values to recycle to match the longer vector's length.

Say we intend to add bonus points of 2 and 5 to the first two students only.

`scores <- c(Ada=8,Grace=10,Linus=12,Barbara=4)`
`bonus  <- c(2,5)`

Performing `scores + bonus` executes careless recycling since R will recycle bonus from `c(2,5)` to `c(2,5,2,5)` to apply across four elements of `scores`. The resulting output would then be `c(Ada=10,Grace=15,Linus=14,Barbara=9)`, giving Linus and Barbara unintended bonus points and changing the answer.

### Which implementation would you keep in an R program, and what tradeoff does that choice make?
`clean_scores_vector()`
Pros:
- Faster execution speed
- Concise code
- Idiomatic R

Cons:
- Requires understanding of logical vector masking ang vector subassignment syntax


`clean_scores_scalar()`
Pros:
- Step-by-step transparency during debugging
- Slot-by-slot conditional branching

Cons:
- Slower due to interpreter overhead
- Requires pre-allocation and double-bracket indexing to preserve attributes