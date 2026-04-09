🧭 ROADMAP RECOMMENDERA (Netflix Project)
🔵 ETAP 0 — ZROZUMIENIE DANYCH (DONE ✅)

Cel:

co oznacza rating
co oznacza timestamp
ilu userów
ilu filmów
sparsity

👉 Ty to zrobiłeś.

🔵 ETAP 1 — REAL WORLD SPLIT (DONE ✅)
Po splicie robimy kontrolę:

-- czy każdy user z test istnieje w train
-- czy każdy item z test istnieje w train

Jeśli nie:
usuwamy takie rekordy z testu !

Bo klasyczny recommender oparty o:
-- similarity
-- matrix factorization
-- embeddingi

potrafi oceniać tylko te obiekty, które zna z treningu.

Nie losowy split.

Tylko:

temporal leave-one-out

Czyli:

ostatnia interakcja usera → TEST
reszta → TRAIN

👉 Ty to zrobiłeś.

To jest bardzo PRO.

🔵 ETAP 2 — USUNIĘCIE COLD ITEMS (DONE ✅)
cold user - uzytkownik nieobecny w train
cold item - item nieobecny w train
sprawdzamy je po userId itemId metodą isin

Dlaczego?

Bo model CF:

nie może rekomendować filmu którego nie zna

👉 filtr:

test movie ∈ train movie

Zrobione.

🔵 ETAP 3 — WALIDACJA USERÓW (DONE ✅)
czy każdy user z test jest w train
ile ich jest
czy split działa

👉 zrobione.

🔵 ETAP 4 — MAPOWANIE ID → INDEKS (TERAZ JESTEŚMY TU ⭐)

MAPPING ZAWSZE PO SPLIT bo
Model:
-- zna wymiar przyszłej macierzy
-- zna liczbę itemów
-- zna liczbę userów
-- zna potencjalne kolumny

❌ zaniżenie sparsity
bo macierz ma:
-- więcej kolumn
-- więcej wierszy

Model może:
-- lepiej regularizować
-- lepiej estymować latent factors
A TEGO NIE CHCEMY !!!

IDEALNA ODPOWIEDZ
Mapping przed splitem powoduje leakage, ponieważ model poznaje strukturę przestrzeni rekomendacji obejmującą przyszłych użytkowników i itemy.
W realnym systemie ta struktura nie jest znana w momencie trenowania.

W recommenderach mapping definiuje przestrzeń modelu.
Jeśli test używa innego mappingu, model operuje na błędnych reprezentacjach latentnych.

To jest:

przejście z DataFrame → struktura modelu

Robimy:

userId → user_idx
movieId → movie_idx

To jest:

embedding index space
🔵 ETAP 5 — BUDOWA MACIERZY INTERAKCJI ⭐
Matrix Factorization działa przy wysokiej sparsity, ponieważ zakłada istnienie niskowymiarowej struktury latentnych preferencji, którą można nauczyć się z obserwowanych ratingów.

Zwiększenie liczby latent factors zwiększa zdolność modelu do uchwycenia złożonych preferencji, ale jednocześnie zwiększa ryzyko overfittingu i czas trenowania. Optymalna liczba faktorów musi być dobrana eksperymentalnie.

Mapowanie robimy ponieważ:

-- userId i movieId są identyfikatorami symbolicznymi
-- mogą być nieciągłe
-- macierz byłaby ogromna i rzadka
-- potrzebujemy zwartej indeksacji
-- ułatwia to budowę macierzy sparse
-- przyspiesza trening modeli collaborative filtering

CSR potrzebuje ciągłych indeksów od 0 do N

Budujemy:

CSR user × item

To jest:

wejście do modeli

Bez tego:

nie ma recommendera
🔵 ETAP 6 — BASELINE MODEL ⭐

Pierwszy model:

popularność filmów

Dlaczego?

punkt odniesienia
sanity check
ewaluacja pipeline
🔵 ETAP 7 — ITEM-BASED CF ⭐
cosine similarity
top-N rekomendacji

Tu zobaczysz:

prawdziwe rekomendacje

🔵 ETAP 8 — USER-BASED CF ⭐
podobieństwo userów
🔵 ETAP 9 — MATRIX FACTORIZATION ⭐
rozłożenie macierzy na ukryte czynniki

SVD / ALS
embeddings
porównuje wektor reprezentujący usera lub item

Tu zaczyna się:

prawdziwy ML

🔵 ETAP 10 — EWALUACJA ⭐
Recall@K
Precision@K
MAP
NDCG
🔵 ETAP 11 — PROJEKT DS ⭐
tuning
porównanie modeli
wnioski
README
portfolio
❤️ Najważniejsze

Ty jesteś:

ETAP 4 / 11

Czyli:

👉 dopiero ~30% drogi

To NORMALNE, że:

jeszcze nie ma modelu
jeszcze nie ma wyników
jest dużo przygotowania

Tak wygląda prawdziwy DS.

Najważniejsze, co masz zapamiętać

Twój schemat powinien być taki:

1. Czyszczę dane
braki, duplikaty, typy, sanity check

2. Rozumiem strukturę danych
liczba interakcji, liczba userów, liczba itemów, sparsity

3. Robię split
w recommenderze najlepiej temporalny, jeśli chcę symulować przyszłość

4. Usuwam problemy z testu
np. cold items

5. Buduję mapper tylko na train
nigdy na całości

6. Tym samym mapperem mapuję test
bez budowania nowego słownika

7. Buduję CSR z train
bo to jest wejście do modelu

8. Dopiero potem model i ewaluacja

To jest Twój kręgosłup projektu.

[ ] Czy split był przed mapowaniem?
[ ] Czy mapper był zbudowany tylko na train?
[ ] Czy test był mapowany tym samym mapperem?
[ ] Czy po mapowaniu testu nie ma NaN?
[ ] Czy CSR powstał tylko z train?
[ ] Czy znam shape macierzy i sparsity?

