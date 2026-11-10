# Prerequisites Knowledge Graph

<a target="_blank" href="https://cookiecutter-data-science.drivendata.org/">
    <img src="https://img.shields.io/badge/CCDS-Project%20template-328F97?logo=cookiecutter" />
</a>

Un grafo de conocimiento dirigido que conecta conceptos matemáticos mediante relaciones is-prerequisite-of, extraído de textos educativos, y que se utiliza para generar itinerarios de aprendizaje ordenados hacia un concepto objetivo.

## Project Organization

```
├── LICENSE            <- Open-source license if one is chosen
├── Makefile           <- Makefile with convenience commands like `make data` or `make train`
├── README.md          <- The top-level README for developers using this project.
├── data
│   ├── external       <- Data from third party sources.
│   ├── interim        <- Intermediate data that has been transformed.
│   ├── processed      <- The final, canonical data sets for modeling.
│   └── raw            <- The original, immutable data dump.
│
├── docs               <- A default mkdocs project; see www.mkdocs.org for details
│
├── models             <- Trained and serialized models, model predictions, or model summaries
│
├── notebooks          <- Jupyter notebooks. Naming convention is a number (for ordering),
│                         the creator's initials, and a short `-` delimited description, e.g.
│                         `1.0-jqp-initial-data-exploration`.
│
├── pyproject.toml     <- Project configuration file with package metadata for 
│                         prerequisites_knowledge_graph and configuration for tools like black
│
├── references         <- Data dictionaries, manuals, and all other explanatory materials.
│
├── reports            <- Generated analysis as HTML, PDF, LaTeX, etc.
│   └── figures        <- Generated graphics and figures to be used in reporting
│
├── requirements.txt   <- The requirements file for reproducing the analysis environment, e.g.
│                         generated with `pip freeze > requirements.txt`
│
├── setup.cfg          <- Configuration file for flake8
│
└── prerequisites_knowledge_graph   <- Source code for use in this project.
    │
    ├── __init__.py             <- Makes prerequisites_knowledge_graph a Python module
    │
    ├── config.py               <- Store useful variables and configuration
    │
    ├── dataset.py              <- Scripts to download or generate data
    │
    ├── features.py             <- Code to create features for modeling
    │
    ├── modeling                
    │   ├── __init__.py 
    │   ├── predict.py          <- Code to run model inference with trained models          
    │   └── train.py            <- Code to train models
    │
    └── plots.py                <- Code to create visualizations
```

--------

## 1. Motivacion

El conocimiento matemático es acumulativo: comprender un concepto determinado suele depender de haber interiorizado primero un conjunto específico de ideas previas. Hacer explícita esa estructura de dependencia en forma de grafo, en lugar de dejarla implícita en el orden que por casualidad utilice un libro de texto en particular, convierte la “comprensión de las matemáticas” en algo sobre lo que un sistema puede realmente razonar: qué ideas dan acceso a cuáles y cuál es el camino más corto hacia cualquier concepto dado.


## 2. Planteamiento del problema

Construir un grafo dirigido de conceptos matemáticos vinculados por aristas `is-prerequisite-of`, extraídos de textos educativos, y utilizarlo para desarrollar una herramienta que, dado un concepto de destino, devuelva el conjunto ordenado mínimo de prerequisitos que un estudiante debe dominar primero.

## 3. Trabajos relacionados

- **KnowEdu** (Chen et al., *IEEE Access*, 2018) combina un extractor de conceptos basado en el etiquetado de secuencias con la extracción de reglas de asociación a partir de datos sobre el rendimiento de los estudiantes para construir un grafo de conocimiento educativo, e incluye un ejemplo demostrativo de matemáticas en el que se presentan los valores de F1 y AUC mencionados anteriormente. ([DOI](https://doi.org/10.1109/access.2018.2839607))
- **Math-KG** (arXiv:2205.03772, 2022) etiquetas *Entidad conceptual* y *Entidad teorema* mencionadas directamente en textos matemáticos mediante el etiquetado de secuencias BIO; una plantilla directa para el esquema de entidades de este proyecto. ([arXiv](https://arxiv.org/abs/2205.03772))
- **LectureBank** (arXiv:1811.12181, 2019) Estableció la tarea de "aprendizaje en cadena de requisitos previos" y la metodología de construcción de grafos conceptuales, que se aplicaron originalmente al contenido de las clases del curso de PLN; este proyecto adapta ese mismo marco de trabajo a las matemáticas. ([arXiv](https://arxiv.org/abs/1811.12181))
- **ACE** (*Journal of Educational Data Mining*, 16(2), 2024) - califica los pares de conceptos candidatos según la similitud de sus representaciones y solo envía los pares con mayor puntuación a un evaluador humano para su etiquetado, lo que reduce considerablemente el esfuerzo de anotación; se puede reutilizar directamente para mantener baja la carga de etiquetado manual de este proyecto. ([DOI](https://doi.org/10.5281/zenodo.14250896))
- **Manrique** (Tesis de maestria, Universidad de los Andes, 2019) compara una heurística simple basada en enlaces de un grafo de conocimiento con un enfoque de aprendizaje automático supervisado para la identificación de requisitos previos; una referencia útil y de bajo esfuerzo que conviene implementar primero. ([URL](https://repositorio.uniandes.edu.co/server/api/core/bitstreams/77635595-097f-4919-88f2-2e716100a5c2/content))
- **Curriculum knowledge-gap detection via LLMs** (arXiv:2606.10736, 2026) Muestra cómo GPT-4 extrae temas y relaciones de requisitos previos dirigidas del material de la lección mediante un protocolo de tres etapas (extracción de temas → consolidación de límites → evaluación de relaciones por pares); una plantilla para la línea de base de extracción basada en LLM de este proyecto. ([arXiv](https://arxiv.org/abs/2606.10736))

## 4. Datos

### 4.1 Conjuntos de datos con relaciones de requisitos previos explícitos

Estos recursos contienen pares de requisitos previos anotados y pueden utilizarse directamente para el entrenamiento supervisado, las pruebas comparativas y la evaluación de modelos de extracción de requisitos previos.

#### 4.1.1 AL-CPL / Prerequisite-Learning-Dataset

Conjunto de datos fundamental para el entrenamiento y la evaluación de clasificadores de relaciones de requisitos previos. Basado en el conjunto de datos AL-CPL (Liang et al., 2018) y el conjunto de datos de mapas conceptuales de Wiki (Wang et al., 2016), proporciona pares de requisitos previos etiquetados, artículos de Wikipedia y contenido normalizado de libros de texto para Geometría (89 conceptos, 1,681 pares, 524 requisitos previos) y Precálculo (224 conceptos, 2,060 pares, 699 requisitos previos), con la Física como un dominio adicional. Este conjunto de datos es el recurso que mejor se alinea con el enfoque matemático de este proyecto, ya que ofrece tanto anotaciones a nivel de concepto como un contexto textual rico para la extracción de características.

https://github.com/itsshivampal/Prerequisite-Learning-Dataset

Liang, C., Ye, J., Wang, S., Pursel, B., & Giles, C. L. (2018). Investigating Active Learning for Concept Prerequisite Learning. Proceedings of the AAAI Conference on Artificial Intelligence, 32(1).

#### 4.1.2 LectureBank

Un conjunto de datos compuesto por 1,352 archivos de clases en inglés (51,939 diapositivas) de cursos universitarios, clasificados según una taxonomía existente, con 208 temas de relaciones de requisitos previos etiquetados manualmente. Aunque su ámbito principal es el procesamiento del lenguaje natural (NLP) y campos relacionados, proporciona un estándar de referencia rigurosamente anotado para el aprendizaje de cadenas de requisitos previos y puede servir de base para la transferencia de metodología a las matemáticas.

https://github.com/Yale-LILY/LectureBank

Li, I., Fabbri, A. R., Tung, R. R., & Radev, D. R. (2019). What Should I Learn First: Introducing LectureBank for NLP Education and Prerequisite Chain Learning. Proceedings of the AAAI Conference on Artificial Intelligence, 33(01), 6674–6681.

### 4.2 Corpus de textos educativos para la extracción de relaciones

Estos corpus proporcionan material textual sin procesar a partir del cual se pueden extraer relaciones de requisitos previos mediante técnicas de procesamiento del lenguaje natural, como la extracción basada en patrones, la puntuación basada en incrustaciones o el uso de prompts en modelos de lenguaje a gran escala.

#### 4.2.1 Libros de texto abiertos y recursos educativos abiertos

Los libros de texto de matemáticas de alta calidad y de libre acceso de OpenStax, CK-12 y la Open Textbook Library ofrecen contenido estructurado (índice, secciones de los capítulos, secuencia pedagógica) del cual se pueden extraer los requisitos previos. El propio conjunto de datos «Prerequisite-Learning-Dataset» proporciona datos normalizados de libros de texto de geometría, incluyendo secciones del índice y contenido de los capítulos, lo que sirve como un ejemplo concreto de cómo se pueden procesar los datos de los libros de texto para esta tarea.


OpenStax Mathematics: https://openstax.org/subjects/math

CK-12 Mathematics: https://www.ck12.org/student/

Open Textbook Library: https://open.umn.edu/opentextbooks/

#### 4.2.2 Catálogos de cursos universitarios
Los catálogos de instituciones como el MIT, Princeton y la CMU incluyen descripciones de cursos y requisitos previos formales. Estos sirven como fuentes semiestructuradas para extraer dependencias a nivel de curso, las cuales pueden alinearse con extracciones a nivel de concepto para la construcción de grafos de granularidad múltiple

MIT Course Catalog: https://catalog.mit.edu/

Princeton Course Catalog: https://catalog.princeton.edu/

CMU Course Catalog: https://coursecatalog.web.cmu.edu/

#### 4.2.3 Wikipedia y Wikidata
Los artículos de Wikipedia sobre conceptos matemáticos suelen describir temas fundamentales en sus secciones introductorias. El conjunto de datos de mapas conceptuales de Wikipedia (Wang et al., 2016), que sirvió de base para AL-CPL, demuestra que es posible extraer hipótesis previas a partir del contenido de Wikipedia. Además, los datos de clics de Wikipedia pueden utilizarse para inferir dependencias conceptuales basadas en los patrones de navegación de los usuarios.

https://dumps.wikimedia.org/ | https://www.wikidata.org/

### 4.3 Planes de estudio estructurados y grafos de conocimiento
Estas fuentes ofrecen estructuras predefinidas de conceptos y relaciones, que pueden utilizarse para su adaptación, validación o como estándares de referencia.

#### 4.3.1 Indic-Concept-Graph (NCERT)
Un gráfico multilingüe de requisitos previos de conceptos para el plan de estudios escolar de la India (grados 6 a 10), que abarca Física, Química, Matemáticas y Biología. A partir de un nombre de concepto en cualquier idioma de la India, devuelve los requisitos previos, los conceptos dependientes o la ruta de aprendizaje más corta. Este recurso muestra una implementación lista para producción de la búsqueda de requisitos previos y la generación de rutas de aprendizaje, y ofrece un punto de referencia multilingüe para la extensión interlingüística del proyecto.

https://github.com/joyboseroy/indic-concept-graph

#### 4.3.2 Atlas of Knowledge
Un mapa del conocimiento universitario que combina explicaciones de conceptos con requisitos previos explícitos y aristas de relación, procedencia verificable y generación determinista de versiones. Los nodos son cursos, las aristas son requisitos previos y los nodos se organizan por profundidad topológica. Resulta útil como recurso de validación para cadenas de requisitos previos a nivel macro.

https://github.com/EthanVieira/atlas-of-knowledge

#### 4.3.3 OSSU Mathematics Curriculum
Un plan de estudios completo de matemáticas de pregrado para el autoaprendizaje, en el que los cursos ya están ordenados según sus requisitos previos y se comparan con la Guía CUPM de 2015. Ofrece una estructura de itinerarios de aprendizaje ya preparada que puede servir como referencia para evaluar la calidad de la generación de itinerarios.

https://github.com/ossu/math | https://math.ossu.dev


## 5. Metodología

1. **Extracción de conceptos**: NER sobre textos de libros de texto o de Wikipedia (etiquetado BIO, siguiendo el esquema de entidades de Math-KG) mediante un clasificador basado en MathBERT o la extracción con modelos de lenguaje grande (LLM) de pocos ejemplos.
2. **Generación y puntuación de pares candidatos**: se incorporan los conceptos (Sentence-BERT / MathBERT) y se puntúan los pares según su proximidad semántica, siguiendo el enfoque al estilo ACE de clasificar los pares antes de cualquier etiquetado.
3. **Clasificación de relaciones**: un clasificador supervisado (un transformador ajustado o un modelo ligero sobre pares etiquetados manualmente) determina la dirección y la validez de los requisitos previos; se compara con una línea de base de LLM con pocos ejemplos.
4. **Ensamblaje de grafos y garantía de consistencia**: se insertan aristas en un grafo dirigido; se detectan y resuelven los ciclos para preservar la propiedad de grafo aciclo dirigido (DAG) requerida para un orden de aprendizaje válido.
5. **Generación de la ruta de aprendizaje**: el recorrido topológico inverso a partir de un concepto de destino produce la ruta ordenada de requisitos previos.

## 6. Tecnologías

| Capa | Herramientas |
|---|---|
| Lenguaje/entorno de ejecución | Python |
| PLN | spaCy (análisis sintáctico, NER), Hugging Face Transformers (MathBERT), Sentence-Transformers |
| Referencia de extracción | API de LLM (extracción de temas/vértices con pocos ejemplos) |
| Clasificación de relaciones | Transformer ajustado o clasificador ligero (scikit-learn / small BERT head) |
| Almacenamiento de grafos | Neo4j (grafo de propiedades, consultas de ruta con Cypher) |
| Algoritmos de grafos | NetworkX (detección de ciclos, ordenación topológica) |