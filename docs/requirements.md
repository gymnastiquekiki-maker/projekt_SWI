# Požadavky systému

Tento dokument shrnuje hlavní požadavky na systém **Village Marketplace** podle stávající projektové specifikace.

## Cíl systému

Cílem je vytvořit webový informační systém pro menší komunitu, který umožní jednoduše propojit uživatele se zájmem o nabídku nebo poptávku zboží a služeb.

## Hlavní aktéři

- **Uživatel** - běžný uživatel systému, který podle situace vystupuje jako odběratel nebo dodavatel,
- **Admin** - správce systému s rozšířenými oprávněními,
- **SMS Provider** - externí služba pro ověřování telefonního čísla,
- **Google OAuth** - externí služba pro přihlášení.

## Funkční oblasti

### 1. Účty a přístup

- registrace uživatele pomocí e-mailu, hesla a telefonu,
- přihlášení přes e-mail/heslo nebo Google OAuth,
- ověření telefonu pomocí SMS,
- změna hesla a základní správa přístupu,
- administrátorská správa uživatelů.

### 2. Nabídky a poptávky

- vytvoření, úprava a smazání poptávky,
- vytvoření, úprava a smazání nabídky,
- přiřazení kategorií,
- evidence množství, jednotek a termínů,
- přidání volitelné fotografie.

### 3. Reakce a obchodní proces

- reakce na nabídku nebo poptávku,
- přijetí nebo odmítnutí nabídky,
- automatické vytvoření objednávky po přijetí nabídky,
- zobrazení historie objednávek a filtrování podle období nebo stavu.

### 4. Komunikace a notifikace

- interní komunikace mezi uživateli,
- poznámky k nabídce nebo objednávce,
- e-mailové nebo SMS notifikace,
- upozornění na důležité změny stavu.

### 5. Reporty a administrativa

- přehled statistik pro administrátora,
- export dat do CSV nebo Excelu,
- reporty aktivních nabídek a poptávek,
- identifikace neaktivních účtů.

## Nefunkční požadavky

- responzivní webové rozhraní pro desktop i mobil,
- čeština jako primární jazyk rozhraní,
- důraz na jednoduché ovládání,
- rychlá odezva při přihlášení, notifikacích a generování faktur,
- zabezpečení osobních údajů a audit důležitých operací,
- cloudové nasazení s nízkými provozními náklady,
- škálování přibližně pro 2000 uživatelů,
- dokumentace API pomocí OpenAPI nebo Swaggeru.

## Poznámky k rozsahu

Role `odběratel` a `dodavatel` nejsou samostatné typy účtů. Jde o kontext, ve kterém jeden a tentýž uživatel vystupuje v rámci konkrétního inzerátu nebo procesu.
