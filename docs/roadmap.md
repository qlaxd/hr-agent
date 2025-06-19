# Fejlesztési Roadmap – HR-Kibit Agent

## 1. Áttekintés

Ez a roadmap a HR-Kibit Agent projekt főbb fejlesztési fázisait, mérföldköveit és prioritásait tartalmazza. A cél egy biztonságos, skálázható, AI-alapú belső eszköz, amely a Google Workspace-ben tárolt jelöltadatbázisra építve támogatja a toborzási folyamatokat.

---

## 2. Főbb mérföldkövek és fázisok

### 2.1. Fázis 1: Alapinfrastruktúra és backend

- **Google Cloud Platform (GCP) projekt létrehozása**
- **Service Account-ok, jogosultságok, titokkezelés (Secret Manager) beállítása**
- **Google Drive API integráció (read-only)**
- **Backend API (FastAPI) alapok, CI/CD pipeline (GitHub Actions, Docker, Cloud Run)**
- **Alapvető adatmodellek, séma (SQLAlchemy, Pydantic)**
- **Metaadat-tárolás (Firestore vagy Cloud SQL)**
- **Alapvető hitelesítés (Google OAuth 2.0, Authlib)**

### 2.2. Fázis 2: Adatfeldolgozó pipeline és tudásbázis

- **Dokumentum-feldolgozó pipeline (Cloud Functions, Apache Tika)**
- **Szövegkinyerés, chunkolás, metaadatok mentése**
- **Embedding generálás (Vertex AI Embedding API)**
- **Vektor-adatbázis (Vertex AI Vector Search) integráció**
- **Dokumentumok biztonságos tárolása (Cloud Storage)**
- **Alap audit logolás, naplózás**

### 2.3. Fázis 3: AI mag, keresés és relevancia

- **RAG pipeline (LlamaIndex, Vertex AI Gemini 1.5 Pro)**
- **Szemantikus keresés, relevancia pontszámítás**
- **PII anonimizálás (Microsoft Presidio)**
- **AI által generált összefoglalók, helpdesk Q&A**
- **Jogosultságkezelés, szerepkörök (backend oldalon)**

### 2.4. Fázis 4: Frontend és felhasználói élmény

- **SPA frontend (React + MUI, Zustand)**
- **Alapvető kereső és helpdesk UI**
- **Google OAuth 2.0 integráció a frontendben**
- **Frontend-backend integráció, API hívások**
- **Firebase Hosting beállítása**

### 2.5. Fázis 5: Biztonság, audit, compliance

- **Fenyegetésmodellezés, jogosultságok finomhangolása**
- **PII-kezelés, anonimizálás, GDPR megfelelés**
- **Audit logok, naplózás, monitoring (Stackdriver, GCP Audit Logs)**
- **Penetrációs tesztek, belső biztonsági review**

### 2.6. Fázis 6: Tesztelés, bevezetés, visszacsatolás

- **Automatizált tesztek (unit, integrációs, e2e)**
- **Felhasználói elfogadási tesztelés (UAT)**
- **Dokumentáció, oktatás, belső rollout**
- **Visszacsatolás gyűjtése, iteratív fejlesztés**

---

## 3. Prioritások és időzítés (javaslat)

| Fázis | Főbb feladatok | Időtartam (becslés) |
|-------|----------------|---------------------|
| 1     | Infrastruktúra, backend alapok | 1 hét |
| 2     | Adatfeldolgozás, tudásbázis | 1 hét |
| 3     | AI mag, keresés, relevancia | 1 hét |
| 4     | Frontend, UX | 1 hét |
| 5     | Biztonság, compliance | 0,5 hét |
| 6     | Tesztelés, rollout | 0,5 hét |

---

## 4. Kiemelt technológiák, könyvtárak, indoklás

- **GCP (Cloud Run, Functions, Firestore, Vertex AI, Secret Manager, Pub/Sub, Scheduler):** Menedzselt, biztonságos, skálázható, natív Google integráció.
- **FastAPI, Uvicorn, SQLAlchemy, Pydantic:** Modern, gyors Python backend, AI/ML ökoszisztéma.
- **React, MUI, Zustand:** Modern, gyors, jól karbantartható frontend.
- **LlamaIndex, Vertex AI Gemini, Presidio:** RAG pipeline, szemantikus keresés, PII védelem.
- **GitHub, GitHub Actions, Docker:** Forráskódkezelés, CI/CD, konténerizáció.
- **Google OAuth 2.0, Secret Manager:** Biztonságos authentikáció, titokkezelés.

---

## 5. Sikerességi kritériumok

- Gyors, pontos keresés és relevancia
- Magas biztonsági szint, PII védelem, auditálhatóság
- Felhasználóbarát, letisztult UI
- Könnyű üzemeltetés, skálázhatóság, alacsony karbantartási igény
- Pozitív felhasználói visszajelzések, gyors bevezetés

---

## 6. Iterációk és továbbfejlesztési lehetőségek

- **Haladó keresési funkciók (pl. szűrők, aggregációk)**
- **További AI modellek, finomhangolás**
- **Mobil támogatás**
- **Részletesebb admin felület, riportok**
- **Integráció további belső rendszerekkel**
