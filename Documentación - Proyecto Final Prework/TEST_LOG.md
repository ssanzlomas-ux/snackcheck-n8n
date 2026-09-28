# 🧪 Registro de pruebas: SnackCheck

**Versión probada:** 1.0.0
**Entorno:** n8n · Test URL / Production URL
**Fecha:** 2026-09-28
**Autor / Tester:** Sergio Sanz Lomas

> Instrucciones: completa las columnas **Obtenido**, **Tiempo** y **Estado** (✅ pasa / ❌ falla) con los resultados reales de cada ejecución.

---

## Funcionales (código válido → veredicto)

| ID | Caso | Entrada | Resultado esperado | Obtenido | Tiempo | Estado |
|---|---|---|---|---|---|---|
| TC-001 | Producto no saludable por semáforo y Nutri-Score | `{"barcode":"3017620422003"}` (Nutella) | 200 · `text/plain`. 🛑 Titular "capricho ocasional". Azúcar 🔴, grasa 🔴, sal 🟢. Nutri-Score E. Preocupación 2/3. Tono cauteloso | | | ⬜ |
| TC-002 | Bebida azucarada: gana el Nutri-Score | `{"barcode":"5449000000996"}` (Coca-Cola) | 200. Lectura de semáforo: saludable (0 altos). Lectura Nutri-Score: no saludable. **Veredicto: no saludable**. Preocupación 0/3. Tono cauteloso | | | ⬜ |
| TC-003 | Producto saludable | `{"barcode":"<código Nutri-Score A>"}` | 200. ✅ Titular "buena elección". Veredicto saludable. Tono alentador | | | ⬜ |
| TC-004 | Producto moderado | `{"barcode":"<código Nutri-Score C o 1 nutriente alto>"}` | 200. 🟡 Titular "con moderación". Tono alentador con un aviso suave | | | ⬜ |
| TC-005 | Barcode enviado como número | `{"barcode":3017620422003}` | Se acepta como válido. Mismo resultado que TC-001 | | | ⬜ |

## Integración (APIs en vivo)

| ID | Caso | Comprobación | Resultado esperado | Obtenido | Tiempo | Estado |
|---|---|---|---|---|---|---|
| TC-006 | Open Food Facts: producto encontrado | OUTPUT de `API Call - Open Food Facts Product` con TC-001 | `status: 1`. Llegan `product_name`, `brands`, `nutriscore_grade` y `nutriments` | | | ⬜ |
| TC-007 | Extracción con notación de corchetes | OUTPUT de `Set - Extract Nutrition Fields` | 8 campos planos. `energy_kcal_100g` relleno. Nutrientes de tipo Number | | | ⬜ |
| TC-008 | Groq, rama cautelosa | OUTPUT de `AI - Health Verdict (Cautionary)` con TC-001 | Un párrafo de 70 palabras como máximo, con el nombre del producto y cifras reales | | | ⬜ |
| TC-009 | Groq, rama alentadora | OUTPUT de `AI - Health Verdict (Encouraging)` con TC-003 | Un párrafo con tono positivo y cifras reales | | | ⬜ |
| TC-010 | Merge con una sola rama activa | OUTPUT de `Merge - Combine AI Branches` | Pasa 1 ítem con el campo `text` | | | ⬜ |

## Error

| ID | Caso | Entrada | Resultado esperado | Obtenido | Tiempo | Estado |
|---|---|---|---|---|---|---|
| TC-011 | Cuerpo vacío | `{}` | 200 · JSON de autodocumentación con el servicio, el campo requerido y un ejemplo | | | ⬜ |
| TC-012 | Barcode no numérico | `{"barcode":"abc123"}` | 400 · `INVALID_BARCODE`, `recibido: "abc123"` y ejemplo | | | ⬜ |
| TC-013 | Cuerpo sin campo barcode | `{"producto":"nutella"}` | 400 · `INVALID_BARCODE`, `recibido: null` | | | ⬜ |
| TC-014 | Barcode vacío | `{"barcode":""}` | 400 · `INVALID_BARCODE` | | | ⬜ |
| TC-015 | Código desconocido | `{"barcode":"0000000000000"}` | 404 · `PRODUCT_NOT_FOUND` con ejemplo válido. El flujo no se cae | | | ⬜ |
| TC-016 | Producto sin datos de nutrición | TC-001 con datos fijados en el nodo HTTP y el objeto `nutriments` borrado | 200 · `INSUFFICIENT_DATA`. **No** se clasifica | | | ⬜ |
| TC-017 | Nutri-Score desconocido | TC-001 con datos fijados y `nutriscore_grade` eliminado | Veredicto calculado solo con el semáforo (2 altos → no saludable) | | | ⬜ |

## Rendimiento

| ID | Caso | Medición | Objetivo | Obtenido | Estado |
|---|---|---|---|---|---|
| TC-018 | Veredicto completo (API + IA) | Tiempo total de TC-001 (panel *Executions*) | < 5 s | | ⬜ |
| TC-019 | Respuesta de error temprana | Tiempo de TC-012, sin llamadas externas | < 1 s | | ⬜ |

**Nota de rendimiento:**
_Completa con tus observaciones. Por ejemplo: "El veredicto completo tarda de media X s; el ~Y % del tiempo corresponde a Groq y el resto a Open Food Facts. Los errores de validación responden al instante porque no llaman a ninguna API."_

---

## Resumen

| Tipo | Total | ✅ | ❌ |
|---|---|---|---|
| Funcional | 5 | | |
| Integración | 5 | | |
| Error | 7 | | |
| Rendimiento | 2 | | |
| **Total** | **19** | | |
