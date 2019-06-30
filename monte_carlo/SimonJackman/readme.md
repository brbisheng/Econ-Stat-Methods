The objective:
1. Replicationg of all the results without using the BUGS/WinBUGS/JAGS package.
2. Thoughts and variations on theories and methods.

# Chapter III
```r
## Uniform, mean, standard deviation, and quantile.

set.seed(123)

temp <- list() ## to store data

## parameters
temp$T <- 20e2
temp$lb <- 0
temp$ub <- 1
temp$qt <- 0.95

## function to generate data
temp$dgp <- function(n_iter = temp$T, lower = temp$lb, upper = temp$ub) {
    result <- list()
    
    # store data and stats
    result$theta <- runif(n=n_iter, min = lower, max = upper) 
    result$theoretical_mean <- (lower + upper)/2 
    result$theoretical_sd <- ((upper-lower)^2)/12 %>% sqrt
    
    return(result)
}

## list to result
temp$dgp.result <- temp$dgp()

## function to generate progressive mean
temp$mean <- function(dt = temp$dgp.result) {
    
    ## Initialize vectors with the same length as theta
    theta_mean <- rep(x = NA, temp$T)
    for (t in 1:length(dt$theta)) {
        theta_mean[t] <- sum(dt$theta[1:t])/t
    }
    return(theta_mean)
}

temp$mean.result <- temp$mean()

temp$sd <- function(dt = temp$dgp.result) {
    
    ## Initialize vectors with the same length as theta
    theta_sd <- rep(x = NA, temp$T)
    for (t in 1:length(dt$theta)) {
        theta_sd[t]   <- (sum((dt$theta[1:t] - temp$mean.result[t])^2)/(t-1)) %>% sqrt
    }
    return(theta_sd)
}

temp$sd.result <- temp$sd()

temp$quantile <- function(dt = temp$dgp.result, arg_q = temp$qt) {
    
    ## Initialize vectors with the same length as theta
    theta_quantile <- rep(x = NA, temp$T)
    for (t in 1:length(dt$theta)) {
        theta_quantile[t] <- quantile(dt$theta[1:t], probs = arg_q)
    }
    return(theta_quantile)
}

temp$quantile.result <- temp$quantile()

temp$bias <- function(dt = temp$dgp.result, stats = list(mean = temp$mean.result,
                                                        sd = temp$sd.result,
                                                        q = temp$quantile.result), arg_q = temp$qt) {
    
    result <- list()
    
    q_value <- arg_q * (temp$ub - temp$lb)

    result[['bias_mean']] <- abs(stats$mean - dt$theoretical_mean) # %>% log10()
    result[['bias_sd']] <- abs(stats$sd - dt$theoretical_sd) # %>% log10()
    result[['bias_q']] <- abs(stats$q - q_value) # %>% log10()
    
    return(result)
}

temp$bias.result <- temp$bias()

graphs <- new.env()
graphs$stats <- function( n = temp$T) {
    p1 <- ggplot(data = NULL, mapping = aes(x = 1:n, y = temp$mean.result)) + 
    geom_line() + geom_hline(yintercept = temp$dgp.result$theoretical_mean, linetype = 'dashed', color = 'red')
    p2 <- ggplot(data = NULL, mapping = aes(x = 1:n, y = temp$sd.result)) + 
    geom_line() + geom_hline(yintercept = temp$dgp.result$theoretical_sd, linetype = 'dashed', color = 'red')
    p3 <- ggplot(data = NULL, mapping = aes(x = 1:n, y = temp$quantile.result)) + 
    geom_line() + geom_hline(yintercept = (temp$ub-temp$lb)*temp$qt, 
                             linetype = 'dashed', color = 'red')
    
    gridExtra::grid.arrange(p1, p2, p3, nrow = 3)
}

graphs$stats()

graphs$bias <- function( n = temp$T) {
    p1 <- ggplot(data = NULL, mapping = aes(x = 1:n, y = temp$bias.result$bias_mean)) + 
    geom_line() + scale_x_continuous(trans='log10') + scale_y_continuous(trans='log10')
    p2 <- ggplot(data = NULL, mapping = aes(x = 1:n, y = temp$bias.result$bias_sd)) + 
    geom_line() + scale_x_continuous(trans='log10') + scale_y_continuous(trans='log10')
    p3 <- ggplot(data = NULL, mapping = aes(x = 1:n, y = temp$bias.result$bias_q)) + 
    geom_line() + scale_x_continuous(trans='log10') + scale_y_continuous(trans='log10')
    
    gridExtra::grid.arrange(p1, p2, p3, nrow = 3)
}

```
