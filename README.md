# Sklad Frontend — migrace na statický hosting

`index.html` v této složce je aktuální kopie `Sklad_index.html` z Apps
Script (zdroj pravdy před migrací) — funguje zatím jen uvnitř Apps Script
Web App (`google.script.run`), na statickém hostingu nepojede beze změny.

## TODO pro Claude Code

**Úkoly k dokončení zde:**

1. ~~Zkopírovat aktuální `Sklad_index.html`~~ — hotovo, viz `index.html`.
2. Nahradit všechna volání `google.script.run.withSuccessHandler(...).xxx(...)`
   voláními `fetch(BACKEND_URL, { method: 'POST', body: JSON.stringify({action, ...params, pin: APP_PIN}) })`.
   `BACKEND_URL` = nasazená Apps Script Web App URL (`/exec`).
3. Přidat proměnnou `APP_PIN` a jednoduchou přihlašovací obrazovku
   (input pro PIN, uložit do `sessionStorage`, aby se nemusel zadávat
   při každé akci).
4. Ověřit, že `sklad-backend/Sklad_app.gs` má `doPost` s ověřením PINu
   odkomentované (viz TODO komentář v tom souboru).
5. Otestovat CORS — Apps Script Web App defaultně vrací CORS hlavičky
   pro `doPost`/`doGet`, ale ověřit na reálném volání z GitHub Pages domény.
6. Otestovat EAN skener (ZXing) — teď by měl fungovat, protože stránka
   neběží v Apps Script iframe sandboxu.
7. Nastavit GitHub Pages: push do repozitáře, zapnout Pages v nastavení
   repozitáře (Settings → Pages → Deploy from branch → main / root).

## Proč tahle migrace

Google Apps Script Web App renderuje HTML v sandboxovaném iframe, který
blokuje `getUserMedia()` (přístup ke kameře) přes Permissions Policy.
Viz `docs/architecture.md` v rootu repozitáře pro detaily.

## Zachovat

- Veškerou existující UI logiku (search, alerty, inventura wizard,
  nákupní seznam, hodnota skladu, správa produktů) — mění se jen
  transportní vrstva (fetch místo google.script.run), ne byznys logika.
