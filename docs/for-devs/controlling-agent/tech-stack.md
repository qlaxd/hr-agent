# Technológiai Stack: Controlling AI Agent

## 1. Áttekintés

Ez a dokumentum a "Controlling AI Agent" projekt megvalósításához javasolt technológiai veremet (stack) részletezi. A választás a `shared.md`-ben és a projekt-specifikus `architecture.md`-ben lefektetett elveken alapul, kiemelt hangsúlyt fektetve a pénzügyi adatok biztonságos kezelésére, a folyamatok auditálhatóságára és a skálázhatóságra.

## 2. Részletes Technológiai Stack

### 2.1. Frontend (Felhasználói Felület)

A cél egy adat-intenzív, de letisztult és könnyen kezelhető felület, ahol a pénzügyi szakemberek hatékonyan tudják áttekinteni az egyeztetés eredményeit és elvégezni a szükséges manuális párosításokat.

- **Keretrendszer**: **React.js**
  - **Indoklás**: A `resourcing-agent`-tel való konzisztencia és a széleskörű közösségi támogatás miatt ideális választás. Komponens-alapú felépítése lehetővé teszi a komplex táblázatok és adatbeviteli űrlapok hatékony kezelését.
- **UI Komponens Könyvtár**: **Material-UI (MUI)**
  - **Indoklás**: Az MUI kiválóan alkalmas adat-sűrű alkalmazásokhoz. Professzionális, testreszabható komponenseket (Data Grid, gombok, listák) kínál, amelyek elengedhetetlenek a tranzakciós listák megjelenítéséhez és a manuális párosítási felület kialakításához.
- **Állapotkezelés**: **Zustand**
  - **Indoklás**: Az alkalmazás állapotkezelési igényei (pl. kijelölt tranzakciók, szűrők állapota) nem indokolják a Redux komplexitását. A Zustand egy egyszerű, gyors és könnyen tanulható alternatíva.
- **Csomagkezelő**: **NPM** vagy **Yarn**

### 2.2. Backend (API Réteg)

A backend felel az adatfeldolgozásért, az egyeztető motor futtatásáért és a tanuló modul logikájának kezeléséért.

- **Keretrendszer**: **FastAPI (Python 3.11+)**
  - **Indoklás**: A Python az adatfeldolgozás de-facto szabványa, a FastAPI pedig modern, aszinkron képességeket biztosít. Ez kritikus az egyeztetési folyamat aszinkron, nem-blokkoló futtatásához, ahogy azt a PRD is előírja. Az automatikus Swagger UI (OpenAPI) dokumentáció felgyorsítja a fejlesztést.
- **Szerver**: **Uvicorn**
  - **Indoklás**: Ipari szabvány ASGI szerver a FastAPI-alapú alkalmazások futtatásához.

### 2.3. Egyeztető Motor és Üzleti Logika

Ez a rendszer központi logikai egysége, amely a pénzügyi adatok párosítását végzi.

- **Adatfeldolgozás és Normalizálás**: **Pandas** & **Openpyxl**
  - **Indoklás**: A bemeneti adatok (`.csv`, `.xlsx`) feldolgozása a controlling agent alapkövetelménye. A Pandas egy rendkívül hatékony eszköz a táblázatos adatok beolvasására, tisztítására és egységes, normalizált formátumra hozására. Az `openpyxl` könyvtár a `.xlsx` fájlok kezeléséhez szükséges.
- **Szabályalapú Párosítás (Fuzzy Matching)**: **FuzzyWuzzy**
  - **Indoklás**: Az egyeztetés során gyakoriak az elgépelések vagy kisebb eltérések a közleményekben. A `FuzzyWuzzy` lehetővé teszi a "laza" (fuzzy) szövegegyezésen alapuló szabályok implementálását, ami növeli az automatikus párosítások pontosságát.
- **Adatbázis ORM**: **SQLAlchemy**
  - **Indoklás**: Az SQLAlchemy egy objektum-relációs leképező (ORM), amely elabstrahálja az SQL-adatbázis kezelését. Lehetővé teszi a Python-objektumok és az adatbázis-táblák közötti tiszta leképezést, valamint biztosítja a tranzakciók biztonságos, konzisztens kezelését (ACID), ami pénzügyi adatoknál elengedhetetlen.
- **Adatvalidáció**: **Pydantic**
  - **Indoklás**: A FastAPI-ba integrált Pydantic garantálja, hogy az API végpontokra érkező és onnan távozó adatok megfelelnek az előre definiált sémáknak. Ez csökkenti a hibák lehetőségét és növeli a rendszer robusztusságát.

### 2.4. Adatfeldolgozó Pipeline

Ez az automatizált folyamat felel a Google Drive-ban lévő pénzügyi dokumentumok feldolgozásáért és az adatbázisba való betöltéséért.

- **Infrastruktúra**: **Google Cloud Functions (2nd gen)**
  - **Indoklás**: Az adatbetöltés eseményvezérelt. Egy Cloud Function ideális erre a feladatra, mivel csak akkor fut (és generál költséget), amikor egy új fájl feldolgozására van szükség.
- **Trigger**: **Google Drive API & Google Cloud Storage Triggerek**
  - **Indoklás**: A közös architektúrának megfelelően egy folyamat figyeli a megadott Google Drive mappát. Új fájl esetén a fájl átkerül egy Cloud Storage bucket-be, amelynek a változása triggereli a `pandas` alapú feldolgozást végző Cloud Function-t.

### 2.5. Adatbázisok és Adattárolás

- **Tranzakciós Adatbázis**: **Google Cloud SQL (PostgreSQL)**
  - **Indoklás**: A pénzügyi tranzakciók, egyeztetési futamok, párosítások és tanult szabályok tárolására egy relációs adatbázis a legmegfelelőbb a szigorú adatintegritás (pl. külső kulcsok) és a tranzakciós garanciák miatt. A PostgreSQL egy megbízható, robusztus választás.
- **Fájl Tárolás**: **Google Cloud Storage**
  - **Indoklás**: Az eredeti, feltöltött fájlok (bankszámlakivonatok, főkönyvek) tárolására szolgál archiválási és auditálási célokból.

### 2.6. Infrastruktúra, Hoszting és CI/CD

Ez a réteg konzisztens a `resourcing-agent`-tel a `shared.md`-ben leírt közös alapelvek miatt.

- **Cloud Platform**: **Google Cloud Platform (GCP)**
- **API Hoszting**: **Google Cloud Run**
  - **Indoklás**: A FastAPI alkalmazás Docker konténerben fut, amit a Cloud Run menedzsel. Szerver nélküli, skálázódó és költséghatékony megoldás.
- **Frontend Hoszting**: **Firebase Hosting**
  - **Indoklás**: Gyors, biztonságos, globális CDN-t biztosító statikus hoszting React alkalmazásokhoz.
- **Forráskódkezelés**: **GitHub**
- **CI/CD**: **GitHub Actions**
  - **Indoklás**: Automatizált munkafolyamatok a kód tesztelésére, Docker image-ek építésére és a GCP szolgáltatásokra (Cloud Run, Firebase) történő telepítésre.
- **Konténerizáció**: **Docker**

### 2.7. Authentikáció és Biztonság

- **Authentikáció**: **Google Identity Platform (OAuth 2.0)**
  - **Indoklás**: Biztonságos és kényelmes SSO (Single Sign-On) a meglévő KIBIT Google Workspace fiókokkal.
- **Titokkezelés**: **Google Secret Manager**
  - **Indoklás**: API kulcsok, adatbázis jelszavak és egyéb szenzitív adatok biztonságos tárolása.
- **API Hozzáférés**: **Google Drive API**
  - **Indoklás**: Dedikált Service Account, read-only jogosultsággal a forrásdokumentumok biztonságos eléréséhez. 