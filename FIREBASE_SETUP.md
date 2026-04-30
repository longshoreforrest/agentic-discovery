# IdeaAgent — Firebase-projektin pystytys

## 1. Luo Firebase-projekti

1. Mene https://console.firebase.google.com
2. Klikkaa "Add project"
3. Anna nimeksi esim. `ideaagent` tai `ideaagent-prod`
4. Ota Analytics pois päältä (ei tarvita)
5. Klikkaa "Create project"

## 2. Ota Firestore käyttöön

1. Vasemmasta valikosta: **Build → Firestore Database**
2. Klikkaa "Create database"
3. Valitse sijainti: `europe-west1` (Belgia) tai `europe-west3` (Frankfurt)
4. Valitse "Start in **production mode**"
5. Klikkaa "Enable"

## 3. Aseta Firestore-säännöt

Mene Firestore → Rules ja korvaa oletussäännöt:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Kategoriadokumentit — luku kaikille, kirjoitus rajoitettu
    match /categories/{categoryId} {
      allow read: if true;
      allow write: if true;

      // Ideat — luku kaikille, kirjoitus kaikille (anonyymi)
      match /ideas/{ideaId} {
        allow read: if true;
        allow create: if true;
        allow update: if true;
      }

      // Sessiot — luku/kirjoitus kaikille (presence-seuranta)
      match /sessions/{sessionId} {
        allow read, write: if true;
      }
    }
  }
}
```

**Huom:** Nämä säännöt ovat avoimet koska sovellus on anonyymikirjautuminen
nimimerkillä. Tuotantokäytössä voi kiristää sääntöjä Firebase Auth
-integraatiolla.

## 4. Luo web-sovellus

1. Projektin asetukset (rattaan ikoni) → General
2. Alhaalla "Your apps" → klikkaa web-ikoni `</>`
3. Anna nickname: `IdeaAgent Web`
4. ÄLÄ ota Firebase Hosting käyttöön (käytetään GitHub Pages)
5. Kopioi `firebaseConfig`-objekti

## 5. Päivitä firebaseConfig sovellukseen

Avaa `index.html` ja korvaa placeholder-arvot:

```javascript
const firebaseConfig = {
  apiKey: "AIzaSy...",
  authDomain: "ideaagent-xxxxx.firebaseapp.com",
  projectId: "ideaagent-xxxxx",
  storageBucket: "ideaagent-xxxxx.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abcdef123456"
};
```

## 6. Service Account (agentin Firestore-yhteys)

Claude Code -agentti tarvitsee palvelinpuolen yhteyden Firestoreen.

1. Firebase Console → Project Settings → Service accounts
2. Klikkaa "Generate new private key"
3. Tallenna JSON-tiedosto projektin juureen nimellä `serviceAccountKey.json`
4. **TÄRKEÄÄ:** Lisää `.gitignore`-tiedostoon:
   ```
   serviceAccountKey.json
   ```

## 7. Firestore-tietorakenne

```
categories/
  {categoryId}/                    ← esim. "siili-site-tour-2026"
    name: "Siili Site Tour 2026"
    categoryId: "siili-site-tour-2026"
    createdAt: "2026-04-29T12:00:00Z"
    blockedUsers: ["hacker123"]
    
    ideas/
      {ideaId}/                    ← esim. "20260429_1430_001"
        ideaId: "20260429_1430_001"
        author: "Matti"
        text: "Lisää karttanäkymä..."
        createdAt: "2026-04-29T14:30:00Z"
        status: "new" | "in-progress" | "done" | "blocked"
        category: "siili-site-tour-2026"
        messages: [
          { role: "user", author: "Matti", text: "...", timestamp: "..." },
          { role: "user", author: "Matti", text: "Lisätoive...", timestamp: "..." }
        ]
        implementationLog: [
          { timestamp: "...", agent: "claude-code", message: "Aloitan..." },
          { timestamp: "...", agent: "claude-code", message: "Valmis: ..." }
        ]
        blockedAt: null
        blockedReason: null
        completedAt: null
    
    sessions/
      {username}/
        user: "Matti"
        loginAt: "2026-04-29T14:30:00Z"
        ua: "desktop" | "mobile"
```

## 8. Testaa

1. Avaa `index.html` selaimessa (voi avata suoraan tiedostona tai live serverillä)
2. Kirjoita nimimerkki ja klikkaa "Aloita"
3. Kirjoita idea tai puhu mikrofooniin
4. Tarkista Firebase Consolesta että data tallentui

## Tiedossa olevat rajoitukset

- Web Speech API (puheentunnistus) toimii parhaiten Chromessa/Edgessä
- Safari/Firefox tuki voi olla rajoitettu
- Firestore free tier: 50K lukua, 20K kirjoitusta, 20K poistoa päivässä
