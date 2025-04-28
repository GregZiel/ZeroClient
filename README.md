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

