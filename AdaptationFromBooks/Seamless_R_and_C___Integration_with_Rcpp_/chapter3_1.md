
### Run in R

 ---- IntegerVector

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

-------- NumericVector

```r
## First example.. using two inputs
src <- '
Rcpp::NumericVector vec(vec_type_in_R);
double p = Rcpp::as<double>(p_type_in_R);
double sum = 0.0;
for (int i = 0; i<=vec.size(); i++){
  sum = sum + pow(vec[i],p);
}
return Rcpp::wrap(sum);
'

fun <- 
  inline::cxxfunction(signature(vec_type_in_R = 'numeric',
                                p_type_in_R = 'numeric'),
                      src, plugin = 'Rcpp')
fun(1:4, 2)


## Second example.. without using clone.

src <- '
Rcpp::NumericVector invec(vec_type_in_R);
Rcpp::NumericVector outvec(vec_type_in_R);
for (int i=0; i<invec.size(); i++){
  outvec[i] = log(invec[i]);
}
return outvec;
'

fun <- 
  inline::cxxfunction(signature(vec_type_in_R = "numeric"),
                      src, plugin = "Rcpp")

x <- c(1:3)

cbind(x,fun(x))

# Variation 1

src <- '

Rcpp::NumericVector invec(vec_input_in_R);
Rcpp::NumericVector outvec = Rcpp::clone(vec_input_in_R);
for (int i=0; i<invec.size(); i++){
  outvec[i] = log(invec[i]);
}
return outvec;

'

fun <- 
  inline::cxxfunction(signature(vec_input_in_R = "numeric"),
                      src, plugin = "Rcpp")

x <- seq(1,3)
cbind(x,fun(x))

# Variation 2

src <- '

Rcpp::NumericVector invec(vec_input_in_R);
Rcpp::NumericVector outvec = log(invec);
return outvec;
'
fun <- 
  inline::cxxfunction(signature(vec_input_in_R = "numeric"),
                      src, plugin = "Rcpp")

x <- seq(1,3)
cbind(x,fun(x))


#######################
### Matrices
#######################

src <- '

Rcpp::NumericMatrix mat = Rcpp::clone<Rcpp::NumericMatrix>(mx);
std::transform(mat.begin(), mat.end(), mat.begin(), ::sqrt);
return mat;
'

func <- 
  inline::cxxfunction(sig = signature(mx = 'numeric'),
                      body = src,
                      plugin = 'Rcpp')

func(matrix(c(1:16),4,4))
```
