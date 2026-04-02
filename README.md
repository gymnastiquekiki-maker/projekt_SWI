# projekt-SWI

Repozitář pro projekt z předmětu Softwarové inženýrství. Projekt řeší návrh systému **Village Marketplace**, tedy webové aplikace pro lokální komunitní tržiště zaměřené na nabídku a poptávku zboží a služeb.

## Co repozitář obsahuje

- textovou specifikaci projektu v souboru `village-marketplace.md`,
- konceptuální, logický a databázový model ve formátu Draw.io,
- základní projektovou dokumentaci ve složce `docs/`.

## Struktura repozitáře

```text
.
|-- README.md
|-- village-marketplace.md
|-- class logicky.drawio
|-- class-diagram-konceptualni.drawio
|-- ERD.drawio
`-- docs/
    |-- index.md
    |-- requirements.md
    `-- architecture.md
```

## Přehled projektu

Village Marketplace propojuje uživatele v menší komunitě, například v obci nebo malém městě. Uživatelé mohou:

- vytvářet nabídky a poptávky,
- reagovat na existující inzeráty,
- uzavírat objednávky,
- komunikovat nad konkrétní nabídkou,
- dostávat notifikace a související dokumenty, například faktury.

Doménový návrh počítá s webovým přístupem z počítače i mobilních zařízení a s nasazením v prostředí s omezeným IT zázemím.

## Dokumentace

Základní rozcestník dokumentace je v souboru [`docs/index.md`](./docs/index.md).

- [`docs/requirements.md`](./docs/requirements.md) shrnuje funkční a nefunkční požadavky.
- [`docs/architecture.md`](./docs/architecture.md) popisuje navržené části systému a práci s diagramy.

## Práce s diagramy

Diagramy jsou uložené ve formátu Draw.io:

- `class-diagram-konceptualni.drawio` - konceptuální pohled na hlavní entity a vztahy,
- `class logicky.drawio` - logický model dat a vazeb,
- `ERD.drawio` - entitně relační diagram databáze.

Pro úpravy lze soubory otevřít v [draw.io / diagrams.net](https://app.diagrams.net/).

## Další doporučené kroky

1. doplnit finální technologický stack a implementační rozhodnutí,
2. sjednotit názvy diagramů a případně přidat exporty do PNG nebo PDF,
3. navázat tuto dokumentaci na backlog, use cases a implementaci.
