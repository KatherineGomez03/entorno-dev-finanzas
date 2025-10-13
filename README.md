# 🚀 Entorno de Desarrollo: Finanzas Gamificadas (Full-Stack)

Este repositorio actúa como el **orquestador central** para el proyecto de aplicación de **finanzas personales gamificadas**.  
Utiliza **Docker Compose** para definir, construir y ejecutar todos los servicios necesarios:

- 🖥️ Frontend: **Next.js**
- ⚙️ Backend: **NestJS**
- 🗃️ Base de datos: **MongoDB**

Los repositorios de código base (`app-finanza-back` y `app-finanzas`) están vinculados como **Git Submodules**.

---

## 📑 Índice

- [🛠️ Tecnologías Utilizadas](#🛠️-tecnologías-utilizadas)
- [👨‍💻 Cómo Poner el Proyecto en Marcha (Setup Local)](#👨‍💻-cómo-poner-el-proyecto-en-marcha-setup-local)
  - [📋 Requisitos Previos](#📋-requisitos-previos)
  - [1. 🧭 Clonar el Repositorio de Orquestación](#1-🧭-clonar-el-repositorio-de-orquestación)
  - [2. 📦 Inicializar y Descargar el Código Base (Submodules)](#2-📦-inicializar-y-descargar-el-código-base-submodules)
  - [3. 🐳 Levantar la Pila Completa con Docker Compose](#3-🐳-levantar-la-pila-completa-con-docker-compose)
- [🔗 Acceso a los Servicios](#🔗-acceso-a-los-servicios)
- [🛑 Comandos Útiles](#🛑-comandos-útiles)
- [🗂️ Estructura del Repositorio](#🗂️-estructura-del-repositorio)
- [⚙️ Desarrollo y Submódulos](#⚙️-desarrollo-y-submódulos)

---

## 🛠️ Tecnologías Utilizadas

| Componente        | Tecnología Principal | Gestión de Paquetes | Orquestación     |
|--------------------|------------------------|-----------------------|--------------------|
| Frontend           | Next.js (React)        | PNPM                  | Docker             |
| Backend (API)      | NestJS (TypeScript)    | PNPM                  | Docker             |
| Base de Datos      | MongoDB                | N/A                   | Docker             |
| Entorno            | WSL 2 / Docker Compose | N/A                   | Docker Compose     |

---

## 👨‍💻 Cómo Poner el Proyecto en Marcha (Setup Local)

Sigue estos pasos para clonar el proyecto, inicializar los submódulos de código y levantar la pila completa.

### 📋 Requisitos Previos

Asegúrate de tener instalados:

- [WSL 2](https://learn.microsoft.com/es-es/windows/wsl/install) (Subsistema de Windows para Linux)  
- [Docker Desktop](https://www.docker.com/) (con integración WSL habilitada)  
- [Git](https://git-scm.com/)  
- [PNPM](https://pnpm.io/) *(opcional, recomendado para desarrollo local fuera de Docker)*

---

### 1. 🧭 Clonar el Repositorio de Orquestación

Este repositorio contiene únicamente el archivo de configuración de Docker y las referencias a los submódulos.

```bash
# Clona este repositorio
git clone git@github.com:KatherineGomez03/entorno-dev-finanzas.git

# Entra a la carpeta
cd entorno-dev-finanzas
```
### 2. 📦 Inicializar y Descargar el Código Base (Submodules)

Este paso es **crucial**: descarga el código del Backend (`app-finanza-back`) y del Frontend (`app-finanzas`).

```bash
git submodule update --init --recursive
```
### 3. 🐳 Levantar la Pila Completa con Docker Compose

Ejecuta este comando para construir las imágenes y arrancar los contenedores:

```
docker compose up --build

```
## 🔗 Acceso a los Servicios

Una vez que los contenedores estén levantados y se muestren los logs de ejecución (`start:dev` y `dev`), podrás acceder a:

| Servicio              | URL de Acceso                   | Descripción                          |
|------------------------|----------------------------------|----------------------------------------|
| Frontend (Next.js)     | [http://localhost:3001](http://localhost:3001) | Interfaz de usuario.                  |
| Backend (NestJS API)   | [http://localhost:3000](http://localhost:3000) | Servidor de API REST.                |
| MongoDB                | `mongodb://localhost:27017`     | Acceso desde clientes como Compass.   |

---

## 🛑 Comandos Útiles

| Comando                        | Propósito                                                                 |
|----------------------------------|----------------------------------------------------------------------------|
| `docker compose up`             | Arranca los contenedores sin reconstruir.                                  |
| `docker compose up --build`     | Reconstruye las imágenes y arranca (usar tras modificar un `Dockerfile`).  |
| `docker compose logs -f`        | Muestra los logs en tiempo real.                                          |
| `docker compose down`           | Detiene y elimina contenedores y redes (conserva los datos de MongoDB).   |
| `docker compose down -v`        | Detiene, elimina y **borra volúmenes de datos** (incluido MongoDB). ⚠️ Usar con precaución. |

---

## 🗂️ Estructura del Repositorio

| Archivo/Carpeta         | Contenido                                                                 |
|--------------------------|----------------------------------------------------------------------------|
| `docker-compose.yml`     | Orquestador central que define los 3 servicios y su conexión.             |
| `app-finanza-back/`      | Submódulo del Backend (NestJS) con su `Dockerfile` y código fuente.      |
| `app-finanzas/`          | Submódulo del Frontend (Next.js) con su `Dockerfile` y código fuente.    |
| `.gitmodules`            | Archivo generado por Git que almacena las URLs de los submódulos.       |
| `README.md`              | Este documento.                                                         |

---

## ⚙️ Desarrollo y Submódulos

Cuando trabajes en el código, recuerda:

- Realizar commits y pushes dentro de la carpeta correspondiente (`app-finanza-back` o `app-finanzas`).

Solo hacer commits en este repositorio (`entorno-dev-finanzas`) cuando:

- Modifiques el `docker-compose.yml`.
- Añadas nuevos archivos de infraestructura.
- Actualices la referencia de un submódulo a una nueva versión estable.