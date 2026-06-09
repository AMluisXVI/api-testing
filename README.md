# 🧪 API Testing — Urban Grocers

**TripleTen QA Engineering Bootcamp**
👤 Luis Manco

[![API Testing](https://img.shields.io/badge/API-Postman%20%7C%20REST-2E5FA3?style=flat-square)](./)
[![Status](https://img.shields.io/badge/Status-Complete-green?style=flat-square)](./)
[![Test Cases](https://img.shields.io/badge/Test%20Cases-43-blue?style=flat-square)](./)
[![Pass Rate](https://img.shields.io/badge/Pass%20Rate-42%25-orange?style=flat-square)](./)
[![Bugs](https://img.shields.io/badge/Bugs%20Found-25-red?style=flat-square)](./)

🇺🇸 [English](#-english) · 🇪🇸 [Español](#-español)

---

## 🇺🇸 English

### Project Description

This project covers API testing for **Urban Grocers** — a grocery delivery backend. Tests were designed and executed using Postman, applying equivalence partitioning and boundary value analysis to three REST endpoints. The high failure rate (25/43) reflects genuine backend validation gaps discovered during testing.

### Test Environment

| Field | Value |
|---|---|
| Application | Urban Grocers API (backend) |
| Tool | Postman |
| Testing Type | Manual API Testing (positive + negative) |
| Test Design | Equivalence classes + Boundary Value Analysis |
| Data Format | JSON over HTTP |

### Endpoints Under Test

| Requirement | Endpoint | Method | Description |
|---|---|---|---|
| REQ-1 | `/api/v1/kits/{id}/products` | POST | Add groceries to a kit. Returns 400 when unique products exceed 30. |
| REQ-2 | `/order-and-go/v1/delivery` | POST | Check if Order and Go delivery is available and return its price. |
| REQ-3 | `/api/v1/kits` | POST / PUT / DELETE | Create, rename, and delete kits. Name: 2–15 Latin chars, spaces or hyphens only. |

### Results Summary

| Endpoint | Total Cases | ✅ Passed | ❌ Failed |
|---|---|---|---|
| POST /kits/{id}/products | 20 | 8 | 12 |
| POST /order-and-go/v1/delivery | 15 | 5 | 10 |
| POST · PUT · DELETE /kits | 8 | 5 | 3 |
| **Total** | **43** | **18** | **25** |

**Pass rate: 42%** — The low pass rate indicates that the Urban Grocers API was missing input validation for most field types and edge cases at the time of testing.

### Key Findings

| Finding | Impact |
|---|---|
| Negative quantities accepted (quantity = -2, 0) | Orders can be placed with invalid amounts |
| Missing `id` field in productsList returns 200 OK | Required field not validated |
| Float quantities (2.3) trigger 500 Internal Server Error | Server crash instead of graceful 400 |
| Kit name length not enforced (1 char, 35 chars → 201 Created) | Name rules not implemented |
| PUT /kits returns 405 Method Not Allowed | Documented endpoint does not exist |
| Non-existent product ID returns 200 OK | Catalog integrity not validated |

### Bug Reports

Defects documented in Jira under project **Urban Grocers API (UGA-xx)** — full export included.

| Bug ID | Title | Priority |
|---|---|---|
| UGA-10 | Negative quantity accepted | Medium |
| UGA-11 | Missing id field returns 200 OK | Medium |
| UGA-16 | Float quantity causes 500 error | High |
| UGA-20 | Letters and symbols accepted as product count | Medium |
| + 21 more | Various validation gaps | Low–High |

### Project Files

| File | Description |
|---|---|
| `test-cases/Luis_Manco_Sprint4_API_Testing_v3.xlsx` | Full bilingual workbook: theory summary, 43 test cases (EN/ES), pass/fail results |
| `evidence/bug-reports/Urban_Grocers_API_bugs_jira.doc` | Jira export — all bug reports with steps to reproduce and environment details |

### Skills Demonstrated

- ✅ REST API concepts: HTTP methods (GET, POST, PUT, DELETE), status codes, JSON
- ✅ Equivalence class partitioning applied to API request fields
- ✅ Boundary value analysis for numeric fields (quantity, weight, kit name length)
- ✅ Positive and negative test execution via Postman
- ✅ Identifying 500-level server errors caused by missing input validation
- ✅ Structured Jira bug reporting for API defects (HTTP method, URL, request body, expected vs. actual)

### Tools Used

| Tool | Purpose |
|---|---|
| Postman | Send HTTP requests, inspect responses, manage collections |
| Jira | Bug tracking and defect reporting |
| Urban Grocers API | Application under test |

---

## 🇪🇸 Español

### Descripción del Proyecto

Este proyecto cubre las pruebas de API de **Urban Grocers** — un backend de entrega de comestibles. Las pruebas fueron diseñadas y ejecutadas con Postman, aplicando clases de equivalencia y análisis de valores límite a tres endpoints REST. La alta tasa de fallos (25/43) refleja brechas reales de validación en el backend descubiertas durante las pruebas.

### Entorno de Prueba

| Campo | Valor |
|---|---|
| Aplicación | API de Urban Grocers (backend) |
| Herramienta | Postman |
| Tipo de prueba | Pruebas de API manuales (positivas y negativas) |
| Diseño de prueba | Clases de equivalencia + Análisis de valores límite |
| Formato de datos | JSON sobre HTTP |

### Endpoints Probados

| Requisito | Endpoint | Método | Descripción |
|---|---|---|---|
| REQ-1 | `/api/v1/kits/{id}/products` | POST | Agregar comestibles a un kit. Devuelve 400 si los productos únicos superan 30. |
| REQ-2 | `/order-and-go/v1/delivery` | POST | Verificar disponibilidad del servicio Order and Go y devolver precio. |
| REQ-3 | `/api/v1/kits` | POST / PUT / DELETE | Crear, renombrar y eliminar kits. Nombre: 2–15 chars latinos, espacios o guiones. |

### Resumen de Resultados

| Endpoint | Total de casos | ✅ Aprobados | ❌ Fallidos |
|---|---|---|---|
| POST /kits/{id}/products | 20 | 8 | 12 |
| POST /order-and-go/v1/delivery | 15 | 5 | 10 |
| POST · PUT · DELETE /kits | 8 | 5 | 3 |
| **Total** | **43** | **18** | **25** |

**Tasa de aprobación: 42%** — La baja tasa refleja que la API de Urban Grocers carecía de validación de entrada para la mayoría de tipos de campo y casos límite al momento de las pruebas.

### Hallazgos Clave

| Hallazgo | Impacto |
|---|---|
| Cantidades negativas aceptadas (quantity = -2, 0) | Se pueden realizar pedidos con cantidades inválidas |
| Campo `id` ausente en productsList devuelve 200 OK | Campo obligatorio no validado |
| Cantidades decimales (2.3) generan error 500 | Crash del servidor en lugar de 400 |
| Longitud del nombre del kit no validada | Reglas de nombre no implementadas |
| PUT /kits devuelve 405 Method Not Allowed | Endpoint documentado no existe |
| ID de producto inexistente devuelve 200 OK | Integridad del catálogo no validada |

### Archivos del Proyecto

| Archivo | Descripción |
|---|---|
| `test-cases/Luis_Manco_Sprint4_API_Testing_v3.xlsx` | Libro completo bilingüe: resumen teórico, 43 casos de prueba (EN/ES), resultados |
| `evidence/bug-reports/Urban_Grocers_API_bugs_jira.doc` | Exportación de Jira — todos los informes de error con pasos de reproducción |

### Habilidades Demostradas

- ✅ Conceptos REST: métodos HTTP, códigos de estado, JSON
- ✅ Clases de equivalencia aplicadas a campos de solicitudes API
- ✅ Análisis de valores límite para campos numéricos
- ✅ Ejecución de pruebas positivas y negativas con Postman
- ✅ Identificación de errores 500 causados por falta de validación de entrada
- ✅ Registro estructurado de bugs en Jira para defectos de API

---

> 📚 Project developed as part of the **TripleTen QA Engineering Bootcamp**
