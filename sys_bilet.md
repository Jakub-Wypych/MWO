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
```mermaid
flowchart TD
    Aktor((Aktor system biletowy)) -->A
    A(Odebranie żądania listy biletów) -.->|include| B(Sprawdzenie bazy taryf)
    B --> C(Przesłanie listy biletów)
    B -.->|extend| D[Błąd komunikacji]
```
