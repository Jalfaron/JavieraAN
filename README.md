---
editor_options: 
  markdown: 
    wrap: 72
---

### Proyecto canciones de Bob Dylan 

#### ¿Cuáles son las palabras que más utiliza?

Un experimento de Javiera Alfaro

Encontré en Kaggle una base de datos con toda la información
discográfica de Bob Dylan (gracias persona que hizo este trabajo). Lo
que queremos hacer es seguir las sagradas instrucciones de aaumaitre en
GitHub, donde analizó las canciones de Taylor Swift. Replicaremos su
trabajo pero con Dylan.

Comenzamos cargándola a nuestro proyecto markdown, utilizando las
librerías tidyverse, readr y tidytext.

```{r}
library(tidyverse)
library(readr)
library(tidytext)
```

```{r}
dylan_albums<- read_csv("bobdylansongs.csv")
show_col_types = FALSE
```

Visualizamos la base de datos utilizando la función glimpse.

```{r}
glimpse(dylan_albums)

```

Vemos que la base de datos contiene 4 columnas: year, album, song y
lyrics.

Ahora, separamos las letras de las canciones en palabras individuales
utilizando la función unnest_tokens.

```{r}
dylan_words<- dylan_albums |>
  unnest_tokens(word, lyrics)
```

Visualizamos la nueva base de datos con las palabras individuales.

```{r}
glimpse(dylan_words)

```

Vemos que la nueva base de datos contiene 5 columnas: year, album, song,
word y un índice adicional.

Ahora, vamos a contar la frecuencia de cada palabra en sus canciones.

```{r}
word_counts<- dylan_words |>
  count(word, sort = TRUE)

```

```{r}
word_clean<- word_counts |>
 anti_join(stop_words)

```

Limpiamos las palabras más comunes que poca información nos entregan del
caracter de las letras de Dylan.

```{r}
word_clean_2 <- word_clean|>
  #count(word, sort = TRUE)|>
  filter(n < 70 & 
         word != "i'm",
         word != "don't",
         word != "it's",
         word != "you're",
         word != "gonna",
         word != "ain't",
         word != "can't",
         word != "i'll",
         word != "there's",
         word != "i've",
         word != "he's")


word_clean_3 <- word_clean_2 |>
  filter(n > 20)
```

Visualizamos las palabras más frecuentes después de la limpieza.

```{r}
glimpse(word_clean_3)

```

VAhora queremos comenzar a mostrar la información de manera "visual".
Para esto usaremos el paquete wordcloud.

```{r}
install.packages("wordcloud2")
```

```{r}
library("wordcloud2")
```

```{r}
wordcloud2(word_clean_3)
```

Generamos una nube de palabras con las palabras más frecuentes en las
letras de las canciones de Bob Dylan, después de la limpieza.

También podemos generar barras donde se muestre la información por la
cantidad de veces utiliza ciertas palabras en sus canciones.

```{r}
word_clean_3 |>
  arrange(desc(n)) |>
  head(10) |>
  ggplot(aes(x = reorder(word, n), y = n)) +
  geom_col(fill = "red") +
  coord_flip() +
  labs(title = "Palabras más frecuentes en las canciones de Bob Dylan",
       x = "Palabras",
       y = "Frecuencia")
```
