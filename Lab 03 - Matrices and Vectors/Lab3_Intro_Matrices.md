PLS 801: Lab 3 - Intro to Matrices and Vectors
================
Constanza F. Schibber
September 14, 2017

# Agenda for Today:

1.  Creating Vectors and Matrices
2.  Structure of Vectors and Matrices
3.  Arithmetic and Algebraic Operations

# Creating Vectors and Matrices

There are several possible ways of creating a *vector* or *matrix*
object.

## Vectors

- A vector of specific values: e.g., $\mathbf{a}=[3,4,5]$

``` r
a <- c(3,4,5)
a
```

    ## [1] 3 4 5

``` r
# You can check whether an object is a vector
is.vector(a)
```

    ## [1] TRUE

``` r
# You can also get the length of a vector
length(a)
```

    ## [1] 3

- A vector of a repeated number: E.g., $\mathbf{b}=[1,1,1,1,1]$

``` r
b <- rep(1, 5)
b
```

    ## [1] 1 1 1 1 1

**Class Question.** Is an object $b$ a vector? What is its length?

- A vector of consecutive numbers: E.g., $\mathbf{c} = [1,2,3,4,5]$

``` r
c<- c(1:5)
c
```

    ## [1] 1 2 3 4 5

- A vector with an incremental sequence: E.g.,
  $\mathbf{d} = [-1, -0.5, 0, 0.5, 1]$

``` r
d <- seq(-1, 1, by=0.5)
d
```

    ## [1] -1.0 -0.5  0.0  0.5  1.0

## Matrices

- A matrix filled with the same constant: E.g.,
  $\mathbf{A}= \begin{bmatrix}2 & 2 & 2 \\2 & 2 & 2 \\ 2 & 2 & 2 \\2 & 2 & 2\\2 & 2 & 2 \end{bmatrix}$

``` r
A <- matrix(2, ncol=3, nrow=5, byrow=FALSE)
A
```

    ##      [,1] [,2] [,3]
    ## [1,]    2    2    2
    ## [2,]    2    2    2
    ## [3,]    2    2    2
    ## [4,]    2    2    2
    ## [5,]    2    2    2

``` r
# You can check whether an object is a matrix
is.matrix(A)
```

    ## [1] TRUE

``` r
# You can also get the dimension of a matrix
dim(A)
```

    ## [1] 5 3

- A matrix by treating several vectors as columns:  
  E.g., Create a matrix $\mathbf{B}$, whose columns are $\mathbf{c}$ and
  $\mathbf{d}$

``` r
B <- cbind(c,d)
B
```

    ##      c    d
    ## [1,] 1 -1.0
    ## [2,] 2 -0.5
    ## [3,] 3  0.0
    ## [4,] 4  0.5
    ## [5,] 5  1.0

**Class Question.** Create a matrix $\mathbf{C}$, where the 1st column
is all 1’s and the 2nd and the 3rd columns are vectors $\mathbf{c}$ and
$\mathbf{d}$. <br><br> That is,
$\mathbf{C}= \begin{bmatrix}1 & 1 & -1.0 \\1 & 2 & -0.5 \\ 1 & 3 & 0.0 \\1 & 4 & 0.5 \\1 & 5 & 1.0 \end{bmatrix}$.
Test if an object $\mathbf{C}$ is a matrix. Calculate its dimensions.
<br><br>

- A matrix by treating several vectors as rows: E.g., Create a matrix
  $\mathbf{R}$, whose rows are vectors $\mathbf{c}$ and $\mathbf{d}$

``` r
R<-rbind(c, d)
R
```

    ##   [,1] [,2] [,3] [,4] [,5]
    ## c    1  2.0    3  4.0    5
    ## d   -1 -0.5    0  0.5    1

``` r
# Note R is the transpose of a matrix B. 
t(B)  # B Transpose 
```

    ##   [,1] [,2] [,3] [,4] [,5]
    ## c    1  2.0    3  4.0    5
    ## d   -1 -0.5    0  0.5    1

- A matrix by listing all element, filling either column-by-column or
  row-by-row.  
  E.g., $\mathbf{W} =\begin{bmatrix}1 & 3 \\2 & 4 \end{bmatrix}$

``` r
W <- matrix(c(1,2,3,4), ncol=2, nrow=2)
W
```

    ##      [,1] [,2]
    ## [1,]    1    3
    ## [2,]    2    4

E.g.,
$\mathbf{Z} =\begin{bmatrix}1 & 2 & 3 \\4 & 5 & 6 \\7 & 8 & 9 \end{bmatrix}$

``` r
Z <- matrix(c(1:9), ncol=3, nrow=3, byrow=TRUE)
Z
```

    ##      [,1] [,2] [,3]
    ## [1,]    1    2    3
    ## [2,]    4    5    6
    ## [3,]    7    8    9

- You can ask to print a particular element of a matrix using
  subscripting:

``` r
#view an element of a vector a
a
```

    ## [1] 3 4 5

``` r
a[3]
```

    ## [1] 5

``` r
#view a column of a matrix
B
```

    ##      c    d
    ## [1,] 1 -1.0
    ## [2,] 2 -0.5
    ## [3,] 3  0.0
    ## [4,] 4  0.5
    ## [5,] 5  1.0

``` r
B[,2]   # the second column of a matrix B
```

    ## [1] -1.0 -0.5  0.0  0.5  1.0

``` r
#use column name to call a matrix column
B[,"d"]
```

    ## [1] -1.0 -0.5  0.0  0.5  1.0

``` r
#view a row of a matrix
B[1,]   # the first row of a matrix B
```

    ##  c  d 
    ##  1 -1

- Another way to create a matrix is to begin with a blank matrix and
  then reasign element by element.

``` r
#create a blank matrix 
BLANK <- matrix(NA, ncol=3, nrow=3)
BLANK
```

    ##      [,1] [,2] [,3]
    ## [1,]   NA   NA   NA
    ## [2,]   NA   NA   NA
    ## [3,]   NA   NA   NA

``` r
#fill in values of the blank matrix piecewise
BLANK[1,1] <- 2
BLANK[2,1] <- pi
BLANK[,2] <- a    # use a vector a to fill in the second column 
#check our progress
BLANK
```

    ##          [,1] [,2] [,3]
    ## [1,] 2.000000    3   NA
    ## [2,] 3.141593    4   NA
    ## [3,]       NA    5   NA

``` r
#You can create row and column names for a matrix
rownames(BLANK)<-c('ID1','ID2','ID3')  # add row names
colnames(BLANK)<-c("var1", "var2", "var3")  # add column names
colnames(BLANK)[1]<-"one"  # change the first column name 
#check our progress
BLANK
```

    ##          one var2 var3
    ## ID1 2.000000    3   NA
    ## ID2 3.141593    4   NA
    ## ID3       NA    5   NA

**Class Question.** Fill in the values and make a $\mathbf{BLANK}$
matrix as following:
$\mathbf{BLANK}=\begin{bmatrix} 2 & 3 & 1 \\3.141593 & 4 & 2 \\NA & 5 & 3 \end{bmatrix}$

## Special Matrices

- Diagonal matrix

E.g.,
$\mathbf{D}=\begin{bmatrix} 1 & 0 & 0 & 0\\0 & 2 & 0 & 0 \\0 & 0 & 3 & 0 \\0 & 0 & 0 & 4 \end{bmatrix}$

``` r
D <- diag(c(1:4),4,4)  #diag(x,nrow,ncol)
D
```

    ##      [,1] [,2] [,3] [,4]
    ## [1,]    1    0    0    0
    ## [2,]    0    2    0    0
    ## [3,]    0    0    3    0
    ## [4,]    0    0    0    4

``` r
#You can ask R to print the diagonal elements of a sqaure matrix
diag(D)
```

    ## [1] 1 2 3 4

``` r
#To calculate the trace of D you could do
sum(diag(D))
```

    ## [1] 10

- Identity matrix

E.g.,
$\mathbf{I}=\begin{bmatrix} 1 & 0 & 0 & 0 & 0\\0 & 1 & 0 & 0 & 0\\0 & 0 & 1 & 0 & 0\\0 & 0 & 0 & 1 & 0 \\0 & 0 & 0 & 0 & 1 \end{bmatrix}$

``` r
I <- diag(1,5,5)
I
```

    ##      [,1] [,2] [,3] [,4] [,5]
    ## [1,]    1    0    0    0    0
    ## [2,]    0    1    0    0    0
    ## [3,]    0    0    1    0    0
    ## [4,]    0    0    0    1    0
    ## [5,]    0    0    0    0    1

- J matrix

E.g.,
$\mathbf{J}=\begin{bmatrix} 1 & 1 & 1 & 1 & 1\\1 & 1 & 1 & 1 & 1\\1 & 1 & 1 & 1 & 1\\1 & 1 & 1 & 1 & 1 \\1 & 1 & 1 & 1 & 1 \end{bmatrix}$

``` r
J <- matrix(1, ncol=5, nrow=5)
J
```

    ##      [,1] [,2] [,3] [,4] [,5]
    ## [1,]    1    1    1    1    1
    ## [2,]    1    1    1    1    1
    ## [3,]    1    1    1    1    1
    ## [4,]    1    1    1    1    1
    ## [5,]    1    1    1    1    1

- Triangular matrix

E.g., Given a matrix
$\mathbf{Z} =\begin{bmatrix}1 & 2 & 3 \\4 & 5 & 6 \\7 & 8 & 9 \end{bmatrix}$,
the lower triangular matrix is
$\mathbf{Z_{LT}} =\begin{bmatrix}1 & 2 & 3 \\0 & 5 & 6 \\0 & 0 & 9 \end{bmatrix}$
and the upper triangular matrix is
$\mathbf{Z_{UT}} =\begin{bmatrix}1 & 0 & 0 \\4 & 5 & 0 \\7 & 8 & 9 \end{bmatrix}$.

``` r
# e.g., given a matrix Z, create the lower trinagular matrix
lower.tri(Z, diag=FALSE)
```

    ##       [,1]  [,2]  [,3]
    ## [1,] FALSE FALSE FALSE
    ## [2,]  TRUE FALSE FALSE
    ## [3,]  TRUE  TRUE FALSE

``` r
Z_lt <- Z
Z_lt[lower.tri(Z)] <- 0
Z_lt
```

    ##      [,1] [,2] [,3]
    ## [1,]    1    2    3
    ## [2,]    0    5    6
    ## [3,]    0    0    9

``` r
# e.g., given a matrix Z, create the upper trinagular matrix
upper.tri(Z, diag=FALSE)
```

    ##       [,1]  [,2]  [,3]
    ## [1,] FALSE  TRUE  TRUE
    ## [2,] FALSE FALSE  TRUE
    ## [3,] FALSE FALSE FALSE

``` r
Z_ut <- Z
Z_ut[upper.tri(Z)] <- 0
Z_ut
```

    ##      [,1] [,2] [,3]
    ## [1,]    1    0    0
    ## [2,]    4    5    0
    ## [3,]    7    8    9

# Structure of Vectors and Matrices

As shown above, `length( )` gives the length of a vector, `dim( )` gives
the dimension of a matrix. The transposition of a matrix is `t( )`. You
can also ask to calculate important characteristics of a matrix, the
trace and determinant.

``` r
#the trace of a matrix
sum(diag(Z))   #tr(Z)
```

    ## [1] 15

``` r
#the determinant of a matrix
det(W)
```

    ## [1] -2

``` r
det(Z)
```

    ## [1] 6.661338e-16

The inverse of a matrix is calculated using `solve( )`.

``` r
P <- matrix(c(5,6,7,8), ncol=2, nrow=2)
P   
```

    ##      [,1] [,2]
    ## [1,]    5    7
    ## [2,]    6    8

``` r
solve(P)  
```

    ##      [,1] [,2]
    ## [1,]   -4  3.5
    ## [2,]    3 -2.5

``` r
# Checking whether we get the identity matrix back
P%*%solve(P) # You can see that the result has some rounding issues
```

    ##      [,1]         [,2]
    ## [1,]    1 3.552714e-15
    ## [2,]    0 1.000000e+00

``` r
round(P%*%solve(P)) # With rounding, you can now see the identity matrix
```

    ##      [,1] [,2]
    ## [1,]    1    0
    ## [2,]    0    1

But not all matrices are invertible. The matrix $\mathbf{Z}$ is
singular.

``` r
#Not all square matrix are invertible
Z
```

    ##      [,1] [,2] [,3]
    ## [1,]    1    2    3
    ## [2,]    4    5    6
    ## [3,]    7    8    9

``` r
solve(Z) 
```

    ## Error in solve.default(Z): system is computationally singular: reciprocal condition number = 2.59052e-18

**Class Question.** Compute the inverse of a matrix $\mathbf{W}$. Is the
multiplication of the original matrix ($\mathbf{W}$) and its inverse
($\mathbf{W}^{-1}$) is equal to $\mathbf{I}$?

``` r
# STUDENTS: FILL IN ANSWER HERE
```

# Arithmetic and Algebraic Operations

| Operator         | Description                                                 |
|------------------|-------------------------------------------------------------|
| `a * b`          | Inner Product of Vectors                                    |
| `A + B`          | Matrix Addition                                             |
| `A - B`          | Matrix Subtraction                                          |
| `A * s`          | Scalar Multiplication                                       |
| `A / s`          | Scalar Division                                             |
| `A %*% B`        | Matrix multiplication. $\mathbf{AB}$                        |
| `kronecker(A,B)` | Kronecker product. $\mathbf{A\otimes B}$                    |
| `A %o% B`        | Outer product. $\mathbf{AB}'$                               |
| `solve(A)`       | Inverse of A, where A is a sqaure matrix. $\mathbf{A}^{-1}$ |

In the table, $\mathbf{A}$ and $\mathbf{B}$ are matrices and $s$ is a
scalar.

$~$

**Class Question.** Answer the following questions using R.

Perform some math arithmetic using the following vectors and matrices.

$\mathbf{A} =\begin{bmatrix} -1\\ 3\\ 4 \end{bmatrix}$
$\mathbf{B} = \begin{bmatrix} 3\\ -2\\ 1 \end{bmatrix}$  
$\mathbf{C} = \begin{bmatrix} 3 & 2 & -4\\ -8 & 0 & 6 \end{bmatrix}$
$\mathbf{D} = \begin{bmatrix} 6 & -2\\ -1 & 3\\ -3 & 8 \end{bmatrix}$

1.  $3\mathbf{A}-2\mathbf{B}$
2.  $\mathbf{CA}$
3.  $\mathbf{BD}$
4.  $\mathbf{B}\otimes \mathbf{D}$
5.  $\mathbf{CD}$
6.  $\mathbf{C}^\prime \mathbf{C}$

``` r
A <- matrix(c(-1,3,4), ncol=1, nrow=3, byrow=TRUE)
B <- matrix(c(3,-2,1), ncol=1, nrow=3, byrow=TRUE)
C <-matrix(c(3,2,-4,
            -8,0,6), ncol=3, nrow=2, byrow=TRUE)
D <-matrix(c(6,-2,
            -1,3,
            -3,8), ncol=2, nrow=3, byrow=TRUE)
```
