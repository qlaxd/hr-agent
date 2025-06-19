# Resourcing AI Agent (HR-Kibit Agent)

## Áttekintés

A Resourcing AI Agent egy belső, mesterséges intelligencia alapú alkalmazás, amely a KIBIT Solutions Talent Tanácsadó csapatát támogatja a toborzási folyamatokban. A rendszer a Google Workspace-ben tárolt jelöltadatbázist használja, és fejlett AI/ML technológiákkal segíti a releváns jelöltek gyors megtalálását, értékelését és a toborzók munkáját.

## Fő funkciók

- **Jelölt-keresés és illesztés:** Munkaköri leírás alapján releváns jelöltek keresése, AI által generált összefoglalóval és pontszámmal.
- **Helpdesk Q&A:** Természetes nyelvű kérdések a jelöltadatbázisról.
- **Biztonságos, role-based hozzáférés:** Google SSO, jogosultságkezelés, PII-védelem.
- **Automatizált dokumentum-feldolgozás:** Google Drive integráció, szövegkinyerés, metaadat-kezelés, embedding pipeline.
- **Modern, reszponzív webes felület:** React + MUI alapokon.

## Architektúra

- **Cloud-native, modularis felépítés** (GCP ajánlott)
- **Retrieval-Augmented Generation (RAG) pipeline:** LLM + vektor-alapú keresés
- **Backend:** FastAPI (Python), GCP Cloud Run
- **Frontend:** React.js, Material-UI, Firebase Hosting
- **Adattárolás:** Firestore, Vertex AI Vector Search, Google Cloud Storage
- **Biztonság:** Google OAuth 2.0, IAM, titkosítás, audit logok

## Fő technológiák

- **Frontend:** React, Material-UI, Zustand, Axios, Firebase Hosting
- **Backend:** FastAPI, Uvicorn, Pydantic, SQLAlchemy, Gunicorn, Google Cloud Libraries
- **AI & RAG:** Vertex AI Gemini, Vertex AI Embedding, LlamaIndex, Presidio
- **Adatfeldolgozás:** Apache Tika, Google Cloud Functions, Cloud Scheduler, Pub/Sub
- **Adattárolás:** Firestore, Vertex AI Vector Search, Cloud SQL, Google Cloud Storage
- **Biztonság:** Google Identity Platform, Secret Manager, FastAPI Security, audit logok

## Fejlesztés & futtatás

**Követelmények:**

- Python 3.11+
- Node.js (frontendhez)
- GCP fiók és jogosultságok

**Telepítés:**

Klónozd a repót:
```bash
git clone <repo-url>
```

**Backend:**
```bash
cd backend && pip install -r requirements.txt
```

**Frontend:**
```bash
cd frontend && npm install
```

**Futtatás:**

**Backend:**
```bash
uvicorn main:app --reload
```

**Frontend:**
```bash
npm start
```

**Deployment:**

- Backend: GCP Cloud Run (Docker támogatott)
- Frontend: Firebase Hosting

## Dokumentáció

- `architecture.md` – Részletes architektúra
- `data-models.md` – Adatmodellek, ERD, séma
- `security.md` – Biztonsági terv, PII-kezelés
- `roadmap.md` – Fejlesztési ütemterv
- `software-design.md` – Szoftverterv, komponensek

## Biztonság & megfelelőség

- GDPR-kompatibilis adatkezelés
- PII anonimizálás minden AI/LLM hívás előtt
- Audit logok, titkosítás, role-based access

## Kapcsolat

Kérdés vagy hibajelentés esetén keresd a KIBIT Solutions fejlesztői csapatát.