# Changelog

**Autor:** Sergio Sanz Lomas

Todos los cambios relevantes de este proyecto se documentan en este archivo.

El formato sigue [Keep a Changelog](https://keepachangelog.com/es-ES/1.1.0/) y el proyecto usa [Versionado Semántico](https://semver.org/lang/es/).

## [1.0.0] - 2026-09-28

### Añadido
- Documentación en línea en todos los nodos (Notes con *Display Note in Flow*) y Sticky Notes por sección.
- README del flujo, registro de pruebas `TC-XXX` y este CHANGELOG.

### Cambiado
- Todos los nodos renombrados con la convención `[Acción] - [Propósito]`.

## [0.5.0] - 2026-09-28

### Añadido
- `Set - Format Verdict Message`: mensaje con titular con emoji, nutrientes con marcadores 🔴/🟢, Nutri-Score, puntuación de preocupación y párrafo de la IA.
- `Respond - Verdict Text`: respuesta en texto plano (`200`, `text/plain; charset=utf-8`).

## [0.4.0] - 2026-09-28

### Añadido
- `IF - Route By Verdict Tone`: enrutamiento condicional según el nivel del veredicto.
- `AI - Health Verdict (Cautionary)` y `AI - Health Verdict (Encouraging)`: dos cadenas LLM con Groq y prompts de tono distinto.
- `Merge - Combine AI Branches` en modo Append para unir las dos ramas.

## [0.3.0] - 2026-09-28

### Añadido
- `Set - Classify Traffic Lights`: marcas de semáforo para el azúcar (bajo, medio o alto), la sal y la grasa.
- `Code - Calculate Concern Score`: puntuación de preocupación (0–3) calculada sobre un solo ítem.
- `Set - Determine Verdict Level`: lectura de semáforo, lectura de Nutri-Score y veredicto final (la peor de las dos, con respaldo cuando el Nutri-Score es `unknown`).

## [0.2.0] - 2026-09-28

### Añadido
- `API Call - Open Food Facts Product`: consulta GET sin clave, con el código inyectado en la URL y header `User-Agent`.
- `IF - Check Product Found` y `Respond - Product Not Found` (404 JSON con un ejemplo válido).
- `Set - Extract Nutrition Fields`: variables planas, con la energía leída por notación de corchetes.
- `IF - Check Nutrient Data` y `Respond - Insufficient Data` (200 JSON).

### Corregido
- Los nutrientes se extraían como String, lo que hacía fallar la comprobación de datos. Ahora se extraen como **Number**, sin convertir los vacíos en `0`.
- El nodo HTTP trataba el 404 de la API como un fallo. Se ha activado **Never Error** para leer el cuerpo con `status: 0`.

## [0.1.0] - 2026-09-27

### Añadido
- `Webhook - Receive Barcode` (`POST /nutrition-check`), con respuesta mediante nodos Respond.
- `IF - Check Empty Request` y `Respond - Self Documentation` (200 JSON).
- `IF - Validate Barcode Format` (solo dígitos) y `Respond - Invalid Barcode Error` (400 JSON).
- Diagrama del flujo en Excalidraw.

### Corregido
- `Response Body` en modo **Expression** en las respuestas con `{{ }}`, que antes devolvían un JSON inválido.
- Header `Content-Type` añadido en todas las respuestas.
