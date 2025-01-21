1. Jako użytkownik, chcę płacić za bilet kartą, gotówką lub telefonem, aby mieć
większą elastyczność w wyborze metody płatności.
2. Jako użytkownik, chcę otrzymać wyraźne instrukcje na ekranie, aby wiedzieć,
jak dokonać zakupu krok po kroku.
3. Jako użytkownik, chcę widzieć czas pozostały na decyzję (np. wyświetlany
licznik czasu), aby móc szybko podjąć działanie.
4. Jako użytkownik, chcę szybko wybrać rodzaj biletu, aby zminimalizować czas 
spędzony przy biletomacie.
5. Jako użytkownik, chcę mieć możliwość wyboru języka, aby móc korzystać z 
biletomatu bez względu na znajomość języka lokalnego.
6. Jako użytkownik, chcę sprawdzić poprawność transakcji przed jej finalizacją, 
aby uniknąć pomyłek.
7. Jako użytkownik, chcę otrzymać potwierdzenie zakupu (np. wydruk biletu lub 
elektroniczny bilet), aby móc korzystać z transportu zgodnie z przepisami.

## Diagramy przypadków użycia
### Płatność za bilet

1. Użytkownik wybiera metodę płatności (karta, gotówka, telefon) (Wybór metody płatności).
2. System weryfikuje dostępność wybranej metody (Weryfikacja metody płatności).
3. Użytkownik dokonuje płatności (np. wprowadza kartę, gotówkę, korzysta z NFC) (Realizacja płatności).
4. System potwierdza zakończenie transakcji (Potwierdzenie transakcji).
5. Użytkownik w dowolnym momencie może anulować proces (Anulowanie transakcji).

#### Wizualizacja

```mermaid
flowchart TD
    A(Wybór metody płatności) -.->|include| B[Weryfikacja metody płatności]
    B --> C[Realizacja płatności]
    C --> D[Potwierdzenie transakcji]
    C --> E{Anulowanie transakcji?}
    E -->|Tak| F[Anulowanie transakcji]
    E -->|Nie| D
```
### Anulowanie transakcji
1. Użytkownik rozpoczyna proces zakupu biletu (Rozpoczęcie interakcji).
2. W dowolnym momencie użytkownik wybiera opcję "Anuluj" (Wybranie opcji 
anulowania).
3. System wyświetla komunikat potwierdzający anulowanie transakcji (Komunikat 
o anulowaniu).
4. System resetuje interfejs do ekranu głównego (Reset interfejsu).
Relacje:

• Include: Wyświetlenie komunikatu potwierdzającego anulowanie (Komunikat o 
anulowaniu).

• Extend: Zapisanie powodu anulowania (opcjonalnie) (Zapis powodu 
anulowania).

#### Wizualizacja
```mermaid
flowchart TD
    Aktor((Aktor użytkownik)) -->A
    A(Rozpoczęcie interakcji) --> B(Wybranie opcji anulowania)
    B -.->|include| C(Komunikat o anulowaniu)
    C --> C1(Reset interfejsu)
    D[Zapis powodu anulowania] -.->|extend| B
```

## Wspólny diagram przypadków użycia

```mermaid
flowchart TD
    Aktor --> A
    Aktor((Aktor użytkownik)) -->A0
    A0(Rozpoczęcie interakcji) --> B0(Wybranie opcji anulowania)
    B0 -.->|include| C0(Komunikat o anulowaniu)
    C0 --> C01(Reset interfejsu)
    D0[Zapis powodu anulowania] -.->|extend| B0
    A(Wybór metody płatności) -.->|include| B[Weryfikacja metody płatności]
    B --> C[Realizacja płatności]
    C --> D[Potwierdzenie transakcji]
    C --> E{Anulowanie transakcji?}
    E -->|Tak| F[Anulowanie transakcji]
    E -->|Nie| D 
```

## Diagram sekwencji dla przypadku użycia "Anulowanie transakcji"

```mermaid
sequenceDiagram
    actor Użytkownik
    participant Interfejs płatności
    participant Serwer aplikacji
    participant Baza danych

    Użytkownik->>Interfejs płatności: Rozpoczęcie interakcji
    activate Interfejs płatności
    Użytkownik->>Interfejs płatności: Wysłanie id transakcji do anulowania
    Interfejs płatności->>Serwer aplikacji: Przekazanie danych
    activate Serwer aplikacji
    Serwer aplikacji->>Baza danych: Wysłanie zapytania
    activate Baza danych
    alt Zapytanie powiodło się
        Baza danych-->>Serwer aplikacji: informacja o sukcesie
        deactivate Baza danych
        Serwer aplikacji-->>Interfejs płatności: informacja o sukcesie
        deactivate Serwer aplikacji
        Interfejs płatności->>Użytkownik: Pokazanie informacji o poprawnym anulowniu transakcji
        activate Użytkownik
        deactivate Użytkownik
        activate Baza danych
        activate Serwer aplikacji        
    else Zapytanie nie powiodło się
        Baza danych-->>Serwer aplikacji: informacja o błędzie
        deactivate Baza danych
        Serwer aplikacji-->>Interfejs płatności: informacja o błędzie
        deactivate Serwer aplikacji
        Interfejs płatności->>Użytkownik: Pokazanie informacji o błędzie podczas próby anulowania transakcji
        activate Użytkownik
    end
    Użytkownik-->>Interfejs płatności: Potwierdzenie przeczytania komunikatu
    deactivate Użytkownik
    Interfejs płatności-->>Użytkownik: Reset interfejsu i powrót do poprzedniego ekranu
    deactivate Interfejs płatności  
```
