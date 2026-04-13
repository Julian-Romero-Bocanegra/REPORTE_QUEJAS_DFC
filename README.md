# REPORTE_QUEJAS_DFC
Defensoría del Consumidor Financiero (PG ABOGADOS Y ASOCIADOS). Este bufete de abogados se destaca como una Firma Especializada en la Protección del Consumidor Financiero, enfocándose en salvaguardar los derechos de los usuarios. Su objetivo principal es operar de manera: autónoma, independiente y objetiva como representante y mediador ante las instituciones supervisadas, para atender de manera efectiva, justa y oportuna las quejas, solicitudes y reclamos (PQR) de los consumidores financieros. 

La firma tiene un ámbito de actuación que incluye la gestión de controversias relacionadas con todos los productos y servicios de las entidades financieras asociadas, abarcando bancos, compañías de seguros, administradoras de pensiones y cesantías, fideicomisos y entidades cooperativas. Su público son los consumidores financieros, ya sean individuos o empresas, que utilizan los servicios de estas entidades. Las funciones principales comprenden recibir, gestionar, analizar legal y fácticamente, y responder formalmente a cada QSR. La firma opera como un canal de comunicación formal y riguroso entre el consumidor y la institución, emitiendo pronunciamientos fundamentados y cumpliendo con los plazos legales requeridos. También desempeña un papel como portavoz, brindando recomendaciones a las entidades para mejorar sus servicios y prácticas.

## Diseño del Proceso ETL
El proceso ETL (Extracción, Transformación y Carga) diseñado para PG Abogados y Asociados se estructura de la siguiente manera:
### 1). Extracción
- Fuentes de datos: Archivos Excel existentes que contienen registros de quejas, consultas y reclamos (PQR).
- Método de extracción: Conexión directa de Power BI a los archivos Excel mediante conectores nativos.
### 2). Transformación
- Limpieza de datos: Eliminación de duplicados, estandarización de formatos de fecha, corrección de inconsistencias en nombres de instituciones financieras
- Clasificación y categorización: Aplicación de reglas de negocio para clasificar automáticamente los casos por tipo (queja, consulta, reclamo), estado y prioridad
- Cálculo de métricas: Generación automática de porcentajes, tasas de resolución, tiempos promedio de respuesta y otros indicadores legales clave
- Enriquecimiento de datos: Adición de dimensiones temporales (mes, trimestre, año) para facilitar el análisis por períodos
### 3). Carga
- Método: Carga incremental para optimizar el rendimiento
- Actualización: Programación de actualizaciones automáticas según la frecuencia requerida (diaria/semanal)
Modelos de Datos (Estructura General del Modelo)
El modelo sigue una arquitectura tipo estrella con:
### Tabla Central:
- RADICADOS: Contiene todas las métricas y medidas del proceso

### Tablas de Dimensiones Conectadas:
- PETICIONARIO: Dimensión de clientes/peticionarios
- VALIDACION DE PRODUCTO: Dimensión de productos financieros
- VALIDACION DE MOTIVO GENERAL: Dimensión de categorías de quejas
- VALIDACION DE MOTIVO ESPECÍFICO: Dimensión de subcategorías detalladas
- CAUSA FIN DE TRAMITE: Dimensión de motivos de cierre
- Calendario DAX: Dimensión de tiempo para análisis por fecha
- 
<img width="888" height="628" alt="image" src="https://github.com/user-attachments/assets/369e28fa-a112-4769-bfe1-77233d4e9ecd" />
### Relaciones del Modelo
1. RADICADOS (Hechos) → PETICIONARIO (Dimensión) \
- Campo de relación: `ID_Peticionario` \
- Descripción: Relaciona cada radicado con la información del peticionario que presenta la queja/reclamo\
- Cardinalidad: Uno a muchos (un peticionario puede tener múltiples radicados)\
2. RADICADOS (Hechos) → VALIDACION DE PRODUCTO (Dimensión)\
- Campo de relación: `ID_Producto`\
- Descripción: Asocia cada radicado con el tipo de producto financiero involucrado\
- Cardinalidad: Uno a muchos (un producto puede estar en múltiples radicados)\
3. RADICADOS (Hechos) → VALIDACION DE MOTIVO GENERAL (Dimensión)\
- Campo de relación: `ID_Motivo_General`\
- Descripción: Clasifica los radicados por categorías generales de motivos de queja\
- Cardinalidad: Uno a muchos (un motivo general puede aplicarse a múltiples radicados)\
4. RADICADOS (Hechos) → VALIDACION DE MOTIVO ESPECÍFICO (Dimensión)\
- Campo de relación: `ID_Motivo_Específico`\
- Descripción: Especifica el motivo detallado de cada radicado dentro de la categoría general \
- Cardinalidad: Uno a muchos (un motivo específico puede repetirse en varios radicados)\
5. RADICADOS (Hechos) → CAUSA FIN DE TRAMITE (Dimensión)\
- Campo de relación: `ID_Causa_Fin`\
- Descripción: Indica la causa o motivo de cierre/finalización del trámite del radicado\
- Cardinalidad: Uno a muchos (una causa de fin puede aplicarse a múltiples radicados)\
6. RADICADOS (Hechos) → Calendario DAX (Dimensión) \
- Campo de relación: `FechaRecibo` → `Date`\
- Descripción: Conecta cada radicado con la dimensión de tiempo para análisis temporal\
- Cardinalidad: Uno a muchos (una fecha puede tener múltiples radicados)\

### Análisis
El examen del proceso actual muestra una fuerte dependencia de herramientas que no son específicas para la gestión de datos, como Excel y Word, en una labor que es principalmente analítica y en constante evolución. Este enfoque presenta tres problemas cruciales: Inconsistencia en los Datos: Los cálculos manuales de porcentajes e indicadores son naturalmente susceptibles a fallos. Un leve descuido al teclear o un error en una fórmula después de una reclasificación puede poner en riesgo la fiabilidad de todo el informe, causando desconfianza en las entidades financieras y dificultando la toma de decisiones internas acertadas. Elevado Costo de Oportunidad: El tiempo que el equipo legal dedica a trabajos automáticos de recopilación y formateo es una pérdida de su capital intelectual. Este esfuerzo podría ser utilizado para identificar las causas fundamentales de las quejas, elaborar estrategias de prevención o manejar casos complicados, pero se ve desplazado por tareas administrativas, disminuyendo el valor estratégico que la firma puede aportar. Rigidez e Ineficiencia en las Operaciones: La estructura actual del proceso carece de flexibilidad. Cada modificación, como reclasificar un caso o incorporar una nueva métrica, provoca una serie de trabajos manuales para rehacer tablas, gráficos y textos. Esto no solo retrasa la entrega, sino que disuade la mejora continua y el análisis exhaustivo, ya que cada exploración de datos se convierte en una tarea pesada. La sugerencia de usar Power BI se relaciona directamente con la superación de estas limitaciones. Siendo una herramienta de inteligencia empresarial, su principal ventaja es la automatización, integración y visualización. Al desarrollar un modelo de datos unificado y paneles dinámicos, el proceso se transforma de reactivo y manual a proactivo y automático. El análisis deja de ser una carga al final del periodo y se convierte en una capacidad en tiempo real, donde los abogados pueden interactuar con los datos y recibir respuestas inmediatas.
