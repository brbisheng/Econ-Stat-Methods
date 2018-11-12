
### The objective is to find the binary representation of an arbitrary Integer.
- The 1st step is to find the length of binary representation
- The 2nd step is to find the binary representation

```r
## Run in r.
sourceCpp("ch2.cpp")
myfunc(5)

## The following a .cpp file!
# include <Rcpp.h>
# include <iostream>
# include <cmath>

using namespace Rcpp;
using namespace std;

// [[Rcpp::export]]
NumericVector timesTwo(NumericVector x) {
  return x * 2;
}

// void findLength(unsigned short val);

void findLength(unsigned short val){
  int j, quotient;
  quotient = val;
  j = 0;
  // Find the power of the leading term.
  while (quotient > 0) {
    j = j + 1;
    quotient = quotient / 2;
  }
  // The power of the leading term is j-1.
  // However, the length of the binary representation is j.
  cout << j-1;
  }

// [[Rcpp::export]]

int myfunc(unsigned short inVal) {
  
//  cout << "Enter an integer: ";
//  cin >> inVal;
//  cout << "Your number is of length: ";
  findLength(inVal);
  cout << endl;
  return -1;
}





```
