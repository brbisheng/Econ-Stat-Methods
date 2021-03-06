
### 1.2 First Example, Fibonacci sequences 

(cautious, I always do some adaptions based on the original code..)

------**1st solution**-----

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


------**2nd solution**-----

```r
## R version..

mfibR <- local({
  # A vector to store results 
  memo <- c(1, 1)
  f <- function(n) {
    if (n%%1 != 0) {return('Input is not integer.')}
    if (n <0 ) {return('Input is negative.')}
    if (n == 0 ) {return(0)}
    if (n<=length(memo)) {return(memo[n])}
    ans <- f(n-2) + f(n-1)
    memo <<- c(memo, ans) 
    ans
  }
})

mfibR(10)


## C++ solution

cppinline1 <- '
#include <Rcpp.h>
#include <algorithm>
#include <vector>
#include <stdexcept>
#include <cmath>
#include <iostream>

using namespace Rcpp;

// [[Rcpp::export]]

class Fib {
  
  public:
    // built constructor..
  Fib(unsigned int n = 3) {
    memo.resize(n);
    std::fill(memo.begin(),memo.end(), NAN);
    memo[0] = 0.0;
    memo[1] = 1.0;
    memo[2] = 1.0;
  };
  // fibonacci method.
    double fibonacci(int n){
      if (n <  0) {
        std::cout << "Negative Input.";
        return(-1.0);
      };
      
      if (n%1 != 0) {
        std::cout << "Input not an integer.";
        return(-1.0);
      };
      
      if (n < memo.size()) {
        return(memo[n]);
      };
      
      double ans;

      ans = fibonacci(n-1) + fibonacci(n-2);

      memo.push_back(ans);

      return(memo[n]);
    }
  private:
    std::vector <double> memo;
};
'


mfibRcpp <- inline::cxxfunction(signature(xs="int"),
                        plugin="Rcpp",
                        includes=cppinline1,
                        body='
                        int x = Rcpp::as<int>(xs);
                        Fib f;
                        return Rcpp::wrap( f.fibonacci(x) );
                        ')
mfibRcpp(10)

microbenchmark::microbenchmark(mfibR(10), mfibRcpp(10))
```

------- **Third Method**-------

```r
######### Part 1, Third example - linear, interative solution

## R version..

fibRiter <- function(n) {
  
  if (n%%1!=0) {return('Invalid Input.')}
  
  ## We only pay attention to 3 consecutive elements each time. 
  
  f_n0  <- 0    ## The first element
  f_n1  <- 1    ## The second element
  f_n2  <- 0    ## The third element
  
  for (i in 1:n) {
    f_n2  <- f_n1 + f_n0
    f_n0  <- f_n1
    f_n1  <- f_n2
  }
  
  return(f_n0)
}

fibRiter(5)

## C++ version

fibRcppIter <- inline::cxxfunction(signature(xs = "int"),
                                   plugin = 'Rcpp',
                                   body = '
                                   int n = Rcpp::as<int>(xs);
                                   double f_n0 = 0;
                                   double f_n1 = 1;
                                   double f_n2 = 0;
                                   for (int i = 0; i<n ;i++) {
                                     f_n2 = f_n0 + f_n1;
                                     f_n0 = f_n1;
                                     f_n1 = f_n2;
                                   }
                                   return Rcpp::wrap(f_n0);

                                   ')
fibRcppIter(10)

microbenchmark::microbenchmark(fibRiter(10), fibRcppIter(10))
```
