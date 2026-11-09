# Publicera Nordpool Level Card i Home Assistant Marketplace

Den här guiden beskriver hur kortet tas in i Marketplace-katalogen, så att alla kan hitta det via sök under *Inställningar → Marketplace* utan att lägga till repot från en länk. Marketplace (inbyggt i Home Assistant 2026.11, tidigare HACS) använder katalogen i [hacs/default](https://github.com/hacs/default).

Källa: [Home Assistant contributors – Marketplace](https://contributors.home-assistant.io/marketplace/).

## Vad som redan finns i repot

| Krav | Var |
|---|---|
| `hacs.json` med `name` i roten | [`hacs.json`](hacs.json) |
| Kortets JS-fil på en plats Marketplace letar på | [`dist/nordpool-level-card.js`](dist/nordpool-level-card.js) |
| README med bilder (krävs för dashboard-kort) | [`README.md`](README.md), [`preview.png`](preview.png) |
| Licens | [`LICENSE`](LICENSE) (MIT) |
| HACS-validering vid varje push och PR samt varje natt | [`.github/workflows/validate.yaml`](.github/workflows/validate.yaml) |
| Förslag i kortväljarens Community-sektion | `getEntitySuggestion` i `window.customCards` |

## 1. Repo-inställningar på GitHub (en gång)

På https://github.com/em0g/nordpool-level-card:

1. **Beskrivning:** klicka på ⚙ bredvid *About* och fyll i t.ex. *Kompakt Home Assistant-kort för Nord Pool-elpriser med nivåerna Billigt / Normalt / Dyrt*.
2. **Topics** (i samma ruta): `home-assistant`, `lovelace`, `dashboard`, `custom-card`, `hacs`, `nordpool`.
3. **Issues:** under *Settings → General → Features* ska *Issues* vara ikryssat.

## 2. Skapa en release

Marketplace visar taggen för den senaste **releasen** som version. Det räcker inte med en tagg utan release.

1. Kontrollera att `VERSION` i `dist/nordpool-level-card.js` har det nya numret (just nu `3.2.0`).
2. Gå till *Releases → Draft a new release*.
3. Tag: `v3.2.0` (skapas på `main`). Title: `v3.2.0`.
4. Release-noter: beskriv vad som ändrats. Lägg sådant användaren måste göra först (t.ex. ändrade inställningar) överst. Noterna visas under *Inställningar → Uppdateringar*.
5. Publicera. Kryssa i *Set as a pre-release* om versionen bara ska nå dem som aktiverat förhandsversioner.

Vid varje ny version: höj `VERSION` i JS-filen, pusha och skapa en ny release med samma nummer.

## 3. Kontrollera valideringen

Under fliken *Actions* ska workflowen **Validate** vara grön på senaste commit. Den kontrollerar bland annat:
- beskrivning, topics och aktiverade issues (steg 1)
- att `hacs.json` finns
- att README:n har bilder.

Katalogen godtar inte körningar med `ignore`, så alla kontroller måste gå igenom. Starta om körningen via *Run workflow* efter att du har ändrat repo-inställningarna.

## 4. Ansök till katalogen (hacs/default)

PR:en ska komma från **ditt personliga GitHub-konto**, inte från en organisation.

1. Forka https://github.com/hacs/default.
2. Skapa en ny gren från `master` i din fork. Arbeta inte direkt på `master`.
3. Öppna filen `plugin` (en JSON-lista) och lägg till
   ```json
   "em0g/nordpool-level-card",
   ```
   i **alfabetisk ordning**.
4. Öppna en pull request mot `hacs/default` och fyll i hela mallen:
   - checklistan
   - en länk till senaste release, t.ex. `https://github.com/em0g/nordpool-level-card/releases/tag/v3.2.0`
   - en länk till en grön körning av **Validate** (utan `ignore`) under *Actions*.
5. Begär inte granskning av enskilda personer. Det leder till att PR:en stängs.

Ofullständiga eller felaktiga mallar stängs utan förvarning. Vid mindre fel görs PR:en om till utkast tills du har rättat.

## 5. Efter godkännandet

- Katalogdata genereras var 4:e timme, och Home Assistant läser katalogen var 6:e timme. Det kan alltså ta några timmar innan kortet syns i sökningen.
- Användare ser en varning före installationen om att community-innehåll inte granskas av Home Assistant-projektet. Du ansvarar för att svara på issues och uppdatera kortet när Home Assistant ändras.
- Arkivera inte repot, eftersom arkiverade repon tas bort ur katalogen.

## Länk för användare

Fungerar redan nu, även innan kortet finns i katalogen (Home Assistant 2026.11 eller senare):

```
https://my.home-assistant.io/redirect/marketplace_repository/?owner=em0g&repository=nordpool-level-card&category=plugin
```
