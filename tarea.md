## **1. Introducción**

El **análisis de datos ómicos**, como la **secuenciación masiva de ADN
(NGS)**, se ha convertido en una herramienta fundamental para estudiar
la **diversidad genética y funcional** de los organismos y sus
comunidades. Gracias a la disponibilidad de **repositorios públicos como
NCBI SRA o ENA**, es posible acceder a **grandes volúmenes de datos de
secuenciación**. El tratamiento de estos datos permite obtener
información relevante sobre **ensamblado genómico, identificación de
variantes o análisis de expresión diferencial**, contribuyendo así al
conocimiento de distintos **procesos biológicos y enfermedades**.

Los corales son organismos clave en los **ecosistemas marinos
tropicales**, pero en las últimas décadas, las enfermedades infecciosas
se han consolidado como una de las principales causas de su declive a
nivel global. Desde el enfoque ecológico y microbiano, la **teoría de la
enfermedad en corales** sostiene que su salud depende del equilibrio
entre el hospedador, su microbioma y el ambiente. Cuando este equilibrio
se rompe (principalmente por el aumento de la temperatura del agua o la
contaminación) ciertos patógenos bacterianos oportunistas proliferan y
desencadenan enfermedades como la “*white plague*”, que provoca necrosis
tisular rápida y mortal.

Un ejemplo claro de esta interacción es la bacteria *Thalassotalea
loyana*, identificada como la causa de la *white plague-like disease* en
el coral. Esta bacteria invade los tejidos del coral, provoca su lisis y
compromete la estructura del arrecife. La relación coral-bacteria se
convierte entonces en el eje de la patología, donde el coral actúa como
hospedador, la bacteria como agente patógeno, y el ambiente como
modulador de la virulencia.

Ante la falta de un **sistema inmune adaptativo** en los corales y la
**inviabilidad ecológica del uso de antibióticos** en ambientes marinos
abiertos, surge la **fagoterapia** como una alternativa terapéutica
incipiente y prometedora. Esta estrategia utiliza **bacteriófagos
específicos** (virus que infectan bacterias) para atacar y eliminar
patógenos sin dañar el resto del microbioma ni afectar al hospedador. En
el caso de la *Thalassotolea loyana*, el **fago BA3** ha demostrado
experimentalmente su capacidad para infectar y destruir a esta bacteria,
logrando detener la progresión de la enfermedad en acuarios e incluso
evitar la transmisión a corales sanos.

La **fagoterapia**, aunque todavía en etapa experimental en ambientes
marinos, representa un enfoque innovador y respetuoso con el ecosistema.
Su uso podría no sólo **reducir la carga bacteriana patógena**, sino
también convertirse en una **herramienta clave para mitigar la
propagación de enfermedades en arrecifes coralinos**, protegiendo así
uno de los ecosistemas más amenazados del planeta.

En este contexto, profundizar en la **relación entre el coral, la
bacteria patógena y el fago** permite no sólo comprender mejor los
mecanismos que intervienen en la aparición y progresión de estas
enfermedades, sino también explorar nuevas alternativas de control
basadas en la biotecnología. El **análisis genómico y funcional** de
cada uno de estos actores ofrece una visión integrada del **sistema
coral-bacteria-fago** y abre la puerta a evaluar el **potencial de la
fagoterapia** como estrategia emergente para la protección y
recuperación de los ecosistemas coralinos.

## **2. Objetivos**

El **objetivo principal** de este trabajo es **analizar la dinámica
genómica** en la **interacción entre una especie de coral, una bacteria
patógena específica y su fago**, a partir de **datos de secuenciación
masiva (NGS)**. Para ello, se implementará un **pipeline
bioinformático** que permita **identificar y caracterizar** los
**elementos genéticos clave** involucrados en esta interacción
tripartita. Los objetivos específicos son:

-   Obtener y preprocesar datos de secuenciación de los genomas del
    **coral, la bacteria y el fago**, asegurando su calidad mediante
    herramientas como **FastQC y Trimmomatic**.

-   **Realizar el ensamblaje y anotación de los genomas** para
    identificar genes asociados a **virulencia bacteriana, resistencia a
    fagos y respuesta inmune del coral**, utilizando herramientas como
    **SPAdes, Prokka y MAKER**.

-   Analizar la presencia de elementos móviles y genes de resistencia
    mediante la identificación de **profagos**, con herramientas como
    **PHASTER y PhageScope**.

-   **Comparar la estructura genómica y relaciones filogenéticas** entre
    los organismos involucrados, evaluando similitudes con bases de
    datos de referencia mediante **BLAST y Kraken**.

Este enfoque permitirá **comprender mejor los mecanismos genéticos que
regulan la interacción coral-bacteria-fago**, proporcionando información
relevante para el estudio de la **dinámica ecológica y la biología de
los corales en contextos de infección**.

## **3. Metodología y resultados**

### **3.1. Adquisición de datos:**

Las secuencias genómicas utilizadas en este análisis provienen de bases
de datos públicas como **NCBI**, desde donde se han descargado los
genomas de *Acropora millepora* (coral), *Thalassotalea loyana*
(bacteria) y el **bacteriófago BA3**.

Para obtener los archivos en formato **FASTA (.fna)**, NCBI ofrece dos
versiones del ensamblado: **GCA (Genomic Contig Assembly)**, que
representa un ensamblado sin curación adicional, y **GCF (Genomic Contig
Finished)**, que corresponde a la versión revisada y aceptada en
**RefSeq**. En este estudio, se utilizará la versión **GCF** por su
mayor fiabilidad y curación.

Para acceder a los datos crudos de secuenciación en formato **FASTQ**,
se recurre al **SRA (Sequence Read Archive) de NCBI**, una base de datos
pública que almacena lecturas obtenidas mediante tecnologías de
secuenciación como **Illumina, PacBio o Oxford Nanopore**. Estos
archivos **FASTQ** contienen tanto las secuencias de ADN/ARN como sus
valores de calidad, lo que permite realizar análisis de calidad con
herramientas como **FastQC** para los procedentes de Ilumina y
**Nanoplot** para los procedentes de PacBio u Oxford Nanopore.

Para descargar los archivos FASTQ desde **NCBI**, se debe seguir el
siguiente procedimiento:

1.  Acceder a **NCBI** y buscar la herramienta **“NCBI SRA Tools”** en
    el panel izquierdo.

2.  Introducir el **SRA ID** correspondiente.

3.  Seleccionar la opción para descargar en **formato FASTQ**.

4.  Una vez descargado, correr **FastQC** o **Nanoplot** en **Galaxy**
    para evaluar la calidad de las lecturas.

En el caso del **fago BA3**, el mensaje *“No items found”* en SRA indica
que no existen datos crudos de secuenciación disponibles en formato
**FASTQ**, por lo que sólo se dispone de su archivo **FASTA**.

### **3.2. Preprocesamiento de datos:**

Para realizar el control de calidad de las secuencias, utilizaremos la
herramienta **FastQC** disponible en **Galaxy**. Esta herramienta nos
permitirá evaluar la calidad de las lecturas antes de proceder con el
análisis, identificando posibles problemas como **sesgo de
secuenciación, adaptadores remanentes o baja calidad en ciertas
posiciones**.

El procedimiento en **Galaxy** es el siguiente:

1.  **Acceder a Galaxy** a través de
    [usegalaxy.org](https://usegalaxy.org/).

2.  **Subir los archivos FASTQ** obtenidos desde **SRA** o previamente
    descargados.

3.  **Buscar la herramienta “FastQC” o “Nanoplot”** en el panel de
    herramientas de la izquierda.

4.  **Seleccionar los archivos FASTQ** correspondientes al **coral y la
    bacteria** para ejecutar el análisis.

5.  **Ejecutarlo y esperar los resultados**, que incluirán gráficos
    sobre la calidad de las lecturas.

6.  **Interpretar los resultados** para determinar si es necesario
    realizar algún filtrado o recorte antes del ensamblaje.

Posteriormente, si la calidad de las lecturas no es óptima, se puede
hacer uso de la herramienta **Trimmomatic** para quitar las peores
lecturas y mejorar la calidad total del conjunto de las mismas.

**Trimmomatic** es una herramienta de preprocesamiento de datos de
secuenciación, utilizada para filtrar y recortar lecturas en archivos
FASTQ con el fin de mejorar la calidad de los datos antes del análisis
bioinformático. Su objetivo principal es **eliminar bases de baja
calidad, adaptadores y regiones problemáticas**, optimizando así los
datos de secuenciación para ensamblajes, alineamientos o cualquier otro
análisis posterior.

En este caso, el análisis se ha realizado para el **coral (*Acropora
millepora*)** y la **bacteria (*Thalassotalea loyana*)**.

#### **3.2.1. Análisis de calidad - coral (*Acropora millepora*):**

Para el análisis del coral, se emplea la herramienta **NanoPlot**, ya
que los datos de secuenciación correspondientes fueron generados
mediante tecnología **PacBio**. A diferencia de **Illumina**, que
produce lecturas cortas, **PacBio** genera lecturas largas que requieren
herramientas específicas para evaluar su calidad y distribución.
NanoPlot es especialmente útil para este tipo de datos, permitiendo una
caracterización detallada de la longitud, calidad y rendimiento de las
lecturas obtenidas.

<figure>
<img src="/home/alumno08/ADO/pacbio.png"
alt="Captura donde se observa que la tecnología de secuenciación del coral fue PacBio." />
<figcaption aria-hidden="true">Captura donde se observa que la
tecnología de secuenciación del coral fue PacBio.</figcaption>
</figure>

El **informe de NanoPlot** muestra que se obtuvieron **19.247 lecturas
PacBio** con un total de 235,5 millones de bases. La longitud media de
lectura es de aproximadamente 12.237 pb y la mediana es de 11.752 pb,
con una N50 de 14.125 pb, lo cual indica una **buena consistencia en la
longitud de las lecturas**. La calidad media y mediana de las lecturas
es de **Q30**, lo que refleja una excelente calidad en general. Además,
el 100% de las lecturas supera los umbrales de calidad Q10, Q15, Q20 y
Q25, aunque ninguna alcanza un valor superior a Q30, lo que sugiere que
Q30 es el máximo registrado. También se observan lecturas largas de
hasta 57.314 pb, manteniendo esa misma calidad, lo cual es ideal para
ensamblajes o análisis estructurales complejos. En conjunto, los datos
indican un **rendimiento robusto y de alta calidad** para estudios
genómicos.

<figure>
<img src="/home/alumno08/ADO/nanoplot_report.png"
alt="Captura del resultado de la ejecución de Nanoplot." />
<figcaption aria-hidden="true">Captura del resultado de la ejecución de
Nanoplot.</figcaption>
</figure>

#### **3.2.2. Análisis de calidad - bacteria (*Thalassotalea loyana*):**

El análisis de calidad de las lecturas del coral muestra que, en
general, los datos son adecuados para su uso posterior. La **calidad por
base** es buena en prácticamente todas las posiciones, sin advertencias
de baja calidad, y no se detectan problemas en la **calidad por tile**,
lo que indica que la secuenciación funcionó correctamente. Además, la
**distribución de calidades por secuencia** muestra valores altos, lo
que sugiere que la mayoría de las lecturas son confiables. En cuanto a
la composición, el **contenido por base** no presenta anomalías
significativas. El contenido de **GC (39%)** se encuentra dentro de un
rango aceptable, y no se detectan bases **N** en exceso, lo que confirma
la buena calidad del *dataset*. La **distribución de longitud de las
secuencias** varía ampliamente entre **35 y 301 pb**, lo que podría
deberse a un recorte en los datos o a la combinación de diferentes
estrategias de secuenciación (combinando Illumina y Nanopore, por
ejemplo). Respecto a la duplicación y adaptadores, no se observan
**secuencias sobre-representadas ni un alto grado de duplicación**, lo
que descarta contaminación significativa en el *dataset*.

<figure>
<img src="/home/alumno08/ADO/f.png"
alt="Captura del FASTQC realizado a la bateria." />
<figcaption aria-hidden="true">Captura del FASTQC realizado a la
bateria.</figcaption>
</figure>

La calidad de las secuencias ha mejorado tras el uso de **Trimmomatic**.
Antes de aplicar Trimmomatic, las lecturas mostraban una caída
pronunciada en la calidad hacia el final, con valores por debajo de
**Q30** y una alta variabilidad en las últimas bases. Tras el
procesamiento, la calidad se mantiene estable en la mayoría de la
secuencia, reduciendo el ruido en los extremos y manteniendo un **perfil
más uniforme**. Aunque sigue habiendo una ligera disminución de calidad
en las últimas bases, la caída es menos pronunciada y la mayoría de las
bases se encuentran en la **zona verde**, indicando una calidad
aceptable para análisis posteriores.

En conclusión, **Trimmomatic ha sido efectivo en mejorar la calidad del
dataset**, eliminando bases problemáticas y disminuyendo la variabilidad
en los extremos. No obstante, si aún persisten regiones de baja calidad,
se podría aplicar un recorte más agresivo o un filtrado adicional según
los requerimientos del análisis.

<figure>
<img src="/home/alumno08/ADO/t.png"
alt="Captura del FASTQC realizado a la bacteria tras usar Trimmomatic." />
<figcaption aria-hidden="true">Captura del FASTQC realizado a la
bacteria tras usar Trimmomatic.</figcaption>
</figure>

### **3.3. Análisis genómico comparativo:**

#### **3.3.1. Ensamblado en SPAdes:**

Utilizamos **SPAdes** como ensamblador de genomas por su capacidad de
procesar lecturas cortas de secuenciación y generar contigs de alta
calidad. SPAdes es ampliamente utilizado en genómica de pequeños
organismos y metagenómica por su eficiencia y precisión, especialmente
en muestras con posibles mezclas de ADN, como ocurre en corales que
funcionan como holobiontes.

Se accedió a la herramienta desde **Galaxy** y para obtener los
ensamblados elegimos la opción *Isolate: highly recommended for
high-coverage isolate and multi-cell data (–isolate)* como *pipeline*
para el análisis.

Del coral se obtienen 48 contigs. Un estracto del primero:

    >NODE_1_length_623049_cov_55.640268
    TAACCGACGAATTTACTTGCTTCATCTACTGCGTTAACTCATTTATTGAGAAACAAAGAA
    ATTATGGCTCGAAAAAAAGCAAAGATTTTTATGGTTTAACTTGACGTGGGGAGTCATACC
    AACGCCCAACTAAGGGGCAAATAATTAGCTTGGCTAAAATTTTGAGCGCAGCGACAAAAG
    CCAAGCTTTATTTGTCCCGCTTTAGTTACTTGTTAGCTGTTTACTTCAATTAATTGAGCC
    AACACTACAAAACCACCCCACCAAAAGCAAACCCAAAGCATAGATACAATTGCTTTCCGT
    CGAAAACTTTTACCAACTGAGGTTAAACAATTTGGATACCATATAACATTTACAGGTGTG
    ATTTTAGTGTGAAAACTTTCAAAAGCTGCCTTTGTGGTATTTTGAGTTGCTTTTACAAAA
    AAGTAGACTGCCTTAACAAAACAAATAATTCCTAACACCATCGAAAATGGGAAAAGGATT
    ACTAAGAGAAAGTTAATTATTTGCACACTTCACACCTAAACCTCAATGAACAGCTAACCA
    TTTAAATAAACGGCGGCGTAGCAAGTCCAATTTAATTTGCTTGTTATGTTGCCACTACTG
    AAATTAAGGAAATAAAAATGACAGACCAAGAATTAAAAAATATAGATGAAGATTTTGTTT
    GTGTATGGGAGTAAGAAGTAAAAAGCTCATTAAGTGCTAATGGTAAATCAGGAGGAAATA
    AGAAAAGTATTGAAAAATTATTTGTGCACAACTCACAAGTTGATTGTGCATCATAACAGT
    TTATTAGGCGGAACCATTCCGTGTTTCACACAGCTTAGGTTTCCACCTTTTTACATAATA
    ATAAAAATTTACACGCTAACTTATTGCTTGTAAACAATAAATATTTTTAGCTATGGAGAA
    ATGCGGAATTTATACCAATGTTTGGCGCACGTTGGGAATCGAATACAACTTGTATAAATA
    AACAGTAATCATTATTCAAATAGTTTTTATGGTTTAAACAAAAAACTCGGTATCGTTAAA
    ...

De la bacteria se obtienen 88 contings. Un estracto del primero:

    >NODE_1_length_601090_cov_47.056704
    CCTCATAAACATTTACTAATTCAGGCGCGGGATGGAGCAGCCTGGTAGCTCGTCGGGCTC
    ATAACCCGAAGGTCGTCGGTTCAAATCCGGCTCCCGCAACCAATTCTTCCTTTAGAATTA
    TTGCCTAGGTCAGTGGTAACATGACCTCATAAACATTTACTAATTCAGGCGCGGGATGGA
    GCAGCCTGGTAGCTCGTCGGGCTCATAACCCGAAGGTCGTCGGTTCAAATCCGGCTCCCG
    CAACCAATTCTTCCTTTAGAATTATTGCCTAAGGTCAGTGGTAACATGACCTCTTAAACA
    TTTACTAAATTCAGGCGCGGGATGGAGCAGCCTGGTAGCTCGTCGGGCTCATAACCCGAA
    GGTCGTCGGTTCACTCTTTACAAACAAGGGCTCCCACAACCAACTTTCTCTGATTACTAC
    ATCACGCTTCCCAACGACCTAAGTAATTAGCTAGCCAAACTACCTACCCTACAAGCTAAT
    TGTAATTCCCATAGAATTATTCATGGTTATATATTAGGCACAAAAAAAGGCCGATTAATC
    AGCCTTTTTCATTAAATAAGTAATTTCTAGAATGCGTAAGTCACATTAAAGTATTGGGCC
    ACACCTGTCGTATCTACAGGTTTACCGTCGAGTATCGTACCATCTTTCCATTGCGTCATG
    TCATCATAAAACTTGAGGCCATAACCTACCTTGAAGTTATGGTTAGGCATGTTGTACCAA
    ACACCTAAGTAAGTTTGTAAAGAAGAATCTGTACGGTATTCCTGTTCAAAAGCTGTACCT
    TTGTCTAAATCGCTACCAAATTCATAATCAAAATAGCCTTGGAAGCTCAAAAATGATTTA
    ...

#### **3.3.2. Anotación funcional de genes:**

Para la anotación de los genomas obtenidos, se utilizaron herramientas
como **Prokka** y **MAKER**, las cuales generan archivos en formato
**GBK (GenBank)** que contienen información detallada sobre las
características genómicas, como genes, ARN, regiones codificantes y
otras anotaciones funcionales. Estos archivos pueden ser visualizados y
explorados fácilmente con programas como **Artemis**, una herramienta
gratuita y de código abierto desarrollada por el *Sanger Institute*, o
con **Geneious**, una plataforma más completa y con interfaz gráfica
intuitiva, aunque de carácter **comercial** y de **pago**. Estas
herramientas permiten revisar, editar y validar visualmente las
anotaciones obtenidas, facilitando la interpretación biológica del
contenido genómico.

#### **3.3.3. Comparación genómica con BLAST:**

Se realizó un análisis de similitud utilizando **BLAST** en el clúster
del máster, enfrentando los genomas del coral y la bacteria. Para ello,
se construyó una base de datos BLAST a partir del genoma del coral y se
compararon contra ella las secuencias de la bacteria. El objetivo era
identificar posibles transferencias horizontales de genes o regiones
compartidas, pero **no se encontraron similitudes significativas**. Del
mismo modo, se llevó a cabo un BLAST entre el genoma del **fago** y el
de la **bacteria** para comprobar si existían **restos genómicos del
fago integrados en la bacteria**, lo que podría indicar un **ciclo
lisogénico**. Sin embargo, **tampoco se detectaron coincidencias**, lo
que sugiere que el fago actúa únicamente de forma **lítica**, sin
integrarse en el genoma bacteriano.

#### **3.3.4. Aproximación taxonómica bacteriana:**

**Kraken2** ha procesado 1.59 millones de secuencias y ha generado un
archivo de clasificación en formato tabular en el que se observa que
*Saccharomyceta* tiene 139 secuencias asignadas, lo que sugiere que este
grupo es dominante en la muestra. Otras especies como *Scheffersomyces
stipitis* (118 secuencias) también están bien representadas.
*Tetrapisispora blattae* y *Yarrowia lipolytica* tienen solo 1
secuencia, lo que indica que son presencias marginales.

#### **3.3.5. Análisis genómico del fago BA3:**

Para el análisis del genoma del fago BA3, se utilizaron las bases de
datos **Virus-Host DB**. V-HDB (*Virus-Host Database*) es una base de
datos que relaciona virus con sus hospedadores conocidos. Permite
explorar con qué bacterias (u otros organismos) puede interactuar o
infectar un fago como BA3, aportando contexto ecológico y funcional
sobre su ciclo de vida y su rol en la microbiota. Se pretende por tanto
obtener información sobre posibles hospedadores bacterianos, además de
relaciones evolutivas con otros virus similares. Según la información
disponible, el hospedador de este fago es la bacteria marina
*Thalassotalea loyana* (TAX:280483), perteneciente a la familia
*Alteromonadaceae* dentro del filo *Gammaproteobacteria*. El fago
identificado como **Thalassomonas phage BA3** (TAX:469660) se clasifica
dentro del dominio *Viruses*, orden *Caudoviricetes*, y está asociado al
genoma completo registrado con el identificador NC\_009990. Además, se
encuentra vinculado a la base de datos KEGG BRITE (entrada NC\_009990),
lo que facilita la integración funcional y comparativa con rutas
metabólicas y otros organismos. Esta información permite contextualizar
la relación hospedador-virus y profundizar en el análisis genómico del
fago BA3 desde una perspectiva filogenética y funcional.

<figure>
<img src="/home/alumno08/ADO/captura%20.png"
alt="Captura de la búsqueda en Virus-Host DB." />
<figcaption aria-hidden="true">Captura de la búsqueda en Virus-Host
DB.</figcaption>
</figure>

Durante el análisis del fago, se observó una aparente inconsistencia en
los nombres científicos utilizados en distintas bases de datos.
Históricamente, la bacteria fue aislada y descrita como *Thalassomonas
loyana*, especialmente en estudios relacionados con enfermedades
coralinas como la *white plague-like disease*. Sin embargo, con el
avance de los estudios filogenéticos y el análisis de secuencias como el
16S rRNA y genomas completos, se propuso en 2014 la reclasificación de
esta especie bajo un nuevo género, pasando a denominarse *Thalassotalea
loyana*.

Este cambio taxonómico aún no ha sido reflejado de forma uniforme en
todas las bases de datos. Por ejemplo, el fago **BA3**, que fue aislado
originalmente de esta bacteria antes del cambio de clasificación, sigue
apareciendo en NCBI asociado al nombre antiguo del hospedador
(*Thalassomonas loyana*). Esta discrepancia no representa un error, sino
un desfase común en la actualización de nombres científicos en registros
públicos, que puede afectar la interpretación de los resultados si no se
tiene en cuenta el contexto histórico y taxonómico.

Con el objetivo de evaluar la relación entre el fago **BA3** y
diferentes organismos hospedadores, se realizaron análisis de
alineamiento mediante **BLAST**. El genoma completo del fago
*Thalassomonas* phage BA3 mostró alineamientos significativos con
diversas cepas de *Vibrio* y con su propio genoma depositado en GenBank
(NC\_009990.1), indicando una alta similitud con otros fagos
relacionados y secuencias bacterianas marinas. No obstante, al realizar
un BLAST directo entre el genoma de la bacteria *Thalassotalea loyana
LMG 22536* (secuenciada como BSSV01000001.1) y el genoma del fago BA3,
**no se encontraron alineamientos significativos**, lo cual resulta
llamativo dada la supuesta relación hospedador-fago.

<figure>
<img src="/home/alumno08/ADO/blast%20fago.png"
alt="Captura del BLAST del fago BA3." />
<figcaption aria-hidden="true">Captura del BLAST del fago
BA3.</figcaption>
</figure>

<figure>
<img src="/home/alumno08/ADO/blastfagobacteria.png"
alt="Captura del BLAST de la bacteria y su fago BA3." />
<figcaption aria-hidden="true">Captura del BLAST de la bacteria y su
fago BA3.</figcaption>
</figure>

Para comprobar si el fago **BA3** está integrado en el genoma de su
hospedador como un **profago** (es decir, en un estado lisogénico), se
utilizó la herramienta **PhageScope**, que permite predecir el estilo de
vida de un fago en función de su genoma completo. Esta herramienta
aplica algoritmos de predicción basados en factores genéticos asociados
a la virulencia y a la integración cromosómica, permitiendo diferenciar
entre fagos **líticos** (que destruyen la célula hospedadora tras la
infección) y **lisogénicos** (que se integran en el genoma del
hospedador en forma de profagos). El resultado obtenido indica que el
fago BA3 presenta un estilo de vida **lítico**, lo que confirma la
hipótesis de que **no se encuentra integrado en el genoma de la
bacteria** *Thalassotalea loyana*. Esta predicción complementa y
respalda los resultados del análisis BLAST, en el cual no se detectaron
alineamientos significativos entre el genoma bacteriano y el del fago,
descartando su presencia como profago.

<figure>
<img src="/home/alumno08/ADO/phagescope.png"
alt="Captura de PhageScope con los trabajos en cola." />
<figcaption aria-hidden="true">Captura de PhageScope con los trabajos en
cola.</figcaption>
</figure>

<figure>
<img src="/home/alumno08/ADO/aaa.png"
alt="Captura de la predicción del estilo de vida del fago." />
<figcaption aria-hidden="true">Captura de la predicción del estilo de
vida del fago.</figcaption>
</figure>

<figure>
<img src="/home/alumno08/ADO/PHAGESCOPE2.png"
alt="Captura de la predicción de los factores de virulencia y genes antimicrobianos." />
<figcaption aria-hidden="true">Captura de la predicción de los factores
de virulencia y genes antimicrobianos.</figcaption>
</figure>

Como parte del análisis funcional del fago BA3, se utilizó el **módulo
de detección de factores de virulencia y genes de resistencia
antimicrobiana** de PhageScope. Este análisis tiene como objetivo
evaluar el potencial riesgo del fago en aplicaciones terapéuticas o
biotecnológicas, descartando la presencia de **genes que puedan conferir
ventajas patogénicas a bacterias o resistencia a antibióticos**. Los
resultados obtenidos mostraron que no se detectaron genes de virulencia
ni de resistencia antimicrobiana en el genoma del fago BA3, lo cual
refuerza su perfil de seguridad. No obstante, para obtener una
evaluación más detallada y complementaria de posibles elementos móviles
o integraciones relacionadas con fagos, se decidió realizar un análisis
más específico utilizando la herramienta PHASTEST, especializada en la
identificación de profagos y elementos relacionados en genomas
bacterianos.

<figure>
<img src="/home/alumno08/ADO/PHASTEST-circular-ZZ_371646dc48.png"
alt="Captura de la anotación funcional de los genes." />
<figcaption aria-hidden="true">Captura de la anotación funcional de los
genes.</figcaption>
</figure>

<figure>
<img src="/home/alumno08/ADO/PHASTEST2.png"
alt="Captura con los detalles de los resultados de la anotación funcional de genes." />
<figcaption aria-hidden="true">Captura con los detalles de los
resultados de la anotación funcional de genes.</figcaption>
</figure>

<figure>
<img src="/home/alumno08/ADO/tree.png"
alt="Captura con los detalles de los resultados de la anotación funcional de genes." />
<figcaption aria-hidden="true">Captura con los detalles de los
resultados de la anotación funcional de genes.</figcaption>
</figure>

A partir del genoma completo de **Thalassomonas phage BA3**
(NC\_009990.1), PHASTEST identificó una única región fágica con alta
confianza, abarcando aproximadamente 37.9 kb con una identidad del 67.6%
respecto a otros fagos conocidos, y clasificada como una región intacta.
Este resultado se representa gráficamente mediante un **mapa circular
anotado**, donde se visualizan genes virales, genes bacterianos y
características estructurales clave. La clasificación como “intacta”
refuerza la idea de que BA3 corresponde a un fago completo y activo, y
no a un fragmento génico o un profago. Esta evidencia complementa la
predicción realizada con PhageScope, que indicó un estilo de vida
lítico, y apoya su uso en estudios funcionales o terapéuticos donde es
importante descartar elementos lisogénicos o incompletos.

Por último, se pueden implementar herramientas como **ViPTree** (*Viral
Proteomic Tree*), una herramienta en línea que permite comparar
**genomas virales completos y construir árboles filogenéticos** basados
en la similitud de sus proteínas. Se utiliza principalmente para
analizar y clasificar virus, especialmente bacteriófagos, a partir de
sus genomas ensamblados.

## **4. Conclusiones**

Este trabajo ha permitido explorar la interacción entre el **coral, una
bacteria patógena y su fago asociado** mediante herramientas
bioinformáticas aplicadas a datos genómicos públicos.

El **análisis de calidad** confirmó que las lecturas del coral eran
aptas para el ensamblaje, mientras que las de la bacteria fueron
descartadas por baja calidad.

A través de **BLAST** y **PhageScope**, se confirmó que el fago BA3
tiene un **estilo de vida lítico** y **no está integrado en el genoma
bacteriano**. Además, no se detectaron genes de **virulencia ni
resistencia antimicrobiana**, y **PHASTEST** validó que se trata de un
**fago completo e intacto**.

En conjunto, los resultados destacan el valor de integrar herramientas
de análisis genómico para estudiar las relaciones entre microorganismos
y su papel en la salud del ecosistema coralino.

## **5. Bibliografía**

-   Bankevich, A., Nurk, S., Antipov, D., Gurevich, A. A., Dvorkin, M.,
    Kulikov, A. S., Lesin, V. M., Nikolenko, S. I., Pham, S.,
    Prjibelski, A. D., Pyshkin, A. V., Sirotkin, A. V., Vyahhi, N.,
    Tesler, G., Alekseyev, M. A., & Pevzner, P. A. (2012). SPAdes: A New
    Genome Assembly Algorithm and Its Applications to Single-Cell
    Sequencing. *Journal Of Computational Biology*, 19(5), 455-477.
    <https://doi.org/10.1089/cmb.2012.0021>

-   Andrews, S. (n.d.). FastQC A Quality Control tool for High
    Throughput Sequence Data.
    <http://www.bioinformatics.babraham.ac.uk/projects/fastqc/>

-   Wood, D. E., & Salzberg, S. L. (2014). Kraken: ultrafast metagenomic
    sequence classification using exact alignments. Genome Biology,
    15(3), R46. <https://doi.org/10.1186/gb-2014-15-3-r46>

-   Bolger, A. M., Lohse, M., & Usadel, B. (2014). Trimmomatic: a
    flexible trimmer for Illumina sequence data. Bioinformatics, 30(15),
    2114–2120. <https://doi.org/10.1093/bioinformatics/btu170>

-   Wang, R. H., Yang, S., Liu, Z., Zhang, Y., Wang, X., Xu, Z., Wang,
    J., & Li, S. C. (2023). PhageScope: a well-annotated bacteriophage
    database with automatic analyses and visualizations. *Nucleic Acids
    Research*, 52(D1), D756-D761. <https://doi.org/10.1093/nar/gkad979>

-   Arndt, D., Marcu, A., Liang, Y., & Wishart, D. S. (2017). PHAST,
    PHASTER and PHASTEST: Tools for finding prophage in bacterial
    genomes. *Briefings In Bioinformatics*, 20(4), 1560-1567.
    <https://doi.org/10.1093/bib/bbx121>

-   Bouras, G., Nepal, R., Houtak, G., Psaltis, A. J., Wormald, P.-J., &
    Vreugde, S. (2022). Pharokka: a fast scalable bacteriophage
    annotation tool. *Bioinformatics*, 39(1).
    <https://doi.org/10.1093/bioinformatics/btac776>

-   Nishimura, Y. et al. ViPTree: the viral proteomic tree server.
    *Bioinformatics*, 33, 2379–2380, <doi:10.1093/bioinformatics/btx157>
    (2017)

-   Chen LH, Yang J, Yu J, Yao ZJ, Sun LL, Shen Y and Jin Q, 2005. VFDB:
    a reference database for bacterial virulence factors. *Nucleic Acids
    Res*. 33(Database issue):D325-D328.

-   Mihara, T., Nishimura, Y., Shimizu, Y., Nishiyama, H., Yoshikawa,
    G., Uehara, H., Hingamp, P., Goto, S., and Ogata, H.; Linking virus
    genomes with host taxonomy. *Viruses* 8, 66 <doi:10.3390/v8030066>
    (2016).

-   Syberg-Olsen, M. J., Garber, A. I., Keeling, P. J., McCutcheon, J.
    P., & Husnik, F. (2022). Pseudofinder: Detection of Pseudogenes in
    Prokaryotic Genomes. *Molecular Biology And Evolution*, 39(7).
    <https://doi.org/10.1093/molbev/msac153>

-   Efrony, R., Atad, I. & Rosenberg, E. Phage Therapy of Coral White
    Plague Disease: Properties of Phage BA3. *Curr Microbiol* 58,
    139–145 (2009). <https://doi.org/10.1007/s00284-008-9290-x>

## **6. Anexos**

El trabajo se encuentra disponible en un repositorio de Github:

Los archivos anexos a este trabajo se encuentran disponibles en una
carpeta que se envía adjunta en la entrega del mismo.
