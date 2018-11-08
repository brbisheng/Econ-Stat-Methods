```r
## Part 4 Stacks and Queues, 

## Array-based Stack, with some modifications

aStack <- setRefClass(Class  = "aStack",
                      fields = list(
                        maxSize    ="integer",
                        topPos     ="integer",
                        arrayStack ="array"
                      ),
                      methods = list(
                        # Initialization function
                        initialize = function(defaultSize=100L,...){
                          topPos     <<- 0L
                          maxSize    <<- defaultSize # 100L
                          arrayStack <<- array(dim = maxSize)
                        },
                        # Check if stack is empty
                        isEmpty=function(){
                          if (topPos == 0L) {
                            print('Empty Stack')
                            return(TRUE)
                          } else {
                            return(FALSE)
                          }
                        },
                        
                        # push value to stack
                        push=function(pushval){
                          if (topPos + 1L > maxSize) {
                            print('Stack is out of memory.')
                          }
                          topPos <<- topPos + 1L
                          arrayStack[topPos] <<- pushval
                        },
                        
                        # Pop value from stack
                        pop=function(){
                          # Check if the stack is empty.
                          if(isEmpty()) {
                            return('The stack is already empty.')
                          } else {
                            popval <- arrayStack[topPos]
                            arrayStack[topPos] <<- NA
                            topPos <<- topPos - 1L
                            return(popval)
                          }
                          
                        },
                        
                        # Function to get size of stack
                        stacksize=function(){
                          ifelse(isEmpty(), return(0L), return(topPos))
                        },
                        
                        # Function to get top value of stack
                        top=function(n){
                          if (isEmpty()) {
                            print('The stack is empty.')
                          } else {
                            topvals <- arrayStack[c(topPos:(topPos - (n-1L)))]
                            return(topvals)
                          }
                          
                        },
                        
                        # Function to get top value of stack
                        tail=function(n){
                          if (isEmpty()) {
                            print('The stack is empty.')
                          } else {
                            tailvals <- arrayStack[c(n:1L)]
                            return(tailvals)
                          }
                          
                        }
                      )
)

k <- aStack$new(defaultSize= 5L)

k$push(1L)
k$push(2L)
k$push(3L)
k$top(2L)
k$tail(2L)
k$pop()
k$push(5L)
k$pop()
k$pop()
k$top()
k

```
