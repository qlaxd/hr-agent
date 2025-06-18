# Technológiai Stack: Erőforrás-tervező AI Ügynök

## 1. Áttekintés

Ez a dokumentum a "Resourcing AI Agent" projekt megvalósításához javasolt technológiai veremet (stack) részletezi. A kiválasztott technológiák az architektúra tervezés során felállított elveket követik: biztonság, skálázhatóság, karbantarthatóság és a Google Cloud Platform (GCP) ökoszisztémájával való szoros integráció.

## 2. Részletes Technológiai Stack

### 2.1. Frontend (Felhasználói Felület)

A cél egy egyszerű, gyorsan fejleszthető, de modern felhasználói felület, amely minimális karbantartást igényel.

- **Keretrendszer**: **React.js**
  - **Indoklás**: A React a legnépszerűbb frontend keretrendszer, hatalmas ökoszisztémával és közösségi támogatással rendelkezik. Komponens-alapú architektúrája ideális egy letisztult, egyoldalas alkalmazás (SPA) létrehozására. A választás a jövőbeli bővíthetőséget is biztosítja.
- **UI Komponens Könyvtár**: **Material-UI (MUI)**
  - **Indoklás**: A MUI egy Google által fejlesztett, React alapú komponens könyvtár, amely követi a Material Design elveit. Letisztult, professzionális megjelenést biztosít, és használata jelentősen felgyorsítja a fejlesztést, mivel előre elkészített, testreszabható komponenseket kínál (gombok, beviteli mezők, listák stb.).
- **Stílusozás**: **Styled-components / Emotion**
  - **Indoklás**: Ezek a CSS-in-JS könyvtárak a MUI alapját képezik, és lehetővé teszik a komponens-szintű, dinamikus stílusozást, ami megkönnyíti a karbantarthatóságot.
- **Állapotkezelés**: **Zustand**
  - **Indoklás**: A Redux-szal ellentétben a Zustand egy sokkal egyszerűbb, minimális boilerplate-et igénylő állapotkezelő. A projekt egyszerűsége miatt nincs szükség a Redux komplexitására; a Zustand gyors, hatékony és könnyen tanulható megoldást nyújt.
- **Csomagkezelő**: **NPM** vagy **Yarn**

### 2.2. Backend (API Réteg)

A backend felel az üzleti logika végrehajtásáért, az AI modellekkel való kommunikációért és a frontend kiszolgálásáért.

- **Keretrendszer**: **FastAPI (Python 3.11+)**
  - **Indoklás**: A FastAPI egy modern, nagy teljesítményű Python keretrendszer. Aszinkron működése kiválóan alkalmas az AI modellek esetenként hosszabb válaszidejének kezelésére. Automatikus OpenAPI (Swagger) dokumentáció generálása felgyorsítja a fejlesztést és a frontend-backend integrációt. A Python nyelv választása kulcsfontosságú az AI/ML könyvtárakkal való natív integráció miatt.
- **Szerver**: **Uvicorn**
  - **Indoklás**: A Uvicorn az iparági szabvány ASGI (Asynchronous Server Gateway Interface) szerver a FastAPI futtatásához.

### 2.3. AI Mag és Üzleti Logika (RAG Pipeline)

Ez a rendszer "agya", amely a keresést, az összefoglalást és a kérdések megválaszolását végzi.

- **LLM (Nagy Nyelvi Modell)**: **Google Vertex AI - Gemini 1.5 Pro**
  - **Indoklás**: A Gemini 1.5 Pro egy multimodális, rendkívül nagy kontextusablakkal rendelkező, viszonylag olcsó modell. Képességei tökéletesen alkalmasak a dokumentumok mélyértelmezésére, a pontos összefoglalók generálására és a komplex, kontextus-alapú kérdések megválaszolására. Mivel menedzselt szolgáltatás, nincs szükség a modell saját infrastruktúrán történő hosztolására és karbantartására.
- **Embedding Modell**: **Google Vertex AI - Text Embedding API (`text-embedding-004`)**
  - **Indoklás**: Ez a Google legújabb és legnagyobb teljesítményű embedding modellje, amely a szövegeket sűrű vektorreprezentációkká alakítja. A minőségi embeddingek elengedhetetlenek a szemantikus keresés pontosságához.
- **Orchesztrációs Keretrendszer**: **LlamaIndex**
  - **Indoklás**: A LlamaIndex egy adatközpontú keretrendszer a RAG (Retrieval-Augmented Generation) pipeline-ok építéséhez. Jelentősen leegyszerűsíti és meggyorsítja a fejlesztést azáltal, hogy kész komponenseket kínál az adatok betöltésére (data loaders), indexelésére (indexing), a dokumentumok feldarabolására (chunking), és a lekérdezési motorok (query engines) felépítésére.
- **PII Anonymization (Személyes Adatok Anonimizálása)**: **Microsoft Presidio**
  - **Indoklás**: Mielőtt a releváns szövegrészleteket elküldenénk a Gemini API-nak, minden személyes adatot (név, email, telefonszám) el kell távolítani. A Presidio egy robusztus, konfigurálható könyvtár, amely hatékonyan képes felismerni és eltávolítani a PII-t a szövegből.

### 2.4. Adatfeldolgozó Pipeline

Ez az automatizált folyamat felel a Google Drive-ban lévő dokumentumok feldolgozásáért és a tudásbázis naprakészen tartásáért.

- **Dokumentum Feldolgozás**: **Apache Tika (Python wrapper)**
  - **Indoklás**: A Tika egy Java-alapú, rendkívül sokoldalú könyvtár, amely képes szöveget kinyerni szinte bármilyen fájltípusból, beleértve a PDF, DOCX, és DOC formátumokat is. Pythonból könnyen használható a `tika-python` csomagon keresztül.
- **Infrastruktúra**: **Google Cloud Functions (2nd gen)**
  - **Indoklás**: A dokumentumok feldolgozása eseményvezérelt módon történik. Egy Cloud Function tökéletes választás, mivel csak akkor fut (és generál költséget), amikor egy új fájl feldolgozására van szükség. A 2. generációs Cloud Functions hosszabb futási időt és jobb konfigurálhatóságot biztosít.
- **Ütemezés/Trigger**: **Google Cloud Scheduler & Pub/Sub**
  - **Indoklás**: A Cloud Scheduler egy cron-szerű szolgáltatás, amely periodikusan (pl. óránként) elindíthat egy folyamatot, amely a Google Drive API segítségével új fájlokat keres. Az új fájlok eseményeit a Pub/Sub üzenetsoron keresztül küldjük a feldolgozó Cloud Function-nek, ami egy robusztus, skálázható, aszinkron architektúrát eredményez.

### 2.5. Adatbázisok és Adattárolás

- **Vektor Adatbázis**: **Vertex AI Vector Search** (korábban Matching Engine)
  - **Indoklás**: Ez egy teljesen menedzselt, nagy teljesítményű vektor adatbázis, amely rendkívül gyors és pontos hasonlósági keresést tesz lehetővé milliós nagyságrendű vektorok között is. Tökéletesen integrálódik a Vertex AI többi szolgáltatásával.
- **Metaadat Tároló**: **Google Firestore**
  - **Indoklás**: A Firestore egy NoSQL, szerver nélküli dokumentum-adatbázis, amely ideális a feldolgozott dokumentumok metaadatainak (pl. `document_id`, fájlnév, forrás link, darabolt chunk-ok azonosítói) tárolására. Skálázhatósága és rugalmas adatsémája tökéletesen illeszkedik a projekt szerver nélküli természetéhez.

### 2.6. Infrastruktúra, Hoszting és CI/CD

- **Cloud Platform**: **Google Cloud Platform (GCP)**
- **API Hoszting**: **Google Cloud Run**
  - **Indoklás**: A FastAPI alkalmazást egy Docker konténerbe csomagoljuk és Cloud Run-on futtatjuk. Ez egy szerver nélküli platform, amely automatikusan skálázódik a beérkező forgalom alapján (akár nullára is, ha nincs használatban), így költséghatékony és nem igényel szerver menedzsmentet.
- **Frontend Hoszting**: **Firebase Hosting**
  - **Indoklás**: Gyors, biztonságos és globális CDN-nel (Content Delivery Network) rendelkező statikus hoszting szolgáltatás, amely tökéletes React alkalmazásokhoz.
- **Forráskódkezelés**: **GitHub**
- **CI/CD (Folyamatos Integráció és Telepítés)**: **GitHub Actions**
  - **Indoklás**: Automatizált munkafolyamatokat hozunk létre a tesztelésre, a Docker image-ek buildelésére és a Google Cloud Run-ra, valamint a Firebase Hosting-ra történő automatikus telepítésre minden `main` branch-re történő push esetén.
- **Konténerizáció**: **Docker**

### 2.7. Authentikáció és Biztonság

- **Authentikáció**: **Google Identity Platform (OAuth 2.0)**
  - **Indoklás**: Lehetővé teszi a felhasználók számára, hogy a meglévő KIBIT Google Workspace fiókjukkal jelentkezzenek be (SSO), ami biztonságos és kényelmes.
- **Titokkezelés (Secret Management)**: **Google Secret Manager**
  - **Indoklás**: Minden szenzitív adatot, mint például API kulcsok, adatbázis jelszavak, és service account kulcsok, biztonságosan tárolunk a Secret Managerben, és futási időben, biztonságos módon adjuk át az alkalmazásnak.
- **API Hozzáférés**: **Google Drive API**
  - **Indoklás**: Egy dedikált Service Account segítségével, a legkevesebb jogosultság elvét követve (read-only), férünk hozzá a Google Drive-ban tárolt dokumentumokhoz.
