# Zadanie 2. Punkt odniesienia i pierwszy kompletny eksperyment

**Termin:** niedziela 29 listopada 2026, 23:59
**Punkty:** 15

## Cel

Budujesz pierwszy pełny pipeline: dane, reprezentacja, model, predykcje klas, metryki, interpretacja.

## Wymagania

1. Trivial baseline, na przykład `DummyClassifier`, jako prosty punkt odniesienia.
2. Prosty baseline z poziomu A, z uzasadnieniem wyboru reprezentacji i modelu.
3. Preprocessing tylko uzasadniony, dopasowany wyłącznie na train (najlepiej w `Pipeline`).
4. Wyniki na validation w metryce głównej i pomocniczych oraz macierz pomyłek.
5. Krótki przegląd 3 do 5 błędów z hipotezami, co mogło je spowodować.
6. Czas trenowania i sprzęt.
7. Final test pozostaje nieużywany.

Oceniamy przede wszystkim poprawność metodologiczną, nie wysokość wyniku.

## Co oddajesz

Link do repozytorium i tag `task-2`. Uzupełnij tabelę wyników na validation w README i dopisz wpisy do `results/experiment_log.md` ([szablon dziennika eksperymentów](../templates/experiment_log_template.md)).

## Jak oceniamy

* poprawność pipeline'u i brak wycieku danych: 35%;
* wybór i uzasadnienie baseline: 20%;
* ewaluacja i interpretacja: 25%;
* reprodukowalność i jakość kodu: 20%.

## Przed oddaniem sprawdź

1. Jest trivial baseline i baseline z poziomu A.
2. Wszystko, co uczy się z danych, jest dopasowane tylko na train.
3. Jest macierz pomyłek i przegląd błędów.
4. Wyniki są w `results/` i zgadzają się z README.
5. Final test nie był używany.

[Pełne zasady projektu](../PROJECT_RULES.md)
