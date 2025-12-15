---
date : '2025-08-19'
draft : false
title : 'Projekt'
---
Nejdůležitější a také nejnáročnější částí předmětu ZPC je vybrat si projekt, kterému se budu věnovat celý semestr. Projekt, který bude dostatečně náročný, užitečný a zároveň originální. A ideálně takový, který mi kromě nových znalostí přinese i něco praktického do života.

Položil jsem si tedy otázku: „Co splňuje všechny tyhle parametry?“
A začal jsem brainstormovat. Prošel jsem své koníčky a zájmy (viz stránka O mně), až jsem se dostal k hudbě – konkrétně k elektrické kytaře. Uvědomil jsem si, že právě tady se krásně prolíná strojařina, elektrotechnika i kreativita. A že jde o oblast, kde je stále spousta prostoru pro inovaci.

Další otázka, kterou jsem si položil, zněla: „Co mi tady chybí?“
A odpověď na sebe nenechala dlouho čekat: variabilita a možnost real-time modulace zvuku.

Pro nezasvěcené – k úpravě zvuku se používají kytarové multiefekty. Jsou to fyzické „krabičky“, které analogově nebo digitálně mění čistý zvuk kytary. Mají ale jeden zásadní problém: nastavení lze změnit jen před hraním, ne během samotné hry.
Co když ale chci v průběhu písničky měnit zkreslení, nebo si při sólu pohrát s intenzitou ozvěny? A co teprve ovládat oba parametry současně?
V takovém případě musí kytarista postupně zapínat a vypínat několik efektů, což je nepohodlné a zpomaluje to hru.

A tak jsem našel problém, který stojí za řešení:

Problém: Nelze naráz a plynule měnit nastavení více efektů během hry.

Řešení: Postavit multiefekt, který to umožní.

Víceosý kytarový pedál

Cílem mého projektu je navrhnout a sestavit digitální multiefektový pedál, který umožní kytaristovi ovládat více parametrů zvuku současně a plynule — a to přímo během hraní.

Mechanické tělo pedálu funguje na principu joysticku, který hráč ovládá nohou. Pohyby joysticku v osách X a Y jsou snímány a posílány do řídicí jednotky (Arduina). Ta následně podle těchto hodnot digitálně upravuje zvuk, který putuje z kytary do komba.


{{< figure src="/images/schema.jpg" caption="Schéma zapojení" >}}
































