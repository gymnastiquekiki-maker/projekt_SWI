# Architektura a návrh

Tento dokument shrnuje návrhový pohled na systém **Village Marketplace** a vazbu na existující diagramy v repozitáři.

## Navržený charakter systému

Projekt je koncipován jako webová aplikace dostupná z desktopu i mobilních zařízení. Zadání zároveň předpokládá menší provozní tým a omezené IT zázemí, proto dává smysl:

- jednoduchá vícevrstvá webová architektura,
- cloudové nasazení,
- relační databáze pro silně provázaná data,
- integrace externích služeb pro autentizaci a notifikace.

## Hlavní moduly systému

### Prezentační vrstva

- uživatelské rozhraní pro správu účtu,
- práce s nabídkami a poptávkami,
- přehled objednávek, komunikace a notifikací,
- administrační rozhraní pro správu a reporty.

### Aplikační vrstva

- autentizace a autorizace,
- správa inzerátů, reakcí a objednávek,
- generování faktur,
- notifikační logika,
- reporting a exporty.

### Datová vrstva

- relační databáze MySQL,
- ukládání uživatelů, inzerátů, objednávek, zpráv, notifikací a faktur,
- vazby podporující audit a historii změn.

## Externí integrace

- **Google OAuth** pro alternativní přihlášení,
- **SMS provider** pro ověření telefonního čísla a případné notifikace,
- **e-mailová služba** pro zasílání důležitých oznámení a faktur.

## Přehled diagramů

### `class-diagram-konceptualni.drawio`

Konceptuální diagram zachycuje doménové entity, jejich základní atributy a vztahy mezi nimi. Hodí se pro vysvětlení business domény a vazeb mezi objekty systému.

### `class logicky.drawio`

Logický model rozvádí datové struktury podrobněji a zpřesňuje vazby, klíče a předpokládanou podobu ukládaných dat.

### `ERD.drawio`

ERD představuje databázový pohled na řešení a slouží jako základ pro návrh relační databáze a případnou implementaci schématu.

## Doporučení pro další rozpracování

1. doplnit deployment diagram nebo komponentový diagram,
2. sjednotit názvosloví mezi textovou specifikací a diagramy,
3. přidat sekci s technologickým stackem a způsobem nasazení,
4. navázat návrh na backlog nebo seznam prioritizovaných use cases.
