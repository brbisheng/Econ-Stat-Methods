
As you could see, there are a lot of parameters to play around with, especially the gambling strategy part.

```r
library(ggplot2)
library(magrittr)

## Gambler's wealth.

initial_wealth_gambler <- 100   ## Initial wealth of gambler
initial_wealth_house   <- 2000  ## Initial wealth of house

gamble_once <- function(wealth_gambler = initial_wealth_gambler,
                        wealth_house = initial_wealth_house,
                        initial_w_g = initial_wealth_gambler,
                        initial_w_h = initial_wealth_house) {

  while ((wealth_gambler[length(wealth_gambler)]>0) & 
         (wealth_gambler[length(wealth_gambler)]< 
          (initial_w_g + initial_w_h))) {
    
    ## gain == 0/1
    gain <- rbinom(n = 1, size = 1, prob = 0.5) 
    
    ## gamble strategy
    gi <- seq(from = 10, to = 50, length.out = 5) ## gambling intensity
    
    pgi <- seq(from = 50, to = 10, length.out = 5)/sum(gi)## probability density of gambling intensity
    
    d <- sample(x = gi,
                size = 1, 
                prob = pgi)
    
    ## gain == +/-1 * d
    gain <- (2 * gain - 1) * d
    
    
    
    wealth_gambler <- 
      c(wealth_gambler, wealth_gambler[length(wealth_gambler)] + gain) 
    
    wealth_house <- 
      c(wealth_house, wealth_house[length(wealth_house)] - gain)
  }
  
  df <- data.frame(wg = wealth_gambler,
                   wh = wealth_house)
  
  return(df)
}

## Test gamble_once

winner <- gamble_once()

ggplot(data = winner, 
       mapping = aes(x = 1:nrow(winner),
                     y = winner[,1])) + 
  geom_line()

winner[nrow(winner),]  

## Gamble several times

replications <- 200

result <- c()

for (i in 1:replications) {
  result <- c(result, nrow(gamble_once()))
}

## Basic visualization of histogram
ggplot(data=NULL, mapping=aes(x=result)) + 
  geom_histogram(fill = 'lightblue', color = 'red')

## Quantiles
quantile(x = result, probs = seq(0,1,length.out = 21), type = 3)

```
