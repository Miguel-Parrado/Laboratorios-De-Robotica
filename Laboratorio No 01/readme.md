#
#Laboratorio #01 de Robótica.
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
