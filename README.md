# ZeroClient

Zbiór aplikacji zapewniających dostęp do danych gromadzonych w różnych instancjach systemu ZeroSCADA, w tym instancji URHydro.

## Założenia początkowe

Potrzebujemy kilku rodzajów dostępu:
- **Informacyjny** - dostęp do gromadzonych danych i ich wizualizacji
- **Monitorujący** - to co w **Informacyjny** plus dostęp do szczegółowych logów, informacji o problemach itp.
- **Serwisowy** - to co w **Monitorujący** plus możliwość dodawania urządzeń, zmiany kluczy dostępowych itp.
- **Administracyjny** - to co w **Serwisowy** plus możliwość zarządzania użytkownikami

Dostęp ma być na kilka sposóbów:
- **Widget** - element do osadzenia na dowolnej stronie www, najprostsze informacje (vide strona gminy z danymi o poziomie wody w ciekach)
- **Aplikacja Web**
- **Aplikacja Android/iOS**
- **API**

Aplikacje klienckie mają być w pełni odsprzęgnięte od backendu ZeroSCADA - jedna aplikacja kliencka (Web / Android / iOS) powinna móc subskrybować dane z wielu backendów, jednocześnie każda instancja backendu powinna móc w pełni niezależnie obsługiwać wiele instancji frontendu (w tym wiele instancji ZeroClient Web / Android / iOS).

Aplikacje powinny być bardzo efektywne kosztowo pod względem wymogów sprzętowych.

Stack technologiczny nie jest zupełnie swobodny. Projekt ZeroClient, oprócz tworzenia określonej wartości biznesowej poprzez:
- umożliwianie wizualizacji gromadzonych danych 
- ułatwianie wdrażania backendu ZeroSCADA na obiektach
ma również być bazą do współpracy z zespołami IT w zaprzyjaźnionych firmach.
Stąd następujące wymogi:
- przetwarzanie i wizualizacja danych - Python, Dash
- architektura mikrofrontendów
- orchestracja - TypeScript, React
- React Native dla uruchamiania na urządzeniach mobilnych
- zaawansowane cache'owanie po stronie klienta.

Ten ostatni wymóg - zaawansowane cache'owanie po stronie klienta - wymaga rozwinięcia. Działanie ma być następujące: po zainstalowaniu aplikacji na urządzeniu mobilnym użytkownik musi być w stanie na żądanie pobrać do cache urządzenia wszystkie niezbędne elementy - widgety, strony, definicje, niezbędne dane itp. - tak, aby późniejszy monitoring wybranych zmiennych odbywał się przy skrajnie małym transferze danych. Jest to podyktowane tym, że wdrożenia systemu nierzadko odbywają się w terenie, przy bardzo słabym zasięgu sieci GSM i przy braku WiFi. Takie podejście było wykorzystane w INFINIT8 i znakomicie się sprawdzało.

