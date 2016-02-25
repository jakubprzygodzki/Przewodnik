# Jak wczytywać tabele z formatu HTML?

Czasem dane które chcemy wczytać są umieszczone w postaci tabeli na stronie HTML.

Takie dane można odczytać na różne sposoby.
Jeżeli tabela HTML jest poprawnie sformatowana, to najwygodniej jest wykorzystać funkcję `readHTMLTable` z pakietu `XML`. Wczytuje ona całą stronę internetową do R, następnie ją analizuje tak by wyłuskać z niej tabele. Wynikiem działania tej funkcji jest lista z tabelami znalezionymi na stronie.

Prześledźmy działanie tej funkcji na bazie strony Wikipedii o reprezentacji Polski w piłce nożnej
http://pl.wikipedia.org/wiki/Reprezentacja_Polski_w_pi%C5%82ce_no%C5%BCnej. 

NA ten stronie znajduje się wiele tabel. Wczytajmy je wszystkie i zobaczmy ile ich jest.


```r
library(XML)
html <- "https://pl.wikipedia.org/wiki/Reprezentacja_Polski_w_pi%C5%82ce_no%C5%BCnej"
tabele <- readHTMLTable(html, stringsAsFactors = FALSE)
```

```
## Error: XML content does not seem to be XML: 'https://pl.wikipedia.org/wiki/Reprezentacja_Polski_w_pi%C5%82ce_no%C5%BCnej'
```

```r
length(tabele)
```

```
## [1] 0
```



```r
# Tabel jest dużo, a ta której szukamy jest akurat 19
zwyciestwa <- tabele[[19]]

library(dplyr)
zwyciestwa$ProcentZwyciestw <- as.numeric(as.character(zwyciestwa[,2])) /
                                 as.numeric(as.character(zwyciestwa[,5]))
arrange(zwyciestwa, ProcentZwyciestw)
```


![Pozycja Polski w rankingu FIFA wg. Wikipedii](rankingFifa.png)





```r
# Odczytanie treści strony www
html <- readLines("http://pl.wikipedia.org/wiki/Reprezentacja_Polski_w_pi%C5%82ce_no%C5%BCnej")
html <- paste(html, collapse="")

# Wydzieramy dane łyżeczką
tmp1 <- strsplit(html, split="Reprezentacja Polski w Rankingu FIFA")[[1]][5]
tmp2 <- strsplit(tmp1, split="najlepsze miejsce")[[1]][1]
tmp3 <- strsplit(tmp2, split="<[^>]+>")[[1]]
wartosci <- na.omit(as.numeric(tmp3))

# i rysunek pozycji reprezentajci Polski w rankingu FIFA
barplot(wartosci[wartosci < 1000], las=1, col="black")
```
