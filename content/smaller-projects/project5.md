---
date : '2025-08-19'
draft : false
title : 'Elektrotechnika a řízení'
---
Poslední miniprojekt sloužil jako seznámení se se základy elektrotechniky a mikrokontrolérů.
Během výuky jsme absolvovali několik přednášek a cvičení, na kterých jsme se postupně naučili pracovat s nejčastěji používanými součástkami, poznali jejich zapojení a využití. Poté jsme se dostali k mikrokontrolérům – k tomu, jak fungují a k čemu je lze využít.
Vše jsme si samozřejmě i prakticky vyzkoušeli.

Na závěr jsme dostali zadání: vytvořit vlastní jednoduchý projekt s využitím Arduina a dostupných komponent, abychom si ověřili a upevnili nové znalosti.
Já jsem se rozhodl vytvořit jednoduchou postřehovou hru, jejímž cílem je co nejrychleji zmáčknout tlačítko odpovídající rozsvícené diodě.

{{< figure src="/images/Elektro_zapojeni_1.png" caption="Schéma zapojení hry" >}}

Zapojení hry není nijak složité – jde v podstatě jen o systém diod, tlačítek a rezistorů.
Zajímavější částí projektu byl program. Při tvorbě kódu jsem se snažil ho co nejvíce optimalizovat – využíval jsem cykly, podmínky a funkce tak, aby byl přehledný, stručný a v případě potřeby snadno rozšiřitelný.

{{< figure src="/images/Elektro_kod.png" caption="Ukázka kódu" >}}

Projekt svůj cíl splnil. Seznámil jsem se se základy elektrických obvodů a vyzkoušel si navrhnout i realizovat vlastní jednoduché zařízení.

{{< figure src="/images/elektro_funkcni.png" caption="Hra" >}}

Odkaz na Tinkercad kde se projek nachází
https://www.tinkercad.com/things/0ciniP9VUEv-hra-na-postreh/editel?returnTo=%2Fdashboard%2Fdesigns%2Fall

Celý kód :

// Piny
int ledPins[3] = {2, 3, 4};
int buttonPins[3] = {8, 9, 10};

void setup() {
  Serial.begin(9600);

  // Nastavení pinu
  for (int i = 0; i < 3; i++) {
    pinMode(ledPins[i], OUTPUT);
    pinMode(buttonPins[i], INPUT_PULLUP); 
  }
}

void loop() {
  for (int i = 0; i < 3; i++) {
    digitalWrite(ledPins[i], LOW); //Zhasni
  }

  delay(random(1000, 5000));  //  čekání

  int sviti = random(0, 3);   //  náhodnou LED
  digitalWrite(ledPins[sviti], HIGH);
  unsigned long start = millis(); //Start času

  int tlacitko = -1; //tlačítková proměná
  
  while (tlacitko == -1) { //než se zmáčkne tlačítko
    for (int i = 0; i < 3; i++) {
      if (digitalRead(buttonPins[i]) == LOW) {
        tlacitko = i; //zmačknu tlačítko - dám no na Low - přepíšu 
        break;
      }
    }
  }

  unsigned long reakce = millis() - start; //vysledný čas

  digitalWrite(ledPins[sviti], LOW); // zhasni LED

  if (tlacitko == sviti) {
    Serial.print("Správně! Reakční čas: ");
    Serial.print(reakce);
    Serial.println(" ms");
  } else {
    Serial.print("Špatné tlačítko! Správná LED byla číslo ");
    Serial.println(sviti + 1);
  }

  //dokud tlačítko nepustí
  while (digitalRead(buttonPins[tlacitko]) == LOW) { }

  delay(1000); //  pauza 
}



























