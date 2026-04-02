# Village Marketplace

## Úvod do problému

Kolikrát se stane, že doma něco chybí nebo naopak přebývá, ale v okolí by se snadno našel někdo, kdo by to nabídl nebo využil. Cílem systému je umožnit jednoduché propojení lidí v rámci menší komunity, například vesnice nebo malého města, aby si mohli rychle předávat nabídky a poptávky po zboží a službách.

## Popis systému

Village Marketplace je webový informační systém určený pro rychlé propojení uživatelů v lokálním prostředí. V dokumentaci se používají pojmy `odběratel` a `dodavatel`, nejde však o samostatné typy účtů ani trvalé role v systému. V obou případech se jedná o stejného uživatele, který pouze v konkrétním procesu nebo u konkrétního inzerátu vystupuje buď jako ten, kdo poptává, nebo jako ten, kdo nabízí.

Systém umožňuje:

- zadávání poptávek na zboží nebo služby,
- vytváření nabídek uživatelem vystupujícím v daném případě jako dodavatel,
- zařazení inzerátů do kategorií pro přehlednější třídění a vyhledávání,
- evidenci množství a jednotky u inzerátů,
- přidávání fotografií k inzerátům,
- reakce na nabídky a poptávky,
- přijetí nebo odmítnutí nabídky,
- automatické vytvoření objednávky,
- generování faktury,
- zasílání notifikací,
- vedení jednoduché komunikace mezi stranami.

## Prostředí zadavatele

Systém je určen pro malé město nebo skupinu uživatelů s očekávaným počtem až 2000 uživatelů. Zadavatel má omezené IT zázemí a nevlastní serverovou infrastrukturu, proto je vhodné cloudové nasazení s nízkými pořizovacími i provozními náklady.

Přístup do systému bude probíhat přes webový prohlížeč na počítači i mobilních zařízeních. Důraz je kladen na jednoduchou obsluhu, vysokou dostupnost a srozumitelné uživatelské rozhraní i pro ne-IT uživatele.

## Volba databázového systému

Pro implementaci byl zvolen databázový systém MySQL.

Tato volba vychází především z toho, že backendová část projektu je již pro MySQL připravena prostřednictvím nástroje Prisma, přičemž proměnná `DATABASE_URL` je nastavena právě pro tento databázový systém a databázové schéma i migrační soubory z něj přímo vycházejí. MySQL zároveň představuje osvědčený relační databázový systém vhodný pro daný typ aplikace, protože projekt pracuje s provázanými entitami, jako jsou uživatelé, inzeráty, objednávky, zprávy, notifikace a faktury. Mezi další výhody patří široká podpora nástrojů, vysoká stabilita a relativně snadná správa při lokálním vývoji i následném nasazení.

## Funkční požadavky

- FR01: Systém musí umožnit registraci nového uživatele zadáním e-mailu, hesla a telefonního čísla.
- FR02: Systém musí umožnit přihlášení uživatele buď přes Google OAuth, nebo klasickým přihlášením pomocí e-mailu a hesla.
- FR03: Systém musí po registraci vyžadovat ověření telefonního čísla prostřednictvím SMS kódu.
- FR04: Systém musí umožnit změnu hesla po ověření identity starým heslem nebo SMS kódem.
- FR05: Systém musí podporovat přístup podle rolí, zejména roli `Admin` se správou uživatelů.
- FR06: Systém musí umožnit uživateli vystupujícímu v daném případě jako odběratel vytvořit nový požadavek s popisem, množstvím, jednotkou, termínem, volitelnou kategorií a volitelnou fotografií a zobrazit seznam svých aktivních požadavků.
- FR07: Systém musí umožnit uživateli vystupujícímu v daném případě jako odběratel upravit nebo smazat svůj aktivní požadavek před přijetím nabídky.
- FR08: Systém musí umožnit uživateli vystupujícímu v daném případě jako dodavatel zobrazit aktivní požadavky ostatních uživatelů a vytvořit k nim nabídku s množstvím, cenou a poznámkou.
- FR09: Systém musí umožnit uživateli vystupujícímu v daném případě jako dodavatel upravit nebo smazat svou nabídku, pokud ještě nebyla přijata druhou stranou.
- FR10: Systém musí umožnit uživateli vystupujícímu v daném případě jako dodavatel vytvořit vlastní nabídku zboží nebo služby, přiřadit jí kategorii, přiložit k ní volitelnou fotografii a zobrazit seznam svých aktivních nabídek.
- FR11: Systém musí umožnit uživateli vystupujícímu v daném případě jako odběratel prohlížet nabídky k jeho požadavku nebo obecné aktivní nabídky ostatních uživatelů, filtrovat je podle kategorií a reagovat na ně.
- FR12: Systém musí po přijetí nabídky automaticky vytvořit objednávku a zobrazit ji v historii oběma stranám.
- FR13: Systém musí umožnit uživateli zobrazit přehled svých objednávek za vybrané období a podle stavu.
- FR14: Systém musí umožnit adminovi exportovat globální statistiky, například počet uživatelů, objednávek a celkovou hodnotu transakcí, do Excelu nebo CSV.
- FR15: Systém musí umožnit automatickou generaci a odeslání faktury ve formátu PDF po vytvoření objednávky na e-mail obou stran.
- FR16: Systém musí generovat report o aktivních požadavcích a nabídkách pro uživatele.
- FR17: Systém musí umožnit adminovi zobrazit seznam dlouho neaktivních účtů s možností filtrace a exportu.
- FR18: Systém musí umožnit oběma stranám konkrétní interakce přidávat k nabídce nebo požadavku textové poznámky viditelné oběma stranám.
- FR19: Systém musí uživatele upozornit notifikací nebo e-mailem na důležité události, jako je nová nabídka, přijetí nebo odmítnutí nabídky, nebo vytvoření objednávky.
- FR20: Systém musí umožnit odeslání SMS nebo e-mailové notifikace při ověřování telefonního čísla a při důležitých změnách stavu objednávky.
- FR21: Systém musí umožnit jednoduchou vnitřní komunikaci mezi dvěma uživateli u konkrétní nabídky nebo objednávky.

## Nefunkční požadavky

- NFR01: Přihlášení a SMS ověření nesmí trvat déle než 3 sekundy.
- NFR02: Generování a odeslání faktury PDF nesmí trvat déle než 5 sekund.
- NFR03: Real-time notifikace musí dorazit do 2 sekund.
- NFR04: Systém musí být responzivní pro zařízení od šířky 320 px do 1920 px.
- NFR05: Rozhraní musí podporovat češtinu jako primární jazyk a mít intuitivní navigaci.
- NFR06: Databáze musí být automaticky zálohována každý den s retencí 30 dnů.
- NFR07: Kritické funkce, zejména vytvoření objednávky, musí být spolehlivé bez ztráty dat.
- NFR08: Osobní data, například e-mail a telefon, musí být v databázi šifrována.
- NFR09: Po 15 minutách nečinnosti musí být uživatelská relace ukončena.
- NFR10: Admin musí být automaticky upozorněn na chyby systému.
- NFR11: Všechny důležité transakce musí být logovány pro audit po dobu 12 měsíců.
- NFR12: Systém musí zvládnout 2000 současně připojených zákazníků.
- NFR13: Všechny API endpointy musí být dokumentovány pomocí OpenAPI nebo Swaggeru.
- NFR14: Databáze musí zvládnout alespoň 1 milion záznamů a být vhodně indexována.
- NFR15: Systém musí automaticky detekovat a restartovat selhané služby do 30 sekund.

## Aktéři

- Uživatel: hlavní aktér systému. Podle situace může u konkrétního inzerátu nebo procesu vystupovat jako odběratel nebo jako dodavatel, nejde však o dva odlišné typy uživatelů.
- Admin: správce systému s plnými oprávněními.
- SMS Provider: externí služba pro zasílání ověřovacích SMS kódů.
- Externí přihlašovací systém Google: služba poskytující OAuth autentizaci.

## Přehled use case

Poznámka: V následujících use casech označují pojmy `odběratel` a `dodavatel` pouze kontextové vystupování jednoho typu aktéra, tedy uživatele.

- Registrace uživatele včetně ověření telefonu pomocí SMS.
- Přihlášení pomocí e-mailu a hesla nebo přes Google OAuth.
- Vytvoření poptávky uživatelem v roli odběratele.
- Publikování nabídky uživatelem v roli dodavatele.
- Reakce na poptávku nebo nabídku.
- Přijetí nabídky a vytvoření objednávky.
- Generování faktury a notifikací.
- Zobrazení reportů a statistik.
- Správa uživatelů administrátorem.

## Use case specifikace

### UC01 Registrace

- Primární aktér: Uživatel
- Předpoklady: Uživatel nemá účet a má platný e-mail i telefonní číslo.
- Výsledek: Účet je vytvořen, telefon ověřen a uživatel se může přihlásit.

Hlavní scénář:

1. Uživatel zadá e-mail, heslo a telefonní číslo.
2. Systém odešle SMS kód.
3. Uživatel zadá ověřovací kód.
4. Systém vytvoří účet.

Alternativy:

- Neplatné telefonní číslo.
- Třikrát chybně zadaný kód, po kterém následuje časový blok.

### UC02 Přihlášení

- Primární aktér: Registrovaný uživatel
- Předpoklady: Účet existuje a je ověřen.
- Výsledek: Uživatel má aktivní relaci a načtená oprávnění.

Hlavní scénář:

1. Uživatel zvolí přihlášení přes Google OAuth nebo přes e-mail a heslo.
2. Systém ověří identitu.
3. Systém zobrazí hlavní stránku.

Alternativy:

- Selhání Google OAuth a návrat na klasické přihlášení.
- Více chybných pokusů a dočasné zablokování účtu.

### UC03 Vytvoření požadavku

- Primární aktér: Uživatel v roli odběratele
- Předpoklady: Uživatel je přihlášen a v tomto procesu vystupuje jako odběratel.
- Výsledek: Požadavek je uložen, zveřejněn a dostupný ostatním uživatelům, kteří na něj mohou reagovat jako dodavatelé.

Hlavní scénář:

1. Uživatel zadá popis, množství, jednotku a termín.
2. Uživatel může zvolit kategorii požadavku.
3. Uživatel může přiložit volitelnou fotografii k požadavku.
4. Systém požadavek uloží a publikuje.
5. Systém odešle notifikaci relevantním uživatelům.

### UC04 Odeslání nabídky

- Primární aktér: Uživatel v roli dodavatele
- Předpoklady: Uživatel je přihlášen, v tomto procesu vystupuje jako dodavatel a požadavek je aktivní.
- Výsledek: Nabídka je přiřazena k požadavku a druhá strana je informována.

Hlavní scénář:

1. Uživatel vybere aktivní požadavek.
2. Zadá množství, cenu a poznámku.
3. Systém nabídku uloží a odešle notifikaci druhé straně.

### UC05 Přijetí nabídky

- Primární aktér: Uživatel v roli odběratele
- Předpoklady: Existuje aktivní nabídka.
- Výsledek: Je vytvořena objednávka, vystavena faktura a otevřena komunikace mezi stranami.

Hlavní scénář:

1. Uživatel vybere nabídku a potvrdí množství.
2. Systém vytvoří objednávku.
3. Systém vygeneruje fakturu PDF.
4. Systém odešle notifikaci oběma stranám.

### UC06 Správa uživatelů

- Primární aktér: Admin
- Předpoklady: Admin je přihlášen.
- Výsledek: Uživatelé jsou zobrazeni, filtrovatelní a exportovatelní.

Hlavní scénář:

1. Admin otevře seznam všech uživatelů.
2. Admin použije filtry, například na neaktivní účty.
3. Admin exportuje přehled nebo statistiky.

### UC07 Publikování nabídky

- Primární aktér: Uživatel v roli dodavatele
- Předpoklady: Uživatel je přihlášen a v tomto procesu vystupuje jako dodavatel.
- Výsledek: Nabídka je zveřejněna a dostupná ostatním uživatelům.

Hlavní scénář:

1. Uživatel zadá popis zboží nebo služby, dostupné množství, jednotku a cenu.
2. Uživatel vybere kategorii nabídky.
3. Uživatel může přiložit volitelnou fotografii k nabídce.
4. Systém nabídku uloží a publikuje.
5. Systém odešle notifikaci relevantním uživatelům.

## Vazba na diagramy

- [`class-diagram-konceptualni.drawio`](./class-diagram-konceptualni.drawio) zachycuje konceptuální pohled na entity, jejich atributy, základní operace a vztahy.
- [`class logicky.drawio`](./class%20logicky.drawio) zachycuje logický datový model včetně primárních a cizích klíčů a základních datových typů.
- [`ERD.drawio`](./ERD.drawio) zachycuje databázový pohled na relační strukturu systému.

- Tento dokument je s oběma diagramy sladěn zejména v oblasti inzerátů, reakcí, objednávek, faktur, notifikací a komunikace mezi uživateli.

## Konstrukty class diagramu

Konceptuální diagram je tam, kde to dává smysl, rozšířen i o typické konstrukty class diagramu:

- základní metody u hlavních tříd, například u `Uživatel`, `Inzerát`, `Reakce`, `Objednávka` a `Faktura`,
- vztahy mezi třídami rozlišené na `asociaci`, `agregaci` a `kompozici`,
- návrhy dědičnosti, například pro specializace `Uživatel`, `Notifikace` a `Reakce`,
- role, a to jak systémové přes třídu `Role`, tak kontextové role uživatele v procesu, například `odběratel` a `dodavatel`.
