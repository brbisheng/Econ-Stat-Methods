```r

library(ggplot2)
library(magrittr)

############################################
## Buffon's needle - one trial
## Ref: Explorations in Monte Carlo Methods
##    : https://crunchingnumbers.live/2016/02/01/monte-carlo-simulations-buffons-needle/
############################################

## Set seed
set.seed(123)

## Parameters

para <- list() ## List to store parameters

para[['l']] <- 1   ## lenght of the needle
para$d <- 2   ## distance between borders
para$N <- 10  ## number of slots

para$throws <- 1000

## Number of trials/iterations 

replications <- 2000

## Data geenration process

dgp_fct <- function(l=para$l,d=para$d,N=para$N,throws=para$throws) {
  x1 <- runif(n = throws, min = 0, max = d * N)
  y1 <- runif(n = throws, min = 0, max = d * N)
  
  theta <- 2 * pi * runif(n = throws, min = 0, max = 1)
  
  x2 <- x1 + l * cos(theta)
  y2 <- y1 + l * sin(theta)
  
  df <- data.frame(X1 = x1, X2 = x2, 
                   Y1 = y1, Y2 = y2)
  return(df)
}

dgp_result <- dgp_fct()

## Obtain empirical probability of hits

## Recall that we should normalize by d!

prob_hits_per_trial <- function(dgp = dgp_result,
                                d = para$d,
                                throws = para$throws) {
  criterion <-  
    ceiling(pmin(dgp$Y1,dgp$Y2)/d) <= floor(pmax(dgp$Y1,dgp$Y2)/d)
  prob_hits <-
    criterion %>% sum %>% (function(x) x/throws)
  
  return(prob_hits)

}

prob_hits_per_trial()

## Draw Needles

draw_needles <- function(dgp = dgp_result, 
                         d = para$d, 
                         N = para$N) {
  # input <- dgp[sample(x = c(1:nrow(dgp)),size = 100),]
  
  ggplot(data = dgp) +
    geom_segment(aes(x = dgp$X1, y = dgp$Y1, 
                     xend = dgp$X2, yend = dgp$Y2)) + 
    xlim(c(-2*d,d*(N+2))) + ylim(c(-2*d,d*(N+2))) + 
    geom_hline(data = data.frame(border = seq(from = -2*d, to = d*(N+1), 
                                              length.out = N)),
               mapping = aes(yintercept=border), alpha = 0.3)
  
}

draw_needles()

## piEst

piEst <- function(l = para$l,d=para$d,
                  prob_hits = prob_hits_per_trial()) {
  if (l <= d) {
    result <- (2/prob_hits) * (l/d)
  } else {
    result <- 
      (2/prob_hits) * ((l/d) - ((l/d)^2-1)^(-1/2) + (1/(pracma::sec(l/d))))
  }
  return(result)
}

piEst()





### Number of experiments == replications

result_vector <- c()

for (i in 1:replications) {
  dgp_result <- dgp_fct()
  result_vector <- 
    c(result_vector, 
      piEst())
}

### Draw results

ggplot(data = NULL, mapping = aes(x = result_vector)) + 
  geom_histogram()
```
