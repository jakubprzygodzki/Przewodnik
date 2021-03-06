# Jak dodawać funkcje do pakietu?

Najczęstszym zastosowaniem pakietów jest udostępnianie zbioru funkcji realizujących podobne zadanie.

Aby dodać funkcję do pakietu, wystarczy jej definicję dodać w pliku o rozszerzeniu R do katalogu o nazwie `R`. Podczas ładowania pakietu wczytywane są wszystkie instrukcje z plików `*.R` umieszczonych w katalogu `R`.

Nazwa pliku `*.R` nie musi być taka sama jak nazwa funkcji, ale zwyczajowo nazwy zbliżone, by łatwiej było znaleźć odpowiednią definicję.

Przykładowo, poniżej przedstawiamy definicję funkcji `jakiPrzebieg`, liczącą średni przebieg aut z danego rocznika. 

Definicję tej funkcji należy wkleić do dowolnego pliku z rozszerzeniem `R` w katalogu o nazwie `R`.


```r
jakiPrzebieg <- function(rok = '', auta) {
  wybrane <- filter(auta, Rok.produkcji == rok)
  mean(wybrane$Przebieg.w.km, na.rm=TRUE)
}
```

## Jak dokumentować funkcje?

Dokumentacja dla funkcji i danych przechowywana jest w plikach `Rd` w katalogu `man`. Można ją tworzyć w dowolnym tekstowym edytorze.

Ale wygodniej jest pracować z dokumentacją, gdy jest ona w tym samym pliku co kod R. Z tego powodu sugerowanym rozwiązaniem do tworzenia dokumentacji jest stosowanie coraz bardziej popularnego pakietu `roxygen2`. 

Jak z niego korzystać?

Opis funkcji umieszcza się w tym samym pliku `R` co kod funkcji w wierszach rozpoczynających się od znaków `#' `. 
Pierwsza linia dokumentacji określa jednozdaniowy tytuł funkcji, kolejny akapit to krótki opis funkcji. Następne akapity są traktowane jako rozszerzony opis.

W dokumentacji wykorzystywane są tagi `roxygen2`, rozpoczynające się od znaku `@`.

Najczęściej stosowane tagi to

* `@param` - opis dla określonego parametru opisywanej funkcji
* `@return` - opis dla wyniku funkcji
* `@export` - tag określający, że dana funkcja ma być udostępniona przez pakiet
* `@examples` - blok z przykładami
* `@rdname` - określa nazwę pliku Rd

Minimalna dokumentacja powinna zawierać tytuł oraz opisy argumentów funkcji. Dla przykładowej funkcji `jakiPrzebieg()` zawartość pliku `R` mogłaby wyglądać tak:

```
#' Średni przebieg aut wyprodukowanych w danym roku
#'
#' Funkcja jakiPrzebieg() wyznacza średni przebieg aut,
#' które jako data produkcji mają podany wskazany rok.
#'
#' @param rok Rok, dla którego liczona ma być średnia, domyślnie 2012.
#' @param auta Zbiór danych, na bazie którego liczona ma być średnia.
#'
#' @export

jakiPrzebieg <- function(rok = '2012', auta) {
  wybrane <- filter(auta, Rok.produkcji == rok)
  mean(wybrane$Przebieg.w.km, na.rm=TRUE)
}
```

Po zbudowaniu dokumentacji, w pakiecie opis dla funkcji `jakiPrzebieg()` będzie dostępny po uruchomieniu instrukcji `?jakiPrzebieg`. 

![Pomoc dla funkcji jakiPrzebieg](grafika/manPage.png)

Poniżej przedstawiona jest przykładowa dokumentacja dla funkcji `proton` z pakietu `proton`. Jest ona bardziej rozbudowana, można wiec zobaczyć jak wykorzystywać inne tagi.

![Przykładowa dokumentacja dla funkcji proton()](grafika/pakiet3.png)

O tym jak przetworzyć taki plik na plik z poprawnie zbudowaną dokumentacją, piszemy w rozdziale *Jak budować stworzony pakiet?*.
