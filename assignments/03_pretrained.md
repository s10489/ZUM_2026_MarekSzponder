# Zadanie 3. Reprezentacja i podejście pretrained

**Termin:** niedziela 20 grudnia 2026, 23:59
**Punkty:** 20

## Cel

Sprawdzasz, co daje w Twoim zadaniu reprezentacja z modelu trenowanego wcześniej na dużych danych, w porównaniu z Twoim baseline.

## Wymagania

1. Co najmniej jedno podejście pretrained odpowiednie dla modalności. Zalecany pierwszy krok to zamrożony encoder i prosty klasyfikator, jeśli ma to sens dla Twojego problemu.
2. Opis modelu pretrained na podstawie model card albo publikacji. Odpowiedz na pytanie: **jaki był cel pretrainingu, na jakich danych model był trenowany i jaką reprezentację wykorzystujesz w swoim zadaniu?** Opisz:
    * cel pretrainingu (pretraining objective);
    * dane pretrainingowe, jeśli są podane;
    * architekturę albo rodzaj encodera;
    * sposób uzyskania reprezentacji (na przykład który wektor albo jaki pooling);
    * licencję wag i limit długości wejścia;
    * które parametry zmieniają się w Twoim eksperymencie, a które pozostają zamrożone.

   Jeżeli informacji nie ma w model card ani publikacji, napisz wprost, że nie została podana. Nie zgaduj.
3. Uczciwe porównanie z baseline: ten sam podział, ta sama metryka, ta sama dyscyplina wyboru na validation. Jeśli porównanie nie może być identyczne (na przykład inny limit długości wejścia), opisz różnicę.
4. Koszt obliczeniowy: czas, sprzęt, pamięć.
5. Krótkie porównanie błędów: gdzie podejście pretrained pomaga, a gdzie nie.

## Co oddajesz

Link do repozytorium i tag `task-3`.

## Jak oceniamy

* wybór modelu i opis pretrainingu: 20%;
* poprawna implementacja i uczciwość porównania: 35%;
* opis reprezentacji oraz tego, co się zmienia, a co jest zamrożone: 25%;
* koszt i reprodukowalność: 20%.

## Przed oddaniem sprawdź

1. Opis celu pretrainingu, danych i reprezentacji jest w README; braki informacji są opisane wprost.
2. Porównanie z baseline odbywa się na tym samym podziale i w tej samej metryce.
3. Koszt jest podany.
4. Final test nie był używany.

[Pełne zasady projektu](../PROJECT_RULES.md)
