# Dokumentace implementace

Tento dokument navazuje na puvodni navrhovy projekt **Village Marketplace** a shrnuje, jak byla domena prevedena do skutecne implementace 

## Prehled implementace

Implementace vznikla jako plnohodnotna webova aplikace rozdelena na dve samostatne casti:

- **frontend** v Reactu 18, TypeScriptu a Vite,
- **backend** v NestJS 11,
- **relacni databaze** MySQL pres Prisma ORM,
- **API dokumentace** pomoci Swaggeru,
- **testy** pomoci Jest, Supertest, Vitest a castecne Playwright konfigurace.

Oproti puvodnimu navrhu tedy neslo jen o prevod diagramu do databaze, ale o realizaci kompletniho beziciho systemu vcetne autentizace, formularu, API, validace, generovani dokumentu, notifikaci a testu.

## Jak implementace probihala

### 1. Zalozeni technicke architektury

Nejprve byl pripraven zakladni technologicky ramec:

- backend jako modularni NestJS aplikace,
- frontend jako SPA aplikace v Reactu,
- spolecna komunikace pres REST API,
- Prisma schema a migrace nad MySQL,
- Swagger dokumentace pro hlavni endpointy,
- vyvojove skripty pro lokalni spusteni frontendu i backendu.

Tato etapa prevedla puvodni obecny navrh "webova aplikace + MySQL" do konkretni implementacni podoby.

### 2. Implementace identity a pristupu

Dalsi krok pokryl vstup do systemu:

- registraci uzivatele,
- prihlaseni e-mailem a heslem,
- podporu Google OAuth,
- overeni telefonu pres SMS kod,
- obnovu hesla,
- JWT autentizaci,
- role `USER` a `ADMIN`.

Na frontendu se to promitlo do samostatnych stranek pro autentizaci, overeni telefonu a reset hesla. Na backendu vznikly moduly `auth`, `users`, guardy, dekoratory a datove struktury pro lokalni i externi identitu.

### 3. Prevod domeny do aplikacni logiky

Po autentizaci nasledovala hlavni obchodni cast systemu:

- nabidky a poptavky,
- jejich vytvareni, upravy a ruseni,
- kategorie,
- vlastni prehledy uzivatele,
- reakce na poptavky,
- zobrazeni detailu inzeratu.

Implementace zde nezustala jen u databazoveho modelu. Vznikly samostatne backendove moduly `offers`, `demands`, `categories`, `demand-responses` a na frontendu odpovidajici seznamy, detailni obrazovky i formulare pro vytvoreni a editaci.

### 4. Zavedeni objednavkoveho procesu

Dalsi etapou bylo dotazeni zivotniho cyklu obchodu:

- prijeti reakce na poptavku,
- vytvoreni objednavky z nabidky,
- evidence polozek objednavky,
- prace se stavy objednavky,
- generovani faktury PDF,
- stazeni faktury pres API.

Tady uz byla implementace detailnejsi nez puvodni koncept. Misto jednoducheho "objednavka vznikne po prijeti nabidky" vznikl samostatny workflow s vice stavy objednavky a navazujici fakturaci.

### 5. Rozsireni o platby, komunikaci a notifikace

Ve chvili, kdy byl zaklad objednavek hotovy, bylo potreba doplnit provozni detaily skutecne aplikace:

- platebni instrukce k objednavce,
- potvrzeni odeslani a prijeti platby,
- chatova vlakna nad reakci nebo objednavkou,
- interni notifikace pro dulezite udalosti,
- auditni zaznamy dulezitych akci.

Prave zde je dobre videt prechod od konceptu k realnemu informacnimu systemu. Implementace musela resit nejen "co system eviduje", ale take "jak spolu strany skutecne komunikuji" a "jak se pozna prubeh obchodu v case".

### 6. Administrativa, exporty a provozni doplneni

Po stabilizaci hlavnich uzivatelskych toku byla doplnena administrativni vrstva:

- prehled uzivatelu,
- filtrovani neaktivnich uctu,
- agregovane statistiky,
- exporty uzivatelu a objednavek do CSV,
- health endpoint,
- seed dat pro lokalni vyvoj a demonstraci systemu.

To odpovida puvodnim pozadavkum na reporting, ale implementace je prevedla do konkretnich API endpointu, DTO trid a testovanych exportnich vystupu.

### 7. AI vrstva jako nove rozsireni

Implementace navic pridala oblast, ktera v puvodnim konceptu vubec nebyla:

- AI generovani navrhu inzeratu,
- AI generovani ilustracniho obrazku,
- interni LLM abstrakci oddelujici aplikaci od konkretniho poskytovatele,
- frontendoveho asistenta pro doplneni navrhu do formulare.

Tato cast ukazuje, ze implementace uz nesledovala jen puvodni zadani, ale rozvijela projekt i smerem k modernimu rozsireni pouzitelnosti.

## O co se musel puvodni koncept rozsirit nebo upravit

Pri implementaci se ukazalo, ze puvodni koncept byl dobry jako domenovy zaklad, ale pro bezici aplikaci nestacil v nekolika dulezitych oblastech.

### 1. Koncept se musel zpresnit do konkretni technicke architektury

Puvodni dokumentace pocitala s webovou aplikaci a databazi MySQL, ale implementace musela rozhodnout i:

- rozdeleni na frontend a backend,
- konkretni frameworky,
- zpusob autentizace pres JWT,
- strukturu API,
- validaci vstupu,
- testovaci strategii,
- lokalni vyvojove prostredi.

Bez techto rozhodnuti by neslo navrh realne provozovat ani overovat.

### 2. Puvodni domena se sjednotila do jedne entity inzeratu

Misto oddeleneho modelovani nabidky a poptavky jako dvou samostatnych hlavnich objektu implementace pouzila jednotnou entitu `Ad` s typem:

- `OFFER`,
- `DEMAND`.

To zjednodusilo databazovy model i sdilenou logiku seznamu, detailu, kategorii a autorstvi. Soucasne se ale musela doplnit pravidla, ktera urcuji, ktera pole davaji smysl pro nabidku a ktera pro poptavku.

### 3. Bylo nutne doplnit detailni stavove modely

Puvodni koncept pocital se zakladnimi procesy, ale implementace potrebovala presneji rozlisovat stavy:

- stav inzeratu,
- stav reakce,
- stav objednavky,
- stav platby,
- stav faktury,
- stav SMS overeni.

Bez stavovych enumeraci by nebylo mozne bezpecne ridit workflow ani testovat prechody mezi jednotlivymi kroky.

### 4. Objednavkovy proces se rozsiril o dohodu a platbu

Puvodni navrh mluvil hlavne o vytvoreni objednavky a faktury. Implementace musela doplnit:

- snapshot dohody mezi stranami,
- jmeno, e-mail a telefon kupujiciho i prodavajiciho v dobe objednavky,
- polozky objednavky,
- instrukce k bankovni platbe,
- potvrzeni odeslani a prijeti platby.

To je dulezite hlavne proto, aby objednavka zustala konzistentni i v pripade, ze se pozdeji zmeni profil uzivatele nebo samotny inzerat.

### 5. Komunikace musela byt modelovana samostatne

Puvodni koncept pocital s poznamkami a jednoduchou komunikaci. Implementace to rozsyrila na:

- samostatne konverzace,
- typ kontextu konverzace,
- zpravy vazane na ucastniky,
- rizeni pristupu ke vlaknum.

Tim se komunikace zmenila z doplnkove poznamky na plnohodnotnou cast workflow.

### 6. Fotografie bylo potreba resit technicky, ne jen domenove

V navrhu byla fotografie jen jako volitelny atribut inzeratu. Implementace ji musela rozpracovat do samostatne oblasti:

- samostatna entita `AdImage`,
- prace se zdrojovym URL i binarnimi daty,
- storage vrstva pro ukladani souboru,
- limity velikosti requestu na backendu.

Jde o typicky priklad rozdilu mezi konceptem a implementaci: domenove staci "inzerat muze mit fotku", technicky je ale potreba vyresit zpusob ukladani a prenosu.

### 7. Puvodni pozadavek na notifikace se rozpadl do vice mechanismu

Navrh zminoval e-mailove a SMS notifikace obecne. Implementace musela oddelit:

- SMS overovani telefonu,
- interni aplikacni notifikace,
- mailovou vrstvu,
- metadata notifikaci pro navazani na konkretni objednavku, reakci nebo zpravu.

Tim se z puvodne jedne oblasti stalo nekolik technicky odlisnych mechanismu.

### 8. Administrativa se musela prevest na konkretni reporting

Puvodni koncept uvadel statistiky a exporty. Implementace to rozsyrila na:

- filtrovane seznamy uzivatelu,
- detekci neaktivnich uctu podle obdobi,
- agregovane statistiky za zvolene obdobi,
- CSV export uzivatelu,
- CSV export objednavek,
- audit administrativnich akci.

To je presne ta cast, kde se obecny pozadavek musel promenit ve velmi konkretni datove dotazy a vystupy.

### 9. Bezpecnostni a provozni vrstva musela byt explicitni

V navrhu byly bezpecnostni a provozni pozadavky spise deklarativni. Implementace je musela rozlozit do konkretnich prvku:

- guardy a role checky,
- hashovani hesel a tokenu,
- refresh tokeny,
- CORS konfiguraci,
- validacni pipe,
- audit log,
- health endpoint,
- seed data a testovatelne lokalni prostredi.

Tato vrstva v konceptu nebyla rozkreslena do detailu, ale v implementaci je zasadni.

### 10. Projekt se rozsiril o AI funkce

To je nejvetsi funkcni rozsireni oproti puvodnimu konceptu. Pribylo:

- generovani navrhu textu inzeratu,
- generovani obrazku,
- samostatna AI service,
- LLM adapter vrstva kvuli vymene poskytovatele,
- frontendovy AI asistent ve formularich.

Jde o novou oblast, ktera nebyla soucasti puvodniho zadani ani navrhovych diagramu.

## Shrnuti

Puvodni koncept poslouzil jako dobry zaklad pro domenu a hlavni procesy systemu. Skutecna implementace ale ukazala, ze mezi navrhem a bezici aplikaci je jeste nekolik dulezitych vrstev: technicka architektura, bezpecnost, stavove rizeni, ukladani souboru, provozni workflow, testy a administrativni nastroje.

Nejvetsi posun oproti puvodnimu navrhu nastal ve trech smerech:

- domena byla prevedena do presnych stavovych a datovych modelu,
- obchodni proces se rozsiril o komunikaci, platby a audit,
- nad puvodni zadani byla doplnena AI asistence pro tvorbu inzeratu.

Vysledkem tedy neni jen "implementovany koncept", ale rozsirena a technicky zpresnena verze puvodniho navrhu.
