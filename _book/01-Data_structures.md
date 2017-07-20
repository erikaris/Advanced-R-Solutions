
# Data structures

## Vectors

1. __<span style="color:red">Q</span>__: What are the six types of atomic vector? How does a list differ from an
   atomic vector?  
__<span style="color:green">A</span>__: The six types are logical, integer, double, character, complex and raw. The elements of a list
don't have to be of the same type.

2. __<span style="color:red">Q</span>__: What makes `is.vector()` and `is.numeric()` fundamentally different to
   `is.list()` and `is.character()`?  
__<span style="color:green">A</span>__: The first two tests don't check for a specific type.

3. __<span style="color:red">Q</span>__: Test your knowledge of vector coercion rules by predicting the output of
   the following uses of `c()`: 

    
    ```r
    c(1, FALSE)      # will be coerced to numeric   -> 1 0
    c("a", 1)        # will be coerced to character -> "a" "1"
    c(list(1), "a")  # will be coerced to a list (since not all elements are atomic)
                     # with two elements of type double and character 
    c(TRUE, 1L)      # will be coerced to integer   -> 1 1
    ```

4.  __<span style="color:red">Q</span>__: Why do you need to use `unlist()` to convert a list to an 
    atomic vector? Why doesn't `as.vector()` work?  
__<span style="color:green">A</span>__: To get rid of (flatten) the nested structure.
Recall that lists are considered vectors, so `as.vector()` will leave it the same.
`as.vector()` can be used to convert a non-recursive list to an atomic vector if
the desired `mode` is passed explicitly. But the core problem is that `as.vector()`
cannot handle recursion within lists.  Only `unlist()` (or `purrr::flatten()`
functions) can do that.

5. __<span style="color:red">Q</span>__: Why is `1 == "1"` true? Why is `-1 < FALSE` true? Why is `"one" < 2` false?  
__<span style="color:green">A</span>__: These operators are all functions which coerce their arguments (in these cases) to character, double and character. To enlighten the latter case: "one" comes after "2" in ASCII.

6. __<span style="color:red">Q</span>__: Why is the default missing value, `NA`, a logical vector? What's special
   about logical vectors? (Hint: think about `c(FALSE, NA_character_)`.)  
__<span style="color:green">A</span>__: It is a practical thought. When you combine `NA`s in `c()` with other atomic types they will be coerced
like `TRUE` and `FALSE` to integer `(NA_integer_)`, double `(NA_real_)`, complex `(NA_complex_)` and character `(NA_character_)`. Recall that in R there is a hierarchy of recursion that
goes logical -> integer -> double -> character.  If `NA` were, for example, a character,
including `NA` in a set of integers or logicals would result in them getting coerced
to characters which would be undesirable. Making `NA` a logical means that involving an `NA`
in a dataset (which happens often) will not result in coercion.

## Attributes

1.  __<span style="color:red">Q</span>__: An early draft used this code to illustrate `structure()`:

    
    ```r
    structure(1:5, comment = "my attribute")
    #> [1] 1 2 3 4 5
    ```

    But when you print that object you don't see the comment attribute.
    Why? Is the attribute missing, or is there something else special about
    it? (Hint: try using help.) \index{attributes!comment}
    
    __<span style="color:green">A</span>__: From the help of comment `(?comment)`:  
    
    > Contrary to other attributes, the comment is not printed (by print or print.default).
    
    
    Also from the help of attributes `(?attributes)`:  
    
    > Note that some attributes (namely class, comment, dim, dimnames, names, row.names and tsp) are treated specially and have restrictions on the values which can be set.
    
    
2.  __<span style="color:red">Q</span>__: What happens to a factor when you modify its levels? 
    
    
    ```r
    f1 <- factor(letters)
    levels(f1) <- rev(levels(f1))
    ```
    
    __<span style="color:green">A</span>__: Both, the entries of the factor and also its levels are being reversed:
    
    
    ```r
    f1
    #>  [1] z y x w v u t s r q p o n m l k j i h g f e d c b a
    #> Levels: z y x w v u t s r q p o n m l k j i h g f e d c b a
    ```
    

3.  __<span style="color:red">Q</span>__: What does this code do? How do `f2` and `f3` differ from `f1`?

    
    ```r
    f2 <- rev(factor(letters)) # changes only the entries of the factor
    f3 <- factor(letters, levels = rev(letters)) # changes only the levels of the factor
    ```
    
    __<span style="color:green">A</span>__: Unlike `f1` `f2` and `f3` change only one thing. They change the order of the factor or its levels, but not both at the same time.

## Matrices and arrays

1.  __<span style="color:red">Q</span>__: What does `dim()` return when applied to a vector?  
__<span style="color:green">A</span>__: `NULL`

2.  __<span style="color:red">Q</span>__: If `is.matrix(x)` is `TRUE`, what will `is.array(x)` return?  
    __<span style="color:green">A</span>__: `TRUE`, as also documented in `?array`:
    
    > A two-dimensional array is the same thing as a matrix.

3.  __<span style="color:red">Q</span>__: How would you describe the following three objects? What makes them
    different to `1:5`?

    
    ```r
    x1 <- array(1:5, c(1, 1, 5)) # 1 row, 1 column, 5 in third dimension
    x2 <- array(1:5, c(1, 5, 1)) # 1 row, 5 columns, 1 in third dimension
    x3 <- array(1:5, c(5, 1, 1)) # 5 rows, 1 column, 1 in third dimension
    ```
    
    __<span style="color:green">A</span>__: They are of class array and so they have a `dim` attribute.

## Data frames

1.  __<span style="color:red">Q</span>__: What attributes does a data frame possess?  
__<span style="color:green">A</span>__: names, row.names and class.

2.  __<span style="color:red">Q</span>__: What does `as.matrix()` do when applied to a data frame with 
    columns of different types?  
    __<span style="color:green">A</span>__: From `?as.matrix`:
    
    > The method for data frames will return a character matrix if there is only atomic columns and any non-(numeric/logical/complex) column, applying as.vector to factors and format to other non-character columns. Otherwise the usual coercion hierarchy (logical < integer < double < complex) will be used, e.g., all-logical data frames will be coerced to a logical matrix, mixed logical-integer will give a integer matrix, etc.

3.  __<span style="color:red">Q</span>__: Can you have a data frame with 0 rows? What about 0 columns?  
__<span style="color:green">A</span>__: Yes, you can create them easily. Also both dimensions can be 0:

    
    ```r
    # here we use the recycling rules for logical subsetting, but you could
    # also subset with 0, a negative index or a zero length atomic (i.e.
    # logical(0), character(0), integer(0), double(0))
    
    iris[FALSE,]
    #> [1] Sepal.Length Sepal.Width  Petal.Length Petal.Width  Species     
    #> <0 rows> (or 0-length row.names)
    
    iris[ , FALSE] # or iris[FALSE]
    #> data frame with 0 columns and 150 rows
    
    iris[FALSE, FALSE] # or just data.frame()
    #> data frame with 0 columns and 0 rows
    ```