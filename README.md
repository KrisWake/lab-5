# lab-5
---
title: "lab5"
author: "Kondrasik Kristina"
date: "`r format(Sys.time(), '%d %B, %Y')`"
output: 
  html_document:
    self_contained: no
---


## Задание
 Интерактивная карта-хороплет стран мира (GVis), показатель –
 любой из раздела «Economy & Growth»
 (экономика и рост)
 базы Всемирного банка.

# Интерактивная картограмма  
Построена на *данные об экономике и росту за 2011 год* базы Всемирного банка.  
Adjusted net savings, including particulate emission damage (% of GNI)
```{r, echo=TRUE, message=F, warning=FALSE, cashe=T, results='asis'}
library('WDI')
library('googleVis')
library('data.table')

# пакет с API для WorldBank
# загрузка данных по всем странам, 2011 год, показатель
# Agricultural irrigated land (% of total agricultural land)
dat <- WDI(indicator = 'NY.ADJ.SVNG.GN.ZS', start = 2011, end = 2011)
DT <- data.table(country = dat$country, value = dat$NY.ADJ.SVNG.GN.ZS)
# объект: таблица исходных данных
g.tbl <- gvisTable(data = DT, 
                   options = list(width = 220, height = 400))
# объект: интерактивная карта
g.chart <- gvisGeoChart(data = DT, 
                        locationvar = 'country', 
                        colorvar = 'value', 
                        options = list(width = 600, 
                                       height = 400, 
                                       dataMode = 'regions'))
# размещаем таблицу и карту на одной панели (слева направо)
TG <- gvisMerge(g.tbl, g.chart, 
                horizontal = TRUE, 
                tableOptions = 'bgcolor=\"#CCCCCC\" cellspacing=10')

# вставляем результат в html-документ
print(TG, 'chart')
```


# Карта на основе leaflet  
На этой карте показаны 5 самых популярных университетов Москвы:

* Московский государственный университет
* НИТУ МИСиС Московский горный институт
* МГТУ им. Баумана
* Высшая школа экономики
* РУДН

```{r, echo = T, message=FALSE, warning=F, results='asis'}
library(leaflet)
parks <- data.frame(place = c("МГУ", "МИСиС",
                                 "МГТУ", "ВШЭ", "РУДН"),
latitude = c(55.702597, 55.727187, 55.772662, 55.766821, 55.652338),
longitude = c(37.531758, 37.606629, 37.685591, 37.663037, 37.498458))
parks %>% leaflet() %>% addTiles() %>%
 addMarkers(popup = parks$place,
 clusterOptions = markerClusterOptions()) %>%
 addCircles(weight = 1, radius = 10)
```
