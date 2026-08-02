# outer-joins-ministore — Análisis de Inventario y Ventas

Repositorio analítico de **MiniStore** enfocado en la auditoría de calidad de datos y detección de registros no coincidentes mediante el uso de Outer JOINs en SQL.

##  Estructura del Repositorio
```text
outer-joins-ministore/
├── README.md
├── schema.sql 
└── soluciones.sql
```

## Preguntas Teóricas y Análisis de Resultados

```text
1. ¿Por qué usaste LEFT JOIN para la Consulta 1 y no INNER JOIN? ¿Qué se perdería si usaras INNER JOIN?
Se utilizó un LEFT JOIN porque la tabla principal de análisis es productos (ubicada a la izquierda). El objetivo de negocio era identificar aquellos artículos que nunca registraron ventas.

Si se hubiera usado un INNER JOIN, se habría descartado automáticamente los productos 108 (Hub USB-C 7p) y 109 (Parlante Bluetooth), ya que no tienen transacciones asociadas en la tabla ventas. El INNER JOIN solo demuestra coincidencias exactas, ocultando por completo la información de inventario sin rotación.

2. ¿Por qué usaste RIGHT JOIN para la Consulta 2? ¿Qué tabla está a la izquierda y cuál a la derecha en tu consulta?
Se utilizó un RIGHT JOIN para priorizar la tabla de la derecha (ventas), asegurando que se conserven la totalidad de las transacciones registradas.

En la estructura implementada (FROM productos p RIGHT JOIN ventas v), la tabla productos se encuentra a la izquierda y la tabla ventas a la derecha. Esto permite detectar anomalías operativas, como la venta con producto_id = 999, la cual no existe en el catálogo oficial pero figura en el historial de transacciones.

3. ¿Qué representan los valores NULL en cada resultado?
Los valores NULL indican ausencia de correspondencia o relación lógica en la tabla cruzada:

En la Consulta 1 (LEFT JOIN): Que el campo venta_id aparezca como NULL (por ejemplo, en los productos 108 y 109) significa que esos artículos del catálogo nunca fueron vendidos.

En la Consulta 2 (RIGHT JOIN): Que el campo producto_id o nombre de productos aparezca como NULL (en la venta 10) significa que la transacción apunta a un producto inexistente en el catálogo, evidenciando un error de carga o registro en el sistema.

4. ¿Cuándo usarías FULL OUTER JOIN en un caso real de negocio?
El FULL OUTER JOIN es una herramienta clave en procesos de auditoría integral y conciliación de bases de datos. Se utilizaría, por ejemplo, al migrar información entre sistemas o al integrar bases de datos de plataformas con sistemas de inventario físicos independientes. Permite visualizar simultáneamente y en un solo reporte tanto las discrepancias como los errores de transaccionalidad, sin perder ninguna observación de ambos extremos.

```
