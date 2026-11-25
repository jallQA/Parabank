# Contexto del proyecto final (lo lee Claude en cada sesión)

## Caso

- Sistema bajo prueba: **ParaBank** (banco demo de Parasoft) · tipo **web + API** · ambiente de pruebas público: https://parabank.parasoft.com/parabank/ (sin autorización requerida)
  - Usuarios: {PENDIENTE: ¿perfiles a considerar? p. ej. cliente nuevo, cliente registrado, administrador}
- Problema de calidad: la ejecución de casos de prueba es **100 % manual** y la **regresión toma 4 días**; no existe automatización ni se usa IA en el proceso.
- Valor esperado: reducir el tiempo de diseño, generación y ejecución de casos de prueba.
- Flujos críticos (3):
  1. Registro en canales digitales (registro de cliente en banca digital)
  2. Apertura / creación de cuentas
  3. Transacciones bancarias (p. ej. transferencias) {PENDIENTE: ¿qué transacciones entran: transferencia, pago de facturas, solicitud de préstamo?}
- Agente IA previsto: generador de casos · automatizador de casos · analizador de defectos · evaluador de requerimientos. IA en la nube (Claude/Gemini).
- Modalidad: **grupo de 2** · Integrantes: Jean Claude Ezquerra Ferruzo, Javier Loarte Leiva

## Restricciones

- Horas por semana: **32 h** (fuente: `Plan_Estimacion_QA.xlsx`, hoja «PERT proyecto», B31) {PENDIENTE: ¿32 h es el total del equipo (16 h c/u) o por persona?}
- Semanas hasta el 13/02/2027: **11**
  - Cálculo: del 24/11/2026 al 13/02/2027 = 6 (nov) + 31 (dic) + 31 (ene) + 13 (feb) = 81 días → 81 / 7 = 11,6 → **11 semanas completas**
  - Ojo: el Excel usa 4 semanas (B32) y la propuesta habla de 12 semanas. {PENDIENTE: confirmar cuál es la ventana de planificación}
- Estimación vigente (hoja «PERT proyecto»):
  - Total esperado PERT E = 147,25 h · σ = √Σσ² = 6,54 h · E + 2σ = 160,3 h
  - Con 32 h × 4 sem = 128 h → **no alcanza** (128 < 160,3), como indica el Excel
  - Con 32 h × 11 sem = 352 h → alcanzaría (352 ≥ 160,3), si se confirma la ventana de 11 semanas
- Hitos: E1 23/11 · E2 04/12 · E3 18/12 · E4 22/01 · Final 13/02
- Stack: Playwright + TypeScript, GitHub Actions
- Datos: solo sintéticos o anonimizados (el equipo genera datos sintéticos). Nunca datos reales de clientes ni secretos.

## Cómo quiero que trabajes

- Escribe en español, en Markdown, en la carpeta proyecto/.
- Muestra los cálculos. No inventes cifras: si te falta un dato, pregúntalo o márcalo como SUPUESTO.
- Antes de crear o cambiar un archivo, dime qué vas a hacer.
