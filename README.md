# prueba_pix
# Link video de sustentacion:
https://drive.google.com/file/d/1LIPnbmnVELq6YNjEoeAfiwOTeLmBFQea/view?usp=sharing
Este proyecto muestra cómo autenticarse contra Microsoft Graph API utilizando OAuth2 con client credentials y realizar operaciones en OneDrive, como subir y listar archivos.

Requisitos previos:

Cuenta de Azure AD con acceso a App Registrations.

Una aplicación registrada en Azure AD con Client ID, Client Secret y Tenant ID.

Permisos asignados en Microsoft Graph (por ejemplo: Files.ReadWrite.All).

Entorno Pix Studio RPA para ejecutar los flujos.

Flujo de Autenticación:

Obtener Token de Acceso

Endpoint: POST https://login.microsoftonline.com/{TENANT_ID}/oauth2/v2.0/token

Headers: Content-Type: application/x-www-form-urlencoded

Body:
grant_type=client_credentials
client_id=YOUR_CLIENT_ID
client_secret=YOUR_CLIENT_SECRET
scope=https://graph.microsoft.com/.default

La respuesta incluye el campo access_token, que se debe guardar en una variable llamada tokenapi.

Consumir Graph API / OneDrive
Ejemplo para listar archivos en la raíz:

Endpoint: GET https://graph.microsoft.com/v1.0/me/drive/root/children

Headers:
Authorization: Bearer {tokenapi}
Content-Type: application/json
El body va vacío en el caso de un GET.

Flujo en Pix Studio RPA:

Realizar una petición HTTP para obtener el token de acceso. Guardar el access_token en la variable tokenapi.

Realizar otra petición HTTP al endpoint de OneDrive. Agregar el header Authorization con el valor Bearer {tokenapi}. El body se envía según la operación (ejemplo: contenido del archivo si es upload).

Ejemplos de uso:

Listar archivos del root: GET /me/drive/root/children

Subir archivo: PUT /me/drive/root:/nombreArchivo.txt:/content con el contenido del archivo en el body

Errores comunes:

494 Request Header Too Large: ocurre si los headers están mal escritos.

InvalidAuthenticationToken: Access token is empty: significa que no se envió correctamente el header Authorization con el token.

AADSTS53003: la cuenta no tiene permisos o está bloqueada por políticas de acceso condicional.

Referencias:

Microsoft Identity Platform Docs

Graph API Explorer

OneDrive API Reference

Prueba Técnica - Desarrollo RPA con PIX RPA

Objetivo General
Desarrollar un proceso RPA utilizando la plantilla universal de PIX RPA, integrando:

Consumo de APIs

Almacenamiento en base de datos

Generación de reportes en Excel

Automatización web para envío de formularios

Integración con OneDrive mediante la API de Microsoft Graph

Contexto
Una empresa ficticia desea automatizar el análisis diario de productos disponibles en una tienda online.

El proceso RPA debe realizar las siguientes tareas:

Obtener productos desde una API pública.

Guardar los datos originales y estructurados.

Almacenarlos en una base de datos.

Generar un reporte de Excel con estadísticas relevantes.

Subir ese reporte a OneDrive automáticamente.

Enviar el reporte a través de un formulario web.

Registrar evidencias del proceso.

Requerimientos Técnicos

Consumo de API Pública

Fuente: Fake Store API

Endpoint: https://fakestoreapi.com/products

Documentación: https://fakestoreapi.com/docs#tag/Products

Tareas:

Realizar una solicitud HTTP GET al endpoint indicado.

Guardar la respuesta completa en un archivo .json como respaldo de la descarga.

Subir ese archivo .json a OneDrive utilizando la API de Microsoft Graph.
Ruta sugerida: /RPA/Logs/Productos_YYYY-MM-DD.json

Extraer de la respuesta los campos: id, title, price, category, description

Almacenamiento en Base de Datos

Tecnología libre: SQLite, PostgreSQL, SQL Server, u otra equivalente.

Nombre de la tabla: Productos

Estructura esperada:
id (entero, clave primaria)
title (texto)
price (decimal)
category (texto)
description (texto)
fecha_insercion (timestamp con la hora del registro)

Condición adicional: Evitar duplicados al insertar validando id.

Generación de Reporte

Formato: Excel (.xlsx)

Nombre del archivo: Reporte_YYYY-MM-DD.xlsx
Contenido:

Hoja 1 - Productos: lista completa de productos.

Hoja 2 - Resumen:
Total de productos
Precio promedio general
Precio promedio por categoría
Cantidad de productos por categoría

Guardar localmente en /Reportes/

Subir a OneDrive en la ruta: /RPA/Reportes/Reporte_YYYY-MM-DD.xlsx

Automatización Web - Subida de Formulario

Entregar el reporte a través de un formulario web.

Plataformas permitidas: Google Forms, Jotform, Typeform.
