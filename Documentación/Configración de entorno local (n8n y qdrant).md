## Implementación local de n8n y Qdrant mediante Docker

Esta es la primera etapa de configuración. Aquí he preparado un entorno local para ejecutar **n8n** y **Qdrant** en mi computadora, para ello he utilizado **Docker** como plataforma de contenerización. El objetivo fue disponer de un ambiente aislado y reproducible para desarrollar y probar futuros workflows sin depender inicialmente de infraestructura en la nube.

### 1. Verificación del entorno de virtualización

Inicialmente comprobé que Windows contara con los componentes necesarios para ejecutar contenedores Linux. Además verifiqué que la **virtualización estuviera habilitada** y que **WSL 2 (Windows Subsystem for Linux)** estuviera correctamente instalado y funcionando.

La configuración es:

- Distribución utilizada: **Ubuntu**
- Versión de WSL: **WSL 2**
- Kernel Linux ejecutándose mediante WSL 2.
- Arquitectura: **x86_64**

Para comprobar que el kernel Linux estaba funcionando correctamente ingresé al entorno Ubuntu mediante `wsl` y se utilizó `uname -a`.

### 2. Verificación de Docker

Una vez realizado lo anterior probé el funcionamiento de **Docker Desktop** mediante:

```bash
docker info
```

Esto me sirve para confirmar que Docker podía comunicarse correctamente con su motor y que estaba preparado para ejecutar contenedores Linux utilizando la infraestructura proporcionada por WSL 2.

He usaso Docker para evitar instalar y administrar directamente en Windows cada aplicación junto con sus dependencias. De esta manera, n8n y Qdrant pueden ejecutarse dentro de entornos aislados con las versiones de software y dependencias necesarias definidas en sus respectivas imágenes.

### 3. Preparación del proyecto

Mi directorio local en el que estoy trabajando es:

```text
C:\Users\User\n8n-local
```

Este directorio funciona como el espacio desde el cual se administra el entorno mediante **Docker Compose**.

La arquitectura local que implementé puede resumirse de la siguiente manera:

![Arquitectura local de n8n y qdrant con Docker](https://github.com/KarlaSiles/BOT-PEC/blob/main/Images/local-architecture_n8n_qdrant_docker.png?raw=true)

### 4. Despliegue con Docker Compose

Para levantar toda la infraestructura se tiene que ejecutar:

```bash
docker compose up -d
```

Docker Compose lee la configuración del proyecto y se encargó automáticamente de descargar las imágenes necesarias y crear los recursos requeridos.

Las imagenes que se descargaron fueron:

```text
docker.n8n.io/n8nio/n8n
qdrant/qdrant
```

A partir de ellas se crearon dos contenedores:

```text
n8n
qdrant
```

El parámetro `-d` permite ejecutar ambos servicios en **detached mode**, es decir, en segundo plano sin mantener ocupada la terminal.

### 5. Creación de la red interna

Docker Compose creó automáticamente la red:

```text
n8n-local_default
```

Esta red permite que los contenedores pertenecientes al proyecto puedan comunicarse entre sí dentro del entorno de Docker.

Esto será particularmente importante para los workflows en los que **n8n necesite comunicarse con Qdrant**, ya que ambos servicios forman parte de la misma infraestructura local.

### 6. Persistencia de datos mediante volúmenes

También se crearon dos volúmenes:

```text
n8n-local_n8n_data
n8n-local_qdrant_data
```

Estos volúmenes permiten separar los datos persistentes del ciclo de vida de los contenedores.

`n8n_data` conserva información relacionada con n8n, mientras que `qdrant_data` permite conservar la información almacenada por Qdrant, incluyendo las colecciones y vectores que posteriormente se utilicen.

Esto significa que detener o recrear un contenedor no implica necesariamente perder los datos almacenados.

### 7. Resultado obtenido

Al finalizar la configuración anteriormente descrita quedó disponible una infraestructura local compuesta por **n8n como herramienta de automatización y orquestación** y **Qdrant como base de datos vectorial**, ambos se ejectuan mediante contenedores Docker.

La arquitectura resultante puede representarse conceptualmente como:

![Arquitectura completa](https://github.com/KarlaSiles/BOT-PEC/blob/main/Images/Entire-architecture.png?raw=true)

### Estado actual

La infraestructura base quedó instalada y se puede acceder a ella mediante los siguietes enlaces: http://localhost:5678/ y http://localhost:6333/dashboard. Con esto se dispone de un entorno local donde es posible **crear y probar workflows en n8n, utilizar Qdrant para almacenamiento y recuperación vectorial y experimentar sin necesidad de desplegar inmediatamente los servicios en servidores externos**.

Esta configuración constituye además la base para posteriormente integrar otros componentes, como modelos de IA, APIs, sistemas RAG, embeddings y fuentes externas de información.
