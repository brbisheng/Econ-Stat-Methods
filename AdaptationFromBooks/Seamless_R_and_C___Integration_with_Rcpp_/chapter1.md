
### 1.2 First Example, Fibonacci sequences 

**1st solution**

```r
library(Rcpp)

############# Part 1, First example

## R code
fibR <- function(n) {
  
  ## Check whether the input is an integer.
  if (n<0) {return('Input is negative.')}
  
  if (n%%1 != 0) {
    return('The input is not an integer.')
  }
  
  if (n == 0 ) return(0)
  
  if (n == 1 ) return(1)
  
  fib_n <- fibR(n-1) + fibR(n-2) 
  return(fib_n)
}

fibR(-1)


## Cplusplus code
cppinline <- '
int fibcpp(const int n) {
  if (n%1!=0) {
    std::cout << "The input is not an integer.";
    return(atoi("no"));
  }
  if (n<0) {
    std::cout << "The input is negative.";
    return atoi("no");
  }

  if (n==0) return 0; 
  if (n==1) return 1; 
  int fib_n = fibcpp(n-1) + fibcpp(n-2);
  return fib_n;
}'

cppFunction(cppinline)

fibcpp(-1)

microbenchmark::microbenchmark(fibR(20), fibcpp(20))
```
