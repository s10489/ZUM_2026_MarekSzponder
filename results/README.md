# Wyniki

Zapisuj tu małe pliki z wynikami, żeby dało się je sprawdzić bez ponownego trenowania.

Proponowany układ:

```text
experiment_log.md        dziennik eksperymentów
validation/              metryki i predykcje na validation dla każdego podejścia
final_system/            małe pliki finalnego systemu, na przykład klasyfikator .joblib
final_test/              wyniki final test, tylko po utworzeniu tagu final-freeze
```

Szablon dziennika eksperymentów: [experiment_log_template.md](../templates/experiment_log_template.md).

Zasady:
1. Predykcje zapisuj z identyfikatorami przykładów, prawdziwą klasą i przewidywaną klasą.
2. Metryki zapisuj w plikach JSON albo CSV.
3. Katalog `final_test/` może pojawić się dopiero w commicie późniejszym niż tag `final-freeze` i zawiera wynik tylko jednego systemu.
4. Nie zapisuj tu dużych wag modeli ani pełnych danych.
