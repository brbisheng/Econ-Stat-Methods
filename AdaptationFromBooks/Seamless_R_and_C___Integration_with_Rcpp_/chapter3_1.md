
### Run in R

```r
## First example

src = '
Rcpp::IntegerVector vec(3); // The initial size is 3.
vec[0] = 1;
vec[1] = 2;
vec[2] = 3;
return vec;
'

func <- 
  inline::cxxfunction(signature(),src,
                      plugin = "Rcpp")

func()


## Second example

src <- '
Rcpp::IntegerVector vec_cpp(vec_type);
int result_product = 1;
for (int i =0; i<vec_cpp.size(); i++) {
  result_product = result_product * vec_cpp[i];
};
return Rcpp::wrap(result_product);
'

func <- 
  inline::cxxfunction(signature(vec_type="integer"),
                      src, plugin = "Rcpp")

func(c(1,10,9))

## Second example variation

src <- '
Rcpp::IntegerVector vec_cpp(vec_type);

int result_product;

result_product = 
std::accumulate(vec_cpp.begin(), vec_cpp.end(), 
                1, std::multiplies<int>());

return Rcpp::wrap(result_product);
'

func <- 
  inline::cxxfunction(signature(vec_type="integer"),
                      src, plugin = "Rcpp")

func(c(1,10,9))
```
