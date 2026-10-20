# Objetivo 3: Armado de Plantilla Base de Reporte

## Diseño general

Diseñé una plantilla titulada “Informe de Evaluación de Seguridad” para presentar los resultados de una evaluación de seguridad. La portada usa el color verde #33d17a, incluye el nombre del proyecto, la fecha y una clasificación de confidencialidad. El documento también tiene índice, encabezado y pie de página para mantener el mismo formato en todas sus secciones.

## Estructura de la plantilla

### 1. Resumen Ejecutivo y Ficha Técnica

Presenta una descripción general de la evaluación para que cualquier lector pueda entender su objetivo. La ficha técnica permite registrar el tipo de prueba, los estándares utilizados, el período, el entorno y la clasificación del documento.

### 2. Metodología y Estándares Aplicados

Registra las técnicas que se utilizaron, el alcance de las pruebas y los marcos de referencia aplicados. También indica la versión de CVSS usada para puntuar los hallazgos y la escala de severidad correspondiente.

### 3. Métricas y Distribución de Hallazgos

Incluye un gráfico de barras agrupado por puntaje CVSS para comparar la severidad de los hallazgos y un gráfico de torta agrupado por estado del ticket para mostrar el avance de su seguimiento.

### 4. Alcance Técnico y Componentes Evaluados

Describe qué sistemas, aplicaciones y controles se revisaron. La lista se completa de acuerdo con el alcance de cada evaluación; por ejemplo, autenticación y sesiones, control de acceso, validación de entradas, comunicaciones, criptografía y manejo de errores.

### 5. Hallazgos Identificados

Cada hallazgo se presenta por separado con los siguientes campos:

Resumen ejecutivo: (nombre del hallazgo, activo afectado e impacto principal)

Detalles técnicos: (descripción, componente afectado y condiciones necesarias)

Métricas CVSS: (versión, puntaje, severidad y vector)

Prueba de concepto:

Condiciones previas: (cuenta, rol o configuración necesaria)

Pasos para reproducir:

1. (primer paso)
2. (segundo paso)
3. (paso final)

Resultado esperado: (qué debería ocurrir)

Resultado observado: (qué ocurrió durante la prueba)

Evidencia: (capturas o datos que demuestran el hallazgo)

Recomendaciones de remediación: (acciones para corregir la causa y validar la solución)

Como ejemplo de presentación, cargué el hallazgo “Se filtran las contraseñas”, con una descripción, un puntaje CVSS y un campo para relacionarlo con un control ASVS cuando corresponde.

### 6. Conclusiones y Plan de Remediación

Resume cómo priorizar los hallazgos, considerando su severidad y el contexto del sistema. Después de aplicar las correcciones, recomienda realizar una reevaluación para verificar que los problemas se hayan mitigado.

## Valor para quien recibe el ticket

La plantilla organiza la información para que quien recibe un hallazgo pueda entender qué ocurre, a qué sistema afecta y cuál es su posible impacto. Los detalles técnicos y los pasos de la prueba ayudan a reproducir el problema. El puntaje CVSS facilita comparar y priorizar los hallazgos, mientras que las recomendaciones indican qué corregir y cómo comprobarlo. Así, el equipo puede pasar del reporte a una acción concreta y hacer seguimiento de la solución.
