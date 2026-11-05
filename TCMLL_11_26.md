# Suite de Casos de Prueba - Plataforma de Subasta de Vehículos

---

## 1. Monitoreo en Línea de Subastas (Módulo Administrador)

| ID | Título del Caso | Tipo | Pasos | Resultado Esperado |
|---|---|---|---|---|
| **MEL-01** | Visualización simultánea (1 a 2 subastas) | Funcional | 1. Ingresar al módulo de Monitoreo.<br>2. Seleccionar 2 subastas activas simultáneas. | El panel se divide correctamente mostrando información dinámica en tiempo real de ambas subastas sin lag ni distorsión visual. |
| **MEL-02** | Límite máximo de subastas en pantalla | Regla de Negocio | Intentar seleccionar una 3ª subasta activa para monitorear. | El sistema impide seleccionar más de 2 subastas simultáneas (muestra alerta o desactiva la opción de añadir otra). |
| **MEL-03** | Diferenciación de tipos de subasta | Funcional / UI | Monitorear en paralelo una subasta de **Vehículo Individual**, una de **Litigio** y una de **Conjunto**. | La interfaz muestra claramente los distintivos/etiquetas del tipo de subasta y la estructura de datos correspondiente (ej. en subasta de conjunto muestra el paquete global y no puja individual por auto). |
| **MON-04** | Actualización en tiempo real (WebSockets/Sockets) | Rendimiento / UI | Realizar pujas desde la vista de cliente mientras el admin monitorea. | El cronómetro, el historial de pujas y la oferta actual se actualizan en tiempo real en la vista de monitoreo sin refrescar la página. |

---

## 2. Carga Masiva de Aprobación de Pagos (CSV)

| ID | Título del Caso | Tipo | Pasos | Resultado Esperado |
|---|---|---|---|---|
| **CMP-01** | Carga masiva exitosa con CSV válido | Funcional | 1. Subir un archivo `.csv` con datos válidos de comprobantes (ID transacción, estado "Aprobado", monto, etc.).<br>2. Procesar el archivo. | Los pagos se marcan como aprobados en la base de datos y se actualiza el estado de la oferta/adjudicación de los clientes correspondientes. |
| **CMP-02** | Validación de estructura y formato de archivo | Validación / Negativo | Subir un archivo en formato `.xlsx`, `.pdf` o un `.csv` con delimitadores incorrectos (ej. punto y coma en lugar de comas). | El sistema muestra un mensaje de error claro indicando que el formato no es válido y rechaza la carga. |
| **CMP-03** | Manejo de registros parciales o inconsistentes | Negativo / Edge Case | Subir un CSV donde 5 registros son válidos y 2 tienen ID de comprobante/usuario inexistente. | El sistema procesa los 5 registros correctos y genera un reporte/resumen de errores indicando exactamente qué filas fallaron (ej. "Fila 4 y 6: ID no encontrado"). |
| **CMP-04** | Prevención de duplicidad de pagos | Regla de Negocio | Cargar un CSV con un comprobante que ya fue aprobado previamente. | El sistema omite el registro duplicado o notifica que el pago ya fue procesado, evitando inconsistencias contables. |

---

## 3. Carga Masiva de Precios Bases de Vehículos (CSV)

| ID | Título del Caso | Tipo | Pasos | Resultado Esperado |
|---|---|---|---|---|
| **CMV-01** | Actualización masiva de precios base exitosa | Funcional | Cargar un CSV con pares `ID_Vehiculo` y `Nuevo_Precio_Base` válidos. | Los precios base de los vehículos especificados se actualizan inmediatamente en el inventario y en las subastas no iniciadas. |
| **CMV-02** | Modificación de precio en subasta activa/finalizada | Regla de Negocio | Incluir en el CSV el ID de un vehículo que pertenece a una subasta que **ya está en curso** o **finalizada**. | El sistema rechaza la actualización para ese vehículo en particular y notifica que no se pueden modificar precios de subastas activas o cerradas. |
| **CMV-03** | Validación de valores numéricos y no negativos | Validación | Incluir valores negativos, letras, caracteres especiales o ceros en la columna de precios. | El sistema bloquea el procesamiento de esas filas y despliega alertas de validación de formato numérico. |

---

## 4. Nuevas Plantillas de Correos (Visual & Transaccional)

| ID | Título del Caso | Tipo | Pasos | Resultado Esperado |
|---|---|---|---|---|
| **PCO-01** | Renderizado y diseño Responsive | UI / Cross-client | Gatillar el envío de correos (Creación de cuenta, Subasta ganada, Cambio de contraseña) y abrir en Gmail, Outlook y dispositivos móviles. | El diseño HTML/CSS se ajusta correctamente, sin imágenes rotas, sin desbordamiento de texto y respetando la nueva línea gráfica. |
| **PCO-02** | Inyección correcta de variables dinámicas | Funcional | Generar correos de "Subasta Ganada" y "Cambio de Contraseña". | Los tags dinámicos se reemplazan correctamente (ej. Nombre del cliente, Monto ganador, Placa del vehículo, Enlace de recuperación tokenizado funcional). |
| **PCO-03** | Entrega de correos masivos / Spam check | Integración | Ejecutar acciones que disparen notificaciones a múltiples usuarios simultáneamente. | Los correos llegan a la bandeja de entrada (no Spam) y respetan las cabeceras/DKIM/SPF configuradas. |

---

## 5. Registro de Vehículos Nuevos (Nuevo Formulario)

| ID | Título del Caso | Tipo | Pasos | Resultado Esperado |
|---|---|---|---|---|
| **RVH-01** | Registro completo de vehículo exitoso | Funcional / UX | Completar el nuevo formulario simplificado con todos los campos obligatorios y opcionales. Save/Guardar. | El vehículo se crea exitosamente en estado "Disponible/Borrador" y el flujo resulta más fluido y rápido que en la versión anterior. |
| **RVH-02** | Validación de campos obligatorios y máscaras | Validación | Intentar guardar el formulario dejando campos requeridos vacíos (ej. VIN/Chasis, Marca, Modelo, Año). | El formulario resalta los campos faltantes con mensajes de error descriptivos e impide el envío. |
| **RVH-03** | Persistencia de datos al navegar/descartar | UX | Llenar parcialmente el formulario y refrescar o salir de la pantalla. | El sistema solicita confirmación de salida para evitar pérdida accidental de datos. |

---

## 6. Registro / Repositorio Fotográfico

| ID | Título del Caso | Tipo | Pasos | Resultado Esperado |
|---|---|---|---|---|
| **RFO-01** | Carga y almacenamiento sin vehículo previo | Funcional | 1. Ingresar al Repositorio Fotográfico.<br>2. Subir imágenes sin asignarlas a ningún vehículo aún. | Las fotos se cargan correctamente en el repositorio central y quedan en estado "Sin asignar" / "Disponibles". |
| **RFO-02** | Edición y sustitución masiva/directa de imágenes | Funcional | Alterar/reemplazar imágenes de un vehículo directamente desde el repositorio general. | Las imágenes se actualizan inmediatamente en el detalle del vehículo sin necesidad de entrar a la ficha de edición individual del vehículo. |
| **RFO-03** | Restricciones de formato, peso y resolución | Validación | Subir archivos con formatos no permitidos (`.exe`, `.pdf`) o imágenes que superen el tamaño máximo permitido (ej. > 10MB). | El sistema rechaza los archivos no válidos y muestra las restricciones de carga. |
| **RFO-04** | Vinculación posterior de imágenes a un vehículo | Funcional | Asignar un lote de fotos guardadas en el repositorio a un vehículo recién creado. | El vehículo adopta las imágenes seleccionadas y su galería principal se actualiza adecuadamente. |

---

## 7. Subastas Masivas (Creación mediante CSV)

| ID | Título del Caso | Tipo | Pasos | Resultado Esperado |
|---|---|---|---|---|
| **CSM-01** | Creación masiva de subastas desde CSV | Funcional | Subir un CSV con la parametrización de múltiples subastas (Fechas inicio/fin, Vehículos vinculados, Incremento mínimo de puja, etc.). | Se crean los lotes de subastas correctamente en estado "Programada" o "Borrador" con todas sus variables asociadas. |
| **CSM-02** | Validación de traslape de fechas y vehículos ocupados | Regla de Negocio | Intentar crear mediante CSV una subasta con un vehículo que ya pertenece a otra subasta activa en el mismo rango de fechas. | El sistema detecta la colisión/conflicto de vehículo o fechas y rechaza la creación de ese lote específico con un mensaje explicativo. |
| **CSM-03** | Carga masiva de subastas de Tipo "Conjunto" vs "Individual" | Funcional | Incluir en el CSV subastas de paquete (varios vehículos en 1 sola subasta) y subastas individuales. | El sistema interpreta correctamente la relación 1-a-1 o 1-a-muchos entre subasta y vehículos según el archivo de carga. |