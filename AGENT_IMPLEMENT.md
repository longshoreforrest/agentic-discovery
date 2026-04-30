# IdeaAgent — Ideoiden toteutus (jonotyöskentely)

Tämä ohje kertoo Claude Code -agentille miten poimia ja toteuttaa ideoita
jatkuvana vuoropuheluna käyttäjien kanssa.

## Periaate: jatkuva vuoropuhelu

Yhden idean sisällä käyttäjän ja agentin vuoropuhelu voi olla **rajaton**.
Kaikki viestit — sekä käyttäjän toiveet että agentin vastaukset — ovat
samassa `messages`-ketjussa. Kenttä `lastProcessedIndex` kertoo agentille
mihin asti se on jo käsitellyt.

```
[0] 👤 Käyttäjä: "Haluan sääsovelluksen Helsinkiin"
[1] 🤖 Agentti:  "Aloitan toteutuksen: sääsovellus"     ← lastProcessedIndex → 0
[2] 🤖 Agentti:  "Toteutettu: perusnäkymä kolmella lähteellä"
[3] 👤 Käyttäjä: "Lisää myös tuulitiedot"                ← uusi käsittelemätön!
[4] 👤 Käyttäjä: "Ja pyöräilysuositus"                   ← uusi käsittelemätön!
[5] 🤖 Agentti:  "Toteutettu: tuulitiedot + pyöräilysuositus" ← lastProcessedIndex → 4
```

## Toteutuskierros (yksi ajo kerrallaan)

Prosessi ei ole automaattinen — ylläpitäjä ajaa yhden kierroksen kerrallaan.

### Vaihe 1: Hae toteuttamattomat

```bash
node _agent.js queue Siili-Site-Tour-2026
```

Tai tarkista tietty idea:

```bash
node _agent.js pending Siili-Site-Tour-2026 <ideaId>
```

### Vaihe 2: Turvatarkistus (PAKOLLINEN)

**Lue AGENT_SAFETY.md ennen jokaista toteutusta.**

### Vaihe 3: Merkitse aloitus

```bash
node _agent.js status Siili-Site-Tour-2026 <ideaId> in-progress "Aloitan toteuttamaan: [lista toiveista]"
```

### Vaihe 4: Toteuta

Toteuta käyttäjän toiveet. Toteutuksen aikana voit kirjoittaa
väliraportteja:

```bash
node _agent.js reply Siili-Site-Tour-2026 <ideaId> "Perusrakenne valmis, testaan mobiililla"
```

### Vaihe 5: Merkitse valmistuminen

```bash
node _agent.js reply Siili-Site-Tour-2026 <ideaId> "Toteutettu: [mitä tehtiin]"
```

Tai jos kaikki idean toiveet on toteutettu:

```bash
node _agent.js status Siili-Site-Tour-2026 <ideaId> done "Kaikki toiveet toteutettu: [yhteenveto]"
```

### Vaihe 6: Tarkista uudet viestit

```bash
node _agent.js pending Siili-Site-Tour-2026 <ideaId>
```

Jos uusia käsittelemättömiä viestejä → toista vaiheesta 2.
Jos ei uusia → siirry seuraavaan ideaan tai lopeta.

## Komennot yhteenvetona

| Komento | Milloin käytetään |
|---|---|
| `queue <cat>` | Kierroksen alussa: mitä ideoita on jonossa |
| `pending <cat> <id>` | Onko idealle tullut uusia toiveita |
| `status <cat> <id> in-progress "viesti"` | Aloita työ |
| `reply <cat> <id> "viesti"` | Väliraportti tai toteutuskuittaus |
| `status <cat> <id> done "viesti"` | Idea kokonaan valmis |
| `status <cat> <id> blocked "syy"` | Turvatarkistus epäonnistui |

## Tärkeät säännöt

- **ÄLÄ KOSKAAN** toteuta ideaa ilman turvatarkistusta (AGENT_SAFETY.md)
- Käytä `reply` kirjoittaaksesi mihin asti olet toteuttanut
- `reply` päivittää automaattisesti `lastProcessedIndex`:n
- Jos toteutus epäonnistuu, kirjoita virhe `reply`-komennolla
- Yksi kierros kerrallaan — ei automaattista silmukkaa
