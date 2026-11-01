# SnackCheck — Registro de pruebas

Este registro distingue las pruebas observadas durante el desarrollo de las verificaciones que aún faltan en la configuración final.

| Caso | Entrada o procedimiento | Resultado observado | Estado |
| --- | --- | --- | --- |
| Sin código de barras | GET sin `barcode` | Se devolvió un mensaje de bienvenida en una versión anterior | Aprobado anteriormente; pendiente de regresión |
| Código no numérico | `?barcode=abc123` | Se devolvió un mensaje de código inválido en una versión anterior | Aprobado anteriormente; pendiente de regresión |
| Producto inexistente | Código numérico no registrado | Se devolvió el mensaje de producto no encontrado en una versión anterior | Aprobado anteriormente; pendiente de regresión |
| Nutella | `?barcode=3017620422003` | `NO SALUDABLE`; azúcar 56.3 g/100 g (rojo), sal 0.107 g/100 g (verde), grasa 30.9 g/100 g (rojo), preocupación 2/3 | Aprobado en prueba observada |
| Generación con Groq | Consulta de Nutella | Se generó un comentario nutricional en español | Aprobado en prueba observada |
| Respuesta tras conectar Merge | Pruebas desde Postman con Webhook de prueba | En una prueba se observó HTTP 200 con cuerpo `1`; Merge mostró `No output data` en la inspección indicada | No aprobado / requiere diagnóstico |
| Ejecución completa | Botón Execute Workflow | El usuario informó que una prueba funcionó mediante ejecución completa | Funcionamiento reportado; no valida todas las rutas |
| Producto sin nutrientes | Producto sin azúcar, sal o grasa | No se completó una prueba de extremo a extremo | Pendiente |
| Producto saludable | Código de un producto con datos suficientes | No se registró una prueba | Pendiente |
| Nutri-Score desfavorable | Producto con Nutri-Score `d` o `e` | No se registró una prueba específica de precedencia | Pendiente |
| Falla de Open Food Facts | Simulación de error de API o conexión | No se registró una prueba | Pendiente |

## Resultado esperado de la prueba de Nutella

```text
🥫 Nutella — NO SALUDABLE
Azúcar: 🔴 56.3 g/100 g
Sal: 🟢 0.107 g/100 g
Grasa: 🔴 30.9 g/100 g
Nutri-Score: unknown · Preocupación: 2/3
```

El párrafo explicativo puede variar entre ejecuciones porque lo genera el modelo de lenguaje.

## Observación de integración

La conexión de dos rutas excluyentes a las entradas de **Merge (Append)** no quedó validada. Antes de considerar la automatización terminada, deben comprobarse por separado las rutas de producto existente y producto inexistente en la versión exportada definitiva. Un código HTTP 200 no basta para aprobar una prueba si el cuerpo no contiene el mensaje esperado.
