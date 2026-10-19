# P0.0 - Despliegue de infraestructura (Módulo 0379)

Proyecto intermodular de administración de sistemas informáticos en red. Institut Tecnològic de Barcelona - ASIX c2 - Equipo JFR03

## Índice

1. [Equipo](#1-equipo)
2. [Objetivo](#2-objetivo)
3. [Requisitos del proyecto](#3-requisitos-del-proyecto)
4. [Arquitectura](#4-arquitectura)
5. [Planificación por sprints](#5-planificación-por-sprints)
6. [Mejoras (al final del proyecto)](#6-mejoras-al-final-del-proyecto)
7. [Pruebas](#7-pruebas)
8. [Control de versiones con Git y GitHub](#8-control-de-versiones-con-git-y-github)
9. [Árbol de documentación](#9-árbol-de-documentación)

## 1. Equipo

| Persona | Nombre | Responsabilidad principal |
|---|---|---|
| Persona 1 | Rubén Mateos Amores | Red e infraestructura base |
| Persona 2 | Jesús Palma Palma | Datos y aplicación |
| Persona 3 | Fran Mendoza Jiménez | Acceso, clientes, control de versiones y documentación |
| Persona 4 | Joel Muñoz Llerin | Servicios secundarios y algunas mejoras |

Profesorado: Miguel Ángel González del Rio y Sergi Grangel Massana

## 2. Objetivo

Preparar la infraestructura para una aplicación multicapa (servidor web, monitor de redes, SSH, base de datos, DHCP, DNS y FTP).

Duración prevista: 6 semanas, hasta el 18/11.

En los nombres de equipo, NCC indica el número de equipo (N01, N02, ...)

## 3. Requisitos del proyecto

- Planificar las tareas en Proofhub.
- Sprints quincenales de 10 horas de trabajo (5 horas semanales). En total, 3 sprints
- Definir las tareas en el backlog del proyecto en Proofhub
- Cada grupo tiene un proyecto con la nomenclatura P0.0-ASIXc2gC-Gnn, donde g es el grupo (A o B) y nn el número de grupo con dos dígitos
- Todos los sistemas (y aplicaciones que se monten) deben tener un usuario bchecker que permita acceder a los equipos. La contraseña está indicada en el enunciado de la práctica y no se publica en este repositorio.
- Definir un diagrama de la arquitectura que se desplegará

### 3.1 Hardware de red

Router con hostname R-JFR y 3 redes:

- DMZ
- Intranet
- NAT

### 3.2 Hardware de servicios

| Servicio | Hostname | Detalles |
|---|---|---|
| Servidor web | W-JFR | Servidor web de la aplicación |
| SSH | - | Acceso remoto a los equipos |
| Base de datos | B-JFR | MySQL, usuario bchecker, carga del CSV de equipamientos de educación de Barcelona |
| DHCP | - | Reparto de direcciones en las redes del router |
| DNS | - | Debe resolver R-NCC y R para el router |
| FTP | F-NCC | Servidor FTP |

CSV de datos: listado de equipamientos de educación de la ciudad de Barcelona (Open Data BCN), que se carga en la base de datos creada.

### 3.3 Hardware de clientes

- 1 PC Windows
- 1 PC Linux

## 4. Arquitectura

Esquema lógico de nuestro diagrama de arquitectura de red:

```
![Primera versión de nuestro diagrama de red](docs/arquitectura/Diagrama Arquitectura G3.png)
```

Nota: la ubicación definitiva de cada servicio en DMZ o Intranet se confirma en el diagrama final del Sprint 1.

## 5. Planificación por sprints

| Sprint | Fechas | Contenido |
|---|---|---|
| Sprint 1 | 07/10/2026 - 21/10/2026 | Planificación y base |
| Sprint 2 | 21/10/2026 - 04/11/2026 | Servicios |
| Sprint 3 | 04/11/2026 - 18/11/2026 | Integración, pruebas, documentación y mejoras |

Cada persona dedica 10 horas por sprint.

### 5.1 Sprint 1 (07/10 - 21/10)

#### Persona 1 - Rubén Mateos Amores

| Tarea | Descripción | Inicio | Fin | Horas |
|---|---|---|---|---|
| Crear proyecto y backlog en Proofhub | Crear el proyecto P0.0-ASIXc2gC-Gnn y cargar el backlog con las tareas de los 3 sprints | 07/10 | 09/10 | 1 |
| Diagrama de la arquitectura | Definir el diagrama de la arquitectura que se desplegará (router, redes DMZ/Intranet/NAT, servidores y clientes) | 07/10 | 14/10 | 3 |
| Router R-JFR con 3 redes | Desplegar el router R-JFR con las redes DMZ, Intranet y NAT | 09/10 | 21/10 | 4 |
| Servidor DHCP (instalación) | Instalar y configurar el servicio DHCP para las redes del router | 14/10 | 21/10 | 2 |

#### Persona 2 - Jesús Palma Palma

| Tarea | Descripción | Inicio | Fin | Horas |
|---|---|---|---|---|
| Cargar mis tareas en Proofhub | Revisar el backlog y asignarse las tareas propias | 07/10 | 09/10 | 1 |
| Servidor web W-JFR | Desplegar el servidor web W-JFR y crear el usuario bchecker | 09/10 | 21/10 | 5 |
| Preparar servidor BBDD B-JFR e instalar MySQL | Desplegar el equipo B-JFR e instalar MySQL | 14/10 | 21/10 | 4 |

#### Persona 3 - Fran Mendoza Jiménez

| Tarea | Descripción | Inicio | Fin | Horas |
|---|---|---|---|---|
| Cargar mis tareas en Proofhub | Revisar el backlog y asignarse las tareas propias | 07/10 | 09/10 | 1 |
| Repositorio GitHub del proyecto | Crear el repositorio de GitHub del proyecto | 09/10 | 14/10 | 2 |
| Validación por clave pública/privada | Configurar que el servidor se valide en GitHub mediante intercambio de clave pública/privada | 14/10 | 21/10 | 2 |
| Estructura inicial de documentación Markdown | Crear el árbol de documentación Markdown en el repositorio | 14/10 | 21/10 | 2 |

#### Persona 4 - Joel Muñoz Llerin

| Tarea | Descripción | Inicio | Fin | Horas |
|---|---|---|---|---|
| Cargar mis tareas en Proofhub | Revisar el backlog y asignarse las tareas propias | 07/10 | 09/10 | 1 |
| Servidor SSH | Desplegar el servidor SSH y crear el usuario bchecker | 09/10 | 21/10 | 3 |

### 5.2 Sprint 2 (21/10 - 04/11)

#### Persona 1 - Rubén Mateos Amores

| Tarea | Descripción | Inicio | Fin | Horas |
|---|---|---|---|---|
| Servidor DHCP (finalizar y probar) | Terminar la configuración del DHCP y comprobar que los clientes reciben dirección | 21/10 | 28/10 | 2 |
| Servidor DNS | Desplegar el DNS; debe resolver R-NCC y R para el router | 21/10 | 04/11 | 5 |
| Usuario bchecker en router, DHCP y DNS | Crear el usuario bchecker en los equipos y aplicaciones de la persona 1 | 28/10 | 04/11 | 1 |
| Revisión parcial de conectividad | Comprobar la conectividad entre DMZ, Intranet y NAT | 28/10 | 04/11 | 2 |

#### Persona 2 - Jesús Palma Palma

| Tarea | Descripción | Inicio | Fin | Horas |
|---|---|---|---|---|
| Configurar B-JFR y usuario bchecker | Configurar MySQL en B-JFR con el usuario bchecker | 21/10 | 28/10 | 2 |
| Descargar y analizar el CSV de equipamientos de educación | Descargar el CSV de equipamientos de educación de Barcelona (Open Data BCN) y revisar sus columnas | 21/10 | 28/10 | 2 |
| Cargar el CSV en la BBDD | Crear las tablas y cargar los datos del CSV en la base de datos de B-JFR | 28/10 | 04/11 | 3 |
| Configurar W-JFR y usuario bchecker | Terminar la configuración del servidor web y comprobar el acceso con bchecker | 28/10 | 04/11 | 2 |
| Prueba de conexión web - BBDD | Verificar que W-JFR puede conectarse a B-JFR | 04/11 | 04/11 | 1 |

#### Persona 3 - Fran Mendoza Jiménez

| Tarea | Descripción | Inicio | Fin | Horas |
|---|---|---|---|---|
| PC cliente Windows | Desplegar el PC cliente Windows y crear el usuario bchecker | 28/10 | 04/11 | 2 |
| PC cliente Linux | Desplegar el PC cliente Linux y crear el usuario bchecker | 28/10 | 04/11 | 2 |
| Documentar control de versiones | Documentar git add / push / pull / clone / commit -m y la subida a producción | 21/10 | 04/11 | 2 |

#### Persona 4 - Joel Muñoz Llerin

| Tarea | Descripción | Inicio | Fin | Horas |
|---|---|---|---|---|
| Servidor FTP F-NCC | Desplegar el servidor FTP F-NCC y crear el usuario bchecker | 21/10 | 28/10 | 3 |
| Usuario bchecker en SSH | Verificar el acceso con bchecker en el servidor SSH | 28/10 | 04/11 | 1 |
| Pasar capturas y pasos a Fran | Pasar a Fran las capturas y los pasos de SSH y FTP para la documentación | 28/10 | 04/11 | 1 |

### 5.3 Sprint 3 (04/11 - 18/11)

#### Persona 1 - Rubén Mateos Amores

| Tarea | Descripción | Inicio | Fin | Horas |
|---|---|---|---|---|
| Revisión final de conectividad | Revisar la conectividad entre redes y servicios antes de las pruebas finales | 04/11 | 11/11 | 2 |
| [MEJORA] Honeypot en la DMZ | Implementar un honeypot en la DMZ (opcional entre Rubén y Joel) | 11/11 | 18/11 | 3 |
| [MEJORA] Conexión con DNS y dominio públicos | Conectar el DNS con un dominio público | 11/11 | 18/11 | 2 |
| [MEJORA] Certificados y dominio público para FTP y web | Certificados y dominio público para F-NCC y el servidor web (coordinar con las personas 2 y 3) | 11/11 | 18/11 | 3 |

#### Persona 2 - Jesús Palma Palma

| Tarea | Descripción | Inicio | Fin | Horas |
|---|---|---|---|---|
| Aplicación de prueba | Desplegar una pequeña aplicación (se puede generar con IA) que muestre el contenido de las tablas cargadas | 04/11 | 11/11 | 3 |
| Pruebas finales de la aplicación | Probar la aplicación y la conexión web - BBDD de extremo a extremo | 11/11 | 13/11 | 2 |
| [MEJORA] Segmentar la BBDD | Segmentar la BBDD optimizando las relaciones 1-m y n-m que se detecten | 11/11 | 18/11 | 2 |
| [MEJORA] Modelo CRUD con rutas tipo API REST | Implementar un CRUD con rutas tipo API REST (Python-Flask o Phalcon PHP) | 11/11 | 18/11 | 3 |

#### Persona 3 - Fran Mendoza Jiménez

| Tarea | Descripción | Inicio | Fin | Horas |
|---|---|---|---|---|
| Documentación final en Markdown | Completar el árbol de documentación con el trabajo de las 4 personas | 04/11 | 15/11 | 3 |
| Documentar pruebas y subida a producción | Recoger las pruebas realizadas y el proceso de subida a producción | 11/11 | 15/11 | 1 |

#### Persona 4 - Joel Muñoz Llerin

| Tarea | Descripción | Inicio | Fin | Horas |
|---|---|---|---|---|
| [MEJORA] SSH con port knocking y fail2ban | Configurar port knocking y fail2ban en el servidor SSH | 11/11 | 18/11 | 3 |
| [MEJORA] Script de despliegue automatizado | Crear un script que automatice el despliegue de la infraestructura | 11/11 | 18/11 | 3 |
| [MEJORA] Honeypot en la DMZ | Implementar un honeypot en la DMZ (opcional entre Rubén y Joel) | 11/11 | 18/11 | 3 |

## 6. Mejoras (al final del proyecto)

Las mejoras se empiezan solo cuando la práctica base está terminada y probada.

| Mejora | Responsable |
|---|---|
| Segmentar la BBDD optimizando las relaciones 1-m y n-m que se detecten | Jesús Palma Palma |
| Modelo CRUD con rutas tipo API REST (Python-Flask o Phalcon PHP micro MVC) | Jesús Palma Palma |
| Conexión con DNS y dominio públicos | Rubén Mateos Amores |
| Implementar un honeypot en la DMZ | Rubén Mateos Amores o Joel Muñoz Llerin (opcional) |
| Certificados y dominio público para FTP y servidor web | Rubén Mateos Amores (con Jesús y Fran) |
| Script de despliegue automatizado | Joel Muñoz Llerin |
| SSH con port knocking y fail2ban | Joel Muñoz Llerin |

Prioridad si falta tiempo: API REST, SSH con port knocking y fail2ban, y segmentar la BBDD.

## 7. Pruebas

Desplegar una pequeña aplicación (se puede generar con apoyo de IA) que muestre el contenido de las tablas cargadas en la base de datos.

Lista de comprobación final:

- [ ] El router R-JFR enruta entre DMZ, Intranet y NAT.
- [ ] Los clientes reciben dirección por DHCP.
- [ ] El DNS resuelve R-NCC y R para el router.
- [ ] El usuario bchecker accede a todos los equipos y aplicaciones.
- [ ] W-JFR se conecta a B-JFR.
- [ ] El CSV está cargado en MySQL.
- [ ] La aplicación de prueba muestra el contenido de las tablas.
- [ ] El acceso por SSH y la subida por FTP funcionan.
- [ ] El repositorio valida con clave pública/privada.

## 8. Control de versiones con Git y GitHub

### 8.1 Validación del servidor en GitHub con clave pública/privada

Generar el par de claves en el servidor:

```bash
ssh-keygen -t ed25519 -C "servidor-jfr03"
```

Mostrar la clave pública para copiarla:

```bash
cat ~/.ssh/id_ed25519.pub
```

Añadir la clave pública en GitHub (Settings, SSH and GPG keys, New SSH key) y comprobar la conexión:

```bash
ssh -T git@github.com
```

### 8.2 Comandos básicos

Clonar el repositorio (sustituir USUARIO y REPOSITORIO):

```bash
git clone git@github.com:USUARIO/REPOSITORIO.git
```

Ver el estado de los cambios:

```bash
git status
```

Añadir cambios al área de preparación:

```bash
git add .
```

Guardar los cambios con un mensaje:

```bash
git commit -m "Descripción del cambio"
```

Subir los cambios a GitHub:

```bash
git push origin main
```

Descargar los cambios de los compañeros:

```bash
git pull origin main
```

### 8.3 Flujo de trabajo y subida a producción

1. Hacer `git pull` antes de empezar a trabajar.
2. Realizar los cambios en local.
3. Hacer `git add` y `git commit -m` con un mensaje claro.
4. Subir con `git push`.
5. En el servidor de producción, hacer `git pull` para desplegar la última versión.
6. Comprobar que el servicio funciona tras la actualización.

## 9. Árbol de documentación

Estructura propuesta para el repositorio:

```
.
├── README.md
└── docs
    ├── arquitectura
    │   └── diagrama.md
    ├── red
    │   ├── router.md
    │   ├── dhcp.md
    │   └── dns.md
    ├── servicios
    │   ├── web.md
    │   ├── bbdd.md
    │   ├── ssh.md
    │   └── ftp.md
    ├── clientes
    │   ├── windows.md
    │   └── linux.md
    ├── git
    │   └── control-de-versiones.md
    └── mejoras
        └── propuestas.md
```
