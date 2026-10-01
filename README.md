# JPO Úpice – dostupnost členů

Export aktuální aplikace ze dne 1. 10. 2026. Zdrojový commit: 23fe612c703462035e59d9d9c013b1711498b258.

## Co obsahuje
Přehled dostupných členů, kvalifikace, poznámky, kalendář s hodinovým plánem, pracovní dobu, plánování nepřítomnosti, administraci, historii akcí a světlý/tmavý režim. Přihlášení je přes jméno a heslo; dočasné heslo se musí změnit. Naplánovaná nepřítomnost má přednost před ruční dostupností, pokud člen nepotvrdí výjimku.

## Nahrání na GitHub
1. Rozbalte ZIP.
2. Vytvořte repozitář a nahrajte obsah složky `jpo-upice` (ne samotný ZIP).
3. Zachovejte také soubory s tečkou v názvu, například `.gitignore` a `.openai/hosting.json`.

GitHub slouží pro zdrojový kód. **GitHub Pages tuto aplikaci nespustí**: přihlášení a ukládání vyžadují server a databázi.

## Technologie a spuštění
Projekt používá React, Vinext/Vite a Cloudflare Workers s databází D1 (SQLite). Vyžaduje Node.js alespoň 22.13 a pnpm; verze pnpm je uvedena v `package.json`.

```sh
pnpm install
pnpm dev
```

Lokální vývoj se otevírá na portu 5173. Pro plnou funkčnost API je potřeba připravit lokální databázi D1 a aplikovat všechny migrace `drizzle/*.sql` ve vzestupném pořadí. Konfigurace bindingu `DB` je ve `vite.config.ts`. Nejde o projekt pro XAMPP ani statický web.

```sh
pnpm build
```

Produkční nasazení potřebuje vlastní Cloudflare Worker a D1 databázi, binding `DB` a aplikované migrace. Aktuální `.openai/hosting.json` identifikuje původní nasazení v Sites; samotné nahrání na GitHub toto nasazení ani databázi nepřenese. Pro jiné nasazení je nutné nastavit vlastní Cloudflare konfiguraci a skutečné ID D1 databáze místo vývojového zástupného ID.

## Administrátor a účty
Na nové prázdné databázi nastavte serverovou proměnnou `BOOTSTRAP_PASSWORD` na vlastní dočasné heslo (nejméně 10 znaků). První přihlášení uživatelem `admin` vytvoří administrátora, pokud dosud žádný neexistuje. Potom bude vyžadována změna hesla. Tajné hodnoty ukládejte jako serverové secrets, nikoli do repozitáře.

Zdroj obsahuje jednorázový import 28 členů (`lib/roster-import.ts`). Na nové databázi proběhne při prvním načtení API. Obsahuje původní seznam jmen, kvalifikací a hashovaných dočasných hesel. Pokud chcete použít aplikaci pro jinou jednotku, tento import upravte před spuštěním. Vytvořený příznak v tabulce `settings` brání opakovanému importu a obnovování později smazaných členů.

## Co ZIP neobsahuje
Živou databázi, aktuální dostupnost, poznámky, plány, relace, historii ani později změněná hesla. Také neobsahuje přístupové tokeny, `.env`, závislosti `node_modules`, sestavené soubory `dist`, Git historii ani lokální databázi. Existující data na původním webu zůstávají beze změny.
