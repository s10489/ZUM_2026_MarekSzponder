# Zadanie 1. Problem, dane i protokół eksperymentu

**Termin:** niedziela 8 listopada 2026, 23:59
**Punkty:** 15

## Cel

Zanim cokolwiek wytrenujesz, ustalasz, co mierzysz i jak, a final test zamrażasz.

## Wymagania

1. Problem klasyfikacji i pytanie eksperymentalne.
2. Jednostka generalizacji z uzasadnieniem: do jakich przyszłych danych ma działać model.
3. Audyt danych:
    * liczba przykładów i rozkład klas;
    * braki danych;
    * duplikaty (co najmniej dokładne, a jeśli to możliwe, także bardzo podobne);
    * podstawowe statystyki wejścia (długość tekstu, rozmiar obrazu, czas trwania nagrania);
    * kilka przykładów.
4. Liczba niezależnych jednostek generalizacji w train, validation i final test, obok liczby przykładów. Na przykład 8000 nagrań może pochodzić tylko od 24 mówców. Wtedy podajesz zarówno liczbę nagrań, jak i liczbę mówców w każdym zbiorze. Jeżeli liczba niezależnych jednostek w validation albo final test jest mała, skomentuj, jak może to wpływać na stabilność estymaty.
5. Potencjalne źródła wycieku danych w Twoim zbiorze i sposób, w jaki im zapobiegasz. Typowe źródła to te same osoby, nagrania, dokumenty albo źródła po obu stronach podziału, duplikaty, augmentacje i zależności czasowe.
6. Podział train, validation i final test zgodny z jednostką generalizacji: grupowy, stratyfikowany, czasowy albo oficjalny, z uzasadnieniem.
7. Metryka główna i pomocnicze, z uzasadnieniem (na przykład macro F1 przy nierównych klasach).
8. Seed i zasady losowości.
9. Licencja i podstawowa dokumentacja danych: skąd pochodzą, jak oznaczono klasy, znane ograniczenia.
10. Zamrożenie final test. Identyfikatory przykładów każdego zbioru zapisujesz w `splits/` ([opis formatu](../splits/README.md)), robisz commit i tworzysz tag `task-1`. Od tej chwili nie uruchamiasz na final test żadnego modelu. Jeśli później musisz poprawić podział, stosujesz procedurę z [zasad projektu](../PROJECT_RULES.md) (część 6, „Poprawa podziału po Zadaniu 1”).

## Co oddajesz

Link do repozytorium i tag `task-1`. README zawiera uzupełnione sekcje „Problem i pytanie eksperymentalne”, „Dane i licencja” oraz „Protokół”. Repozytorium zawiera notebook albo skrypt audytu i pliki podziału.

## Jak oceniamy

* problem, pytanie i jednostka generalizacji: 20%;
* audyt danych, w tym liczba niezależnych jednostek: 25%;
* analiza wycieku danych i projekt podziału: 30%;
* metryka, seed, licencja i dokumentacja danych: 15%;
* czytelność repozytorium: 10%.

## Przed oddaniem sprawdź

1. Pliki z identyfikatorami train, validation i final test są w `splits/`.
2. W README jest tabela z liczbą przykładów i niezależnych jednostek w każdym zbiorze.
3. Commit ma tag `task-1` i tag jest wypchnięty na GitHub.
4. Żaden model nie był uruchamiany na final test.

[Pełne zasady projektu](../PROJECT_RULES.md)
