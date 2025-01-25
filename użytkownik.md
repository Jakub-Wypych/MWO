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

## DIAGRAMY SEKWENCJI
### Płatność za bilet
AKTOR: UŻYTKOWNIK
OBIEKTY: INTERFEJS, SERWER
KOLEJNOSC KOMUNIKATÓW: 
1. UŻYTKOWNIK WYBIERA METODA PŁATNOŚCI NA INTERFEJSIE
2. INTERFEJS PRZEKAZUJE DANE DO SERWERA DLA WERYFIKACJI
3. SERWER WERYFIKUJE DANE I ZWRACA WYNIK DO INTERFEJSU
4. INTERFEJS PROSI UŻYTKOWNIKA O DOKONANIA PŁATNOŚCI
5. UŻYTKOWNIK DOKONUJE PŁATNOŚCI
6. INTERFEJS PRZEKAZUJE DANE DO SERWERA DLA WERYFIKACJI
7. SERWER WERYFIKUJE DANE
8. SERWER REALIZUJE PŁATNOŚĆ I ZWRACA WYNIK DO INTERFEJSU
9. INTEREFEJS WYŚWIETLA KOMUNIKAT O REALIZACJI PŁATNOŚCI

SCENARIUSZ ALTERNATYWNY 1 ( BŁAD WYBORU PŁATNOŚCI ):
1. UŻYTKOWNIK WYBIERA METODA PŁATNOŚCI NA INTERFEJSIE
2. INTERFEJS PRZEKAZUJE DANE DO SERWERA DLA WERYFIKACJI
3. SERWER WERYFIKUJE DANE I ZWRACA BŁEDNY WYNIK DO INTERFEJSU
4. INTERFEJS WYŚWIETLA KOMUNIKAT BŁĄDU WYBORU PŁATNOŚCI

SCENARIUSZ ALTERNATYWNY 2 ( BŁAD DOKONANIA PŁATNOŚCI ):
1. UŻYTKOWNIK WYBIERA METODA PŁATNOŚCI NA INTERFEJSIE
2. INTERFEJS PRZEKAZUJE DANE DO SERWERA DLA WERYFIKACJI
3. SERWER WERYFIKUJE DANE I ZWRACA WYNIK DO INTERFEJSU
4. INTERFEJS PROSI O DOKONANIA PŁATNOŚCI
5. UŻYTKOWNIK DOKONUJE PŁATNOŚCI
6. INTERFEJS PRZEKAZUJE DANE DO SERWERA DLA WERYFIKACJI
7. SERWER WERYFIKUJE DANE I ZWRACA BŁEDNY WYNIK DO INTERFEJSU
8. INTERFEJS WYŚWIETLA KOMUNIKAT BŁĄDU DOKONANIA PŁATNOŚCI

SCENARIUSZ ALTERNATYWNY 3 ( ANULOWANIE TRANSAKCJI ):
1. UZYTKOWNIKA WYBIERA OPCJE ANULOWANIA TRANSAKCJI
2. INTERFEJS PRZEKAZUJE DANE DO SERWERA
3. SERWER ANULUJE TRANSAKCJE
4. INTERFEJS POKAZUJE INFORMACJE O ANULOWANIU TRANSAKCJI

#### WIZUALIZACJA DIAGRAMU SEKWENCJI
```mermaid
sequenceDiagram
    PARTICIPANT USER AS UŻYTKOWNIK
    PARTICIPANT UI AS INTERFEJS
    PARTICIPANT SR AS SERWER

    USER->>UI: Wybór płatności
    UI->>SR: Weryfikacja danych
    ALT WYBÓR PŁATNOŚCI JEST POZYTYWNE
        SR-->>UI: Pozytywny wynik weryfikacji
        UI-->>USER: Wyświetlenie prośby o dokonania płatności
        USER->>UI: Dokonanie płatności
        UI->>SR: Weryfikacja płatności
        ALT DOKONANIE PŁATNOŚCI JEST POZYTYWNE
            SR->>SR: Dokonanie płatności
            SR-->>UI: Pozytywny wynik weryfikacji
            UI-->>USER: Wyświetlenie komunikatu o realizacji płatności
        ELSE
            SR-->>UI: Błędny wynik weryfikacji
            UI-->>USER: Wyświetlenie komunikatu o błędzie dokonania płatności
        END
    ELSE WYBÓR PŁATNOŚCI JEST NEGATYWNE
        SR-->>UI: Błędny wynik weryfikacji
        UI-->>USER: Wyświetlenie komunikatu o błędzie wyboru płatności
    END
    OPT ANULOWANIE PŁATNOŚCI
        USER-)UI: Wybór anulowania transakcji
        UI->>SR: Anuluj transakcje
        SR-->>UI: Anulowanie transakcji
        UI-->>USER: Wyświetlenie komunikatu o anulowaniu transakcji
    END
```
### Anulowanie transakcji
**AKTOR**: Użytkownik
**OBIEKTY**: Interfejs płatności, serwer aplikacji, baza danych
**KOLEJNOSC KOMUNIKATÓW (scenariusz główny)**: 
1. Użytkownik rozpoczyna interakcje (płatność)
2. Użytkownik wysyła id transakcji do anulowania, do interfejsu.
3. Interfejs przekazuje dane do serwera.
4. Serwer wysyła zapytanie do bazy danych.
5. Baza zwraca informacje o sukcesie
6. Interfejs informuje użytkownika o poprawnym anulowaniu.
7. Użytkownik potwierdza przeczytanie komunikatu
8. Interfejs zostaje zresetowany

**SCENARIUSZ ALTERNATYWNY 1 (błędne id transakcji)**:
4. -||-
5a. Baza zwraca informacje o błędzie. 
6a. Interfejs informuje użytkownika o błędzie
JOIN -> 7

#### Wizualizacja diagramu sekwencji
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
