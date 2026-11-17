# Proyecto Final de Bioinformática 2026

**Respuesta transcripcional al Huanglongbing en portainjertos de cítricos tolerante y susceptible**

Materia Bioinformática, 4° año de Bioingeniería. Docente: Lic. Esp. Ignacio Garcia Labari.

---

## La pregunta

> **¿Qué diferencia la respuesta a la infección por *Candidatus* Liberibacter asiaticus entre un
> portainjerto de cítrico tolerante (US-897) y uno susceptible (mandarina 'Cleopatra')?**

No existen cultivares de cítricos resistentes al HLB, pero sí genotipos tolerantes: plantas que se
infectan igual pero no colapsan. Entender en qué se diferencia su respuesta es lo que podría
orientar el mejoramiento hacia portainjertos que aguanten la enfermedad.

El Huanglongbing es la enfermedad más destructiva de la citricultura mundial y no tiene cura. En
Argentina se detectó por primera vez en Misiones en 2012 y hoy existe un sistema de vigilancia del
que participan SENASA, la EEAOC, el INTA Famaillá, el Ministerio de Producción de Tucumán y AFINOA.

## Cómo se descompone

El **bloque 1** caracteriza al patógeno: reconstruye la filogenia de *Candidatus* Liberibacter a
partir del gen 16S rDNA, con las siete secuencias argentinas publicadas por Badaracco et al. (2017),
e identifica qué especie circula acá y hasta qué nivel taxonómico alcanza ese marcador. El resultado
define sobre qué tipo de datos se puede responder la pregunta: como el 16S no resuelve cepas, con
datos de campo no se podría separar la contribución del patógeno de la del hospedante, así que hace
falta un experimento donde el patógeno esté controlado por diseño.

El **bloque 2** mide la respuesta del hospedante sobre ese experimento: una matriz de expresión
génica que compara los dos portainjertos, infectados y sanos, con minería de datos y machine
learning. El núcleo es la comparación de la respuesta a la infección medida por separado en cada
genotipo.

## Datos

| | Bloque 1 | Bloque 2 |
|---|---|---|
| Fuente | GenBank (NCBI) | GEO (NCBI), serie **GSE30502** |
| Contenido | 12 secuencias 16S rDNA de *Ca.* Liberibacter | Microarray Affymetrix Citrus Genome Array |
| Diseño | 7 secuencias argentinas + 5 referencias | 2 genotipos × 2 tratamientos × 3 réplicas |
| Paper cabecera | Badaracco et al. (2017) | Albrecht y Bowman (2012) |

Ambos conjuntos son de acceso público y el notebook los descarga solo, sin credenciales.

## Papers de referencia

- Badaracco, A., Redes, F.J., Preussler, C.A. y Agostini, J.P. (2017). *Citrus huanglongbing in
  Argentina: detection and phylogenetic studies of 'Candidatus Liberibacter asiaticus'*.
  Australasian Plant Pathology, 46(2), 171-175. Acceso abierto en CONICET Digital:
  http://ri.conicet.gov.ar/handle/11336/72858
- Albrecht, U. y Bowman, K.D. (2012). *Transcriptional response of susceptible and tolerant citrus
  to infection with 'Candidatus Liberibacter asiaticus'*. Plant Science, 185-186, 118-130.
- Bové, J.M. (2006). *Huanglongbing: a destructive, newly-emerging, century-old disease of citrus*.
  Journal of Plant Pathology, 88(1), 7-37.
- Jia, H. et al. (2017). *Genome editing of the disease susceptibility gene CsLOB1 in citrus confers
  resistance to citrus canker*. Plant Biotechnology Journal, 15(7), 817-823.
- Peng, A. et al. (2017). *Engineering canker-resistant plants through CRISPR/Cas9-targeted editing
  of the susceptibility gene CsLOB1 promoter in citrus*. Plant Biotechnology Journal, 15(12),
  1509-1519.

## Técnicas de la materia aplicadas

| Clase | Tema | Dónde aparece |
|-------|------|---------------|
| 13 | Filogenia molecular, alineamiento múltiple | Bloque 1, secciones 3 a 6 |
| 15 | Árboles de decisión | Sección 10 |
| 16 | Random Forest | Sección 10 |
| 17 | Máquinas de vectores soporte (SVM) | Sección 10 |
| 18 | Redes neuronales profundas | Sección 12, breve, para mostrar el límite del tamaño muestral |
| 19 | PCA y minería de datos | Sección 8 |
| 20 | Clustering (k-means y jerárquico) | Sección 9 |
| - | Expresión diferencial y corrección por FDR | Sección 10 (agregado, consultado con el docente) |

## Cómo ejecutarlo

El notebook está pensado para Google Colab y no necesita configuración previa.

1. Abrir `Proyecto_Final_HLB.ipynb` en Colab.
2. Cambiar el mail de contacto de Entrez en la celda de configuración (el NCBI lo exige para
   descargar secuencias).
3. Ejecutar todas las celdas en orden (Entorno de ejecución → Ejecutar todas).

Tiempo aproximado de ejecución completa: entre 20 y 30 minutos. Las partes más lentas son las
pruebas de permutación de la sección 11 y el entrenamiento repetido de la red neuronal en la
sección 12.

### Dependencias

Se instalan desde el propio notebook: `biopython`, `muscle`, `mafft`. El resto (`pandas`, `numpy`,
`scikit-learn`, `scipy`, `matplotlib`, `tensorflow`) ya viene en Colab.

## Archivos del repositorio

```
├── Proyecto_Final_HLBioinformatica_solanagalindez_.ipynb
│                                 Notebook con todo el análisis, ejecutado de punta a punta
├── Informe_Proyecto_Final_GalindezSolana.pdf
│                                 Informe escrito del proyecto
├── datos_hlb.csv                 Matriz de expresión usada (12 muestras x 2000 sondas + etiquetas)
├── README.md                     Este archivo
├── papers/REFERENCIAS.md         Citas, DOI y enlaces a los dos papers de referencia
└── *.png                         Las 16 figuras que genera el notebook
```

Al ejecutar el notebook se crean además dos carpetas que no están versionadas, porque su contenido
se reconstruye solo: `datos/`, con las secuencias descargadas de GenBank, el alineamiento, la matriz
de distancias Kimura 2-parámetros, el árbol en formato Newick, la tabla de anotación de la
plataforma y los resultados de expresión diferencial; y `figuras/`, con las mismas figuras PNG que
están en la raíz de este repositorio.

## Principales resultados

**El 16S rDNA no resuelve cepas de *Ca.* L. asiaticus.** El marcador separa con claridad las tres
especies del género: la distancia K2P media entre especies (0,0303) es unas 9 veces la distancia
media dentro de *Ca.* L. asiaticus (0,0034), y los rangos no se superponen. Pero las siete secuencias
argentinas no forman un clado propio: la referencia de Florida quedó ubicada entre ellas, y no hubo
agrupamiento por localidad, hospedante ni año. Importante: eso **no** prueba que la bacteria sea
genéticamente uniforme, solo que esta región no tiene variación suficiente para resolver por debajo
del nivel de especie. Es el marcador correcto para diagnóstico, no para rastrear el origen de una
introducción.

**La variación dominante en el hospedante es el genotipo, no la infección.** PC1 explica el 77,3% de
la varianza y separa los cultivares; PC2 explica el 16,5% y separa infectados de controles. Con k = 4,
k-means y el clustering jerárquico recuperan los cuatro grupos experimentales sin un error (ARI = 1).
El análisis gen por gen da 3023 sondas diferenciales entre genotipos contra 232 entre tratamientos.

**La respuesta a la pregunta central: la planta tolerante no responde distinto, responde igual y
mucho menos.** Midiendo la respuesta a la infección por separado en cada genotipo, 'Cleopatra'
susceptible tiene 1617 sondas que responden y US-897 tolerante ninguna que supere los umbrales. La
pendiente entre ambas respuestas es 0,265, o sea que para los mismos genes el tolerante se mueve
alrededor de una cuarta parte, y la correlación de 0,554 indica que se mueve en la misma dirección.
No son dos programas de defensa distintos, es el mismo programa atenuado.

La diferencia entre los dos cultivares ya estaba antes de la infección: comparando el contraste de
genotipo medido solo en plantas sanas contra el medido solo en infectadas, la correlación es 0,893 y
el 74% de las sondas se mantiene. La tolerancia es **constitutiva**, no inducida por el patógeno.

Un chequeo importante: la variabilidad interna de los dos genotipos es prácticamente idéntica (razón
de 1,01), así que el contraste entre 1617 y 0 no se explica por diferencias de potencia estadística.
Separando los criterios del filtro, en la susceptible 7574 sondas pasan el filtro estadístico; en la
tolerante, ninguna. La tolerante tiene 323 sondas con cambios grandes pero que no se repiten entre
sus réplicas, así que la lectura precisa es que **no muestra una respuesta reproducible** a la
infección.

**Limitación principal:** la carga bacteriana de cada planta no está en los datos públicos, así que
no se puede distinguir si US-897 tolera la misma infección o si directamente se infecta menos. Son
dos mecanismos distintos de tolerancia y para el mejoramiento no da lo mismo cuál sea.

**El gen cuya respuesta más difiere es una lectina específica del floema (tipo PP2)**, que sube unas
500 veces en la susceptible y casi nada en la tolerante. El HLB es una enfermedad del floema, así
que el análisis está capturando biología del tejido donde ocurre la enfermedad.

**El número de genes de respuesta a la infección es del mismo orden que el del paper.** Los 326 genes
que reportan Albrecht y Bowman corresponden a la respuesta a la infección, así que el contraste
comparable es infectado contra control: 232 sondas, el mismo orden de magnitud. El contraste de
genotipo (3023 sondas) mide otra cosa y no es comparable con ese número.

**Las exactitudes altas de clasificación no son mérito del modelo.** Con 12 muestras y 2000 variables
cualquier clasificador separa perfectamente. El trabajo lo cuantifica con una prueba de permutación y
comparando la selección de variables dentro y fuera del ciclo de validación cruzada. Por el mismo
motivo se descarta la optimización de hiperparámetros del SVM. Las redes neuronales profundas de la
Práctica 18 se aplican igual, pero no como clasificador sino como demostración: 555.330 parámetros
entrenables contra 8 ejemplos de entrenamiento, una relación de casi 70.000 a 1, con un conjunto de
validación de 4 muestras cuya resolución mínima es de 25 puntos porcentuales.

## Limitaciones

El tamaño muestral de 12 arrays es la limitación principal y condiciona todas las conclusiones sobre
clasificación. Los datos de expresión provienen de un ensayo realizado en Florida con genotipos que
no son necesariamente los del NOA, así que las conclusiones sobre tolerancia no son trasladables sin
más a la citricultura tucumana. La anotación de las sondas viene de la plataforma GPL5731, que se
diseñó a partir de ESTs y no de un genoma anotado: buena parte de las sondas no tiene un símbolo de
gen asignado y su descripción es la similitud con un gen de otra especie, no una identificación
firme. El análisis no incluye enriquecimiento de
términos GO, que sería el paso siguiente natural.
