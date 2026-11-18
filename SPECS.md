# Especificación del panel de administración de AgentHub

**Estado:** especificación funcional y visual  
**Alcance:** prototipo de demostración estático; datos ficticios definidos en el código fuente.

## 1. Producto y administrador

AgentHub es un panel administrativo para supervisar ingresos y descuentos, consultar usuarios, gestionar agentes y sus skills, revisar contratos de alquiler y resolver errores operativos. Está dirigido a una persona administradora que necesita una vista rápida del estado del negocio y acceso directo al detalle de cada registro, sin depender de servicios externos.

El idioma de la interfaz es español. Los datos que se muestran son inventados, coherentes entre vistas y están definidos en el código. No se conectan a API ni backend y las interacciones de demostración no persisten al recargar.

## 2. Restricciones técnicas

- La entrega consiste en **un único `index.html`** con seis vistas que comparten la misma barra lateral, barra superior, paleta visual, tipografía y componentes.
- Usar HTML semántico y JavaScript vanilla. Tailwind CSS se carga **únicamente por CDN** y se usa para todos los estilos.
- No usar frameworks, jQuery, herramientas de build, CSS propio (ni bloques `<style>`, hojas CSS o estilos embebidos) ni atributos `style`.
- Los datos ficticios se declaran en el código de `index.html`; los listados relacionados deben consumir conjuntos consistentes.
- No implementar alta de usuarios/agentes/skills, filtros de datos ni flujos de aprobación de candidatos/contrataciones.
- La aplicación debe ser adaptable a escritorio y tablet, evitar desbordamiento horizontal de página y mantener controles utilizables en ambas orientaciones.

## 3. Estructura y lenguaje visual compartidos

### 3.1 Carcasa y navegación

La aplicación presenta una barra lateral persistentemente visible, una barra superior y un área principal que cambia entre seis vistas: **Dashboard, Gestión de usuarios, Gestión de agentes, Skills, Contrataciones de agentes y Log de errores**. La navegación señala claramente la vista activa, visual y semánticamente (`aria-current="page"`). Cambiar de vista no recrea ni reinicia el estado del tema.

La barra superior incluye el título de la vista actual, contexto del espacio de trabajo y un selector accesible de tema. El selector alterna claro/oscuro aplicando utilidades `dark:` de Tailwind a todo el panel. La preferencia seleccionada se conserva al navegar entre vistas durante la sesión. No se requieren cuentas reales ni autenticación.

### 3.2 Diseño y accesibilidad

Mantener una estética de panel SaaS: tipografía sans serif, superficies y separadores consistentes, acento índigo/violeta y estados claramente identificados. Los estados no se comunican exclusivamente mediante color: siempre incluyen texto. Tablas, encabezados, botones, diálogos, alertas y listas usan elementos semánticos adecuados.

Todos los controles interactivos tienen nombre accesible y foco visible. Los menús de fila se cierran al hacer clic fuera; también deben poder operarse con teclado y cerrarse con Escape. Todos los modales se cierran por su botón de cierre y al hacer clic en el fondo exterior; Escape también los cierra. Al abrir un modal, el foco entra en él; al cerrarlo, vuelve al control que lo abrió. Evitar que el clic dentro del contenido del modal lo cierre.

## 4. Datos ficticios compartidos

Utilizar el mismo conjunto de registros en todas las vistas; identificadores y nombres deben coincidir. Las cifras siguientes son valores fijos de demostración.

### 4.1 Métricas del Dashboard

- Ingresos totales del mes: **$12.480,00**.
- Pérdida por descuentos y cupones: **$860,00**.
- Agentes activos: **2**.
- Agentes fallando: **1**.

### 4.2 Usuarios

| ID | Nombre | Email | Plan | Estado |
|---|---|---|---|---|
| U-101 | Lucía Méndez | lucia@acme.test | Enterprise | Activo |
| U-102 | Mateo Ruiz | mateo@acme.test | Pro | Activo |
| U-103 | Camila Torres | camila@acme.test | Starter | Activo |
| U-104 | Bruno Díaz | bruno@acme.test | Pro | Suspendido |
| U-105 | Sofía Vega | sofia@acme.test | Starter | Invitado |

### 4.3 Agentes y asignaciones de skills

| ID | Nombre | Propietario | Estado | Skills habilitadas |
|---|---|---|---|---|
| AG-01 | Soporte web | Lucía Méndez | Activo | Responder preguntas, Consultar pedidos, Escalar a humano |
| AG-02 | Ventas | Mateo Ruiz | Activo | Responder preguntas, Recomendar productos |
| AG-03 | Devoluciones | Camila Torres | Fallando | Consultar pedidos, Escalar a humano |
| AG-04 | FAQ interno | Lucía Méndez | Inactivo | Responder preguntas |

Conteo por skill: Responder preguntas, 3 agentes; Consultar pedidos, 2; Escalar a humano, 2; Recomendar productos, 1. Total: 8 asignaciones. El conjunto incluye 2 agentes Activos, 1 Inactivo y 1 Fallando.

### 4.4 Catálogo de skills

| ID | Nombre | Descripción | Agentes asignados |
|---|---|---|---:|
| SK-01 | Responder preguntas | Redacta respuestas usando la información de soporte disponible. | 3 |
| SK-02 | Consultar pedidos | Busca el estado y los datos de seguimiento de pedidos. | 2 |
| SK-03 | Escalar a humano | Deriva consultas a una persona del equipo cuando se necesita asistencia. | 2 |
| SK-04 | Recomendar productos | Sugiere productos según las necesidades expresadas por el cliente. | 1 |

### 4.5 Contratos de alquiler

| ID | Cliente | Agente | Skills contratadas | Inicio | Fin | Importe pagado |
|---|---|---|---|---|---|---:|
| CT-501 | Lucía Méndez | Soporte web (AG-01) | Responder preguntas ($120,00), Consultar pedidos ($80,00), Escalar a humano ($50,00) | 01/11/2026 | 30/11/2026 | $250,00 |
| CT-502 | Mateo Ruiz | Ventas (AG-02) | Responder preguntas ($120,00), Recomendar productos ($90,00) | 01/11/2026 | 30/11/2026 | $210,00 |
| CT-503 | Camila Torres | Devoluciones (AG-03) | Consultar pedidos ($80,00), Escalar a humano ($50,00) | 01/10/2026 | 31/10/2026 | $130,00 |
| CT-504 | Lucía Méndez | FAQ interno (AG-04) | Responder preguntas ($120,00) | 01/10/2026 | 31/10/2026 | $120,00 |

Los importes de cada contrato deben coincidir exactamente con la suma de los precios individuales de sus skills. CT-501 y CT-502 están activos en noviembre; CT-503 y CT-504 son contratos pasados.

### 4.6 Errores

| ID | Fecha y hora | Agente | Tipo | Descripción breve |
|---|---|---|---|---|
| ER-801 | 18/11/2026 09:42 | Devoluciones (AG-03) | Error de conexión | No se pudo conectar con el servicio de pedidos. |
| ER-800 | 18/11/2026 09:18 | Soporte web (AG-01) | Tiempo de espera | La consulta de pedidos excedió el tiempo límite. |
| ER-799 | 18/11/2026 08:55 | Ventas (AG-02) | Límite de uso | Se alcanzó el límite de recomendaciones por minuto. |
| ER-798 | 18/11/2026 08:31 | Devoluciones (AG-03) | Error de skill | Falló la ejecución de «Escalar a humano». |
| ER-797 | 18/11/2026 07:46 | FAQ interno (AG-04) | Configuración | Falta una instrucción requerida en el prompt del sistema. |
| ER-796 | 18/11/2026 07:12 | Soporte web (AG-01) | Error de respuesta | El servicio devolvió una respuesta no válida. |

## 5. Especificaciones de las seis vistas

### 5.1 Dashboard

1. Mostrar el título «Dashboard» y cuatro tarjetas de métricas fijas: ingresos totales del mes ($12.480,00), pérdida por descuentos y cupones ($860,00), agentes activos (2) y agentes fallando (1). Cada tarjeta incluye icono decorativo, etiqueta y valor; el icono tiene alternativa accesible si aporta información o queda oculto a lectores de pantalla si es decorativo.
2. Las cuatro tarjetas se ubican en la parte superior del contenido; en escritorio se ordenan en una cuadrícula de cuatro columnas y en tablet en dos columnas. Las métricas se mantienen fijas y no aparentan actualizarse mediante filtros.
3. Inmediatamente debajo, mostrar un **marcador de posición de ancho completo** para el gráfico de actividad semanal. El área debe ocupar todo el ancho de la columna principal, tener título y texto «Gráfico de actividad semanal» y distinguirse visualmente como espacio reservado, no como gráfico funcional ni como tarjeta lateral.
4. La estructura se adapta al ancho sin crear desplazamiento horizontal de la página; el marcador de gráfico conserva su ancho completo bajo las tarjetas en todos los tamaños contemplados.

### 5.2 Gestión de usuarios

1. Mostrar una tabla semántica con al menos los cinco registros U-101 a U-105. Las columnas incluyen nombre, email, plan, estado y menú de acciones. El estado se presenta como badge con texto: Activo, Suspendido o Invitado.
2. Cada fila incluye un botón de menú «⋮» con nombre accesible vinculado al usuario y las opciones «Ver detalle» y «Eliminar». El menú se puede abrir por teclado, se cierra al hacer clic fuera y no se recorta en los bordes del contenedor.
3. «Ver detalle» abre un modal con el registro completo: ID, nombre, email, plan y estado. El contenido usa etiquetas descriptivas; el modal dispone de un botón para cerrar y cierra también al hacer clic en el fondo exterior o presionar Escape.
4. «Eliminar» solicita confirmación antes de retirar el registro del estado local de demostración. Cancelar no modifica la tabla; confirmar actualiza la lista y anuncia el resultado.
5. Adaptar la tabla a pantallas estrechas mediante un contenedor con desplazamiento local o tarjetas con las etiquetas de columna, sin ocultar datos ni generar desbordamiento de página.

### 5.3 Gestión de agentes

1. Mostrar al menos los cuatro registros AG-01 a AG-04 con nombre, propietario y badge textual de estado: Activo, Inactivo o Fallando. Los nombres y propietarios coinciden exactamente con las tablas de contratos, skills y errores.
2. Las skills asociadas se ocultan al inicio. Cada agente tiene un control accesible para expandir/colapsar la lista, con `aria-expanded` y `aria-controls`; la transición es suave, respeta `prefers-reduced-motion` y no impide el uso por teclado.
3. Cada agente tiene un menú «⋮» con «Configurar» y «Eliminar». Los menús se cierran al hacer clic fuera. Eliminar requiere confirmación y no borra ni altera las asignaciones de otros agentes.
4. «Configurar» abre un modal con nombre del agente y el prompt de sistema en un `textarea` editable, etiquetado y con valor ficticio inicial. Un control para guardar aplica el texto solo al estado local y anuncia el resultado; cerrar/cancelar descarta cambios no guardados.
5. El modal puede cerrarse con su botón, clic en el fondo exterior o Escape; al abrirse recibe el foco y al cerrarse lo devuelve al botón «Configurar» correspondiente.

### 5.4 Skills

1. Mostrar una explicación breve: «Una skill es una capacidad reutilizable que puede habilitarse en uno o más agentes». Presentar el catálogo de cuatro skills SK-01 a SK-04 con nombre, descripción y cantidad de agentes que la tienen habilitada.
2. Los contadores deben ser 3 para Responder preguntas, 2 para Consultar pedidos, 2 para Escalar a humano y 1 para Recomendar productos, según las asignaciones visibles en Gestión de agentes.
3. Cada skill dispone de menú «⋮» con «Ver detalle» y «Eliminar». «Ver detalle» abre un modal que incluye ID, nombre, descripción y los agentes con la skill habilitada; sus nombres deben coincidir con el listado de agentes.
4. «Eliminar» requiere confirmación y actualiza solo la demo. La interfaz no debe dejar contadores contradictorios: si se confirma la eliminación de una skill, debe retirarla también de las asignaciones presentadas en agentes y recalcular los conteos de la sesión, o alternativamente marcar la acción como demostrativa sin alterar el catálogo.
5. Los modales y menús se operan con teclado y se cierran con los comportamientos compartidos; los nombres y descripciones largas se adaptan sin desbordamiento.

### 5.5 Contrataciones de agentes

1. Mostrar una tabla con al menos los cuatro contratos CT-501 a CT-504. Incluir cliente, agente, skills contratadas, fechas de inicio y fin e importe pagado; identificar claramente cuáles están activos y cuáles pasados según el periodo de referencia.
2. Los nombres de cliente/agente y skills deben coincidir con las vistas de usuarios, agentes y catálogo. Las skills se muestran resumidas en la tabla, sin omitirlas; el importe total debe ser igual a la suma de los precios individuales definidos en los datos comunes.
3. Cada fila incluye menú «⋮» con «Ver detalle». El modal muestra el desglose completo: ID, cliente, agente, estado temporal, fechas, cada skill, su precio individual y el importe total. Validar la suma al construir los datos de demostración.
4. El menú se cierra al hacer clic fuera. El modal se cierra con su botón, clic en el fondo exterior o Escape y administra el foco correctamente.
5. En pantallas estrechas, permitir desplazamiento horizontal dentro de la tabla o presentar tarjetas con todas las etiquetas y el desglose accesible; no provocar desbordamiento de la página.

### 5.6 Log de errores

1. Mostrar al menos los seis eventos ER-796 a ER-801 con fecha y hora, agente, tipo y descripción breve. Los cuatro nombres de agente deben ser idénticos a los usados en Gestión de agentes.
2. Presentar el tipo/estado mediante badge que combine texto y color, conservando significado en tema claro y oscuro. Ordenar de más reciente a más antiguo como en los datos de ejemplo.
3. Cada fila incluye menú «⋮» con «Ver detalle» y «Marcar como resuelto». Al marcar como resuelto, actualizar la etiqueta de estado local, impedir duplicar el cambio y anunciar la confirmación; la severidad original sigue visible.
4. «Ver detalle» abre un modal con ID, fecha y hora, agente, tipo, descripción y una traza completa ficticia, claramente identificada como simulada. No incluir secretos ni atribuir la traza a un sistema real.
5. Modal y menú siguen los comportamientos comunes de teclado, clic exterior y botón de cierre. La tabla se adapta a tablet sin desplazamiento horizontal de la página.

## 6. Inventario de componentes reutilizables

| Componente | Requisitos |
|---|---|
| Carcasa de aplicación | Barra lateral persistente, barra superior, área de contenido y tema global compartido. |
| Navegación lateral | Seis enlaces, foco visible, vista actual marcada visual y semánticamente. |
| Selector claro/oscuro | Usa utilidades `dark:` de Tailwind y mantiene preferencia al cambiar de vista. |
| Encabezado de vista | Título consistente y descripción opcional. |
| Tarjeta métrica | Icono, etiqueta y valor; adaptable y accesible. |
| Tabla/listado | Encabezados semánticos, filas asociadas a datos y adaptación a pantallas estrechas. |
| Badge | Texto y color para estado/errores; no depender solo del color. |
| Menú de acciones | Botón ⋮ accesible, opciones enfocables, cierre al clic exterior y Escape. |
| Modal de detalle/configuración | Título, contenido etiquetado, foco gestionado, cierre con botón, clic exterior y Escape. |
| Confirmación | Explica el impacto de eliminar y ofrece cancelar/confirmar sin ambigüedad. |
| Expandible de skills | Control con estado `aria-expanded`, asociación al contenido y transición respetuosa de movimiento reducido. |
| Notificación | Mensajes de resultado con región viva apropiada, sin mover el foco inesperadamente. |
| Marcador de gráfico | Espacio reservado de ancho completo, título y texto que indica que no es visualización funcional. |

## 7. Criterios de aceptación

1. La entrega descrita consta de un solo `index.html` y presenta seis vistas: Dashboard, Gestión de usuarios, Gestión de agentes, Skills, Contrataciones de agentes y Log de errores, con una carcasa visual compartida.
2. La especificación de implementación se cumple usando HTML semántico, JavaScript vanilla y Tailwind únicamente por CDN; no usa frameworks, jQuery, build tools, CSS propio ni atributos `style`.
3. Dashboard presenta cuatro tarjetas con icono, etiqueta y los valores fijos definidos; debajo, el marcador del gráfico semanal ocupa el ancho completo.
4. Gestión de usuarios muestra al menos cinco usuarios con nombre, email, plan y badge de estado. El menú de cada fila se cierra al clic exterior; «Ver detalle» abre un modal con el registro completo y «Eliminar» pide confirmación.
5. Gestión de agentes muestra al menos cuatro agentes con nombre, propietario y estado correcto. Las skills empiezan ocultas y se expanden/colapsan con control accesible y transición; «Configurar» abre un modal cuyo prompt es editable en textarea y «Eliminar» confirma antes de actuar.
6. Skills incluye explicación, cuatro registros con nombre, descripción y contadores 3, 2, 2 y 1. «Ver detalle» muestra la skill y agentes asociados en modal; la eliminación no deja asignaciones ni conteos contradictorios.
7. Contrataciones de agentes muestra al menos cuatro registros activos/pasados con cliente, agente, skills, fechas e importe. Cada detalle modal enumera precios individuales y la suma coincide con el total del contrato.
8. Log de errores incluye al menos seis filas con fecha/hora, agente, tipo y descripción. Badges comunican texto y color; detalle muestra traza ficticia completa y «Marcar como resuelto» actualiza el estado local una sola vez.
9. Los nombres de agentes coinciden en listados, contratos y errores; propietarios/clientes y skills relacionados concuerdan con las tablas de referencia.
10. La barra lateral permanece visible, marca la vista activa y permite navegar entre las seis vistas sin perder el tema seleccionado.
11. El selector de tema cambia claro/oscuro en todo el panel mediante utilidades `dark:` de Tailwind y mantiene la preferencia al cambiar de vista.
12. Los menús se cierran al hacer clic fuera. Todos los modales se cierran por botón y clic en el fondo exterior; el clic dentro del modal no lo cierra.
13. Interacciones principales disponibles por teclado, foco visible y nombres accesibles. Apertura/cierre de modales gestiona el foco y Escape los cierra.
14. Las tablas se adaptan sin provocar desplazamiento horizontal de página; solo se permite desplazamiento dentro del contenedor de tabla.
15. No se incluyen formularios de alta, filtros ni flujo de aprobación de candidatos; las acciones requeridas se limitan a las especificadas para cada vista.
