# Zadanie 4. Adaptacja albo rozszerzenie

**Termin:** niedziela 17 stycznia 2027, 23:59
**Punkty:** 20

## Cel

Wybierasz jedno rozszerzenie z poziomu C i sprawdzasz, czy pomaga odpowiedzieć na pytanie Twojego projektu.

**Zasada.** Rozszerzenie musi pozostawać związane z tym samym pytaniem projektu i tym samym celem eksperymentalnym. Nie może zmienić zmiennej docelowej ani typu problemu klasyfikacyjnego: wynikiem systemu nadal jest jedna przewidywana klasa. Detekcja, segmentacja, ASR, retrieval albo element generatywny mogą być użyte tylko jako komponent systemu, który wspiera ten sam końcowy problem klasyfikacyjny. Nie zmieniasz klasyfikacji na segmentację, ASR ani retrieval jako nowy główny problem.

## Przykłady rozszerzeń

Nie każde pasuje do każdego projektu.

* Tekst: fine tuning; PEFT; embeddingi i wyszukiwanie jako komponent systemu klasyfikacyjnego; cechy wzbogacone przez retrieval (retrieval augmented features), jeśli finalnym zadaniem nadal jest klasyfikacja.
* Obraz: fine tuning pretrained CNN lub ViT; detekcja albo segmentacja tylko jako komponent pipeline'u prowadzącego do klasyfikacji; reprezentacje z uczenia samonadzorowanego (SSL).
* Audio: pretrained audio encoder; fine tuning; ASR jako komponent poprzedzający klasyfikację, jeśli finalnym targetem nadal jest klasa.
* Multimodalność: połączenie modalności w jednym systemie klasyfikacyjnym.
* Generative ML: tylko jeśli wspiera finalny problem klasyfikacyjny, na przykład przez augmentację, generowanie reprezentacji albo pomocniczą analizę.

## Wymagania

1. Uzasadnienie wyboru oparte na wynikach Zadania 3: różnica wyników, typy błędów, wielkość danych, charakter problemu. Opisz, jak rozszerzenie wiąże się z pytaniem projektu.
2. Przed uruchomieniem eksperymentu zapisujesz w README hipotezę i kryterium, po którym ocenisz, czy rozszerzenie pomogło. Ten zapis musi trafić do commita przed eksperymentem.
3. Ten sam protokół co wcześniej. Wybór konfiguracji tylko na validation.
4. Porównanie na validation z baseline i z podejściem pretrained z Zadania 3, w tej samej metryce klasyfikacji.
5. Koszt (czas, sprzęt, pamięć, wielkość zapisanego modelu) obok jakości.
6. Interpretacja: czy hipoteza się potwierdziła i czego wynik nie dowodzi.

## Co oddajesz

Link do repozytorium i tag `task-4`.

## Jak oceniamy

* uzasadnienie, związek z pytaniem projektu i hipoteza: 25%;
* poprawna implementacja i protokół: 35%;
* uczciwe porównanie i koszt: 25%;
* interpretacja: 15%.

## Przed oddaniem sprawdź

1. Hipoteza i kryterium są w commicie wcześniejszym niż eksperyment.
2. Rozszerzenie nie zmienia zmiennej docelowej ani problemu klasyfikacyjnego.
3. Porównanie odbywa się na validation, w tej samej metryce.
4. Final test nie był używany.

[Pełne zasady projektu](../PROJECT_RULES.md)
