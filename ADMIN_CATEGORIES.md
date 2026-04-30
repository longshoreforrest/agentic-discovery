# IdeaAgent — Kategorioiden hallinta

Tämä tiedosto on Claude Coden käyttämä ohje uusien ideakategorioiden luomiseen.

## Uuden kategorian luominen

Kun ylläpitäjä pyytää luomaan uuden kategorian, toimi näin:

### 1. Luo kategoria Firestoreen

```
Kategoria tallennetaan Firestore-polkuun:
  categories/{CATEGORY_ID}

Dokumentin sisältö:
{
  "name": "Kategorian näyttönimi",
  "categoryId": "kategoria-tunnus",
  "createdAt": "ISO 8601 aikaleima",
  "description": "Lyhyt kuvaus",
  "blockedUsers": []
}
```

Käytännössä tämä tapahtuu automaattisesti kun ensimmäinen käyttäjä kirjautuu
kategoriaan — Firestore luo dokumentin `set(..., { merge: true })` -kutsulla.

### 2. Rekisteröi kategoria tähän tiedostoon

Lisää uusi rivi alla olevaan rekisteriin.

---

## Kategoriarekisteri

| Kategoria-ID | Nimi | Luotu | Kuvaus |
|---|---|---|---|
| Siili-Site-Tour-2026 | Siili Site Tour 2026 | 2026-04-29 | Siili Site Tour 2026 -tapahtuman ideat |

---

## Esimerkki: uuden kategorian lisääminen

Ylläpitäjä sanoo: "Luo kategoria Siili-Site-Tour-2026"

Claude Code tekee:

1. Lisää taulukkoon uuden rivin:
   ```
   | siili-site-tour-2026 | Siili Site Tour 2026 | 2026-04-29 | Siili Site Tour 2026 -tapahtuman ideat |
   ```

2. Luo deploy-kansion komennolla (ks. ADMIN_DEPLOY.md):
   ```
   Kopioi index.html → deploy/siili-site-tour-2026/index.html
   Aseta window.__IDEAAGENT_CATEGORY = 'siili-site-tour-2026'
   ```

3. Ilmoita ylläpitäjälle:
   - Kategorian nimi ja ID
   - Deploy-hakemiston sijainti
   - GitHub Pages URL-muoto
