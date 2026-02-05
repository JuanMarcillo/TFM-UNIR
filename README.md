# **TFM - Máster en Visual Analytics and Big Data**  
## Análisis geoespacial de la relación entre infraestructura petrolera, acceso a salud y población afroecuatoriana en Ecuador (2020-2022).

[![Python](https://img.shields.io/badge/Python-3.10+-blue.svg)](https://www.python.org/)
[![Streamlit](https://img.shields.io/badge/Streamlit-1.28+-red.svg)](https://streamlit.io/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-14+-336791.svg)](https://www.postgresql.org/)
[![PostGIS](https://img.shields.io/badge/PostGIS-3.4+-4CAF50.svg)](https://postgis.net/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

---

## 📋 Resumen Ejecutivo

Este TFM investiga la **paradoja extractivista** en Ecuador: la contradicción entre la generación de riqueza a través de la explotación de recursos naturales y el acceso desigual a servicios públicos en las zonas de extracción.

### Hallazgo Principal

Las parroquias con actividad petrolera presentan **33% menos acceso a servicios de salud** en comparación con parroquias sin actividad petrolera:

| Métrica | Sin Petróleo | Con Petróleo | Diferencia |
|---------|-------------|--------------|------------|
| Establecimientos/10k hab | 8.88 | 5.87 | **-33%** |
| Número de parroquias | ~1,186 | ~50 | 4.3% |

### Hallazgo Secundario

Las comunidades afroecuatorianas con alta población (>5%) **no están significativamente expuestas** a actividad petrolera, concentrándose principalmente en la provincia de Esmeraldas, fuera de los bloques petroleros.

---

## 🏗️ Arquitectura del Proyecto

```
├── data/
│   ├── raw/                    # Datos originales (CONALI, INEC, MSP, MAATE)
│   ├── processed/              # Datos procesados (CSV intermedios)
│   └── geo/                    # Archivos geoespaciales (GeoJSON, shapefiles)
├── notebooks/                  # Pipeline ETL y análisis (7 notebooks)
│   ├── 01_exploracion_inicial.ipynb
│   ├── 02_etl_limites.ipynb
│   ├── 03_etl_censo_etnia_v2.ipynb
│   ├── 04_etl_salud_ras_v2.ipynb
│   ├── 05_etl_petroleo.ipynb
│   ├── 06_analisis_espacial.ipynb
│   └── 07_carga_postgis.ipynb
├── dashboard/                  # Aplicación Streamlit interactiva
│   ├── app.py                  # Punto de entrada principal
│   ├── config.py               # Configuración y constantes
│   ├── pages/                  # Páginas del dashboard
│   │   ├── 1_Overview.py
│   │   ├── 3_Analisis_Espacial.py
│   │   └── 4_Explorador_Datos.py
│   └── utils/                  # Módulos utilitarios
│       ├── db_connection.py    # Conector PostgreSQL/PostGIS
│       └── queries.py          # Queries SQL reutilizables
├── docs/                       # Documentación adicional
├── requirements.txt            # Dependencias del proyecto
└── README.md                   # Este archivo
```

---

## 📊 Fuentes de Datos

| Fuente | Descripción | Año | Registros |
|--------|-------------|-----|-----------|
| **CONALI** | Límites administrativos parroquiales | 2022 | 1,236 parroquias |
| **INEC** | Censo de población (etnia, demografía) | 2022 | ~18M habitantes |
| **MSP** | Registro de establecimientos de salud (RAS) | 2020 | ~4,500 establecimientos |
| **MAATE** | Infraestructura petrolera y contaminación | 2020-2022 | 6,287 pozos, 7,850 sitios |

---

## 🔧 Requisitos Técnicos

### Software

- **Python** >= 3.10
- **PostgreSQL** >= 14 con extensión PostGIS >= 3.4
- **Docker** (opcional, para PostgreSQL)

### Dependencias Principales

```
pandas >= 2.0.0          # Manipulación de datos
geopandas >= 0.14.0      # Análisis geoespacial
streamlit >= 1.28.0      # Dashboard interactivo
plotly >= 5.14.0         # Visualizaciones interactivas
scikit-learn >= 1.3.0    # Machine learning (clustering)
psycopg2-binary >= 2.9.0 # Conexión PostgreSQL
SQLAlchemy >= 2.0.0      # ORM y queries
GeoAlchemy2 >= 0.14.0    # Soporte espacial PostgreSQL
```

---

## 🚀 Instalación y Configuración

### 1. Clonar el repositorio

```bash
git clone https://github.com/JuanMarcillo/TFM-UNIR.git
cd TFM-UNIR
```

### 2. Crear entorno virtual

```bash
python -m venv venv
source venv/bin/activate  # Linux/Mac
# o
venv\Scripts\activate     # Windows
```

### 3. Instalar dependencias

```bash
pip install -r requirements.txt
```

### 4. Configurar base de datos

```bash
# Opción A: Docker (recomendado)
docker run -d \
  --name postgis-tfm \
  -e POSTGRES_PASSWORD=postgres \
  -e POSTGRES_DB=prototipo_salud \
  -p 5434:5432 \
  postgis/postgis:14-3.4

# Opción B: PostgreSQL local
# Crear base de datos 'prototipo_salud' y habilitar extensión PostGIS
```

### 5. Configurar conexión

Editar `dashboard/config.py` o usar variables de entorno:

```python
DB_CONFIG = {
    'host': 'localhost',
    'port': 5434,
    'database': 'prototipo_salud',
    'user': 'postgres',
    'password': 'postgres'
}
```

```bash
# O variables de entorno
export DB_HOST=localhost
export DB_PORT=5434
export DB_NAME=prototipo_salud
export DB_USER=postgres
export DB_PASSWORD=postgres
```

### 6. Ejecutar pipeline ETL

```bash
# Ejecutar notebooks en orden
jupyter notebook notebooks/

# O con nbconvert (headless)
jupyter nbconvert --to notebook --execute notebooks/01_exploracion_inicial.ipynb
# ... repetir para cada notebook
```

### 7. Verificar conexión

```bash
cd dashboard
python test_connection.py
```

---

## 🖥️ Uso del Dashboard

### Iniciar aplicación

```bash
cd dashboard
streamlit run app.py
```

La aplicación estará disponible en: `http://localhost:8501`

### Páginas disponibles

| Página | Descripción |
|--------|-------------|
| **Inicio** | Métricas generales, hallazgos principales y metodología |
| **Overview** | Análisis exploratorio: scatter plots, rankings, comparativas |
| **Análisis Espacial** | Mapas interactivos (Plotly Mapbox) y clustering K-Means |
| **Explorador de Datos** | Filtros dinámicos, tabla de datos y descarga CSV |

---

## 📈 Metodología

### 1. Proceso ETL (Extract, Transform, Load)

| Notebook | Proceso | Salida |
|----------|---------|--------|
| 01 | Exploración inicial y setup | Configuración del entorno |
| 02 | ETL límites parroquiales | `parroquias_base.csv` |
| 03 | ETL censo etnia | `parroquias_con_etnia.csv` |
| 04 | ETL establecimientos de salud | `parroquias_con_salud.csv` |
| 05 | ETL infraestructura petrolera | `parroquias_con_petroleo.csv` |
| 06 | Análisis espacial y clustering | `parroquias_con_clusters.csv` |
| 07 | Carga a PostGIS | Base de datos PostgreSQL |

### 2. Técnicas Analíticas

- **Spatial Joins**: Intersección espacial entre geometrías (PostGIS)
- **Clustering (K-Means, k=4)**: Segmentación de parroquias por características
- **Análisis de Correlación**: Pearson/Spearman entre variables
- **Autocorrelación Espacial**: Moran's I (opcional con PySAL)
- **Estadística Descriptiva**: Pruebas no paramétricas (Mann-Whitney U)

### 3. Variables Principales

```
Variables Dependientes:
  - establecimientos_por_10k_hab  # Acceso a salud

Variables Independientes:
  - num_infraestructura_petrolera # Pozos + contaminación
  - num_pozos                     # Actividad extractiva directa
  - num_sitios_contaminados       # Pasivos ambientales
  - pct_poblacion_afro            # Composición étnica
  - densidad_petroleo_km2         # Intensidad extractiva
```

---

## 🗺️ Resultados del Clustering

El análisis de clustering K-Means (k=4) identificó los siguientes grupos de parroquias:

| Cluster | Característica | N | Infraestructura Promedio | Salud Promedio | % Afro Promedio |
|---------|----------------|---|-------------------------|----------------|-----------------|
| **0** | Baja actividad petrolera | ~700 | Baja | Media | Bajo |
| **1** | **Alta actividad petrolera (Amazonía)** | ~50 | **Alta** | **Baja** | Bajo |
| **2** | Sin petróleo, mejor salud | ~350 | Nula | **Alta** | Bajo |
| **3** | Alta población afro (Esmeraldas) | ~100 | Baja | Media | **Alto** |

---

## 📚 Estructura de la Base de Datos

### Tabla principal: `parroquias`

| Campo | Tipo | Descripción |
|-------|------|-------------|
| `codigo_dpa` | VARCHAR(6) | Código DPA único |
| `nombre_parroquia` | VARCHAR | Nombre de la parroquia |
| `nombre_canton` | VARCHAR | Cantón al que pertenece |
| `nombre_provincia` | VARCHAR | Provincia |
| `poblacion_total` | INTEGER | Población total (INEC 2022) |
| `pct_poblacion_afro` | FLOAT | % población afroecuatoriana |
| `num_pozos` | INTEGER | Número de pozos petroleros |
| `num_sitios_contaminados` | INTEGER | Sitios de contaminación |
| `num_infraestructura_petrolera` | INTEGER | Suma de pozos + contaminación |
| `establecimientos_por_10k_hab` | FLOAT | Establecimientos de salud per cápita |
| `densidad_petroleo_km2` | FLOAT | Infraestructura por km² |
| `tiene_petroleo` | BOOLEAN | Flag binario |
| `cluster_kmeans` | INTEGER | Cluster asignado (0-3) |
| `geometry` | GEOMETRY | Geometría espacial (PostGIS) |

---

## 🎯 Limitaciones y Consideraciones

1. **Datos de salud (RAS 2020)**: Pueden no incluir establecimientos privados o informales
2. **Temporalidad**: Los datos de fuentes diferentes tienen años distintos (2020-2022)
3. **Escala de análisis**: Nivel parroquial puede ocultar heterogeneidades intra-parroquiales
4. **Causalidad**: El análisis muestra asociaciones, no relaciones causales directas
5. **Población afroecuatoriana**: Autoidentificación en censo puede subestimar

---

## 🤝 Contribuciones

Este proyecto fue desarrollado como Trabajo Fin de Máster (TFM) para el programa de **Máster en Visual Analytics and Big Data**.

---

## 📄 Licencia

Este proyecto está licenciado bajo la Licencia MIT

---

## 📧 Contacto

Para consultas académicas o preguntas sobre la metodología:

- **Autor**: Juan Franciso Marcillo & Arturo Córdova Ortega
- **Programa**: Máster en Visual Analytics and Big Data
- **Año**: 2026

---

## 🙏 Agradecimientos

- **CONALI** por los datos de límites administrativos
- **INEC** por el Censo de Población y Vivienda 2022
- **MSP** por el Registro de Establecimientos de Salud
- **MAATE** por los datos de infraestructura petrolera

---

<div align="center">

**[⬆ Volver al inicio](#paradoja-extractivista-en-ecuador)**

</div>
