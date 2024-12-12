PROJECTE CONTROL HIDROESTUFA

Aquest projecte preten controlar i supervisar el funcionament d'una hidroestufa de llenya.
L'objectiu és facilitar una capa més de seguretat al sistema i optimitzar el funcionament en termes d'efeciciencia i confort.

El projecte es dissenya per a una hidrostufa de llenya instal.lada en una casa unifamiliar als voltants del Montseny (Catalunya).

La marca és FOGOSUR i el model és el "Calella" o "Hidro 23KW"
Les seves caracteristiques son les següents (segons el fabricant):
Potencia Max.	23,0kW
Potencia útil al agua	12,0kW
Potencia nominal al agua	8,0kW
Potencia nominal al ambiente	11,0kW
Rendimiento	82%
Superficie calefactable	500m3
Peso	210kg
Salida de humos	200mm
Dimensiones totales (A-P-H)	990 – 615 – 880mm
Dimensiones hogar (A-P)	670 – 380 mm
Etiqueta energética	A+
URL: https://fogosureco.com/producto/chimenea-c-hidro/

S'ha fet una instal.lació hidraulica amb un circuit primari de 28mm de diametre propulsat per una bomba i conectat a una agulla tèrmica (per a futures ampliacions). 
De l'agulla en surt el circuit secundari amb una altre bomba que impulsa l'aigua a un colector on estan conectats els radiadors.
El circuit disposa d'una valvula anticondensats de 50ºC que evita que es produeixin condensats a l'interior de la caldera.

Proteccions passives:
El sistema disposa d'una vàlvula de sobre pressió (3bar) damunt la propia caldera, un purgador automàtic i una valvula de 2 vies de descàrrega tèrmica. 
Aquesta vàlvula quan l'aigua arriba a 97ºC obre el circuit primari i el deriva cap al desaigua i al mateix temps obre una vàlvula que fa entrar aigua freda de la xarxa al circuit primari.
L'objectiu és evitar un sobreescalfament del sistema en cas de mal funcionament de la bomba o de caiguda d'energia electrica.
També disposa d'una segona vàlvula de sobre pressió (3bar) en el punt més baix del sistema (redundant amb la primera) i una vàlvula de reompliment del sistema en cas de perdua de pressió. 
El sistema es completa amb un vas d'expansió per absorvir les dilatacions del circuit al augmentar la temperatura.

Proteccions actives electromemcàniques:
Per distribuir l'energia calorica generada per la caldera fins als radiadors hi ha dos bombes d'impulsió (una al circuit primari i l'altre al secundari) controlades per dos termointerruptors.
A la sortida de la caldera hi ha un termointerruptor que tanca un circuit al arribar 50ºC i posa en marxa la bomba recirculadora del circuit primari.
TP = "Termostat primari" serveix per engegar la bomba d'impulsió.
BP = "Bomba Primaria" Bomba que fa circular aigua dins la caldera.
A la sortida de l'agulla , hi ha un segon termointerruptor que tanca un circuit al arribar a 55ºC i posa en marxa la bomba d'impulsió als radiadors.
TS = "Termostat Secundari" és el que impulsa l'aigua al circuit de radiadors.
BS = "Bomba Secundaria" Fa circular l'aigua per els circuits dels radiadors.
Si les respectives temperatures baixen de la temperatura de consigna, tanquen les respectives bombes. 

Sensors electronics:
Completen el sistema dos sensors termopar (un conectat a la part superior de la caldera) i l'altre dins de la ximeneia d'extracció de fums.
TC = "Temperatura Caldera" llegeix el valor de la temperatura de l'aigua en el punt més alt de la caldera.
TF = "Temperatura Fums" llegeix el valor de la temperatura dels fums que surten de la caldera.
També hi ha dos sensors digitals que mesuren la temperatura de l'aigua a la entrada del colector dels radiadors i un altre a la sortida del retorn del colector.
TI = "Temperatura Impulsió" Mesura a quina temperatura s'impulsa l'aigua cap als radiadors.
TR = "Temperatura Retorn" Mesura a quina temperatura retorna l'aigua enviada als radiadors.
Per finalitzar hi ha un sensor de pressió que llegeix la pressió del sistema.
PS = "Pressió Sistema" es un transductor situat al costat del vas d'expansió.

Actuadors:
RBP = "Rele Bomba Primaria" Rele en pararl.lel amb el TP que engega la Bomba Primaria.
RBS = "Rele Bomba Secundaria" Relé en paral.lel amb TS que engega la Bomba Secundaria.
RBS1 = "Rele Bomba Secundaria marxa 1" Activa la marxa 1 **
RBS2 = "Rele Bomba Secundaria marxa 2" Activa la marxa 2 **
RBS3 = "Rele Bomba Secundaria mrxa 3" Activa la marxa 3 **
** Cal veure si és factible i si cal.
RA = "Relé Alarma" Relé per activar un avís sonor/lluminós de perill.
SCA= "Servo Control Admisió" Servomotor que pot obrir o ofegar l'entrada d'aire secundaria (posterior a la caldera - provinent del exterior)

Idea: Valorar la possibilitat d'utilitzar un SAI per alimentar les dues bombes i el controlador de seguretat. En cas de fallida electrica, podem tenir lectures per activar alarmes i tancar pas d'aire. EL controlador de confort no li cal, ja que si cau lectricitat els actuadors termics NO s'obriràn i deixan passar aigua a tots els radiadors. La bomba secundaria s'obrirà automàticament amb el termo interruptor i disisparà l'escalfor a tots els radiadors...

Objectius:
Creació d'un sistema de control per supervisar els valors del sistema, avisar de mal funcionaments i optimitzar el rendiment.
Per motius de seguretat dividirem el sistema amb dos controladors separats. 

Controlador de seguretat:
Basat en un ESP32 i amb funcionament autonom estarà conectat fisicament a TC, TF, PS i controlarà RBP, RA i SCA. Pot permetre el control RBP i SCA de forma remota si no està en alarma. En cas d'alarma només permetrà control manual usuari avançat. En cas d'alarma pot passar a controlar el Controlador de confort ordenant obrir tots els radiadors

Controlador de confort:
Basat en un ESP32 i amb funcionament autonom estarà connectat fisicament a TI, TR i controlarà RBS (cal veure si cal RBS1, RBS2, RBS3) Podrà accedir a les lectures de TC, TF, PS i al estat de RBP (relé Bomba Primaria)

Llegint els valors de TF deduirem si la caldera està en funcionament. Qualsevol valor superior a 45ºC serà considerat que la caldera està en funcionament i per tant hi ha presencia de foc/brasa en el seu interior. 
Els valors de TF poden servir per calibrar ajustar els nivells del SCA, per optimitzar el consum de llenya.
Idea: Podem mostrar una icona de flames en el display de control.

Seguretat del sistema:
De forma local el controlador de seguretat ha de funcionar amb indepencia de si està conectat a Home Assistant o la xarxa wifi

Llegint els valors de TC sabrem la temperatura de l'aigua de la caldera. Un valor superior a 50ºC hauria de donar la ordre d'engegar el el RELE de la BP (Bomba Primaria). Aquest valor hauria de ser lleugement inferior al d'activació del Termostat TP per poder tenir constancia que s'ha activat. Al deixar un valor superior al TP, aquest s'activa en cas que el sistema de control digital falli. Tant el RBP com el TP han d'estar conectats en paral.lel, d'aquesta forma es pot activar la BP des de qualsevol dels dos.

Si el valor de TC passa dels 85ºC hauriem d'indicar una pre-alerta d'excés de temperatura de la caldera. Idea: Podem mostrar una icona de no posar més llenya al foc i indicador de tancar el tir frontal.
Si el valor de TC passa dels 90ºC hauriem d'activar una alerta de sobretemperatura a la caldera.
Si el valor de TC passa dels 95ºC hauriem d'activar una alerta sonora i sol.licitar l'activació d'una maniobra per refredar: Baixar a la sala de màquines i obrir els circuits purgadors perque el sistema de re-ompliment faci entrar aigua freda al sistema.
Si el valor de TC passa dels 99ºC activar alerta sonora i sol.licitar apartar-se de la caldera per risc d'explosió.

Llegint els valors de PS podem coneixer la pressió del sistema.

Si el valor de PS és inferior a 0,5Bar cal activar un avís de pressió insuficient al circuit. Indicar que cal revisar el circuit: Assegurar que les aixetes de pas estan obertes. Idea: Mostrar icona de no posar en marxa la caldera.
Si el valor de PS és inferior a 0,5Bar i TC és superior a 50ºC i/o TF és superior a 50ºC (caldera en funcionament i poca pressió del sistema) activar els avisos anteriors i també avís sonor. 
Si el valor de PS és superior a 2,0Bar activar avís de pressió alta del circuit.
Si el valor de PS és superior a 2,5Bar activar avís de pressió alta de circuit i avís sonor. Sol.licitar maniobra de alleujament de pressió: Baixar a la sala màquines i obrir els circuits purgadors per alleugerir la pressió.
Si el valor de PS passa dels 2,9Bar activar alerta sonora i sol.licitar apartar-se de la caldera per risc d'explosió.

Idea: En el cas que TC passi de 95 i/o PS de 2,5 podriem forçar una maniobra automàtica de tancar el servo SCA obstruint completament l'entrada secundaria d'aire per intentar ofegar el foc del tot. Aquesta maniobra serviria per en cas de no atenció (per no estar a la casa, o per no estar conscients (dormint) intentar parar el funcionament de la caldera.

Autoregulació de la combustió:
Combinant les dades del segon sistema de control i sempre que no es donin situacions de perill, podem donar el control del SCA externament, ja sigui a un control manual o automatic de regulació de la combustió.

Controlador de confort:
Estarà fiscament connectat a TI i TR, RBS (possibilitat de control de marxes RBS1, RBS2, RBS3), i al control de la placa d'actuadors termics. Rebrà la informació dels valors de TC i TF i dels sensors de temperatura i presencia de les habitacions i temperatura exterior.
L'objectiu serà distribuir l'escalfor generada per la caldera de forma optima.
Habitacions:
  - Rebedor
  - Cuina
  - Menjador
  - Sala d'estar
  - Bany P0
  - Despatx
  - Escala
  - Distribuidor
  - Suite
  - Bany Suite
  - Habitació 1 i 2 (van juntes amb els radiadors)
  - Bany superior
  - Estudi

Podem distingir diversos modes de funcionament:
- Automàtic
- Manual: indicant zones i temperatures

- Confort total: S'intentarà mantenir tota la casa a la temperatura de confort (preestablerta).
- Priorització de una o diverses zones: Es podra determinar de forma dinàmica quines zones tenen prioritat per intentar aconseguir la temperatura de confort en el temps mes breu.

En funció de les hores del dia: 
Mati: Sala estar, menjador, cuina, dormitoris i banys.
Dia: Sala estar, menjador, bany P0, cuina, estudi i despatx
Vespre: Sala estar, menjador, bany P0, cuina, dormitoris i banys (tot excepte estudi, despatx, rebedor, distribuidor, escala)
Nit: Dormitoris i banys

Aixo combinat amb els sensors de presencia. Si detectem presencia més de X temps, engeguem la zona com prioritaria








