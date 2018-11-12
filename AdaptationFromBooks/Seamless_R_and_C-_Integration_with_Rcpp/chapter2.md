
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

// This is a simple example of exporting a C++ function to R. You can
// source this function into an R session using the Rcpp::sourceCpp 
// function (or via the Source button on the editor toolbar). Learn
// more about Rcpp at:
//
//   http://www.rcpp.org/
//   http://adv-r.had.co.nz/Rcpp.html
//   http://gallery.rcpp.org/
//

// [[Rcpp::export]]
NumericVector timesTwo(NumericVector x) {
  return x * 2;
}

// void findLength(unsigned short val);

void findLength(unsigned short val){
  int j, result;
  result = val;
  j = 0;
  while (result > 0) {
    j = j + 1;
    result = result / 2;
  }
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
