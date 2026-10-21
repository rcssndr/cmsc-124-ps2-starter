# Vector and Scalar Evaluation

## Tracing

**Input**

```r
sample_scores<-c(Ada=8,Grace=NA_real_,Linus=12,Barbara=4)
```

### `clean_scores_vector()`

**`clean_scores_vector(sample_scores)`**

```r
scores=c(Ada=8,Grace=NA,Linus=12,Barbara=4)
```
<center>⇓</center>

```r
returns<-pmin(2*scores,20)
```
<center>⇓</center>

```r
c(Ada=16,Grace=NA,Linus=20,Barbara=8)
```
<center>⇓</center>

```r
returns[is.na(scores)]<-0
```
<center>⇓</center>

```r
c(Ada=16,Grace=0,Linus=20,Barbara=8)
```
<center>⇓</center>

```r
returns
```
**Output**

```r
  Ada  Grace  Linus Barbara
   16      0     20       8
```
***
### `clean_scores_scalar()`

**`clean_scores_scalar(sample_scores)`**
```r
scores=c(Ada=8,Grace=NA,Linus=12,Barbara=4)
```
<center>⇓</center>

```r
out<-setNames(as.double(scores),names(scores))
```
<center>⇓</center>

```r
c(Ada=8,Grace=NA,Linus=12,Barbara=4)
```
<center>⇓</center>

```r
for(i in seq_along(scores))
```
<center>⇓</center>

```r
i=1:Ada
is.na(scores[[1]])  #FALSE
scores[[1]]*2>20    #FALSE
out[[1]]<-min(scores[[1]]*2)
```
<center>⇓</center>

```r
c(Ada=16,Grace=NA,Linus=12,Barbara=4)
```
<center>⇓</center>

```r
i=2:Grace
is.na(scores[[2]])  #TRUE
out[[2]]<-0
```
<center>⇓</center>

```r
c(Ada=16,Grace=0,Linus=12,Barbara=4)
```
<center>⇓</center> 

```r
i=3:Linus
is.na(scores[[3]])  #FALSE
scores[[3]]*2>20    #TRUE
out[[3]]<-20
```
<center>⇓</center>

```r
c(Ada=16,Grace=0,Linus=20,Barbara=4)
```
<center>⇓</center>

```r
i=4:Barbara
is.na(scores[[4]])  #FALSE
scores[[4]]*2>20    #FALSE
out[[4]]<-min(scores[[4]]*2)
```
<center>⇓</center>

```r
c(Ada=16,Grace=0,Linus=20,Barbara=8)
```
<center>⇓</center>

```r
out
```
**Output**
```r
  Ada  Grace  Linus Barbara
   16      0     20       8
```

****
## Questions
### Which operations in the vector version act on all elements at once?
- In 'clean_scores_vector()', there is `2 * scores`, `pmin(2 * scores, 20)`, `is.na(scores)`, and `returns[is.na(scores)] <- 0`. All of the operations in the function have vector operands, which means that those functions act  on all elements of the vector simultaneously.

### Where does the scalar loop make evaluation order visible?
- In `clean_scores_scalar()`, the for loop with `seq_along(scores)` processes each element sequentially, making the evaluation order visible. The `if (is.na(scores[[i]]))` condition allows step-by-step branching for each element, unlike `clean_scores_vector()`, which processes elements together.

### Give one concrete input where careless recycling would change the answer.
Say we intend to add bonus points of 2 and 5 to the first two students only.

`scores <- c(Ada=8,Grace=10,Linus=12,Barbara=4)`
`bonus  <- c(2,5)`

Performing `scores + bonus` will lead to careless recycling since R will recycle bonus from `c(2,5)` to `c(2,5,2,5)` to apply across four elements of `scores`. The resulting output would then be `c(Ada=10,Grace=15,Linus=14,Barbara=9)`, giving Linus and Barbara unintended bonus points and changing the answer.

### Which implementation would you keep in an R program, and what tradeoff does that choice make?
- We would keep `clean_scores_vector()` over `clean_scores_scalar()` in an R program, since it has faster execution speed and a cleaner, more concise code. Moreover, it is written in idiomatic R, which we believe is a good practice when using R. However, this choice trades away the step-by-step transparency of the scalar loops and it would mean that we would have to learn and understand the workings of the language such as how logical vector masking works or what the subassignment syntax is. But, if you think about it in a different light, trading step-by-step loop execution for vector performance and idiomatic style will be beneficial in the long run.