Actividad: Autenticación y Autorización (OAuth 2.0 / OIDC)
Asignatura: Desarrollo Cloud Native I  
Servicio Seleccionado: Miro  
Flujo: OAuth 2.0 Authorization Code Grant  


## 1. Mapeo de Actores OAuth 2.0 / OpenID Connect

Resource Owner (Propietario del Recurso): El usuario o administrador del Workspace de Miro. Es la persona que posee la cuenta, tiene acceso a los tableros y otorga los permisos a la aplicación.
Cliente (Client): Nuestra aplicación web o integración externa. Es el software que requiere interactuar con la API de Miro para leer o crear recursos.
Authorization Server (Servidor de Autorización): miro.com/oauth/authorize. Es la infraestructura de Miro encargada de autenticar al usuario, mostrar la pantalla de consentimiento con los permisos y emitir los tokens.
Resource Server (Servidor de Recursos): Miro REST API (api.miro.com/v2/). Es el servidor que almacena los tableros y elementos del canvas. Recibe el Access Token válido y responde con la información solicitada.

## 2. Pasos del Flujo de Autorización

1. Solicitud de Autorización: El usuario hace clic en Conectar con Miro dentro de la app. La app redirige al navegador hacia miro.com/oauth/authorize enviando el client_id, redirect_uri y los scopes.
2. Autenticación y Selección: El Resource Owner inicia sesión en Miro y selecciona el Workspace específico al que dará acceso.
3. Consentimiento: Miro muestra los permisos requeridos y el usuario aprueba la solicitud.
4. Emisión del Código: Miro redirige al navegador a la redirect_uri con un código de autorización.
5. Intercambio de Token: La app realiza una petición POST interna a api.miro.com/v1/oauth/token enviando el code, client_id y client_secret.
6. Entrega de Tokens: El Authorization Server valida la solicitud y devuelve un Access Token y un Refresh Token.
7. Acceso a Recursos: La app realiza peticiones HTTP a la API REST de Miro pasando el encabezado de Authorization.

  
9. **Respuesta:** El Resource Server valida el token y devuelve los datos requeridos.

---
