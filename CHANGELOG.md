# SnackCheck — Historial de cambios

Las versiones siguientes describen etapas de desarrollo, no publicaciones formales.

## 0.1 — Estructura de la automatización

- Creación del Webhook GET con la ruta `snackcheck`.
- Consulta a Open Food Facts mediante HTTP Request.
- Incorporación de nodos IF y Respond to Webhook para gestionar las solicitudes.

## 0.2 — Evaluación nutricional

- Extracción de nombre, marca, Nutri-Score y nutrientes por 100 g.
- Implementación de semáforos para azúcar, sal y grasa total.
- Cálculo de `concernScore` mediante JavaScript.
- Determinación del veredicto final con Nutri-Score cuando está disponible.

## 0.3 — Explicación con IA

- Incorporación de Basic LLM Chain y Groq Chat Model.
- Redacción de comentarios breves en español.
- Formato de respuesta con producto, semáforos, Nutri-Score y preocupación.

## 0.4 — Validaciones y pruebas

- Validación de códigos de barras vacíos o no numéricos.
- Manejo de productos no encontrados.
- Incorporación de IF3 para verificar datos nutricionales suficientes.
- Pruebas manuales mediante Postman, incluida Nutella.

## 0.5 — Revisión de conexiones (pendiente de validación)

- Simplificación de IF1 para comprobar la existencia del producto.
- Revisión de las conexiones de entrada y de las rutas de error.
- Ajuste del mensaje de producto inexistente en Edit Fields3.
- Configuración e incorporación de Merge en modo Append.
- Se detectó que algunas ejecuciones con Merge no devolvían el mensaje esperado; queda pendiente corregir o validar esta integración.

## Estado actual

La evaluación de Nutella funciona en una prueba observada. La configuración final del flujo, especialmente Merge y las rutas alternativas, **no ha superado una validación completa**. No se han documentado como aprobadas las pruebas pendientes.
