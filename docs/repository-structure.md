## Repository Struktúra és Verziókövetési Stratégia

### 1. Monorepo Megközelítés (Ajánlott)

**Indoklás:**
- Mindkét projekt sok közös infrastruktúrát használ (autentikáció, API réteg, adatbetöltő pipeline)
- Egyszerűbb karbantartás - egy helyen lehet kezelni a függőségeket
- Konzisztencia - mindkét projekt ugyanazokat a verziókat használja
- Könnyebb refaktorálás a közös kódban

### 2. Javasolt Könyvtárstruktúra

```
hr-kibit-agent/
├── shared/
│   ├── backend/
│   │   ├── api/           # Közös API komponensek
│   │   ├── core/          # Konfiguráció, adatbázis kapcsolat
│   │   └── utils/         # Google Drive API wrapper, validátorok
│   ├── frontend/
│   │   ├── components/    # Közös UI komponensek
│   │   ├── hooks/         # Közös React hooks
│   │   └── services/      # API hívások
│   └── infrastructure/    # Terraform, Docker
├── resourcing-agent/
│   ├── backend/
│   │   ├── services/      # RAG specifikus szolgáltatások
│   │   ├── models/        # Jelölt, keresési modellek
│   │   └── api/           # Keresési, helpdesk végpontok
│   ├── frontend/          # Resourcing specifikus UI
│   └── tests/
├── controlling-agent/
│   ├── backend/
│   │   ├── services/      # Egyeztető motor, párosító algoritmus
│   │   ├── models/        # Tranzakció, egyeztetési modellek
│   │   └── api/           # Egyeztetési, tanulási végpontok
│   ├── frontend/          # Controlling specifikus UI
│   └── tests/
├── docs/                  # Dokumentáció
└── scripts/               # Deployment, setup scriptek

ez csak egy nagyvonalakban ábrázolt monorepo structure a projekthez
```

### 3. Branch Stratégia

```
main (production)
├── develop (integration)
│   ├── feature/resourcing-search
│   ├── feature/controlling-reconciliation
│   ├── feature/shared-auth
│   └── hotfix/security-patch
└── release/v1.0.0
```

### 4. Commit Konvenciók

```
<type>(<scope>): <description>

feat(resourcing): add candidate search functionality
fix(shared): resolve authentication token refresh issue
docs(controlling): update reconciliation algorithm documentation
```

**Scope példák:**
- `shared`: Közös komponens
- `resourcing`: Resourcing Agent specifikus
- `controlling`: Controlling Agent specifikus
- `infra`: Infrastruktúra

### 5. Függőség Kezelés

**Python (requirements.txt):**
```
# Shared dependencies
fastapi==0.104.1
pydantic==2.5.0
google-auth==2.23.4

# Resourcing Agent specific
vertexai==1.38.0
chromadb==0.4.18

# Controlling Agent specific
pandas==2.1.4
openpyxl==3.1.2
```

### 6. Fejlesztési Workflow

1. **Branch létrehozás:**
   ```bash
   git checkout develop
   git pull origin develop
   git checkout -b feature/your-feature-name
   ```

2. **Fejlesztés:** Kód írása a megfelelő projekt mappában, közös komponensek használata

3. **Commit és Push:**
   ```bash
   git add .
   git commit -m "feat(scope): description"
   git push origin feature/your-feature-name
   ```

4. **Pull Request:** PR létrehozása `develop` branch-re, code review, CI/CD

### 7. Deployment Stratégia

- **Development:** Lokális fejlesztés
- **Staging:** Tesztelés és validáció  
- **Production:** Éles környezet

### 8. Alternatív Megoldás

Ha a monorepo túl komplexnek bizonyul:
- **Shared Library Repository:** Külön repo a közös komponenseknek
- **Project-Specific Repositories:** Mindkét projekt saját repo-ja

Ez a struktúra biztosítja a hatékony fejlesztést, a kód újrafelhasználást és a könnyű karbantarthatóságot mindkét projekt számára.