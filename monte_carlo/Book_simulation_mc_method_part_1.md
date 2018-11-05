```r
## Inverse Transform method.
## Algorithm 2.3.1 
## Ref: Simulation and the MC method.

## Generate discrete distribution.

mydiscretepdf <- function(val, probability_density, values) {
  
  check <- val  %in% values
  if (sum(check) < length(val)) {
    return('Invalid Value.')
  }
  
  result <- c()
  
  for (each in val) {
    index <- which(x = each == values)
    result <- c(result, probability_density[index])
  }
  
  return(result)
} 

## Generate discrete cdf
mydiscretecdf <- function(val, probability_density, values) {
  
  check <- val  %in% values
  if (sum(check) < length(val)) {
    return('Invalid Value.')
  }
  
  cdf <- c()
  for (i in 1:length(values)) {
    cdf <- c(cdf, sum(probability_density[c(1:i)]))
  }
  
  result <- c()
  
  for (each in val) {
    index <- which(x = each == values)
    result <- c(result, cdf[index])
  }
  
  return(result)
}

## Random variable
U <- runif(1)

## Find Inverse
findinverse <- function(x=U, cdf=mydiscretecdf, 
                        prob_density, values) {
  comparison <- 
    x - cdf(val = values, probability_density = prob_density, 
            values = values)
  
  if (length(comparison[comparison<=0])==0) {
    get0 <- min(comparison)
  } else {
    get0 <- max(comparison[comparison <= 0])
  }
  
  
  return(values[which(x = get0 == comparison)])
  
}

findinverse(x = 0.1, cdf = mydiscretecdf, 
            prob_density = rep(0.1, 10), values = c(1:10))
```




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
