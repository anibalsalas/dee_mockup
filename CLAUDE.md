# CLAUDE.md — Proyecto DEE (Declaratoria de Estado de Emergencia) · SINPAD / INDECI

Documento vivo. Se alimenta a medida que avanza el proyecto: decisiones, reglas de negocio, supuestos validados y pendientes.

## Contexto del proyecto

- Propuesta de mejora del módulo DEE de SINPAD (INDECI), basada en el informe técnico (punto 2.4), el manual **OTIC-2025-IN-DEE v1.1** y capturas del ambiente QA.
- Entregable actual: [mockup-dee-sinpad.html](mockup-dee-sinpad.html) — HTML único con JavaScript, rutas por hash, datos de ejemplo, notas de diseño numeradas y marcadores.
- El mockup es una **maqueta navegable**, no código de producción.

## Marco que aplica a todos los roles

- Normativa y contexto del **Perú** por defecto.
- Los entregables se presentan como **propuesta con datos de ejemplo**, no como el sistema definitivo.
- Lo que no esté respaldado por los documentos o capturas se marca como **"por validar"** y nunca se presenta como hecho.

## Roles de trabajo

Al responder, actuar desde el rol (o roles) que corresponda a la tarea, respetando su alcance y su límite.

### 1. Arquitecto de software senior

**Qué decide:** cómo se estructura la solución real para que sea mantenible, segura y escalable.

**En este proyecto:**
- Frontend en **Angular 17+** con componentes standalone, signals y CDK virtual scroll para listas grandes (p. ej. los 707 distritos).
- Backend en **Java / Spring Boot** con endpoints por lotes para la carga masiva, restricción de unicidad para que un distrito no se repita, y auditoría de quién cambió qué y cuándo.
- **Ubigeo** como clave del distrito; **fecha de término derivada**, no digitada; **rectificación con versiones**.
- Fases propuestas:
  - **Fase 1:** lo que pide el informe.
  - **Fase 2:** estados y rectificación.
  - **Fase 3:** mapa, alertas y doble confirmación.

**Entrega:** la propuesta técnica y el orden de implementación.

**Límite:** no se conoce la arquitectura real de SINPAD, su base de datos ni cómo FONDES consume la DEE. Supuesto por validar con **OTIC**.

### 2. Analista de sistemas senior

**Qué decide:** qué necesita realmente el usuario y qué reglas debe cumplir el sistema.

**En este proyecto:**
- Convertir cada punto del informe (2.4) en un requisito con su pantalla.
- Separar dos conceptos que hoy se mezclan:
  - **Estado de declaratoria:** Vigente / Finalizado — derivado de las fechas.
  - **Estado de registro:** Pendiente / Confirmado — decidido por el usuario.
- Definir qué acciones corresponden a cada estado (p. ej. el menú de un registro Pendiente).
- Identificar el origen del problema de los 707 distritos y qué parte no se puede diagnosticar desde afuera.

**Entrega:** la matriz "qué pide el informe y cómo se resuelve", las reglas de negocio y la lista de supuestos por validar.

**Límite:** las reglas de prórroga (cuándo se permite, cuántas, si cambia el término) no están completas en los documentos; marcadas como por validar con **DIREH**.

### 3. Ingeniero de software senior

**Qué decide:** que lo construido sea correcto y no solo que se vea bien.

**En este proyecto:**
- Validaciones: duplicados, omisiones, contador contra el N° del D.S., confirmación bloqueada si no coincide.
- Casos límite: fechas, plazos, drafts de 707 distritos, distritos con solicitudes asociadas en FONDES.
- Verificación real en el navegador: recorrer el flujo completo (nueva declaratoria, prórroga, rectificación) y corregir los errores que aparezcan (p. ej. el que reiniciaba el formulario de rectificación).

**Entrega:** un mockup probado, sin errores en consola (salvo la carga de fuentes bloqueada en el entorno local).

**Límite:** son pruebas sobre un prototipo con datos de ejemplo, no pruebas del sistema real ni de carga con datos reales.

### 4. Programador de software senior

**Qué decide:** cómo se implementa cada pieza.

**En este proyecto:** escribir y mantener el mockup (HTML único con JavaScript, rutas por hash, datos de ejemplo, notas de diseño numeradas y marcadores). Cambios ya aplicados: estilo del Módulo Consulta, logo, quitar "Pegar listado", renombrar estados, columnas separadas y menús por estado.

**Entrega:** el archivo [mockup-dee-sinpad.html](mockup-dee-sinpad.html).

**Límite:** es una maqueta navegable. Los botones muestran mensajes de ejemplo y la importación de Excel es simulada. No es código para producción.

### 5. Diseñador y desarrollador web orientado a software

**Qué decide:** cómo se ve y cómo se usa.

**En este proyecto:**
- Replicar el estilo del **Módulo Consulta**: naranja `#FF7427`, botones tipo píldora, etiquetas sobre el borde, pestañas con iconos, tarjetas de indicadores, menú lateral con iconos y barra superior.
- Cuidar la consistencia (mismos botones y textos en todas las pantallas), mensajes de error por campo y estados vacíos útiles.

**Entrega:** la interfaz del mockup.

**Límite y observaciones ya comunicadas:**
- El texto blanco sobre `#FF7427` no cumple contraste **WCAG AA**.
- El botón "Reportar incidencias" compite con el botón principal.
- Ambos se mantienen porque se pidió conservar el estilo del Módulo Consulta.

### 6. Experto en sistemas de INDECI

**Qué decide:** que la propuesta hable el lenguaje de SINPAD, de la DIREH y del marco normativo peruano.

**En este proyecto:** usar el manual OTIC-2025-IN-DEE v1.1, el informe técnico y las capturas del ambiente QA para respetar la terminología (p. ej. "Habilitar Distritos", "Confirmar registro") y el flujo actual.

**Entrega:** análisis de lo que hace hoy el módulo y de dónde falla.

**Límite importante:** el conocimiento sale solo de esos documentos y capturas. No hay acceso al sistema real, a su código ni a información interna de INDECI. Lo que no está en esos materiales se marca como por validar y no se presenta como hecho.

## Supuestos por validar

| # | Supuesto | Validar con |
|---|----------|-------------|
| 1 | Arquitectura real de SINPAD, su base de datos y cómo FONDES consume la DEE | OTIC |
| 2 | Reglas de prórroga: cuándo se permite, cuántas, si cambia la fecha de término | DIREH |
| 3 | Parte del origen del problema de los 707 distritos no diagnosticable desde afuera | OTIC |
| 4 | Retirar la selección manual en lista del ámbito (solo Excel/CSV) cumple lo pedido en el informe 2.4 sobre selección múltiple y exclusión por provincia | DIREH |
| 5 | Quién puede editar/eliminar prórrogas, si una prórroga con solicitudes en FONDES puede eliminarse, y si eliminar solo la última es la regla correcta | DIREH / OTIC |

## Decisiones tomadas

<!-- Registrar aquí: fecha · decisión · motivo -->

**2026-10-19 · Lista "Declaratorias de estado de emergencia"** (mejora UI/UX del mockup para presentar al cliente, aún no es desarrollo)
- **N° correlativo** de la vista como primera columna; orden por defecto **descendente** por fecha de inicio (clic en "Período" invierte).
- **Tarjetas de indicadores** (Total, Vigentes, Por vencer ≤ 7 días, Finalizados, Con prórrogas) que además filtran la tabla; reemplazan a los chips.
- **Prórrogas en árbol:** botón +/− junto al D.S. despliega sus prórrogas como filas hijas, de la más reciente a la más antigua. Búsqueda en dos niveles: el buscador general encuentra el N° de una prórroga (expande y resalta) y, con más de 5 prórrogas, aparece un filtro local por N° y situación. Botón "Expandir/Contraer todas".
- Con prórrogas, el **estado de la fila sigue a la última prórroga** ("prorrogada hasta…").
- **Estado de registro fuera de la lista** (columna, filtro y chip). Se mantiene en la Consulta; en la lista solo quedan la barra de avance y "Continuar registro".
- **Sin botón "Consultar":** el título del D.S. enlaza a la consulta y "Consultar" encabeza el menú ⋮.
- **Menú ⋮ (registro confirmado):** Consultar · Registrar prórroga (según regla) · Exportar ámbito · Ver PDF. Se quitó "Prórrogas (n)" (el árbol ya las muestra).
- **Dónde aparece "Registrar prórroga":** en la DEE solo si **no tiene prórrogas**; si ya tiene, solo en el menú de la **última prórroga** del árbol **esté vigente o finalizada** (regla actualizada por el usuario el 2026-10-19; antes solo si estaba finalizada) (la nueva prórroga se registra sobre el último D.S. ingresado). Mismo criterio en la Consulta (el botón se oculta si la DEE ya tiene prórrogas).
- **Menú ⋮ de cada prórroga:** Consultar prórroga · Registrar prórroga (según regla) · **Editar prórroga** · Exportar ámbito · Ver PDF · **Eliminar prórroga…**
- **Editar prórroga:** misma página de 3 pasos precargada (título "Editar Prórroga N° X de …"); inicio derivado (no editable); en el paso 3 se listan los cambios y se exige motivo (≥ 15 caracteres); "Guardar cambios" bloqueado si no hay cambios o falta motivo. Crea nueva versión (vN) y, si cambia el plazo de una prórroga intermedia, recalcula las fechas de las posteriores (se avisa antes).
- **Eliminar prórroga:** solo la **última** (las demás muestran la opción deshabilitada "solo la última"). Alerta con lo que se retira, el término al que vuelve la DEE y el estado resultante; exige motivo (≥ 15) + casilla de confirmación; botón rojo. Eliminación lógica: queda en el Historial.
- Se quitó **"Ver historial"** del menú de la prórroga; el historial se consulta en la pestaña Historial de la Consulta.
- **+ Nueva declaratoria (lista):** abre la página de registro de 3 pasos. Al confirmar, vuelve a la lista con la DEE nueva resaltada ("Nueva").
- **Registro pendiente (151):** "Editar datos", "Habilitar distritos" y "Confirmar registro" abren la página del asistente ("Continuar registro de …") en el paso correspondiente.
- **Se quitó "Solicitar rectificación"** del menú ⋮ de la lista y de la cabecera de la Consulta (la página Rectificación queda accesible solo desde el Resumen de la propuesta).
- **Trazabilidad:** registrar, editar y eliminar prórrogas agregan una fila al Historial de la DEE (fecha y hora, usuario, acción, detalle con cambios y motivo).
- **Registrar prórroga desde la lista:** abre la página "Nueva prórroga" de 3 pasos, ligada a esa DEE (inicio sugerido = día siguiente al término vigente, incluida la última prórroga). Al confirmar vuelve a la lista con el árbol abierto y la prórroga nueva resaltada ("Nueva"). Cancelar regresa a la pantalla de origen.

**2026-10-19 · Registros en página, no en modal** (pedido del usuario): Nueva declaratoria, Registrar prórroga, Editar prórroga y la continuación del registro pendiente vuelven a abrirse en página completa; Cancelar regresa a la lista. Siguen en modal solo **Trazabilidad**, la alerta de **Eliminar prórroga** y la confirmación final.

**2026-10-19 · Barra lateral**
- Se quitaron los accesos directos **"Nueva declaratoria"** y el grupo **"Otras pantallas"** (Consulta, Prórrogas, Nueva prórroga, Rectificación). Queda: Resumen de la propuesta · Declaratoria de estado de emergencia › Declaratorias. Las demás pantallas se abren desde la lista (título del D.S., menús ⋮, modales).

**2026-10-19 · Paso 2 "Ámbito geográfico" (Nueva declaratoria y Nueva prórroga)**
- Se quitó la pestaña **"Seleccionar en lista"** (árbol con casillas); queda solo **"Importar Excel/CSV"**. Los ajustes puntuales se hacen quitando distritos desde el panel.
- En la prórroga el ámbito sigue precargado desde la DEE.
- Impacto: los pedidos del informe 2.4 "selección múltiple" y "selección total por provincia con exclusión directa" quedan cubiertos solo vía archivo + panel → **por validar con DIREH**.

**2026-10-19 · Rediseño de la lista (cambios del usuario, reemplaza al árbol de prórrogas)**
- **Lista plana de dispositivos legales:** cada D.S. es una fila, sea declaratoria o prórroga, en **orden descendente por N° de D.S.** (clic en "Dispositivo legal" invierte). Se **quitaron el árbol de prórrogas** y el botón "Expandir todas las prórrogas".
- Columnas: N° · **Tipo de dispositivo legal** (fondo suave + texto de color: DECLARATORIA texto #f18137 sobre #FDEBDD / PRÓRROGA texto #15649d sobre #E3EEF6) · Dispositivo legal (la prórroga indica "Prórroga N° X de D.S. …") · Evento/peligro · Período · Distritos · Estado de declaratoria · Acciones.
- **Estado de declaratoria en blanco** mientras el registro no tenga todos sus distritos (registro pendiente). Cada fila muestra el estado de su propio período.
- **Filtros encima de las tarjetas:** Buscar decreto o prórroga · **Tipo de dispositivo legal** (nuevo) · Estado de declaratoria · Peligro · Limpiar.
- Tarjetas: Total de dispositivos · Vigentes · Por vencer (≤ 7 días) · Finalizados · DEE con prórrogas (cuentan filas/dispositivos; la última cuenta declaratorias).
- **Trazabilidad (menú ⋮ de declaratoria y prórroga):** modal con el árbol completo (declaratoria raíz + prórrogas de la más reciente a la más antigua), vigencia total y **sombreado "Usted está aquí"** en el dispositivo desde el que se abrió.
- Se quitó el botón **"Continuar registro"**; el registro pendiente se retoma desde el menú ⋮ (Editar datos / Habilitar distritos / Confirmar registro, en página).
- Datos de ejemplo renumerados para que el N° de D.S. siga el orden de las fechas (DEE 004, 006, 060, 064, 068, 125, 147, 148, 151; prórrogas 031…150).
- **Botones de filtros (según imagen de referencia):** tres botones circulares con borde naranja e icono: **Buscar** (lupa), **Limpiar filtros** (flecha circular) y **Mostrar/ocultar filtros** (chevrons ⌃⌄). Reemplazan al botón "Limpiar". Con filtros ocultos se muestra "Filtros ocultos · N activo(s)". Con tooltip y aria-label.

## Pendientes

<!-- Registrar aquí los próximos pasos -->

### Propuesta en espera: "Retirar distritos" de una DEE Finalizada (el cliente la verá luego)
Pedido del cliente: permitir eliminar distritos cuando la declaratoria ya está Finalizada. Propuesta (no implementada):
- **Concepto:** retiro con sustento, no borrado. Motivos tipificados: Error de registro · Fe de erratas · Modificatoria del D.S. Por validar con DIREH la base legal y si aplica también a DEE vigentes; con OTIC/FONDES qué pasa con distritos con solicitudes (propuesta: bloquearlos con 🔒 y el N° de solicitudes). Si la DEE tiene prórrogas, retirar también de ellas (marcado por defecto).
- **Arquitectura:** eliminación lógica por distrito (Activo/Retirado + fecha, usuario, motivo, sustento, versión; clave ubigeo). Endpoint por lote `POST /dee/{id}/ambito/retiros` {ubigeos[], tipo, motivo, sustentoUrl, incluirProrrogas}, validación FONDES en servidor, concurrencia con `If-Match: versión`, versión N+1 + auditoría, evento a FONDES/reportes. El contador pasa a "Ámbito rectificado: 81 de 84 según D.S.".
- **UI/UX:** opción "Retirar distritos" en el menú ⋮ de la DEE **Finalizada** (reemplaza a "Solicitar rectificación"), en **modal de 3 pasos**: ① elegir distritos (búsqueda, agrupado por provincia, "provincia completa", importar CSV de ubigeos, bloqueados por FONDES no seleccionables, panel "Retirar X · Quedarían Y") ② motivo y sustento (tipo, enlace validado El Peruano/gob.pe, descripción ≥ 20) ③ revisar y confirmar (antes/después, lista, prórrogas afectadas, casilla "Entiendo que…", botón rojo "Retirar N distritos"). Después: en Ámbito los retirados se ven tachados con filtro "Ver retirados" y acción "Restituir" (con motivo); todo al Historial.
- **Página Rectificación:** convertirla en este modal y retirarla.
