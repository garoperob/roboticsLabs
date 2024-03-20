# Laboratorio 1
### Gustavo Adolfo Ropero Bastidas
### Joan Sebastián Arcila Cardozo
- ## Desarrollo de solución
El desarrollo de este laboratorio partió de escoger una marca reconocida y dibujar, mediante el robot IRB140 que se encuentra en el laboratorio de Sistemas Intelegentes y Robotizados (SIR), el logo, escribir el nombre de la marca y las iniciales del primer nombra de cada integrante. La selección fue facebook. Por consiguiente, se diseñó la herramienta y el workobject en Inventor. Tal como aparece en las siguientes figuras

![Image](https://github.com/garoperob/lab1robotics/blob/main/tool.png "Title")

![Image](https://github.com/garoperob/lab1robotics/blob/main/wobject.png  "Title")

La herramienta fue diseñada con una inclinación de 30° respecto a la horizontal de la base que conecta con el flanché del robot. El tablero se diseñó partiendo de las medidas de los tableros que se encuntran en el labSIR.

Los modelos se exportaron en formato .sat para importarlos directamente a RobotStudio; la herramienta se le añadió un cono truncado para tener en cuenta la longitud del marcador. Dentro del programa se creó una nueva herramienta y se guardó en la librería, con esto se eligió el robot a usar, siendo el IRB140 con el controlador IRB140_6_81_C_03. Después de esto se instaló la herramienta y se importó la geometría del worobject resultando de la sigueinte forma

![image](https://github.com/garoperob/lab1robotics/assets/80500171/f82e330b-2526-427d-b77b-18d5a882ccc7 "Title")

A partir de esto se cobstruyó el workobject sobre la geometría del tablero, mediante situar 3 puntos, donde el eje rojo es X y el eje verde es Y. El sentido del eje Z del workbject (WO) debía ser hacia dentro del tablero, de lo contrario el robot al intentar seguir la trayectoria giraría la herramienta hasta ponerla en el mismo sentido del marco WO. La parte encerrada en amarillo es el resultado de la ubicación del WO

![wobject-construction](https://github.com/garoperob/lab1robotics/assets/80500171/4182048d-6f26-458c-bdc7-0eca4246d41a "Title")

Esto permitió crear los puntos sobre la superficie de la geometría 

![image](https://github.com/garoperob/lab1robotics/assets/80500171/5510546b-9877-4788-bc1b-f9e368032e3c "Title")

Los puntos se ubicaron usando las opciones Surface Selection, Snap Edge y Snap End. Se tuvo en cuenta las discontinuidades cuando terminaba de dibujar una forma para generar un alejamiento de 30 mm de distancia y luego se acercará de forma más segura al siguiente punto para empezar la próxima forma. Estos puntos se les conoce como puntos de evacuación. Finalmente, se llegó a la construcción de la trayectoria que debía recorrer a través de los puntos, la cual se generó de forma automática. Sin embargo, fue necesario cambiar tanto los comandos MoveL para algunos puntos de evacuación como la configuración del movimiento que generaba muchas singularidades. Obteniendo el siguiente resultado

![image](https://github.com/garoperob/lab1robotics/assets/80500171/4eab3b7a-8a3f-4dba-a122-393fe6c6e707 "Title")

Teniendo el path establecido se crearon las entradas y salidas digitales, esto se hace entrando al menú del controlador y dando click derecho en signal, luego en New signal

![image](https://github.com/garoperob/lab1robotics/assets/80500171/31f534c5-1afd-466c-b099-5ebe49c5fc38 "Title")

En el cuadro desplegado se pone el nombre y el tipo de señal. En este caso, Digital Input para entradas y Digital Output para salidas

![image](https://github.com/garoperob/lab1robotics/assets/80500171/15455136-56b4-4ce9-9ff8-49e8f5c9c826 "Title")

Asignadas las señales, se edita el código de RAPID en el main() con el fin de activar entradas y salidas, se reinició el controlador y se sincronizó desde la estación hacia el RAPID, como se ve a continuación

![image](https://github.com/garoperob/lab1robotics/assets/80500171/af08ca5c-2b13-47f5-91b9-40dbbfd98a52 "Title")

Se instanció el valor de las salidas en 0, mediante la función SetDO, para crear un WHILE TRUE que contuviese las entradas y activara el recorrido del path, más precisamente, Path_10. La otra entrada debía ser la posición de mantenimiento. Dicha posición se construyó creando otro punto cercano a home y otra trayectoria con MoveJ con el fin de evitar singularidades, por ende, la otra condición que aparece en el código pertence a esta construcción. Por último, se sincronizó desde el RAPID hacia la estación para poder llamar la rutina.

# Diagrama de flujo de acciones del robot

![image](https://github.com/garoperob/lab1robotics/assets/80500171/fcb8a0eb-7802-4dbb-ab7a-b8089858105d)

# Funciones utilizadas
Se empleó SetDO para instanciar las salidas en 0, un ciclo WHILE TRUE para que la acción fuera repetitiva. Dentro del WHILE se colocaron dos condicionales IF, cada uno activa un path. La trayectoria recorrió usaron MoveL y MoveJ, donde el primero se empleó para seguir de forma lineal la trayectoria de la herramienta y el segundo para facilitar el movimiento mediante ejes de los puntos de evacuación y la trayectoria path_20.

# Diseño de la herramienta

Partiendo de las medidas del flanche encontradas en el manual del brazo robótico IRB140, por tanto, la base fue de 50 mm con tornillos M6x20 con una profundidad de 10 mm. Por otro lado, se extruyó el tubo que contenía la herramienta y se le dio una inclinación de 30°.

# Código de RAPID

```
MODULE Module1
        CONST robtarget Target_10:=[[-146.011735072,285.000005165,-418.275448773],[0.917477142,-0.000000014,0.397788505,-0.000000018],[0,0,-1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_20:=[[-110.326,144.763,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_30:=[[-112.285,154.948,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_40:=[[-119.41,171.308,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_50:=[[-128.902,183.285,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_60:=[[-140.641,192.479,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_70:=[[-150.69,197.459,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_80:=[[-161.604,200.64,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_90:=[[-179.666,201.598,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_100:=[[-193.559,198.789,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_110:=[[-207.794,192.206,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_120:=[[-219.207,183.169,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_130:=[[-228.609,171.263,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_140:=[[-234.365,159.198,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_150:=[[-237.999,140.207,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_160:=[[-235.915,121.423,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_170:=[[-226.914,101.719,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_180:=[[-216.638,90.01,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_190:=[[-198.759,78.733,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_200:=[[-186.648,75.014,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_210:=[[-174.685,73.756,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_220:=[[-163.966,74.542,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_230:=[[-156.701,76.131,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_240:=[[-148.071,79.233,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_250:=[[-138.144,84.726,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_260:=[[-128.651,92.566,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_270:=[[-122.065,100.311,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_280:=[[-114.593,113.85,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_290:=[[-110.369,130.454,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_300:=[[-151.389,130.454,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_310:=[[-151.389,121.927,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_320:=[[-165.698,117.081,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_330:=[[-165.698,130.454,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_340:=[[-177.755,130.454,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_350:=[[-179.086,130.357,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_360:=[[-180.273,129.434,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_370:=[[-180.607,127.146,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_380:=[[-180.607,117.081,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_390:=[[-194.916,117.081,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_400:=[[-194.916,130.922,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_410:=[[-194.729,136.328,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_420:=[[-192.965,140.07,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_430:=[[-189.331,143.419,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_440:=[[-185.256,144.909,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_450:=[[-165.698,144.763,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_460:=[[-165.698,158.493,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_470:=[[-151.389,158.493,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_480:=[[-151.389,144.763,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_490:=[[-110.326,144.763,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_500:=[[-110.326,144.763,-20],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_510:=[[-51.129,253.926,-20],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_520:=[[-51.129,253.926,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_530:=[[-51.129,250.133,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_540:=[[-70.505,250.133,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_550:=[[-70.505,243.983,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_560:=[[-73.641,243.983,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_570:=[[-73.641,250.255,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_580:=[[-75.544,250.206,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_590:=[[-77.375,249.802,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_600:=[[-79.139,247.693,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_610:=[[-79.336,245.319,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_620:=[[-78.82,242.475,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_630:=[[-82.296,242.475,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_640:=[[-82.656,245.918,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_650:=[[-82.426,248.607,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_660:=[[-81.342,251.07,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_670:=[[-80.231,252.241,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_680:=[[-78.529,253.229,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_690:=[[-77.133,253.655,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_700:=[[-73.641,253.926,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_710:=[[-73.641,256.479,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_720:=[[-70.505,256.479,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_730:=[[-70.505,253.926,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_740:=[[-51.129,253.926,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_750:=[[-51.129,253.926,-20],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_760:=[[-73.155,240.968,-20],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_770:=[[-73.155,240.968,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_780:=[[-69.29,240.749,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_790:=[[-70.821,234.112,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_800:=[[-70.068,230.174,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_810:=[[-66.664,228.642,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_820:=[[-65.935,228.642,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_830:=[[-64.233,238.683,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_840:=[[-61.797,241.704,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_850:=[[-59.21,242.668,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_860:=[[-56.085,242.637,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_870:=[[-53.29,241.454,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_880:=[[-50.903,238.3,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_890:=[[-50.553,234.585,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_900:=[[-51.183,232.224,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_910:=[[-52.135,230.469,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_920:=[[-53.512,228.642,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_930:=[[-51.129,228.642,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_940:=[[-51.129,224.85,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_950:=[[-66.518,224.85,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_960:=[[-69.467,225.232,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_970:=[[-71.575,226.332,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_980:=[[-73.364,228.791,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_990:=[[-74.121,232.393,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1000:=[[-74.152,235.083,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1010:=[[-73.155,240.968,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1020:=[[-73.155,240.968,-20],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1030:=[[-52.393,203.942,-20],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1040:=[[-52.393,203.942,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1050:=[[-56.648,203.942,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1060:=[[-55.344,205.892,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1070:=[[-53.96,209.863,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1080:=[[-54.369,213.123,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1090:=[[-56.142,215.435,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1100:=[[-58.085,216.502,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1110:=[[-60.316,217.04,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1120:=[[-63.113,217.15,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1130:=[[-65.632,216.802,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1140:=[[-67.95,215.854,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1150:=[[-69.778,214.173,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1160:=[[-70.786,211.579,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1170:=[[-70.583,208.762,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1180:=[[-69.764,206.575,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1190:=[[-68.171,203.942,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1200:=[[-72.426,203.942,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1210:=[[-73.703,207.259,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1220:=[[-74.128,209.801,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1230:=[[-73.916,212.987,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1240:=[[-72.784,216.05,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1250:=[[-70.947,218.291,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1260:=[[-68.707,219.765,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1270:=[[-66.848,220.496,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1280:=[[-64.996,220.9,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1290:=[[-62.588,221.08,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1300:=[[-60.143,220.966,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1310:=[[-57.854,220.532,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1320:=[[-55.623,219.662,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1330:=[[-53.134,217.783,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1340:=[[-51.575,215.429,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1350:=[[-50.655,211.612,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1360:=[[-50.99,207.637,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1370:=[[-52.393,203.942,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1380:=[[-52.393,203.942,-20],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1390:=[[-52.515,183.034,-20],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1400:=[[-52.515,183.034,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1410:=[[-56.648,183.034,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1420:=[[-55.315,185.353,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1430:=[[-54.053,189.106,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1440:=[[-54.493,194.007,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1450:=[[-56.969,196.983,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1460:=[[-59.241,197.91,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1470:=[[-61.996,198.205,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1480:=[[-61.996,182.621,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1490:=[[-64.075,182.621,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1500:=[[-67.286,182.907,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1510:=[[-70.319,184.041,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1520:=[[-73.087,186.736,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1530:=[[-74.206,190.297,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1540:=[[-73.786,194.97,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1550:=[[-71.683,198.52,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1560:=[[-68.849,200.64,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1570:=[[-65.232,201.809,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1580:=[[-61.567,202.034,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1590:=[[-58.803,201.712,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1600:=[[-55.263,200.343,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1610:=[[-52.667,197.951,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1620:=[[-51.327,195.369,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1630:=[[-50.655,191.873,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1640:=[[-50.697,189.027,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1650:=[[-51.372,186.056,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1660:=[[-52.515,183.034,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1670:=[[-52.515,183.034,-20],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1680:=[[-82.466,178.78,-20],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1690:=[[-82.466,178.78,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1700:=[[-82.466,174.987,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1710:=[[-71.283,174.987,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1720:=[[-72.506,173.432,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1730:=[[-73.119,172.451,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1740:=[[-73.656,171.384,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1750:=[[-74.098,169.937,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1760:=[[-74.269,168.402,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1770:=[[-74.069,166.24,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1780:=[[-73.497,164.672,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1790:=[[-71.532,162.335,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1800:=[[-69.453,161.103,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1810:=[[-67.046,160.335,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1820:=[[-64.72,159.993,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1830:=[[-62.462,159.916,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1840:=[[-60.052,160.092,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1850:=[[-58.412,160.406,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1860:=[[-56.884,160.884,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1870:=[[-55.077,161.753,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1880:=[[-54.07,162.429,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1890:=[[-52.456,163.935,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1900:=[[-51.418,165.45,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1910:=[[-50.684,167.473,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1920:=[[-50.5,169.604,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1930:=[[-50.687,171.448,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1940:=[[-51.126,172.907,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1950:=[[-52.174,174.987,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1960:=[[-51.129,175.231,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1970:=[[-51.129,178.78,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1980:=[[-82.466,178.78,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_1990:=[[-82.466,178.78,-20],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2000:=[[-74.273,147.516,-20],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2010:=[[-74.273,147.516,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2020:=[[-73.604,143.672,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2030:=[[-71.984,141.025,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2040:=[[-69.055,138.811,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2050:=[[-66.414,137.891,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2060:=[[-62.662,137.477,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2070:=[[-58.075,137.955,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2080:=[[-55.538,138.892,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2090:=[[-51.621,142.677,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2100:=[[-50.556,148.757,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2110:=[[-51.488,152.118,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2120:=[[-53.398,154.635,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2130:=[[-56.125,156.407,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2140:=[[-59.057,157.297,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2150:=[[-61.231,157.55,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2160:=[[-63.802,157.534,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2170:=[[-66.999,157.006,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2180:=[[-68.433,156.506,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2190:=[[-70.933,154.995,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2200:=[[-72.514,153.318,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2210:=[[-73.579,151.391,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2220:=[[-74.273,147.516,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2230:=[[-74.273,147.516,-20],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2240:=[[-74.273,125.125,-20],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2250:=[[-74.273,125.125,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2260:=[[-73.442,120.891,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2270:=[[-71.935,118.58,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2280:=[[-69.716,116.777,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2290:=[[-67.65,115.845,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2300:=[[-65.666,115.35,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2310:=[[-62.814,115.089,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2320:=[[-60.073,115.21,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2330:=[[-56.629,116.015,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2340:=[[-54.749,116.954,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2350:=[[-52.778,118.635,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2360:=[[-51,121.742,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2370:=[[-50.501,125.468,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2380:=[[-51.243,129.191,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2390:=[[-52.628,131.45,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2400:=[[-54.784,133.3,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2410:=[[-57.796,134.616,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2420:=[[-60.942,135.141,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2430:=[[-64.014,135.128,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2440:=[[-66.816,134.666,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2450:=[[-69.096,133.808,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2460:=[[-71.459,132.132,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2470:=[[-73.301,129.625,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2480:=[[-74.273,125.125,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2490:=[[-74.273,125.125,-20],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2500:=[[-82.466,111.195,-20],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2510:=[[-82.466,111.195,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2520:=[[-82.466,107.402,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2530:=[[-64.136,107.402,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2540:=[[-73.641,98.504,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2550:=[[-73.641,93.788,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2560:=[[-64.233,103.026,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2570:=[[-51.129,92.548,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2580:=[[-51.129,97.532,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2590:=[[-61.996,105.895,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2600:=[[-60.538,107.402,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2610:=[[-51.129,107.402,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2620:=[[-51.129,111.195,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2630:=[[-82.466,111.195,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2640:=[[-82.466,111.195,-20],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2650:=[[-74.273,55.06,-20],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2660:=[[-74.273,55.06,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2670:=[[-79.087,55.06,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2680:=[[-81.514,62.449,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2690:=[[-81.103,69.228,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2700:=[[-78.542,74.205,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2710:=[[-74.808,77.199,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2720:=[[-69.922,78.866,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2730:=[[-65.09,79.157,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2740:=[[-60.1,78.392,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2750:=[[-56.293,76.694,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2760:=[[-52.597,72.955,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2770:=[[-50.572,65.936,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2780:=[[-51.352,59.601,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2790:=[[-53.123,54.939,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2800:=[[-66.226,54.939,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2810:=[[-66.226,65.83,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2820:=[[-62.726,65.83,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2830:=[[-62.726,58.877,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2840:=[[-54.97,58.877,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2850:=[[-54.086,63.028,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2860:=[[-54.819,69.176,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2870:=[[-59.335,73.665,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2880:=[[-64.245,74.932,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2890:=[[-69.079,74.82,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2900:=[[-74.433,72.747,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2910:=[[-77.915,67.807,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2920:=[[-77.498,60.904,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2930:=[[-74.273,55.06,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2940:=[[-74.273,55.06,-20],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2950:=[[-77.944,36.073,-20],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2960:=[[-77.944,36.073,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2970:=[[-81.129,36.073,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2980:=[[-81.129,25.741,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_2990:=[[-58.933,25.741,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_3000:=[[-55.185,26.451,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_3010:=[[-53.041,27.947,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_3020:=[[-51.283,30.873,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_3030:=[[-50.716,34.7,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_3040:=[[-51.299,39.915,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_3050:=[[-55.14,39.915,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_3060:=[[-54.168,35.15,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_3070:=[[-55.797,30.579,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_3080:=[[-60.465,29.728,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_3090:=[[-77.944,29.728,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_3100:=[[-77.944,36.073,0],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_3110:=[[-77.944,36.073,-20],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_3130:=[[-110.326,144.763,-30],[1,0,0,0],[-1,-1,0,1],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    CONST robtarget Target_3140:=[[-146.011963343,284.999999727,-418.275696297],[0.917477141,0,0.397788507,0],[0,-1,0,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
    PERS tooldata TCP_pencil3:=[TRUE,[[127.306,0,93.5],[0.866025404,0,0.5,0]],[1,[28.146,-16.5,36.25],[1,0,0,0],0,0,0]];
    PERS tooldata TCP_pencil:=[TRUE,[[103.945,0,113.945],[0.923879533,0,0.382683432,0]],[1,[22.981,16.25,32.981],[1,0,0,0],0,0,0]];
    TASK PERS wobjdata WO_T:=[FALSE,TRUE,"",[[529.117901891,-285,130.019091873],[0.397788828,0.000000334,0.917477001,-0.000000771]],[[-136.705619267,0.000245253,145.984974927],[1,-0.00000084,0.00000035,0]]];
    CONST robtarget Target_3150:=[[-219.002968595,84.999999658,-486.630016779],[0.998198181,0,0.060003262,0],[-1,0,-1,0],[9E+09,9E+09,9E+09,9E+09,9E+09,9E+09]];
!***********************************************************
    !
    ! Module:  Module1
    !
    ! Description:
    !   <Insert description here>
    !
    ! Author: volplet
    !
    ! Version: 1.0
    !
    !***********************************************************
    
    
    !***********************************************************
    !
    ! Procedure main
    !
    !   This is the entry point of your program
    !
    !***********************************************************
    PROC main()
        !Add your code here
        SetDO DO_01,0;
        SetDO DO_02, 0;
        WHILE TRUE DO
            IF DI_01 = 1 THEN
                Path_10;
            ENDIF
            IF DI_02 = 1 THEN
                Path_20;
            ENDIF
        ENDWHILE
    ENDPROC
    PROC Path_10()
        MoveJ Target_10,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveJ Target_3130,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_20,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_30,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_40,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_50,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_60,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_70,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_80,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_90,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_100,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_110,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_120,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_130,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_140,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_150,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_160,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_170,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_180,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_190,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_200,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_210,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_220,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_230,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_240,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_250,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_260,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_270,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_280,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_290,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_300,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_310,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_320,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_330,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_340,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_350,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_360,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_370,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_380,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_390,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_400,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_410,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_420,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_430,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_440,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_450,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_460,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_470,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_480,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_490,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_500,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_510,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_520,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_530,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_540,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_550,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_560,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_570,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_580,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_590,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_600,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_610,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_620,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_630,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_640,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_650,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_660,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_670,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_680,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_690,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_700,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_710,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_720,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_730,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_740,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_750,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_760,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_770,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_780,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_790,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_800,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_810,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_820,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_830,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_840,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_850,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_860,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_870,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_880,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_890,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_900,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_910,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_920,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_930,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_940,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_950,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_960,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_970,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_980,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_990,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1000,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1010,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1020,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1030,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1040,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1050,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1060,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1070,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1080,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1090,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1100,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1110,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1120,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1130,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1140,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1150,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1160,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1170,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1180,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1190,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1200,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1210,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1220,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1230,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1240,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1250,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1260,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1270,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1280,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1290,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1300,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1310,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1320,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1330,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1340,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1350,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1360,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1370,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1380,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1390,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1400,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1410,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1420,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1430,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1440,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1450,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1460,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1470,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1480,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1490,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1500,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1510,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1520,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1530,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1540,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1550,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1560,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1570,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1580,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1590,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1600,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1610,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1620,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1630,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1640,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1650,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1660,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1670,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1680,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1690,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1700,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1710,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1720,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1730,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1740,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1750,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1760,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1770,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1780,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1790,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1800,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1810,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1820,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1830,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1840,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1850,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1860,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1870,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1880,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1890,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1900,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1910,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1920,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1930,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1940,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1950,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1960,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1970,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1980,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_1990,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2000,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2010,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2020,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2030,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2040,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2050,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2060,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2070,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2080,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2090,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2100,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2110,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2120,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2130,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2140,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2150,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2160,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2170,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2180,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2190,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2200,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2210,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2220,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2230,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2240,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2250,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2260,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2270,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2280,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2290,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2300,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2310,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2320,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2330,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2340,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2350,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2360,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2370,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2380,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2390,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2400,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2410,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2420,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2430,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2440,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2450,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2460,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2470,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2480,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2490,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2500,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2510,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2520,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2530,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2540,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2550,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2560,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2570,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2580,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2590,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2600,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2610,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2620,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2630,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2640,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2650,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2660,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2670,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2680,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2690,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2700,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2710,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2720,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2730,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2740,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2750,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2760,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2770,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2780,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2790,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2800,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2810,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2820,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2830,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2840,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2850,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2860,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2870,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2880,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2890,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2900,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2910,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2920,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2930,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2940,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2950,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2960,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2970,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2980,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_2990,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_3000,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_3010,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_3020,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_3030,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_3040,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_3050,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_3060,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_3070,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_3080,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_3090,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_3100,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveL Target_3110,v60,z0,TCP_pencil3\WObj:=WO_T;
        MoveJ Target_3140,v60,z30,TCP_pencil3\WObj:=WO_T;
    ENDPROC
    PROC Path_20()
        MoveJ Target_3150,v30,z0,TCP_pencil3\WObj:=WO_T;
    ENDPROC
ENDMODULE
```

# Video de comprobación de funcionamiento

[![IMAGE ALT TEXT HERE](https://img.youtube.com/vi/V4ZzZhOP_X8/0.jpg)](https://www.youtube.com/watch?v=V4ZzZhOP_X8)
[![IMAGE ALT TEXT HERE](https://img.youtube.com/vi/qPzvd4asezQ/0.jpg)](https://www.youtube.com/watch?v=qPzvd4asezQ)





