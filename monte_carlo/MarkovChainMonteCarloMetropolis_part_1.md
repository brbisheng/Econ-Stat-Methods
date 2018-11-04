
## rolling two dies. 

The proposal function is 'minimal neighborhoods'

```r
## Markov Chain Monte Carlo Sampling Metropolis Algorithm

## Rolling a two dies

## Proposal: Minimal Neighborhoods

## Target / True frequency: 

f <- c(0:6,5:1)

## Number of data points sampled

N <- 2000

# Proposal 'matrix'

g <- function(x, ycut) {
  if (x == 2) {
    y <- ifelse(ycut < 0.5, 2, 3)
  } else if (x==12) {
    y <- ifelse(ycut < 0.5, 11, 12)
  } else {
    y <- ifelse(ycut < 0.5, yes = x-1, no = x+1)
  }
  return(y)
}

# Acceptance 'criterion'


h <- function(x,y) {
  value <- min(1, f[y]/f[x])
  return(value)
}

# Loop

x0 <- 5 # initialize x

result <- c()

for (i in 1:N){
  Ug <- runif(1)
  if (i == 1) {
    xold <- x0
  }
  xnew <- g(x = xold, ycut = Ug)
  
  Uh <- runif(1)
  if (Uh < h(x = xold,y = xnew)) {
    xold <- xnew
  }
  result <- c(result, xold)
}

ggplot(data=NULL, mapping=aes(x=factor(result))) +
  geom_histogram(stat='count', alpha = 0.5, color = 'red', fill='green')

```
