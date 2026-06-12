# API Testing — Urban Grocers

[![API Testing](https://img.shields.io/badge/API-Postman%20%7C%20REST-2E5FA3?style=flat-square)](./)
[![Status](https://img.shields.io/badge/Status-Complete-green?style=flat-square)](./)
[![Test Cases](https://img.shields.io/badge/Test%20Cases-43-blue?style=flat-square)](./)
[![Pass Rate](https://img.shields.io/badge/Pass%20Rate-42%25-orange?style=flat-square)](./)
[![Bugs](https://img.shields.io/badge/Bugs%20Found-25-red?style=flat-square)](./)

🇺🇸 [English](#-english) · 🇪🇸 [Español](#-español)

---

## 🇺🇸 English

### The Problem

**Urban Grocers** was a grocery delivery backend — and it was broken.

The API accepted negative quantities (-2, 0) for product orders. It crashed with a 500 error when receiving a decimal number (2.3) instead of returning a proper 400. It accepted requests without required fields like `id` and returned 200 OK. It didn't enforce kit name length — a 1-character name and a 35-character name both returned 201 Created.

25 out of 43 test cases failed. **A 42% pass rate means the backend shipped without basic input validation.**

### Why This Matters

This wasn't "edge case" testing. These are **basic validation failures**:

- A user can place an order with quantity = -2.
- A customer can order with quantity = 0 and pay $0.
- Sending a decimal crashes the server instead of rejecting it gracefully.
- Kit names can be anything — the rules exist on paper but not in code.

Each of these is a real bug that affects real users. And they existed because the API team hadn't tested negative scenarios. That's the gap API testing is supposed to close.

### The Approach — Scope & Cuts

I tested **3 REST endpoints** of the Urban Grocers API using **manual API testing via Postman**:

| Endpoint | Method | Cases | ✅ Passed | ❌ Failed |
|---|---|---|---|---|
| `/api/v1/kits/{id}/products` | POST | 20 | 8 | 12 |
| `/order-and-go/v1/delivery` | POST | 15 | 5 | 10 |
| `/api/v1/kits` | POST / PUT / DELETE | 8 | 5 | 3 |
| **Total** | | **43** | **18** | **25** |

**What was cut:**
- No automation — manual first to understand the API behavior before writing scripts
- No UI — pure backend testing, the frontend wasn't ready
- No performance testing — functional validation was the priority

> 43 cases, 25 failures. The low pass rate isn't a reflection of bad testing — it's a reflection of a backend that needed validation.

### Tools — Chosen by Need

| Tool | Why, Not Just What |
|---|---|
| **Postman** | Best tool for manual REST API testing — construct requests, inspect responses, organize collections. No code needed for the initial cycle. |
| **Jira** | The team used it. 25 bugs tracked with HTTP method, URL, request body, expected vs. actual. |
| **Urban Grocers API** | The actual backend endpoint. No mock, no stub — real server responses. |

Postman was chosen because it lets you test APIs without writing a single line of code. The team needed to validate the backend behavior first. Automation comes after you know what the API actually does.

### How It Was Broken Down

3 endpoint sessions, each completable in one sitting:

1. **POST /kits/{id}/products** (20 cases) — the most complex endpoint. 12 failures. Product validation was missing across the board.
2. **POST /order-and-go/v1/delivery** (15 cases) — 10 failures. Delivery availability endpoint had no input guards.
3. **POST / PUT / DELETE /kits** (8 cases) — 3 failures. Kit CRUD operations, including a documented endpoint (PUT) that returned 405.

Each session focused on one endpoint, running positive + negative cases. No dependency between endpoints — any order works.

### The Results

| Metric | Value |
|---|---|
| Test cases | 43 |
| ✅ Passed | 18 |
| ❌ Failed | 25 |
| **Pass rate** | **42%** |

**The 6 most critical findings:**

| Finding | What went wrong |
|---|---|
| Negative quantities accepted (`-2`, `0`) | Orders can be placed with invalid amounts — financial impact |
| Missing `id` field returns 200 OK | Required field not validated — data integrity risk |
| Float quantities (`2.3`) trigger **500 Internal Server Error** | Server crashes instead of returning a proper 400 |
| Kit name length not enforced (1 char, 35 chars → 201 Created) | Naming rules not implemented — data inconsistency |
| PUT /kits returns 405 Method Not Allowed | Documented endpoint doesn't exist — spec vs. reality mismatch |
| Non-existent product ID returns 200 OK | Catalog integrity not validated — phantom products accepted |

All 25 bugs documented in `evidence/bug-reports/Urban_Grocers_API_bugs_jira.doc` with request/response details.

---

## 🇪🇸 Español

### El Problema

**Urban Grocers** era un backend de entrega de comestibles — y estaba roto.

La API aceptaba cantidades negativas (-2, 0) para pedidos de productos. Se caía con error 500 al recibir un número decimal (2.3) en lugar de devolver un 400 correcto. Aceptaba solicitudes sin campos obligatorios como `id` y devolvía 200 OK. No validaba la longitud del nombre del kit — un nombre de 1 carácter y uno de 35 caracteres ambos devolvían 201 Created.

25 de 43 casos de prueba fallaron. **Una tasa de aprobación del 42% significa que el backend se lanzó sin validación de entrada básica.**

### Por Qué Importa

Esto no es "casos límite". Son **fallos de validación básicos**:

- Un usuario puede hacer un pedido con cantidad = -2.
- Un cliente puede pedir con cantidad = 0 y pagar $0.
- Enviar un decimal tira abajo el servidor en lugar de rechazarlo correctamente.
- Los nombres de kit pueden ser cualquier cosa — las reglas existen en papel pero no en código.

Cada uno es un bug real que afecta a usuarios reales. Y existían porque el equipo de API no había probado escenarios negativos. Ese es el gap que las pruebas de API deben cerrar.

### El Enfoque — Alcance y Recortes

Probé **3 endpoints REST** de la API de Urban Grocers con **pruebas de API manuales via Postman**:

| Endpoint | Método | Casos | ✅ Aprobados | ❌ Fallidos |
|---|---|---|---|---|
| `/api/v1/kits/{id}/products` | POST | 20 | 8 | 12 |
| `/order-and-go/v1/delivery` | POST | 15 | 5 | 10 |
| `/api/v1/kits` | POST / PUT / DELETE | 8 | 5 | 3 |
| **Total** | | **43** | **18** | **25** |

**Lo que se recortó:**
- Sin automatización — primero manual para entender el comportamiento de la API
- Sin UI — pruebas de backend puro, el frontend no estaba listo
- Sin pruebas de rendimiento — la validación funcional era la prioridad

> 43 casos, 25 fallos. La baja tasa de aprobación no refleja malas pruebas — refleja un backend que necesitaba validación.

### Stack — Elegido por Necesidad

| Herramienta | Por Qué, No Solo Qué |
|---|---|
| **Postman** | La mejor herramienta para pruebas manuales de API REST — construís requests, inspeccionás responses, organizás colecciones. Sin código para el ciclo inicial. |
| **Jira** | El equipo lo usaba. 25 bugs con método HTTP, URL, body, esperado vs. actual. |
| **Urban Grocers API** | El backend real. Sin mock, sin stub — respuestas reales del servidor. |

Postman fue elegido porque permite probar APIs sin escribir una línea de código. El equipo necesitaba validar el comportamiento del backend primero. La automatización viene después de saber lo que la API realmente hace.

### Cómo se Fragmentó

3 sesiones por endpoint, cada una completable en una sentada:

1. **POST /kits/{id}/products** (20 casos) — el endpoint más complejo. 12 fallos. La validación de productos faltaba por completo.
2. **POST /order-and-go/v1/delivery** (15 casos) — 10 fallos. El endpoint de disponibilidad no tenía guardas de entrada.
3. **POST / PUT / DELETE /kits** (8 casos) — 3 fallos. Operaciones CRUD de kits, incluyendo un endpoint documentado (PUT) que devolvía 405.

Cada sesión se centró en un endpoint, ejecutando casos positivos y negativos. Sin dependencias entre endpoints.

### Los Resultados

| Métrica | Valor |
|---|---|
| Casos de prueba | 43 |
| ✅ Aprobados | 18 |
| ❌ Fallidos | 25 |
| **Tasa de aprobación** | **42%** |

**Los 6 hallazgos más críticos:**

| Hallazgo | Qué salió mal |
|---|---|
| Cantidades negativas aceptadas (`-2`, `0`) | Pedidos con montos inválidos — impacto financiero |
| Campo `id` ausente devuelve 200 OK | Campo obligatorio no validado — riesgo de integridad |
| Cantidades decimales (`2.3`) generan **error 500** | El servidor se cae en lugar de devolver un 400 |
| Longitud de nombre no validada | Reglas de nombre no implementadas — inconsistencia |
| PUT /kits devuelve 405 Method Not Allowed | Endpoint documentado no existe — especificación vs. realidad |
| ID de producto inexistente devuelve 200 OK | Productos fantasma aceptados — integridad del catálogo |

Los 25 bugs documentados en `evidence/bug-reports/Urban_Grocers_API_bugs_jira.doc` con detalles de request/response.
