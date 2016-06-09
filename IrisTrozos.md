Objetivo
--------

Particionar Iris en tantos trozos como se quiera aleatoriamente.

### En este ejemplo lo voy a trocear 5 veces

``` r
library(data.table)
library(dplyr)
mi.iris <-data.table(iris)
mi.iris$indice <- row.names(iris)
n <- 5
numrows <- (nrow(mi.iris)/n)/(length(unique(mi.iris$Species)))
numrows
```

    ## [1] 10

Troceo mediante un bucle for
----------------------------

``` r
for (i in 1:n-1){
 if (i==0){
      M <- group_by(mi.iris,Species) 
      assign(paste0("m", i, sep=""), M) 
  }
   S <- sample_n(M,numrows) #De cada especie se queda con 10
   assign(paste0("s", i, sep=""),S) #Genero el nombre que le doy al Data.table dinamicamente.
   M <- setdiff(M,S) #Ahora el Data.table grande del que partimos será M, con las filas quitadas.
   assign(paste0("m",i+1, sep=""),M)
}
```

Comprobación
------------

Si se suman todos los trozos generados, sale lo mismo que si se suma la tabla iris, original.

``` r
sum(s0$Sepal.Length) + sum(s1$Sepal.Length) + sum(s2$Sepal.Length) + 
sum(s3$Sepal.Length) + sum(s4$Sepal.Length)
```

    ## [1] 876.5

Resultado de iris original

``` r
sum(mi.iris$Sepal.Length)
```

    ## [1] 876.5
