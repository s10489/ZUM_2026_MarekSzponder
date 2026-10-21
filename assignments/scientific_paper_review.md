# Krytyczne czytanie artykułu naukowego

**Termin zgłoszenia artykułu spoza listy:** niedziela 22 listopada 2026, 23:59
**Termin oddania:** niedziela 6 grudnia 2026, 23:59
**Punkty:** 10, poza punktacją projektu

**Jeżeli miejsce publikacji nie znajduje się na liście automatycznie akceptowanych źródeł, musisz uzyskać akceptację artykułu najpóźniej do 22 listopada 2026. Bez wcześniejszej akceptacji praca na podstawie takiego źródła nie będzie oceniana.**

## Cel

Nie piszesz streszczenia. Uczysz się czytać pracę naukową krytycznie: jak zbudowano eksperyment, co naprawdę porównano i czego wynik nie dowodzi.

## Wybór artykułu

1. Wybierasz jeden artykuł naukowy z eksperymentem z uczenia maszynowego, najlepiej związany z Twoim problemem, modalnością albo metodą. Nie wybieraj przeglądu literatury ani pracy czysto teoretycznej.
2. Artykuł musi pochodzić z renomowanego czasopisma albo renomowanej konferencji z recenzją.
3. Nie akceptujemy publikacji z czasopism i konferencji drapieżnych, stron internetowych, blogów, materiałów marketingowych ani tekstów bez recenzji.

## Akceptacja źródła

1. Prace z głównych konferencji i czasopism z poniższej listy są akceptowane automatycznie.
2. Inne źródła wymagają akceptacji prowadzącej przed rozpoczęciem zadania. Dotyczy to innych czasopism i konferencji, prac z warsztatów (workshops) i preprintów, na przykład z arXiv.
3. Zgłaszając źródło do akceptacji, podajesz pełny opis bibliograficzny oraz DOI albo oficjalny link.
4. Jeśli prowadząca ma wątpliwości co do źródła, może poprosić o wybór innej pracy.
5. Jeśli artykuł z arXiv ukazał się w źródle z listy, podajesz wersję opublikowaną. Wtedy akceptacja jest automatyczna.

### Lista źródeł akceptowanych automatycznie

* Ogólne ML i AI: NeurIPS, ICML, ICLR, AAAI, IJCAI, JMLR, Machine Learning (Springer).
* NLP: ACL, EMNLP, NAACL, TACL, Computational Linguistics.
* Computer Vision: CVPR, ICCV, ECCV, TPAMI, IJCV.
* Mowa i audio: Interspeech, ICASSP, IEEE/ACM Transactions on Audio, Speech, and Language Processing.
* Multimodalność: ACM Multimedia oraz ACL, EMNLP, NeurIPS albo CVPR, zależnie od problemu.
* Zastosowania medyczne: Nature Machine Intelligence, npj Digital Medicine, IEEE Journal of Biomedical and Health Informatics, Artificial Intelligence in Medicine.

### Pomoc przy odnalezieniu i sprawdzeniu pracy

DBLP, ACL Anthology, PMLR, CVF Open Access, IEEE Xplore, ACM Digital Library i ISCA Archive pomagają sprawdzić, gdzie i kiedy praca została opublikowana. Wpis w takim serwisie nie jest jednak automatycznym dowodem jakości źródła. O akceptacji źródła spoza listy decyduje prowadząca.

## Co podajesz

1. pełną referencję;
2. DOI albo oficjalny link;
3. miejsce publikacji (czasopismo lub konferencja) i rok;
4. dla źródła spoza listy: informację o akceptacji prowadzącej.

## Pytania

Odpowiadasz na 10 pytań w [szablonie analizy artykułu](../templates/paper_review_template.md). Każde twierdzenie o pracy wskazuje miejsce w artykule (sekcja, tabela albo rysunek).

Jeśli autorzy czegoś nie podają, napisz to wprost, na przykład: „Autorzy nie podają tej informacji.”, „Nie da się ustalić, czy…”, „Na podstawie artykułu można wnioskować…, ale nie można tego potwierdzić.” Taka odpowiedź może otrzymać pełną punktację, jeśli wyjaśnisz, co udało Ci się sprawdzić. Nie zgadujesz brakujących informacji.

1. Jakie jest główne pytanie badawcze albo główna teza pracy?
2. Jakie dane wykorzystano (źródło, wielkość, etykiety)?
3. Jaka jest jednostka generalizacji i jak zbudowano podział train, validation i test? Czy test jest niezależny od wyborów modelu? Jeśli autorzy nie definiują jednostki generalizacji wprost, nie wymyślaj jej. Napisz na przykład: „Autorzy nie definiują jednostki generalizacji wprost. Na podstawie sposobu podziału danych można wnioskować X, ale nie da się tego potwierdzić.”
4. Jaki jest baseline i co dokładnie porównują autorzy? Czy porównanie odbywa się na tych samych danych i w tym samym protokole?
5. Jakie są główne metryki? Czy pasują do problemu? Czy autorzy raportują zmienność wyników (na przykład kilka seedów)?
6. Wskaż jedno konkretne ryzyko metodologiczne (na przykład wyciek danych, strojenie na teście, nierówny budżet strojenia porównywanych metod). Jeśli nie znajdujesz takiego ryzyka, opisz, jakie informacje sprawdziłeś oraz czego na podstawie artykułu nie da się zweryfikować.
7. Jaka jest jedna mocna strona metodologii?
8. Jakie jest jedno ograniczenie (podane przez autorów albo zauważone przez Ciebie)?
9. Czego wynik nie dowodzi?
10. Jaką jedną rzecz wykorzystasz we własnym projekcie albo jakiej unikniesz, i dlaczego?

## Format i długość

* Odpowiedź ma strukturę według pytań, po polsku.
* Przy każdym pytaniu piszesz od dwóch do pięciu zdań.
* Całość ma od 600 do 900 słów, bez danych bibliograficznych.
* Ogólny opis pracy mieści się w najwyżej trzech zdaniach.
* Krótkie cytaty są dozwolone, z podaniem miejsca w artykule.
* Oddajesz plik PDF albo Markdown na platformie kursu.

## Punktacja (10 punktów)

* źródło i opis bibliograficzny (zgodność z zasadami wyboru, kompletne dane): 1 punkt;
* zrozumienie pracy i eksperymentu (pytania 1 do 5: trafność, konkretność, odwołania do sekcji, jawne opisanie braków informacji): 4 punkty;
* krytyczna ocena (pytania 6 do 9): 4 punkty;
* wniosek dla projektu (pytanie 10): 1 punkt.

Praca oparta na źródle spoza listy, które nie zostało zaakceptowane do 22 listopada 2026, nie jest oceniana.
