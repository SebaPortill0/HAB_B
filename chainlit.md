# Introducción a Jar3d

## ¿Qué es Jar3d?
Jar3d es un agente versátil para tareas de investigación intensiva que combina [chain-of-reasoning](https://github.com/ProfSynapse/Synapse_CoR), Meta-Prompting y técnicas de Agentic RAG.

- Cuenta con integraciones con proveedores populares y modelos de código abierto, permitiendo una operación 100% local si se dispone de los recursos de hardware necesarios.

### Cómo usarlo
Jar3d se presentará y te ayudará a refinar tus requisitos. Una vez que hayas proporcionado toda la información requerida, escribe `/end`. Jar3d enviará tus requisitos a MetaExpert, quien trabajará con su equipo de expertos para cumplir con tus necesidades.

### Casos de uso y aplicaciones
- Tareas de investigación de larga duración, redacción de revisiones de literatura, boletines informativos, etc.
- Posible adaptación para su uso con documentos internos de la empresa, sin necesidad de acceso a internet.
- Puede funcionar como asistente de investigación o una versión local de servicios como Perplexity.
- Investigación de mercado.
- Sourcing de productos (por ejemplo, "Encuéntrame el Wagyu A5 más barato de proveedores franceses").

## Cómo funciona Jar3d

### Ingeniería de Prompts
Jar3d utiliza dos prompts potentes escritos completamente en Markdown:
1. Jar3d Meta-Prompt
2. Jar3d Requirements Prompt

Ambos prompts incorporan adaptaciones de la técnica Chain of Reasoning.

### Arquitectura de Jar3d
La arquitectura de Jar3d incorpora aspectos de Meta-Prompting, Agentic RAG y una adaptación de [Chain of Reasoning](https://github.com/ProfSynapse/Synapse_CoR).


### Mecanismo de recuperación de Jar3d para investigación en Internet

Este sistema emplea un mecanismo de recuperación sofisticado para realizar investigaciones en internet. El proceso incluye varios pasos, utilizando diversas herramientas y técnicas para garantizar resultados completos y relevantes.

#### 1. Descubrimiento de páginas web

- Utiliza la herramienta SERPER para encontrar páginas web relevantes.
- Emplea un algoritmo de búsqueda ejecutado por LLM, expresado en lenguaje natural.
- Cada iteración del algoritmo genera una consulta de búsqueda para SERPER.
- SERPER devuelve una página de resultados del motor de búsqueda (SERP).
- Otra llamada LLM selecciona la URL más apropiada del SERP.
- Este proceso se repite un número predeterminado de veces para compilar una lista de URLs para investigación en profundidad.

#### 2. Extracción y segmentación de contenido

- Emplea [LLM Sherpa](https://github.com/nlmatics/llmsherpa) como un ingestor de documentos.
- Segmenta inteligentemente el contenido de cada URL en la lista compilada.
- Esto resulta en un corpus de texto segmentado a lo largo de todas las URLs acumuladas.

#### 3. Embedding de texto

- Realiza embeddings del texto segmentado usando un modelo alojado localmente de [FastEmbed](https://qdrant.github.io/fastembed/#installation).
- Indexa los embeddings en una tienda de vectores en memoria [FAISS](https://api.python.langchain.com/en/latest/vectorstores/langchain_community.vectorstores.faiss.FAISS.html).

#### 4. Búsqueda por similitud

- Realiza la recuperación usando una búsqueda por similitud sobre la tienda de vectores FAISS.
- Utiliza la similitud coseno entre los embeddings indexados y el meta-prompt (escrito por el meta-agente).
- Recupera la información más relevante en función de esta medida de similitud.

#### 5. Re-ranking

- Emplea [FlashRank](https://github.com/PrithivirajDamodaran/FlashRank) como un servicio de re-ranking alojado localmente.
- FlashRank utiliza cross-encoders para evaluar de manera más precisa la relevancia de los documentos con respecto a la consulta.

#### 6. Selección final

- Selecciona un percentil designado de los documentos con mayor puntuación de los resultados reordenados.
- Pasa este conjunto final de documentos recuperados al meta-agente para su procesamiento o análisis posterior.