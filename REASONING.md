# Reasoning Before Execution

Write the rule behind each case before you commit your predictions. Replace
every placeholder line below with one to three sentences that name the rule
producing that case's value, type, length, and dimensions. One rule is enough.
After you run and find a miss, add a short correction entry under that case in a
later commit.

## P01
Atomic vectors get coerced into the same type determined using the ladder. Highest in presence is double, so everything gets coerced to that type, and the logical TRUE becomes numeric. This produces a value **[1,2,3.5]** with storage `type` **double**, a `length` of **3**, and **none** for `dim`.

## P02
Atomic vectors get coerced into the same type determined using the ladder. Highest in presence is character, so all elements in the vector gets coerced into character because of that. The logical `TRUE` is coerced into `"TRUE"`, producing **["1","2","TRUE"]** with `type` **character**, `length` **3**, and `dim` **none**.

Correction: In our predictions table, the vector in value has elements wrapped in extra quotes **[""1"",""2"",""TRUE""]** instead of just **["1","2","TRUE"]** which is why it was reported a failure.

## P03
Everything has the same type (integer), no coercion required. The shorter integer vector `c(1L, 2L)` recycles on itself due to element-wise arithmetic a to form `c(1L, 2L, 1L, 2L)`. Adding the two vectors means adding the values in each corresponding position, resulting to **[11,22,31,42]** with storage `type` **integer**, `length` **4**, and **none** for `dim`.

Correction: In our predictions table, the length was 3 instead of 4 which is a typographical error since our initial reasoning had 4 as length that is why it was reported as a failure.

## P04
Everything has the same type (integer), no coercion required. Named vectors follow the format **[name1=val1,name2=val2]**, and usage of indices access the element in each index. For this case, integers 3 and 1 extracts only the third and first element in that order, which results to **[third=9,first=8]** with storage `type` **integer**, `length` **2**, and **none** for `dim`.

Correction: In our predictions table, the length was 4 instead of 2, which is a typographical error, likely due to copy-pasting, since our initial reasoning had 2 as length that is why it was reported as a failure.

## P05
Everything has the same type (integer), no coercion required. Negative indices exclude the element, and here it specifies position 2 to be excluded from the vector, resulting in **[4,9]** with `type` **integer**, `length` **2**, and `dim` **none**.

## P06
The condition includes the element if it passes the comparison or the is.na() function. Then, subsetting `x` with the logical mask`c(FALSE, TRUE, TRUE, FALSE)` extracts **[NA,12]** with `type` **integer**, `length` **2**, and **none** for `dim`.

## P07
Factors are similar to ordinal, where they are associated with a number underlying each category. Integer storage codes are used to represent the category labels. This produces **factor[low,high,low];levels=[low,high]** with an underlying storage `type` of **integer**, `length` **3**, and `dim` **none**.

## P08
The type list allows for multiple data types without coercion. Because it contains scores and label as the two top-level elements, the list `length` is **2**, and it produces **list{scores=[3,5],label=["A"]}** with storage `type` **list** and `dim` **none**.

## P09
A data frame is list-like, with the difference that it has dimensions so it can be represented physically as a 2-dimensional table of equal-length vector columns. In this case, it stores columns `id` and `passed` as a named list with 2D array attributes attached. This yields **data.frame[2x2]{id=[1,2],passed=[TRUE,FALSE]}** with `type` **list**, `length` **2**, and `dim` **2x2**.

## P10
A matrix fills itself first with `c(1, 2, 3, 4, 5, 6)` in column-major order. Adding a vector `c(10, 20)` recycles `10` across row 1 and `20` across row 2. This returns **matrix\[2x3\]\(11,22,13,24,15,26\)** with `type` **integer**, `length` **6**, and `dim` **2x3**.

## P11
`mean()` calculates the average given the values in a numeric vector. However, if is applied on a vector with a NA, it results in NA. Without `na.rm = TRUE`, `mean()` propagates the missing value, returning **[NA]** with storage `type` **double**, `length` **1**, and `dim` **none**.

## P12
Same as P11, however, the mean with a NA results in the proper mean when na.rm = TRUE is added, which is what happened to this case. `na.rm = TRUE` strips `NA_real_` before averaging. The arithmetic mean of `c(8, 10)` is (8 + 10) / 2 = 9, returning **[9]** with storage `type` **double**, `length` **1**, and `dim` **none**.

## P13
x was initially an atomic vector of type integer, but it was coerced into a double (which is higher in the coercion ladder) when 2.5 was subassigned. This produces **[1,2.5,3]** with `type` **double**, `length` **3**, and `dim` **none**.

## P14
ifelse is a vectorized conditional function that evaluates each element, and in this case it changed NA into 0, doubled the remaining non-NA values, and then capped out the values at 15 via logical subassignment. It then returns **[14,0,15,8]** with storage `type` **double**, `length` **4**, and `dim` **none**.

## P15
Array attaches a 3-dimension attribute to the integer vector 1:8. This stores values in column-major order, giving **array\[2x2x2\]\(1,2,3,4,5,6,7,8\)** with an underlying storage `type` of **integer**, a `length` of **8**, and `dim` **2x2x2**. 

## P16
Indexing an atomic vector with 0 selects no elements, resulting in an empty vector **[]** carrying the `type` of the original atomic vector which in this case is **integer**. It also has **0** `length` and no `dimensions` (**none**).

