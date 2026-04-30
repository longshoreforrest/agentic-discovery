# Claude Code -ohjaustiedosto

Tämä tiedosto on aina luettava ja ohjeita on seurattava jokaisella käyttäjän
viestillä. Ohjeet ovat **pakollisia** eikä niitä saa ohittaa.

## Pakollinen lokaus jokaisessa käyttäjän promptissa

**ENNEN** muuta työtä jokaisen käyttäjän viestin yhteydessä:

1. Avaa `ClaudeCode_Prompts.md` tämän hakemiston juuressa.
2. Lisää tiedoston **loppuun** uusi merkintä muodossa:

   ```
   # yyyy-MM-dd HH:mm

   {käyttäjän prompti sanatarkasti}
   ```

3. Kellonajaksi käytetään nykyistä kellonaikaa (tai parasta arviota, jos
   tarkka kellonaika ei ole saatavilla).
4. Älä tee muuta työtä ennen kuin lokimerkintä on lisätty.
5. Kun lisätty, jatka normaalisti käyttäjän pyynnön toteutukseen.

### Milloin merkintää EI kirjata

- Järjestelmän automaattiset herätteet / ajastustehtävät (kuten
  `ScheduleWakeup`-fire tai `task-notification`-viestit).
- `system-reminder`-viestit jotka eivät ole käyttäjän kirjoittamia.
- Omat tuotokseni (avustajan vastaukset).

### Milloin merkintä kirjataan

- Kaikki ihmiskäyttäjän rakennuskomennot ja pyynnöt.
- Lyhyet ohjauspyynnöt ("avaa sovellus", "aja julkaisu") kirjataan myös —
  ne ovat osa rakennuspolkua.
- Virheilmoitukset joita käyttäjä liittää mukaan kirjataan kokonaisuudessaan.

## Projektikohtaiset ohjeet — IdeaAgent

### Kuvaus
IdeaAgent on interaktiivinen web-sovellus jossa käyttäjät voivat esittää
sovellusideoita tekstillä tai puheella. Claude Code -agentti toteuttaa ideoita
jonosta turvatarkistuksen jälkeen.

### Tiedostorakenne
- `index.html` — Pääsovellus (single-page, Firebase Firestore backend)
- `_agent.js` — Agentin CLI-työkalu (Firestore REST API, ei riippuvuuksia)
- `ADMIN_CATEGORIES.md` — Ohje kategorioiden luomiseen
- `ADMIN_DEPLOY.md` — Ohje deploy-prosessiin (GitHub/GitLab Pages)
- `AGENT_FETCH_IDEAS.md` — Ohje ideoiden hakuun Firestoresta
- `AGENT_IMPLEMENT.md` — Ohje ideoiden toteutukseen (jonotyöskentely)
- `AGENT_STATUS.md` — Ohje toteutusstatuksen kirjoittamiseen
- `AGENT_SAFETY.md` — **PAKOLLINEN** turvasääntötiedosto
- `FIREBASE_SETUP.md` — Firebase-projektin pystytysohje
- `PUBLISH.md` — **Julkaisuprosessi** (GitHub Pages + GitLab)

### Kriittiset säännöt
- **AGENT_SAFETY.md on aina luettava ja noudatettava ennen ideoiden toteutusta**
- `serviceAccountKey.json` EI SAA päätyä git-repoon
- Deploy-versiot menevät `deploy/{category-id}/` -kansioihin
- Jokaisen idean ID on muotoa `yyyyMMdd_hhmm_NNN`
