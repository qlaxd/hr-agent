# Dokumentációs és Repository Stratégiai Terv / Documentation and Repository Strategy Plan

## 1. Dokumentációs Terv / Documentation Plan

A projekt sikeres megvalósítása és hosszútávú karbantarthatósága érdekében az alábbi dokumentumok létrehozását és karbantartását javaslom.

To ensure the successful implementation and long-term maintainability of the project, I recommend creating and maintaining the following documents.

| Dokumentum / Document                                | Cél / Purpose                                                                                                                              | Célközönség / Audience                               | Státusz / Status    |
| ---------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------- | ------------------- |
| `README.md`                                          | Magas szintű projekt bemutató, gyors telepítési útmutató, alapvető használat. / High-level project overview, quick start guide, basic usage.     | Mindenki / Everyone                                  | Frissítendő / To be updated |
| `docs/architecture.md`                               | Az eredeti, részletes architektúra terv. / The original, detailed architectural plan.                                                        | Fejlesztők, Architektek / Developers, Architects     | **Létezik / Exists** |
| `docs/shared.md`                                     | A közös, újrafelhasználható komponensek és alapelvek leírása. / Description of shared, reusable components and principles.                    | Fejlesztők, Architektek / Developers, Architects     | **Létezik / Exists** |
| `docs/not-shared.md`                                 | A projekt-specifikus (nem megosztott) komponensek részletezése. / Detailing of project-specific (non-shared) components.                      | Fejlesztők / Developers                              | **Létezik / Exists** |
| `docs/documentation-plan.md`                         | Ez a dokumentum. / This document.                                                                                                          | Tech Lead, Fejlesztők / Tech Lead, Developers        | **Létezik / Exists** |
| `docs/repository-structure.md`                       | A választott repository stratégia (monorepo) részletes leírása, fejlesztői workflow. / Detailed description of the repository strategy, dev workflow. | Fejlesztők / Developers                              | **Létezik / Exists** |
| `docs/for-devs/local-setup.md`                       | Útmutató a fejlesztői környezet beállításához. / Guide for setting up the development environment.                                         | Fejlesztők / Developers                              | Létrehozandó / To be created |
| `docs/for-users/resourcing-agent-guide.md`           | Felhasználói útmutató a Resourcing Agent-hez. / User guide for the Resourcing Agent.                                                        | Végfelhasználók (Recruiterek) / End-users (Recruiters) | Létrehozandó / To be created |
| `docs/for-users/controlling-agent-guide.md`          | Felhasználói útmutató a Controlling Agent-hez. / User guide for the Controlling Agent.                                                      | Végfelhasználók (Pénzügy) / End-users (Finance)      | Létrehozandó / To be created |

## 2. Repository Stratégia: Monorepo vs. Git Submodule / Version Control Strategy: Monorepo vs. Git Submodule

A tech lead javaslata, hogy a megosztott kódot `git submodule`-ként kezeljük, egy nagyon jó kiindulási pont a beszélgetéshez. Ez egy bevett gyakorlat, de fontos összevetni egy másik modern megközelítéssel, a monorepo-val, hogy a projekt számára legmegfelelőbbet válasszuk.

The tech lead's suggestion to manage shared code as a `git submodule` is an excellent starting point for discussion. It's a valid pattern, but it's important to compare it with another modern approach, the monorepo, to choose the best fit for the project.

### 2.1. A Git Submodule megközelítés

Ebben a modellben több különálló Git repository-nk lenne:
- `shared-components` (a közös backend, frontend és infra kód)
- `resourcing-agent` (ami submodule-ként hivatkozik a `shared-components`-re)
- `controlling-agent` (ami szintén submodule-ként hivatkozik a `shared-components`-re)

**Előnyök (Pros):**
- **Tiszta elválasztás**: A komponensek teljesen különállóak, saját verziótörténettel rendelkeznek.
- **Független verziókezelés**: A `shared-components` könyvtárnak lehet saját kiadási ciklusa.
- **Hozzáférési kontroll**: Repository szinten szabályozható, hogy ki férhet hozzá a közös komponensekhez.

**Hátrányok (Cons) - Miért lehet problémás ebben a projektben:**
- **Komplexebb Workflow**: A fejlesztői munka nehézkesebb. Egy olyan változtatás, ami a `shared` kódot és a `resourcing-agent`-et is érinti, két külön commit-ot, két külön pull request-et és bonyolultabb koordinációt igényel.
- **"Dependency Hell"**: Nehéz nyomon követni, hogy a `resourcing-agent` melyik `shared-components` commit-tal működik együtt. A submodule frissítése egy külön lépés (`git submodule update --remote`), amit a fejlesztők gyakran elfelejtenek, ami inkonzisztens állapothoz vezet.
- **CI/CD Bonyolultság**: A build pipeline-nak minden submodule-t külön kell klónoznia a megfelelő verzióra, ami hibalehetőséget rejt magában.
- **Nehezebb refaktorálás**: A kódbázisok közötti refaktorálás (pl. egy funkció áthelyezése a projekt-specifikus kódból a megosztottba) nagyon nehézkes.

### 2.2. A Monorepo megközelítés (Javasolt)

Ebben a modellben egyetlen Git repository van, amely tartalmazza az összes projektet és a megosztott kódot is, ahogyan azt a `repository-structure.md`-ben felvázoltam.

**Előnyök (Pros) - Miért ez a javasolt út:**
- **Atomikus Változtatások (Atomic Commits)**: A legfontosabb előny. **Egyetlen commitban** lehet módosítani a megosztott kódot és az azt használó összes projektet. Ez drasztikusan leegyszerűsíti a refaktorálást és az olyan feature-ök fejlesztését, amelyek a rendszer több részét érintik.
- **Egyszerűsített Függőségkezelés**: Az egész projektcsomagnak egyetlen, közös függőséglistája lehet (`requirements.txt`), elkerülve a verzióütközéseket a projektek között.
- **Egységes Kód-Tudatosság**: A fejlesztők és az IDE-k (pl. VS Code) az egész kódbázist egyszerre látják. A kódkeresés, definícióra ugrás, és a refaktorálás zökkenőmentesen működik a `shared` és a projekt-specifikus kódok között.
- **Könnyebb CI/CD**: A pipeline egyszerűbb. Egyetlen `git clone` letölt mindent. Okos pipeline-okkal (path-based filtering) megoldható, hogy csak az érintett részek tesztjei és buildjei fussanak le.

**Hátrányok (Cons) és hogyan kezeljük őket:**
- **Repository mérete**: Nagyobb lehet a repository, de a modern Git ezt hatékonyan kezeli.
- **Build idők**: Optimalizálás nélkül minden változásra lefuthat az összes build. Ezt a CI/CD pipeline-ban `path filtering`-gel könnyen lehet orvosolni.

### 2.3. Összegzés és Javaslat

**A tech lead iránymutatása az absztrakcióról és az újrafelhasználásról kulcsfontosságú.** Ezt a célt mindkét módszerrel el lehet érni. A különbség a "hogyan"-ban van.

Bár a `git submodule` logikailag tiszta szeparációt kínál, a gyakorlatban gyakran vezet egy nehézkesebb, hibákra hajlamosabb fejlesztői folyamathoz, különösen egy szorosan együttműködő csapat és szorosan kapcsolódó komponensek esetén.

**Ezért a Monorepo megközelítést javaslom.** Jobban támogatja a gyors, agilis fejlesztést, drámaian csökkenti a fejlesztési "overhead"-et, és összhangban van az olyan modern fejlesztési gyakorlatokkal, amelyeket a nagy technológiai cégek (pl. Google, Facebook) is alkalmaznak. A `repository-structure.md`-ben leírt könyvtárstruktúra pontosan egy ilyen jól szervezett monorepo-t vázol fel. 