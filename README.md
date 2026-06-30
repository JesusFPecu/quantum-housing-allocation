# quantum-matching-optimization
# By Jesús Pérez
A quantum optimization project that models a 4×4 bipartite matching problem as a QUBO, validates the solution with classical methods, and solves it using QAOA in a local Qiskit simulator.

## Asignación Óptima de Modalidades de Vivienda mediante QUBO y QAOA
## Proyecto Final – Computación Cuántica

# 1. Introducción

La asignación eficiente de recursos públicos constituye uno de los problemas más importantes dentro de la planeación gubernamental. En particular, los programas de vivienda requieren distribuir recursos limitados entre diferentes regiones que presentan distintos niveles de necesidad social. Esta decisión implica evaluar simultáneamente múltiples criterios, como el rezago habitacional existente, el historial de apoyos otorgados y las modalidades disponibles de financiamiento o subsidio.

Desde el punto de vista computacional, este tipo de problemas pertenece a la familia de los problemas de optimización combinatoria, caracterizados por un crecimiento exponencial del número de soluciones posibles conforme aumenta el tamaño de la instancia. Debido a esta complejidad, en los últimos años se han desarrollado nuevas estrategias basadas en computación cuántica que buscan aproximar soluciones de alta calidad mediante algoritmos híbridos.

Uno de los algoritmos más estudiados para este propósito es el Quantum Approximate Optimization Algorithm (QAOA), propuesto para resolver problemas formulados como modelos Quadratic Unconstrained Binary Optimization (QUBO). Esta formulación permite representar una gran variedad de problemas de optimización mediante variables binarias y términos cuadráticos, haciendo posible su implementación tanto en simuladores clásicos como en hardware cuántico.

En este proyecto se desarrolla una instancia educativa del problema utilizando información pública relacionada con el rezago habitacional y las acciones de vivienda realizadas en municipios del estado de Puebla. A partir de estos datos se construye un problema de asignación bipartita de dimensión 4 × 4, el cual posteriormente se transforma a un modelo QUBO y se resuelve mediante tres enfoques distintos:

búsqueda clásica exacta;
simulación local de QAOA;
ejecución sobre hardware cuántico de IBM Quantum (cuando está disponible).

El propósito del proyecto no consiste en proponer una política pública definitiva, sino demostrar de manera reproducible cómo un problema real puede formularse matemáticamente y resolverse utilizando herramientas de computación cuántica.

# 2. Objetivo del proyecto

El objetivo general consiste en construir y resolver un problema de asignación bipartita entre municipios y modalidades de apoyo para vivienda mediante una formulación QUBO, evaluando posteriormente el desempeño del algoritmo QAOA respecto a la solución clásica exacta.

Para alcanzar este objetivo se desarrollaron las siguientes actividades:

construir una instancia 4×4 utilizando información proveniente de fuentes oficiales;
definir una función de compatibilidad entre municipios y modalidades de vivienda;
formular el problema como un modelo de optimización binaria;
transformar dicho modelo en un problema QUBO;
validar el modelo mediante búsqueda exhaustiva;
implementar una simulación local del algoritmo QAOA;
comparar los resultados obtenidos mediante métodos clásicos y cuánticos;
analizar el comportamiento del algoritmo cuando se ejecuta en hardware cuántico real.
3. Contexto del problema

El acceso a una vivienda adecuada constituye uno de los principales indicadores del bienestar social de una población. En México, diferentes instituciones públicas implementan programas orientados a disminuir el rezago habitacional mediante apoyos para adquisición, construcción, ampliación o mejoramiento de viviendas.

Sin embargo, los recursos disponibles son limitados, por lo que resulta necesario establecer mecanismos que permitan priorizar aquellas regiones donde la necesidad social es mayor.

Desde una perspectiva de optimización, este problema puede modelarse como una asignación entre dos conjuntos:

municipios que requieren atención;
modalidades disponibles de apoyo para vivienda.

Cada municipio debe recibir una modalidad prioritaria y cada modalidad solamente puede asignarse una vez dentro de la instancia considerada.

Este tipo de formulación corresponde naturalmente a un problema de matching bipartito, ampliamente estudiado dentro de la investigación de operaciones.

# 4. Justificación del uso de computación cuántica

Los problemas de asignación pertenecen a una familia de problemas cuya complejidad aumenta rápidamente conforme crece el número de variables binarias involucradas.

Aunque una instancia de tamaño 4×4 puede resolverse fácilmente mediante métodos clásicos, el interés de este proyecto radica en desarrollar un pipeline completo que permita escalar posteriormente hacia instancias de mayor tamaño.

La computación cuántica ofrece nuevas alternativas para abordar este tipo de problemas mediante algoritmos híbridos como QAOA, el cual combina:

optimización clásica de parámetros;
evolución cuántica del estado;
muestreo probabilístico de soluciones.

Actualmente los dispositivos cuánticos disponibles pertenecen al paradigma NISQ (Noisy Intermediate-Scale Quantum), por lo que aún presentan limitaciones importantes en número de qubits, profundidad de circuitos y ruido experimental. Por esta razón, el algoritmo utilizado en este trabajo se implementa principalmente mediante simulación local, utilizando hardware cuántico únicamente como una validación experimental adicional.

# 5. Descripción general del problema

El problema consiste en determinar cuál modalidad de vivienda debe asignarse prioritariamente a cada municipio considerando simultáneamente dos criterios:

el nivel de rezago habitacional del municipio;
el historial de acciones realizadas para cada modalidad de vivienda.

La solución buscada corresponde a una asignación uno a uno entre ambos conjuntos que maximice una función de prioridad previamente definida.

La instancia utilizada contiene exactamente cuatro municipios y cuatro modalidades de vivienda, permitiendo construir un problema de asignación con dieciséis variables binarias.

# 6. Fuentes de información

La construcción del dataset se realizó utilizando información pública proveniente de instituciones gubernamentales relacionadas con el sector vivienda.

Se emplearon dos conjuntos de información independientes.

6.1 Información sobre rezago habitacional

Este conjunto representa la demanda habitacional.

Para cada municipio se recopilaron los siguientes indicadores:

viviendas con rezago;
viviendas sin rezago;
porcentaje de rezago habitacional.

A partir de estos datos se calculó el porcentaje de rezago mediante

[
\text{Rezago}=
\frac{\text{Viviendas con rezago}}
{\text{Viviendas con rezago}+\text{Viviendas sin rezago}}
]

Los municipios seleccionados fueron:

ID	Municipio	Porcentaje de rezago
A1	Puebla	7.05 %
A2	Tehuacán	23.81 %
A3	Atlixco	22.58 %
A4	Huauchinango	42.90 %

Estos municipios fueron elegidos porque presentan diferentes niveles de rezago habitacional, permitiendo construir una instancia representativa para efectos académicos.

6.2 Información sobre acciones de vivienda

Este conjunto representa la oferta histórica.

Se utilizaron las acciones registradas para cuatro modalidades oficiales:

ID	Modalidad
B1	Vivienda Nueva
B2	Vivienda Existente
B3	Mejoramiento
B4	Otros Programas

Las acciones corresponden a apoyos otorgados por organismos públicos relacionados con la política nacional de vivienda, entre ellos:

INFONAVIT;
CONAVI;
FOVISSSTE;
otros organismos participantes.

Para cada municipio se contabilizó el número de acciones registradas en cada modalidad, obteniendo la matriz utilizada posteriormente para construir el score de compatibilidad.

# 7. Organización del repositorio

El proyecto fue estructurado siguiendo una organización sencilla que facilita la reproducibilidad de todos los experimentos.

Proyecto_QAOA_Vivienda/


├── data/

│   ├── housing_deficit.csv


│   ├── housing_actions_4x4.csv
│   
│

├── notebooks/

│   └── proyecto_qaoa.ipynb
│

├── README.md
│

├── requirements.txt
│

└── LICENSE

# 8. Construcción del dataset 4×4

Uno de los requisitos fundamentales del proyecto consiste en construir una instancia de tamaño 4×4 utilizando información proveniente de un problema real o semi-real. En este trabajo, la instancia fue desarrollada a partir de información pública relacionada con el rezago habitacional y las acciones históricas de vivienda en municipios del estado de Puebla.

A diferencia del ejemplo molecular utilizado inicialmente para validar el pipeline computacional, el dataset empleado en este proyecto corresponde a una instancia propia, diseñada específicamente para representar un problema de asignación entre municipios y modalidades de apoyo para vivienda.

La construcción de la instancia se realizó separando claramente dos componentes del problema:

Demanda habitacional, representada por los municipios y su nivel de rezago.
Oferta de programas, representada por las modalidades oficiales de vivienda.

Esta separación facilita la interpretación del problema como un modelo de matching bipartito y permite mantener la trazabilidad de las fuentes originales de información.

8.1 Conjunto A: Municipios

El conjunto (A) representa la demanda del problema y está formado por cuatro municipios del estado de Puebla.

Estos municipios fueron seleccionados porque presentan diferentes niveles de rezago habitacional, permitiendo construir una instancia con suficiente variabilidad para evaluar el algoritmo de optimización.

Formalmente,

[
A={A_1,A_2,A_3,A_4}
]

donde:

ID	Municipio	Porcentaje de rezago
A1	Puebla	7.05 %
A2	Tehuacán	23.81 %
A3	Atlixco	22.58 %
A4	Huauchinango	42.90 %

Cada municipio constituye un nodo del lado izquierdo del grafo bipartito utilizado posteriormente en el modelo de asignación.

8.2 Cálculo del porcentaje de rezago

Para cada municipio se dispone del número de viviendas con rezago y del número de viviendas sin rezago.

Con el propósito de obtener un indicador comparable entre municipios de diferentes tamaños, se calculó el porcentaje de rezago habitacional mediante

[
R_i=
\frac{\text{Con Rezago}}
{\text{Con Rezago}+\text{Sin Rezago}}
]

donde

(R_i) representa el porcentaje de rezago del municipio (i);
el numerador corresponde al número de viviendas con rezago;
el denominador representa el total de viviendas consideradas.

Los valores utilizados fueron:

Municipio	Con rezago	Sin rezago	Rezago (%)
Puebla	33 147	436 797	7.05
Tehuacán	20 156	64 501	23.81
Atlixco	7 957	27 283	22.58
Huauchinango	11 911	15 847	42.90

Este indicador representa la necesidad relativa de atención habitacional en cada municipio.

8.3 Conjunto B: Modalidades de vivienda

El conjunto (B) representa la oferta del problema.

Está conformado por cuatro modalidades oficiales utilizadas por distintos programas públicos de vivienda.

Formalmente,

[
B={B_1,B_2,B_3,B_4}
]

donde

ID	Modalidad
B1	Vivienda Nueva
B2	Vivienda Existente
B3	Mejoramiento
B4	Otros Programas

Cada modalidad constituye un nodo del lado derecho del grafo bipartito.

8.4 Acciones históricas de vivienda

Además del rezago habitacional, se incorporó un segundo indicador correspondiente al número de acciones históricas realizadas en cada municipio para cada modalidad de vivienda.

Las acciones representan apoyos otorgados por instituciones del sector vivienda, incluyendo programas operados por organismos como INFONAVIT, CONAVI y FOVISSSTE.

La información utilizada se resume en la siguiente matriz:

Municipio	Vivienda Nueva	Vivienda Existente	Mejoramiento	Otros Programas
Puebla	1245	864	392	110
Tehuacán	698	415	310	82
Atlixco	502	301	228	74
Huauchinango	280	194	257	61

Cada elemento (a_{ij}) representa el número de acciones históricas registradas para el municipio (i) bajo la modalidad (j).

8.5 Organización del dataset

Para mantener la trazabilidad de los datos originales se decidió conservar dos archivos independientes.

housing_deficit.csv

Representa la demanda habitacional.

Contiene:

municipio;
viviendas con rezago;
viviendas sin rezago;
porcentaje de rezago.
housing_actions_4x4.csv

Representa la oferta histórica.

Contiene el número de acciones registradas para cada combinación municipio–modalidad.

Esta separación permite identificar claramente el origen de cada variable utilizada durante la construcción del modelo.

# 9. Construcción de la matriz de compatibilidad

El algoritmo QAOA requiere una matriz numérica de compatibilidad que permita evaluar la calidad de cada posible asignación.

Esta matriz se denota por

[S=(S_{ij})]

donde cada elemento representa la prioridad de asignar la modalidad (j) al municipio (i).

En lugar de asignar valores arbitrarios, la matriz fue construida mediante una función de score basada en dos indicadores objetivos.

9.1 Normalización del rezago

El porcentaje de rezago presenta una escala distinta al número de acciones.

Por ello fue necesario normalizar ambos indicadores utilizando el método Min–Max.

Para el rezago,

[z_r(i)=
\frac{R_i-R_{\min}}
{R_{\max}-R_{\min}}]

donde:

(R_i) es el porcentaje de rezago del municipio;
(R_{\min}) corresponde al menor porcentaje observado;
(R_{\max}) corresponde al mayor porcentaje observado.

Después de esta transformación todos los municipios tienen valores comprendidos entre 0 y 1.

9.2 Normalización de acciones

El número de acciones también fue normalizado mediante Min–Max,

[
z_a(i,j)=
\frac{a_{ij}-a_{\min}}
{a_{\max}-a_{\min}}
]

donde:

(a_{ij}) representa el número de acciones para la modalidad (j);
(a_{\min}) y (a_{\max}) corresponden a los valores mínimo y máximo observados en toda la matriz.

La normalización evita que el número absoluto de acciones domine el cálculo del score.

9.3 Definición del score

Una vez normalizados ambos indicadores, la compatibilidad entre municipios y modalidades se definió mediante

0.6,z_r(i)
+
0.4,z_a(i,j)
]

Esta expresión asigna un peso del 60 % al rezago habitacional y un peso del 40 % al historial de acciones.

La elección de estos pesos constituye una decisión metodológica del proyecto y responde al objetivo de priorizar la necesidad social sobre el comportamiento histórico de los programas de vivienda.

En consecuencia, un municipio con alto rezago obtiene un score elevado incluso cuando el número de acciones históricas haya sido relativamente bajo.

9.4 Ejemplo de cálculo

Supóngase la asignación entre el municipio de Huauchinango y la modalidad Vivienda Nueva.

Una vez normalizados los datos se obtiene

[z_r=1.000]

[z_a=0.185]

Por lo tanto,

[0.674]

Este procedimiento se repite para las dieciséis combinaciones posibles entre municipios y modalidades, generando la matriz completa de compatibilidad utilizada posteriormente por el modelo QUBO.

9.5 Interpretación del score

Es importante destacar que el score no representa una probabilidad, ni una recomendación oficial de política pública.

El score constituye únicamente un indicador de prioridad construido con fines académicos para demostrar la formulación de un problema real como un modelo de optimización combinatoria.

Diferentes criterios de ponderación podrían producir matrices de compatibilidad distintas y, en consecuencia, soluciones diferentes.

La carpeta data/ contiene exclusivamente los datos empleados durante la construcción de la instancia 4×4, mientras que el notebook implementa todo el pipeline 
computacional, desde la lectura del dataset hasta la comparación entre métodos clásicos y cuánticos.

# 10. Formulación matemática del problema

Una vez construida la instancia 4×4, el siguiente paso consiste en expresar el problema mediante un modelo matemático de optimización.

La formulación utilizada corresponde a un problema clásico de asignación uno a uno (One-to-One Assignment Problem), en el cual se busca maximizar una medida de compatibilidad entre dos conjuntos disjuntos.

En este proyecto, dichos conjuntos corresponden a los municipios y las modalidades de apoyo para vivienda.

10.1 Conjunto de municipios

Sea

[
A={A_1,A_2,A_3,A_4}
]

el conjunto de municipios considerados.

Cada elemento representa una región que requiere atención mediante algún programa de vivienda.

En la instancia utilizada:

Símbolo	Municipio
(A_1)	Puebla
(A_2)	Tehuacán
(A_3)	Atlixco
(A_4)	Huauchinango
10.2 Conjunto de modalidades

Sea

[
B={B_1,B_2,B_3,B_4}
]

el conjunto de modalidades de apoyo.

Cada modalidad representa un tipo distinto de intervención habitacional.

Símbolo	Modalidad
(B_1)	Vivienda Nueva
(B_2)	Vivienda Existente
(B_3)	Mejoramiento
(B_4)	Otros Programas
10.3 Variable binaria de decisión

Para representar cada posible asignación se define la variable binaria

\begin{cases}
1,&\text{si la modalidad }j\text{ se asigna al municipio }i,\
0,&\text{en otro caso.}
\end{cases}
]

Cada variable representa una posible arista del grafo bipartito.

Como existen cuatro municipios y cuatro modalidades, el número total de variables binarias es

[
4\times4=16.
]

Estas dieciséis variables corresponden posteriormente a los dieciséis qubits lógicos empleados por el modelo QUBO.

10.4 Matriz de compatibilidad

Sea

[
S=(S_{ij})
]

la matriz de compatibilidad construida a partir del rezago habitacional y del historial de acciones de vivienda.

Cada elemento representa la prioridad de asignar la modalidad (j) al municipio (i).

Por ejemplo,

[
S_{23}
]

corresponde al score obtenido al asignar la modalidad Mejoramiento al municipio de Tehuacán.

Mientras mayor sea el valor de (S_{ij}), más conveniente resulta dicha asignación según los criterios definidos durante la construcción del dataset.

10.5 Función objetivo

El objetivo consiste en maximizar la suma de los scores asociados a las asignaciones seleccionadas.

Matemáticamente,

[
\max
\sum_{i=1}^{4}
\sum_{j=1}^{4}
S_{ij}x_{ij}.
]

Esta expresión representa el beneficio total obtenido por una asignación determinada.

Cada variable activa ((x_{ij}=1)) aporta el score correspondiente.

Las variables con valor cero no contribuyen a la función objetivo.

# 11. Restricciones del problema

La asignación debe satisfacer dos restricciones fundamentales.

11.1 Restricción por municipio

Cada municipio debe recibir exactamente una modalidad de vivienda.

Por tanto,

[
\sum_{j=1}^{4}x_{ij}=1,
\qquad
\forall i.
]

Esta restricción impide que un municipio reciba múltiples modalidades simultáneamente o que permanezca sin asignación.

11.2 Restricción por modalidad

Cada modalidad solamente puede asignarse una vez.

Formalmente,

[
\sum_{i=1}^{4}x_{ij}=1,
\qquad
\forall j.
]

Con ello se garantiza que ninguna modalidad sea utilizada por más de un municipio dentro de la instancia considerada.

# 12. ¿Por qué este problema es un matching bipartito?

El modelo construido pertenece a la familia de los problemas de matching bipartito.

Un grafo bipartito está formado por dos conjuntos de vértices disjuntos y las conexiones únicamente pueden establecerse entre elementos pertenecientes a conjuntos diferentes.

En este proyecto:

el conjunto izquierdo corresponde a los municipios;
el conjunto derecho corresponde a las modalidades de vivienda.

No existen conexiones entre municipios ni entre modalidades.

Cada arista posible representa una asignación municipio–modalidad.

Las restricciones del problema obligan a seleccionar exactamente una arista incidente sobre cada vértice de ambos conjuntos, lo cual equivale a construir un matching perfecto.

Este tipo de formulación es ampliamente utilizado en problemas clásicos de asignación, logística, planificación y transporte.

# 13. Conversión del problema a un modelo QUBO

Los algoritmos cuánticos utilizados en este proyecto requieren que el problema se exprese como un modelo de optimización binaria sin restricciones explícitas.

Para ello se utiliza la formulación Quadratic Unconstrained Binary Optimization (QUBO).

En un modelo QUBO, las restricciones originales se incorporan a la función objetivo mediante términos de penalización cuadrática.

La energía total del sistema puede escribirse como

\sum_kQ_{kk}x_k
+
\sum_{k<l}Q_{kl}x_kx_l
+
\text{offset}.
]

La matriz (Q) contiene todos los coeficientes lineales y cuadráticos del problema.

13.1 Incorporación de la función objetivo

Como el objetivo original consiste en maximizar el score,

[
\max
\sum_{ij}S_{ij}x_{ij},
]

la formulación QUBO transforma este problema en una minimización de energía utilizando

[

S_{ij}
]

como coeficiente lineal.

De esta forma, las asignaciones con mayor score producen energías más pequeñas y, por lo tanto, resultan preferidas durante la optimización.

13.2 Penalización por municipio

La restricción

[
\sum_jx_{ij}=1
]

se incorpora mediante

[
\lambda_A
\left(
\sum_jx_{ij}-1
\right)^2.
]

Esta expresión tiene valor cero únicamente cuando el municipio recibe exactamente una modalidad.

Cualquier violación incrementa automáticamente la energía del sistema.

13.3 Penalización por modalidad

Análogamente,

[
\sum_ix_{ij}=1
]

se transforma en

[
\lambda_B
\left(
\sum_ix_{ij}-1
\right)^2.
]

Con ello se garantiza que cada modalidad aparezca exactamente una vez.

13.4 Selección automática de las penalizaciones

Las constantes de penalización no fueron elegidas arbitrariamente.

El notebook calcula automáticamente

\left\lceil
4\cdot
\max(|S|)
+
1
\right\rceil.
]

Esta elección asegura que cualquier violación de las restricciones resulte más costosa que la posible ganancia obtenida al aumentar el score.

En consecuencia, las soluciones óptimas del modelo QUBO coinciden con asignaciones factibles del problema original.

# 14. Construcción de la matriz QUBO

Una vez incorporadas la función objetivo y las penalizaciones, el notebook genera automáticamente la matriz

[
Q
]

de dimensión

[
16\times16.
]

Cada fila y columna corresponde a una variable binaria.

Los elementos diagonales representan los términos lineales asociados a cada posible asignación.

Los elementos fuera de la diagonal representan las interacciones cuadráticas originadas por las restricciones.

Esta matriz constituye la entrada principal utilizada posteriormente por el algoritmo QAOA.

# 15. Relación entre variables binarias y qubits

Cada variable binaria

[
x_{ij}
]

se asigna a un qubit lógico mediante la relación

[
k=iN_B+j.
]

Como la instancia contiene dieciséis variables binarias, el circuito cuántico requiere dieciséis qubits lógicos.

La correspondencia entre variables y qubits se mantiene durante toda la ejecución del algoritmo, permitiendo reconstruir posteriormente la asignación municipio–modalidad a partir de los bitstrings medidos.

# 16. Pipeline computacional

Una vez construida la instancia del problema y formulado el modelo QUBO, se implementó un pipeline computacional completamente reproducible utilizando Python y Google Colab.

El flujo de trabajo fue diseñado para ejecutarse íntegramente en un entorno gratuito, sin requerir grandes recursos computacionales y manteniendo compatibilidad con IBM Quantum.

El pipeline puede dividirse en seis etapas principales:

Preparación del entorno.
Construcción del modelo.
Validación clásica.
Simulación local de QAOA.
Ejecución en hardware cuántico.
Comparación de resultados.

Cada una de estas etapas se describe a continuación.

16.1 Importación de librerías

La primera sección del notebook carga todas las bibliotecas necesarias para el proyecto.

Entre ellas se encuentran:

NumPy, utilizado para operaciones matriciales y manejo de vectores.
Pandas, empleado para cargar y manipular los conjuntos de datos.
Matplotlib, utilizado para la generación de gráficas.
SciPy, utilizado para la optimización clásica mediante el algoritmo COBYLA y para resolver problemas de asignación mediante el algoritmo Húngaro.
IPython.display, empleado para mostrar tablas durante la ejecución del notebook.

Además, se fija una semilla aleatoria

[
SEED=2026
]

con el objetivo de garantizar la reproducibilidad de todos los experimentos.

16.2 Carga del dataset

El notebook lee los archivos ubicados dentro de la carpeta

data/

Los archivos utilizados son:

housing_deficit.csv
housing_actions_4x4.csv

A partir de ellos se construyen los objetos:

A_df, que representa el conjunto de municipios.
B_df, que representa las modalidades de vivienda.
S, correspondiente a la matriz de compatibilidad.

Esta organización mantiene separados los datos originales de la información derivada utilizada por el modelo matemático.

16.3 Construcción automática de la matriz de compatibilidad

Después de cargar ambos conjuntos de datos se realiza la normalización Min-Max de los indicadores.

Posteriormente se calcula automáticamente cada elemento

[
S_{ij}
]

utilizando la expresión

0.6z_r(i)
+
0.4z_a(i,j).
]

De esta manera la matriz de compatibilidad se genera directamente a partir del dataset y no mediante valores definidos manualmente.

Esto hace que el proceso sea completamente reproducible y facilita modificar posteriormente la instancia del problema.

16.4 Validación del dataset

Antes de continuar con la construcción del modelo se verifica que la instancia cumpla las condiciones esperadas.

Se comprueba que:

existan exactamente cuatro municipios;
existan exactamente cuatro modalidades;
la matriz de compatibilidad tenga dimensión 4×4;
todos los valores sean numéricos y finitos.

En caso contrario, la ejecución del notebook se detiene inmediatamente.

Esta validación evita que errores en los archivos CSV produzcan resultados incorrectos durante las etapas posteriores.

16.5 Definición de variables binarias

Cada posible asignación municipio–modalidad se representa mediante una variable binaria

[
x_{ij}.
]

Como existen cuatro municipios y cuatro modalidades, el número total de variables binarias es

[
16.
]

Cada variable se asigna a un índice entero

[
k=iN_B+j,
]

que posteriormente corresponde directamente a un qubit lógico del circuito cuántico.

16.6 Selección automática de penalizaciones

Para transformar el problema en un modelo QUBO es necesario incorporar las restricciones mediante penalizaciones.

En lugar de utilizar un valor fijo, el notebook calcula automáticamente

\left\lceil
4\max(|S|)+1
\right\rceil.
]

Esta estrategia garantiza que cualquier violación de las restricciones produzca un incremento de energía mayor que cualquier posible mejora obtenida mediante la función objetivo.

16.7 Construcción de la matriz QUBO

Utilizando la función objetivo y las penalizaciones se construye automáticamente la matriz

[
Q.
]

El notebook genera:

términos lineales;
términos cuadráticos;
término constante (offset).

Posteriormente se crea una tabla que muestra todos los coeficientes distintos de cero, facilitando la inspección del modelo construido.

16.8 Funciones auxiliares

Se implementan diversas funciones que permiten trabajar con el modelo QUBO.

Entre ellas destacan:

evaluación de energía;
reconstrucción de la matriz de asignación;
cálculo del score;
verificación de factibilidad;
extracción de los pares seleccionados.

Estas funciones son utilizadas tanto por los algoritmos clásicos como por las simulaciones cuánticas.

# 17. Validación clásica

Antes de ejecutar cualquier algoritmo cuántico resulta indispensable verificar que el modelo matemático fue construido correctamente.

Para ello se utilizaron dos métodos independientes.

17.1 Búsqueda exhaustiva

La instancia contiene dieciséis variables binarias.

Por lo tanto, existen

[
2^{16}=65,536
]

configuraciones posibles.

El notebook genera todas estas configuraciones y calcula su energía QUBO.

Posteriormente identifica aquella con menor energía.

Esta solución constituye el óptimo global del modelo.

17.2 Validación mediante permutaciones

El problema original corresponde a un matching perfecto.

Por ello también puede resolverse evaluando únicamente las

[
4!=24
]

permutaciones factibles.

Para cada permutación se calcula el score total.

La mejor de ellas debe coincidir con el óptimo encontrado mediante el modelo QUBO.

Esta comparación constituye una validación adicional de la correcta construcción de las penalizaciones.

# 18. Simulación local de QAOA

Una vez validado el modelo QUBO comienza la etapa cuántica.

El algoritmo utilizado corresponde al Quantum Approximate Optimization Algorithm (QAOA).

La simulación se ejecuta completamente en Google Colab utilizando representación vectorial del estado cuántico.

Como el problema contiene dieciséis variables binarias, el estado cuántico posee

[
2^{16}=65,536
]

amplitudes complejas.

Este tamaño resulta suficientemente pequeño para ejecutarse cómodamente en memoria RAM.

18.1 Estado inicial

La simulación comienza en el estado uniforme

[
|+\rangle^{\otimes16},
]

en el cual todas las configuraciones binarias poseen la misma amplitud inicial.

Este estado representa una superposición uniforme sobre todas las soluciones posibles.

18.2 Hamiltoniano de costo

El Hamiltoniano de costo se construye directamente a partir de la energía QUBO.

Como dicho Hamiltoniano es diagonal en la base computacional, la evolución correspondiente puede implementarse mediante fases aplicadas directamente al vector de estado.

Para mejorar la estabilidad numérica, las energías son previamente centradas y escaladas.

18.3 Mixer

Se utiliza el mixer estándar

e^{-i\beta\sum X_k}.
]

Este operador favorece la exploración del espacio de soluciones permitiendo modificar simultáneamente el valor de los qubits.

Debe destacarse que este mixer no preserva automáticamente las restricciones del problema, razón por la cual posteriormente se analiza la factibilidad de las soluciones obtenidas.

18.4 Construcción del circuito QAOA

Cada capa del algoritmo aplica secuencialmente:

Hamiltoniano de costo.
Hamiltoniano mixer.

En este proyecto se utilizó inicialmente

[
p=1,
]

con el objetivo de mantener tiempos de ejecución reducidos y hacer compatible la simulación con Google Colab.

18.5 Optimización clásica

Los parámetros

[
(\gamma,\beta)
]

se optimizan mediante el algoritmo COBYLA.

Durante cada iteración se calcula la energía esperada del estado cuántico y el optimizador ajusta los parámetros para minimizar dicha energía.

El resultado corresponde a los parámetros utilizados posteriormente tanto en la simulación local como en el circuito ejecutado sobre IBM Quantum.

18.6 Muestreo

Una vez optimizado el circuito se calcula la distribución de probabilidades asociada al estado final.

Posteriormente se generan miles de mediciones simuladas ("shots"), obteniendo una colección de bitstrings comparables con los producidos por hardware cuántico real.

Cada bitstring representa una posible solución del problema de asignación.

18.7 Métricas probabilísticas

Además de identificar la mejor solución observada, el notebook calcula indicadores adicionales como:

probabilidad de soluciones factibles;
probabilidad del óptimo clásico;
energía media esperada;
distribución de energías observadas.

Estos indicadores permiten evaluar la calidad global de la distribución generada por QAOA y no únicamente su mejor muestra.

# 16. Pipeline computacional

Una vez construida la instancia del problema y formulado el modelo QUBO, se implementó un pipeline computacional completamente reproducible utilizando Python y Google Colab.

El flujo de trabajo fue diseñado para ejecutarse íntegramente en un entorno gratuito, sin requerir grandes recursos computacionales y manteniendo compatibilidad con IBM Quantum.

El pipeline puede dividirse en seis etapas principales:

1. Preparación del entorno.
2. Construcción del modelo.
3. Validación clásica.
4. Simulación local de QAOA.
5. Ejecución en hardware cuántico.
6. Comparación de resultados.

Cada una de estas etapas se describe a continuación.

16.1 Importación de librerías

La primera sección del notebook carga todas las bibliotecas necesarias para el proyecto.

Entre ellas se encuentran:

• NumPy, utilizado para operaciones matriciales y manejo de vectores.
• Pandas, empleado para cargar y manipular los conjuntos de datos.
• Matplotlib, utilizado para la generación de gráficas.
• SciPy, utilizado para la optimización clásica mediante el algoritmo COBYLA y para resolver problemas de asignación mediante el algoritmo Húngaro.
• IPython.display, empleado para mostrar tablas durante la ejecución del notebook.

Además, se fija una semilla aleatoria (SEED = 2026) con el objetivo de garantizar la reproducibilidad de todos los experimentos.

El uso de una semilla fija asegura que procesos estocásticos, como la inicialización de parámetros de QAOA o el muestreo de estados, produzcan resultados consistentes entre distintas ejecuciones del notebook.

16.2 Carga del dataset

El notebook carga la información desde la carpeta data/, donde se almacenan los archivos que contienen la instancia utilizada en el proyecto.

Los archivos principales son:

• housing_deficit.csv
• housing_actions_4x4.csv

El primer archivo contiene la información relacionada con la demanda habitacional, incluyendo el número de viviendas con rezago, sin rezago y el porcentaje de rezago para cada municipio.

El segundo archivo contiene la oferta histórica de acciones de vivienda clasificadas por modalidad.

A partir de estos archivos se construyen tres objetos fundamentales:

• A_df: representa el conjunto de municipios.
• B_df: representa el conjunto de modalidades de vivienda.
• S: matriz de compatibilidad calculada automáticamente.

Separar ambos archivos mantiene la trazabilidad del origen de los datos y facilita futuras actualizaciones del proyecto sin modificar el código principal.

16.3 Construcción automática de la matriz de compatibilidad

Una vez cargados los datos, el notebook calcula automáticamente la matriz de compatibilidad S.

El procedimiento inicia normalizando el porcentaje de rezago habitacional mediante el método Min-Max.

Posteriormente se normaliza, también mediante Min-Max, el número de acciones históricas registradas para cada modalidad de vivienda.

Con ambos indicadores normalizados se calcula el score utilizando la expresión:

Sij = 0.6 · zr(i) + 0.4 · za(i,j)

donde:

• zr(i) representa el rezago normalizado del municipio i.
• za(i,j) representa las acciones históricas normalizadas para la modalidad j en el municipio i.

El peso de 0.6 asignado al rezago prioriza la necesidad social del municipio, mientras que el peso de 0.4 considera la evidencia histórica de implementación de programas de vivienda.

Este procedimiento garantiza que la matriz S no contiene valores arbitrarios, sino resultados obtenidos directamente a partir de los datos oficiales.

16.4 Validación del dataset

Antes de construir el modelo QUBO, el notebook verifica automáticamente que la instancia cumpla las condiciones requeridas.

Se realizan las siguientes comprobaciones:

• A_df contiene exactamente cuatro municipios.
• B_df contiene exactamente cuatro modalidades.
• La matriz S tiene dimensión 4×4.
• Todos los valores de S son numéricos.
• No existen valores infinitos o indefinidos.

Si alguna de estas condiciones no se cumple, la ejecución se detiene inmediatamente mediante instrucciones assert.

Esta etapa evita que errores en los archivos CSV se propaguen al resto del pipeline y garantiza que todas las etapas posteriores trabajen con una instancia válida.

16.5 Definición de variables binarias

Cada posible asignación entre un municipio y una modalidad de vivienda se representa mediante una variable binaria xij.

El significado de esta variable es:

xij = 1  si la modalidad j se asigna al municipio i.
xij = 0  en cualquier otro caso.

Como existen cuatro municipios y cuatro modalidades, el modelo contiene un total de dieciséis variables binarias.

Cada variable se asigna a un índice entero utilizando la relación:

k = i × NB + j

donde NB representa el número de modalidades.

Esta correspondencia permite asociar posteriormente cada variable binaria con un qubit lógico dentro del circuito cuántico implementado mediante QAOA.

16.6 Selección automática de penalizaciones

Para transformar el problema de asignación en un modelo QUBO es necesario incorporar las restricciones mediante términos de penalización.

En este proyecto las constantes de penalización no fueron elegidas manualmente.

El notebook calcula automáticamente el valor de λ mediante la expresión:

λ = ceil(4 × max(|S|) + 1)

donde max(|S|) representa el mayor valor absoluto presente en la matriz de compatibilidad.

Esta estrategia garantiza que cualquier violación de las restricciones incremente la energía del sistema más que cualquier posible mejora obtenida en la función objetivo.

De esta forma, las soluciones óptimas del modelo QUBO corresponden a asignaciones factibles del problema original.

16.7 Construcción de la matriz QUBO

Una vez definidos el conjunto de variables binarias, la función objetivo y las constantes de penalización, el siguiente paso consiste en construir la matriz QUBO del problema.

La formulación QUBO (Quadratic Unconstrained Binary Optimization) permite transformar un problema de optimización con restricciones en un problema de minimización sin restricciones explícitas.

La función de energía utilizada tiene la forma:

E(x)=ΣQkkxk+ΣQklxkxl+offset

donde:

• x representa el vector de variables binarias.
• Q es la matriz QUBO.
• offset es un término constante que no modifica la posición del óptimo.

La construcción de la matriz se realiza en tres etapas.

Primero, cada elemento de la matriz de compatibilidad S se incorpora como un término lineal negativo en la diagonal principal de Q. Esta transformación convierte el problema original de maximización del score en un problema equivalente de minimización de energía.

Posteriormente se agregan las penalizaciones correspondientes a las restricciones de asignación por municipio.

Finalmente se agregan las penalizaciones correspondientes a las restricciones por modalidad.

El resultado es una matriz cuadrada de dimensión 16×16 cuyos elementos representan las interacciones lineales y cuadráticas entre las variables binarias.

Como parte del proceso de validación, el notebook genera una tabla con todos los coeficientes distintos de cero presentes en la matriz QUBO. Esto facilita inspeccionar la estructura del modelo y verificar que las restricciones fueron incorporadas correctamente.

16.8 Funciones auxiliares

Con el objetivo de facilitar el análisis del modelo, el notebook implementa un conjunto de funciones auxiliares que permiten evaluar soluciones binarias y reconstruir la interpretación del problema original.

Entre las funciones implementadas se encuentran:

• qubo_energy()
Calcula la energía QUBO asociada a cualquier vector binario.

• assignment_matrix()
Reconstruye la matriz de asignación 4×4 correspondiente a un bitstring determinado.

• is_feasible()
Verifica automáticamente si una solución satisface las restricciones del problema de matching.

• assignment_score()
Calcula el score total de una solución factible utilizando la matriz S.

• selected_pairs()
Obtiene las asignaciones municipio-modalidad seleccionadas por una solución determinada.

Estas funciones son utilizadas durante todo el pipeline, tanto en la validación clásica como en la simulación cuántica y el análisis de resultados.

Su implementación permite mantener un código modular, reutilizable y fácil de verificar.

# 17. Validación clásica

Antes de ejecutar cualquier algoritmo cuántico resulta indispensable verificar que la formulación matemática del problema sea correcta.

Por esta razón se implementaron dos procedimientos independientes de validación clásica.

El primero consiste en una búsqueda exhaustiva sobre todas las configuraciones binarias posibles.

El segundo utiliza exclusivamente las asignaciones factibles mediante permutaciones.

Si ambos métodos producen exactamente la misma solución óptima, puede concluirse que la formulación QUBO fue construida correctamente.

17.1 Búsqueda exhaustiva

El modelo contiene dieciséis variables binarias.

Por lo tanto, el espacio completo de soluciones contiene:

2¹⁶ = 65 536 configuraciones posibles.

El notebook genera automáticamente todas estas configuraciones mediante operaciones vectorizadas de NumPy.

Posteriormente calcula la energía QUBO asociada a cada estado y determina aquella que presenta el menor valor de energía.

La solución encontrada constituye el óptimo global del problema QUBO.

Debido al reducido tamaño de la instancia, esta búsqueda puede realizarse completamente en memoria utilizando Google Colab sin requerir técnicas adicionales de optimización.

Esta solución sirve como referencia para evaluar posteriormente el desempeño del algoritmo QAOA.

17.2 Validación mediante permutaciones

Aunque el modelo QUBO considera las 65 536 configuraciones binarias posibles, únicamente 24 representan asignaciones perfectas entre municipios y modalidades.

Estas asignaciones corresponden a todas las permutaciones posibles de cuatro elementos.

El notebook genera automáticamente las 24 permutaciones utilizando la función permutations() del módulo itertools.

Para cada permutación se calcula el score total obtenido mediante la matriz de compatibilidad S.

La mejor permutación encontrada representa el óptimo del problema original de asignación.

Finalmente se compara esta solución con el óptimo obtenido mediante la búsqueda exhaustiva del modelo QUBO.

La coincidencia entre ambos resultados confirma que las penalizaciones fueron seleccionadas correctamente y que el modelo QUBO conserva exactamente el problema original.

# 18. Simulación local de QAOA

Una vez validado el modelo QUBO comienza la etapa de optimización cuántica.

El algoritmo utilizado corresponde al Quantum Approximate Optimization Algorithm (QAOA), uno de los algoritmos variacionales más utilizados para resolver problemas de optimización combinatoria.

La simulación se realiza completamente de forma local mediante representación vectorial del estado cuántico.

Como el modelo contiene dieciséis variables binarias, el sistema cuántico posee dieciséis qubits lógicos.

El espacio de Hilbert asociado contiene:

2¹⁶ = 65 536 amplitudes complejas.

Este tamaño resulta suficientemente pequeño para ejecutarse completamente en Google Colab sin exceder la memoria disponible.

La simulación local permite validar el funcionamiento completo del algoritmo antes de ejecutarlo sobre hardware cuántico real.

18.1 Estado inicial

La simulación comienza preparando el estado uniforme

|+⟩⊗16

mediante la aplicación de compuertas Hadamard sobre los dieciséis qubits.

Este estado representa una superposición uniforme sobre todas las configuraciones binarias posibles.

Como consecuencia, todas las soluciones candidatas poseen inicialmente la misma probabilidad.

Posteriormente, las sucesivas capas de QAOA modifican dichas probabilidades utilizando la información contenida en el Hamiltoniano de costo y el Hamiltoniano mixer.

18.2 Hamiltoniano de costo

El Hamiltoniano de costo se construye directamente a partir de las energías del modelo QUBO.

Debido a que dicho Hamiltoniano es diagonal en la base computacional, su aplicación puede implementarse multiplicando cada amplitud compleja por un factor de fase.

Antes de aplicar estas fases, las energías se normalizan mediante un proceso de centrado y escalamiento.

Esta normalización evita que diferencias excesivamente grandes entre energías produzcan fases muy rápidas que dificulten la optimización de los parámetros variacionales.

El Hamiltoniano de costo constituye el mecanismo mediante el cual la información del problema de optimización se incorpora al circuito cuántico.

18.3 Hamiltoniano mixer

Después de aplicar el Hamiltoniano de costo, cada capa de QAOA incorpora un Hamiltoniano mixer.

En este proyecto se utiliza el mixer estándar basado en operadores Pauli-X.

Su función consiste en redistribuir amplitudes entre las distintas configuraciones binarias permitiendo explorar nuevas soluciones durante la optimización.

Este mixer no preserva automáticamente las restricciones del problema de asignación.

Como consecuencia, algunas mediciones pueden corresponder a soluciones no factibles.

Por esta razón el proyecto incorpora posteriormente un procedimiento de reparación clásica que proyecta dichas soluciones sobre el conjunto de asignaciones válidas.

18.4 Construcción del estado QAOA

El estado cuántico utilizado por el algoritmo se construye aplicando de forma alternada el Hamiltoniano de costo y el Hamiltoniano mixer.

Para una profundidad p, el estado final se expresa como

|ψ(γ,β)⟩ = UM(βp) UC(γp) ··· UM(β1) UC(γ1) |+⟩⊗n

donde:

• UC corresponde al operador de costo derivado del modelo QUBO.
• UM representa el operador mixer.
• γ y β son los parámetros variacionales que deben optimizarse.

En este proyecto se utiliza inicialmente una profundidad

p = 1

con el objetivo de mantener tiempos de ejecución reducidos y asegurar compatibilidad con Google Colab y hardware cuántico actual.

La implementación del notebook permite incrementar fácilmente el número de capas modificando la variable QAOA_P, aunque esto incrementa el tiempo de simulación y el número de parámetros que deben optimizarse.

18.5 Optimización clásica de parámetros

QAOA pertenece a la familia de algoritmos variacionales híbridos.

Esto significa que el circuito cuántico no produce directamente la mejor solución, sino que depende de un conjunto de parámetros continuos que deben ajustarse mediante un algoritmo clásico.

En este proyecto se utiliza el algoritmo COBYLA (Constrained Optimization BY Linear Approximation), implementado en la biblioteca SciPy.

El procedimiento consiste en:

1. Generar un conjunto inicial de parámetros (γ, β).
2. Construir el estado QAOA correspondiente.
3. Calcular la energía esperada del Hamiltoniano de costo.
4. Ajustar los parámetros para reducir dicha energía.
5. Repetir el proceso hasta alcanzar el criterio de convergencia o el número máximo de iteraciones.

Para mantener tiempos de ejecución compatibles con Google Colab se utilizaron:

• Un reinicio (restart).
• Un número reducido de iteraciones (MAXITER = 25).

Al finalizar la optimización, el notebook conserva el conjunto de parámetros que produjo la menor energía esperada.

Estos mismos parámetros son utilizados posteriormente para construir el circuito ejecutado en IBM Quantum, evitando repetir la optimización sobre hardware real.

18.6 Muestreo local

Una vez obtenidos los parámetros óptimos, el estado cuántico final se utiliza para generar una distribución de probabilidades sobre todas las configuraciones binarias posibles.

Cada probabilidad representa la posibilidad de observar un determinado bitstring durante una medición.

El notebook realiza un proceso de muestreo utilizando un número fijo de mediciones (shots).

En este proyecto se emplean:

SHOTS_LOCAL = 2000

Cada medición produce un bitstring de dieciséis bits que representa una posible solución del problema.

Todos los bitstrings observados se almacenan utilizando la estructura Counter de Python, permitiendo posteriormente calcular frecuencias, probabilidades y energías asociadas.

La mejor solución observada no necesariamente corresponde al bitstring más frecuente, sino al que presenta la menor energía QUBO entre todas las muestras obtenidas.

18.7 Métricas probabilísticas

Además de identificar la mejor solución encontrada, el proyecto analiza la distribución completa generada por QAOA.

Para ello se calculan diversas métricas probabilísticas.

Entre ellas destacan:

• Probabilidad de obtener soluciones factibles.

Esta métrica representa la suma de probabilidades de todos los estados que satisfacen simultáneamente las restricciones de asignación.

• Probabilidad del óptimo clásico.

Corresponde a la probabilidad asignada por QAOA exactamente a la mejor solución encontrada mediante búsqueda exhaustiva.

• Energía esperada.

Se calcula como el promedio ponderado de las energías de todos los estados utilizando la distribución de probabilidades producida por el circuito.

• Ranking de soluciones.

El notebook identifica los estados con mayor probabilidad y reporta para cada uno:

- energía QUBO;
- score;
- factibilidad.

Estas métricas permiten evaluar no solamente la mejor solución encontrada, sino también la calidad global de la distribución generada por el algoritmo variacional.

18.8 Visualización de resultados

Con el objetivo de facilitar la interpretación del comportamiento del algoritmo, el notebook genera diversas representaciones gráficas.

Entre ellas se incluyen:

• Histograma de energías observadas durante el muestreo.

• Comparación entre la energía óptima clásica y la mejor energía obtenida mediante QAOA.

• Distribución de probabilidades de las soluciones más relevantes.

Estas visualizaciones permiten identificar rápidamente si el algoritmo concentra probabilidad alrededor del óptimo o si, por el contrario, distribuye la amplitud sobre soluciones de menor calidad.

Las gráficas también facilitan comparar el comportamiento de la simulación local con los resultados obtenidos posteriormente sobre hardware cuántico real.

# 19. Ejecución en IBM Quantum

Una vez validados el modelo QUBO y la simulación local, el circuito QAOA puede ejecutarse sobre un procesador cuántico real mediante IBM Quantum.

El notebook construye automáticamente un circuito equivalente al utilizado durante la simulación local.

La construcción conserva:

• El mismo número de qubits.
• La misma profundidad.
• Los mismos parámetros optimizados clásicamente.

Esta estrategia evita realizar la optimización directamente sobre hardware cuántico, lo que reduciría significativamente el número de trabajos (jobs) enviados al sistema.

Posteriormente el circuito es enviado mediante SamplerV2, obteniendo un conjunto de mediciones comparable con el muestreo realizado localmente.

19.1 Extracción de conteos

Los resultados devueltos por SamplerV2 pueden variar ligeramente dependiendo de la versión de Qiskit Runtime utilizada.

Por esta razón el notebook implementa una función de extracción robusta que identifica automáticamente el registro clásico donde se almacenan las mediciones.

El procedimiento convierte los resultados obtenidos por IBM Quantum en un diccionario de conteos cuya estructura es completamente compatible con la utilizada durante la simulación local.

Esta normalización permite comparar directamente ambos conjuntos de resultados utilizando exactamente las mismas funciones de análisis.

# 20. Reparación clásica conservadora

El mixer estándar utilizado por QAOA no garantiza que todas las soluciones observadas satisfagan las restricciones del problema de asignación.

Como consecuencia, algunas mediciones pueden corresponder a bitstrings no factibles.

Para evitar descartar completamente estas muestras, el proyecto incorpora un procedimiento de reparación clásica.

La reparación consiste en proyectar cada solución observada sobre el conjunto de asignaciones factibles utilizando el algoritmo Húngaro.

Durante este proceso se construye una matriz de proyección que combina:

• La matriz de compatibilidad S.
• La información contenida en el bitstring observado.

Posteriormente se resuelve un nuevo problema de asignación que produce la solución factible más cercana.

Es importante destacar que este procedimiento constituye un postprocesamiento híbrido.

Por lo tanto, sus resultados se reportan por separado de los obtenidos directamente mediante el algoritmo cuántico.

20.1 Comparación entre métodos

El notebook genera una tabla comparativa que resume el desempeño de todos los métodos evaluados.

Entre ellos se incluyen:

• Solución clásica exacta.
• Simulación local de QAOA.
• Simulación local con reparación.
• Hardware cuántico.
• Hardware cuántico con reparación.

Para cada método se reportan indicadores como:

• Mejor energía observada.
• Energía promedio.
• Mejor score obtenido.
• Probabilidad de soluciones factibles.
• Probabilidad de observar el óptimo clásico.

Esta comparación permite evaluar objetivamente las diferencias entre los distintos enfoques utilizados durante el proyecto.

# 41. Interpretación mínima del resultado
# 1. ¿Cuál fue la mejor asignación encontrada?

La mejor asignación corresponde al emparejamiento uno-a-uno entre municipios y modalidades de vivienda que minimiza la energía del QUBO.
Esto indica qué modalidad de vivienda se asigna prioritariamente a cada municipio dentro de la instancia 4×4.

# 2. ¿Cuál fue su score en el dominio?

El score corresponde a la suma de compatibilidades S
ij
	​

 de la asignación encontrada.
Un score más alto indica mejor alineación entre el nivel de rezago habitacional del municipio y la modalidad de vivienda asignada.

# 3. ¿La asignación cumple todas las restricciones?

Sí. La solución es factible si cumple que cada municipio recibe exactamente una modalidad y cada modalidad se asigna exactamente una vez.
Esto equivale a un matching bipartito perfecto.

# 4. ¿QAOA local observó el óptimo clásico?

Se verifica comparando la mejor solución obtenida por QAOA con la solución óptima encontrada por búsqueda exhaustiva.
En el caso ideal, ambas soluciones coinciden; si no, QAOA se considera una aproximación del óptimo.

# 5. ¿Qué tan frecuente fue observar soluciones factibles?

Se mide mediante la probabilidad de factibilidad, calculada como la proporción de muestras de QAOA que cumplen las restricciones del problema.
Este valor indica qué tan bien el algoritmo respeta la estructura del matching bipartito.

# 6. ¿Qué limitaciones tiene el modelo 4×4?

El modelo es una simplificación del problema real:

Solo incluye 4 municipios y 4 modalidades.
No incorpora restricciones presupuestales reales.
No modela dinámicas temporales ni políticas públicas completas.
El score es una aproximación basada en normalización.
# 7. ¿Qué cambiaría si el dataset creciera?

Si el dataset aumenta:

El espacio de búsqueda crece factorialmente.
Aumenta el número de qubits necesarios.
La simulación clásica deja de ser viable.
Serían necesarias técnicas de reducción de dimensión o heurísticas más avanzadas.
# 8. ¿Qué riesgos éticos existen y cómo se mitigaron?

El principal riesgo es interpretar el modelo como una herramienta real de política pública.

Otros riesgos:

Sesgo en la construcción del score.
Simplificación excesiva de un problema social complejo.
Sobreinterpretación de resultados.

Mitigación:

Uso estrictamente académico.
Documentación clara de supuestos.
Separación entre simulación y decisiones reales.
# 9. Si se usó hardware real, ¿cómo compara contra QAOA local?

El hardware cuántico introduce ruido y errores que pueden degradar la calidad de las soluciones.
QAOA local representa el caso ideal sin ruido, por lo que suele producir resultados más estables.

# 10. Si se usó reparación clásica, ¿qué parte del resultado corresponde al postprocesamiento híbrido?

La reparación clásica no forma parte del algoritmo cuántico.
Se trata de un postprocesamiento que proyecta soluciones no factibles hacia soluciones válidas utilizando un algoritmo clásico (Hungarian algorithm).

Por lo tanto, los resultados reparados deben interpretarse como híbridos (cuántico + clásico).

20.2 Gráficas comparativas

Finalmente, el notebook genera un conjunto de gráficas que resumen visualmente los resultados obtenidos.

Entre las métricas representadas se encuentran:

• Energía mínima observada.

• Energía promedio de muestreo.

• Probabilidad de obtener soluciones factibles.

• Probabilidad de observar el óptimo clásico.

Estas gráficas facilitan la comparación entre la simulación local y el hardware cuántico, permitiendo identificar el efecto del ruido cuántico y evaluar el beneficio obtenido mediante el procedimiento de reparación clásica.

