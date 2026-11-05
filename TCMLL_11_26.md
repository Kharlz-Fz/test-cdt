# Suite de Casos de Prueba - MeLoLlevo - Sprint Noviembre

---

## 1. Monitoreo en Línea de Subastas (Módulo Administrador)

| ID | Título del Caso | Tipo | Pasos | Resultado Esperado |
|---|---|---|---|---|
| **MEL-01** | Visualización simultánea (1 y 2 subastas) | Funcional | 1. Ingresar como Administrador a la plataforma.<br>2. Ingresar al módulo de “Monitor en Línea” del panel de subastas.<br>3. Seleccionar 2 subastas activas simultáneas. | El panel se divide correctamente mostrando información dinámica en tiempo real de ambas subastas sin lag ni distorsión visual. Los botones de funcionalidad realizan cada acción entre eliminar vistas de subastas o intercambiar posiciones. |
| **MEL-02** | Límite máximo de subastas en pantalla | Regla de Negocio | Intentar seleccionar una 3ª subasta activa para monitorear. | El sistema impide seleccionar más de 2 subastas simultáneas, impide añadir una nueva bloqueando la acción o reemplaza la primera seleccionada con sustitución estilo cascada. |
| **MEL-03** | Diferenciación de tipos de subasta | Funcional / UI | Monitorear en paralelo una subasta de **Vehículos**, una de **Litigios** y una de **Lotes**. | La interfaz muestra claramente los distintivos/etiquetas del tipo de subasta y la estructura de datos correspondiente (ej. en subasta de lotes se muestra en rojo y con el lote de vehículos y no pujas individuales por auto). |
| **MON-04** | Actualización en tiempo real | Rendimiento / UI | Realizar pujas desde la vista de cliente mientras el administrador monitorea. | El cronómetro, el historial de pujas y la oferta actual se actualizan en tiempo real en la vista de monitoreo sin refrescar la página, respetando el tiempo de consulta a la base de datos para evitar saturación de consultas/seg (cada 5 segundos realiza la consulta). |

---

## 2. Solicitud de Modificación y Aprobación de Remanentes

| ID | Título del Caso | Tipo | Pasos | Resultado Esperado |
|---|---|---|---|---|
| **MAR-01** | Gestión de aprobación de modificaciones de remanentes | Funcional | 1. Ingresar como Administrador a la plataforma.<br>2. Ingresar al módulo de “Carga Masiva de Remanentes” del panel de pagos.<br>3. Ir al apartado de solicitudes de modificación.<br>4. Aprobar la modificación y ver reflejado el cambio en el historial. | Los pagos se marcan como aprobados en la base de datos y se actualiza el estado de la remanente con su cliente correspondiente. |
| **MAP-02** | Gestión de rechazo de modificaciones de remanentes | Funcional | Realizar el proceso previo, pero con un rechazo de la modificación. | Los pagos se marcan como rechazados en el registro dando la oportunidad de realizar los cambios de la remanente correspondiente. |
| **MAP-03** | Revisión correcta de remanentes gestionados | UI | Ir al historial de remanentes del módulo, revisando la lista completa de valores y que coincidan con las respectivas modificaciones. | El sistema procesa la lista completa de remanentes con todos los datos en vista de tabla. |
---

## 3. Carga Masiva de Precios Bases de Vehículos (CSV)

| ID | Título del Caso | Tipo | Pasos | Resultado Esperado |
|---|---|---|---|---|
| **CMP-01** | Actualización masiva de precios base exitosa | Funcional | 1. Ingresar como Administrador a la plataforma.<br>2. Ingresar al módulo de “Mis vehículos” del panel de subastas.<br>3. Ir al apartado de “Carga masiva precios base”.<br>4. Realizar la carga de un CSV con vehículos disponibles y valores numéricos validos (>$0). | Los precios base de los vehículos especificados se actualizan inmediatamente en el inventario y quedan preparados con ese valor para subastas. |
| **CMP-02** | Modificación de precio en subasta activa/finalizada | Regla de Negocio | Incluir en el CSV el ID de un vehículo que pertenece a una subasta **En curso** o **Finalizada**. | El sistema rechaza la actualización para ese vehículo en particular y notifica que no se pueden modificar precios de subastas activas o cerradas. |
| **CMP-03** | Validación de valores numéricos y no negativos | Validación | Incluir valores negativos, letras, caracteres especiales y valores ruido en la columna de “precio base”. | El sistema bloquea el procesamiento de esas filas y despliega alertas de validación de formato numérico. |

---

## 4. Nuevas Plantillas de Correos.

| ID | Título del Caso | Tipo | Pasos | Resultado Esperado |
|---|---|---|---|---|
| **PCO-01** | Renderizado y diseño Responsive | UI / Cross-client | Desplegar el envío de correos (Creación de cuenta, Líder de puja, Cambio de contraseña, etc..) y abrir en el dominio especifico de correo electrónico, aplicando también uso de responsive. | El diseño HTML/CSS se ajusta correctamente, sin imágenes rotas, sin desbordamiento de texto y respetando la nueva línea gráfica. |
| **PCO-02** | Inyección correcta de variables dinámicas | Funcional | Generar correos de por ejemplo “Líder de pujas" y "Cambio de Contraseña". | Los tags dinámicos se reemplazan correctamente (ej. Nombre del cliente, Monto ganador, Placa del vehículo, etc..). |
| **PCO-03** | Entrega de correos masivos / Spam check | Integración | Ejecutar acciones que disparen múltiples correos. | Los correos llegan a la bandeja de entrada (no Spam) y respetan las cabeceras/DKIM/SPF configuradas. Verificar que el uso excesivo no bloquea el servicio de Masive |

---

## 5. Registro de Vehículos Nuevos (Nuevo Formulario)

| ID | Título del Caso | Tipo | Pasos | Resultado Esperado |
|---|---|---|---|---|
| **RVH-01** | Registro completo de vehículo exitoso | Funcional / UX | 1. Ingresar como Administrador a la plataforma.<br>2. Ingresar al módulo de “Registrar vehiculos” del panel de subastas.<br>3. Ir al apartado de “Registro de vehículo individual”.<br>4. Llenar el formulario con un vehículo no registrado en la plataforma | El vehículo se crea exitosamente en estado "Disponible" y el flujo resulta más fluido, rápido e intuitivo con respecto a la versión anterior. |
| **RVH-02** | Validación de campos obligatorios y máscaras | Validación | Intentar guardar el formulario dejando campos requeridos vacíos (ej. Marca, Empresa, Modelo, Valor Base, etc..). | El formulario resalta los campos faltantes con alertas de error descriptivas y bloquea el envío. |
| **RVH-03** | Persistencia de datos al navegar/descartar | UX | Llenar parcialmente el formulario y refrescar o salir de la pantalla. | El sistema solicita confirmación de salida para evitar pérdida accidental de datos o bien deja en cache las casillas ya llenadas previamente de forma que al ingresar queden los datos ya ingresados. |

---

## 6. Registro / Repositorio Fotográfico

| ID | Título del Caso | Tipo | Pasos | Resultado Esperado |
|---|---|---|---|---|
| **RRF-01** | Carga y almacenamiento sin vehículo previo | Funcional | 1. Ingresar como Administrador a la plataforma.<br>2. Ingresar al módulo de “Registrar vehículos” del panel de subastas.<br>3. Ir al apartado de “Repositorio fotográfico”.<br>4. Ingresar una placa no existente en la ventana generada en “Nueva inspección”. | Las fotos se cargan correctamente en el repositorio central y quedan en estado "Sin asignar" / "Disponibles". |
| **RRF-02** | Edición y sustitución directa de imágenes | Funcional | Alterar/reemplazar imágenes de un vehículo directamente desde el repositorio general. | Las imágenes se actualizan inmediatamente en el detalle del vehículo sin necesidad de entrar a la ficha de edición individual del vehículo. |
| **RRF-03** | Restricciones de formato, peso y resolución | Validación | Subir archivos con formatos no permitidos (`.pdf`, `.csv`) o imágenes que superen el tamaño máximo permitido (ej. > 20MB). | El sistema rechaza los archivos no válidos y muestra las restricciones de carga. |
| **RRF-04** | Vinculación posterior de imágenes a un vehículo | Funcional | Asignar un lote de fotos guardadas en el repositorio a un vehículo recién creado. | El vehículo adopta las imágenes seleccionadas y su galería principal se actualiza adecuadamente. |
| **RRF-05** | Visualización y carga correcta desde cámara de dispositivo | Funcional | Cargar fotos desde la cámara desde el dispositivo verificando la correcta compatibilidad tanto del formato generado por la cámara como la correcta visualización para la carga de imágenes. | El vehículo toma las imágenes capturadas y su galería se actualiza adecuadamente luego de la carga. |

---

## 7. Subastas Masivas (Creación mediante CSV)

| ID | Título del Caso | Tipo | Pasos | Resultado Esperado |
|---|---|---|---|---|
| **CSM-01** | Creación masiva de subastas desde CSV | Funcional | 1. Ingresar como Administrador a la plataforma.<br>2. Ingresar al módulo de “Subastas en línea” del panel de subastas.<br>3. Seleccionar la opción de “Programación masiva”.<br>4. Llenar el formulario junto con la carga del CSV siguiendo el formato solicitado y crear el lote de subastas. | Se crean los lotes de subastas correctamente en estado "Programada" con todas sus variables asociadas. |
| **CSM-02** | Validación de traslape de fechas y vehículos ocupados | Regla de Negocio | Intentar crear mediante CSV una subasta con un vehículo que ya pertenece a otra subasta activa, subastas con vehículos similares y vehículos en estados no disponibles o de subastas ya finalizadas con ganador asignado. | El sistema detecta la colisión/conflicto de vehículo o fechas y rechaza la creación de ese lote específico con un mensaje explicativo. |
| **CSM-03** | Carga masiva de subastas de Tipo "Conjunto" vs "Individual" | Funcional | Incluir en el CSV subastas de lotes y subastas individuales. | El sistema interpreta correctamente la relación entre subastas y vehículos según el archivo de carga. |
