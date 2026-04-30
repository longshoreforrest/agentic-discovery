# IdeaAgent — Deploy-ohje

Tämä tiedosto on Claude Coden käyttämä ohje sovelluksen julkaisuun
GitHub Pages / GitLab Pages -ympäristöön tietylle kategorialle.

## Periaate

Jokainen kategoria saa oman `index.html`-tiedoston joka on kopio
pääsovelluksesta. Ainoa ero on `window.__IDEAAGENT_CATEGORY`-muuttujan arvo,
joka määrittää mitä Firestore-kokoelmaa sovellus käyttää.

## Deploy-komento

Kun ylläpitäjä pyytää deployta tietylle kategorialle, toimi näin:

### 1. Luo deploy-hakemisto

```bash
mkdir -p deploy/{CATEGORY_ID}
```

### 2. Kopioi ja konfiguroi index.html

Kopioi `index.html` → `deploy/{CATEGORY_ID}/index.html` ja lisää
**ennen** `<script>`-tagia jossa Firebase-konfiguraatio seuraava rivi:

```html
<script>window.__IDEAAGENT_CATEGORY = '{CATEGORY_ID}';</script>
```

Tämä rivi tulee **ennen** pääsovelluksen `<script>`-blokkia.

### 3. Varmista Firebase-konfiguraatio

Tarkista että `firebaseConfig`-objekti `index.html`-tiedostossa sisältää
oikeat arvot. Jos ylläpitäjä on antanut Firebase-tunnukset, päivitä ne.

### 4. Päivitä title ja brändäys (valinnainen)

Jos ylläpitäjä haluaa kategoriakohtaisen otsikon:
- Päivitä `<title>` tagiin kategorian nimi
- Päivitä `<h1>` headerissa

### 5. GitHub Pages -julkaisu

```bash
# Siirry deploy-hakemistoon
cd deploy/{CATEGORY_ID}

# Alusta erillinen git-repo tai käytä samaa repoa
git init
git add .
git commit -m "Deploy IdeaAgent: {CATEGORY_ID}"
git remote add origin https://github.com/{USER}/{REPO}.git
git push -u origin main
```

Tai jos käytetään samaa repoa ja GitHub Pages servaa `docs/`-kansiosta:

```bash
mkdir -p docs/{CATEGORY_ID}
cp deploy/{CATEGORY_ID}/index.html docs/{CATEGORY_ID}/index.html
git add docs/{CATEGORY_ID}
git commit -m "Deploy IdeaAgent: {CATEGORY_ID}"
git push
```

### 6. GitLab Pages -julkaisu

Sama periaate, mutta tiedosto menee `public/`-kansioon ja `.gitlab-ci.yml`
tarvitaan:

```yaml
pages:
  stage: deploy
  script:
    - echo "Deploying"
  artifacts:
    paths:
      - public
  only:
    - main
```

---

## Deploy-rekisteri

Kirjaa tähän kaikki tehdyt deployt.

| Kategoria-ID | Deploy-polku | URL | Päivämäärä |
|---|---|---|---|
| (ei vielä deployta) | | | |

---

## Esimerkki

Ylläpitäjä: "Deploaa Siili-Site-Tour-2026"

Claude Code:
1. `mkdir -p deploy/siili-site-tour-2026`
2. Kopioi `index.html` → `deploy/siili-site-tour-2026/index.html`
3. Lisää ennen pääscriptiä: `<script>window.__IDEAAGENT_CATEGORY = 'siili-site-tour-2026';</script>`
4. Päivittää `<title>` → "IdeaAgent — Siili Site Tour 2026"
5. Raportoi ylläpitäjälle valmiin polun
