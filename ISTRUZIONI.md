# Come generare l'APK — istruzioni

Questo pacchetto contiene tutto il necessario per trasformare la tua web app
"DJ Mixer Mobile" in un'app Android reale e farla compilare automaticamente
da GitHub Actions (che, a differenza del mio ambiente, ha accesso pieno a
internet e può scaricare Android SDK / Gradle).

## Cosa contiene lo zip

- `android/` — il progetto Android nativo generato da Capacitor
  (`npx cap add android`), già pulito dai file temporanei/generati.
- `package.json` e `package-lock.json` — aggiornati con le dipendenze
  Capacitor (`@capacitor/core`, `@capacitor/android`, `@capacitor/cli`).
- `.github/workflows/android.yml` — nuovo workflow che sostituisce quello
  esistente. Il vecchio si aspettava un progetto Android già pronto alla
  radice del repo (non c'era), quindi falliva. Questo invece: installa le
  dipendenze, builda la web app, sincronizza Capacitor, e compila l'APK
  con l'Android SDK scaricato al volo dal runner.

## Cosa fare

1. Copia queste cartelle/file dentro il tuo repo locale `DJ`, sovrascrivendo
   quelli esistenti:
   - `android/`
   - `package.json`
   - `package-lock.json`
   - `.github/workflows/android.yml`
2. Da dentro il repo:
   ```
   git add android package.json package-lock.json .github/workflows/android.yml
   git commit -m "Add Android native project and fix CI build"
   git push
   ```
3. Vai su GitHub → tab **Actions** del repo: la run "Build Android APK"
   parte da sola. Impiega un paio di minuti.
4. A run completata, apri la run → sezione **Artifacts** in basso: troverai
   `dj-mixer-mobile-debug-apk` da scaricare. È l'APK (debug, non firmato per
   il Play Store, ma installabile direttamente su un telefono Android
   abilitando "Origini sconosciute").

## Note

- L'APK generato è una build di **debug**, pensata per testare l'app sul
  telefono. Per pubblicarla sul Play Store servirebbe una build "release"
  firmata con un keystore — se ti interessa questo passaggio, fammelo
  sapere e prepariamo anche quello.
- La dipendenza `@google/genai` presente in `package.json` non risulta
  usata nel codice sorgente: non serve nessuna API key per compilare
  l'APK.
