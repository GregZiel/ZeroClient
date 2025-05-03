# ZeroClient MVP - 2025-05-01

## 1. Identyfikacja kluczowego problemu
Efektywne (1) monitorowanie wielu (2) heterogenicznych (3) instalacji przemysłowych (4) w warunkach mobilnego (5), ograniczonego pod względem przepustowości (6), stabilności (7) i sensu ekonomicznego (8) dostępu do Internetu, przy minimalizacji zależności od stanu wdrożenia (9) warstwy sprzętowo-programowej na poszczególnych instalacjach.

(1) Efektywne - skupiamy się na sprawności obsługi, w tym na urządzeniach mobilnych

(2) Wielu - pracujemy w modelu N do M - wielu użytkowników o różnie skonfigurowanych aplikacjach monitoruje wiele instalacji

(3) Heterogenicznych - podstawą są instalacje monitorowane via ZeroSCADA, ale od razu otwieramy się też na instancje ThingsBoard, a docelowo potencjalnie na inne źródła via generyczne / customowe microfrontendy + stosowne moduły w backendzie ZeroClient

(4) Przemysłowych - więc nawet w mikroskali priorytetem jest niezawodność i wiarygodność

(5) Mobilnie - więc aplikacja webowa musi być wygodnie używana na telefonie, a docelowo chcemy aplikację natywną

(6) Niska przepustowość łącz - przynajmniej czasami, a więc wydajny i wygodnie kontrolowany cache elementów, najlepiej z wizualizacją co mamy w cache, ręczną kontrolą cache'owania, usuwania / odświeżania cache, kontrolą "czego brakuje"

(7) Problematyczna stabilność łącz - a więc wszystkie komponenty muszą być gotowe na takie warunki, plus dobrze umieścić gdzieś wskaźnik stanu komunikacji z serwerem (per instalacja) oraz czas ostatniego kontaktu

(8) Ograniczenia sensu ekonomicznego transmisji - to przede wszystkim roaming, łącza płatne, pakiety, jednak ten aspekt w dużym stopniu obsługują same urządzenia mobilne, więc jest to głównie uwrażliwienie aplikacji na informacje płynące z systemu, rozwiązanie podobne jak w Spotify

(9) Minimalizacja zależności od stanu wdrożenia - punkt o największym wpływie na działanie aplikacji, ponieważ obejmuje:

- niezależność od gotowości projektowanych / konfigurowanych ręcznie ekranów synoptycznych przez wprowadzenie autogenerowanych ekranów tekstowo-tabelarycznych, z możliwością przełączania się na graficzne w miarę postępów wdrożenia

- ułatwienia typu "autodiscovery" redukujące czas tworzenia konfiguracji po stronie ZeroClient na podstawie subskrybowanej instalacji (ZeroSCADA / TB jako źródło)

- łatwe wymuszanie sterowań dla wybranych zmiennych (np. łatwa konfiguracja dla danej zmiennej ekranu sterującego z kilkoma przyciskami wymuszającymi wartości, np. "0", "1", "100" itp.)

Ta lista będzie rozbudowywana.

Ponadto należy podkreślić:

- Aplikacje klienckie mają być w pełni odsprzęgnięte od backendu ZeroSCADA

- Jedna aplikacja kliencka powinna móc subskrybować dane z wielu backendów

- Każda instancja backendu powinna obsługiwać wiele instancji frontendu

### Dodatkowe wymogi spoza podstawowego kontekstu biznesowego

Produkty rzadko powstają w próźni "pełnego greenfieldu". Stąd istnienie dodatkowych wymogów / ograniczeń, nierzadko "na pograniczu" - wymóg techniczny narzucany przez biznes (vide: musimy korzystać z chmury Azure ze względu na umowy o współpracy z Microsoft; musimy korzystać z modeli Gemini bo szykujemy zgłoszenie do konkursu Google).

- Ze względu na planowaną współpracę z wybranymi partnerami - kierunek (analityka) i ich preferowany stack (Python) - ROZWAŻAMY użycie Pythona i frameworka Dash, jednak nie jest to konieczne.

- Ze względu na założenie dużej reużywalności kodu - minimalizujemy koszt utrzymania - należy rozważyć orchestrację z użyciem architektury mikrofrontendów i technologii React Native.

- Ze względu na konieczność dostarczania wybranych wizualizacji partnerom publicznym, takim jak gminy czy uczelnie, mikrofrontendy powinny mieć autogenerowaną lekką wersję do umieszczania jako widget na stronie www.

- Ze względu na efektywność ekonomiczną i aspekt edukacyjny podstawę deploymentu jest Mikrus 1.0 Pro.

- Ze względu na konieczność adresowania również potrzeb klientów indywidualnych, np. w projektach publicznych czy naukowo-badawczych, prezentacja danych musi być bardzo atrakcyjna wizualnie.

### Wartość biznesowa

- Umożliwienie efektywnej wizualizacji gromadzonych danych

- Ułatwianie wdrażania backendu ZeroSCADA na obiektach

- Baza do współpracy z zespołami IT w zaprzyjaźnionych firmach

## 2. Najmniejszy zestaw funkcjonalności

- Rejestracja + logowanie użytkownika

- Poziomy dostępu

    - **Informacyjny:** dostęp do gromadzonych danych i ich wizualizacji

    - **Monitorujący:** dodatkowo dostęp do szczegółowych logów i informacji o problemach

    - **Serwisowy:** dodatkowo możliwość dodawania urządzeń i zmiany kluczy dostępowych

    - **Administracyjny:** pełna kontrola, włącznie z zarządzaniem użytkownikami

- Subskrypcja min. 2 backendów opartych o ZeroSCADA, z użyciem URL API i klucza API:

    - dummy (symulowany)

    - URHydro (faktyczny projekt rozwijany równolegle, mamy pewien wpływ na jego kształt, np. na kształt API)

- Persistence dla subskrypcji obiektów

- Zarządzanie zasubskrybowanymi obiektami:

    - Edycja przyjaznej nazwy

    - Usuwanie subskrypcji

- Autogenerowane widoki tabelaryczno/tekstowe

- Podstawowe 2 widoki:

    - stan bieżący, z ostatnim pomiarem i czasem ostatniego kontaktu z backendem

    - prosty wykres historii (ostatnie 12h, uśrednienie)

- Deployment web (Docker @ Mikrus 1.0 Pro)

- Osadzenie widgetu w stronie www

- Obsługa jedną ręką w przeglądarce na smartfonie

- Podstawowa wersja cache ("cache all")

- Zaawansowane cache'owanie:

    - Pobieranie wszystkich niezbędnych elementów na żądanie
  
    - Widgety, strony, definicje, dane

    - Monitoring przy minimalnym transferze

    - Optymalizacja dla słabego zasięgu GSM/braku WiFi


## 3. Co NIE wchodzi w skład MVP

- Raporty

- Alarmy

- Aplikacja React Native na telefonie

- Obsługa instancji TB / generycznych / innych

- Obsługa informacji o łączu na urządzeniach (WiFi / GSM)

- Szczegółowe zarządzanie cache

- Pełna obsługa użytkowników (samodzielny reset hasła itp.)

- Tickety do supportu wewnątrz aplikacji

- Pełne API autodiscovery (dostarczamy schemat)

- Moduły przetwarzające w backendzie ZeroClient

- MCP i analizy wyników z użyciem LLM

- Obsługa wielu języków - na początek całość w języku angielskim

## 4. Kryteria sukcesu

- Aplikacja pozwala zasubskrybować dwa obiekty: IBI Lab i URHydro

- Możliwość bieżącego podglądu stanu stacji przy instalacji / serwisie stacji URHydro (kiepskie warunki GSM, konieczność pracy z smartfona, nie z laptopa)

- Widget z pomiarami URHydro na stronie www mojego bloga (Wordpress) / fallback do dedykowanej landing page

