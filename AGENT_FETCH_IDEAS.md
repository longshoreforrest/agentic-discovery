# IdeaAgent — Ideoiden haku tietokannasta

Tämä ohje kertoo Claude Code -agentille miten hakea tietyn kategorian ideat
Firestore-tietokannasta CLI-työkalulla `_agent.js`.

## Edellytys

Tiedosto `_agent.js` on projektin juuressa. Se käyttää Firestore REST API:a
suoraan — ei tarvita service accountia eikä npm-riippuvuuksia.

## Komennot

### Listaa kaikki ideat

```bash
node _agent.js list <category> [status]
```

Status-vaihtoehdot: `all` (oletus), `new`, `in-progress`, `done`, `blocked`

Tuloste näyttää jokaisen idean kohdalla myös käsittelemättömien viestien
lukumäärän: `[N uutta]`.

### Näytä toteuttamattomien jono

```bash
node _agent.js queue <category>
```

Listaa kaikki `new`-statuksella olevat ideat ja niiden käsittelemättömät
käyttäjäviestit.

### Hae yksittäisen idean koko viestiketju

```bash
node _agent.js fetch <category> <ideaId>
```

Näyttää kaikki viestit aikajärjestyksessä (sekä käyttäjien että agentin).
Kirjoittaa samalla viestiketjuun merkinnän "Agentti kävi lukemassa ideoitasi".

### Hae vain käsittelemättömät viestit

```bash
node _agent.js pending <category> <ideaId>
```

Näyttää vain ne käyttäjäviestit joiden indeksi > `lastProcessedIndex`.
Tämä on pääkomento jota agentti käyttää tarkistaakseen onko uusia toiveita.

## Esimerkit

```bash
# Listaa kaikki Siili-Site-Tour-2026 kategorian ideat
node _agent.js list Siili-Site-Tour-2026

# Näytä vain uudet ideat
node _agent.js list Siili-Site-Tour-2026 new

# Tarkista onko idealla käsittelemättömiä viestejä
node _agent.js pending Siili-Site-Tour-2026 20260429_0623_124

# Hae koko viestiketju
node _agent.js fetch Siili-Site-Tour-2026 20260429_0623_124
```

## Tietomalli

| Kenttä | Tyyppi | Selite |
|---|---|---|
| `ideaId` | string | Uniikki tunnus: `yyyyMMdd_hhmm_NNN` |
| `author` | string | Idean alkuperäinen luoja |
| `text` | string | Alkuperäinen ideateksti |
| `status` | string | `new` / `in-progress` / `done` / `blocked` |
| `category` | string | Kategoriatunnus |
| `createdAt` | string | ISO 8601 aikaleima |
| `messages` | array | Koko viestiketju (käyttäjä + agentti) |
| `lastProcessedIndex` | integer | Viimeisen agentin käsittelemän viestin indeksi |

### Viesti-objekti (messages-taulukon alkio)

| Kenttä | Selite |
|---|---|
| `role` | `user` tai `agent` |
| `author` | Kirjoittajan nimi |
| `text` | Viestin sisältö |
| `timestamp` | ISO 8601 aikaleima |
