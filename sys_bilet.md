1. Jako system biletowy, chcę rejestrować każde sprzedane bilety, aby śledzić
ruch i sprzedaż w systemie.
2. Jako system biletowy, chcę współpracować z aplikacjami mobilnymi, aby
użytkownik mógł uzyskać elektroniczny bilet w przypadku takiego wyboru.
3. Jako system biletowy, chcę dostarczać aktualne dane o taryfach i typach 
biletów do biletomatu, aby użytkownik miał zawsze poprawne informacje.
4. Jako system biletowy, chcę umożliwiać sprawdzenie ważności biletu w czasie 
rzeczywistym, aby zapobiegać oszustwom.

## Diagramy przypadków użycia
### Wyświetlenie dostępnych biletów

1. Użytkownik wybiera metodę płatności (karta, gotówka, telefon) (Wybór metody płatności).
2. System weryfikuje dostępność wybranej metody (Weryfikacja metody płatności).
3. Użytkownik dokonuje płatności (np. wprowadza kartę, gotówkę, korzysta z NFC) (Realizacja płatności).
4. System potwierdza zakończenie transakcji (Potwierdzenie transakcji).
5. Użytkownik w dowolnym momencie może anulować proces (Anulowanie transakcji).

#### Wizualizacja

```mermaid
flowchart TD
    A[Uruchomienie ekranu powitalnego] --> B[Pobranie listy biletów]
    B --> C[Wyświetlenie biletów]
    C --> D[Oczekiwanie na wybór użytkownika]
```
### Dostarczanie listy biletów do biletomatu
1. System biletowy odbiera żądanie od biletomatu dotyczące listy biletów 
(Odebranie żądania listy biletów).
2. System biletowy sprawdza aktualne taryfy i typy biletów w bazie danych 
(Sprawdzenie bazy taryf).
3. System biletowy przesyła listę dostępnych biletów do biletomatu (Przesłanie 
listy biletów).
Relacje:

• Include: Sprawdzenie bazy taryf (Weryfikacja taryf).

• Extend: Obsługa błędów komunikacji (np. brak połączenia z biletomatem) (Błąd 
komunikacji).

#### Wizualizacja
```mermaid
flowchart TD
    Aktor((Aktor system biletowy)) -->A
    A(Odebranie żądania listy biletów) -.->|include| B(Sprawdzenie bazy taryf)
    B --> C(Przesłanie listy biletów)
    B -.->|extend| D[Błąd komunikacji]
```

## Wspólny diagram przypadków użycia
```mermaid
flowchart TD
    A0[Uruchomienie ekranu powitalnego] --> B0[Pobranie listy biletów]
    B0 --> C0[Wyświetlenie biletów]
    C0 --> D0[Oczekiwanie na wybór użytkownika]
    Aktor --> A0
    Aktor((Aktor system biletowy)) -->A
    A(Odebranie żądania listy biletów) -.->|include| B(Sprawdzenie bazy taryf)
    B --> C(Przesłanie listy biletów)
    B -.->|extend| D[Błąd komunikacji]
```
