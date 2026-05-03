# Agentic Discovery — Deploy-ohje

Tämä tiedosto on Claude Coden käyttämä ohje sovelluksen julkaisuun
GitHub Pages / GitLab Pages -ympäristöön tietylle kategorialle.

## Periaate

Sovellus lukee aktiivisen kategorian **URL-parametrista** `?category=...`.
Sama `index.html`-tiedosto palvelee kaikkia kategorioita — eristys tapahtuu
sillä että jokainen kategoria saa oman linkin.

```
https://example.org/agentic-discovery/?category=Siili-Site-Tour-2026
https://example.org/agentic-discovery/?category=Ilmatieteen_Laitos_20260504
```

Jos `?category=` puuttuu, käyttäjä päätyy **CATEGORY-MISSING**-näytölle eikä
pääse sisään. Tällaiset käynnit kirjataan kokoelmaan
`categories/CATEGORY-MISSING/login_history`.

> **Vanha tapa**: `window.__IDEAAGENT_CATEGORY = '...'` -override toimii edelleen
> taaksepäinyhteensopivuuden vuoksi, mutta uusia deployja ei tarvitse tehdä
> sillä tavoin.

## Deploy-prosessi

### 1. Yksi yhteinen `index.html`

Pelkkä yksi `index.html` riittää. Sama tiedosto sopii kaikille kategorioille.

### 2. Julkaisu (GitHub Pages tai GitLab Pages)

```bash
# Pääbranchin push tekee julkaisun
git add index.html ADMIN_CATEGORIES.md
git commit -m "Update Agentic Discovery"
git push
```

### 3. Kategoriakohtaiset linkit

Jokaiselle kategorialle generoidaan linkki muodossa:

```
{BASE_URL}/?category={CATEGORY_ID}
```

Linkki näkyy automaattisesti adminin **Statistics**-tabilla heti kun kategoria
on luotu.

### 4. Brändäys / otsikko

Yhteisen `index.html`:n `<title>` on geneerinen. Sovellus näyttää kategorian
nimen sisäänkirjautumisruudulla ja yläpalkissa Firestore-dokumentin
`name`-kentästä, joten kategoriakohtaista otsikkoa ei yleensä tarvita.

Jos halutaan, kategoriakohtainen kopio voidaan kuitenkin tehdä:

```bash
mkdir -p deploy/{CATEGORY_ID}
cp index.html deploy/{CATEGORY_ID}/index.html
# Tämä on valinnainen — ei yleensä tarpeen URL-param-skeemassa
```

---

## Deploy-rekisteri

| Kategoria-ID | URL | Päivämäärä |
|---|---|---|
| Siili-Site-Tour-2026 | `{BASE_URL}/?category=Siili-Site-Tour-2026` | 2026-04-29 |
| Ilmatieteen_Laitos_20260504 | `{BASE_URL}/?category=Ilmatieteen_Laitos_20260504` | 2026-05-01 |
