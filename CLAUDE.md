# IMPOSTOR — párty slovní hra

## Co to je
Jednosouborová webová appka (`index.html`, čistý HTML/CSS/JS, žádný build krok,
žádné závislosti). Česká párty hra typu "kdo je podvodník" pro pass-and-play
na jednom telefonu/tabletu — hráči si postupně předávají zařízení, každý si
přečte svou roli/slovo a schová.

## Aktuální stav (funkční, hratelné)
- Kompletní herní tok: nastavení hry → přidání hráčů → postupné odhalení
  rolí → kolo nápověd → diskuze → hlasování → případná "poslední šance"
  impostora → výsledek → statistiky
- Nastavení: počet impostorů (1–2), výběr kategorií slov (teď **multi-select**
  — víc kategorií najednou přes klikací chipy, ne jen jedna), informace pro
  impostora (nic / kategorie / obecná nápověda), pořadí hráčů u nápověd,
  časovač diskuze, zapnutí/vypnutí "poslední šance"
- Databáze slov: pole `WORDS`, 320 záznamů v 16 kategoriích (Jídlo, Nápoje,
  Zvířata, Předměty, Domácnost, Povolání, Sport, Doprava, Příroda, Místa,
  Technologie, Filmy a seriály, Aktivity, Lidé, **Minecraft**, Ostatní).
  Každý záznam: `{word, category, hint}`.
- **Důležité:** `hint` je JEDNO slovo (ne věta) — přesně to slovo, které si
  impostor může v "kole nápověd" říct, aby zapadl mezi ostatní hráče. Při
  jakémkoli dalším doplňování slov do databáze dodržet tenhle formát
  (jednoslovné, smysluplné, ne přímo prozrazující odpověď).
- Statistiky hráčů (počet her, kolikrát impostor, výhry) + "Naposledy hráli"
  chipy pro rychlé doplnění jména — ukládá se do `localStorage`.
- Ruční záloha/obnova statistik přes base64 kód (přidáno kvůli tomu, že
  localStorage appek přidaných na iOS plochu se občas maže — automatická
  cloudová synchronizace zatím záměrně NENÍ řešená, viz níže).
- PWA meta tagy pro "Přidat na plochu" na iPhonu (apple-mobile-web-app-capable,
  safe-area padding pro notch atd.).
- Vlastní ikonka appky: `apple-touch-icon.png`, `favicon-32.png`,
  `icon-192.png`, `icon-512.png`, `manifest.json` — motiv maska v barvách
  appky (tmavé pozadí + růžovo-oranžový gradient, styl shodný s UI appky).

- Opraveno 2026-09-25: tip v „poslední šanci“ se porovnává bez diakritiky,
  prázdný tip nejde odeslat, nový hráč dostane volné jméno „Hráč N“
  a hra nezačne při duplicitních jménech, 11 nápověd, které prozrazovaly
  slovo, bylo nahrazeno.

## Plán / co zbývá
1. ✅ HOTOVO (2026-09-25): Git repo + GitHub — veřejné repo
   https://github.com/romanslahunek-beep/impostor-hra, větev `main`.
2. ✅ HOTOVO (2026-09-25): GitHub Pages — hra běží na
   https://romanslahunek-beep.github.io/impostor-hra/ (nasazuje se
   automaticky z `main` při každém pushi).
   Na ploše PC je zástupce `Impostor.lnk` (Edge v režimu `--app`, ikona
   `%LOCALAPPDATA%\Impostor\impostor.ico`).
3. Později: vlastní doména. Uživateli vadí jméno účtu v adrese — alternativa
   je GitHub organizace (adresa `<nazev>.github.io`, repo jde přesunout).
3b. **OTEVŘENÁ OTÁZKA — 2 impostoři:** při 3 hráčích se 2 impostoři domluví
   a přehlasují jediného poctivého. Navrženo (čeká na souhlas uživatele):
   2 impostoři až od 5 hráčů; hráči vyhrávají, když chytí aspoň jednoho.
3c. Nevyřešené drobnosti z kontroly kódu (uživatel zatím neřekl, zda opravit):
   Google Fonts jako externí závislost (GDPR), v kódu zálohy se čísla
   neescapují (self-XSS), `correctVotes` se počítá, ale nezobrazuje, přerušená
   hra se počítá jako odehraná, prázdná CSS pravidla na začátku `<style>`,
   „Trůny“ → „Hra o trůny“, „Frozen“ → „Ledové království“.
4. Později: možnost přepnout jazyk (čeština/angličtina) — vyžaduje vytáhnout
   UI texty do slovníku (aktuálně jsou napevno v render funkcích) a vytvořit
   druhou, anglickou databázi slov + jednoslovných nápověd (ne jen strojový
   překlad — nápovědy musí dávat smysl v jiném jazykovém/kulturním kontextu).
5. Zvažuje se App Store verze (potřebný nativní obal přes Capacitor/Cordova,
   Apple Developer účet 99 $/rok, schvalovací proces) — uživatel to zatím
   odkládá, chce nejdřív přes web/sociální sítě ověřit, jestli je o hru
   vůbec zájem.
6. **Trvale odloženo** (nerozjíždět bez výslovného zadání): automatická
   cloudová synchronizace statistik napříč zařízeními (zvažovány
   Supabase/Firebase). Uživatel to zatím nechce řešit.

## Kontext, který je dobré znát
- Hra je primárně pro rodinné/soukromé použití, ale uživatel zvažuje
  veřejné publikování (web, případně App Store) a paralelně se přes tenhle
  projekt učí stavět appky, dělat marketing a pracovat s AI agenty — je to
  pro něj záměrně i cvičný projekt, ne jen produkt.
- Kategorie "Minecraft" obsahuje značkové názvy (Creeper, Enderman,
  Redstone, Wither...) — před jakýmkoli komerčním/veřejným vydáním (hlavně
  App Store) je tohle riziko z hlediska ochranné známky Mojang/Microsoft a
  stálo by za zvážení/úpravu.
- Při úpravách appky dodržovat: žádný build krok, žádné externí závislosti
  (appka musí zůstat jeden samostatný HTML soubor, co jde otevřít odkudkoli).
