# Actividad 3 — Infraestructura Virtual con Databricks y Big Data

**Estudiante:** Juan Felipe Valencia Renteria
**Asignatura:** Big Data
**Entorno:** Databricks Community Edition — Serverless Compute

---

## Descripcion General

Esta actividad despliega un conjunto de datos real sobre una infraestructura virtual en Databricks Community Edition, cubriendo el diseño del esquema de almacenamiento, la configuracion del entorno, la ingesta de datos, la validacion con Spark y SQL, y el analisis comparativo de ambos paradigmas.

---

## Dataset

**Archivo:** `users_connectatel.csv`
**Descripcion:** Registro de usuarios de la empresa ficticia de telecomunicaciones **Connectatel**.

| Atributo | Detalle |
|---|---|
| Total de registros | 4.000 filas |
| Total de campos | 15 columnas |
| Tipos de datos | Enteros, cadenas de texto, marcas de tiempo y decimales |
| Valores nulos | Campo `city` con valores ausentes / `churn_date` vacio para usuarios activos |
| Planes disponibles | Basico y Premium |
| Contexto de negocio | Analisis de abandono de servicio (churn) en telecomunicaciones |

---

## Infraestructura Utilizada

| Componente | Descripcion |
|---|---|
| Plataforma | Databricks Community Edition |
| Tipo de computo | Serverless (aprovisionamiento automatico) |
| Almacenamiento | Unity Catalog Volume |
| Catalogo | workspace |
| Schema | default |
| Volume | actividad3 |
| Formato de tabla | Delta Lake |
| Tabla registrada | workspace.default.connectatel_users |

---

## Estructura de la Solucion

El desarrollo completo se encuentra en el notebook `Valencia_Renteria_Juan_Felipe_Actividad_2.ipynb`, organizado en cinco secciones:

---

### Seccion 1 — Diseño del Esquema

Define la estructura formal del dataset antes de cualquier carga de datos.

- **Diccionario de datos:** tabla con los 15 campos del dataset, incluyendo tipo de dato, nulabilidad, descripcion y llave primaria (`user_id`).
- **Esquema PySpark:** definicion formal mediante `StructType` y `StructField` con los tipos correctos para cada campo.
- **Diagrama de entidad:** representacion visual de la entidad `USERS` y sus atributos mediante Mermaid.
- **DDL Spark SQL:** sentencia `CREATE TABLE` equivalente con comentarios por campo.

---

### Seccion 2 — Configuracion de Infraestructura en Databricks CE

Evidencia el entorno de ejecucion configurado.

- **Entorno de computo:** Databricks Serverless — sin configuracion manual de cluster.
- **Versiones del entorno:** version de Apache Spark y version de Python impresas desde el notebook.
- **Configuracion Spark:** parametros del entorno obtenidos via consulta SQL directa al contexto de sesion.
- **Almacenamiento:** estructura del Unity Catalog Volume con ruta, nombre y tamaño del archivo cargado.
- **Runtime y configuracón del cluster - Serverless**
  <img width="1917" height="626" alt="Runtime y configuracipon del cluster - Serverless" src="https://github.com/user-attachments/assets/ff4f1bec-c659-4b37-936a-40941b4e22eb" />

- **Volumen creado correctamente**
  <img width="1915" height="944" alt="Volumen creado correctamente" src="https://github.com/user-attachments/assets/1c5b6363-bf8c-4d1e-bde2-abb27dda08f5" />

- **Detalles del volumen**
  <img width="1918" height="720" alt="Detalles del volumen" src="https://github.com/user-attachments/assets/fdb28a13-a2bc-4fab-8a4c-74e3ed3dc2b3" />


---

### Seccion 3 — Ingesta de Datos y Creacion de Tabla

Cubre el proceso completo desde la lectura del archivo hasta la persistencia como tabla Delta.

- **Metodo de carga:** Opcion B — carga manual del archivo CSV desde la maquina local hacia el Unity Catalog Volume `workspace.default.actividad3` a traves de la interfaz grafica de Databricks.
- **Lectura del archivo:** lectura del CSV desde el Volume con deteccion automatica de esquema y tratamiento de valores nulos.
- **Verificacion de lectura:** conteo total de filas, numero de columnas y muestra de los primeros registros.
- **Persistencia:** escritura de la tabla en formato Delta con soporte para sobreescritura de esquema.
- **Confirmacion:** descripcion de la tabla registrada en el catalogo mediante `DESCRIBE TABLE`.

---

### Seccion 4 — Validaciones en Spark y SQL

Cada validacion se ejecuta en su equivalente PySpark y SQL para comparar resultados y sintaxis.

| Bloque | Validacion | Proposito |
|---|---|---|
| 1 | Metadatos del esquema | Verificar tipos, nulabilidad y estructura registrada de la tabla |
| 2 | Descripcion estadistica | Conocer distribucion, minimos, maximos y promedios de campos numericos |
| 3 | SELECT con filtro | Recuperar subconjuntos especificos de datos aplicando condiciones multiples |
| 4 | GROUP BY por plan | Comparar metricas clave entre los planes Basico y Premium |
| 5 | GROUP BY por ciudad | Identificar las ciudades con mayor concentracion de usuarios |
| 6 | Clasificacion por churn | Cuantificar usuarios activos frente a usuarios dados de baja del servicio |

---

### Seccion 5 — Ventajas y Desventajas: SQL vs Spark

Analisis comparativo entre los dos paradigmas evaluados en cinco criterios:

| Criterio | SQL | Spark (PySpark) |
|---|---|---|
| Facilidad de uso | Alta — sintaxis declarativa accesible | Media — requiere conocimiento de Python y la API |
| Expresividad | Limitada para logica compleja | Alta — soporta logica arbitraria y UDFs |
| Escalabilidad | Depende del motor subyacente | Nativa — disenada para procesamiento distribuido |
| Integracion con BI | Excelente via JDBC/ODBC | Limitada — requiere exposicion previa como tabla |
| Pipelines y ML | Inadecuado para pipelines complejos | Ideal — integra con MLlib y frameworks externos |

**Conclusion:** SQL y PySpark son complementarios. SQL es ideal para exploracion rapida y reportes; PySpark es indispensable para transformaciones complejas, ETL a escala y machine learning.

---

## Archivos del Repositorio

| Archivo | Descripcion |
|---|---|
| `Valencia_Renteria_Juan_Felipe_Actividad_2.ipynb` | Notebook principal con toda la solucion |
| `users_connectatel.csv` | Dataset de usuarios de Connectatel |
| `README.md` | Documentacion de la estructura de la solucion |

---

## Pasos para Reproducir

1. Crear una cuenta en Databricks Community Edition.
2. Crear un Volume en Unity Catalog: `workspace > default > actividad3`.
3. Cargar el archivo `users_connectatel.csv` al Volume desde la interfaz grafica.
4. Importar el notebook `.ipynb` al workspace de Databricks.
5. Conectar el notebook al computo Serverless.
6. Ejecutar todas las celdas en orden secuencial de arriba hacia abajo.
