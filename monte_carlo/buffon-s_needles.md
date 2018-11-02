```r

############################################
## Buffon's needle - one trial
## Ref: Explorations in Monte Carlo Methods
##    : https://crunchingnumbers.live/2016/02/01/monte-carlo-simulations-buffons-needle/
############################################

set.seed(123)

l <- 1   ## lenght of the needle
d <- 2   ## distance between borders
N <- 10  ## number of slots

throws <- 10000

x1 <- runif(n = throws, min = 0, max = d * N)
y1 <- runif(n = throws, min = 0, max = d * N)

theta <- 2 * pi * runif(n = throws, min = 0, max = 1)

x2 <- x1 + l * cos(theta)
y2 <- y1 + l * sin(theta)

# (x2-x1)^2 + (y2-y1)^2

criterion <- function(y1,y2) {
   
  #result <- (pmin(y1,y2) - mod(pmin(y1,y2),d) + 1) <= 
  #   (pmax(y1,y2) - mod(pmax(y1,y2),d))
  
  result <- ceiling(pmin(y1,y2)/d) <= 
    floor(pmax(y1,y2)/d)   ## remember to normalize by d!
  
  return(result)
  
  }

prob_hits <- criterion(y1,y2) %>% sum %>% (function(x) x/throws)


## Draw Needles

draw_needles <- function(x1, x2, y1, y2, d, N) {
  
  df <- data.frame(X1 = x1, X2 = x2, 
                   Y1 = y1, Y2 = y2)
  
  ggplot(data = df[sample(x = c(1:nrow(df)),size = 100),]) +
    geom_segment(aes(x = X1, y = Y1, xend = X2, yend = Y2)) + 
    xlim(c(-2*d,d*(N+2))) + ylim(c(-2*d,d*(N+2))) + 
    geom_hline(data = data.frame(border = seq(from = -2*d, to = d*(N+1), 
                                              length.out = N)),
               mapping = aes(yintercept=border), alpha = 0.3)
  
}

draw_needles(x1,x2,y1,y2,d,N)

## piEst

piEst <- function(l,d,prob_hits) {
  if (l <= d) {
    result <- (2/prob_hits) * (l/d)
  } else {
    result <- 
      (2/prob_hits) * ((l/d) - ((l/d)^2-1)^(-1/2) + (1/(pracma::sec(l/d))))
  }
  return(result)
}

piEst(l,d,prob_hits)

```
