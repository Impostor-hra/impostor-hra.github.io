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

## Plán / co zbývá
1. Git repo + GitHub (propojit stejně jako u sesterského projektu
   `gramaticka-posilovna`) a push.
2. GitHub Pages hosting — soubor je záměrně pojmenovaný `index.html`
   přesně kvůli tomu, aby GitHub Pages fungovalo bez dalšího nastavování.
3. Později: vlastní doména.
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
