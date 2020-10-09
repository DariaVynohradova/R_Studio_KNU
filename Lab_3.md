##### 1. Install packages
```
install.packages("magrittr")
install.packages("rvest")
install.packages("stringr")
```
```
library(rvest)
library(magrittr)
library(stringr)
```
##### 2. Save url and read html file
```
url_data<-"http://www.imdb.com/search/title?count=100&release_date=2017,2017&title_type=feature"
url_file<-read_html(url_data)
```

##### 3. Get rank_data
```
rank_data <- url_file %>% html_nodes(".text-primary") %>% html_text() %>% as.numeric() 
head(rank_data)

[1] 1 2 3 4 5 6
```

##### 4. Get title_data
```
title_data <- url_file %>% html_nodes(".lister-item-header a") %>% html_text() 
head(title_data)

[1] "Нянька"                       "Той, хто біжить по лезу 2049"
[3] "Тор: Раґнарок"                "Воно"                        
[5] "Красуня і Чудовисько"         "Вбивство у Східному експресі"
```

##### 5. Get runtime_data
```
runtime_data <- url_file %>% html_nodes(".text-muted .runtime") %>% html_text() 
head(runtime_data)

[1] "85 min"  "164 min" "130 min" "135 min" "129 min" "114 min"
```
##### Remove 'min' to convert into numeric
```
runtime_data <- runtime_data %>% str_remove('min') %>% as.numeric() 
head(runtime_data)

[1]  85 164 130 135 129 114
```

##### 6. Create data.frame "movies"
```
movies <- data.frame(Rank = rank_data, Title = title_data, Runtime = runtime_data, stringsAsFactors = FALSE)
```
##### Output first 6 movies
```
head(movies)

  Rank                        Title Runtime
1    1                       Нянька      85
2    2 Той, хто біжить по лезу 2049     164
3    3                Тор: Раґнарок     130
4    4                         Воно     135
5    5         Красуня і Чудовисько     129
6    6 Вбивство у Східному експресі     114

```

##### Output all the  movies with runtime more then 120 min
```
movies[movies$Runtime < 120, ]$Title

 [1] "Нянька"                            
 [2] "Вбивство у Східному експресі"      
 [3] "Найвеличніший шоумен"              
 [4] "Гарні часи"                        
 [5] "Пастка"                            
 [6] "Дюнкерк"                           
 [7] "Три білборди під Еббінґом, Міссурі"
 [8] "Леді Бьорд"                        
 [9] "Коко"                              
[10] "На драйві"                         
[11] "Удача Лоґана"                      
[12] "Джуманджі: Поклик джунглів"        
[13] "xXx: Реактивізація"                
[14] "Я, Тоня"                           
[15] "Гра Джеральда"                     
[16] "Рятувальники Малібу"               
[17] "Тебе ніколи тут не було"           
[18] "Життя"                             
[19] "Диво"                              
[20] "Пастка часу"                       
[21] "Баррі Сіл: Король контрабанди"     
[22] "Джунглі"                           
[23] "Іноземець"                         
[24] "Геошторм"                          
[25] "Спекотні літні ночі"               
[26] "Вітряна ріка"                      
[27] "Дух в оболонці"                    
[28] "Щасливий день смерті"              
[29] "Пригоди Паддінгтона 2"             
[30] "Конг: Острів черепа"               
[31] "Темна вежа"                        
[32] "Ритуал"                            
[33] "Розваги дорослих дівчат"           
[34] "П'ятдесят відтінків темряви"       
[35] "Гора між нами"                     
[36] "Тілоохоронець кілера"              
[37] "Американський убивця"              
[38] "Секретне досьє"                    
[39] "Мумія"                             
[40] "Горе-творець"                      
[41] "Яскраві"                           
[42] "1922"                              
[43] "Проект 'Флорида'"                  
[44] "Новизна"                           
[45] "Marshall"                          
[46] "Обдарована"                        
[47] "The Little Hours"                  
[48] "Нескінченність"                    
[49] "Атомна Блондинка"                  
[50] "Сфера"                             
[51] "Тачки 3"                           
[52] "Смерть Сталіна"                    
[53] "Анабель: Створення"                
[54] "Пила 8"                            
[55] "Історія примари"                   
[56] "Воно приходить уночі"              
[57] "Зошит смерті"                      
[58] "Dark River"                        
[59] "Ідеальний голос 3"                 
[60] "Ноша"                              
[61] "Джиперс Кріперс 3"                 
[62] "Помста"                            
[63] "Оселя тіней"                       
[64] "Sand Castle"  
```

##### Count the number of movies with runtime less then 100 min
```
movies %>% subset(movies$Runtime < 100) %>% nrow

[1] 13
```

