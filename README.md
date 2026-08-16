# Laboratorio 1: Implementación de API Gateway con Spring Cloud Gateway:

Este repositorio contiene la solución e informe del Laboratorio 1 del curso Cloud Native I (DSY1107). Se implementó un API Gateway utilizando Spring Cloud Gateway para actuar como punto único de entrada hacia un servicio backend externo (JSONPlaceholder).

## Datos de la Entrega

- Asignatura: Cloud Native I (DSY1107)
- Proyecto: Laboratorio 1 - API Gateway
- Repositorio: Repositorio_Entregas_Grupales

## Arquitectura y Decisiones de Diseño

### 1. Patrón API Gateway
Se utilizó Spring Cloud Gateway para centralizar:
- Enrutamiento: Redirección dinámica de peticiones hacia la API destino (https://jsonplaceholder.typicode.com).
- Reescritura de Rutas: Transformación de endpoints expuestos (/api/v1/posts/) hacia los endpoints internos del backend (/posts/).
- Inyección de Cabeceras: Versionado explícito (X-API-Version) y marca de laboratorio (X-Gateway-Lab).
- Gestión de CORS: Control centralizado de orígenes permitidos (http://127.0.0.1:5500) para clientes web.

### 2. Modelo de Madurez
El gateway expone los verbos HTTP correspondientes a las operaciones CRUD estándar:
- GET /api/v1/posts/1 -> Consulta de recurso.
- POST /api/v1/posts -> Creación de recurso (201 Created).
- PUT /api/v1/posts/1 -> Modificación de recurso (200 OK).
- DELETE /api/v1/posts/1 -> Eliminación de recurso (200 OK).

## 📁 Estructura del Repositorio

```text
Repositorio_Entregas_Grupales/
├── client/
│   └── index.html             # Cliente web para pruebas de CORS
├── docs/
│   ├── evidencias.md          # Documento detallado con capturas y respuestas
│   ├── backend-directo.png
│   ├── gateway-get.png
│   ├── gateway-post.png
│   ├── gateway-put.png
│   ├── gateway-delete.png
│   ├── headers-v1.png
│   ├── headers-v2.png
│   ├── cors-error.png
│   ├── cors-exito.png
│   └── cors-preflight.png
├── gateway/
│   ├── src/main/java/...      # Código fuente del Gateway
│   └── src/main/resources/
│       └── application.yml    # Configuración de rutas, filtros y CORS
└── README.md                  # Informe principal del proyecto

```
## Ejecución:

### Prerrequisitos

* Java 17 o superior.
* Maven 3.x.
* Extensión Live Server en VS Code (o servidor web local para el cliente HTML).

### Pasos para iniciar el API Gateway

1. Navegar al directorio del gateway:
cd gateway

2. Compilar y ejecutar la aplicación Spring Boot:
./mvnw spring-boot:run
(En Windows PowerShell: .\mvnw.cmd spring-boot:run)

3. El Gateway estará escuchando en `http://localhost:8080`.

### Pasos para ejecutar (CORS):

1. Abrir la carpeta client/ en VS Code.
2. Hacer clic derecho sobre index.html y seleccionar Open with Live Server.
3. El cliente se abrirá en http://127.0.0.1:5500.

## 🧪 Pruebas de Verificación

Se pueden realizar consultas HTTP desde Postman o terminal:

```bash
# Prueba GET V1
curl.exe -i http://localhost:8080/api/v1/posts/1

# Prueba GET V2
curl.exe -i http://localhost:8080/api/v2/posts/1

# Prueba Preflight OPTIONS (CORS)
curl.exe -i -X OPTIONS http://localhost:8080/api/v1/posts -H "Origin: [http://127.0.0.1:5500](http://127.0.0.1:5500)" -H "Access-Control-Request-Method: POST"

```

## 📖 Documentación de Evidencias

Las capturas de pantalla de todas las pruebas ejecutadas, respuestas HTTP y captura de error/éxito de CORS se encuentran detalladas en el archivo:

**[Ver Documento de Evidencias (`docs/evidencias.md`)](https://www.google.com/search?q=docs/evidencias.md)**

```