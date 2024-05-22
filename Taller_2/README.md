# Datación molecular mediante datos heterocrónicos y promediado de modelos de sustitución con métodos Bayesianos

Para este punto se asume que cada investigador tiene acceso a BEAST de manera independiente en su computador.

## Contexto

En este tutorial analizaremos 100 secuencias completas del genoma de la pandemia de gripe H1N1 de 2009 en Norteamérica. Las secuencias se recogieron aproximadamente entre febrero y noviembre, de modo que sus tiempos de muestreo pueden utilizarse para la calibración. Fueron analizadas por Hedge y Rambaut (Hedge et al., 2013). Un aspecto importante del uso de tiempos de muestreo para la calibración es que el tiempo de muestreo debe capturar un número suficiente de sustituciones, es decir, la población debe estar evolucionando de forma medible (Drummond et al., 2003). Una forma de verificar si una población evoluciona de forma medible es comparar la distribución a priori y a posteriori de la altura del árbol, como haremos aquí. También utilizaremos un reloj molecular relajado y el complemento de prueba bModel para promediar los modelos de sustitución (Bouckaert & Drummond, 2017). En los ejercicios opcionales podemos comparar las estimaciones de un reloj molecular relajado con las de un modelo de reloj molecular estricto. En un tutorial posterior utilizaremos la coalescencia exponencial y el modelo constante de nacimiento-muerte para inferir parámetros epidemiológicos.

## Creación de un archivo XML para BEAST

Análisis evolutivo Bayesiano por muestreo de árboles, o por sus siglas en inglés, BEAST, es una herramienta que nos permite realizar análisis filogenéticos utilizando métodos Bayesianos. Una de las mayores ventajas que presenta BEAST sobre otros programas basados en estadística Bayesiana es que es un programa amigable con el usuario; mientras que otros programas generalmente requieren manejo de línea de comandos o hasta tienen su propio lenguaje de programación, BEAST usa una interfaz Java: lo que llamamos "point-and-click!. Al igual que los análisis filogenéticos por máxima verosimilitud, estos análisis requieren como archivo de entrada un alineamiento procesado y listo para analizar. Adicionalmente, BEAST usa un archivo en formato XML que está constituido por los archivos de entrada, las instrucciones y los parámetros con los cuales se llevará a cabo la reconstrucción filogenética. Afortunadamente, existe un programa para crear este archivo el cual se llama **BEAUti**, por lo que en primera instancia usaremos este programa para generar un archivo XML.

Descargue el [alineamiento](https://github.com/malvaradol/INS-Innovaseq/blob/main/DB/NorthAm.Nov.fasta) que se usará como input para BEAUti y abra el programa BEAUti que se encuentra dentro de la carpeta que se descomprime luego de descargar BEAST de su página de internet.

Luego de abrir BEAUti, puede arrastrar el alineamiento descargado hacia el programa o seleccionarlo dentro del programa para subirlo. Se debería ver algo así el programa si las secuencias se subieron correctamente:

![image](https://github.com/malvaradol/INS-Innovaseq/assets/62664336/2a717406-2568-4ead-8265-30e1f08d9429)

Luego de que las secuencias estén cargadas, seleccione en las pestañas superiores la opción "tip dates" para abrir un nuevo menú de opciones. En este menú le indicaremos al programa que tome en cuenta las fechas que están incluidas en cada uno de los nombres de los alineamientos. Este paso es esencial ya que nuestro objetivo es determinar el momento en el tiempo en el cual se generó la epidemia de H1N1 en Estados Unidos. Para esto, dentro de tip dates marque la casilla "use tip dates" y posteriormente seleccione "Auto-configure", se desplegará un nuevo menú con otras opciones. Marque la opción "use everything" y en el menú desplegable cambie la opción "after first" a "after last" y en la casilla de la derecha del menú desplegable escriba el caracter "|". Esto le indicará a BEAUti desde que parte del nombre de las secuencias se encuentra la fecha correspondiente a cada una. Se debería ver así:

![image](https://github.com/malvaradol/INS-Innovaseq/assets/62664336/e8eb3657-3352-43a5-89de-01c66f3e5052)

Luego de esto se deberían ver las fechas en la columna "Date (raw value)", compare los valores de dicha columna con la última parte del nombre de cada secuencia para verificar que se haya tomado la porción correcta del nombre. No hay necesidad de verificarlas todas para el ejercicio, pero con un set de datos propio es bueno para evitar complicaciones en la corrida.

Seleccione el menú "Site Model" de la parte superior del programa. En lugar de utilizar un único modelo de sustitución, haremos un promedio de los que tienen en cuenta las diferencias en el número de transiciones a transversiones. En el primer menú desplegable, seleccione BEAST Model Test. Hay un segundo menú desplegable para seleccionar el rango de modelos que muestrearemos durante el MCMC. Seleccione transitionToTransversionSpit para limitar nuestra búsqueda a los que tienen en cuenta las diferencias en las transiciones a transversiones, y finalmente marque la casilla de "Empirical" en frequencies. Se debería ver así:

![image](https://github.com/malvaradol/INS-Innovaseq/assets/62664336/c914cd79-4a14-4c92-aacd-c38c1d1aa424)

Seleccione "Clock model" en la parte superior. En esta sección solo seleccione "Optimised Relaxed Clock" y deje el resto de opciones predeterminadas, de la siguiente forma:

![image](https://github.com/malvaradol/INS-Innovaseq/assets/62664336/6106e15b-09f9-42d8-8b4f-1c2de032621d)

Haga clic en la pestaña "Priors". Seleccione el modelo de "Coalescent Exponential Population". La mayoría de las distribuciones a priori restantes son adecuadas para nuestros análisis, pero es un buen ejercicio inspeccionar estas distribuciones. En particular, modifique la variable "ORCucldMean.c:NorthAm" a "Uniform" para la tasa con límites inferior y superior de 0 y 1, respectivamente. Esto se basa en nuestro conocimiento de que la gripe probablemente no evoluciona a un ritmo superior a 1 substitución/año. Se deben ver así las opciones del menú:

![image](https://github.com/malvaradol/INS-Innovaseq/assets/62664336/0adaf16c-a033-47f4-b208-dfc647cf6515)

Haga clic en la pestaña MCMC. Aquí podemos seleccionar diferentes opciones para el MCMC. La longitud de la cadena debe ser 20000000. Haga clic en tracelog y treelog para especificar los outputs de BEAST, que es un conjunto de árboles y valores de parámetros, muestreados a partir de la posterior. Asegúrese de que la casilla Log Every es 2000 para los árboles y los archivos log. Nombre los archivos de registro y de árboles h1n1_UCLD.log y h1n1_UCLD.trees, respectivamente. Utilizamos la extensión _ucld para referirnos al modelo de reloj lognormal no correlacionado. Las opciones se deberían ver así:

![image](https://github.com/malvaradol/INS-Innovaseq/assets/62664336/e4f30a32-a990-4c80-a896-00bc254e1fae)


Luego de esto, nuesto archivo está listo para ser corrido. En caso de que tenga algún tipo de problema creando el archivo, [aquí](https://github.com/malvaradol/INS-Innovaseq/blob/main/DB/h1n1_ucld.xml) puede encontrar el archivo XML y descargarlo para correrlo.

## Correr BEAST

Abra BEAST que se encuentra en la carpeta descomprimida luego de descargar el programa, y seleccione las siguientes opciones para poner a correr el programa:

![image](https://github.com/malvaradol/INS-Innovaseq/assets/62664336/748d7df5-2cf1-4f29-9a0d-2e849902b119)

Puede aumentar el número de threads según las capacidades de su computador, tenga cuidado porque aumentar esta opción más de lo que su capacidad computacional permite resultará en que su computador se congele y tenga que apagarlo forzadamente.

Una vez ponga a correr el programa, empezarán a correr números en el fondo de su pantalla de la siguiente manera:

![image](https://github.com/malvaradol/INS-Innovaseq/assets/62664336/3d925ba5-6ea2-4079-b196-a4484d487549)




