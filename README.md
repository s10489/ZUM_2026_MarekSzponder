# ZUM 2026/2027: Automatyczna kategoryzacja produktów modowych na podstawie zdjęcia

## Informacje ogólne

* Tytuł: Automatyczna kategoryzacja produktów modowych w katalogu e-commerce na podstawie zdjęcia produktu
* Autor: Marek Szponder
* Modalność: obraz (zdjęcia produktów z katalogu sklepu internetowego)
* Problem klasyfikacji: klasą jest typ produktu (`articleType`, np. Tshirts, Heels, Watches); po filtrowaniu 64 klasy
* Kurs: ZUM 2026/2027, PJATK
* Wersja Pythona: 3.13.18 (Google Colab)
* Notebook Zadania 1: `01_audyt_i_podzialy_v4.ipynb`

## Problem i pytanie eksperymentalne (Zadanie 1)

**Problem.** Dla zdjęcia produktu z katalogu sklepu internetowego przewiduję jego typ (`articleType`, np. Tshirts, Jeans, Casual Shoes, Watches). Każdy produkt ma dokładnie jedną etykietę, więc jest to nadzorowana klasyfikacja jednolabelowa.

**Scenariusz użycia.** Sprzedawca dodaje nowy produkt do katalogu, a system podpowiada jego kategorię na podstawie zdjęcia.

**Pytanie eksperymentalne.** Czy reprezentacja z zamrożonego modelu pretrained (CLIP) rozpoznaje kategorię produktu lepiej niż prosta reprezentacja pikselowa, gdy oceniam produkty z rodzin produktowych niewidzianych w treningu? Które pary kategorii są mylone (np. Tshirts vs. Tops, Casual Shoes vs. Sports Shoes) i czy wynika to z ograniczeń modelu, czy z niespójności taksonomii katalogu?

**Jednostka generalizacji: rodzina produktu**, czyli ten sam model w różnych wariantach kolorystycznych. Model ma działać na nowych modelach produktów, a nie tylko na nowych kolorach modeli już widzianych. Losowy podział po ID umieściłby niemal identyczne zdjęcia wariantów kolorystycznych po obu stronach podziału i zawyżył wynik.

### Pojęcia używane w projekcie

1. **Produkt (przykład):** jeden rekord katalogu z własnym ID, jednym zdjęciem i jedną etykietą. Na produktach model jest trenowany i oceniany.
2. **Klasa:** typ produktu z pola `articleType` (np. Tshirts, Heels). Model przewiduje dokładnie jedną klasę dla każdego zdjęcia.
3. **Rodzina produktu:** grupa produktów, które uznaję za ten sam model w różnych wariantach kolorystycznych. Rodzina jest **jednostką generalizacji**: produkty z jednej rodziny nie mogą trafić do różnych zbiorów. W plikach podziału identyfikator rodziny to kolumna `group_id` (w scikit-learn takie jednostki nazywa się „grupami”, stąd nazwa kolumny).
4. **Grupowanie:** procedura, która przypisuje produkty do rodzin (opisana w sekcji „Protokół”).
5. **Zbiory train, validation, final test:** train służy do trenowania modeli, validation do porównywania modeli i wyboru najlepszego, a final test do jednorazowej oceny wybranego systemu na końcu projektu.
6. **Wyciek danych:** sytuacja, w której informacja z validation lub final testu przedostaje się do treningu, np. gdy ten sam produkt w innym kolorze jest i w train, i w teście. Model „rozpoznaje” wtedy znane zdjęcie zamiast uogólniać, a wynik jest zawyżony.
7. **MD5:** skrót (odcisk) pliku. Dwa pliki o tym samym MD5 są identyczne bajt po bajcie, więc służy do wykrywania dokładnych duplikatów.
8. **pHash (hasz percepcyjny):** 64-bitowy skrót opisujący ogólny wygląd zdjęcia. Podobne zdjęcia mają podobne hasze.
9. **Odległość pHash:** liczba bitów, którymi różnią się dwa hasze (odległość Hamminga, od 0 do 64). 0 oznacza praktycznie identyczny obraz, a im mniejsza liczba, tym bardziej podobne zdjęcia.
10. **Para:** dwa zdjęcia z różnych rodzin o małej odległości pHash, sprawdzane ręcznie, żeby wykryć warianty, które grupowanie przegapiło.
11. **Stratyfikacja:** podział, w którym proporcje klas w train, validation i final teście są możliwie podobne.
12. **Macro-F1:** średnia z wyników F1 policzonych osobno dla każdej klasy, w której każda klasa waży tyle samo, niezależnie od liczby produktów. F1 dla klasy łączy precyzję (jaka część produktów przypisanych do klasy naprawdę do niej należy) i czułość (jaką część produktów klasy model znalazł).
13. **Accuracy:** odsetek poprawnie sklasyfikowanych produktów. **Balanced accuracy:** średnia czułość po klasach.

## Dane i licencja (Zadanie 1)

**Źródło.** Fashion Product Images (Small), P. Aggarwal, Kaggle: https://www.kaggle.com/datasets/paramaggarwal/fashion-product-images-small. Zdjęcia i metadane pochodzą z katalogu sklepu internetowego Myntra (Indie).

**Sposób pobrania.** Notebook pobiera dane bezpośrednio z Kaggle biblioteką `kagglehub` (`kagglehub.dataset_download("paramaggarwal/fashion-product-images-small")`), bez logowania. Danych nie ma w repozytorium.

**Liczba przykładów.** 44 446 produktów w metadanych (jedno zdjęcie na produkt). Po usunięciu produktów bez zdjęcia, duplikatów i rzadkich klas zostaje 41 636 produktów w 64 klasach (szczegóły w audycie i w sekcji „Wybór klas”).

**Etykiety.** Klasy to wartości pola `articleType` z metadanych katalogu. Etykiety nie powstały w procesie anotacji badawczej, tylko przy katalogowaniu produktów przez sklep, więc mogą zawierać szum i niespójności taksonomii (np. granica między Tshirts a Tops albo Flats a Heels).

**Licencja danych.** Na stronie zbioru w serwisie Kaggle jako licencję podano MIT. Treść licencji wymienia jednak jako właścicieli praw osoby niezwiązane ze zbiorem (Mark Otto, Andrew Fong) i dotyczy „oprogramowania”, co sugeruje użycie gotowego szablonu. Zdjęcia pochodzą z katalogu sklepu Myntra i na podstawie dostępnych informacji nie da się ustalić, czy autor zbioru miał prawo do ich redystrybucji.

Treść licencji w brzmieniu ze strony zbioru (https://www.kaggle.com/datasets/paramaggarwal/fashion-product-images-small, stan z 8 listopada 2026):

<details>
<summary>Pełny tekst licencji podanej na Kaggle</summary>

```text
Released under MIT License

Copyright (c) 2013 Mark Otto.

Copyright (c) 2017 Andrew Fong.

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
```

</details>

**Decyzja dotycząca zdjęć.** Dane wykorzystuję wyłącznie do celów edukacyjnych w ramach kursu. W repozytorium nie publikuję żadnych zdjęć: dane są pobierane bezpośrednio w środowisku Colab, notebook nie wyświetla obrazów, a przykłady są pokazane jako metadane. Zdjęcia do ręcznej weryfikacji grupowania generuję wyłącznie w Colabie i nie trafiają one do repozytorium. Folder `splits/` zawiera wyłącznie numeryczne ID produktów i rodzin, które nie identyfikują osób. Część zdjęć przedstawia modelki i modeli z widoczną twarzą, co jest dodatkowym powodem, żeby ich nie publikować.

**Znane ograniczenia danych.**
1. Niska rozdzielczość zdjęć (60×80 px) ogranicza rozróżnianie kategorii po detalach (materiał, szwy).
2. Asortyment pochodzi z jednego sklepu z rynku indyjskiego, więc rozkład stylów i kategorii (np. Kurtas, Sarees) jest regionalny.
3. Etykiety katalogowe bywają niespójne. Przykład: klasa `Free Gifts` nie opisuje typu produktu, tylko sposób sprzedaży (gratis), i zawiera m.in. torebki i zegarki.

### Audyt danych

| Element audytu | Wynik |
|---|---|
| Wiersze w metadanych | 44 446 |
| Wiersze naprawione (przecinek w nazwie produktu) | 22 |
| Produkty bez pliku zdjęcia / nieczytelne | 5 / 0 |
| Braki w polach metadanych | `season`: 21, `year`: 1, `usage`: 1, pozostałe pola: 0 |
| Rozmiar zdjęć | 60×80 px w 44 418 z 44 441 przypadków, pozostałe 23 nieznacznie mniejsze (np. 53×80, 60×60) |
| Tryb koloru | RGB (kolorowe, 3 kanały): 44 010, skala szarości (1 kanał): 431 |
| Liczba klas `articleType` przed filtrowaniem | 142, w tym 35 z mniej niż 10 produktami |
| Dokładne duplikaty zdjęć (MD5) | 1431 zdjęć w 652 grupach, w tym 7 grup z różną etykietą |
| Pary zdjęć z odległością pHash ≤ 4 | 658 069 |

Najliczniejsze klasy przed filtrowaniem: Tshirts (7069), Shirts (3215), Casual Shoes (2846), Watches (2542), Sports Shoes (2036). Pełny rozkład: `results/task1/class_counts_all.csv`.

**Decyzje.**
1. Produkty bez zdjęcia usuwam (5), bo nie mogą być wejściem modelu. Produktów bez etykiety nie ma.
2. Z każdej grupy identycznych plików zostawiam jeden (najniższe ID), żeby ten sam obraz nie liczył się kilka razy. Usunąłem 779 kopii. Duplikaty z różną etykietą (7 grup) traktuję jako przykład szumu etykiet.
3. Braki w `season`, `year` i `usage` nie mają znaczenia, bo tych pól nie używam.
4. 431 zdjęć zapisano w skali szarości (jeden kanał zamiast trzech). Nie usuwam ich, bo nadal pokazują kształt produktu, ale przy wczytywaniu będę konwertować je do RGB, żeby wszystkie wejścia miały ten sam format. Histogram kolorów w baseline nie niesie dla nich informacji o kolorze. W Zadaniu 2 sprawdzę, czy takie zdjęcia nie są skupione w kilku klasach, bo wtedy brak koloru mógłby stać się sztuczną wskazówką dla modelu.
5. Pozostałe 23 zdjęcia o rozmiarze innym niż 60×80 px będą przeskalowane do wspólnego rozmiaru w preprocessingu.

## Protokół (Zadanie 1)

* Jednostka generalizacji: rodzina produktu, czyli ten sam model w różnych wariantach kolorystycznych (kolumna `group_id` w `splits/`).
* Podział: grupowy po rodzinie i stratyfikowany po klasie (`StratifiedGroupKFold`), ok. 70/15/15. Grupowy, bo warianty jednego modelu mają niemal identyczne zdjęcia i rozdzielenie ich byłoby wyciekiem; stratyfikowany, bo klasy są niezbalansowane.
* Metryka główna: macro-F1, bo klasy są niezbalansowane, a każda kategoria katalogu jest równie ważna. Pomocnicze: accuracy, balanced accuracy, F1 per klasa, macierz pomyłek.
* Seed: 42
* Final test: zamrożony w commicie z tagiem `task-1`, identyfikatory w `splits/`.

| Zbiór | Liczba przykładów | Liczba niezależnych jednostek |
|---|---|---|
| train | 29 367 | 12 464 |
| validation | 6 098 | 2 504 |
| final test | 6 171 | 2 489 |

Szczegółowe uzasadnienie każdej decyzji jest w podsekcjach poniżej.

### Grupowanie w rodziny produktów

Dwa produkty trafiają do jednej rodziny, jeśli:
1. po normalizacji mają tę samą nazwę (małe litery, usunięcie wszystkich słów kolorów występujących w zbiorze, usunięcie znaków specjalnych), albo
2. mają identyczny plik zdjęcia (MD5) i tę samą markę oraz kategorię.

Połączenia są przechodnie. Świadomie wybieram grupowanie raczej za szerokie niż za wąskie: nadmiarowe łączenie zmniejsza liczbę niezależnych jednostek, ale nie powoduje wycieku, a za wąskie grupowanie powoduje wyciek.

**Dlaczego bez łączenia po pHash.** Planowałem łączyć też produkty o bardzo podobnych zdjęciach (pHash), ale na zdjęciach 60×80 px na białym tle pHash słabo rozróżnia produkty. Par z odległością ≤ 4 jest 658 tys., a łączenie przechodnie tworzy łańcuchy przez cały katalog. Testowałem kolejne wersje:

1. pHash ≤ 4 bez warunków: jedna rodzina objęła 32 227 produktów (72% zbioru).
2. pHash ≤ 4 tylko w obrębie tej samej marki i kategorii: największa rodzina miała 707 produktów (buty jednej marki), a klasa Flats miała więcej produktów w final teście niż w train.
3. Reguła końcowa: wybieram najwyższy próg pHash, który nie powiększa największej rodziny ponad to, co daje samo łączenie po nazwie i MD5.

| Próg pHash | Rodziny | Największa rodzina | Rodziny > 100 |
|---|---|---|---|
| bez pHash | 18 145 | 210 | 11 |
| 0 | 17 480 | 309 | 16 |
| 1 | 16 842 | 544 | 20 |
| 2 | 15 678 | 630 | 25 |
| 4 | 12 943 | 707 | 49 |

Już próg 0 (identyczny hasz) powiększa największą rodzinę z 210 do 309, więc reguła wybrała grupowanie bez pHash. Ryzyko przegapionych wariantów, czyli możliwego wycieku, sprawdziłem ręcznie (niżej).

**Wynik grupowania:** 44 441 produktów w 18 145 rodzinach, w tym 12 473 rodziny jednoelementowe. Największa rodzina ma 210 produktów (Catwalk Women Heels, ogólna nazwa modelu). 217 rodzin zawiera więcej niż jedną klasę, np. ta sama nazwa linii u produktów z klas Flats i Heels.

### Ręczna weryfikacja grupowania

1. **50 losowych rodzin (co najmniej 2 produkty):** 34 poprawne, 14 błędnie połączonych, 2 niepewne. Wszystkie błędy to nadmiarowe łączenie produktów jednej marki o ogólnej nazwie, która po usunięciu koloru jest wspólna dla kilku modeli (np. „Lino Perros Women Sling Bag”, „Numero Uno Men Flip Flops”, „Highlander Men Checked Slim Fit Shirt” z długim i krótkim rękawem). Taki błąd zmniejsza liczbę niezależnych jednostek, ale nie powoduje wycieku. W dwóch przypadkach niska rozdzielczość nie pozwoliła rozstrzygnąć.
2. **30 losowych par bardzo podobnych zdjęć (pHash ≤ 4), które trafiły do różnych rodzin:** 29 to różne produkty, 1 to potencjalny wyciek: dwie szminki jednej marki w identycznym opakowaniu (Colorbar Soft Touch i Colorbar Velvet Matte). Formalnie to różne linie produktu, ale zdjęcia są niemal identyczne. Większość par to buty, a większość ma odległość 3–4. Obie obserwacje są spodziewane: buty fotografuje się zawsze w tym samym ujęciu (z boku, na białym tle), więc różne modele mają podobne hasze, a par z większą odległością jest po prostu więcej (z odległością 4 jest ich 363 tys., z odległością 0 tylko 8 tys.), więc losowa próbka składa się głównie z nich. Jednocześnie potwierdza to, że łączenie po samym podobieństwie zdjęć skleiłoby różne modele butów.
3. **60 par zdjęć dość podobnych (pHash 5–10) z różnych rodzin**, sprawdzonych na wcześniejszych wersjach grupowania: wszystkie były różnymi produktami. Pary w tym przedziale nigdy nie są łączone, więc tego przeglądu nie powtarzałem.

**Wniosek.** Grupowanie jest zgodne z założeniem „raczej za szeroko niż za wąsko”. Nadmiarowe łączenie występuje często, a przegapione warianty rzadko (1 na 30 par bardzo podobnych zdjęć). Wynik dla kosmetyków o identycznych opakowaniach może być przez to lekko zawyżony, co traktuję jako ograniczenie.

### Źródła wycieku danych i zapobieganie

| Źródło wycieku | Zapobieganie |
|---|---|
| Ten sam model w różnych kolorach (osobne ID, niemal identyczne zdjęcie) | podział grupowy po rodzinie produktu |
| Dokładne duplikaty zdjęć | wykrycie MD5, łączenie w rodziny, usunięcie kopii |
| Bardzo podobne zdjęcia różnych produktów (np. kosmetyki w tym samym opakowaniu) | ręczny przegląd próbki; pozostałe ryzyko opisane jako ograniczenie |
| Nazwa produktu zawiera klasę (np. „Nike Men Blue Tshirts”) | model dostaje wyłącznie zdjęcie; nazwy używam tylko do zbudowania podziału (patrz niżej) |
| Preprocessing uczący się z danych (np. średnia i odchylenie do skalowania cech) | parametry liczone wyłącznie na train, w `Pipeline` scikit-learn (od Zadania 2) |

**Dlaczego nazwa produktu nie jest wejściem modelu.** Nazwa często wprost zawiera klasę („Tshirts”, „Heels”). Gdyby model ją dostawał, odczytałby odpowiedź z tekstu, a wynik nic by nie mówił o rozpoznawaniu produktu na zdjęciu, o które pyta mój projekt. Użycie nazwy do grupowania nie jest wyciekiem, bo grupowanie decyduje tylko o tym, do którego zbioru trafia produkt, a nie o tym, co model widzi. Etykieta biorąca udział w podziale (stratyfikacja, warunek tej samej kategorii przy MD5) też jest dopuszczalna z tego samego powodu.

**Dlaczego preprocessing dopasowuję tylko na train.** Niektóre kroki preprocessingu uczą się z danych, np. standaryzacja liczy średnią i odchylenie każdej cechy. Gdybym policzył je na wszystkich danych, statystyki validation i final testu wpłynęłyby na trening, co jest ukrytym wyciekiem. Dlatego od Zadania 2 takie kroki umieszczam w `Pipeline` ze scikit-learn: przy `fit` parametry są liczone tylko na train, a validation i final test są jedynie przekształcane tymi samymi parametrami. W Zadaniu 1 nic nie jest uczone z danych (grupowanie i podział nie są modelem).

### Wybór klas

Klasę włączam do projektu, jeśli ma co najmniej **30 niezależnych rodzin** (a nie zdjęć). Klasa z wieloma zdjęciami, ale z małą liczbą rodzin, ma w praktyce mało niezależnych przykładów. Przy podziale ok. 70/15/15 klasa z 30 rodzinami ma w final teście średnio 4–5 rodzin, co uznaję za minimum.

| Minimalna liczba rodzin | 10 | 20 | 30 | 50 | 100 |
|---|---|---|---|---|---|
| Klasy | 93 | 72 | 64 | 47 | 36 |
| Produkty | 43 204 | 42 395 | 41 636 | 39 694 | 38 007 |

Próg 30 zachowuje 64 klasy i 95% produktów po deduplikacji. Wyższy próg odrzucałby kolejne kategorie katalogu, a niższy dodawałby klasy z pojedynczymi rodzinami w validation i final teście.

Wybrane klasy (64): Backpacks, Belts, Bra, Briefs, Capris, Caps, Casual Shoes, Clutches, Compact, Deodorant, Dresses, Duffel Bag, Earrings, Eyeshadow, Face Moisturisers, Flats, Flip Flops, Formal Shoes, Foundation and Primer, Fragrance Gift Set, Free Gifts, Handbags, Heels, Highlighter and Blush, Innerwear Vests, Jackets, Jeans, Kajal and Eyeliner, Kurta Sets, Kurtas, Kurtis, Laptop Bag, Leggings, Lip Gloss, Lip Liner, Lipstick, Lounge Pants, Messenger Bag, Mobile Pouch, Nail Polish, Necklace and Chains, Night suits, Perfume and Body Mist, Sandals, Sarees, Scarves, Shirts, Shorts, Skirts, Socks, Sports Sandals, Sports Shoes, Sunglasses, Sweaters, Sweatshirts, Ties, Tops, Track Pants, Trousers, Trunk, Tshirts, Tunics, Wallets, Watches.

**Klasa `Free Gifts`.** Spełnia próg liczebności, ale nie opisuje typu produktu, więc jest sprzeczna z definicją problemu. Zostawiam ją w plikach podziału, a decyzję o jej wykluczeniu podejmę i uzasadnię w Zadaniu 2, przed trenowaniem. Wykluczenie klasy oznacza usunięcie jej przykładów ze wszystkich trzech zbiorów jednocześnie i nie zmienia przypisania pozostałych ID, więc nie wymaga nowego podziału.

### Podział

Podział grupowy (cała rodzina w jednym zbiorze), stratyfikowany po klasie, za pomocą `StratifiedGroupKFold` ze scikit-learn: najpierw 1/7 danych na final test, potem z reszty 1/6 na validation. Oficjalnego podziału zbioru nie ma, więc final test wydzielam samodzielnie. Notebook sprawdza, że żadna rodzina nie występuje w dwóch zbiorach i że każda klasa jest obecna w każdym zbiorze.

| Zbiór | Produkty | Rodziny (niezależne jednostki) | % produktów |
|---|---|---|---|
| train | 29 367 | 12 464 | 70,5 |
| validation | 6 098 | 2 504 | 14,6 |
| final test | 6 171 | 2 489 | 14,8 |
| razem | 41 636 | 17 457 | 100 |

Liczby produktów i rodzin per klasa i zbiór: `results/task1/split_per_class.csv`.

**Stabilność estymaty.** W skali całego zbioru validation i final test mają po ok. 2,5 tys. niezależnych rodzin, więc macro-F1 powinno być stabilne. Najmniej liczne klasy mają jednak w validation tylko 2 rodziny, a w final teście nawet 1. Dla takiej klasy jedna pomyłka zmienia F1 o kilkadziesiąt punktów procentowych, a ponieważ macro-F1 waży wszystkie klasy jednakowo, rzadkie klasy mają nieproporcjonalny wpływ na wynik. Wyniki per klasa dla najmniejszych klas traktuję jako orientacyjne, a przy porównaniu modeli raportuję też accuracy.

### Metryki

1. **Główna: macro-F1.** Klasy są niezbalansowane (od kilkudziesięciu do kilku tysięcy produktów), a w katalogu rzadka kategoria jest tak samo ważna jak częsta. Accuracy byłaby zdominowana przez kilka dużych klas.
2. **Pomocnicze:** accuracy (łatwa do interpretacji i mniej wrażliwa na małe klasy), balanced accuracy, F1 per klasa i macierz pomyłek (do analizy mylonych par kategorii, co jest częścią pytania eksperymentalnego).

### Seed i losowość

Jeden globalny seed `SEED = 42`, używany w podziale i w losowaniu przykładów do przeglądu. Ponowne uruchomienie notebooka daje identyczny podział co do ID. Wersje bibliotek: `requirements.txt`.

### Zamrożenie final test

Identyfikatory zbiorów są w `splits/` (`train_ids.csv`, `validation_ids.csv`, `test_ids.csv`, kolumny `id` i `group_id`). Podział zamroziłem tagiem `task-1`. Do końca Zadania 4 nie uruchamiam na final test żadnego modelu.

## Wyniki

Uzupełniane od Zadania 2.

## Finalny system

Uzupełniane w Zadaniu 5.

## Reprodukcja

1. Otworzyć `01_audyt_i_podzialy_v4.ipynb` w Google Colab.
2. Uruchomić wszystkie komórki. Dane pobierają się automatycznie z Kaggle (`kagglehub`), logowanie nie jest wymagane. Czas wykonania: ok. 10–15 minut na CPU.
3. Notebook odtwarza pliki w `splits/` i `results/task1/`.

## Ograniczenia

1. Grupowanie w rodziny jest heurystyczne (nazwa produktu i duplikaty plików). Przegląd ręczny pokazał częste nadmiarowe łączenie i rzadkie przegapione warianty, m.in. kosmetyki w identycznym opakowaniu.
2. Najmniejsze klasy mają w validation i final teście pojedyncze rodziny, więc ich F1 jest niestabilne.
3. Prawa do zdjęć nie są jednoznacznie ustalone (patrz „Dane i licencja”).
4. Dane pochodzą z jednego sklepu i jednego rynku, a etykiety z katalogu sklepu.

## Dziennik zmian

* 2026-11-08: Zadanie 1. Audyt danych, grupowanie w rodziny produktów, wybór klas, podział train/validation/final test, tag `task-1`.
    * Grupowanie zmieniałem trzykrotnie przed zamrożeniem podziału: łączenie po pHash bez warunków skleiło 72% zbioru w jedną rodzinę, łączenie po pHash w obrębie marki i kategorii dało rodzinę z 707 produktami, więc ostatecznie grupuję po nazwie i MD5, bez pHash (szczegóły w sekcji „Protokół”).
    * Liczba klas wynosi 64 zamiast szacowanych w zgłoszeniu 20–30, bo próg liczę na liczbie rodzin, a nie zdjęć, i przy progu 30 rodzin zostaje więcej kategorii, niż zakładałem.

## Licencja projektu (kod)

Kod projektu (notebooki i skrypty w tym repozytorium): MIT. Licencja dotyczy wyłącznie mojego kodu, a nie danych, których zasady opisuje sekcja „Dane i licencja”.


---


# Projekt końcowy ZUM 2026/2027

To repozytorium rozwijasz przez cały semestr. Górna część tego pliku mówi, jak pracować. Dolna część to dokumentacja Twojego projektu, którą uzupełniasz w kolejnych zadaniach.

## Jak pracować z tym repozytorium

### Co robisz

Przeprowadzasz jeden wiarygodny eksperyment nadzorowanej klasyfikacji jednolabelowej. Każdy przykład ma dokładnie jedną klasę docelową. Pracujesz na tekście, obrazie, audio albo danych multimodalnych. Projekt jest indywidualny.

Projekt ma trzy poziomy:
1. **Poziom A. Punkt odniesienia.** Prosty, sensowny baseline.
2. **Poziom B. Podejście pretrained.** Co najmniej jedno podejście pretrained. Dobrym pierwszym krokiem jest zamrożony encoder i prosty klasyfikator. Fine tuning nie jest obowiązkowy.
3. **Poziom C. Jedno uzasadnione rozszerzenie.** Odpowiada na to samo pytanie projektu i nie zmienia zmiennej docelowej ani problemu klasyfikacyjnego.

Pełne zasady są w pliku [PROJECT_RULES.md](PROJECT_RULES.md). Przeczytaj je przed zgłoszeniem tematu.

### Od czego zacząć

1. Utwórz własne repozytorium z tego szablonu: wybierz „Use this template”, a następnie „Create a new repository”.
2. Nazwij repozytorium `ZUM_2026_ImięNazwisko`.
3. Repozytorium może być publiczne albo prywatne. Jeśli jest prywatne, dodaj konto prowadzącej wskazane na platformie kursu. Jeśli Twoje dane zawierają informacje osobowe albo wrażliwe, przeczytaj najpierw zasadę o prywatności w [zasadach projektu](PROJECT_RULES.md).
4. Przeczytaj [instrukcję zgłoszenia tematu](assignments/00_topic_submission.md) i zgłoś temat.

### Kolejność i terminy

Wszystkie terminy przypadają w niedzielę o godzinie 23:59. Wszystko oddajesz na platformie kursu.

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

Projekt jest wart 100 punktów. Artykuł jest osobnym zadaniem za 10 punktów.

### Final test w skrócie

1. Final test wydzielasz w Zadaniu 1 i od razu go zamrażasz.
2. Do końca Zadania 4 nie uruchamiasz na nim żadnego modelu. Wszystkie porównania i wybór systemu robisz na validation.
3. W Zadaniu 5 wybierasz dokładnie jeden finalny system, zapisujesz go w `final_system.md` i tworzysz tag `final-freeze`.
4. Nie trenujesz systemu ponownie na `train + validation` ani na żadnym większym zbiorze.
5. Dopiero potem uruchamiasz final test, jeden raz, tylko dla tego systemu. Wynik testu nie może zmienić systemu.

Szczegóły są w [zasadach projektu](PROJECT_RULES.md), w części „Zasady final test”.

### Tagi Git

Każde oddanie wskazuje commit oznaczony tagiem. Oceniamy stan repozytorium z tego commita.

```bash
git add .
git commit -m "Zadanie 1: protokół i podział danych"
git tag -a task-1 -m "Zadanie 1"
git push origin main
git push origin task-1
```

Polecenie `git ls-remote --tags origin` pokazuje, czy tag jest już na GitHub. Tag możesz też utworzyć na stronie GitHub w zakładce Releases. Nie przesuwaj i nie usuwaj tagów po oddaniu.

### Gdzie co zapisujesz

| Miejsce | Co tam trafia |
|---|---|
| `README.md`, część dolna | dokumentacja projektu |
| `notebooks/` | notebooki kolejnych etapów |
| `src/` | kod wspólny dla notebooków (opcjonalnie) |
| `splits/` | identyfikatory przykładów train, validation i final test ([opis formatu](splits/README.md)) |
| `results/` | metryki i predykcje na validation, dziennik eksperymentów, wynik final test (dopiero po `final-freeze`); [opis układu](results/README.md) |
| `final_system.md` | zapis finalnego systemu (Zadanie 5); [szablon](templates/final_system_template.md) |
| `data/sample/` | mała próbka danych, jeśli licencja i zasady prywatności na to pozwalają |
| `templates/` | szablony do skopiowania |

### Czego nie wrzucasz

* pełnych zbiorów danych;
* dużych wag modeli i checkpointów (przechowujesz je poza repozytorium i opisujesz, gdzie są);
* danych, próbek ani identyfikatorów umożliwiających identyfikację osób;
* haseł, tokenów i kluczy API;
* plików tymczasowych środowiska.

Plik `.gitignore` blokuje typowe przypadki, ale to Ty odpowiadasz za zawartość repozytorium.

### Jak oceniamy

Liczy się poprawność metodologiczna, uzasadnienie decyzji i jakość analizy. Wysokość wyniku nie jest sama w sobie kryterium. Projekt z niższym F1, wykonany poprawnie i dobrze przeanalizowany, może dostać więcej punktów niż projekt z bardzo wysokim wynikiem uzyskanym przy wycieku danych. Kryteria każdego zadania są w jego instrukcji, a rubryka ogólna w [zasadach projektu](PROJECT_RULES.md).

---

# Dokumentacja projektu

Uzupełniaj sekcje w zadaniach podanych w nawiasach. Zastępuj teksty w nawiasach kwadratowych.

## Informacje ogólne

* Tytuł: [tytuł projektu]
* Autor: [imię i nazwisko]
* Modalność: [tekst, obraz, audio albo dane multimodalne]
* Problem klasyfikacji: [co jest klasą, ile jest klas]

## Problem i pytanie eksperymentalne (Zadanie 1)

[Jaki problem rozwiązujesz i co dokładnie chcesz sprawdzić.]

## Dane i licencja (Zadanie 1)

[Źródło i link, licencja, sposób pobrania, liczba przykładów, lista klas, sposób oznaczenia etykiet, znane ograniczenia.]

## Protokół (Zadanie 1)

* Jednostka generalizacji: [na przykład recenzja, pacjent, mówca, nagranie źródłowe]
* Podział: [metoda i uzasadnienie]
* Metryka główna i pomocnicze: [z uzasadnieniem]
* Seed: [wartość]
* Final test: zamrożony w commicie z tagiem `task-1`, identyfikatory w `splits/`. Jeśli podział był później poprawiany, podaj commit poprawionej wersji i opisz zmianę w dzienniku zmian.

| Zbiór | Liczba przykładów | Liczba niezależnych jednostek |
|---|---|---|
| train | | |
| validation | | |
| final test | | |

## Wyniki (Zadania 2 do 5)

Wyniki na validation:

| Podejście | Zadanie | Metryka główna | Metryki pomocnicze | Czas i sprzęt | Uwagi |
|---|---|---|---|---|---|
| Trivial baseline | 2 | | | | |
| Baseline | 2 | | | | |
| Podejście pretrained | 3 | | | | |
| Rozszerzenie | 4 | | | | |

Wynik na final test, wyłącznie dla finalnego systemu i dopiero po utworzeniu tagu `final-freeze`:

| System | Metryka główna | Metryki pomocnicze |
|---|---|---|
| [nazwa z `final_system.md`] | | |

## Finalny system (Zadanie 5)

Pełny zapis jest w `final_system.md`. [Krótkie uzasadnienie wyboru na podstawie validation.]

## Reprodukcja

[Wersja Pythona, instalacja środowiska, pobranie danych, kolejność uruchamiania notebooków albo skryptów, sprzęt, przybliżony czas.]

## Ograniczenia

[Ograniczenia danych, protokołu i wyników. Czego wynik nie dowodzi.]

## Dziennik zmian

| Data | Zadanie | Co zmieniono i dlaczego |
|---|---|---|
| | | |

## Licencja projektu

[Na przykład MIT. Dane używane zgodnie z licencją opisaną w sekcji „Dane i licencja”.]
