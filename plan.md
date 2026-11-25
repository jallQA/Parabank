# One-page test plan · ParaBank (web + API)

> Fuentes: `CLAUDE.md`, `propuesta-caso.md`, `Plan_Estimacion_QA.xlsx`. Lo que no consta en ellas está marcado **{PENDIENTE}** (dato por confirmar) o **SUPUESTO** (propuesta a validar por el equipo).

| Sección | Contenido |
| :---- | :---- |
| **Alcance** | **Entra:** 3 flujos críticos, en web (UI) y API: (1) Registro en banca digital, (2) Apertura / creación de cuentas, (3) Transacciones bancarias {PENDIENTE: qué transacciones}. 3 HU · 60 casos (ver cálculo abajo). Diseño, automatización, ejecución, gestión de defectos, re-test y regresión. · **No entra:** otros módulos de ParaBank fuera de los 3 flujos; pruebas de performance, seguridad y visual (no están estimadas en el PERT) {PENDIENTE: confirmar exclusión de performance, mencionada en `riesgos.md`}; datos reales de clientes. |
| **Riesgos (top 10)** | Ver `riesgos.md` (probabilidad × impacto). Riesgo de calidad residual del Excel: ~2,4 defectos podrían escapar (ver Métricas). |
| **Enfoque** | Ver detalle abajo: niveles **API + UI** con Playwright + TypeScript, ejecución en GitHub Actions. **30 casos automatizados (50 %) / 30 manuales (50 %)**. |
| **Ambientes y datos** | Ambiente de pruebas de la propuesta: https://parabank.parasoft.com/parabank/openaccount.htm (página del flujo de apertura de cuentas, sin autorización). URL base web: {PENDIENTE} · URL base / documentación de la API: {PENDIENTE} · Ambiente de ejecución CI (runner, navegadores): {PENDIENTE} · Cuentas de prueba: {PENDIENTE}, se crean con datos sintéticos vía el flujo de registro. · Datos: **solo sintéticos**, generados por el equipo; sin secretos en el repositorio (usar GitHub Secrets) {PENDIENTE: herramienta de generación}. |
| **Criterios** | Entrada · Salida · Suspensión: ver tabla abajo (medibles). |
| **Roles y calendario** | Jean Claude Ezquerra Ferruzo → {PENDIENTE: responsabilidad} · Javier Loarte Leiva → {PENDIENTE: responsabilidad}. Hitos: E1 23/11 · E2 04/12 · E3 18/12 · E4 22/01 · Final 13/02/2027. Capacidad: 32 h/sem {PENDIENTE: ¿equipo o por persona?} × {PENDIENTE: ventana 4 / 11 / 12 semanas}. |
| **Métricas** | 5 métricas con fórmula y decisión: ver tabla abajo. |
| **Uso de IA** | IA en la nube (Claude / Gemini) para: generación de casos, automatización de scripts, análisis de defectos y evaluación de requerimientos. Control: todo artefacto generado por IA lo revisa un integrante antes de entrar al repositorio; a la IA solo se le pasan datos sintéticos. Detalle en `politica-ia.md` {PENDIENTE: archivo aún no existe}. |

## Estimación base (hoja «Esfuerzo de pruebas»)

| Concepto | Cálculo | Resultado |
| :---- | :---- | :---- |
| Total de casos | 3 HU × 20 casos/HU | **60 casos** |
| Diseño (con IA + revisión) | 60 × 10 min = 600 min | 10 h |
| Automatización | 60 × 50 % = 30 casos × 90 min = 2 700 min | 45 h |
| Ejecución manual | 30 casos × 20 min × 2 ciclos = 1 200 min | 20 h |
| Subtotal | 10 + 45 + 20 | 75 h |
| Retrabajo | 75 × 20 % | 15 h |
| **Esfuerzo de pruebas** | 75 + 15 | **90 h** |
| Re-test de defectos (hoja «Defectos esperados») | 21,6 defectos × 1,5 h | 32,4 h |
| **Total pruebas + re-test** | 90 + 32,4 | **122,4 h** |
| Contraste PERT del proyecto completo | E = 147,25 h · E + 2σ = 160,3 h | incluye plan, estrategia, informe y coordinación |

## Enfoque: qué se automatiza y qué queda manual

Regla del Excel: se automatiza el 50 % de los casos, priorizados por **riesgo y repetición**. El reparto por flujo es SUPUESTO (10 + 10 por flujo, 20 casos por HU).

| Nivel | Se automatiza (30 casos · Playwright + TS · CI) | Queda manual (30 casos · 2 ciclos) |
| :---- | :---- | :---- |
| **API** | Casos que se repiten en cada regresión y no dependen de la UI: creación de cliente / cuenta, consulta de saldo, transferencias, validaciones de respuesta y de errores (datos inválidos, saldo insuficiente). También para preparar datos de las pruebas UI. | — |
| **UI (E2E)** | Caminos felices de los 3 flujos de punta a punta: registro → login → apertura de cuenta → transacción. Validaciones negativas críticas de formularios (campos obligatorios, duplicados). Suite de **smoke** para la entrada a cada ciclo. | Casos de baja repetición o cambio frecuente, exploratorias por flujo, mensajes y usabilidad de pantallas, y la primera ejecución de casos nuevos antes de decidir automatizarlos. |
| **Otros** | — | Verificación de defectos (re-test) cuando el caso no está automatizado; revisión humana de casos generados por IA. |

Distribución exacta API vs UI dentro de los 30 automatizados: {PENDIENTE}. Meta de tiempo de regresión, hoy de 4 días: {PENDIENTE}.

## Criterios medibles

| Tipo | Criterio | Umbral | Origen |
| :---- | :---- | :---- | :---- |
| **Entrada** | Test plan aprobado | Aprobación registrada | `riesgos.md` (hito) |
| | Casos diseñados y revisados | 60 / 60 (100 %) | Excel |
| | Ambiente disponible: smoke automatizado pasa | 100 % de la suite de smoke | SUPUESTO |
| | Datos sintéticos generados para los 3 flujos | 3 / 3 flujos con datos | propuesta |
| **Salida** | Casos ejecutados | 60 / 60 (100 %) | Excel |
| | Casos aprobados | ≥ 95 % | SUPUESTO |
| | Defectos críticos / altos abiertos | 0 | `riesgos.md` (cierre de críticos) |
| | Casos automatizados estables en CI | 30 / 30 verdes en 3 corridas seguidas | SUPUESTO |
| | DRE (eficiencia de detección) | ≥ 90 % | Excel |
| **Suspensión** | Ambiente público no disponible | > 4 h continuas en una jornada de prueba | SUPUESTO |
| | Smoke falla al inicio del ciclo | < 100 % → no se ejecuta el ciclo | SUPUESTO |
| | Defecto bloqueante en un flujo crítico | ≥ 1 → se suspende ese flujo | SUPUESTO |
| | Tasa de fallos de un ciclo | > 30 % de casos fallidos | SUPUESTO |
| | **Reanudación** | Causa corregida y smoke en 100 % | SUPUESTO |

## Métricas

| Métrica | Fórmula | Meta | Decisión asociada |
| :---- | :---- | :---- | :---- |
| Avance de ejecución | casos ejecutados / 60 × 100 | 100 % al cierre del ciclo | < plan semanal → reasignar horas o recortar casos de menor riesgo |
| Tasa de aprobación | casos aprobados / casos ejecutados × 100 | ≥ 95 % (SUPUESTO) | Por debajo → no se cumple la salida; priorizar corrección y re-test |
| Defectos encontrados vs esperados | defectos encontrados / 21,6 | ≈ 1 | Muy por debajo → sospechar de cobertura y revisar casos (Excel) |
| DRE | defectos antes de producción / defectos totales × 100 | ≥ 90 % (Excel: 21,6 de 24; escape esperado 2,4) | Por debajo → reforzar regresión automatizada |
| SPI / CPI (valor ganado) | SPI = EV / PV · CPI = EV / AC | ≥ 1 | < 1 → replanificar; con EAC > BAC recortar alcance. Ejemplo del Excel: SPI = CPI = 30 / 32 = 0,94 (atrasado) |
