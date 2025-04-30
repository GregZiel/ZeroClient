### Główny problem
Użytkownicy nowotworzonego systemu SCADA muszą mieć możliwość przeglądania danych z poszczególnych obiektów. Danych bieżących i historycznych w różnych ujęciach, a także ostrzeżeń i alarmów. 
Użytkownicy pełniący role wdrożeniowców lub utrzymania ruchu muszą mieć możliwość łatwego dostępu do danych bieżących i głębszych informacji o działaniu systemu również w warunkach problematycznego dostępu do sieci.
Aplikacje klienckie wcześniej używanych systemów - INFINIT8 i ThingsBoard - miały wady:
- aplikacja kliencka TB jest zbyt "zrośnięta" z backendem systemu i wymaga pełnej konfiguracji a priori
- autogenerowany widok mobilny aplikacji klienckiej INFINIT8 był wygodny, ale brakowało wersji prawdziwie mobilnej w formie aplikacji natywnej
- aplikacja kliencka TB nie limituje komunikacji
- konfiguracja aplikacji klienckiej INFINIT8 była skomplikowana

### Najmniejszy zestaw funkcjonalności
- Wersja web dla urządzeń mobilnych
- Prosty system kont użytkowników
- Subskrypcja obiektów (instalacji) z użyciem URL API oraz klucza API
- Persistence dla subskrypcji
- Proste zarządzanie zasubskrybowanymi obiektami (edycja przyjaznej nazwy, usuwanie)
- Autogenerowane widoki tabelaryczno/tekstowe vide INFINIT8
- Cacheowanie obiektów a vista i minimalizacja transferu
- Prosty deployment (Docker) na minimalnej infrastrukturze (Mikrus 1.0 Pro)

### Co NIE wchodzi w zakres MVP
- Raporty
- Alarmy
- Potwierdzanie alarmów
- Ustawianie poziomów ostrzegawczych i alarmowych
- Notatki użytkownika
- W pełni natywna aplikacja mobilna

### Kryteria sukcesu
- Aplikacja pozwala zasubskrybować dwa obiekty: IBI Lab i URHydro
- Aplikacja pozwala efektywnie wdrażać stacje URHydro (lokalizacje o potwierdzonym słabym dostępie do internetu)
