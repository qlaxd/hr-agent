# Fejlesztési Roadmap (Tech Lead iránymutatása alapján)

## 1. Áttekintés

Ez a dokumentum a **Resourcing AI Agent** és a **Controlling AI Agent** projektek fejlesztési ütemtervét vázolja fel. A terv a tech lead javaslatai alapján készült, az alábbi fő alapelvek mentén:
1.  **Funkcionalitás Elsőként**: A fő hangsúly a működőképes üzleti logikán van.
2.  **CLI-First Megközelítés**: A felhasználói felület (UI) fejlesztése előtt egy parancssori interfészen (CLI) keresztül tesszük tesztelhetővé és akár korai fázisban bemutathatóvá a funkciókat.
3.  **Fázisolt Fejlesztés**: Először a közös dokumentumkezelési alapokat, majd az AI-magot, és csak a végén a felhasználói felületet készítjük el.

## 2. Részletes Fázisok és Feladatok

---

### **1. Fázis: Alapok és Közös Dokumentumkezelés (CLI)**
**Cél:** Egy stabil alap létrehozása a monorepo-ban, és egy CLI alkalmazás, amely képes beolvasni, feldolgozni és alapvető metaadatokkal ellátni a dokumentumokat mindkét projekt számára.
**Becsült időtartam: ~3 hét (14 nap)**

| # | Feladat | Specifikáció | Becsült Idő | Függőség | Deliverable |
|---|---|---|---|---|---|
| 1.1 | **Monorepo & Környezet Setup** | A `repository-structure.md` alapján a könyvtárstruktúra létrehozása. `pyproject.toml` és alap `requirements.txt` beállítása. | 2 nap | - | Verziókövetett, strukturált repository. |
| 1.2 | **Közös Backend Core (`shared/backend`)** | FastAPI alapok, központi konfiguráció (`pydantic-settings`), logging, alapvető hibakezelés. | 3 nap | 1.1 | Működő, de üres FastAPI alkalmazás. |
| 1.3 | **Google Drive Integráció (`shared/utils`)** | Biztonságos wrapper a Google Drive API-hoz (Service Account alapú), ami képes fájlokat listázni és letölteni. | 2 nap | 1.2 | Függvény, ami letölt egy fájlt a Drive-ról. |
| 1.4 | **Közös Adatbázis & Modellek** | Cloud SQL (PostgreSQL) kapcsolat beállítása (`SQLAlchemy`). Közös `FileMetadata` modell létrehozása. | 2 nap | 1.2 | Adatbázis séma és kapcsolat. |
| 1.5 | **Resourcing Agent Parser** | Dokumentumfeldolgozó, ami a `.pdf`, `.docx` fájlokból kinyeri a nyers szöveget (`python-docx`, `pypdf`) és elmenti a metaadatokat az adatbázisba. | 2 nap | 1.3, 1.4 | Script, ami feldolgoz egy CV-t. |
| 1.6 | **Controlling Agent Parser** | Dokumentumfeldolgozó, ami a `.csv`, `.xlsx` fájlokból kinyeri a táblázatos adatokat (`pandas`) és elmenti a metaadatokat az adatbázisba. | 2 nap | 1.3, 1.4 | Script, ami feldolgoz egy kivonatot. |
| 1.7 | **CLI Interfész Létrehozása** | `Typer` vagy `Click` alapú CLI, ami lehetővé teszi a parserek futtatását (pl. `python main.py process-resourcing-files`). | 1 nap | 1.5, 1.6 | Működő CLI eszköz. |

---

### **2. Fázis: AI Mag Fejlesztés (CLI)**
**Cél:** A két projekt specifikus "agyának" megépítése. A funkciók továbbra is CLI-n keresztül tesztelhetők.
**Becsült időtartam: ~2.5 hét (12 nap)**

| # | Feladat | Specifikáció | Becsült Idő | Függőség | Deliverable |
|---|---|---|---|---|---|
| 2.1 | **Resourcing: Embedding & Vektortárolás** | A szöveg-chunkok beágyazása (Vertex AI Embedding API). A vektorok tárolása egy vektoradatbázisban (pl. `ChromaDB` a gyors helyi prototipizáláshoz). | 3 nap | 1.5 | Vektorokkal feltöltött adatbázis. |
| 2.2 | **Resourcing: Szemantikus Keresés (RAG)** | Alap RAG pipeline `LlamaIndex`-szel. A CLI kap egy `search` parancsot, ami egy job profile-ra visszaadja a top 3 releváns dokumentumot. | 3 nap | 2.1 | CLI-n működő jelöltkeresés. |
| 2.3 | **Resourcing: PII Anonimizálás** | A releváns szövegrészek anonimizálása (pl. `presidio`) az LLM hívás előtt. | 1 nap | 2.2 | Anonimizált szöveget használó pipeline. |
| 2.4 | **Controlling: Egyeztető Motor** | Az alap párosító algoritmus megírása (szigorú szabályok: pontos összeg, dátum egyezés). | 3 nap | 1.6 | Script, ami két fájl alapján megtalálja a 100%-os egyezéseket. |
| 2.5 | **Controlling: Tanuló Modul Alapok** | Adatbázis séma a manuális párosítási szabályok tárolására. Logika, ami a manuális párosításból egyszerű szabályt generál. | 2 nap | 2.4 | Adatbázis-alapú szabálytárolás. |

---

### **3. Fázis: API és Felhasználói Felület**
**Cél:** A CLI-n validált funkciók elérhetővé tétele egy egyszerű, de használható webes felületen keresztül.
**Becsült időtartam: ~3 hét (13 nap)**

| # | Feladat | Specifikáció | Becsült Idő | Függőség | Deliverable |
|---|---|---|---|---|---|
| 3.1 | **Hitelesítés & API Végpontok** | Google OAuth 2.0 beállítása a FastAPI backend-en. Védett API végpontok létrehozása a kereséshez és egyeztetéshez. | 3 nap | Fázis 2 | Védett REST API. |
| 3.2 | **Frontend Core (`shared/frontend`)** | Alap React (vagy Vue) projekt beállítása, komponens könyvtár (MUI) integrálása, Google bejelentkezés implementálása. | 2 nap | 3.1 | Bejelentkezési képernyő. |
| 3.3 | **Resourcing Agent UI** | Kereső felület, ahol a recruiter beírhatja a keresést, és egy lista nézetben megjelennek az eredmények (jelölt neve, scoring, rövid indoklás). | 3 nap | 3.2 | Működő jelöltkereső UI. |
| 3.4 | **Controlling Agent UI** | Fájlfeltöltő felület. Háromoszlopos nézet (Párosított, Nem párosított banki, Nem párosított belső), ahol a manuális párosítás elvégezhető. | 4 nap | 3.2 | Működő egyeztető UI. |
| 3.5 | **Deployment Előkészítés** | `Dockerfile`-ok véglegesítése. Alap CI/CD pipeline (GitHub Actions), ami buildel és tesztel. | 1 nap | Mind | Automatizált build. |

---

### **4. Fázis: Tesztelés, Finomhangolás és Bevezetés**
**Cél:** A rendszer stabilizálása, felhasználói visszajelzések becsatornázása és a hivatalos belső bevezetés.
**Becsült időtartam: Folyamatos**

| # | Feladat | Specifikáció | Becsült Idő | Függőség | Deliverable |
|---|---|---|---|---|---|
| 4.1 | **Unit & Integrációs Tesztek** | A kritikus logikai részek (parserek, AI mag) lefedése automatizált tesztekkel. | Folyamatos | - | Magasabb kódminőség. |
| 4.2 | **Felhasználói Tesztelés (UAT)** | A pénzügyi és recruiter kollégák bevonása, visszajelzések gyűjtése. | Folyamatos | Fázis 3 | Visszajelzések listája. |
| 4.3 | **Dokumentáció & Rollout** | Felhasználói útmutatók elkészítése. Belső oktatás és a rendszer élesítése. | Folyamatos | - | Elégedett felhasználók. |
