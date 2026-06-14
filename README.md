<div align="center">
<h1>Exploratory Data Analysis: EDA</h1>
<h3>COVID-19 Mortality Analysis (January - February 2020)</h3>
</div>

[DATASET URL: covid_19_deaths.csv](https://docs.google.com/spreadsheets/d/e/2PACX-1vQU0SIALScXx8VXDX7yKNKWWPKE1YjFlWc6VTEVSN45CklWWf-uWmprQIyLtoPDA18tX9cFDr-aQ9S6/pubhtml)

## 📊 Descripción del Proyecto

Este proyecto realiza un análisis exploratorio de datos (EDA) sobre un dataset de casos de COVID-19, con enfoque en identificar factores de riesgo asociados a la mortalidad. El análisis incluye limpieza de datos, imputación de valores faltantes, ingeniería de características, visualizaciones y generación de conclusiones clave.

## 🛠️ Tecnologías Utilizadas

| Tecnología                                                                                      | Propósito                         |
| ----------------------------------------------------------------------------------------------- | --------------------------------- |
| <img src="https://img.shields.io/badge/Python-3.11-3776AB?logo=python&logoColor=white">         | Lenguaje principal                |
| <img src="https://img.shields.io/badge/Pandas-2.0-150458?logo=pandas&logoColor=white">          | Manipulación y análisis de datos  |
| <img src="https://img.shields.io/badge/NumPy-1.24-013243?logo=numpy&logoColor=white">           | Operaciones numéricas             |
| <img src="https://img.shields.io/badge/Matplotlib-3.7-11557C?logo=matplotlib&logoColor=white">  | Visualizaciones estáticas         |
| <img src="https://img.shields.io/badge/Seaborn-0.12-388E8E?logo=seaborn&logoColor=white">       | Visualizaciones estadísticas      |
| <img src="https://img.shields.io/badge/SciPy-1.10-8CAAE6?logo=scipy&logoColor=white">           | Pruebas estadísticas (t-test)     |
| <img src="https://img.shields.io/badge/Jupyter-F37626?logo=jupyter&logoColor=white">            | Entorno de desarrollo interactivo |
| <img src="https://img.shields.io/badge/Git-F05032?logo=git&logoColor=white">                    | Control de versiones              |
| <img src="https://img.shields.io/badge/VS_Code-007ACC?logo=visual-studio-code&logoColor=white"> | Editor de código                  |
| <img src="https://img.shields.io/badge/GitHub-181717?logo=github&logoColor=white">              | Repositorio remoto                |

## 📁 Estructura del Proyecto

```planetext
project-3-EDA-abel/
├── .venv/                     # Entorno virtual de Python
├── .gitignore                 # Archivos y carpetas ignorados por Git
├── covid_19_deaths.csv        # Dataset original
├── covid.ipynb                # Notebook principal con todo el análisis
├── README.md                  # Documentación del proyecto
└── requirements.txt           # Dependencias del proyecto
```

## 🧹 Proceso de Limpieza de Datos

### 1. Corrección de Encabezados

- Los encabezados reales estaban ubicados en la fila 2 (índice 2) del CSV
- Se utilizó `skiprows=1` para cargar correctamente los nombres de columnas

### 2. Eliminación de Columnas No Relevantes

Se eliminaron columnas con baja relevancia para el análisis:

- `id`, `summary`, `case_in_country`
- `If_onset_approximated`, `traveler`
- `visiting Wuhan`, `from Wuhan`
- `source`, `link`, `Unnamed: 3`
- `symptom`, `symptom_onset`, `hosp_visit_date`

### 3. Manejo de Valores Faltantes

| Columna              | % Faltantes | Método de Imputación           |
| -------------------- | ----------- | ------------------------------ |
| `age`                | 55.6%       | Mediana                        |
| `recovered`          | 52.7%       | Mediana                        |
| `gender`             | 50.4%       | Moda ("Unknown")               |
| `death_confirmation` | 52.3%       | Conversión de fechas a binario |

**Criterio de eliminación:** Se eliminaron filas con valores faltantes restantes cuando representaban <5% del total.

### 4. Transformación de `death_confirmation`

- La columna original contenía fechas (indicando fallecimiento) y valores `0`/`1`
- Se convirtió a formato binario:
  - `0` = Sobreviviente
  - `1` = Fallecido
- Todos los valores no `0` (incluyendo fechas y `1`) se convirtieron a `1`

## 📋 Columnas Finales del Dataset

| Columna                    | Descripción                                                     | Tipo       |
| -------------------------- | --------------------------------------------------------------- | ---------- |
| **reporting_date**         | Fecha en que el caso fue reportado a las autoridades sanitarias | `datetime` |
| **location**               | Ubicación geográfica específica (ciudad, región o provincia)    | `object`   |
| **country**                | País donde se reportó el caso                                   | `object`   |
| **gender**                 | Género del paciente (male, female, Unknown)                     | `category` |
| **age**                    | Edad del paciente en años                                       | `float64`  |
| **international_traveler** | Indica si el paciente viajó internacionalmente (1=Sí, 0=No)     | `int64`    |
| **domestic_traveler**      | Indica si el paciente viajó dentro del mismo país (1=Sí, 0=No)  | `int64`    |
| **exposure_start**         | Fecha de inicio del período de exposición                       | `datetime` |
| **exposure_end**           | Fecha de fin del período de exposición                          | `datetime` |
| **death_confirmation**     | Variable objetivo (1=Falleció, 0=Sobrevivió)                    | `int64`    |
| **recovered**              | Indica si el paciente se recuperó (1=Sí, 0=No)                  | `int64`    |

## 🏗️ Ingeniería de Características (Feature Engineering)

Se crearon 3 variables agrupadoras para mejorar el análisis:

| Variable         | Descripción                             | Categorías                                   |
| ---------------- | --------------------------------------- | -------------------------------------------- |
| **age_group**    | Rangos etarios generales                | 0-18, 19-40, 41-60, 61-80, 80+               |
| **rango_etario** | Rangos etarios detallados               | 0-12, 13-18, 19-30, 31-45, 46-60, 61-75, 75+ |
| **season**       | Estación del año según `reporting_date` | Primavera, Verano, Otoño, Invierno           |

## 📈 Análisis Realizados

### Análisis Univariado

- Distribución de edad (histograma, boxplot, estadísticas descriptivas)
- Distribución por género
- Frecuencia de casos por país y ubicación

### Análisis Bivariado

- Mortalidad por género (tablas cruzadas y gráficos de barras)
- Mortalidad por rangos de edad
- Mortalidad por estación del año
- Boxplots comparativos (edad vs mortalidad)

### Análisis Multivariado

- Matriz de correlación entre variables numéricas
- Heatmap de correlaciones
- Pairplot para visualizar relaciones múltiples
- Prueba t-student para diferencias significativas

## 🔍 Hallazgos Clave (Conclusiones)

### ¿Quiénes tuvieron mayor mortalidad?

- **Hombres:** 6.3% vs **Mujeres:** 3.4% (1.85x más riesgo)
- **Adultos mayores 60+:** 14.2% mortalidad
- **Niños y jóvenes (0-18 años):** 0% mortalidad en la muestra
- **Estaciones:** Invierno y Primavera presentaron mayor mortalidad

### Factores más importantes (por correlación)

1. **Edad** - Correlación más fuerte con mortalidad
2. **Género** - Los hombres tienen riesgo significativamente mayor
3. **Estacionalidad** - La mortalidad varía por época del año

### Patrones sorprendentes

- No se registraron fallecimientos en menores de 18 años
- La mortalidad aumenta exponencialmente después de los 60 años
- Existe variabilidad estacional en la letalidad del virus

## 📊 Visualizaciones Generadas

| Tipo de Gráfico   | Propósito                                     |
| ----------------- | --------------------------------------------- |
| Histograma        | Distribución de edad                          |
| Boxplot           | Detección de outliers y comparación de grupos |
| Gráfico de barras | Conteos y tasas de mortalidad                 |
| Heatmap           | Matriz de correlaciones                       |
| Pairplot          | Relaciones multivariadas                      |
| Violin plot       | Distribución completa de variables            |

## 🚀 Cómo Ejecutar el Proyecto

```bash

# Clonar el repositorio

git clone <repository-url>
cd project-3-EDA-abel

# Activar el entorno virtual

python -m venv .venv
source .venv/bin/activate  # Linux/Mac
.venv\Scripts\activate     # Windows

# Instalar dependencias y librerías

pip install -r requirements.txt
```
