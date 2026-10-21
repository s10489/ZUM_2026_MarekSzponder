# Podział danych

W Zadaniu 1 zapisujesz tu identyfikatory przykładów każdego zbioru. Proponowany format to trzy pliki CSV:

```text
train_ids.csv
validation_ids.csv
test_ids.csv
```

Każdy plik ma kolumnę `id` (identyfikator przykładu). Jeśli dzielisz dane po grupach, dodaj kolumnę `group_id` z identyfikatorem jednostki generalizacji, na przykład mówcy albo nagrania źródłowego.

Zasady:
1. Pliki podziału trafiają do commita z tagiem `task-1`.
2. Jeśli identyfikatory mogłyby umożliwić identyfikację osób, sposób ich przechowywania uzgadniasz przy zgłoszeniu tematu.
3. Jeżeli po Zadaniu 1, ale przed jakimkolwiek użyciem final test, wykryjesz błąd metodologiczny wymagający zmiany podziału, możesz go poprawić. Nie przesuwasz ani nie usuwasz tagu `task-1`. Tworzysz nowy commit z poprawionymi plikami splitu, opisujesz zmianę w README i w dzienniku zmian oraz ponownie wykonujesz wszystkie eksperymenty, których wyniki zależały od starego podziału. Od tego momentu poprawiona wersja splitu jest zamrożona. W `final_system.md` wskazujesz dokładny commit albo wersję plików splitu użytych do finalnego systemu.
4. Jeżeli final test został już użyty przed wykryciem błędu splitu, nie traktujemy poprawionego testu jako niezależnej oceny bez dodatkowego komentarza metodologicznego. Taka sytuacja wymaga konsultacji z prowadzącą.

Pełne zasady: [zasady projektu](../PROJECT_RULES.md), część 6.
