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

## DIAGRAMY SEKWENCJI
### Wyświetlenie dostępnych biletów
AKTOR: UŻYTKOWNIK
OBIEKTY: BILETOMAT, INTERFEJS, BAZA DANYCH
KOLEJNOSC KOMUNIKATÓW: 
1. BILETOMAT WYŚWIETLA EKRAN POWITALNY NA INTERFEJSIE
2. BILETOMAT WYSYŁA ZAPYTANIE DO BAZY DANYCH O LISTĘ DOSTĘPNYCH BILETÓW
3. BAZA DANYCH ODPOWIADA PRZESYŁAJĄC LISTĘ DOSTĘPNYCH BILETÓW
4. BILETOMAT PREKAZUJE DANE DO INTERFEJSU ABY WYŚWIETLIŁ KATEGORIĘ BILETÓW I ICH SZCZEGÓŁY
5. BILETOMAT CZEKA NA WYBÓR UŻYTKOWNIKA

SCENARIUSZ ALTERNATYWNY 1 ( BRAK DANYCH ):
1. BILETOMAT URUCHAMIA INTERFEJS EKRANU POWITALNEGO
2. BILETOMAT WYSYŁA ZAPYTANIE DO BAZY DANYCH O LISTĘ DOSTĘPNYCH BILETÓW
3. BAZA DANYCH ODPOWIADA ŻE JEST BRAK DANYCH
4. BILETOMAT WYŚWIETLA NA INTERFEJSIE KOMUNIKAT O BRAKU DANYCH
5. BILETOMAT CZEKA NA WYBÓR UŻYTKOWNIKA

### WIZUALIZACJA DIAGRAMU SEKWENCJI
```mermaid
sequenceDiagram
    PARTICIPANT USER AS UŻYTKOWNIK
    PARTICIPANT BT AS BILETOMAT
    PARTICIPANT UI AS INTERFEJS
    PARTICIPANT DB AS BAZA DANYCH

    BT-)UI: Wyświetlenie ekranu powitalnego
    BT->>DB: Pobierz listę dostępnych biletów
    ALT DANE ISTNIEJĄ
        DB-->>BT: Przekazanie listy dostępnych biletów
        BT-)UI: Wyświetl liste dostępnych biletów
    ELSE BRAK DANYCH
        DB-->>BT: Przekazanie informacji o braku danych
        BT-)UI: Wyświetl komunikat o braku danych
    END
        BT->>USER: Oczekiwanie na wybór użytkownika
```

## Diagram sekwencji "Dostarczenie listy biletów do biletomatu"

```mermaid
sequenceDiagram
    participant BT as Biletomat
    participant SW as Serwer
    participant DB as Baza Danych

    activate BT
        BT ->> SW: Wysłanie żądania o dostarczenie listy biletów
        activate SW
    alt Serwer dostępny
        SW ->> DB: Wysłanie żądania o dostarczenie aktualnych taryf i typów biletów
        activate DB
        DB -->> SW: aktualne taryfy i typy biletów
        deactivate DB
        SW ->> SW: Stworzenie listy biletów
        SW -->> BT: lista dostępnych biletów
    else Serwer niedostępny
        deactivate SW
        deactivate BT    
        activate BT
        activate SW
        SW -->> BT: informacja o błędzie
        deactivate SW
    end
    deactivate BT
```

## Diagramy klas

### "Wyświetlenie dostępnych biletów"

```mermaid
classDiagram

class TicketMachineInterface
TicketMachineInterface: +displayWelcomeScreen()
TicketMachineInterface: +showError(String)

class TicketMachineService
TicketMachineService: +getAvailableTicketsList() TicketsList

class TicketsList
TicketsList: isEmpty() bool

class Ticket
Ticket: +price int
Ticket: +name String

class Database
Database: +fetchTickets() TicketsList

TicketMachineService --> TicketMachineInterface
TicketMachineService ..> TicketsList
TicketMachineService --> Database
TicketsList *-- Ticket : 0..*
```

### "Dostarczenie listy biletów do biletomatu"
#### OPIS KLAS
#### KLASY
##### Biletomat
 - METODY: `VOID SERWER_NOT_AVAILABLE()`
##### Serwer
 - ATRYBUTY: `LIST<BILET> BILETY`
 - METODY: `LIST<BILET> GET_TICKETS()`, `LIST<BILET> CREATE_TICKET_LIST(BILET[] BILETY)`
##### BazaDanych
 - ATRYBUTY: `BILET[] BILETY`
 - METODY: `BILETY[] GET_TICKETS()`
##### Bilet
 - ATRYBUTY: `STRING TYPE`, `STRING TARYFA`
#### WIZUALIZACJA
``` mermaid
classDiagram
    class Biletomat{
        +VOID SERWER_NOT_AVAILABLE()
    }
    class Serwer{
        +LIST<BILET> BILETY
        +LIST<BILET> GET_TICKETS()
        +LIST<BILET> CREATE_TICKET_LIST(BILET[] BILETY)VOID SEND_ID(INT ID)
        +VOID RESET()
    }
    class BazaDanych{
        +BILET[] BILETY
        +BILETY[] GET_TICKETS()
    }
    class Bilet{
        +STRING TYPE
        +STRING TARYFA
    }

    Biletomat --> Serwer : pyta
    Serwer --> BazaDanych : pyta
    BazaDanych --> Bilet : przechowuje
```
