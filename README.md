# Microservicios Sistema Educativo

## Descripción
Este proyecto implementa un sistema educativo basado en una arquitectura de microservicios, diseñado para gestionar estudiantes, docentes, asignaturas y matrículas de manera distribuida y escalable. Cada microservicio es independiente y se comunica con los demás a través de APIs REST, utilizando tecnologías modernas como Spring Boot, Spring Cloud y MongoDB.

El sistema sigue un enfoque DevOps completo con pruebas automatizadas, integración continua, despliegue continuo, monitorización y orquestación de contenedores.

## Arquitectura del Sistema
### Componentes Principales

### 1. Servidor de Configuración (Config Server)
- Gestión centralizada de configuraciones para todos los microservicios
- Basado en un repositorio Git para almacenar archivos de configuración

### 2. Servidor de Descubrimiento (Eureka Server)
- Registro y descubrimiento de servicios
- Permite la comunicación entre microservicios sin necesidad de URLs hardcodeadas

### 3. Microservicio de Usuarios
- Gestión de estudiantes y docentes
- Autenticación y autorización mediante JWT
- Perfiles de usuario y control de acceso basado en roles
- Implementa seguridad con generación de tokens JWT

### 4. Microservicio de Asignaturas
- CRUD de materias académicas
- Gestión de información relacionada con las asignaturas

### 5. Microservicio de Matrículas
- Registro de estudiantes en materias
- Comunicación con los microservicios de usuarios y asignaturas
- Gestión de las relaciones entre estudiantes y asignaturas

### 6. Monitor Admin
- Dashboard de monitorización para supervisar el estado de los microservicios
- Visualización de métricas y estado de salud

### 7. Sistema de Monitorización
- Prometheus para la recolección y almacenamiento de métricas
- Grafana para la visualización y alerting de métricas
- Integración con Spring Boot Actuator para exponer endpoints de salud y métricas

## Tecnologías Utilizadas
- Lenguaje de programación: Java 17
- Framework: Spring Boot 3.3.10, Spring Cloud 2023.0.5
- Base de datos: MongoDB
- Comunicación entre servicios: Feign Client
- Gestión de dependencias: Maven
- Contenedorización: Docker
- Seguridad: JWT, Spring Security
- Monitorización: Spring Boot Actuator
- Descubrimiento de servicios: Eureka Server
- Configuración centralizada: Config Server
- CI/CD: GitHub Actions

## Estructura del Proyecto
```
Microservicios-Sistema-Educativo/
│
├── .github/workflows/           # Configuración de GitHub Actions para CI/CD
│   └── test.yml                 # Pipeline de pruebas automatizadas
│
├── .idea/                     # Configuración del IDE
│
├── config-server/             # Servidor de configuración centralizada
│
├── eureka-server/             # Servidor de registro y descubrimiento
│
├── monitor-admin/             # Dashboard de monitorización
│
├── registration/              # Microservicio de matrículas
│
├── subjects/                  # Microservicio de asignaturas
│
├── users/                     # Microservicio de usuarios
│
├── .gitattributes             # Configuración de atributos Git
│
├── docker-compose.yml         # Configuración para orquestación con Docker
│
├── prometheus.yml               # Configuración de Prometheus para monitorización
│
└── README.md                  # Documentación general del proyecto
```
## Requisitos Previos

- JDK 17 o superior
- Maven 3.6 o superior
- MongoDB
- Docker y Docker Compose (para despliegue)
- Git

## Instalación y Ejecución
## Opción 1: Ejecución Individual de Servicios

### Clonar el repositorio
```bash
git clone https://github.com/lezmez/Sistema-Educativo-Microservicios-DevOps.git
cd Sistema-Educativo-Microservicios-DevOps
```
### Iniciar los servicios en orden
```bash
# 1. Eureka Server
cd ../eureka-server
mvn spring-boot:run

# 2. Config Server
cd config-server
mvn spring-boot:run

# 3. Microservicios (en ventanas de terminal separadas)
cd ../users
mvn spring-boot:run

cd ../subjects
mvn spring-boot:run

cd ../registration
mvn spring-boot:run

# 4. Monitor Admin (opcional)
cd ../monitor-admin
mvn spring-boot:run
```
## Opción 2: Ejecución con Docker Compose

### Clonar el repositorio
```bash
git clone https://github.com/lezmez/Sistema-Educativo-Microservicios-DevOps.git
cd Sistema-Educativo-Microservicios-DevOps
```
### Construir y ejecutar con Docker Compose
```bash
docker-compose up -d
```
Este comando iniciará todos los servicios definidos en el archivo docker-compose.yml:

- Eureka Server para el registro y descubrimiento de servicios
- Config Server para la configuración centralizada
- MongoDB como base de datos
- Monitor Admin para supervisión
- Microservicios: Users, Subjects y Registration
- Prometheus para la recolección de métricas
- Grafana para la visualización de datos

Los servicios se iniciarán con retrasos programados para asegurar una secuencia de arranque adecuada, garantizando que las dependencias estén disponibles antes de que cada servicio inicie.

## Flujo de Comunicación
1. El usuario se autentica a través del microservicio de usuarios y obtiene un token JWT
2. El token JWT se utiliza para las llamadas posteriores a los diferentes microservicios
3. Al registrar una matrícula, el microservicio de matrículas se comunica con:
    - Microservicio de usuarios para validar el estudiante
    - Microservicio de asignaturas para validar la asignatura
4. Todos los microservicios se registran en Eureka para facilitar el descubrimiento
5. Todas las configuraciones se obtienen del Config Server

## Endpoints Principales
### Microservicio de Usuarios
- `POST /api/users/register`: Registra un nuevo usuario
- `POST /api/users/login`: Autenticación y obtención de token JWT
- `GET /api/users`: Lista todos los usuarios (admin)
- `GET /api/users/{id}`: Obtiene información de un usuario específico

### Microservicio de Asignaturas
- `GET /api/subjects`: Lista todas las asignaturas
- `GET /api/subjects/{id}`: Obtiene información de una asignatura específica
- `POST /api/subjects`: Crea una nueva asignatura

### Microservicio de Matrículas
- `POST /api/registrations`: Registra un estudiante en una asignatura
- `GET /api/registrations`: Lista todas las matrículas
- `GET /api/registrations/student/{userId}`: Lista matrículas por estudiante
- `GET /api/registrations/subject/{subjectId}`: Lista estudiantes por asignatura

### Endpoints de Monitorización
- Spring Boot Actuator: `/actuator/health`, `/actuator/metrics`, `/actuator/info` en cada servicio
- Prometheus: `http://localhost:9090`
- Grafana: `http://localhost:3000`
- Monitor Admin: `http://localhost:8088`
- Eureka Dashboard: `http://localhost:8761`

## Sistema de Monitorización
### Prometheus

- Recolecta métricas de todos los servicios a través de los endpoints de Actuator
- Configurado para escanear los servicios cada 15 segundos
- Accesible en `http://localhost:9090`

### Grafana

- Proporciona visualizaciones avanzadas de las métricas recolectadas por Prometheus
- Dashboards configurables para monitorizar el rendimiento del sistema
- Accesible en `http://localhost:3000`
- Credenciales por defecto: admin/admin

### Configuración de Prometheus
El archivo `prometheus.yml` define los trabajos de scraping para recolectar métricas de:
- Eureka Server
- Config Server
- Monitor Admin
- Servicio de Usuarios
- Servicio de Asignaturas
- Servicio de Matrículas

### Seguridad
El sistema implementa autenticación y autorización mediante JWT:
- Los usuarios se autentican en el microservicio de usuarios
- El token JWT obtenido debe incluirse en las cabeceras de las peticiones a los demás microservicios
- Los roles de usuario determinan el acceso a diferentes funcionalidades

## Pruebas Automatizadas
### Pruebas Unitarias
Cada microservicio incluye pruebas unitarias implementadas con JUnit y Mockito:

- Al menos 2 pruebas unitarias por microservicio
- Verificación de la lógica de negocio aislada

### Pruebas de Integración

- Pruebas con WebTestClient para verificar el comportamiento de los endpoints
- Al menos 1 prueba de integración por microservicio

### Ejecución de Pruebas
Para ejecutar las pruebas de cada microservicio:
```bash
cd [nombre-microservicio]
mvn test
```
## Pipeline CI/CD
El repositorio incluye configuración para integración continua mediante GitHub Actions:

- Archivo de configuración: `.github/workflows/test.yml`
- Ejecuta todas las pruebas automatizadas en cada push al repositorio
- Verifica la calidad del código antes de su integración

Para ver el estado del pipeline, visita la pestaña "Actions" en el repositorio de GitHub.
## Despliegue en Producción
Para un despliegue en producción, se recomienda:

- Configurar HTTPS para todas las comunicaciones
- Ajustar las configuraciones de cada microservicio para entornos de producción
- Implementar múltiples instancias de cada microservicio para alta disponibilidad
- Utilizar un balanceador de carga delante de los microservicios

## Autor (es) ✒️

Este sitio fue realizado por:

* **Cristian Lesmes**
* **Jeffer Pinzon**
