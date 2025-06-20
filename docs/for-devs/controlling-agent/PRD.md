# PRD: Controlling AI Agent

## 1. Termék áttekintés
### 1.1 Dokumentum címe és verziója
- PRD: Controlling AI Agent
- Verzió: 1.0

### 1.2 Termék összefoglaló
Ez a dokumentum a Controlling AI Agent követelményeit vázolja fel. Az eszköz célja, hogy támogassa a pénzügyi osztály munkáját a pénzügyi egyeztetés fáradságos és hibalehetőségekkel teli folyamatának automatizálásával. Az alkalmazás intelligensen párosítja a havi bankszámlakivonatokat a belső nyilvántartásokkal, például a számlákkal és a bérlistákkal, hogy azonosítsa az eltéréseket.

Az alapvető funkcionalitás magában foglalja a pénzügyi dokumentumok feldolgozását, szabályalapú és intelligens párosítási algoritmusok alkalmazását, valamint az eredmények egyértelmű bemutatását. Kulcsfontosságú funkció a rendszer azon képessége, hogy tanul a felhasználó által végzett manuális korrekciókból, fokozatosan javítva a pontosságát és csökkentve az emberi beavatkozás szükségességét. Az elsődleges cél a havi pénzügyi zárási folyamat hatékonyságának és pontosságának növelése.

## 2. Célok
### 2.1 Üzleti célok
- A havi pénzügyi egyeztetési folyamathoz szükséges munkaórák jelentős csökkentése.
- A pénzügyi jelentések pontosságának növelése az emberi hibák minimalizálásával.
- A párosított és nem párosított tranzakciók egyértelmű és ellenőrizhető nyomon követésének biztosítása.
- A havi könyvelési zárás felgyorsítása.

### 2.2 Felhasználói célok
- A bankszámlakivonatok és a belső főkönyvek manuális összehasonlításának időigényes feladatának automatizálása.
- A manuális vizsgálatot igénylő tranzakciók gyors és egyszerű azonosítása.
- Egyszerű felület biztosítása az eltérések megoldásához és a rendszer tanításához.
- A rendszer kimenetének megbízhatósága és a pontos pénzügyi záráshoz való felhasználhatósága.

### 2.3 Nem célok
- Ez az eszköz nem egy teljes körű számviteli vagy könyvelési rendszer.
- Nem használható számlák kiállítására, fizetések feldolgozására vagy követelések/tartozások kezelésére.
- Az alkalmazás nem végez adószámításokat és nem generál hivatalos adóbevallásokat.
- Nem célja a pénzügyi számviteli szoftverek helyettesítése, hanem egy hatékony egyeztető segédprogramként való működés.

## 3. Felhasználói perszónák
### 3.1 Főbb felhasználói típusok
- Pénzügyi Munkatárs
- Pénzügyi Ellenőr

### 3.2 Alap perszóna részletek
- **László (Pénzügyi Munkatárs)**: László felelős a havi számlazárásért. Jelenlegi folyamata magában foglalja a bankszámlakivonatok és a belső nyilvántartások manuális exportálását táblázatokba, és több napot tölt a tételek aprólékos összehasonlításával. Szüksége van egy eszközre, amely automatizálja ezt a párosítási folyamatot, felszabadítva őt, hogy az adatrögzítés helyett a valódi pénzügyi eltérések elemzésére és megoldására összpontosíthasson.

### 3.3 Szerepkör alapú hozzáférés
- **Pénzügyi Felhasználó**: Feltölthet bankszámlakivonatokat és belső főkönyveket, elindíthatja az egyeztetési folyamatot, megtekintheti az eredményeket, és manuális párosításokat végezhet a rendszer tanításához.
- **Adminisztrátor**: Kezelheti a felhasználói hozzáféréseket és a rendszerkonfigurációkat (pl. párosítási szabályok küszöbértékei, adatforrás-konfigurációk).

## 4. Funkcionális követelmények
- **Dokumentum feltöltés** (Prioritás: Magas)
    - A felhasználóknak képesnek kell lenniük bankszámlakivonatok és belső nyilvántartások feltöltésére `.csv`, `.xlsx` és Google Sheets formátumokban.
- **Adatfeldolgozás (Parsing)** (Prioritás: Magas)
    - A rendszernek helyesen kell értelmeznie a különböző strukturált fájlformátumokat és normalizálni kell az adatokat a feldolgozáshoz.
- **Egyeztető motor** (Prioritás: Magas)
    - A rendszernek automatikusan párosítania kell a tranzakciókat a bankszámlakivonatból a belső nyilvántartásokkal egy szabályrendszer alapján (pl. összeg, dátum, hivatkozási szöveg egyezése).
- **Eltérés jelentés** (Prioritás: Magas)
    - A felhasználói felületnek egyértelműen meg kell jelenítenie három különálló listát: sikeresen párosított tételek, nem párosított tételek a bankszámlakivonatból, és nem párosított tételek a belső nyilvántartásokból.
- **Manuális párosítási felület** (Prioritás: Magas)
    - A rendszernek egy intuitív felületet kell biztosítania a felhasználók számára, hogy manuálisan kiválasszanak és párosítsanak egy tételt a nem párosított banki listából egy tétellel a nem párosított belső nyilvántartások listájából.
- **Tanuló modul** (Prioritás: Közepes)
    - A rendszernek képesnek kell lennie tanulni a felhasználó által végzett manuális párosításokból, hogy javítsa a jövőbeli automatizált egyeztetési futamok pontosságát.

## 5. Felhasználói élmény
### 5.1. Belépési pontok és első felhasználói folyamat
- A felhasználók a Google-fiókjukkal hitelesítenek és egy műszerfalra érkeznek.
- A műszerfal egyértelmű felhívást jelenít meg egy új egyeztetés indítására egy adott időszakra (pl. "2024. júniusi egyeztetés").
- Az első felhasználókat a rendszer végigvezeti a szükséges fájlok (bankszámlakivonat és belső főkönyv) feltöltésén.

### 5.2. Alapvető élmény
- **1. lépés: Feltöltés**: A felhasználó kiválaszt egy hónapot és feltölti a megfelelő bankszámlakivonat és belső főkönyvi fájlokat.
- **2. lépés: Feldolgozás**: A felhasználó rákattint az "Egyeztetés futtatása" gombra. A rendszer a háttérben feldolgozza a fájlokat, és egy folyamatjelzőt mutat.
- **3. lépés: Áttekintés**: A rendszer három egyértelmű táblázatban mutatja be az eredményeket: Párosított, Nem párosított banki tételek, és Nem párosított belső tételek. A kulcsadatok (Dátum, Összeg, Leírás) láthatók.
- **4. lépés: Megoldás**: A felhasználó kiválaszthat egy vagy több tételt a nem párosított listákból, és manuálisan megerősítheti a párosítást. A tételek ezután a "Párosított" táblázatba kerülnek.

### 5.3. Haladó funkciók és peremfeltételek
- A rendszernek képesnek kell lennie javaslatokat tenni lehetséges párosításokra egy megbízhatósági pontszámmal (pl. "Az összegek egyeznek, a dátumok 1 nap eltéréssel").
- Egy-a-többhöz és több-az-egyhez párosítások kezelése (pl. egy banki befizetés több számlát fedez).
- Kisebb összegbeli eltérések vagy hivatkozási szövegbeli változatok tolerálása.

### 5.4. UI/UX kiemelések
- Tiszta, letisztult műszerfal, amely a fő feladatra összpontosít.
- Egyszerű fájlfeltöltési mechanizmus, esetleg drag-and-drop támogatással.
- A nem párosított tételek egymás melletti megjelenítése az egyszerű összehasonlítás érdekében.
- Azonnali visszajelzés, amikor egy manuális párosítás megtörténik.

## 6. Narratíva
László egy pénzügyi munkatárs, aki retteg minden hónap első hetétől. Ez azt jelenti, hogy napokat kell töltenie több ezer sornyi bankszámlakivonat kézi egyeztetésével a cég belső nyilvántartásaival táblázatokban. Ez egy unalmas, szem-megerőltető folyamat, ahol egyetlen hiba az egész havi zárást elronthatja. Felfedezi a Controlling AI Agent-et és úgy dönt, kipróbálja. Feltölti a két táblázatot, és kevesebb mint egy perc alatt az eszköz egy jelentést tár elé. A tételek 95%-a már párosítva van. Most már az energiáját a fennmaradó 5% komplex eltérésre összpontosíthatja, amelyeket gyorsan megold az alkalmazás egyszerű felületén keresztül. Ami korábban két napig tartott, az most két órát vesz igénybe, és sokkal nagyobb bizalommal van a pénzügyi nyilvántartások pontosságában.

## 7. Sikerességi metrikák
### 7.1. Felhasználó-központú metrikák
- Az egyeztetési feladatra fordított idő ciklusonként.
- A ciklusonként szükséges manuális párosítások száma (ennek idővel csökkennie kell).
- Felhasználói elégedettségi pontszám (NPS vagy egy egyszerű visszajelzési űrlap).

### 7.2. Üzleti metrikák
- Az emberi beavatkozás nélkül automatikusan egyeztetett tranzakciók százalékos aránya.
- A havi könyvelési záráshoz szükséges teljes idő csökkenése.
- Az ellenőrzések során talált egyeztetési hibák mért csökkenése.

### 7.3. Technikai metrikák
- Egy standard dokumentumkészlet átlagos feldolgozási ideje.
- A párosító algoritmus pontossága, egy "arany standard" adathalmazhoz mérve.
- Rendszer rendelkezésre állása és megbízhatósága.

## 8. Technikai megfontolások
### 8.1. Integrációs pontok
- Az elsődleges integrációs pont a Google Workspace, `.csv`, `.xlsx` és Google Sheets fájlok olvasására a Google Drive-ból.
- A jövőbeni integrációk magukban foglalhatják a közvetlen csatlakozást banki API-khoz vagy számviteli szoftverekhez.

### 8.2. Adattárolás és adatvédelem
- Minden pénzügyi adat rendkívül érzékeny, és a Kibit adatvédelmi szabályzatának megfelelően kell kezelni.
- Az adatokat mind átvitel közben (TLS), mind tároláskor titkosítani kell.
- Az alapvető logikához használt külső felhőszolgáltatások használatát ellenőrzött és szükség esetén anonimizált módon kell végezni.
- A "tanuló" komponens által generált adatokat (azaz a párosítási szabályokat) szintén biztonságosan kell tárolni a jóváhagyott környezetben.

### 8.3. Skálázhatóság és teljesítmény
- A rendszernek hatékonyan kell működnie még több tízezer sort tartalmazó bemeneti fájlok esetén is.
- Az egyeztetési folyamatot aszinkron módon kell kezelni, hogy ne blokkolja a felhasználói felületet nagy feladatok esetén.
- Az adatfeldolgozó és párosító algoritmusokat optimalizálni kell a teljesítményre.

### 8.4. Lehetséges kihívások
- A bemeneti fájlok szerkezetének nagyfokú változatossága és szabványosításának hiánya.
- Robusztus "fuzzy" párosítási szabályok meghatározása, amelyek képesek kezelni a szövegbeli, dátumbeli és összegbeli változatokat anélkül, hogy hamis pozitív eredményeket hoznának.
- Annak biztosítása, hogy a tanuló komponens hatékony, általánosítható szabályokat hozzon létre a manuális párosításokból.

## 9. Mérföldkövek és ütemezés
### 9.1. Projekt becslés
- Közepes: 3-6 hét

### 9.2. Csapat mérete és összetétele
- Kis csapat: 1-3 fő
    - 1-2 mérnök, 1 termékmenedzser/QA szakember

### 9.3. Javasolt fázisok
- **1. fázis: Alap motor és CLI** (2 hét): Az összes megadott fájltípushoz tartozó adatfeldolgozók fejlesztése és az alap egyeztető motor megvalósítása egy alapkészletű szigorú párosítási szabállyal. Parancssori felület biztosítása a teszteléshez.
    - Kulcsfontosságú eredmények: Egy CLI eszköz, amely két bemeneti fájlt fogad el, és jelentéseket készít a párosított és nem párosított tételekről.
- **2. fázis: Webes felhasználói felület és manuális párosítás** (2 hét): Az egyszerű webes felület felépítése a fájlfeltöltéshez, az egyeztetési eredmények megjelenítéséhez és a felhasználók számára a manuális párosítások elvégzéséhez.
    - Kulcsfontosságú eredmények: Egy működőképes webalkalmazás, amely lefedi az alapvető felhasználói utat.
- **3. fázis: Tanuló modul és Fuzzy logika** (1-2 hét): A manuális párosításokból tanuló mechanizmus megvalósítása. A párosító algoritmus továbbfejlesztése fuzzy logikával a kisebb eltérések kezelésére.
    - Kulcsfontosságú eredmények: Egy "okosabb" rendszer, amely idővel javítja az automatikus párosítási pontosságát.

## 10. Felhasználói történetek
### 10.1. Biztonságos bejelentkezés az alkalmazásba
- **ID**: US-101
- **Leírás**: Pénzügyi felhasználóként szeretnék biztonságosan bejelentkezni az alkalmazásba a Google-fiókommal, hogy csak a jogosult személyzet férhessen hozzá az érzékeny pénzügyi adatokhoz.
- **Elfogadási kritériumok**:
    - Az alkalmazás a Google OAuth 2.0-t használja a hitelesítéshez.
    - A hozzáférés a cég domainjén belüli felhasználókra korlátozódik.
    - A munkamenetem biztonságosan van kezelve.

### 10.2. Pénzügyi dokumentumok feltöltése egyeztetéshez
- **ID**: US-102
- **Leírás**: Pénzügyi felhasználóként szeretnék feltölteni egy bankszámlakivonatot és egy belső főkönyvi fájlt egy adott hónapra az egyeztetési folyamat megkezdéséhez.
- **Elfogadási kritériumok**:
    - A felhasználói felület egyértelmű lehetőséget biztosít a fájlok feltöltésére.
    - Feltölthetek fájlokat `.csv` és `.xlsx` formátumban.
    - A rendszer képes csatlakozni és olvasni egy Google Sheet-et is.
    - A rendszer ellenőrzi, hogy a fájlok használható formátumúnak tűnnek-e.

### 10.3. Az automatikus egyeztetési folyamat futtatása
- **ID**: US-103
- **Leírás**: Pénzügyi felhasználóként szeretném elindítani az automatikus párosítási folyamatot, hogy a rendszer összehasonlíthassa a két dokumentumot számomra.
- **Elfogadási kritériumok**:
    - Van egy egyértelmű "Egyeztetés futtatása" gomb.
    - A rendszer visszajelzést ad arról, hogy a folyamat elindult (pl. egy betöltő pörgettyű).
    - A folyamat a háttérben fut, és nem fagyasztja le a felhasználói felületet.

### 10.4. Az egyeztetés eredményeinek megtekintése
- **ID**: US-104
- **Leírás**: Pénzügyi felhasználóként szeretném egyértelmű és érthető formátumban látni az egyeztetés eredményeit.
- **Elfogadási kritériumok**:
    - Az eredmények három különálló listában jelennek meg: Párosított tételek, Nem párosított banki tranzakciók és Nem párosított belső nyilvántartások.
    - Minden lista táblázatként jelenik meg a releváns oszlopokkal (pl. Dátum, Leírás, Összeg).
    - Az egyes listákban lévő tételek összesített száma egyértelműen látható.

### 10.5. Nem párosított tételek manuális párosítása
- **ID**: US-105
- **Leírás**: Pénzügyi felhasználóként szeretném manuálisan párosítani a nem párosított banki listából egy tételt egy megfelelő tétellel a nem párosított belső nyilvántartások listájából az eltérések megoldásához.
- **Elfogadási kritériumok**:
    - Kiválaszthatok egy tételt a banki listából.
    - Kiválaszthatok egy tételt a belső nyilvántartások listájából.
    - Egy "Párosítás" gomb aktívvá válik, amikor két tétel van kiválasztva.
    - A "Párosítás" gombra kattintva mindkét tétel átkerül a "Párosított tételek" listába.

### 10.6. A rendszer tanul a manuális párosításokból
- **ID**: US-106
- **Leírás**: Rendszerként, amikor egy felhasználó manuális párosítást végez, szeretném megtanulni annak a párosításnak a jellemzőit, hogy javítsam a jövőbeli automatikus egyeztetéseket.
- **Elfogadási kritériumok**:
    - Amikor egy felhasználó két tételt párosít, a rendszer elemzi azok tulajdonságait (pl. kulcsszavak a leírásban, dátumkülönbség).
    - Ezen elemzés alapján egy új, specifikus párosítási szabály generálódik és tárolódik.
    - Ez az új szabály a következő egyeztetési futamok során alkalmazásra kerül. 






-----------------------------------------------------------------------------






# (ENG) PRD: Controlling AI Agent

## 1. Product overview
### 1.1 Document title and version
- PRD: Controlling AI Agent
- Version: 1.0

### 1.2 Product summary
This document outlines the requirements for the Controlling AI Agent, a tool designed to support the finance department by automating the tedious and error-prone process of financial reconciliation. The application will intelligently match monthly bank statements against internal records like invoices and salary lists to identify discrepancies.

The core functionality involves ingesting financial documents, applying rule-based and intelligent matching algorithms, and clearly presenting the results. A key feature is the system's ability to learn from manual corrections made by the user, progressively improving its accuracy and reducing the need for human intervention over time. The primary goal is to increase the efficiency and accuracy of the monthly financial closing process.

## 2. Goals
### 2.1 Business goals
- Significantly reduce the man-hours required for the monthly financial reconciliation process.
- Increase the accuracy of financial reporting by minimizing human errors.
- Provide a clear and auditable trail of matched and unmatched transactions.
- Accelerate the monthly closing of the books.

### 2.2 User goals
- To automate the time-consuming task of manually comparing bank statements and internal ledgers.
- To quickly and easily identify all transactions that require manual investigation.
- To have a simple interface for resolving discrepancies and teaching the system.
- To trust the system's output and rely on it for accurate financial closing.

### 2.3 Non-goals
- This tool is not a complete accounting or bookkeeping system.
- It will not be used for creating invoices, processing payments, or managing accounts payable/receivable.
- The application will not perform tax calculations or generate official tax forms.
- It is not intended to replace financial accounting software but to act as a powerful reconciliation utility.

## 3. User personas
### 3.1 Key user types
- Finance Officer
- Financial Controller

### 3.2 Basic persona details
- **László (Finance Officer)**: László is responsible for the monthly closing of accounts. His current process involves manually exporting bank statements and internal records into spreadsheets and spending several days meticulously comparing line items. He needs a tool that can automate this matching process, freeing him up to focus on analyzing and resolving true financial discrepancies rather than performing data entry.

### 3.3 Role-based access
- **Finance User**: Can upload bank statements and internal ledgers, initiate the reconciliation process, view the results, and perform manual matches to train the system.
- **Admin**: Can manage user access and system configurations (e.g., matching rule thresholds, data source configurations).

## 4. Functional requirements
- **Document Upload** (Priority: High)
    - Users must be able to upload bank statements and internal records in `.csv`, `.xlsx`, and Google Sheets formats.
- **Data Parsing** (Priority: High)
    - The system must correctly parse various structured file formats and normalize the data for processing.
- **Reconciliation Engine** (Priority: High)
    - The system must automatically match transactions from the bank statement to the internal records based on a set of rules (e.g., matching amount, date, reference text).
- **Discrepancy Reporting** (Priority: High)
    - The UI must clearly display three distinct lists: successfully matched items, unmatched items from the bank statement, and unmatched items from the internal records.
- **Manual Matching Interface** (Priority: High)
    - The system must provide an intuitive interface for users to manually select and match an item from the unmatched bank list with an item from the unmatched internal records list.
- **Learning Module** (Priority: Medium)
    - The system must be able to learn from manual matches performed by the user to improve the accuracy of future automated reconciliation runs.

## 5. User experience
### 5.1. Entry points & first-time user flow
- Users authenticate via their Google account and land on a dashboard.
- The dashboard presents a clear call-to-action to start a new reconciliation for a specific period (e.g., "Reconcile June 2024").
- First-time users are guided to upload the necessary files (bank statement and internal ledger).

### 5.2. Core experience
- **Step 1: Upload**: The user selects a month and uploads the corresponding bank statement and internal ledger files.
- **Step 2: Process**: The user clicks a "Run Reconciliation" button. The system processes the files in the background, showing a progress indicator.
- **Step 3: Review**: The system presents the results in three clear tables: Matched, Unmatched Bank Items, and Unmatched Internal Items. Key data points (Date, Amount, Description) are visible.
- **Step 4: Resolve**: The user can select one or more items from the unmatched lists and manually confirm a match. The items then move to the "Matched" table.

### 5.3. Advanced features & edge cases
- The system should be able to suggest potential matches with a confidence score (e.g., "Amounts match, dates are 1 day apart").
- Handling one-to-many and many-to-one matches (e.g., one bank deposit covering multiple invoices).
- Tolerating minor discrepancies in amounts or variations in reference text.

### 5.4. UI/UX highlights
- A clean, uncluttered dashboard focused on the core task.
- Simple file upload mechanism, possibly with drag-and-drop support.
- Side-by-side display of unmatched items to facilitate easy comparison.
- Instant feedback when a manual match is made.

## 6. Narrative
László is a finance officer who dreads the first week of every month. It means spending days manually matching thousands of lines in bank statements against the company's internal records in spreadsheets. It's a tedious, eye-straining process where a single mistake can throw off the entire month's closing. He discovers the Controlling AI Agent and decides to try it. He uploads the two spreadsheets, and in less than a minute, the tool presents him with a report. 95% of the items are already matched. He can now focus his energy on the remaining 5% of complex discrepancies, resolving them quickly through the app's simple interface. What used to take two days now takes him two hours, and he has much higher confidence in the accuracy of the financial records.

## 7. Success metrics
### 7.1. User-centric metrics
- Time spent on the reconciliation task per cycle.
- Number of manual matches required per cycle (this should decrease over time).
- User satisfaction score (NPS or a simple feedback form).

### 7.2. Business metrics
- Percentage of transactions automatically reconciled without human intervention.
- Reduction in the overall time required to close the monthly books.
- Measured decrease in reconciliation-related errors found in audits.

### 7.3. Technical metrics
- Average processing time for a standard set of documents.
- Accuracy of the matching algorithm, measured against a golden dataset.
- System uptime and reliability.

## 8. Technical considerations
### 8.1. Integration points
- The primary integration point is Google Workspace for reading `.csv`, `.xlsx`, and Google Sheets files from Google Drive.
- Future integrations could include connecting directly to bank APIs or accounting software.

### 8.2. Data storage & privacy
- All financial data is highly sensitive and must be handled according to Kibit's data policy.
- Data must be encrypted both in transit (TLS) and at rest.
- Any use of external cloud services for the core logic must be done in a controlled and, if necessary, anonymized way.
- The data generated by the "learning" component (i.e., matching rules) must also be stored securely within the approved environment.

### 8.3. Scalability & performance
- The system must perform efficiently even with input files containing tens of thousands of rows.
- The reconciliation process should be handled asynchronously to prevent blocking the user interface on large jobs.
- The data parsing and matching algorithms should be optimized for performance.

### 8.4. Potential challenges
- High variability and lack of standardization in the structure of input files.
- Defining robust fuzzy matching rules that can handle variations in text, dates, and amounts without creating false positives.
- Ensuring the learning component creates effective, generalizable rules from manual matches.

## 9. Milestones & sequencing
### 9.1. Project estimate
- Medium: 3-6 weeks

### 9.2. Team size & composition
- Small Team: 1-3 total people
    - 1-2 engineers, 1 product manager/QA specialist

### 9.3. Suggested phases
- **Phase 1: Core Engine and CLI** (2 weeks): Develop the data parsers for all specified file types and implement the core reconciliation engine with a baseline set of strict matching rules. Provide a command-line interface for testing.
    - Key deliverables: A CLI tool that accepts two input files and outputs reports for matched and unmatched items.
- **Phase 2: Web UI and Manual Matching** (2 weeks): Build the simple web interface for file uploading, displaying reconciliation results, and allowing users to perform manual matches.
    - Key deliverables: A functional web application covering the core user journey.
- **Phase 3: Learning Module and Fuzzy Logic** (1-2 weeks): Implement the mechanism that learns from manual matches. Enhance the matching algorithm with fuzzy logic to handle minor discrepancies.
    - Key deliverables: A "smarter" system that improves its auto-matching accuracy over time.

## 10. User stories
### 10.1. Securely log in to the application
- **ID**: US-101
- **Description**: As a Finance User, I want to securely log in to the application using my Google account so that only authorized personnel can access the sensitive financial data.
- **Acceptance criteria**:
    - The application uses Google OAuth 2.0 for authentication.
    - Access is restricted to users within the company's domain.
    - My session is securely managed.

### 10.2. Upload financial documents for reconciliation
- **ID**: US-102
- **Description**: As a Finance User, I want to upload a bank statement and an internal ledger file for a specific month to begin the reconciliation process.
- **Acceptance criteria**:
    - The UI provides a clear option to upload files.
    - I can upload files in `.csv` and `.xlsx` formats.
    - The system can also connect to and read a Google Sheet.
    - The system validates that the files appear to be in a usable format.

### 10.3. Run the automatic reconciliation process
- **ID**: US-103
- **Description**: As a Finance User, I want to initiate the automatic matching process so that the system can compare the two documents for me.
- **Acceptance criteria**:
    - There is a clear "Run Reconciliation" button.
    - The system provides feedback that the process has started (e.g., a loading spinner).
    - The process runs in the background and does not freeze the UI.

### 10.4. View the results of the reconciliation
- **ID**: US-104
- **Description**: As a Finance User, I want to see the results of the reconciliation in a clear and understandable format.
- **Acceptance criteria**:
    - The results are displayed in three separate lists: Matched Items, Unmatched Bank Transactions, and Unmatched Internal Records.
    - Each list is displayed as a table with relevant columns (e.g., Date, Description, Amount).
    - A summary count of items in each list is clearly visible.

### 10.5. Manually match unmatched items
- **ID**: US-105
- **Description**: As a Finance User, I want to be able to manually match an item from the unmatched bank list with a corresponding item from the unmatched internal records list to resolve discrepancies.
- **Acceptance criteria**:
    - I can select one item from the bank list.
    - I can select one item from the internal records list.
    - A "Match" button becomes active when two items are selected.
    - Clicking "Match" moves both items to the "Matched Items" list.

### 10.6. The system learns from manual matches
- **ID**: US-106
- **Description**: As a system, when a user performs a manual match, I want to learn the characteristics of that match to improve future automatic reconciliations.
- **Acceptance criteria**:
    - When a user matches two items, the system analyzes their properties (e.g., keywords in the description, date difference).
    - A new, specific matching rule is generated and stored based on this analysis.
    - This new rule is applied in subsequent reconciliation runs. 