HIDRO ESTUFA CENTRAL CONTROL

Aquest subprojecte s'encarrega del control del confort de la hidroestufa. Hi ha dos dispositius: HE Central Control i HE Central Confort HE Central Control es un ESP32 que s'encarrega del control dels parametres de la hidroestufa i la seva seguretat. HE Central Confort és un ESP32 que s'encarrega del confort actuant sobre el sistema.

La central de control rep la informació dels sensors:
- Sensor temperatura ximeneia (tub de fums)
- Sensor temperatura caldera (part superior hidro-estufa)
- Sensor de pressió circuit. Sensor 5V 0-0,5MPa (0 a 5Bar)

Disposa dels següents actuadors:
- Relé encesa Bomba impulsió primaria (recirculació caldera)
- Relé encesa Bomba impulsió secundaria (circuit radiadors)
- En estudi: Control marxes (3) de la bomba d'impulsió secundaria. Aquest control podria estar en HE-Control-Confort.

El sistema està instal.lat en paral.lel amb un sistema electro-mecanic que engegarà les bombes d'impulsió si s'assoleix certa temperatura (50ºC en caldera per Bomba primaria) i (55ºC en retorn per Bomba secundaria).
En cas de fallida de sistema s'activaran de forma electromecanica les bombes. Els elements mecanics de seguretat supervisen pressió màxima sistema 3Bar (2 valvules una en caldera i altre punt més baix de la instal.lació), pressió mínima del sistema: 1,2Bar (valvula de reompliment automàtica) i temperatura màxima 98ºC (valvula dos vies que obre circuit aigua calenta cap al desaigua i al meteix temps injecta aigua freda al sistema a la pressió del carrer.

Funcionament:
Quan es detecti un increment per sobre 45ºC en el sensor de la ximeneia el sistema deduirà que la caldera ha estat encesa. Ha de canviar l'estat de la caldera de apagada a encesa i monitoritzar la temperatura dels fums.
Quan es detecti una temperatura igual o superior a 50ºC a la caldera automaticament posarà en marxa la Bomba primaria.
Si la temperatura de caldera es superior a 45ºC però la ximenea és inferior a XX (temperatura ambient) activarà una alerta de fallo de sensors del sistema. Possibles fallos: sensor ximeneia o caldera.
Si la presió excedeix els 2bar activarem una alarma visual de sobrepressió
Si la pressió és inferior a 1,2Bar activarem una alarma de pressió insuficient del sistema o (fallo de del sensor o vàlvula de reomplert automàtic). Recomanació mirar manometres mecanics (un al costat caldera) i dos més a la sala de màquines.
Si la pressió excedeix els 2,5Bar activarem una alarma visual i sonora amb indicacions de buidat del circuit de forma manual.
Si la temperatura de caldera es superior a 85ºC (cal ajustar aquesta temperatura de forma empirica) cal activar alarma visual de sobretemperatura.
Si la temperatura de caldera es superior a 95ºC (cal ajustar aquesta temperatura de forma empirica) cal activar alarma sonora i visual de sobretemperatura. Amb indicacións de com fer baixar la temperatura (obrint valvules de purgat perque entri aigua freda a sistema)

Medició de la pressió:
Hem comprat un senso de 0 a 0,5MPa que medeix entre 0 i 5Bar
El sensor s'alimenta a 5V i dona valors de 0,5V per 0bar i 4,5V per 5bar. Com que el ESP només llegeix entre 0 i 2.45V (atenuant a 12db)cal fer un divisor de tensió.
Farem que R1=330 i R2=1K per tant Vout = (VinxR2)/(R1+R2) donarà com a màxim 3.38V per 5bar. Tenim valvules que obren 3bar, per tant no hi arribarà mai. 
Amb aquest divisor tindrem una lectura màxima (3.4bar) de 2.42V a la que en teoria no arribarem mai (si es que no fallen les 2 valvules de 3bar).
A 3bar (maxim) esperarem una sortira de 2.180V, a 1,5bar (normal) llegirem 1.27V
La presició és de 12bits.

