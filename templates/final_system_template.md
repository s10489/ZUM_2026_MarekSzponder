# Finalny system

Skopiuj ten plik do `final_system.md` w głównym katalogu repozytorium. Wypełnij go przed utworzeniem tagu `final-freeze`. Po utworzeniu tagu nie zmieniasz systemu.

## Wybór

* Nazwa systemu:
* Uzasadnienie wyboru na podstawie validation (jakość, koszt, ryzyka):
* Wynik na validation (metryka główna i pomocnicze):

## Zapis systemu

1. Model:
2. Zapisany model / checkpoint / rewizja modelu zewnętrznego, jeśli dotyczy:
    * plik albo miejsce przechowywania (dokładnie ten zapis modelu, który był użyty przy wyborze na validation):
    * model zewnętrzny: nazwa repozytorium albo modelu oraz dokładna rewizja, wersja albo identyfikator commita (jeśli dostawca ich nie udostępnia, napisz, której informacji o wersji nie udało się jednoznacznie ustalić):
3. Preprocessing:
4. Reprezentacja:
5. Sposób agregacji reprezentacji (na przykład pooling), jeśli dotyczy:
6. Klasyfikator trenowany na frozen features, jeśli dotyczy:
7. Hiperparametry:
8. Próg decyzyjny, jeśli istnieje:
9. Seed:
10. Wersje bibliotek (albo odwołanie do pliku środowiska):
11. Wersja danych i podziału (commit albo wersja plików w `splits/` użytych do finalnego systemu):

## Przechowywanie i uruchomienie

* Gdzie są duże pliki modelu (na przykład wagi) i jak je wczytać:
* Notebook albo skrypt, który uruchomi final test:
* Potwierdzam, że system nie był trenowany ponownie po wyborze na validation: [tak]
