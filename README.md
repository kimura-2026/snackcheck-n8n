# SnackCheck

Automatización de n8n que recibe un código de barras mediante una solicitud HTTP GET, consulta los datos del producto en Open Food Facts y devuelve una evaluación nutricional orientativa en español. Un modelo de lenguaje de Groq redacta un comentario breve a partir de los resultados calculados por el flujo.

## Funcionamiento

1. **Webhook** recibe el parámetro `barcode`.
2. Los nodos **IF** validan la presencia del código y que contenga solo números.
3. **HTTP Request** consulta la API de Open Food Facts.
4. **IF1** comprueba que el producto exista; **IF3** revisa que haya datos numéricos de azúcar, sal y grasa.
5. **Edit Fields** extrae y organiza los datos del producto.
6. **Code in JavaScript** calcula `concernScore`, el número de nutrientes que superan los umbrales altos.
7. **Edit Fields1** determina los semáforos y el veredicto final, considerando también Nutri-Score.
8. **Basic LLM Chain**, conectado a **Groq Chat Model**, genera un comentario breve en español.
9. **Edit Fields2** compone el mensaje y **Respond to Webhook** lo devuelve. Hay respuestas separadas para código ausente, código inválido y datos insuficientes.

## Reglas de clasificación

| Indicador (por 100 g) | Verde | Ámbar | Rojo |
| --- | --- | --- | --- |
| Azúcar | Menos de 5 g | Entre 5 y 22,5 g | Más de 22,5 g |
| Sal | Hasta 1,5 g | No se usa | Más de 1,5 g |
| Grasa total | Hasta 17,5 g | No se usa | Más de 17,5 g |

El puntaje `concernScore` va de 0 a 3: **0 = saludable**, **1 = moderado**, **2–3 = no saludable**. Nutri-Score `a` o `b` equivale a saludable, `c` a moderado y `d` o `e` a no saludable. Se adopta el veredicto más desfavorable; si Nutri-Score es `unknown`, se utiliza la clasificación por nutrientes. No se debe asignar un veredicto cuando faltan datos esenciales.

## Tecnologías

- n8n: automatización y Webhook.
- Open Food Facts: datos nutricionales.
- JavaScript: cálculo del puntaje.
- Groq: generación de explicación en lenguaje natural.
- Postman: pruebas HTTP.
- Excalidraw: diagrama del flujo.

## Ejecución de prueba

1. Importar `SnackCheck.json` en n8n y configurar las credenciales de Groq en la propia instancia.
2. Abrir el nodo Webhook y activar **Listen for test event**, o iniciar una ejecución de prueba del flujo completo.
3. En Postman, enviar una solicitud GET a la URL de prueba que muestre el Webhook, agregando `?barcode=3017620422003`.
4. Revisar el texto devuelto y la ejecución en n8n.

Ejemplo de ruta: `/webhook-test/snackcheck?barcode=3017620422003`.

**Seguridad:** no publicar claves API ni tokens en el repositorio. Las credenciales deben gestionarse desde n8n.

## Estado y limitaciones

La evaluación de Nutella se comprobó en Postman. También se habían comprobado las respuestas de código ausente, código no numérico y producto inexistente en etapas anteriores. Tras incorporar **Merge (Append)**, algunas pruebas devolvieron una respuesta HTTP 200 con contenido `1` o mostraron `No output data`; por tanto, **la última configuración no está validada de extremo a extremo**. La prueba de producto sin nutrientes y otras pruebas de regresión siguen pendientes. Véase `TEST_LOG.md`.

Los resultados dependen de la cobertura y exactitud de Open Food Facts. Son orientativos y no sustituyen una evaluación nutricional profesional.
