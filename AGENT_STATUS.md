# Agentic Discovery — Toteutuksen statuksen kirjoittaminen

Tämä ohje kertoo Claude Code -agentille miten kirjoittaa toteutuksen
edistyminen takaisin idean viestiketjuun.

## Malli: yhtenäinen viestiketju

Agentin viestit menevät samaan `messages`-ketjuun kuin käyttäjien viestit.
Agentin viestit tunnistetaan `role: "agent"` -kentästä. Erillistä
`implementationLog`-kenttää ei enää käytetä.

## Komennot

### Kirjoita viesti ketjuun (ja päivitä lastProcessedIndex)

```bash
node _agent.js reply <category> <ideaId> "Vapaamuotoinen viesti"
```

Tämä lisää viestin ketjuun roolilla `agent` ja päivittää
`lastProcessedIndex`:n viimeiseen käyttäjäviestiin asti.

### Vaihda idean status

```bash
node _agent.js status <category> <ideaId> <status> "Viesti"
```

Statusarvot: `new`, `in-progress`, `done`, `blocked`

## Tyypilliset viestit

### Lukeminen
```bash
node _agent.js fetch Siili-Site-Tour-2026 20260429_0623_124
```
→ Kirjoittaa automaattisesti "Agentti kävi lukemassa ideoitasi"

### Työn aloitus
```bash
node _agent.js status Siili-Site-Tour-2026 20260429_0623_124 in-progress "Aloitan toteuttamaan: sääsovellus Helsingille"
```

### Väliraportti
```bash
node _agent.js reply Siili-Site-Tour-2026 20260429_0623_124 "Perusrakenne valmis, lisäämässä tuulidataa"
```

### Toteutuskuittaus
```bash
node _agent.js reply Siili-Site-Tour-2026 20260429_0623_124 "Toteutettu viestit [0]-[4]: sääsovellus + tuulitiedot + pyöräilysuositus"
```

### Valmistuminen
```bash
node _agent.js status Siili-Site-Tour-2026 20260429_0623_124 done "Kaikki toiveet toteutettu"
```

### Estäminen
```bash
node _agent.js status Siili-Site-Tour-2026 20260429_0623_124 blocked "Haitallinen sisältö: tiedostojen poistokomentoja"
```

## Statusarvot

| Status | Merkitys | Milloin |
|---|---|---|
| `new` | Uusi, ei käsitelty | Oletus luonnissa |
| `in-progress` | Agentti työskentelee | Työn alkaessa |
| `done` | Kaikki toiveet toteutettu | Työn valmistuessa |
| `blocked` | Haitallinen sisältö | Turvatarkistus epäonnistui |

## lastProcessedIndex

Kenttä `lastProcessedIndex` on kokonaisluku joka kertoo viimeisen
käyttäjäviestin indeksin jonka agentti on käsitellyt.

- Alkuarvo: `-1` (mitään ei käsitelty)
- Päivittyy automaattisesti kun agentti kutsuu `reply`
- `pending`-komento käyttää tätä näyttääkseen vain uudet viestit
