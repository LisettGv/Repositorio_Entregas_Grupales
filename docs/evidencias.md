# Evidencias de Laboratorio 1: API Gateway

## 1. Acceso Directo al Backend vs Gateway

### Petición directa al backend
- **URL**: `https://jsonplaceholder.typicode.com/posts/1`
- **Método**: `GET`
- **Status Code**: `200 OK`
- **Body recibido**:
```json
{
  "userId": 1,
  "id": 1,
  "title": "sunt aut facere repellat provident occaecati excepturi optio reprehenderit",
  "body": "quia et suscipit\nsuscipit recusandae consequuntur expedita et cum\nreprehenderi"
}
```

## 2. Pruebas HTTP mediante el Gateway (Richardson Maturity Model Nivel 2)
GET - Obtener recurso individual
URL: http://localhost:8080/api/v1/posts/1

### Método: GET

Status Code: 200 OK

Body recibido:
{
  "userId": 1,
  "id": 1,
  "title": "sunt aut facere repellat provident occaecati excepturi optio reprehenderit",
  "body": "quia et suscipit\nsuscipit recusandae consequuntur expedita et cum\nreprehenderi"
}

### POST - Crear recurso
URL: http://localhost:8080/api/v1/posts

Método: POST

Status Code: 201 Created

Body enviado:
{
  "title": "Cloud Native",
  "body": "Laboratorio API Gateway",
  "userId": 1
}

### PUT - Actualizar recurso
URL: http://localhost:8080/api/v1/posts/1

Método: PUT

Status Code: 200 OK

Body enviado:
{
  "id": 1,
  "title": "Cloud Native actualizado",
  "body": "Prueba PUT mediante gateway",
  "userId": 1
}

### DELETE - Eliminar recurso
URL: http://localhost:8080/api/v1/posts/1

Método: DELETE

Status Code: 200 OK

## 3. Evidencia de Versionado y Headers Globales
Ruta V1 (/api/v1/posts/1)
Headers de respuesta:

### X-API-Version: v1

X-Gateway-Lab: DSY1107

Ruta V2 (/api/v2/posts/1)
Headers de respuesta:

### X-API-Version: v2

X-Gateway-Lab: DSY1107

### 4. Pruebas y Evidencia de CORS desde el Navegador
Comportamiento PREVIO a la configuración de CORS
Al intentar realizar la petición desde la interfaz web en http://127.0.0.1:5500, el navegador bloqueó la llamada por la política de mismo origen (Same-Origin Policy), mostrando el siguiente error en la consola:

Access to fetch at 'http://localhost:8080/api/v1/posts/1' from origin 'http://127.0.0.1:5500' has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.

Comportamiento POSTERIOR a la configuración de CORS
Tras habilitar la política CORS en application.yml especificando el origen http://127.0.0.1:5500 y reiniciando el Gateway, la consulta desde el navegador respondió exitosamente con status 200 OK, mostrando el objeto JSON en pantalla y sin errores en la consola.

Prueba Preflight (OPTIONS)
Comando ejecutado desde consola:
curl -i -X OPTIONS http://localhost:8080/api/v1/posts \
  -H "Origin: [http://127.0.0.1:5500](http://127.0.0.1:5500)" \
  -H "Access-Control-Request-Method: POST"
Headers de control de acceso retornados por el Gateway:

Access-Control-Allow-Origin: http://127.0.0.1:5500

Access-Control-Allow-Methods: GET, POST, PUT, DELETE, OPTIONS

#### 5. Recorrido de la Petición
El cliente (navegador/Postman) envía la petición a http://localhost:8080/api/v1/posts/1.

Spring Cloud Gateway evalúa el Predicate Path=/api/v1/posts/**.

El filtro RewritePath transforma la URL a /posts/1.

El Gateway redirige la petición al backend en https://jsonplaceholder.typicode.com/posts/1.

El backend responde con los datos y el Gateway intercepta la respuesta, inyectando los headers X-API-Version: v1 y X-Gateway-Lab: DSY1107.

El Gateway entrega la respuesta final al cliente.

