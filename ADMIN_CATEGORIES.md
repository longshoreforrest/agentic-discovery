# Agentic Discovery — Kategorioiden hallinta

Tämä tiedosto on Claude Coden käyttämä ohje uusien ideakategorioiden luomiseen.

## Uuden kategorian luominen

Kategoria voidaan luoda kolmella tavalla:

### A) Statistics-tab (admin-UI)

Adminkäyttäjä (`Tapio Pitkäranta`) näkee Statistics-tabilla "Kategoriat"-lohkon
ja "Luo uusi kategoria" -lomakkeen. Anna ID, näyttönimi, kuvaus → Luo.
Sovellus kirjoittaa dokumentin `categories/{CATEGORY_ID}` ja näyttää
valmiin sisäänpääsyn URLin muodossa `?category={ID}`.

### B) `_create_category.js` -skripti

```bash
node _create_category.js "<id>" "<näyttönimi>" "<kuvaus>"
```

Esim:
```bash
node _create_category.js "Ilmatieteen_Laitos_20260504" "Ilmatieteen laitos — 4.5.2026" "Sessio 4.5.2026"
```

### C) Rekisteröi kategoria tähän tiedostoon

Riippumatta luontitavasta lisää uusi rivi alla olevaan rekisteriin
manuaalisesti dokumentointia varten.

### Firestore-rakenne

```
categories/{CATEGORY_ID}
{
  "name": "Kategorian näyttönimi",
  "categoryId": "kategoria-tunnus",
  "createdAt": "ISO 8601 aikaleima",
  "createdBy": "kuka loi",
  "description": "Lyhyt kuvaus",
  "blockedUsers": []
}
```

---

## Kategoriarekisteri

| Kategoria-ID | Nimi | Luotu | Kuvaus |
|---|---|---|---|
| Siili-Site-Tour-2026 | Siili Site Tour 2026 | 2026-04-29 | Siili Site Tour 2026 -tapahtuman ideat |
| Ilmatieteen_Laitos_20260504 | Ilmatieteen laitos — 4.5.2026 | 2026-05-01 | Ilmatieteen laitoksen idea-/sovellussessio 4.5.2026 |

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
