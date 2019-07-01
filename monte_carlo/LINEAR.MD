```r
data(wars)
attach(wars)
X <- cbind(EXTENT,DIVERSE,ALLIANCE,DYADS,TEMPOR,DURATION)
y <- SCOPE
detach(wars)
n <- nrow(X); k <- ncol(X)
nu <- 5
num.sims <- 10000
war.samples <- matrix(NA,nrow=num.sims,(ncol=k+n+1))
beta <- rep(1,ncol(X));sigma.sq <- 3;Omega <- 3*diag(n)
b <- solve(t(X) %*% X) %*% t(X) %*% y
yXb <- y-X%*%b
for (i in 1:num.sims) {
Omega.inv <- solve(Omega)
X2.Om <- solve(t(X) %*% Omega %*% X)
b.star <- X2.Om %*% t(X) %*% Omega %*% y
s.sq.star <- t(yXb) %*% Omega.inv %*% (yXb)
u <- y - X %*% beta
beta <- as.vector( rmultinorm(1, b.star, sigma.sq *solve(t(X)
%*% Omega.inv %*% X) ) )
sigma.sq <- 1/rgamma(1,shape=(n-1)/2,rate=s.sq.star/2)
for (j in 1:n) Omega[j,j] <- 1/rgamma(1, shape=(nu+1)/2,
rate=((sigma.sq^(-1))*u^2 + nu)/2 )
war.samples[i,] <- c(beta,sigma.sq,diag(Omega))
}

```
