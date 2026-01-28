# 📉 Predicción de Churn en Telecomunicaciones

![Python](https://img.shields.io/badge/Python-3.8%2B-blue)
![Scikit-Learn](https://img.shields.io/badge/Library-Scikit--Learn-orange)
![Status](https://img.shields.io/badge/Status-Finalizado-green)

> **Resumen:** Solución End-to-End de Machine Learning para predecir la fuga de clientes (Churn) en una empresa de telecomunicaciones. Incluye limpieza de datos, análisis exploratorio, modelado con Random Forest y un Pipeline automatizado listo para producción.

---

## 📖 Contexto del Proyecto
La pérdida de clientes (Churn) es uno de los desafíos más costosos en la industria de las telecomunicaciones. Este proyecto analiza datos históricos para:
1.  **Entender:** ¿Por qué se van los clientes?
2.  **Predecir:** ¿Qué clientes tienen alta probabilidad de irse el próximo mes?
3.  **Actuar:** Diseñar estrategias de retención basadas en datos.

---

## 📂 Diccionario de Datos
El modelo fue entrenado utilizando un dataset consolidado con las siguientes variables clave:

| Variable | Tipo | Descripción | Ejemplo |
| :--- | :--- | :--- | :--- |
| `customer_id` | Texto | ID único del cliente (No utilizado en predicción) | `7590-VHVEG` |
| `tenure` | Numérico | Meses que el cliente ha permanecido en la empresa | `12`, `24` |
| `monthly_charges` | Numérico | Monto mensual facturado | `29.85` |
| `total_charges` | Numérico | Monto total facturado histórico | `1889.50` |
| `contract_type` | Categórico | Tipo de contrato (Factor crítico) | `Month-to-month`, `Two year` |
| `payment_method` | Categórico | Medio de pago utilizado | `Electronic check`, `Credit card` |
| `internet_service` | Categórico | Tipo de servicio de internet | `Fiber optic`, `DSL`, `No` |
| `churn` | Target | **Variable Objetivo:** ¿El cliente canceló? | `0` (No), `1` (Sí) |

---

## 🛠️ Tecnologías y Herramientas
* **Lenguaje:** Python
* **ETL & Análisis:** Pandas, NumPy
* **Visualización:** Seaborn, Matplotlib
* **Machine Learning:** Scikit-Learn (Random Forest, Pipeline, ColumnTransformer)
* **Control de Versiones:** Git/GitHub

---

## ⚙️ Arquitectura del Proyecto

El desarrollo se estructuró en 4 fases principales:

1.  **Ingeniería de Datos (ETL):** Limpieza de nulos, tratamiento de duplicados y transformación de tipos.
2.  **EDA (Análisis Exploratorio):** Detección de patrones y correlaciones.
3.  **Modelado:** Entrenamiento y validación de algoritmos.
4.  **Deployment:** Creación de un Pipeline serializado.

### 🔄 Flujo del Pipeline (Model Workflow)
El siguiente diagrama ilustra cómo el artefacto `.joblib` procesa los datos automáticamente:

```mermaid
graph TD;
    %% --- DEFINICIÓN DE CLASES (La Paleta de Colores) ---
    %% Input: Morado Profundo (Indica inicio/datos)
    classDef entrada fill:#4A148C,stroke:#B39DDB,stroke-width:2px,color:#fff,font-weight:bold;
    
    %% Procesos: Gris Azulado (Neutro para pasos intermedios)
    classDef proceso fill:#37474F,stroke:#90A4AE,stroke-width:1px,color:#fff,font-weight:bold;
    
    %% Decisión: Naranja Ladrillo (Visible pero no chillón)
    classDef decision fill:#E65100,stroke:#FFCC80,stroke-width:2px,color:#fff,font-weight:bold;
    
    %% Modelo: Azul Profundo (Tecnológico)
    classDef modelo fill:#0D47A1,stroke:#64B5F6,stroke-width:2px,color:#fff,font-weight:bold;
    
    %% Salida: Verde Bosque (Éxito/Resultado)
    classDef salida fill:#1B5E20,stroke:#81C784,stroke-width:2px,color:#fff,font-weight:bold;

    %% --- EL DIAGRAMA ---
    A["Datos Crudos (Raw Data)"]:::entrada --> B("Preprocesador: ColumnTransformer");
    B:::proceso --> C{"¿Tipo de Variable?"};
    
    C:::decision -- Numérica --> D["Passthrough (Sin cambios)"];
    C:::decision -- Categórica --> E["One-Hot Encoding"];
    
    D:::proceso --> F["Concatenación"];
    E:::proceso --> F:::proceso;
    
    F --> G("Modelo: Random Forest Classifier");
    G:::modelo --> H["Predicción Final (0 o 1)"];
    H:::salida
```
## 📊 Resultados e Insights de Negocio

El modelo **Random Forest** alcanzó un **Accuracy aproximado del 90%** en el set de prueba. Basado en la importancia de las variables (Feature Importance), se generaron las siguientes recomendaciones:

1.  **Alerta en Contratos Mensuales:** Los clientes con contrato "Month-to-month" son los más propensos a irse.
    * *Estrategia:* Ofrecer descuentos por migración a planes anuales.
2.  **Riesgo en Nuevos Clientes:** La tasa de cancelación es crítica en los primeros meses (`tenure` bajo).
    * *Estrategia:* Programa de "Onboarding VIP" durante los primeros 90 días.
3.  **Sensibilidad al Precio:** Usuarios con cargos altos sin servicios premium tienden a rotar.
    * *Estrategia:* Revisión de planes y oferta de beneficios exclusivos.

---

## 🚀 Cómo usar este proyecto

### 1. Clonar el repositorio
```bash
git clone [https://github.com/DaisyQuinteros/ChurnInsight.git](https://github.com/DaisyQuinteros/ChurnInsight.git)
```
### 2. Cargar el Modelo (Para integración en Backend)
El proyecto entrega un archivo `pipeline_churninsight_v1.joblib` que acepta datos crudos.

```import joblib
import pandas as pd

# Cargar el pipeline
# Asegúrate de que el archivo .joblib esté en la misma carpeta o ajusta la ruta
modelo = joblib.load('pipeline_churninsight_v1.joblib')

# Ejemplo de cliente nuevo (Datos crudos como vienen de la web)
nuevo_cliente = pd.DataFrame([{
    'contract_type': 'Month-to-month',
    'monthly_charges': 70.5,
    'tenure': 2,
    'payment_method': 'Electronic check',
    # ... otras columnas requeridas por el modelo
}])

# Predicción (0 = Se queda, 1 = Se va)
prediccion = modelo.predict(nuevo_cliente)
print(f"Predicción de Churn: {prediccion[0]}")
```

## ⚙️ Instrucciones de Ejecución

Para replicar el análisis o explorar el código paso a paso:

### 1. Prerrequisitos
Instalar las dependencias necesarias ejecutando:
```bash
pip install -r requirements.txt
```
### 2. Orden de los Notebooks
El proyecto se estructura en dos etapas lógicas. Se recomienda seguir este orden de lectura/ejecución:

#### Paso 1: ETL y Preparación

- Abrir: preparacion_de_las_bases_de_datos.ipynb
- Descripción: Este notebook procesa los archivos CSV crudos (base_clientes_real.csv, etc.), realiza la limpieza y genera el dataset maestro.

#### Paso 2: Análisis y Modelado

- Abrir: proyecto_churninsight_prediccion_de_cancelacion_de_clientes.ipynb
- Descripción: Contiene el Análisis Exploratorio de Datos (EDA), el entrenamiento del modelo Random Forest y la exportación de los pipelines.

## 📂 Estructura de Archivos
```text
├── base_clientes_real.csv                                              # (y otros csv) Datos crudos
├── churn_dataset_procesado_V1.csv                                      # Dataset final limpio
├── preparacion_de_las_bases_de_datos.ipynb                             # Notebook de ETL
├── proyecto_churninsight_prediccion_de_cancelacion_de_clientes.ipynb   # Notebook de Análisis/Modelo
├── pipeline_churninsight_v1.joblib                                     # Pipeline listo para producción
├── modelo_churninsight_random_forest.joblib                            # Modelo entrenado
├── README.md                                                           # Documentación del proyecto
└── requirements.txt                                                    # Librerías necesarias
```
---

## ✒️ Autor
**Daisy Quinteros Silva**
* **Rol:** Data Scientist / Ingeniero en Informática
* [LinkedIn](www.linkedin.com/in/daisy-quinteros-silva-5b0450a5)


---
*Proyecto aplicado en la simulación laboral de No Country, utilizando conocimientos del programa ONE - Alura Latam.*
