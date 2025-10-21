Inicio proyecto
#Tarea 1 Javiera Alfaro
#Variaciones en las canciones de Bob Dylan

#Partimos cargando nuestra base de datos de las canciones de Bob Dylan

library(tidyverse)
library(readr)

dylan_albums <- read_csv("bobdylansongs.csv")
show_col_types = FALSE

#Visualizamos la base de datos utilizando la función glimpse

glimpse(dylan_albums)
#Vemos que la base de datos contiene 4 columnas: year, album, song y lyrics


