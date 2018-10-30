```r
## Using uniformly distribution variable to produce two independent draws from N(0,1)
set.seed(123)
log0 <- function(x) {
  if (x==0) {
    return(0)
  } else {
    return(log(x))
  }
}
two_indp_draws_from_N01 <- function(x) {
  u1 <- runif(1, min = 0, max = 1)
  u2 <- runif(1, min = 0, max = 1)
  x1 <- ((-2 * log0(u1))^(-1/2)) * cos(2 * pi * u2)
  x2 <- ((-2 * log0(u1))^(-1/2)) * sin(2 * pi * u2)
  return(c(x1,x2))
}
two_indp_draws_from_N01()
```
