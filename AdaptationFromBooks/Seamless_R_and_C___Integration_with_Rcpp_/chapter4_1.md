## Named vector and DataFrame

```r
### Named vector

src <- '
Rcpp::NumericVector vec = 
Rcpp::NumericVector::create(
  Rcpp::Named("A") = 1.0,
  Rcpp::Named("B") = 2.0,
  Rcpp::Named("C") = 3.0
);

return vec;
'

func <- inline::cxxfunction(signature(), src, plugin = 'Rcpp')

func()


### DataFrame!

src <- '

Rcpp::CharacterVector ch_v1(3);
ch_v1[0] = "male";
ch_v1[1] = "female";
ch_v1[2] = "female";

Rcpp::IntegerVector int_v = Rcpp::IntegerVector::create(7,8,9);

std::vector<std::string> ch_v2(3);
ch_v2[0] = "heavy";
ch_v2[1] = "light";
ch_v2[2] = "light";

Rcpp::DataFrame df = 
Rcpp::DataFrame::create(Rcpp::Named("ch_v1")= ch_v1,Rcpp::Named("ch_v2")= ch_v2,Rcpp::Named("int_v")= int_v);

return df;

'
func <- inline::cxxfunction(signature(), src, plugin = "Rcpp")
func()

```
