# To parallelize or not to parallelize

Parallel computing involves many calculations/processes  being executed simultaneously in order to solve the same problem.

This can be quite handy when dealing with enormous datasets. A task can be split into numerous subtasks that execute in parallel and drastically increase run time.

## Speedup from parallelization

Unfortunately, parallelizing a task is not necessarily linear. Splitting up a task into 100 parallel subtasks does increase speed by 100 in other words. This is because of ***MATH!!!***

Specifically, Amdahl's law describes the speed up of a algorithm.

* **Amdahl's law:** boring equation

 ![image](https://wikimedia.org/api/rest_v1/media/math/render/svg/e839644b7042451528fa86d608e4ec683acc1173)

 * And a figure to drive it all home! ( depending on the percentage of the task that can be parallelized)
 ![figure](https://upload.wikimedia.org/wikipedia/commons/thumb/e/ea/AmdahlsLaw.svg/600px-AmdahlsLaw.svg.png)

 *** "When a task cannot be partitioned because of sequential constraints, the application of more effort has no effect on the schedule. The bearing of a child takes nine months, no matter how many women are assigned."*** -Michael Scott



 ## Race conditions

 | Thread A	  | Thread B	 |
| ------------- | ------------- |
| 1A: Read variable X | 1B: Read variable X |
| 2A: Add 1 to variable X  | 2A: Add 1 to variable X  |
| 3A: Write back to variable X | 3B: Write back to variable X |

If 1B happens to occur between 1A and 3A, incorrect data will be produced. This is known as a race condition!


## Let's parallelize stuff in R!!

###Looping in R

```python
x <- 5
if(x < 0){
	print("Positive number")
}

x <- -5
if(x < 0){
	print("Positive number")
} else {
	print("Negative number")
}

x <- 0
if (x < 0) {
   print("Negative number")
} else if (x > 0) {
   print("Positive number")
} else
   print("Zero")

> a = c(5,7,2,9)
> ifelse(a %% 2 == 0,"even","odd")

x <- c(2,5,3,9,8,11,6)
count <- 0
for (val in x) {
    if(val %% 2 == 0)  count = count+1
}
print(count)

i <- 1
while (i < 6) {
   print(i)
   i = i+1
}

x <- 1:5
for (val in x) {
    if (val == 3){
        break
    }
    print(val)
}

x <- 1:5
for (val in x) {
    if (val == 3){
        next
    }
    print(val)
}

x <- 1
repeat {
   print(x)
   x = x+1
   if (x == 6){
       break
   }
}
```

###Matrix Manipulations

```python
> B = matrix(
+   c(2, 4, 3, 1, 5, 7),
+   nrow=3,
+   ncol=2)
> B             # B has 3 rows and 2 columns

> t(B)          # transpose of B

> C = matrix(
+   c(7, 4, 2),
+   nrow=3,
+   ncol=1)
> C             # C has 3 rows
> cbind(B, C)

> c(B)
```

###Null Models

```python
#rnorm(n,mean,sd)
#sample(x, size, replace = FALSE, prob = NULL)

>rnorm(100,50,10) -> nullmdl
>hist(nullmdl) #run several times to show randomness
>sample(nullmdl, 10)
>test <- 1:100
>sample(test, 50, replace=F)
>sample(test, 100, replace=T)
```


###Paralellization in R

A. lapply library

```python
lapply(1:3, function(x) c(x, x^2, x^3))
lapply(1:3/3, round, digits=3)
```

B. Parallel library

```python
library(parallel)
```
Calculate the number of cores

```python
no_cores <- detectCores() - 1
```

Initiate cluster

```python
cl <- makeCluster(no_cores)

parLapply(cl, 2:4,
          function(exponent)
            2^exponent)

stopCluster(cl)
```

#Foreach library

```python
library(foreach)
library(doParallel)

cl<-makeCluster(no_cores)
registerDoParallel(cl)
base <- 2

foreach(exponent = 2:4,
        .combine = c)  %dopar%
  base^exponent

foreach(exponent = 2:4,
	      .combine = rbind)  %dopar%
	base^exponent

foreach(exponent = 2:4,
        .combine = list,
        .multicombine = TRUE)  %dopar%
  base^exponent

stopImplicitCluster()
```
