```r

## Inverse Transform method.
## Algorithm 2.3.1 
## Ref: Simulation and the MC method.

## Random variable

U <- runif(1)

## Define your distribution function

myCDF <- function(x) {
  pnorm(q = x, mean = 0, sd = 1)
}

## Define the inverse function
inverseCDF <- function(f, 
                       lower = -1e10,
                       upper = 1e10) {
  
  function(y) {uniroot(f = (function(x) {f(x)-y}),
                              lower = lower,
                              upper = upper)[1]}
}

inverseCDF(f = myCDF)(U)
```
