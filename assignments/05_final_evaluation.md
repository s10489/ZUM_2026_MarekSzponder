# Zadanie 5. Finalna ewaluacja, analiza błędów i wnioski

**Termin:** niedziela 31 stycznia 2027, 23:59
**Punkty:** 30

## Cel

To etap analizy, a nie kolejny model. Final test służy do oceny dokładnie tego systemu, który został wybrany na validation.

## Wymagania

1. Tabela porównawcza wszystkich podejść (baseline, pretrained, rozszerzenie) z wynikami na validation.
2. Wybór jednego finalnego systemu na podstawie validation, z uzasadnieniem (jakość, koszt, ryzyka).
3. Zapis finalnego systemu w `final_system.md` ([szablon](../templates/final_system_template.md)), łącznie z dokładnym zapisem modelu użytym przy wyborze: checkpointem, serializowanym klasyfikatorem albo nazwą modelu zewnętrznego z dokładną rewizją, jeśli dotyczy. Definicja finalnego systemu jest w [zasadach projektu](../PROJECT_RULES.md), w części 6.
4. Nie trenujesz systemu ponownie na `train + validation` ani na żadnym większym zbiorze.
5. Commit z zapisem systemu i uzasadnieniem wyboru oznaczasz tagiem `final-freeze`.
6. Dopiero potem uruchamiasz final test, jeden raz, tylko dla zamrożonego systemu. Wyniki zapisujesz w `results/final_test/` i raportujesz metrykę główną i pomocnicze.
7. Porównanie wyniku final test z wynikiem validation tego systemu i interpretacja różnicy.
8. Analiza błędów: kategorie błędów, przykłady sukcesów i porażek, na wynikach validation i final test. Przyczyny to hipotezy, a nie diagnozy. Analiza nie może prowadzić do zmian systemu.
9. Analiza kosztu.
10. Ograniczenia, ryzyka generalizacji (inne źródła, domeny, osoby) i czego wynik nie dowodzi.
11. Kwestie etyczne i ryzyka użycia, jeśli są istotne dla problemu.
12. Finalne README z instrukcją reprodukcji.
13. Wniosek: dlaczego wybrany system jest właściwy i czy wynik na final test potwierdza zasadność wcześniejszego wyboru.

Jeśli po teście znajdziesz błąd w kodzie ewaluacji, możesz poprawić samą ewaluację i jawnie opisać to w dzienniku zmian. Jeśli błąd dotyczy zamrożonego systemu, nie poprawiasz systemu. Opisujesz błąd jako ograniczenie.

## Co oddajesz

Link do repozytorium oraz tag `final-freeze` (utworzony przed testem) i tag `task-5`.

## Jak oceniamy

* zgodność z zasadami final test (jeden system, bez ponownego treningu, tag przed testem): 20%;
* porównanie na validation i interpretacja wyniku final test: 20%;
* analiza błędów: 20%;
* ograniczenia, ryzyka i to, czego wynik nie dowodzi: 15%;
* reprodukowalność i README: 15%;
* uzasadnienie wyboru finalnego systemu: 10%.

## Przed oddaniem sprawdź

1. Jeden system jest wybrany na validation i zapisany w `final_system.md`, z dokładnym zapisem modelu użytym przy wyborze i wersją podziału danych.
2. System nie był trenowany ponownie po wyborze.
3. Tag `final-freeze` powstał przed uruchomieniem final test.
4. Final test był uruchomiony tylko dla tego systemu, a wyniki są w `results/final_test/`.
5. README jest kompletne, łącznie z reprodukcją, ograniczeniami i dziennikiem zmian.

[Pełne zasady projektu](../PROJECT_RULES.md)
