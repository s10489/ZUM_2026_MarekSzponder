# Zasady projektu ZUM 2026/2027

## 1. Cel projektu

W projekcie przeprowadzasz jeden wiarygodny eksperyment nadzorowanej klasyfikacji na prawdziwym problemie. Zaczynasz od pytania i danych, budujesz uczciwy punkt odniesienia, wykorzystujesz model pretrained, a kończysz niezależną oceną wybranego systemu i analizą błędów.

Celem nie jest najwyższy możliwy wynik. Celem jest eksperyment, któremu można zaufać: z jasną jednostką generalizacji, bez wycieku danych, z uczciwym porównaniem, z opisanym kosztem i z odpowiedzią na pytanie, czego wynik nie dowodzi.

Po zakończeniu projektu potrafisz:
1. zaprojektować protokół z osobnymi zbiorami train, validation i final test;
2. zbudować uczciwy baseline;
3. wykorzystać model pretrained i sprawdzić, co faktycznie daje w Twoim zadaniu;
4. uzasadnić i przeprowadzić jedno rozszerzenie;
5. przeanalizować błędy, koszt i ograniczenia;
6. udokumentować projekt tak, żeby dało się go odtworzyć.

## 2. Typ projektu

Projekt dotyczy wyłącznie nadzorowanej klasyfikacji jednolabelowej. Każdy przykład ma dokładnie jedną klasę docelową.

W tej edycji głównym problemem projektu nie może być:
* regresja;
* klasyfikacja wieloetykietowa;
* segmentacja;
* detekcja;
* ASR;
* retrieval;
* generative ML.

Pracujesz na tekście, obrazie, audio albo danych multimodalnych. Detekcja, segmentacja, ASR, retrieval albo element generatywny mogą występować tylko jako komponent systemu, jeśli końcowym wynikiem nadal jest jedna przewidywana klasa.

## 3. Zasady ogólne

1. **Projekt jest indywidualny.**
2. **Modalność.** Projekt multimodalny jest możliwy, jeśli połączenie modalności wynika z problemu.
3. **Dane.** Potrzebujesz jawnego źródła, licencji pozwalającej na użycie w projekcie i etykiet klas odpowiednich do problemu. Własny zbiór danych jest możliwy po akceptacji tematu.
4. **Prywatność i dane wrażliwe.** Jeżeli dane zawierają informacje osobowe, wrażliwe albo podlegają ograniczeniom dostępu lub redystrybucji, nie umieszczaj w publicznym repozytorium danych, próbek ani identyfikatorów umożliwiających identyfikację osób. Sposób wykorzystania takich danych musi zostać zaakceptowany przy zgłoszeniu tematu. Dodatkowe wymagania formalne zależą od rodzaju danych i zasad uczelni.
5. **Zasoby.** Projekt ma dać się wykonać na darmowych zasobach (Google Colab, Kaggle). Dobierz wielkość danych i modeli do dostępnej mocy obliczeniowej. Zadania 1 i 2 nie wymagają GPU.
6. **Biblioteki.** Wybierasz je tak, jak wymaga problem. Najczęściej będą to scikit-learn, PyTorch i Hugging Face.
7. **Preprocessing.** Stosujesz tylko kroki uzasadnione przez dane, reprezentację i model, a dla każdego kroku wyjaśniasz, po co jest. Wszystko, co uczy się z danych (słownik, skalowanie, normalizacja, selekcja cech), dopasowujesz wyłącznie na train. Nie wykonujesz kroków tylko dlatego, że „tak się zwykle robi”. Pominięcie kroku też jest decyzją: jeśli w zadaniu sentymentu nie usuwasz stop słów, napisz dlaczego.
8. **Ocena.** Liczy się poprawność metodologiczna, uzasadnienie decyzji i jakość analizy. Projekt z niższym F1, wykonany poprawnie i dobrze przeanalizowany, może dostać więcej punktów niż projekt z bardzo wysokim wynikiem uzyskanym przy wycieku danych.
9. **Jedno repozytorium.** Rozwijasz to samo repozytorium przez cały semestr, a każde oddanie wskazuje konkretną wersję (tag Git).
10. **Narzędzia AI.** Możesz korzystać z narzędzi AI. Odpowiadasz za kod i za każdą decyzję i musisz umieć je uzasadnić.
11. **Zmiana tematu.** Do terminu Zadania 1 wystarczy poinformować prowadzącą. Później zmiana wymaga jej zgody.
12. **Informacja zwrotna.** Informacja zwrotna jest przekazywana etapami. Błędy metodologiczne, które trzeba poprawić przed kolejnym etapem, sygnalizujemy w pierwszej kolejności.

## 4. Trzy poziomy projektu

Na każdym poziomie rozwiązujesz ten sam problem klasyfikacji.

**Poziom A. Punkt odniesienia.** Prosty, sensowny baseline odpowiedni dla modalności. Przykłady:
* tekst: TF IDF i LogisticRegression;
* obraz: prosta reprezentacja (na przykład zmniejszone piksele albo histogramy) i klasyczny klasyfikator;
* audio: podstawowe cechy albo prosta reprezentacja (na przykład statystyki log mel spectrogramu) i klasyfikator.

**Poziom B. Podejście pretrained.** Co najmniej jedno podejście pretrained. Zalecany pierwszy krok to zamrożony encoder i prosty klasyfikator, jeśli ma to sens dla danego problemu. Fine tuning nie jest obowiązkowy.

**Poziom C. Jedno uzasadnione rozszerzenie.** Odpowiada na to samo pytanie projektu i nie zmienia zmiennej docelowej ani problemu klasyfikacyjnego. Kluczowe pytanie brzmi: dlaczego to rozszerzenie ma sens dla Twojego problemu?

## 5. Terminy i punktacja

Wszystkie terminy przypadają w niedzielę o godzinie 23:59.

| Etap | Instrukcja | Termin | Punkty | Co oddajesz |
|---|---|---|---|---|
| Zgłoszenie tematu | [00_topic_submission.md](assignments/00_topic_submission.md) | 18 października 2026 | 0, obowiązkowe | wypełniony szablon zgłoszenia |
| Zadanie 1 | [01_data_and_protocol.md](assignments/01_data_and_protocol.md) | 8 listopada 2026 | 15 | link do repozytorium i tag `task-1` |
| Akceptacja artykułu spoza listy | [scientific_paper_review.md](assignments/scientific_paper_review.md) | 22 listopada 2026 | 0, obowiązkowe dla źródeł spoza listy | pełny opis bibliograficzny oraz DOI albo oficjalny link |
| Zadanie 2 | [02_baseline.md](assignments/02_baseline.md) | 29 listopada 2026 | 15 | link i tag `task-2` |
| Artykuł naukowy | [scientific_paper_review.md](assignments/scientific_paper_review.md) | 6 grudnia 2026 | 10, poza projektem | plik PDF albo Markdown |
| Zadanie 3 | [03_pretrained.md](assignments/03_pretrained.md) | 20 grudnia 2026 | 20 | link i tag `task-3` |
| Zadanie 4 | [04_extension.md](assignments/04_extension.md) | 17 stycznia 2027 | 20 | link i tag `task-4` |
| Zadanie 5 | [05_final_evaluation.md](assignments/05_final_evaluation.md) | 31 stycznia 2027 | 30 | link oraz tagi `final-freeze` i `task-5` |

Projekt jest wart łącznie 100 punktów. Artykuł naukowy jest osobnym zadaniem za 10 punktów.

## 6. Zasady final test

### Finalny system

Finalny system to wszystko, co jest potrzebne, żeby z danych wejściowych otrzymać przewidywaną klasę. Przy tagu `final-freeze` zapisujesz go w pliku `final_system.md` ([szablon](templates/final_system_template.md)):

1. model;
2. zapisany model albo checkpoint, jeśli dotyczy: dokładnie ten model, który był użyty przy wyborze na validation. Może to być checkpoint modelu neuronowego, serializowany klasyfikator scikit-learn (na przykład `.joblib`), model pretrained z zewnętrznego repozytorium albo plik lub konfiguracja potrzebna do odtworzenia systemu. Dla modelu pobieranego z zewnętrznego repozytorium zapisujesz nazwę repozytorium albo modelu oraz dokładną rewizję, wersję albo identyfikator commita, jeśli dostawca je udostępnia. Jeśli nie udostępnia, zapisujesz dostępną wersję i piszesz wprost, której informacji o wersji nie udało się jednoznacznie ustalić;
3. preprocessing;
4. reprezentację;
5. sposób agregacji reprezentacji (na przykład pooling), jeśli dotyczy;
6. klasyfikator trenowany na frozen features, jeśli dotyczy;
7. hiperparametry;
8. próg decyzyjny, jeśli istnieje;
9. seed;
10. wersje bibliotek;
11. wersję danych i podziału (commit albo wersję plików splitu użytych do finalnego systemu).

Duże pliki modelu, na przykład wagi sieci, przechowujesz poza repozytorium (na przykład na Dysku Google), a w zapisie systemu podajesz, gdzie są i jak je wczytać. Mały zapisany model, na przykład klasyfikator scikit-learn w pliku `.joblib`, możesz umieścić w repozytorium (proponowane miejsce: `results/final_system/`).

### Zasady

1. Final test wydzielasz w Zadaniu 1, zgodnie z jednostką generalizacji. Identyfikatory zapisujesz w `splits/`, robisz commit i tworzysz tag `task-1`. Podział możesz później zmienić tylko według procedury z punktu „Poprawa podziału po Zadaniu 1”.
2. Do końca Zadania 4 final test nie jest używany.
3. Wszystkie porównania modeli i wybór systemu odbywają się na validation. Strojenie hiperparametrów walidacją krzyżową w obrębie train jest dozwolone jako część trenowania.
4. Wynik na validation używany do wyboru modelu nie jest niezależną oceną końcową.
5. Wybierasz dokładnie jeden finalny system.
6. Zapisujesz dokładny model, checkpoint albo inną formę zapisu modelu, jeśli dotyczy, oraz pełną konfigurację w `final_system.md`.
7. Tworzysz tag `final-freeze`.
8. Nie wykonujesz ponownego treningu na `train + validation`.
9. Nie wykonujesz dodatkowego dopasowania (refitu) na żadnym innym zbiorze.
10. Final test służy do oceny dokładnie tego systemu, który został wcześniej wybrany na validation.
11. Final test uruchamiasz tylko dla jednego systemu.
12. Wynik testu nie może zmienić systemu: modelu, zapisanego modelu lub checkpointu, progu, hiperparametrów, reprezentacji ani preprocessingu.
13. Jeśli po teście znajdziesz błąd w kodzie ewaluacji (na przykład w liczeniu metryki), możesz poprawić samą ewaluację i jawnie opisać to w dzienniku zmian.
14. Jeśli błąd dotyczy zamrożonego systemu, nie poprawiasz systemu po teście. Błąd opisujesz jako ograniczenie.
15. Jeśli oficjalny test zbioru nie ma etykiet albo nie pasuje do jednostki generalizacji, wydzielasz final test samodzielnie i to uzasadniasz.
16. Jeśli final test posłużył do wyboru, uruchomiono na nim kilka systemów albo system trenowano ponownie po wyborze na validation, wynik final test traktujemy jako estymatę deweloperską. Tracisz wtedy punkty za kryterium zasad final test w Zadaniu 5.

### Poprawa podziału po Zadaniu 1

Jeżeli po Zadaniu 1, ale przed jakimkolwiek użyciem final test, wykryjesz błąd metodologiczny wymagający zmiany podziału, możesz go poprawić. Nie przesuwasz ani nie usuwasz tagu `task-1`. Tworzysz nowy commit z poprawionymi plikami splitu, opisujesz zmianę w README i w dzienniku zmian oraz ponownie wykonujesz wszystkie eksperymenty, których wyniki zależały od starego podziału. Od tego momentu poprawiona wersja splitu jest zamrożona. W `final_system.md` wskazujesz dokładny commit albo wersję plików splitu użytych do finalnego systemu.

Jeżeli final test został już użyty przed wykryciem błędu splitu, nie traktujemy poprawionego testu jako niezależnej oceny bez dodatkowego komentarza metodologicznego. Taka sytuacja wymaga konsultacji z prowadzącą.

## 7. Repozytorium i oddawanie

### Utworzenie repozytorium

1. Utwórz repozytorium z szablonu („Use this template”, potem „Create a new repository”).
2. Nazwij je `ZUM_2026_ImięNazwisko`.
3. Repozytorium może być publiczne albo prywatne. Jeśli jest prywatne, dodaj konto prowadzącej wskazane na platformie kursu. Przy danych osobowych, wrażliwych albo z ograniczeniami redystrybucji stosuj zasadę 4 z części „Zasady ogólne”.

### Oddawanie

1. Każde oddanie zadania projektowego zawiera link do repozytorium i nazwę tagu. Tagi to `task-1` do `task-5`, a w Zadaniu 5 także `final-freeze`.
2. Oceniamy stan repozytorium z otagowanego commita. Późniejsze zmiany nie wpływają na ocenę danego zadania.
3. Nie przesuwaj ani nie usuwaj tagów po oddaniu.

### Wymagania

1. README z sekcjami: informacje ogólne; problem i pytanie eksperymentalne; dane i licencja; protokół; wyniki; finalny system (od Zadania 5); reprodukcja; ograniczenia; dziennik zmian; licencja projektu. Szkielet jest w dolnej części pliku [README.md](README.md).
2. Plik środowiska (`requirements.txt` albo `environment.yml`). Najpóźniej przy każdym oddaniu zapisujesz w nim wersje faktycznie użytych bibliotek, a w README wersję Pythona.
3. Pełnych danych nie wrzucasz do repozytorium. Dodajesz instrukcję albo skrypt pobierania, a jeśli licencja i zasada o prywatności na to pozwalają, małą próbkę w `data/sample/`.
4. Pliki podziału danych (identyfikatory) są w `splits/` ([opis formatu](splits/README.md)). Jeśli identyfikatory mogłyby umożliwić identyfikację osób, sposób ich przechowywania uzgadniasz przy zgłoszeniu tematu.
5. Wyniki zapisujesz w małych plikach w `results/`, żeby dało się je sprawdzić bez ponownego trenowania.
6. Notebooki zapisujesz z wynikami i mają działać od początku do końca. Zamiast nich możesz użyć skryptów.
7. Nie wrzucasz dużych wag modeli. Opisujesz, gdzie są przechowywane i jak je wczytać albo odtworzyć.

## 8. Kryteria oceniania

| Kryterium | Oczekiwany standard | Co obniża ocenę |
|---|---|---|
| Protokół | jednostka generalizacji, podział zgodny z nią, liczba niezależnych jednostek, zamrożony final test, jeden system oceniony na final test bez ponownego treningu | wyciek danych, wybór na final test, uruchamianie na final test kilku systemów, ponowny trening po wyborze na validation |
| Uzasadnienie decyzji | reprezentacja, preprocessing, metryka i model wynikające z problemu; rozszerzenie związane z pytaniem projektu | kroki wykonane „bo tak się robi”, rozszerzenie zmieniające zmienną docelową albo typ problemu |
| Uczciwe porównanie | te same dane, metryka i dyscyplina wyboru na validation; koszt obok jakości | porównania na różnych danych albo bez kosztu |
| Analiza | błędy, ograniczenia, odpowiedź, czego wynik nie dowodzi | sam ranking modeli |
| Reprodukowalność | środowisko, seed, instrukcja, kod działający od początku do końca, kompletny zapis finalnego systemu | brak instrukcji, kod niedziałający, niejasne, który system oceniono |
| Dokumentacja | czytelne README, dziennik zmian, jawne braki informacji | brak opisu decyzji, zgadywanie faktów |

**Wynik a metodologia.** Wysokość wyniku nie jest sama w sobie kryterium. Projekt z niższym F1, wykonany poprawnie i dobrze przeanalizowany, może dostać więcej punktów niż projekt z bardzo wysokim wynikiem uzyskanym przy wycieku danych.

**Wyciek danych.** Wyciek obniża punkty za protokół i porównanie w zadaniach, których dotyczy. Jeśli poprawisz protokół w kolejnym zadaniu i opiszesz to w dzienniku zmian, możesz dostać pełne punkty za kolejne etapy.

## 9. Spóźnienia

Zasady dotyczące spóźnień zostaną podane na platformie kursu.

## 10. Przykładowe zbiory i na co uważać

| Zbiór | Fakt ważny dla protokołu | Konsekwencja |
|---|---|---|
| IMDb (recenzje filmów) | oficjalne train i test są rozłączne na poziomie filmów | przy tworzeniu validation zachowaj tę zasadę, jeśli masz dostęp do identyfikatora filmu; jeśli nie, opisz to jako ograniczenie |
| PolEmo 2.0, KLEJ (recenzje po polsku) | dane z wykładów W2, W5 i W6 | jeśli oficjalny test nie ma etykiet, final test wydziel z danych z etykietami przed modelowaniem |
| ESC-50 (dźwięki otoczenia) | 5 oficjalnych foldów; klipy z jednego nagrania źródłowego są w jednym foldzie | nie rozdzielaj klipów z jednego `src_file` między train i test |
| Speech Commands (słowa kluczowe) | oficjalne listy validation i test rozdzielają mówców | nie losuj podziału po plikach |
| RAVDESS (emocje w głosie) | 24 aktorów | jeżeli celem jest generalizacja na nowych mówców, dziel dane po aktorach; sposób podziału powinien odpowiadać docelowemu scenariuszowi użycia modelu |
| GTZAN (gatunki muzyczne) | udokumentowane powtórzenia, zniekształcenia i błędne etykiety | wymaga szczególnej ostrożności albo innego zbioru |

## 11. Checklista przed każdym oddaniem

1. Commit ma właściwy tag, a na platformie podajesz link do repozytorium i nazwę tagu.
2. README jest zaktualizowane, łącznie z dziennikiem zmian.
3. W Zadaniach 1 do 4 final test nie był używany.
4. Wszystko, co uczy się z danych, jest dopasowane tylko na train.
5. Notebooki albo skrypty działają od początku do końca na czystym środowisku.
6. Seed, wersja Pythona i wersje faktycznie użytych bibliotek są zapisane.
7. W repozytorium nie ma pełnych danych, dużych wag modeli ani danych umożliwiających identyfikację osób.
8. Licencja danych pozwala na sposób, w jaki ich używasz.
9. Wyniki są w plikach w `results/` i zgadzają się z README.
10. Każda decyzja ma krótkie uzasadnienie, a brakujące informacje są opisane wprost.
