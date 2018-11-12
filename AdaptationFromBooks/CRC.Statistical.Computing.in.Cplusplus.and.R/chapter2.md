

### The objective is to find the binary representation of an arbitrary Integer.
- The 1st step is to find the length of binary representation
- The 2nd step is to find the binary representation

```r
##### Run the following lines in r.
sourceCpp("ch2.cpp")
myfunc(5)

##### The following is from a .cpp file!
# include <Rcpp.h>
# include <iostream>
# include <cmath>

using namespace Rcpp;
using namespace std;

// void findLength(unsigned short val);

int findLength(unsigned short val){
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
  // cout << j-1;
  return j-1;
  }

void printBinary(int val, int len) {
  for (int i = len; i >= 0; i--) {
    if (val & (1 << i)) {
      cout << "1";
    } else {
      cout << "0";
    }
  }
};

// [[Rcpp::export]]
void myfunc(unsigned short inVal) {
  
  int bitlength;
  bitlength = findLength(inVal);
  
  printBinary(inVal, bitlength);
  //cout << endl;
  //return bitlength;
};




```
