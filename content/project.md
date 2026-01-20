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

Krok 1 – Vytvoření a testování zvukového obvodu
Cíl první verze: Dostat zvuk z kytary do mikrokontroléru, zpracovat ho a poslat ven.

Rozhodl jsem se, že začnu stavbou hardwaru. Kdybych totiž nedokázal dostat čistý signál z kytary do čipu, celý projekt by skončil dřív, než by začal.

1. Vstupní část: Jak zesílit kytaru
Z kytary vystupuje velice slabý střídavý signál (cca 0,1 V), což je pro digitální zpracování problém. Mikrokontrolér potřebuje signál silnější a hlavně "kladný" (v rozsahu 0 až 3,3 V).

Tuto úpravu naštěstí není těžké vyřešit – stejný princip využívá téměř každý kytarový pedál. Po rešerši zapojení jsem zvolil následující řešení:

Vstupní kondenzátor (10 nF): Odděluje kytaru od stejnosměrného napětí obvodu, ale nechává projít zvuk (střídavou složku).

Virtuální zem (DC Bias): Vytvořil jsem bod s napětím 1,65 V (polovina napětí Teensy). To nám slouží jako "nová nula".

{{< figure src="/images/Zapojeni.jpg" caption="Breadbord prototyp" >}}

2. Mozek operace: Teensy 4.0
Když máme signál připravený, potřebujeme procesor s dostatečným výkonem pro real-time modulaci zvuku (bez zpoždění).

Vybral jsem Teensy 4.0 v kombinaci s Audio Shieldem.

Teensy 4.0: Má obrovský výpočetní výkon (600 MHz), což je pro digitální efekty klíčové.

Audio Shield: Obsahuje hardwarový kodek (SGTL5000), který se stará o převod zvuku na jedničky a nuly a zpět. Navíc k němu existuje skvělá knihovna pro mixování zvuku.

{{< figure src="/images/Teensy.jpg" caption="Teensy s Audoi Sheldem" >}}


3. Výstup: Zpátky do analogového světa
Posledním krokem je úprava signálu pro kytarové kombo. Zde nastává opačný problém než na vstupu – signál z Teensy (Line Out) je příliš silný (cca 3 V) a mohl by vstup komba zahltit nebo zkreslit.

Řešení je prosté: Logaritmický potenciometr (100k Audio). Funguje jako klasické "Volume" kolečko. Zapojil jsem ho jako dělič napětí na výstupu, díky čemuž mohu plynule regulovat hlasitost, která jde do zesilovače.

Krok 2 – Tvorba ovladače
Cíl: Vyvinout dostatečně velký a robustní joystick ovladatelný nohou, který bude posílat přesná data do Teensy.

Druhým pilířem projektu je samotný mechanický pedál. Musí být dostatečně pevný, aby snesl ovládání nohou, a zároveň musí spolehlivě převádět fyzický pohyb na digitální signál.

1. Prvotní koncept: Bezkontaktní snímání
Začal jsem rešerší, jak fungují průmyslové zvětšené joysticky. Zvažoval jsem tři možnosti měření náklonu:

Přímé připojení na potenciometry.

Mechanický převod na klasický malý joystick.

Magnetické měření pomocí Hallových senzorů.

Zpočátku mě nejvíce zaujala třetí možnost. Chtěl jsem se vyhnout potenciometrům kvůli úspoře místa a opotřebení, proto jsem se rozhodl navrhnout pedál využívající magnety a Hallův jev.

Klíčovou částí konstrukce byl mechanismus, který vrací páčku po vychýlení zpět do středu. Navrhl jsem systém několika pružin, inspirovaný velkými joysticky z leteckých simulátorů, který musel být dostatečně kompaktní, aby se vešel do těla pedálu.

{{< figure src="/images/Model_1.png" caption="První model" >}}

2. Prototypování a slepá ulička
Po několika dnech modelování a tisku jsem měl v ruce mechanicky funkční prototyp. Vyladil jsem rozměry pro ideální pohyblivost a otestoval pružinový návratový mechanismus. Následně jsem osadil magnety a senzory a připojil vše k Teensy.

{{< figure src="/images/Pedal_1.jpg" caption="První prototyp pedálu" >}}


Zde jsem však narazil na zásadní problém – kalibrace. Najít kompromis, kdy je konstrukce dostatečně pohyblivá, ale zároveň jsou magnety stále v ideální vzdálenosti od senzorů pro silný signál, se ukázalo jako konstrukčně neúměrně náročné. Signál byl nelineární a nespolehlivý.

Místo zdlouhavého boje s kalibrací jsem se rozhodl pro radikální řez: Redesign.

3. Krok 2.1 – Návrat k mechanice (Redesign)
Myšlenku měření magnetického pole jsem opustil a vrátil se k osvědčené "čisté mechanice". Rozhodl jsem se využít přímo osičky potenciometrů jako hřídele, kolem kterých se mechanismus otáčí.

Jako inspiraci jsem využil schémata klasických analogových joysticků a jeden starší kus jsem pro studijní účely rozebral.

{{< figure src="/images/Model_2.png" caption="Druhý model" >}}

Po dalších dnech modelování a několika nepovedených 3D tiscích byl nový joystick na světě. Potenciometry v něm slouží jako nosný prvek i snímač zároveň. Konstrukce okamžitě prošla zkouškou ovladatelnosti a co je hlavní – snímání hodnot je nyní přesné a lineární.

{{< figure src="/images/Pedal_2.jpg" caption="Druhý prototyp pedálu" >}}

Krok 3 – Software
Cíl: Naprogramovat Teensy tak, aby měnilo zvuk v reálném čase v závislosti na poloze joysticku.

S úspěšně zprovozněným hardwarem nadešla chvíle vše propojit dohromady – zajistit, aby se pohyb nohy na pedálu projevil změnou zvuku kytary. Na první pohled se digitální zpracování signálu (DSP) může zdát jako náročný úkol plný složité matematiky, ale v případě Teensy je opak pravdou.

Pro tento mikrokontrolér existuje skvělý nástroj Teensy Audio Design Tool. Je to grafické prostředí, kde si efekty (zkreslení, reverb, mixéry) skládáte jako stavebnici a propojujete je virtuálními kabely.

Samotná tvorba softwaru se tak s využitím AI asistenta pro doladění detailů stala prací na jeden večer. Paradoxně nejvíce času mi nezabralo programování, ale rozhodování, co vlastně budu modulovat. Mám ovládat míru zkreslení (Distortion), velikost dozvuku (Reverb) nebo snad zpoždění ozvěny (Delay)?

Jelikož je přenastavení softwaru otázkou pár minut, mohu efekty měnit podle nálady.

{{< figure src="/images/Program.png" caption="Programovací prostředí" >}}

Finální úpravy (Next Steps)
Ačkoliv je systém po funkční stránce hotový, před finální prezentací mě čekají ještě poslední detaily, které posunou prototyp blíže k hotovému produktu:

Vizuální design: Sladění barevného provedení pedálu a ovládacích prvků.
Cable Management: Návrh a 3D tisk krytu (housingu) pro skrytí a ochranu kabeláže.

{{< figure src="/images/Krabicka.jpg" caption="Krabička na kabeláž" >}}



Závěr
Závěrem mohu říct, že tento projekt považuji za velmi úspěšný. Nejenže se mi podařilo sestavit a naprogramovat plně funkční prototyp zařízení podle vlastního návrhu, ale především jsem se mnohé naučil.

Kromě prohloubení znalostí v "kutilské" elektrotechnice a 3D tisku jsem si na vlastní kůži vyzkoušel, jak probíhá skutečný inženýrský proces. Pochopil jsem, že projekty se málokdy povedou "na první dobrou", že je potřeba umět zahodit nefunkční koncept a že chyby nejsou selháním, ale nezbytným krokem k funkčnímu řešení.

<!-- Embedded video: finalni_test -->
### Video — finální test

<video controls width="100%" height="480">
	<source src="/images/Finalni_test.mp4" type="video/mp4">
	Your browser does not support the video tag. You can <a href="/images/Finalni_test.mp4">download the video</a> instead.
</video>



