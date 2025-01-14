1. Jako system biletowy, chcę rejestrować każde sprzedane bilety, aby śledzić
ruch i sprzedaż w systemie.
2. Jako system biletowy, chcę współpracować z aplikacjami mobilnymi, aby
użytkownik mógł uzyskać elektroniczny bilet w przypadku takiego wyboru.
3. Jako system biletowy, chcę dostarczać aktualne dane o taryfach i typach 
biletów do biletomatu, aby użytkownik miał zawsze poprawne informacje.
4. Jako system biletowy, chcę umożliwiać sprawdzenie ważności biletu w czasie 
rzeczywistym, aby zapobiegać oszustwom.

## DIAGRAMY PRZYPADKÓW UŻYCIA
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
