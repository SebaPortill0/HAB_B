# Meta Expert

Un proyecto para agentes de IA versátiles que pueden ejecutarse con modelos propietarios o completamente de código abierto. El meta experto tiene dos agentes: un [Meta Agent](Docs/Meta-Prompting%20Overview.MD) básico, y [Jar3d](Docs/Introduction%20to%20Jar3d.MD), un agente más sofisticado y versátil.

## Tabla de Contenidos

1. [Conceptos Clave](#conceptos-clave)
2. [Requisitos Previos](#requisitos-previos)
3. [Configuración](#configuración)
   - [Configuración de Claves API](#configuración-de-claves-api)
   - [Configuración de Endpoints](#configuración-de-endpoints)
4. [Configuración del Meta Agent Básico](#configuración-del-meta-agent-básico)
5. [Configuración de Jar3d](#configuración-de-jar3d)
   - [Configuración con Docker para Jar3d](#configuración-con-docker-para-jar3d)
   - [Interacción con Jar3d](#interacción-con-jar3d)
6. [Hoja de Ruta para Jar3d](#hoja-de-ruta-para-jar3d)

## Conceptos Clave

Este proyecto se basa en cuatro conceptos principales:

1. Meta prompting: Para más información, consulta el artículo sobre **Meta-Prompting** ([fuente](https://arxiv.black/pdf/2401.12954)).
2. Cadena de razonamiento (*Chain of Reasoning*): Para Jar3d, aprovechamos una adaptación de [Chain-of-Reasoning](https://github.com/ProfSynapse/Synapse_CoR).
3. Jar3d utiliza generación aumentada por recuperación (*Retrieval-Augmented Generation* o RAG), lo cual no se utiliza en el Meta Agent Básico. 
4. Jar3d puede generar gráficos de conocimiento a partir de páginas web, lo que le permite producir salidas más completas. (PRÓXIMAMENTE... integración con Neo4j)

## Requisitos Previos

1. Clonar este proyecto en tu entorno de trabajo/directorio local:
   ```bash
   git clone https://github.com/SebaPortill0/HAB_B.git
   ```

2. Necesitarás tener instalados Docker y Docker Compose para poner en marcha el proyecto:
   - [Docker](https://www.docker.com/get-started)
   - [Docker Compose](https://docs.docker.com/compose/install/)

3. **Si deseas usar recuperación híbrida, necesitarás crear una cuenta gratuita en Neo4j Aura:**
   - [Neo4j Aura](https://neo4j.com/)

## Configuración

1. Navega al repositorio:
   ```bash
   cd /path/to/your-repo/HAB_B
   ```

2. Abre el archivo `config.yaml`:
   ```bash
   nano config/config.yaml
   ```

### Configuración de Claves API

Ingresa las claves API para tu proveedor de LLM elegido:

- **Clave API de Serper:** Consíguela en [https://serper.dev/](https://serper.dev/)
- **Clave API de OpenAI:** Consíguela en [https://openai.com/](https://openai.com/)
- **Clave API de Gemini:** Consíguela en [https://ai.google.dev/gemini-api](https://ai.google.dev/gemini-api)
- **Clave API de Claude:** Consíguela en [https://docs.anthropic.com/en/api/getting-started](https://docs.anthropic.com/en/api/getting-started)
- **Clave API de Groq:** Consíguela en [https://console.groq.com/keys](https://console.groq.com/keys)

*Para recuperación híbrida, necesitarás una clave API de Claude.*

### Configuración de Endpoints

Establece la variable `LLM_SERVER` para elegir tu proveedor de inferencia. Valores posibles:

- openai
- mistral
- claude
- gemini (Todavía no soportado)
- ollama (Todavía no soportado)
- groq
- vllm (Todavía no soportado)

Ejemplo:

```yaml
LLM_SERVER: claude
```

Recuerda mantener privado tu archivo `config.yaml` ya que contiene información sensible.

## Configuración del Meta Agent Básico

El meta agent básico es una primera versión del proyecto. Demuestra meta prompting pero no es una herramienta útil para investigación. Usa un enfoque ingenuo de analizar toda una página web y alimentar esa información al agente meta, quien continúa la tarea o da una respuesta final.

### Ejecutar tu consulta en la terminal

```bash
python -m agents.meta_agent
```

Luego ingresa tu consulta.

## Configuración de Jar3d

Jar3d es un agente más sofisticado que utiliza RAG, Cadena de Razonamiento y Meta-Prompting para completar tareas de investigación de larga duración.

*Nota: Actualmente, los mejores resultados se obtienen con Claude 3.5 Sonnet y Llama 3.1 70B. Los resultados con GPT-4 son inconsistentes.*

### Configuración con Docker para Jar3d

Jar3d puede ejecutarse usando Docker para una configuración y despliegue más sencillo.
Es necesario configurar LLM Sherpa: https://github.com/nlmatics/nlm-ingestor

#### Requisitos Previos

- [Docker](https://www.docker.com/get-started)
- [Docker Compose](https://docs.docker.com/compose/install/)

#### Inicio Rápido

1. Clona el repositorio:
   ```bash
   git clone https://github.com/SebaPortill0/HAB_B.git
   cd HAB_B
   ```

2. Construye y ejecuta los contenedores:
   ```bash
   docker-compose up --build
   ```

3. Accede a Jar3d:
   Una vez ejecutándose, accede a la interfaz web de Jar3d en `http://localhost:8000`.

Puedes finalizar tu sesión de Docker presionando `Ctrl + C` o `Cmd + C` en tu terminal y ejecutando:
```bash
docker-compose down
```

#### Notas

- La configuración de Docker incluye Jar3d y el servicio NLM-Ingestor.
- Playwright y sus dependencias de navegador están incluidas para capacidades de scraping web.
- Ollama no está incluido en esta configuración Docker. Si lo necesitas, configúralo por separado y ajústalo en `config.yaml`.
- La configuración se maneja a través de `config.yaml`, no de variables de entorno en docker-compose.

Para solucionar problemas, revisa los logs de los contenedores:

```bash
docker-compose logs
```

### Interacción con Jar3d

Una vez configurado, Jar3d se presentará y hará algunas preguntas. Estas preguntas están diseñadas para ayudarte a refinar tus requisitos. Cuando sientas que has proporcionado toda la información relevante a Jar3d, puedes finalizar la parte de preguntas del flujo de trabajo escribiendo `/end`.
