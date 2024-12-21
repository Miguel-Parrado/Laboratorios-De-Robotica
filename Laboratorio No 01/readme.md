
# Laboratorio 01 de Robótica.
### Descripción:

Para la realización idonea del laboratorio, se realizó lo siguiente:

- Se elaboró el diseño de un gripper sencillo, que pudiera sostener un marcador como actuador final. Este modelo se hizo en software CAD, Se obtuvieron los datos relevantes como su masa y sus momentos de inercia, y se imprimió en 3d.

- Se realizó un modelo en software CAD para el Objeto de trabajo [Workobject], que contiene el logotipo de SUZUKI, y las iniciales de los nombres los integrantes del equipo.

- Los modelos en CAD fueron importados a RobotStudio, y allí se configuraron como tool1 y como wobj1.

- Se ubicó cada punto según la ruta que recorreria la punta del gripper tool1 (tcp). a cada punto se le ajustó la pose, al igual que la velocidad a la que el robot llegaría al punto, y la zona de error permitida.

- Con ayuda de el editor de código RAPID con el que viene RobotStudio, se hicieron las siguientes subrutinas, dependientes de dos entradas digitales: 
	1.  Rutina para ubicar al robot en cambio de herramienta. Se puede acceder a esta rutina cuando DI02 sea 1, y cuando el robot no esté decorando el pastel. Esta rutina se acaba cuando cambien tanto DI01 o DI02 de 0 a 1. Al momento de estar en la rutina, DO02 se enciende, y al salir de esta, la salida digital se apaga.
	2. Rutinas para decorar el pastel. Estas se pueden acceder cuando el robot esté en la posición base [HOME], y cuando DI01 sea 1. Estas rutinas hacen que DO01 se encienda, y al momento de terminar esta misma salida se apaga. Cada que se complete una rutina, al pulsar D101 pasará a la siguiente, cumpliendo el siguiente orden:
		+ Dibujar el Logotipo (S de SUSUKI). 
		+ Dibujar las letras (SUZUKI).
		+ Dibujar las iniciales (LAM).
- Se comprobó la funcionalidad del programa, mediante simulaciones realizadas en RobotStudio.
- Para el laboratorio, Se instaló la herramienta al flanche del efector final. Se transfirió el programa al Flexpendant, y se configuró al robot para que tomara un tablero como workobject.
- Para la primera prueba, se ubicó el tablero con la siguiente pose:
	+ X = 320 mm|| Y = 375 mm || Z = 206 mm.
	+ Rx = 0° || Ry = 0°|| Rz = -90°.
	+ Lo anterior según el marco de referencia World (Ubicado en el centro de la base del robot)
- Para la segunda prueba, se ubicó el tablero con la siguiente pose:
	+ X = 330 mm|| Y = 375 mm || Z = 270 mm.
	+ Rx = 0° || Ry = -10°|| Rz = -90°.
	+ Lo anterior según el marco de referencia World (Ubicado en el centro de la base del robot).

### Flujograma:

![](https://github.com/Miguel-Parrado/Laboratorios-De-Robotica/blob/main/Laboratorio%20No%2001/Imagenes/Flujograma3.png?raw=true)

### Plano de Planta:
![](https://github.com/Miguel-Parrado/Laboratorios-De-Robotica/blob/main/Laboratorio%20No%2001/Imagenes/Plano_de_planta.jpg?raw=true)

### Funciones:
Estas fueron:
+ **MoveL [AlPunto], [Velocidad], [Zona], [Herramienta]**
	Esta funcion mueve el TCP de manera lineal desde su posición actual hasta un punto especificado.
	- AlPunto describe el punto en el espacio al que el TCP llega
	- Velocidad describe que velocidad debe llevar el robot.
	- Zona describe el radio en el que el robot puede tomar atajos para llegar al siguiente punto, o si no toma atajos.
	 Herramienta describe que herramienta se usa para llegar al punto.

+ **MoveJ [AlPunto], [Velocidad], [Zona], [Herramienta]**
	 Esta funcion mueve al TCP en una ruta de arco circular, calculada según otros dos puntos.
	- AlPunto1, AlPunto2 describen el punto en el espacio al que el TCP llega
	- Velocidad describe que velocidad debe llevar el robot.
	- Zona describe el radio en el que el robot puede tomar atajos para llegar al siguiente punto, o si no toma atajos.
	 Herramienta describe que herramienta se usa para llegar al punto.

+ **MoveC [AlPunto1], [AlPunto2], [Velocidad], [Zona], [Herramienta]**
	 Esta funcion mueve el tcp a un punto descrito, sin indicar que trayectoria se sigue. Se usa para garantizar un movimiento rápido y eficiente.
	- AlPunto1,  describe el punto que funciona como limitante para calcular el arco de circulo.
	- AlPunto2 Es el punto al cual el TCP llega.
	- Velocidad describe que velocidad debe llevar el robot.
	- Zona describe el radio en el que el robot puede tomar atajos para llegar al siguiente punto, o si no toma atajos.
	 Herramienta describe que herramienta se usa para llegar al punto.

+ **IF [Condición] THEN (código)**
	Esta función ejecuta un código, solamente si se cumple la condición.
	 - Condición describe la lógica que se debe garantizar para que el código se ejecute.
	 La función siempre termina con un ENDIF

+ **ELSE (codigo)**
	Esta función siempre recide dentro de el bloque IF THEN. Si la condición que existe en el IF THEN falla, se ejecuta el código que contiene ELSE; de lo contrario este código no se ejecuta.
+ WHILE [Condición] DO (código)
 	Esta función ejecuta un código que se repite en bucle, solamente si se cumple la condición.
	Todo ciclo WHILE termina con un ENDWHILE.

+ **BitOr ([Dato1],[Cato2])**
	 Esta función realiza la operación O Inclusivo entre 2 datos de tipo byte, y retorna el resultado de la operación.
	 - Dato1 describe un tato cualquiera de tipo byte
	 - Dato2 describe un tato cualquiera de tipo byte

+ **SetDO [SalidaDigital], [Bit]**
	 Esta función prende o apaga la salida digital que se le indique.
	 - SalidaDigital describe el nombre de la salida a la cual se le quiere prender o apagar.
	 - Bit describe el valor para prendido [1] o apagado [0]

+ **GOTO [Etiqueta]**
	 Esta instrucción salta a otra línea, indicada por una etiqueta.
	 - Etiqueta describe el nombre de la linea a la cual GOTO salta.

### Diseño de Herramienta:
![](https://github.com/Miguel-Parrado/Laboratorios-De-Robotica/blob/main/Laboratorio%20No%2001/Imagenes/Soporte_Marcador.jpg?raw=true)

### Código RAPID:

    MODULE MainModule
	TASK PERS tooldata tool1:=[TRUE,[[114.761,0,121.316],[0.92388,0,0.382683,0]],[0.102,[40.951,0.031,48.697],[1,0,0,0],0.000409594,0.000397332,0.000024867]];
	TASK PERS wobjdata wobj1:=[FALSE,TRUE,"",[[320,375,206],[0.707106781,0,0,-0.707106781]],[[-0.000029802,0,0],[1,0,0,0]]];
	CONST robtarget p10:=[[375,253.97,307.45],[0.0922913,-0.701057,0.70106,0.0922846],[0,0,-1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p20:=[[319.32,457.06,-0.01],[0.000024762,-0.707105,0.707108,0.000008947],[0,-1,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p30:=[[376.93,412.71,-0.01],[0.000024723,-0.707105,0.707108,0.000008983],[-1,0,-1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p40:=[[405.19,430.83,-0.01],[0.000024788,-0.707105,0.707108,0.000009002],[-1,0,-1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p50:=[[435.53,444.17,-0.01],[0.000024863,-0.707105,0.707108,0.000009032],[-1,0,-1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p60:=[[361.32,501.6,0],[0.000024791,-0.707105,0.707108,0.000009187],[0,-1,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p70:=[[365.67,504.3,0],[0.00002499,-0.707105,0.707108,0.00000941],[0,-1,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p80:=[[392.39,484.23,0],[0.000024911,-0.707105,0.707108,0.000009552],[-1,0,-1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p90:=[[415.68,477.56,0],[0.000025018,-0.707105,0.707108,0.000009586],[-1,0,-1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p100:=[[436.94,484.81,0],[0.000025161,-0.707105,0.707108,0.000009663],[-1,0,-1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p110:=[[377.01,530.93,0],[0.000025204,-0.707105,0.707108,0.000009697],[-1,0,-1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p120:=[[351.35,513.9,0],[0.000025448,-0.707105,0.707108,0.000010038],[0,-1,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p130:=[[319.06,498.19,-0.01],[0.000025575,-0.707105,0.707108,0.000010287],[0,-1,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p140:=[[393.74,441.05,-0.01],[0.000025705,-0.707105,0.707108,0.000010427],[-1,0,-1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p150:=[[388.63,438.41,-0.01],[0.000025796,-0.707105,0.707108,0.000010464],[-1,0,-1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p160:=[[361.37,459.35,-0.01],[0.000025931,-0.707105,0.707108,0.000010624],[0,-1,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p170:=[[339.98,465.44,-0.01],[0.000026012,-0.707105,0.707108,0.000010746],[0,-1,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p180:=[[319.44,456.8,-0.01],[0.000026075,-0.707105,0.707108,0.000010888],[0,-1,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p190:=[[118.2,378.87,0],[0.000004411,-0.707104,0.707109,-0.000004444],[0,-1,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p200:=[[159.94,378.87,0],[0.000004267,-0.707104,0.70711,-0.000004327],[0,-1,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p210:=[[155.69,392.49,0],[0.000003988,-0.707104,0.70711,-0.000004005],[0,-1,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p220:=[[141.25,403.1,0],[0.00000377,-0.707104,0.70711,-0.000003829],[0,-1,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p230:=[[103.8,407.87,0],[0.000003699,-0.707103,0.70711,-0.000003633],[0,-1,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p240:=[[68.08,404.15,0],[0.000003395,-0.707103,0.70711,-0.000003343],[0,-1,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p250:=[[52.56,393.34,0],[0.000003248,-0.707103,0.707111,-0.000003177],[0,-1,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p260:=[[48.45,382.33,0],[0.000003104,-0.707103,0.707111,-0.000003021],[0,-1,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p270:=[[51.88,364.98,0],[0.000002829,-0.707103,0.707111,-0.000002808],[0,-1,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p280:=[[67.95,353.81,0],[0.000002332,-0.707102,0.707111,-0.000002569],[0,-1,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p290:=[[116.26,346.91,0],[0.000001897,-0.707102,0.707112,-0.000002234],[0,-1,1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p300:=[[121.43,340.7,0],[0.000001525,-0.707102,0.707112,-0.000001939],[0,-1,1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p310:=[[116.98,334.94,0],[0.000001457,-0.707102,0.707112,-0.000001885],[0,-1,1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p320:=[[106.83,333.7,0],[0.000001296,-0.707101,0.707112,-0.00000179],[0,-1,1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p330:=[[97.58,334.94,0],[0.000001099,-0.707101,0.707112,-0.000001685],[0,-1,1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p340:=[[92.69,338.66,0],[0.000000755,-0.707101,0.707113,-0.000001466],[0,-1,1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p350:=[[90.76,342.46,0],[0.000000488,-0.707101,0.707113,-0.000001337],[0,-1,1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p360:=[[47.53,342.08,0],[0.000000423,-0.707101,0.707113,-0.000001284],[0,-1,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p370:=[[55.14,324.16,0],[0.00000021,0.7071,-0.707113,0.000000973],[0,-1,1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p380:=[[72.43,315.56,0],[0.000000404,0.7071,-0.707114,0.000000838],[0,-1,1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p390:=[[106.76,312.33,0],[0.000000831,0.7071,-0.707114,0.000000661],[0,-1,1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p400:=[[140.86,316.44,0],[0.000001132,0.707099,-0.707114,0.000000512],[0,-1,1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p410:=[[158.33,326.34,0],[0.0000014,0.707099,-0.707114,0.000000337],[0,-1,1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p420:=[[164.63,344.52,0],[0.000001598,0.707099,-0.707115,0.000000257],[0,-1,1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p430:=[[159.37,359.61,0],[0.000001893,0.707099,-0.707115,0.000000037],[0,-1,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p440:=[[144.49,369.66,0],[0.000002008,0.707099,-0.707115,-0.000000064],[0,-1,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p450:=[[92.49,377.23,0],[0.000002258,0.707098,-0.707115,-0.000000341],[0,-1,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p460:=[[87.26,381.9,0],[0.000002425,0.707098,-0.707115,-0.000000507],[0,-1,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p470:=[[91.37,387.14,0],[0.000002655,0.707098,-0.707116,-0.00000073],[0,-1,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p480:=[[101.85,388.14,-0.01],[0.000002866,0.707098,-0.707116,-0.000000841],[0,-1,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p490:=[[115.39,385.48,-0.01],[0.000003155,0.707098,-0.707116,-0.000001015],[0,-1,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p500:=[[118.1,382.55,-0.01],[0.000003403,0.707097,-0.707116,-0.000001178],[0,-1,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p510:=[[118.98,378.56,-0.01],[0.00000344,0.707097,-0.707116,-0.000001206],[0,-1,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p520:=[[174.61,405.83,50],[0.000004205,0.707097,-0.707117,-0.000001889],[0,-1,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p530:=[[174.61,405.83,0],[0.000004765,0.707096,-0.707117,-0.000002463],[0,-1,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p540:=[[174.61,346.37,0],[0.000004797,0.707096,-0.707117,-0.000002519],[0,-1,1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p550:=[[184.28,322.44,0],[0.000005168,0.707096,-0.707118,-0.000002754],[0,-1,1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p560:=[[208.86,313.6,0],[0.000005371,0.707096,-0.707118,-0.000002831],[0,-1,1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p570:=[[232.14,312.85,0],[0.000005521,0.707096,-0.707118,-0.000002947],[0,-1,1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p580:=[[254.33,314.98,0],[0.00000566,0.707095,-0.707118,-0.000003052],[0,-1,1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p590:=[[277.61,326.38,0],[0.000006251,0.707095,-0.707119,-0.000003598],[0,-2,1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p600:=[[283.5,348.31,0],[0.000006465,0.707095,-0.707119,-0.000003786],[0,-1,1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p610:=[[283.5,405.55,0],[0.000006646,0.707095,-0.707119,-0.000003896],[0,-1,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p620:=[[247.68,405.55,0],[0.000006852,0.707094,-0.707119,-0.000004051],[0,-1,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p630:=[[247.68,350.54,0],[0.000006744,0.707094,-0.707119,-0.000004175],[0,-1,1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p640:=[[246.23,343.61,0],[0.000006901,0.707094,-0.707119,-0.000004293],[0,-1,1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p650:=[[241.58,338.81,0],[0.000006999,0.707094,-0.707119,-0.000004377],[0,-1,1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p660:=[[229.47,336.11,0],[0.000007159,0.707094,-0.70712,-0.000004537],[0,-1,1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p670:=[[216.6,338.8,0],[0.000007469,0.707094,-0.70712,-0.000004683],[0,-1,1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p680:=[[211.79,345.04,0],[0.00000755,0.707094,-0.70712,-0.000004779],[0,-1,1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p690:=[[210.15,352.28,0],[0.0000076,0.707094,-0.70712,-0.000004876],[0,-1,1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p700:=[[210.15,405.62,0],[0.000007673,0.707093,-0.70712,-0.000005002],[0,-1,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p710:=[[174.11,405.62,0],[0.000007892,0.707093,-0.70712,-0.000005121],[0,-1,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p720:=[[296.91,405.62,50],[0.000008838,0.707093,-0.707121,-0.000005573],[0,-1,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p730:=[[296.91,405.62,0],[0.000009503,0.707093,-0.707121,-0.00000627],[0,-1,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p740:=[[296.91,383.52,0],[0.000009553,0.707093,-0.707121,-0.000006309],[0,-1,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p750:=[[348.17,383.51,0],[0.000006518,0.707092,-0.707121,-0.000008573],[0,-1,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p760:=[[292.88,333.25,0],[0.000005912,0.707092,-0.707122,-0.00000938],[0,-2,1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p770:=[[292.88,315.14,0],[0.000006028,0.707092,-0.707122,-0.000009528],[0,-2,1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p780:=[[394.25,315.16,0],[0.000019101,-0.707103,0.707111,0.000010055],[-1,0,-1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p790:=[[394.25,336.44,0],[0.000019168,-0.707103,0.707111,0.000010101],[-1,0,-1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p800:=[[343.12,336.44,0],[0.000020091,-0.707103,0.707111,0.000010545],[0,-1,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p810:=[[395.72,385.98,0],[0.00002012,-0.707103,0.707111,0.000010754],[-1,0,-1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p820:=[[395.72,404.87,0],[0.000020222,-0.707103,0.707111,0.000010835],[-1,0,-1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p830:=[[296.83,404.87,0],[0.000020436,-0.707103,0.707111,0.000011061],[0,-1,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p840:=[[405.802,405.83,50],[0.000004205,0.707097,-0.707117,-0.000001889],[0,-1,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p850:=[[405.802,405.83,0],[0.000004205,0.707097,-0.707117,-0.000001889],[0,-1,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p860:=[[405.802,346.37,0],[0.000004797,0.707096,-0.707117,-0.000002519],[0,-1,1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p870:=[[415.472,322.44,0],[0.000005168,0.707096,-0.707118,-0.000002754],[0,-1,1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p880:=[[440.052,313.6,0],[0.000005371,0.707096,-0.707118,-0.000002831],[0,-1,1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p890:=[[463.332,312.85,0],[0.000005521,0.707096,-0.707118,-0.000002947],[0,-1,1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p900:=[[485.522,314.98,0],[0.00000566,0.707095,-0.707118,-0.000003052],[0,-1,1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p910:=[[508.802,326.38,0],[0.000006251,0.707095,-0.707119,-0.000003598],[0,-2,1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p920:=[[514.692,348.31,0],[0.000006465,0.707095,-0.707119,-0.000003786],[0,-1,1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p930:=[[514.692,405.55,0],[0.000006646,0.707095,-0.707119,-0.000003896],[0,-1,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p940:=[[478.872,405.55,0],[0.000006852,0.707094,-0.707119,-0.000004051],[0,-1,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p950:=[[478.872,350.54,0],[0.000006744,0.707094,-0.707119,-0.000004175],[0,-1,1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p960:=[[477.422,343.61,0],[0.000006901,0.707094,-0.707119,-0.000004293],[0,-1,1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p970:=[[472.772,338.81,0],[0.000006999,0.707094,-0.707119,-0.000004377],[0,-1,1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p980:=[[460.662,336.11,0],[0.000007159,0.707094,-0.70712,-0.000004537],[0,-1,1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p990:=[[447.792,338.8,0],[0.000007469,0.707094,-0.70712,-0.000004683],[0,-1,1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p1000:=[[442.982,345.04,0],[0.00000755,0.707094,-0.70712,-0.000004779],[0,-1,1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p1010:=[[441.342,352.28,0],[0.0000076,0.707094,-0.70712,-0.000004876],[0,-1,1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p1020:=[[441.342,405.62,0],[0.000007673,0.707093,-0.70712,-0.000005002],[0,-1,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p1030:=[[405.302,405.62,0],[0.000007892,0.707093,-0.70712,-0.000005121],[0,-1,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p1040:=[[530.47,405.7,50],[0.000020834,-0.707103,0.707111,0.000010873],[-1,0,-1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p1050:=[[530.47,405.7,0],[0.000021459,-0.707103,0.70711,0.000010609],[-1,0,-1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p1060:=[[530.47,314.68,-0.01],[0.000021626,-0.707103,0.70711,0.000010469],[-1,0,-1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p1070:=[[565.74,314.68,-0.01],[0.000021739,-0.707103,0.70711,0.000010436],[-1,0,-1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p1080:=[[565.74,338.58,0],[0.000021888,-0.707104,0.70711,0.000010361],[-1,0,-1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p1090:=[[577.75,349.05,0],[0.000022174,-0.707104,0.70711,0.000010095],[-1,0,-1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p1100:=[[602.51,315.2,0],[0.000022299,-0.707104,0.70711,0.000010015],[-1,0,-1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p1110:=[[650.39,315.2,0],[0.000022321,-0.707104,0.707109,0.000009903],[-1,0,-1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p1120:=[[607.23,370.21,0],[0.00002259,-0.707104,0.707109,0.000009826],[-1,0,-1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p1130:=[[646.68,405.35,0],[0.00002268,-0.707104,0.707109,0.000009802],[-1,0,-1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p1140:=[[598.55,405.35,0],[0.000022745,-0.707104,0.707109,0.000009763],[-1,0,-1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p1150:=[[566.01,372.81,0],[0.000022773,-0.707104,0.707109,0.000009746],[-1,0,-1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p1160:=[[566.01,405.22,0],[0.000022747,-0.707104,0.707109,0.000009734],[-1,0,-1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p1170:=[[530.3,405.22,0],[0.000022779,-0.707104,0.707109,0.000009671],[-1,0,-1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p1180:=[[655.34,405.22,50],[0.000023362,-0.707105,0.707109,0.000009182],[-1,0,-1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p1190:=[[655.34,405.22,0],[0.000023829,-0.707105,0.707108,0.000008768],[-1,0,-1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p1200:=[[655.34,315.06,-0.01],[0.000023873,-0.707105,0.707108,0.000008807],[-1,0,-1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p1210:=[[691.4,315.06,-0.01],[0.000023923,-0.707105,0.707108,0.000008681],[-1,0,-1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p1220:=[[691.4,405.47,0],[0.000024005,-0.707105,0.707108,0.000008635],[-1,0,-1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p1230:=[[655.69,405.47,0],[0.00002409,-0.707105,0.707108,0.000008641],[-1,0,-1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p1240:=[[474.71,484.29,0],[0.000026778,-0.707106,0.707108,0.0000113],[-1,0,-1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p1250:=[[474.71,415.12,-0.01],[0.000026833,-0.707106,0.707108,0.000011332],[-1,0,-1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p1260:=[[509.83,415.12,0],[0.00002689,-0.707106,0.707108,0.000011294],[-1,0,-1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p1270:=[[521.66,415.12,50],[0.000027343,-0.707106,0.707108,0.000011181],[-1,0,-1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p1280:=[[521.66,415.12,0],[0.000027517,-0.707106,0.707108,0.000011134],[-1,0,-1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p1290:=[[521.66,481.68,0.01],[0.000027955,-0.707106,0.707108,0.000011267],[-1,0,-1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p1300:=[[559.94,481.68,0.01],[0.000028119,-0.707106,0.707108,0.000011284],[-1,0,-1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p1310:=[[559.94,415.03,0],[0.000028227,-0.707106,0.707107,0.000011347],[-1,0,-1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p1320:=[[559.94,448.19,50],[0.00002888,-0.707106,0.707107,0.000011417],[-1,0,-1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p1330:=[[559.94,448.19,0],[0.000029297,-0.707106,0.707107,0.00001138],[-1,0,-1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p1340:=[[521.66,448.19,0],[0.000029429,-0.707106,0.707107,0.000011356],[-1,0,-1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p1350:=[[571.95,414.97,50],[0.000029835,-0.707106,0.707107,0.000011202],[-1,0,-1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p1360:=[[571.96,414.97,0],[0.000030497,-0.707107,0.707107,0.000010828],[-1,0,-1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p1370:=[[571.96,482.52,0],[0.000030554,-0.707107,0.707107,0.000010846],[-1,0,-1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p1380:=[[608.69,417.31,0],[0.000030728,-0.707107,0.707107,0.000010793],[-1,0,-1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p1390:=[[645.21,483.79,0],[0.000031019,-0.707107,0.707107,0.000010803],[-1,0,-1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p1400:=[[645.21,414.94,0],[0.000031095,-0.707107,0.707107,0.000010701],[-1,0,-1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
	CONST robtarget p1410:=[[-189.77,-250.01,692.32],[0.000023459,0.382692,-0.000002594,-0.923876],[1,0,-1,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];

	PROC main()
        Trayectoria1:
        MoveJ p10,v50,z1,tool1\WObj:=wobj1;
        WHILE BitOr(DI_01,DI_02) = 0 DO            
        ENDWHILE        
        IF DI_01 = 1 THEN
            Path_10;
        ELSE
            MoveJ p1410,v50,z1,tool1\WObj:=wobj1;
            SetDO DO_02, 1;
            WHILE BitOr(DI_01,DI_02) = 0 DO                
            ENDWHILE
            SetDO DO_02, 0;
            GOTO Trayectoria1;
        ENDIF

        Trayectoria2:
        MoveJ p10,v50,z1,tool1\WObj:=wobj1;
        WHILE BitOr(DI_01,DI_02) = 0 DO            
        ENDWHILE        
        IF DI_01 = 1 THEN
            Path_20;
        ELSE
            MoveJ p1410,v50,z1,tool1\WObj:=wobj1;
            SetDO DO_02, 1;
            WHILE BitOr(DI_01,DI_02) = 0 DO                
            ENDWHILE
            SetDO DO_02, 0;
            GOTO Trayectoria2;
        ENDIF
        
        Trayectoria3:
        SetDO DO_01, 0;
        MoveJ p10,v50,z1,tool1\WObj:=wobj1;
        WHILE BitOr(DI_01,DI_02) = 0 DO            
        ENDWHILE
        IF DI_01 = 1 THEN
            Path_30;
        ELSE
            MoveJ p1410,v50,z1,tool1\WObj:=wobj1;
            SetDO DO_02, 1;
            WHILE BitOr(DI_01,DI_02) = 0 DO                
            ENDWHILE
            SetDO DO_02, 0;
            GOTO Trayectoria3;
        ENDIF
        MoveJ p10,v50,z1,tool1\WObj:=wobj1;
	ENDPROC
    
    PROC Path_10()
        SetDO DO_01, 1;
        MoveJ p20,v50,z1,tool1\WObj:=wobj1;
        MoveL p30,v50,z1,tool1\WObj:=wobj1;
        MoveC p40,p50,v50,z1,tool1\WObj:=wobj1;
        MoveL p60,v50,z1,tool1\WObj:=wobj1;
        MoveL p70,v50,z1,tool1\WObj:=wobj1;
        MoveL p80,v50,z1,tool1\WObj:=wobj1;
        MoveC p90,p100,v50,z1,tool1\WObj:=wobj1;
        MoveL p110,v50,z1,tool1\WObj:=wobj1;
        MoveC p120,p130,v50,z1,tool1\WObj:=wobj1;
        MoveL p140,v50,z1,tool1\WObj:=wobj1;
        MoveL p150,v50,z1,tool1\WObj:=wobj1;
        MoveL p160,v50,z1,tool1\WObj:=wobj1;
        MoveC p170,p180,v50,z1,tool1\WObj:=wobj1;
        SetDO DO_01, 0;
    ENDPROC
    
    PROC Path_20()
        SetDO DO_01, 1;
        MoveJ p190,v50,z1,tool1\WObj:=wobj1;
        MoveL p200,v50,z1,tool1\WObj:=wobj1;
        MoveC p210,p220,v50,z1,tool1\WObj:=wobj1;
        MoveC p230,p240,v50,z1,tool1\WObj:=wobj1;
        MoveC p250,p260,v50,z1,tool1\WObj:=wobj1;
        MoveC p270,p280,v50,z1,tool1\WObj:=wobj1;
        MoveL p290,v50,z1,tool1\WObj:=wobj1;
        MoveC p300,p310,v50,z1,tool1\WObj:=wobj1;
        MoveC p320,p330,v50,z1,tool1\WObj:=wobj1;
        MoveC p340,p350,v50,z1,tool1\WObj:=wobj1;
        MoveL p360,v50,z1,tool1\WObj:=wobj1;
        MoveC p370,p380,v50,z1,tool1\WObj:=wobj1;
        MoveC p390,p400,v50,z1,tool1\WObj:=wobj1;
        MoveC p410,p420,v50,z1,tool1\WObj:=wobj1;
        MoveC p430,p440,v50,z1,tool1\WObj:=wobj1;
        MoveL p450,v50,z1,tool1\WObj:=wobj1;
        MoveC p460,p470,v50,z1,tool1\WObj:=wobj1;
        MoveC p480,p490,v50,z1,tool1\WObj:=wobj1;
        MoveC p500,p510,v50,z1,tool1\WObj:=wobj1;
        MoveJ p520,v50,z1,tool1\WObj:=wobj1;
        MoveJ p530,v50,z1,tool1\WObj:=wobj1;
        MoveL p540,v50,z1,tool1\WObj:=wobj1;
        MoveC p550,p560,v50,z1,tool1\WObj:=wobj1;
        MoveC p570,p580,v50,z1,tool1\WObj:=wobj1;
        MoveC p590,p600,v50,z1,tool1\WObj:=wobj1;
        MoveL p610,v50,z1,tool1\WObj:=wobj1;
        MoveL p620,v50,z1,tool1\WObj:=wobj1;
        MoveL p630,v50,z1,tool1\WObj:=wobj1;
        MoveC p640,p650,v50,z1,tool1\WObj:=wobj1;
        MoveC p660,p670,v50,z1,tool1\WObj:=wobj1;
        MoveC p680,p690,v50,z1,tool1\WObj:=wobj1;
        MoveL p700,v50,z1,tool1\WObj:=wobj1;
        MoveL p710,v50,z1,tool1\WObj:=wobj1;
        MoveJ p720,v50,z1,tool1\WObj:=wobj1;
        MoveJ p730,v50,z1,tool1\WObj:=wobj1;
        MoveL p740,v50,z1,tool1\WObj:=wobj1;
        MoveL p750,v50,z1,tool1\WObj:=wobj1;
        MoveL p760,v50,z1,tool1\WObj:=wobj1;
        MoveL p770,v50,z1,tool1\WObj:=wobj1;
        MoveL p780,v50,z1,tool1\WObj:=wobj1;
        MoveL p790,v50,z1,tool1\WObj:=wobj1;
        MoveL p800,v50,z1,tool1\WObj:=wobj1;
        MoveL p810,v50,z1,tool1\WObj:=wobj1;
        MoveL p820,v50,z1,tool1\WObj:=wobj1;
        MoveL p830,v50,z1,tool1\WObj:=wobj1;        
        MoveJ p840,v50,z1,tool1\WObj:=wobj1;
        MoveJ p850,v50,z1,tool1\WObj:=wobj1;
        MoveL p860,v50,z1,tool1\WObj:=wobj1;
        MoveC p870,p880,v50,z1,tool1\WObj:=wobj1;        
        MoveC p890,p900,v50,z1,tool1\WObj:=wobj1;
        MoveC p910,p920,v50,z1,tool1\WObj:=wobj1;
        MoveL p930,v50,z1,tool1\WObj:=wobj1;
        MoveL p940,v50,z1,tool1\WObj:=wobj1;
        MoveL p950,v50,z1,tool1\WObj:=wobj1;
        MoveC p960,p970,v50,z1,tool1\WObj:=wobj1;
        MoveC p980,p990,v50,z1,tool1\WObj:=wobj1;        
        MoveC p1000,p1010,v50,z1,tool1\WObj:=wobj1;
        MoveL p1020,v50,z1,tool1\WObj:=wobj1;
        MoveL p1030,v50,z1,tool1\WObj:=wobj1;
        MoveJ p1040,v50,z1,tool1\WObj:=wobj1;
        MoveJ p1050,v50,z1,tool1\WObj:=wobj1;
        MoveL p1060,v50,z1,tool1\WObj:=wobj1;
        MoveL p1070,v50,z1,tool1\WObj:=wobj1;
        MoveL p1080,v50,z1,tool1\WObj:=wobj1;
        MoveL p1090,v50,z1,tool1\WObj:=wobj1;
        MoveL p1100,v50,z1,tool1\WObj:=wobj1;
        MoveL p1110,v50,z1,tool1\WObj:=wobj1;
        MoveL p1120,v50,z1,tool1\WObj:=wobj1;
        MoveL p1130,v50,z1,tool1\WObj:=wobj1;
        MoveL p1140,v50,z1,tool1\WObj:=wobj1;
        MoveL p1150,v50,z1,tool1\WObj:=wobj1;
        MoveL p1160,v50,z1,tool1\WObj:=wobj1;
        MoveL p1170,v50,z1,tool1\WObj:=wobj1;
        MoveJ p1180,v50,z1,tool1\WObj:=wobj1;
        MoveJ p1190,v50,z1,tool1\WObj:=wobj1;
        MoveL p1200,v50,z1,tool1\WObj:=wobj1;
        MoveL p1210,v50,z1,tool1\WObj:=wobj1;
        MoveL p1220,v50,z1,tool1\WObj:=wobj1;
        MoveL p1230,v50,z1,tool1\WObj:=wobj1;
        SetDO DO_01, 0;
    ENDPROC
    
    PROC Path_30()
        SetDO DO_01, 1;
        MoveJ p1240,v50,z1,tool1\WObj:=wobj1;
        MoveL p1250,v50,z1,tool1\WObj:=wobj1;
        MoveL p1260,v50,z1,tool1\WObj:=wobj1;
        MoveJ p1270,v50,z1,tool1\WObj:=wobj1;
        MoveJ p1280,v50,z1,tool1\WObj:=wobj1;
        MoveL p1290,v50,z1,tool1\WObj:=wobj1;
        MoveL p1300,v50,z1,tool1\WObj:=wobj1;
        MoveL p1310,v50,z1,tool1\WObj:=wobj1;
        MoveJ p1320,v50,z1,tool1\WObj:=wobj1;
        MoveJ p1330,v50,z1,tool1\WObj:=wobj1;
        MoveL p1340,v50,z1,tool1\WObj:=wobj1;
        MoveJ p1350,v50,z1,tool1\WObj:=wobj1;
        MoveJ p1360,v50,z1,tool1\WObj:=wobj1;
        MoveL p1370,v50,z1,tool1\WObj:=wobj1;
        MoveL p1380,v50,z1,tool1\WObj:=wobj1;
        MoveL p1390,v50,z1,tool1\WObj:=wobj1;
        MoveL p1400,v50,z1,tool1\WObj:=wobj1;
        SetDO DO_01, 0;
    ENDPROC
    PROC Path_40()
        MoveJ p1410,v50,z5,tool1\WObj:=wobj1;
    ENDPROC
    
    ENDMODULE

### Video:

 [![Ver video](https://github.com/Miguel-Parrado/Laboratorios-De-Robotica/blob/main/Laboratorio%20No%2001/Imagenes/Resultado.jpg?raw=true)](https://drive.google.com/file/d/1tPxiUrl0gpdxZWy1Te5esJTdrfW_wBJK/view?usp=drive_link)