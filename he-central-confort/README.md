HIDRO ESTUFA Central Confort

Aquest subprojecte s'encarrega del control del confort de la hidroestufa. Hi ha dos dispositius: HE Central Control i HE Central Confort
HE Central Control es un ESP32 que s'encarrega del control dels parametres de la hidroestufa i la seva seguretat. 
HE Central Confort és un ESP32 que s'encarrega del confort actuant sobre el sistema.

Aquesta central està fisicament conectada als següents dispositius:
TI = "Temperatura Impulsió" Sensor Dallasque mesura a quina temperatura s'impulsa l'aigua cap als radiadors.
TR = "Temperatura Retorn" Sensor Dallas que mesura a quina temperatura retorna l'aigua enviada als radiadors.
PR = "Placa reles" Placa de reles d'estat solid que controlen els actuadors termics (valules) d'obertura dels diferents radiadors de la casa. 
També rep informació sobre la temperatura de les diferents estancies de la casa mitjançant Home Assistant

En la instal.lació del col·lector (3+12) hi ha els següents circuits:
0 - Connexió sensors Dallas impuls i retorn TI i TR
1 - Sortida circuit per ACS*
2 - Radiador Despatx (P0)
3 - Radiador Escala
4 - Radiador Cuina i Menjador (van seriats) (P0)
5 - Radiador Rebedor (P0)
6 - Radiador Menjador ? Cal identificar
7 - Radiador Sala estar (2 radiadors seriats)
8 - Tovallolero Bany (P0)
9 - Radiador Suite (P1)
10 - Radiador Estudi (2 radiadors seriats) (P1)
11 - Radiador habitacions (2 radiadors en 2 habitacions seriats) (P1)
12 - Tovallolero Bany Suite (P1)
13 - Radiador passadis (P1)
14 - Tovallolero Bany P1 (P1)
* Està previst que si hi ha excedents de calor es pugui enviar a un termo electric/acumulador amb un serpentí.
  
Els 14 circuits estan controlats per un actuador eletro-termic NO que quan rep tensió tanca el circuit. D'aquesta manera en cas de fallada electrica/software els actuadors obren tots els circuits evitant que la hidroestufa es sobre-escalfi.

Materials:
- Sonda temperatura Dallas (DS18B20)
- Vaina d'inmersió
- Placa de relés d'estat solid:
    www.iomug.com 16 channel I2C Interface Solid State Relay Module basat en PCF8575
- Suport per rail DRG-01
- ESP32

Funcionament:

Donat el sistema de la Hidro Estufa es poden donar cinc situacions:
A - HE No engegada
B - Potencia equilibrada amb demanda
C - Potencia excesiva per demanda
D - Potencia insuficient per demanda
E - Mode ECO (estalvi energia)
F - Fallada/Avaria de funcionament
G - Alarma Sobretemperatura
H - Mode manual (manteniment/test)

A - HE No engegada
No es detecta temperatura en la hidro estufa suficient per engegar el sistema.
En aquest cas la bomba secundaria (Impulsió als radiadors) BS hauria d'estar parada. Per allargar la vida dels actuadors termics i estalviar energia tots els actuadors estaran en mode OFF sense tensió (circuit radiadors oberts)

B - Potencia equilibrada amb demanda
Mode ideal de funcionament: el sistema esta en equilibri. La energia produida és igual a la energia disipada/consumida. La casa (cada habitació) està a la temperatura desitjada. La temperatura d'impulsió es manté estable i el diferencial entre impulsió i retorn també.
Suposo que les temperatures d'impulsió i retorn aniràn en funció de la temperatura externa i la temperatura de confort asiganda a cada estancia.
En aquest cas no s'han de produir canvis en els actuadors ni en les bombes. Es podria indicar en el panel de control que el funcionamnt és optim.

C - Potencia excesiva per demanda
S'han assolit les temperatures de confort consignades i el diferencial entre TI i TR baixa i va augmentant TI
Accions:
- Reduir el tir secundari de Central de Control de forma progresiva en el temps (cal fer-ho de forma suau).
- Indicar al display que cal reduir el tir de forma manual.
  Si un cop el tir secundari es al minim programat cal:
    - Anar desactivant els actuadors termics (obrint circuits per fer circular aigua calenta als radiadors) seguint un ordre de prioritats per potencia excesiva. Es farà de forma sequencial en funció de unes prioritats definides.
      Proposta de prioritats (sempre que no es trobin ja oberts): (On menys molesti una sobretemperatura)
      - Sortida ACS
      - Radiador Escala
      - Radiador Passadis P1
      - Radiador Rebedor P0
      - Tovallolero Bany P1
      - Tovallolero Bany Suite P1
      - Tovallolero Bany P0
      - Radiador Estudi P1
      - Radiador Cuina Menjador P0
      - Radiador Despatx
      - Radiador habitacions
      - Radiador Suite
      - Radiador sala estar (aquest es l'ultim ja que també hi ha la calentor directa de la hidroestufa)

D - Potencia insuficient per demanda
Si la HE esta en funcionament i (amb independencia de que s'hagi assolit la temperatura de confort)
Accions: 
- Obrir el tir secundari de Central de Control de forma progresiva en el temps (cal fer-ho de forma suau).
- Indicar al display que cal obrir el tir de forma manual o si ja està obert procedir a una recàrrega de combustible.
  Si un cop el tir secundari està obert al màxim cal:
      Anar activant els actuadors termics (tancant circuits apagar radiadors) seguint un ordre de prioritats per potencia insuficient. Es farà de forma sequencial en funció de unes prioritats definides i del mode de la casa/presencia.
MODE DIA
    Proposta de prioritats mode dia (sempre que no es trobin ja tancats): (On menys molesti una baixa temperatura)
      - Sortida ACS
      - Radiador Escala
      - Radiador Passadis P1
      - Radiador Rebedor P0
      - Tovallolero Bany P1
      - Tovallolero Bany Suite P1
      - Tovallolero Bany P0
      - Radiador habitacions
      - Radiador Suite
      - Radiador Estudi P1
      - Radiador Despatx
      - Radiador Cuina Menjador P0
      - Radiador sala estar

  MODE NIT
    Proposta de prioritats mode dia (sempre que no es trobin ja tancats): (On menys molesti una baixa temperatura)
      - Sortida ACS
      - Radiador Estudi P1
      - Radiador Rebedor P0
      - Radiador Despatx
      - Radiador Escala
      - Tovallolero Bany P0
      - Radiador Cuina Menjador P0
      - Radiador sala estar
      - Radiador Passadis P1
      - Tovallolero Bany P1
      - Tovallolero Bany Suite P1
      - Radiador habitacions
      - Radiador Suite
 * Tant en Mode DIA com en Mode NIT si s'activa el sensor de presencia mes de x minuts tindrà preferencia i anirà al final de l'escala. En cas de competencia entre zones amb presencia s'utilitzaràn les prioritats (p.e. Mode NIT Hi ha algu al Estudi i també a la Suite -> S'apagarà bans l'estudi i després la Suite.

G - Alarma Sobretemperatura:
La temperatura d'impulsió TI és superior a ?? 90ºC? (cal determinar aquest valor de forma experimental). TI: Temperatura màxima de seguretat (aquest valor experimental s'ha de poder configurar en programació en el propi ESP32)
- Desactivar tots els actuadors termics (obrir tots els circuits)
- Activar la BS (Bomba secundaria) a la màxima potencia. Central Control ha de rebre un avís per fer-ho.
- Tancar el tir secundari de Central de Control.
- Donar activació a un avís visual, sonor i missatge de Telegram.


NOTA: Estaria bé fer un indicador gràfic (o texte només de moment) indicant les zones i la seva situació de confort (amb blaus per menys temperatura i vermells per a excés). 
En el projecte solar, s'utilitzaràn aquestes prioritats per enviar calor a les diferents zones amb els calefactors. L'objectiu és escalfar la zona mes freda evitant que la casa baixi de 18ºC aprofitant excdents solars..



