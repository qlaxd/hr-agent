# Software Design Document (SDD)

## 1. Bevezetés
Ez a dokumentum részletesen bemutatja a HR-Kibit Agent (Resourcing AI Agent) szoftver architektúráját, komponenseit, adatfolyamait, interfészeit, valamint a választott technológiák és könyvtárak indoklását. A cél egy biztonságos, skálázható, könnyen karbantartható, cloud-native AI-alapú rendszer, amely a toborzási folyamatokat támogatja.

---

## 2. Főbb technológiák, frameworkök és könyvtárak

### 2.1. Frontend
- **React.js**: Modern, komponens-alapú SPA fejlesztéshez. Gyors fejlesztés, nagy ökoszisztéma, könnyű bővíthetőség.
- **Material-UI (MUI)**: React-hez készült UI komponens könyvtár, gyors, egységes, reszponzív felület.
- **Zustand**: Egyszerű, minimális boilerplate állapotkezelő, ideális a projekt egyszerűségéhez.
- **Styled-components / Emotion**: Komponens-szintű, dinamikus stílusozás, MUI alapja.
- **Axios**: HTTP kliens az API hívásokhoz.

### 2.2. Backend
- **FastAPI (Python 3.11+)**: Modern, aszinkron REST API framework, automatikus OpenAPI dokumentációval, gyors fejlesztés, natív async támogatás.
- **Uvicorn**: ASGI szerver FastAPI-hoz.
- **Pydantic**: Adatmodellezés, validáció, típusbiztonság.
- **Gunicorn**: (opcionális) Több worker támogatás, production deploymenthez.
- **Python könyvtárak**: requests, httpx, loguru (logolás), pytest (tesztelés), python-dotenv (környezeti változók kezelése).

### 2.3. AI & RAG Pipeline
- **Google Vertex AI - Gemini 1.5 Pro**: LLM, összefoglalók, Q&A, relevancia pontszám generálás.
- **Google Vertex AI - Text Embedding API**: Szövegek embeddinggé alakítása, szemantikus kereséshez.
- **LlamaIndex**: RAG pipeline orchesztráció, dokumentum betöltés, chunkolás, indexelés, query engine.
- **Microsoft Presidio**: PII anonimizálás, személyes adatok eltávolítása a szövegből.

### 2.4. Adatfeldolgozás
- **Apache Tika (tika-python)**: Dokumentumokból (PDF, DOCX, DOC) szöveg kinyerése.
- **Google Cloud Functions (2nd gen)**: Eseményvezérelt, skálázható feldolgozás.
- **Google Cloud Scheduler & Pub/Sub**: Ütemezett pipeline triggerelés, aszinkron feldolgozás.

### 2.5. Adatbázisok, adattárolás
- **Vertex AI Vector Search**: Vektor-adatbázis, gyors szemantikus keresés.
- **Google Firestore**: NoSQL metaadat-tároló, dokumentum metaadatok, chunk-azonosítók.

### 2.6. Infrastruktúra, CI/CD
- **Google Cloud Platform (GCP)**: Menedzselt szolgáltatások, skálázhatóság, biztonság.
- **Google Cloud Run**: Backend API hoszting, automatikus skálázás, konténerizáció.
- **Firebase Hosting**: Frontend hoszting, gyors CDN.
- **GitHub, GitHub Actions**: Forráskódkezelés, CI/CD pipeline, automatikus tesztelés és deployment.
- **Docker**: Konténerizáció, könnyű deployment.

### 2.7. Authentikáció, biztonság
- **Google Identity Platform (OAuth 2.0)**: SSO, biztonságos bejelentkezés.
- **Google Secret Manager**: Titokkezelés, API kulcsok, jelszavak biztonságos tárolása.
- **Google Drive API**: Dokumentumok elérése, olvasási jogosultság.

---

## 3. Rendszer komponensek és modulok

### 3.1. Frontend SPA
- **Felhasználói felület**: React + MUI komponensek, két fő funkció: "Jelöltek keresése" és "Helpdesk Q&A".
- **Állapotkezelés**: Zustand, a keresési eredmények, felhasználói adatok tárolására.
- **API kommunikáció**: Axios, minden adat backend API-n keresztül érkezik.
- **Hitelesítés**: Google SSO, token kezelése frontend oldalon.

### 3.2. Backend API (FastAPI)
- **REST végpontok**:
  - `/search-candidates` (POST): Munkaköri leírás alapján releváns jelöltek keresése.
  - `/helpdesk-query` (POST): Természetes nyelvű kérdések kezelése.
  - `/login` (GET/POST): Auth flow, token validáció.
    - **(részletes dokumentáció majd az `api-specs.md`-ben)**
- **Adatmodellek**: Pydantic modellek, típusbiztos request/response.
- **Hibakezelés**: HTTP státuszkódok, részletes hibaüzenetek, logolás (loguru).
- **Integrációk**: Google Drive API, Vertex AI, Firestore, Vector Search, Presidio.

### 3.3. Adatfeldolgozó pipeline
- **Cloud Function**: Új/Friss dokumentum detektálása, szövegkinyerés (Tika), metaadat mentés (Firestore), embedding generálás (Vertex AI), vektor-adatbázis frissítés.
- **Scheduler & Pub/Sub**: Időzített futtatás, események továbbítása.

### 3.4. AI mag (RAG pipeline)
- **LlamaIndex**: Dokumentum chunkolás, indexelés, query engine.
- **Gemini 1.5 Pro**: Összefoglalók, Q&A, relevancia pontszám.
- **Presidio**: PII anonimizálás minden LLM hívás előtt.

### 3.5. Adatbázisok
- **Firestore**: Dokumentum metaadatok, chunk-azonosítók.
- **Vertex AI Vector Search**: Embeddingek, gyors hasonlósági keresés.

---

## 4. Adatfolyamok, sequence diagramok

### 4.1. Jelölt keresés folyamata
1. Felhasználó bejelentkezik (Google SSO)
2. Munkaköri leírást beküld a frontendről
3. Backend API fogadja, validálja, továbbítja a RAG pipeline-nak
4. Dokumentumok embeddingjei alapján releváns jelöltek keresése (Vector Search)
5. Gemini generál összefoglalót, pontszámot
6. Eredmények vissza a frontendnek

### 4.2. Dokumentum feldolgozás pipeline
1. Cloud Scheduler triggereli a pipeline-t
2. Cloud Function detektálja az új/updated dokumentumokat (Google Drive API)
3. Tika kinyeri a szöveget
4. Metaadat mentés Firestore-ba
5. Embedding generálás (Vertex AI)
6. Embedding mentés Vector Search-be

---

## 5. Hibakezelési stratégia
- **Frontend**: Felhasználóbarát hibaüzenetek, toastok, API hibák kezelése.
- **Backend**: Részletes logolás, HTTP státuszkódok, validációs hibák, try/except blokkok, automatikus alerting (GCP monitoring).
- **Pipeline**: Események naplózása, hibás dokumentumok külön queue-ba kerülnek, manuális review lehetősége.

---

## 6. Külső API-k, integrációk
- **Google Drive API**: Dokumentumok elérése, csak olvasási jog.
- **Vertex AI**: LLM, embedding szolgáltatások.
- **Firestore**: Metaadatok tárolása.
- **Vector Search**: Szemantikus keresés.
- **Presidio**: PII anonimizálás.
- **Firebase Hosting**: Frontend hoszting.
- **Google Identity Platform**: Authentikáció.

---

## 7. Kódolási konvenciók, stílusguide
- **Frontend**: Airbnb React/JSX Style Guide, Prettier, ESLint.
- **Backend**: PEP8, Black, isort, mypy, flake8.
- **CI/CD**: Automatikus linting, tesztelés minden push-ra.

---

## 8. Glosszárium
- **RAG**: Retrieval-Augmented Generation, keresés-alapú AI válaszgenerálás
- **LLM**: Large Language Model
- **Embedding**: Szövegek vektorreprezentációja
- **PII**: Personally Identifiable Information, személyes azonosításra alkalmas adat
- **SPA**: Single Page Application
- **SSO**: Single Sign-On
- **CI/CD**: Continuous Integration / Continuous Deployment

---
