# IdeaAgent — Julkaisuprosessi (toistuva)

Tämä ohje on Claude Coden käyttämä toistuva julkaisuprosessi.
Kun käyttäjä pyytää **"julkaise"**, **"deploy"** tai **"publish"**,
suorita alla olevat vaiheet järjestyksessä.

## Kohdehakemistot

| Kohde | Hakemisto | Tarkoitus |
|---|---|---|
| **GitHub Pages** | `/Users/tapio.pitkaranta/Documents/GitHub/agentic-discovery/` | Julkinen sovellus GitHub Pagesin kautta |
| **GitLab** | `/Users/tapio.pitkaranta/Documents/Gitlab/rapid-agent-prototypes/agentic-discovery/` | Siili GitLab Pages (tuotanto) |

## Lähdetiedostot

Lähde: `/Users/tapio.pitkaranta/Documents/GitHub/RapidPrototypes/ClaudeCode/IdeaAgent/`

### GitHub Pages -kohteeseen kopioidaan:

| Tiedosto | Selite |
|---|---|
| `index.html` | Pääsovellus |
| `_agent.js` | Agentin CLI-työkalu |
| `AGENT_SAFETY.md` | Turvasäännöt (pakollinen agenteille) |
| `AGENT_FETCH_IDEAS.md` | Ideoiden hakuohje |
| `AGENT_IMPLEMENT.md` | Toteutusohje |
| `AGENT_STATUS.md` | Statuskirjoitusohje |
| `FIREBASE_SETUP.md` | Firebase-pystytysohje |
| `ADMIN_CATEGORIES.md` | Kategorioiden hallinta |
| `ADMIN_DEPLOY.md` | Deploy-ohje |
| `PUBLISH.md` | Tämä julkaisuohje |

**Huom:** GitHub Pages -kohteeseen EI kopioida `CLAUDE.md` — se on kehitysympäristökohtainen.

### GitLab-kohteeseen kopioidaan:

Samat tiedostot kuin yllä, PLUS:
- `CLAUDE.md` (Claude Code -ohjaustiedosto)

PAITSI:
- **EI** `ClaudeCode_Prompts.md` (rakentamisen lokitiedosto, ei kuulu julkaisuun)
- **EI** `package.json`, `package-lock.json` (ei tarvita tuotannossa)
- **EI** `firebase.json`, `firestore.rules` (Firebase-konfiguraatio, ei julkaista)

## Vaihe 1: Kopioi GitHub Pages -kohteeseen

```bash
SRC="/Users/tapio.pitkaranta/Documents/GitHub/RapidPrototypes/ClaudeCode/IdeaAgent"
GH="/Users/tapio.pitkaranta/Documents/GitHub/agentic-discovery"

cp "$SRC/index.html" "$GH/index.html"
cp "$SRC/_agent.js" "$GH/_agent.js"
cp "$SRC/AGENT_SAFETY.md" "$GH/AGENT_SAFETY.md"
cp "$SRC/AGENT_FETCH_IDEAS.md" "$GH/AGENT_FETCH_IDEAS.md"
cp "$SRC/AGENT_IMPLEMENT.md" "$GH/AGENT_IMPLEMENT.md"
cp "$SRC/AGENT_STATUS.md" "$GH/AGENT_STATUS.md"
cp "$SRC/FIREBASE_SETUP.md" "$GH/FIREBASE_SETUP.md"
cp "$SRC/ADMIN_CATEGORIES.md" "$GH/ADMIN_CATEGORIES.md"
cp "$SRC/ADMIN_DEPLOY.md" "$GH/ADMIN_DEPLOY.md"
cp "$SRC/PUBLISH.md" "$GH/PUBLISH.md"
```

## Vaihe 2: Kopioi GitLab-kohteeseen

```bash
GL="/Users/tapio.pitkaranta/Documents/Gitlab/rapid-agent-prototypes/agentic-discovery"

cp "$SRC/index.html" "$GL/index.html"
cp "$SRC/_agent.js" "$GL/_agent.js"
cp "$SRC/AGENT_SAFETY.md" "$GL/AGENT_SAFETY.md"
cp "$SRC/AGENT_FETCH_IDEAS.md" "$GL/AGENT_FETCH_IDEAS.md"
cp "$SRC/AGENT_IMPLEMENT.md" "$GL/AGENT_IMPLEMENT.md"
cp "$SRC/AGENT_STATUS.md" "$GL/AGENT_STATUS.md"
cp "$SRC/FIREBASE_SETUP.md" "$GL/FIREBASE_SETUP.md"
cp "$SRC/ADMIN_CATEGORIES.md" "$GL/ADMIN_CATEGORIES.md"
cp "$SRC/ADMIN_DEPLOY.md" "$GL/ADMIN_DEPLOY.md"
cp "$SRC/PUBLISH.md" "$GL/PUBLISH.md"
cp "$SRC/CLAUDE.md" "$GL/CLAUDE.md"
```

## Vaihe 3: Git push GitHub Pages -repoon

```bash
cd /Users/tapio.pitkaranta/Documents/GitHub/agentic-discovery
git add -A
git commit -m "Publish: Agentic Idea Discovery update"
git push
```

## Vaihe 4: Git push GitLab-repoon

```bash
cd /Users/tapio.pitkaranta/Documents/Gitlab/rapid-agent-prototypes
git add agentic-discovery/
git commit -m "Publish: Agentic Idea Discovery update"
git push
```

## Vaihe 5: Raportoi muutokset

Tulosta käyttäjälle:
1. Mitkä tiedostot kopioitiin
2. Oliko eroja edelliseen julkaisuun
3. Git push -tulos molemmille kohteille

## Tarkistuslista ennen julkaisua

- [ ] `index.html` toimii selaimessa (avaa ja testaa)
- [ ] Firebase-konfiguraatio on oikein (`firebaseConfig`-objekti)
- [ ] `CATEGORY`-muuttuja osoittaa oikeaan kategoriaan
- [ ] `_agent.js` toimii: `node _agent.js list Siili-Site-Tour-2026`
- [ ] Ei salaisia avaimia tai `serviceAccountKey.json` mukana

## Pikakomento

Kun käyttäjä sanoo **"julkaise"**, aja vaiheet 1-5 ja raportoi.
