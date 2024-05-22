# Introducción a IQTree

Como vimos en la presentación, la variación de aplicaciones que tiene IQTree hacen que sea una herramienta muy versatil con gran capacidad de adaptación a diferentes modelos biológicos.

Para iniciar, vamos a familiarizarnos con el uso de IQTree desde la línea de comandos, aunque IQTree tiene su propio servidor en línea para poder envíar datos a un servidor y obtener una filogenia. Sin embargo, aunque suene más práctico el uso de la página de internet, la línea de comandos tiene muchísimas más opciones que hacen que sea más personalizable y ajustable a las necesidades del investigador y a los datos como tal.

## 1. Revisión de la calidad de los datos

Antes de cargar un alineamiento a IQTree, es importante que los datos se hayan procesado de manera correcta y que el alineamiento esté en las mejores condiciones posibles. Cualquier introducción o eliminación de datos en un alineamiento podría causar que la topología que se obtenga del programa cambie drásticamente, por lo que hay que ser cuidadosos e invertir tiempo en limpiar correctamente nuestros datos.
_
Para este ejercicio, construiremos un árbol filogenético a partir del marcador nuclear PJH obtenido de las tres especies de _Psammolestes_, por lo que el primer paso será verificar que el alineamiento esté limpio y listo para ser enviado en IQTree. Descarga los datos correspondientes a [PJH](https://github.com/malvaradol/INS-Innovaseq/blob/main/DB/PJH_psammolestes.fasta) de _Psammolestes_ y abrelos en tu herramienta de preferencia para realizar la verificación visual del alineamiento.

PD: Para descargar archivos desde github, hay que seleccionar la siguiente opción en el vínculo de las bases de datos:

![](https://github.com/malvaradol/INS-Innovaseq/assets/62664336/54bfd30b-ee08-481c-ba86-85276ef2e52b)


**¿Está listo el alineamiento para construir nuestro árbol filogenético?**

## 2. Enviar nuestro alineamiento a IQTree para la construcción de un árbol filogenético

Luego de verificar que nuesto alineamiento está listo para ser sometido a análisis, el siguiente paso es cargar nuestros datos en el servidor para construir el árbol con IQTree, o simplemente cargar IQTree en caso de que se pueda usar en el computador personal. Usualmente es mejor enviar los trabajos en el servidor, pero habrán ocasiones donde el servidor esté a su capacidad máxima y no sea posible enviar trabajos. Una alternativa es trabajar con IQTree en el computador personal, especialmente si se trabaja con sets de datos pequeños (secuencias de no más de 10 kb) y no ocupará mucho tiempo ni recursos de computación (esto último también depende de ustedes).

## 3. Correr IQTree

Una vez tengan la base de datos en el servidor, deberán introducir el siguiente código para generar un árbol con IQTree:

```
iqtree -s PJH_psammolestes.fasta -bb 1000 -alrt 1000
```
El parámetro "-s" indica el alineamiento que se le proporciona al programa, "-bb" y "-alrt" corresponden al bootstrap y a un likelihood ratio test llamado SH-like. Bajo este comando, el programa hará automaticamente la selección del modelo y determinará automaticamente el número de CPUs a utilizar en la corrida.

## 4. Análisis de los outputs generados por IQTree

IQTree genera diferentes outputs que contienen diferente información relevante, sin embargo, nos enfocaremos en tres archivos principales que contienen toda la información que necesitaremos tener en cuenta para asegurarnos de que todo salió bien:

- PJH_psammolestes.fasta.iqtree: Este archivo corresponde al reporte final de la corrida. Contiene resultados computacionales y una representación textual del árbol final.
- PJH_pasmmolestes.fasta.treefile: Este es el archivo que usaremos para generar la representación gráfica en la herramienta iTol. Es un árbol en formato Newick.
- PJH_psammolestes.fasta.log: Un archivo de registro que contiene información de toda la corrida.

Si por alguna razón no pudieron correr el programa, [aquí] pueden descargar el archivo .iqtree que se generó, y [aquí] pueden descargar el archivo .treefile que vamos a usar para construir la filogenia en iTol. Pueden revisar estos archivos con cualquier editor de texto, ya que son archivos de texto plano donde solo cambia la extensión.

## 5. Cargar el archivo .treefile en iTol

# Análisis de secuencias de Arenavirus del nuevo mundo

Los arenavirus del nuevo mundo, pertenecientes al complejo Tacaribe y clasificados bajo la familia Arenaviridae, han captado la atención de diferentes organizaciones de salud pública en latinoamerica debido a su notoria capacidad para causar severas enfermedades hemorrágicas en humanos. Los primeros acercamientos al análisis molecular de estos virus fue realizado por el National Center for Infectious Diseases en 1996 cuando investigadores de este centro amplificaron los segmentos conservados del gen de la proteina de la nucleocápside (N). A la fecha, ya se conoce la estructura genética de estos virus, la cual se caracteriza por dos moleculas de RNA monocatenarias de polaridad negativa que no se sobrelapan, denotadas como segmentos S y L; esta estructura está ampliamente conservada a través de la familia de virus. El segmento L codifica la matriz de proteinas (Z) en sentido positivo y el complejo de polimerasa dependiente de RNA (LP) en sentido negativo, mientras que el segmento S codifica la nucleoproteína (NP) en sentido negativo y el complejo precursor de la glicoproteína (GPC) en sentido positivo.

Para este ejercicio, usaremos una base de datos que utiliza el gen GPC de los arenavirus pertenecientes al complejo Tacaribe, con la finalidad de comprobar si nuestra reconstrucción concuerda con lo que se conoce hasta ahora de este grupo de virus, o si hay información discordante que podría estar indicando la detección de un nuevo virus o de otros procesos evolutivos que puedan ser de importancia en salud pública. 

## 1. Descargar los datos

Descargue [aquí]() el alineamiento de arenavirus.

## 2. Control de calidad de los datos

Es importante tener en cuenta que entre más diferentes sean los organismos que estemos analizando, debemos esperar que las secuencias sean mucho más diferentes entre sí y que los gaps sean más comunes. En este caso, el alineamiento ya está correctamente curado, pero es para ejemplificar algunas de las dificultades que se tienen al trabajar con organismos cada vez más diferentes (si lo comparamos por ejemplo con el alineamiento de PJH del ejemplo anterior). Siempre hacerlo lo mejor posible pero tener presente que siempre que se quita o se pone información estamos cambiando la historia evolutiva de los datos crudos, es por esto que hay que evitar modificar en gran medida los alineamientos.

## 3. IQTree

Al igual que con el ejemplo de _Psammolestes_, usar el siguiente código para generar el árbol filogenético de los arenavirus:

```
iqtree -s PJH_psammolestes.fasta -bb 1000 -alrt 1000
```
Es posible que 1000 replicas de bootstrap no sean suficientes, si tenemos en cuenta el gran número de posiciones con variantes en nuestras secuencias (entre más varíen las secuencias, más complejo el modelo y más le tomará al programa finalizar). Para verificar si son suficientes, dentro de las últimas lineas del programa este les indicará si no se logró la convergencia del modelo, y les recomendará aumentar el número de réplicas. En caso de que esto sea necesario, cambiar el parámetro "-bb" de 1000 a 2000 (generalmente se incrementa en múltiplos de 1000).

## 4. Análisis de los outputs para retomar información de importancia.

Teniendo en cuenta lo que se aprendió anteriormente, ¿qué modelo seleccionó IQTree y que parámetro utilizó para seleccionar el mejor modelo?, ¿por qué es importante la selección del modelo para la construcción de la filogenia?

## 5. Procesamiento del archivo .treefile en iTol

Cargue el archivo .treefile en la plataforma iTol y adaptelo para mejorar la visibilidad de los diferentes grupos de la filogenia. Construya un párrafo corto donde interprete los resultados obtenidos en la filogenia y comparelos con el siguiente párrafo obtenido de la literatura sobre los arenavirus:

"Originally, this work led to a classification into three distinct lineages: Lineage A hosted Flexal, Parana, Pichinde, and Tamiami viruses, while lineage B extended its embrace to Amapari, Guanarito (GUA), Junin (JUN), Machupo (MAC), Sabia (SAB), and Tacaribe viruses. The enigmatic Latino and Oliveros viruses were allocated to lineage C. Remarkably, a compelling revelation stood out—lineage B served as the ancestral cradle for the highly pathogenic members of the Tacaribe complex (GUA, JUN, MAC, SAB), offering a glimpse into the potential evolution of these viruses. Currently, New World arenaviruses are categorized into four distinct clades: A, B, C, and A/Rec (Clade D)."

[Aquí](![squared_arenavirus_final](https://github.com/malvaradol/INS-Innovaseq/assets/62664336/d30c49f8-d08d-49b1-9a09-33d9a3ec2340)
) pueden encontrar una versión final del árbol realizada para un capítulo de un libro.
