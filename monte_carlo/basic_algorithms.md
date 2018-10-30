```{r}
## Using uniformly distribution variable to produce two independent draws from N(0,1)

set.seed(123)

u1 <- runif(1, min = 0, max = 1)

u2 <- runif(1, min = 0, max = 1)

log0 <- function(x) {
  if (x==0) {
    return(0)
  } else {
    return(log(x))
  }
}

x1 <- ((-2 * log0(u1))^(-1/2)) * cos(2 * pi * u2)

x2 <- ((-2 * log0(u1))^(-1/2)) * sin(2 * pi * u2)
```
