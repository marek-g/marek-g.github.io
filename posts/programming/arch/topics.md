---
layout: page
title: Architecture topics
---

# Architecture topics

Source: DNA Training: <https://droganowoczesnegoarchitekta.pl/>

## Agenda

### WPROWADZENIE

#### Tydzień 01 - Wstęp do Drogi Nowoczesnego Architekta

- 01 Wstęp do architektury oprogramowania
- 02 Model C4 - wizualizacja architektury
- 2z Zadanie - C4
- 03 Drivery architektoniczne - czym są i dlaczego trzeba je znać?
- 3z Zadanie - Drivery architektoniczne
- 3z Zadanie - ADR
- 04 Architektura sterowana liczbami
- 4z Zadanie - Metryki architektoniczne
- 05 Zwinna architektura

#### Tydzień 02 - Strategiczne DDD (Domain Driven Design) Problem Space

- 01 Architektura a biznes
- 02 Domena i subdomeny
- 03 Odkrywanie subdomen
- 04 Big Picture Event Storming Event Storming
- 05 Odkrywanie subdomen w praktyce
- 06 Metryki dla biznesu
- 07 Zadanie - Subdomeny
- 08 Zadanie - Big Picture Event Storming
- 09 Zadanie - Metryki dla biznesu

#### Tydzień 03 - Strategiczne DDD (Domain Driven Design) Solution Space

- 01 Bounded Context
- 02 Subdomena a Bounded Context
- 03 Walidowanie granic kontekstów
- 04 Definiowanie Bounded Contextów w praktyce
- 05 Lokalne drivery architektoniczne
- 06 Prawo Conway’a
- 07 Zadanie - Subdomena a Bounded Context
- 08 Zadanie - Bounded Context

#### Tydzień 04 - Style architektury (korporacyjnej i systemowej)

- 01 Monolit
- 02 Systemy rozproszone
- 03 Enterprise Service Bus (ESB)
- 04 Mikroserwisy
- 05 Autonomia
- 06 Wybor architektury systemowej
- 07 Zadanie - przegląd architektury

### ARCHITEKTURA APLIKACYJNA

#### Tydzień 05 - Style architektury aplikacyjnej

- 01 Architektura warstwowa
- 02 Architektura Hexagonalna
- 03 Architektura Pipes and Filters
- 04 Architektura typu mikrojądro
- 05 Dobór architektury do modułu
- 06 Strategia testowania a styl architektoniczny

#### Tydzień 06 - Design level

- 01 Model domenowy
- 02 “Design Level Event Storming”
- 03 BDD strategiczne
- 04 Estymacja
- 05 Implementacje modelu domenowego
- 06 Agregaty
- 07 CQRS (Command Query Responsibility Segregation)
- 08 Długo działające procesy
- 09 Bonus - Design Level i Live coding demo
- 08 Zadanie - Design Level Event Storming

#### Tydzień 07 - DDD "taktyczne"

- 01 Elementy konstrukcyjne
- 02 Implementacja Value Objects
- 03 Implementacja Encji
- 04 Łamanie reguł biznesowych
- 05 Implemetacja zdarzeń
- 06 Implementacja - Serwisy
- 07 Publikacja Zdarzeń
- 08 Transport Zdarzeń
- 09 Dobór wzorca
- 10 Zadanie - Implementacja elementów konstrukcyjnych

#### Tydzień 08 - Modularyzacja

- 01 Wstęp do modularyzacji
- 02 Enkapsulacja
- 03 Coupling
- 04 Narzędzia do mierzenia couplingu
- 05 Kohezja
- 06 Single Responsibility Principle
- 07 Wybór modułów
- 08 GRASP
- 07 Testing Done Right
- 08 Zadanie - Kohezja

#### Tydzień 09 - REST

- 01 Podstawy REST
- 02 Zasoby w REST
- 03 Domena w REST
- 04 Caching
- 05 HATEOAS
- 06 Wersjonowanie REST
- 07 Testowalność REST
- 08 Dokumentacja REST
- 09 CORS
- 10 Zadanie - przegląd API

### PERSYSTENCJA

#### Tydzień 10 - Persystencja

- 01 Transakcje
- 02 Anomalie współbieżnego dostępu
- 03 Reaktywne źródła danych
- 04 ORM
05CAP, BASE i Eventual Consistency
- 06 Rozproszony konsensus
- 07 Wybór bazy danych

#### Tydzień 11 - Event Sourcing

- 01 Jakie problemy rozwiązuje event sourcing
- 02 Zapis i odczyt zdarzeń
- 03 Projekcje i modele odczytu
- 04 Wersjonowanie zdarzeń
- 05 GetEventStore vs Postgres vs MongoDB
- 06 Live coding

### SYSTEMY ROZPROSZONE

#### Tydzień 12 - Systemy rozproszone

- 01 Przyczyny rozproszenia systemu
- 02 Podział prac projektowych
- 03 Błędne założenia przy rozproszeniu systemu

#### Tydzień 13 - Mikroserwisy

- 01 Mikroserwisy a systemy rozproszone
- 02 SLA, SLO, SLI
- 03 Dostępność
- 04 Temporal coupling
- 05 Wdrożenia bez niedostępności
- 06 Zadanie SLA/SLO/SLI
- 07 Zadanie temporal coupling

#### Tydzień 14 - Komunikacja

- 01 “Design for failure”
- 02 Komunikacja synchroniczna a asynchroniczna
- 03 Komunikacja asynchroniczna Fire & Forget
- 04 Rozproszone sagi
- 05 API publiczne i prywatne
- 06 Circuit breaker
- 07 Service discovery
- 08 Client-Side Load Balancing

#### Tydzień 15 - Jakość komunikacji

- 01 Jakość komunikacji
- 02 Testy E2E
- 03 Testy kontraktowe
- 04 Consumer driven contracts
- 05 Kontrakty w lokalnym developmencie
- 06 Distributed tracing
- 07 Live demo

#### Tydzień 16 - Security

- 01 Wprowadzenie do bezpieczeństwa
- 02 Kerberos
- 03 SAML 2.0
- 04 OAuth 2.0 + OpenID Connect
- 05 JWT
- 06 Edge Service
- 07 Zarządzanie dostępem do komponentów systemu
- 08 Secure development policy
- 09 Case study zabezpieczenia aplikacji

### CI/CD

#### Tydzień 17 = Continuous Integration, Deployment & Delivery

- 01 Deployment pipeliney
- 02 Continuous Delivery - ciągłe dostarczanie
- 03 Ciągła integracja i wdrażanie
- 04 Fail-safe vs safe-to-fail
- 05 Post-mortem
- 06 Testowanie wydajności
- 07 Monitoring
- 08 Główne problemy wydajnościowe

### INFRASTRUKTURA

#### Tydzień 18 - Infra I

- 01 Infrastruktura
- 02 Infrastruktura jako kod
- 03 Zarządzanie konfiguracją aplikacji
- 04 Centralne logowanie
- 05 Mechanizmy poprawy wydajności i zabezpieczenia DDoS
- 06 Projektowanie infrastruktury z nastawieniem na HA
- 07 Wdrażanie procesów Disaster Recovery

#### Tydzień 19 - Infra II

- 01 Chmura
- 02 Kontenery
- 03 Kubernetes
- 04 Service Mesh
- 05 Rozliczalność projektów
- 06 Serverless

### REFACTORING

#### Tydzień 20 - Refactoring z “Big Ball Of Mud”

- 01 Wstęp
- 02 Refactoring do modularnego monolitu
- 03 Szczegóły ekstrakcji modułów
- 04 Refactor modularnego monolitu do mikroserwisów
