# Biztonsági dokumentáció (Security Plan)

## 1. Miért fontos?
A Resourcing AI Agent rendszer érzékeny, személyes adatokat (PII) kezel, ezért a biztonság, adatvédelem és megfelelőség elsődleges tervezési szempont. A biztonsági terv célja, hogy részletesen bemutassa a fenyegetésmodellt, jogosultságkezelést, PII-kezelést és auditálást, valamint az ezekhez szükséges technológiákat, frameworköket és könyvtárakat.

---

## 2. Fenyegetésmodell

A rendszer főbb fenyegetései:
- **Adatszivárgás**: PII vagy bizalmas adatok illetéktelen kezekbe kerülése.
- **Jogosulatlan hozzáférés**: Nem megfelelően hitelesített vagy jogosulatlan felhasználók hozzáférése az adatokhoz.
- **Külső szolgáltatások kockázata**: LLM API-k, embedding szolgáltatások használata során PII kiszivárgásának veszélye.
- **Adatmódosítás vagy törlés**: Jogosulatlan adatmanipuláció.
- **Szolgáltatásmegtagadás (DoS)**: Rendszer túlterhelése.

### Fenyegetésmodellezési eszközök, ajánlott gyakorlatok:
- **STRIDE** modell alkalmazása (Spoofing, Tampering, Repudiation, Information Disclosure, Denial of Service, Elevation of Privilege)
- **OWASP Top 10** figyelembevétele

---

## 3. Jogosultságok, szerepkörök

### Technológiák, frameworkök:
- **Google OAuth 2.0**: Felhasználói hitelesítés, SSO, token-alapú hozzáférés.
  - *Miért?* Biztonságos, iparági szabvány, natív Google Workspace integráció.
- **FastAPI Security**: Backend oldali jogosultság-ellenőrzés, role-based access control (RBAC).
  - *Miért?* Gyors, aszinkron, jól integrálható Python ökoszisztémába.
- **Google Identity Platform**: Jogosultságkezelés, SSO, audit trail.
- **Principle of Least Privilege**: Minden szolgáltatásfiók csak a minimálisan szükséges jogokat kapja (pl. Google Drive olvasás).

### Szerepkörök:
- **Talent Tanácsadó**: Teljes funkcionalitás, csak olvasási jog az adatokhoz.
- **Rendszeradminisztrátor**: Rendszerkonfiguráció, audit, nincs hozzáférés a toborzási adatokhoz.

---

## 4. PII anonimizálás folyamata

### Technológiák, könyvtárak:
- **Custom Anonymization Service** (Python):
  - *Miért?* Személyre szabott PII felismerés és helyettesítés (pl. [CANDIDATE_NAME], [EMAIL]).
  - **spaCy** vagy **Presidio**: PII entitásfelismerés (NER), szabályalapú és ML-alapú azonosítás.
  - **Cache** (pl. Redis): Átmeneti PII tárolás a visszaanonimizáláshoz.
- **LLM API-k (Vertex AI Gemini)**: Csak anonimizált szöveget kapnak, így a PII nem kerül ki a kontrollált környezetből.

### Folyamat:
1. Dokumentum feldolgozáskor a PII-t felismerjük (NER, regex, szabályok).
2. Helyettesítjük placeholderrel, az eredetit átmenetileg cache-eljük.
3. Az LLM csak az anonimizált szöveget látja.
4. A válaszban visszahelyettesítjük az eredeti PII-t.

---

## 5. Audit logok, naplózás

### Technológiák, könyvtárak:
- **Google Cloud Audit Logs**: Minden API-hívás, hozzáférési kísérlet naplózása.
- **FastAPI Middleware**: Egyedi naplózás, request/response logolás, hibák, sikeres/sikertelen bejelentkezések.
- **Google Cloud Logging**: Központi naplógyűjtés, kereshető, riasztható.
- **GDPR megfelelőség**: Naplómegőrzési idő, hozzáférés kontrollja.

---

## 6. Titkosítás
- **TLS 1.2+**: Minden adatátvitel titkosítva (HTTPS, gRPC).
- **GCP Encryption at Rest**: Minden adat (Cloud SQL, Firestore, Vector Search, Storage) automatikusan titkosítva.
- **Secret Manager**: Jelszavak, API-kulcsok, titkos adatok biztonságos tárolása.

---

## 7. További ajánlott biztonsági technológiák
- **Firewall (GCP, VPC)**: Hozzáférés korlátozása csak szükséges IP-kre.
- **IAM (Identity & Access Management)**: Finomhangolt jogosultságkezelés minden GCP komponenshez.
- **Dependency Scanning**: pl. Snyk, Dependabot – könyvtárak sérülékenységeinek automatikus figyelése.
- **Automatizált tesztelés**: pl. pytest, pytest-security – biztonsági tesztek a CI pipeline-ban.

---

## 8. Összefoglalás
A Resourcing AI Agent biztonsági architektúrája a legmodernebb cloud-native és AI-biztonsági gyakorlatokat követi. A fent részletezett frameworkök, technológiák és könyvtárak mind a PII-védelem, auditálhatóság, jogosultságkezelés és megfelelőség szolgálatában állnak. A rendszer minden komponense a legkisebb jogosultság elvét, titkosítást és naplózást alkalmaz, hogy megfeleljen a GDPR-nak és a KIBIT belső adatvédelmi irányelveinek.
