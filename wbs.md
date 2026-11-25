# WBS y cronograma · ParaBank (web + API)

> Fuentes: `Plan_Estimacion_QA.xlsx` (hoja «PERT proyecto»: E = (O+4M+P)/6), `CLAUDE.md`, `plan.md`. Lo que no consta en ellas está marcado **SUPUESTO** o **{PENDIENTE}**.
> Las horas E son las del Excel (no se inventó ninguna). Las 16 actividades del Excel son las hojas del nivel 3; los niveles 1 y 2 son agrupaciones y suman sus hijos.

## Supuestos del cálculo

| # | Supuesto | Por qué |
| :---- | :---- | :---- |
| S1 | **32 h/sem = total del equipo** (2 personas × 16 h = 3,2 h/día c/u, lun–vie). Equipo = 6,4 h/día laborable. | El Excel compara 32 h × semanas contra el E total del proyecto. {PENDIENTE: confirmar equipo vs. por persona} |
| S2 | Inicio **mar 24/11/2026** (hoy). Sin feriados (25/12, 01/01 y quizá 08/12 se tratan como laborables). | CLAUDE.md calcula las semanas desde el 24/11. |
| S3 | Ventana de capacidad: **11 semanas** (→ 13/02/2027). Se muestra también la del Excel (4 sem). | CLAUDE.md: 81 días / 7 = 11,6 → 11. {PENDIENTE: confirmar ventana} |
| S4 | Dependencias (el Excel no las trae): plan → estrategia → {diseño ×3, datos} → {automatización UI, API} → ejecución ×3 → defectos → re-test → regresión → informe. La ejecución espera a la automatización porque `plan.md` exige «smoke automatizado pasa» como criterio de entrada. | Lógica de proceso + criterios de `plan.md`. |
| S5 | **Qué entrega cada hito (E1–E4, Final) no está documentado**; se asigna así: E1 = análisis y plan · E2 = diseño de casos + datos · E3 = automatización · E4 = ejecución, defectos y re-test · Final = regresión + informe. | {PENDIENTE: confirmar entregables reales de cada hito} |
| S6 | Cada actividad la hace **una persona**; dos personas en paralelo. 1.2.1 (coordinación) es esfuerzo continuo repartido en todo el proyecto. | Simplificación para fechar. |

## WBS hasta nivel 3

| Código | Paquete de trabajo / actividad | E (h) | Inicio | Fin | Depende de | Hito | ¿Ruta crítica? |
| :---- | :---- | ----: | :---- | :---- | :---- | :---- | :---- |
| **1** | **Gestión del proyecto** | **20,08** | 24/11/2026 | 05/01/2027 | — | | Sí |
| **1.1** | **Planificación y estrategia** | **13,00** | 24/11/2026 | 30/11/2026 | — | | **Sí** |
| 1.1.1 | Análisis y plan del proyecto | 7,00 | 24/11/2026 | 26/11/2026 | — | **E1** | **Sí** |
| 1.1.2 | Estrategia QE | 6,00 | 26/11/2026 | 30/11/2026 | 1.1.1 | | **Sí** |
| **1.2** | **Coordinación y cierre** | **7,08** | 24/11/2026 | 05/01/2027 | — | | Sí |
| 1.2.1 | Coordinación Dev / Negocio (continua) | 1,08 | 24/11/2026 | 05/01/2027 | — | | No (esfuerzo continuo) |
| 1.2.2 | Informe y presentación | 6,00 | 01/01/2027 | 05/01/2027 | 4.3.1 | **Final** | **Sí** |
| **2** | **Diseño de pruebas** | **26,67** | 30/11/2026 | 04/12/2026 | 1.1.2 | **E2** | Sí |
| **2.1** | **Diseño de casos** | **18,00** | 30/11/2026 | 04/12/2026 | 1.1.2 | | No ⚠ (ver nota 2) |
| 2.1.1 | Diseño: Registro en banca digital | 6,00 | 30/11/2026 | 01/12/2026 | 1.1.2 | | No · holgura 2,67 h (0,8 d) |
| 2.1.2 | Diseño: Creación de cuentas | 6,00 | 01/12/2026 | 03/12/2026 | 1.1.2 | | No · holgura 2,67 h (0,8 d) |
| 2.1.3 | Diseño: Transacciones | 6,00 | 02/12/2026 | 04/12/2026 | 1.1.2 | | No · holgura 2,67 h (0,8 d) |
| **2.2** | **Datos de prueba** | **8,67** | 30/11/2026 | 02/12/2026 | 1.1.2 | | **Sí** |
| 2.2.1 | Generación de datos (sintéticos) | 8,67 | 30/11/2026 | 02/12/2026 | 1.1.2 | | **Sí** |
| **3** | **Automatización** | **65,33** | 04/12/2026 | 21/12/2026 | 2.1, 2.2 | **E3** | Sí |
| **3.1** | **Scripts Playwright + TypeScript** | **65,33** | 04/12/2026 | 21/12/2026 | 2.1, 2.2 | | **Sí** |
| 3.1.1 | Automatización UI | 33,33 | 04/12/2026 | 21/12/2026 | 2.1.1–2.1.3, 2.2.1 | | **Sí** |
| 3.1.2 | Automatización API | 32,00 | 04/12/2026 | 18/12/2026 | 2.1.1–2.1.3, 2.2.1 | | No · holgura 1,33 h (0,4 d) |
| **4** | **Ejecución, defectos y regresión** | **35,17** | 21/12/2026 | 01/01/2027 | 3.1 | | Sí |
| **4.1** | **Ejecución de casos** | **18,00** | 21/12/2026 | 24/12/2026 | 3.1 | | **Sí** |
| 4.1.1 | Ejecución: Registro en banca digital | 6,00 | 21/12/2026 | 22/12/2026 | 3.1.1, 3.1.2 | | **Sí** |
| 4.1.2 | Ejecución: Creación de cuentas | 6,00 | 21/12/2026 | 22/12/2026 | 3.1.1, 3.1.2 | | **Sí** |
| 4.1.3 | Ejecución: Transacciones | 6,00 | 22/12/2026 | 24/12/2026 | 3.1.1, 3.1.2 | | **Sí** |
| **4.2** | **Defectos y re-test** | **5,17** | 24/12/2026 | 28/12/2026 | 4.1 | | **Sí** |
| 4.2.1 | Registro y gestión de defectos | 2,17 | 24/12/2026 | 25/12/2026 | 4.1.1–4.1.3 | | **Sí** |
| 4.2.2 | Re-test | 3,00 | 25/12/2026 | 28/12/2026 | 4.2.1 | **E4** | **Sí** |
| **4.3** | **Regresión** | **12,00** | 28/12/2026 | 01/01/2027 | 4.2 | | **Sí** |
| 4.3.1 | Regresión | 12,00 | 28/12/2026 | 01/01/2027 | 4.2.2 | | **Sí** |

Cuadre de E: 20,08 + 26,67 + 65,33 + 35,17 = **147,25 h** (= B27 del Excel ✔).

**Notas**
1. Las fechas salen de asignar las actividades a 2 personas (3,2 h/día c/u) respetando las dependencias. Un día laborable = 6,4 h del equipo. «Inicio/Fin» es el día laborable en que arranca/termina la tarea (varias comparten día).
2. 2.1.x tienen 2,67 h de holgura lógica, pero con solo 2 personas los tres diseños y 2.2.1 compiten por recurso: uno de los diseños (en el calendario, 2.1.3) retrasa el arranque de la automatización. En la práctica, **no hay colchón entre 1.1.1 y 3.1**.
3. Los códigos 1.2.1 y 1.2.2 quedan bajo «Coordinación y cierre» aunque sus fechas estén en extremos opuestos del proyecto.

## Ruta crítica

**1.1.1 → 1.1.2 → 2.2.1 → 3.1.1 → {4.1.1, 4.1.2, 4.1.3} → 4.2.1 → 4.2.2 → 4.3.1 → 1.2.2**

Cálculo (CPM, una persona por tarea a 3,2 h/día):

| Tramo | Horas |
| :---- | ----: |
| 1.1.1 + 1.1.2 | 7,00 + 6,00 = 13,00 |
| 2.2.1 (más largo que los diseños: 8,67 > 6,00) | 8,67 |
| 3.1.1 (más largo que la API: 33,33 > 32,00) | 33,33 |
| 4.1.x (empatan, 6,00 c/u) | 6,00 |
| 4.2.1 + 4.2.2 + 4.3.1 + 1.2.2 | 2,17 + 3,00 + 12,00 + 6,00 = 23,17 |
| **Longitud de la ruta crítica** | **84,17 h = 26,3 días lab. (5,3 sem)** |

Con recursos reales (2 personas, 4 tareas compitiendo en el tramo de diseño y datos) el proyecto dura **30,05 días lab. (6,0 sem)** y termina el **05/01/2027**. El lastre de 3,75 días sobre el CPM es el costo de tener solo 2 personas.

## Hitos

| Hito | Fecha | Entregable (S5) | E acumulada requerida | Capacidad acumulada a la fecha¹ | Holgura de capacidad | Fin en cronograma | Holgura calendario |
| :---- | :---- | :---- | ----: | ----: | ----: | :---- | :---- |
| E1 | 23/11/2026 | 1.1.1 | 7,00 h | 0 h (antes del inicio) | **−7,00 h** | 26/11/2026 | **−3 d lab. (vencido)** |
| E2 | 04/12/2026 | 1.1 + 2 | 39,67 h | 9 d × 6,4 = 57,6 h | +17,93 h | 04/12/2026 | **0 d** (sin margen) |
| E3 | 18/12/2026 | 3 | 105,00 h | 19 d × 6,4 = 121,6 h | +16,60 h | 21/12/2026 | **−1 d lab.** |
| E4 | 22/01/2027 | 4.1 + 4.2 | 128,17 h | 44 d × 6,4 = 281,6 h | +153,43 h | 28/12/2026 | +19 d lab. |
| Final | 13/02/2027 | todo | 147,25 h | 59 d × 6,4 = 377,6 h | +230,35 h | 05/01/2027 | +28 d lab. |

¹ Días laborables (lun–vie) del 24/11/2026 a la fecha del hito, × 6,4 h.

**Lectura**
- **E1 (23/11) ya venció**: es anterior al inicio del cálculo (24/11). {PENDIENTE: ¿1.1.1 ya está entregado? Si lo está, resta 7 h de E; si no, E1 se incumple.}
- **E2 y E3 son los hitos en riesgo**: hay horas de sobra en total, pero no en el calendario. E2 cierra el mismo día (04/12) y E3 se pasa 1 día porque 3.1.1 (33,33 h) la hace una sola persona. **Mitigación**: repartir 3.1.1 + 3.1.2 (65,33 h) entre ambos → 32,67 h c/u ≈ 10,2 días → termina 18/12 y E3 se cumple. Un feriado (08/12) deja E3 a −2 d.
- **E4 y Final tienen mucha holgura** (19 y 28 días lab.): ahí cabe el colchón de riesgos y más ciclos de regresión.

## Holgura frente a capacidad

**Total E (h):** 147,25 · σ = √Σσ² = 6,54 · E + 2σ = 147,25 + 13,08 = **160,33 h**

| Escenario | Cálculo capacidad | Capacidad (h) | Holgura vs E (147,25) | Holgura vs E+2σ (160,33) | Uso de capacidad | ¿Alcanza? |
| :---- | :---- | ----: | ----: | ----: | ----: | :---- |
| **11 semanas** (CLAUDE.md, S3) | 32 × 11 | **352** | **+204,75 h** | **+191,67 h** | 147,25 / 352 = 41,8 % | **Sí, con margen** |
| 4 semanas (Excel B32) | 32 × 4 | 128 | −19,25 h | −32,33 h | 147,25 / 128 = 115 % | **No** (como dice el Excel) |

- Semanas mínimas necesarias: 147,25 / 32 = **4,60 sem** (esperado) · 160,33 / 32 = **5,01 sem** (con 2σ). Con 4 semanas no cierra; con 6 sí.
- La holgura de 204,75 h equivale a 204,75 / 32 = 6,4 semanas del equipo (58,2 % de la capacidad).
- Si los 32 h fueran **por persona** (64 h/sem), la capacidad a 11 sem sería 704 h y la holgura 556,75 h (S1 por confirmar).

**Total E (h):** 147,25 · **Capacidad (h):** 352 (11 sem × 32 h) · **Holgura:** +204,75 h (+191,67 h frente a E + 2σ) · **Fin del cronograma:** 05/01/2027, 28 días lab. antes del Final

## Observaciones a revisar

1. **Re-test subestimado en el PERT**: 3 h (fila 16) contra 21,6 defectos × 1,5 h = **32,4 h** en la hoja «Defectos esperados» (cifra tomada de `plan.md`, no recalculada aquí). Si se usa 32,4 h: E = 147,25 − 3 + 32,4 = **176,65 h**; sigue entrando en 352 h (holgura 175,35 h) pero no en 128 h. Como 4.2.2 está en la ruta crítica, alargaría el cronograma ≈ 29,4 / 3,2 = 9,2 días lab.
2. **El Excel usa 4 semanas (B32)**; CLAUDE.md calcula 11 y la propuesta habla de 12. El resultado «No: recortar alcance» del Excel cambia a «Sí, con margen» según la ventana. {PENDIENTE: confirmar y, si corresponde, actualizar B32.}
3. **Fuera de la estimación**: performance (la menciona `riesgos.md`) y seguridad no tienen actividad en el PERT; si entran, hay que estimarlas y añadirlas al WBS.
