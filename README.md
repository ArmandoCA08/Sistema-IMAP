# Sistema IMAP  

![Estado del proyecto](https://img.shields.io/badge/Estado-en%20desarrollo-yellow)
![MATLAB](https://img.shields.io/badge/MATLAB-App%20Designer-orange)
![Python](https://img.shields.io/badge/Python-Procesamiento%20IA-blue)
![GitHub issues](https://img.shields.io/github/issues/ArmandoCA08/Sistema-IMAP)
![GitHub repo size](https://img.shields.io/github/repo-size/ArmandoCA08/Sistema-IMAP)
![GitHub last commit](https://img.shields.io/github/last-commit/ArmandoCA08/Sistema-IMAP)

## Sistema Automatizado para la Identificación de Mecanismos de Atrapamiento en Perforación de Pozos


![Interfaz principal del Sistema IMAP](Im%C3%A1genes/Captura%20de%20pantalla%20input%20sistema%20IMAP.png)

**Sistema IMAP** es una aplicación desarrollada para apoyar la identificación y sistematización del mecanismo de atrapamiento en operaciones de perforación de pozos, a partir de la interpretación de reportes de actividades (SIOP) y de la aplicación de la **Hoja de identificación del mecanismo de atrapamiento** con múltiples usos entre ellos en las Investigaciones Causa-Raíz (ICR).

---

## Objetivo general

Desarrollar un programa que, mediante la lectura de un archivo de reportes de actividades **(SIOP)**, identifique y sistematice la identificación del mecanismo de atrapamiento en las operaciones de perforación de pozos mediante la **Hoja de identificación del mecanismo de atrapamiento**.

---

## Descripción del proyecto

El atrapamiento de sarta de perforación es una de las problemáticas operativas más relevantes durante la perforación de pozos, ya que generan tiempos no productivos, incremento de costos, retrasos operativos y toma de decisiones bajo condiciones críticas.

Este proyecto busca automatizar parte del proceso de análisis mediante una interfaz que permite:

- Cargar reportes de actividades en formato Excel.
- Seleccionar la hoja de trabajo del archivo cargado.
- Visualizar las columnas disponibles del reporte.
- Filtrar información por fecha o rango de días asociados al evento de atrapamiento.
- Descargar el archivo filtrado.
- Ejecutar un procesamiento automatizado con apoyo de inteligencia artificial.
- Generar una hoja de identificación del mecanismo de atrapamiento en formato Excel.
- Presentar el mecanismo de atrapamiento identificado con base en criterios técnicos.

---


## Fundamento técnico

El sistema se basa en la interpretación de eventos operativos registrados antes y después del atrapamiento. A partir de estos eventos, se evalúan condiciones como:

- Dirección del movimiento de la tubería de perforación antes del atrapamiento.
- Movimiento descendente de la sarta después del atrapamiento.
- Condición de rotación posterior al atrapamiento.
- Condición de circulación posterior al atrapamiento.

Estas condiciones permiten asignar valores a tres posibles mecanismos principales:

1. **Empacamiento o Puenteo**
2. **Presión Diferencial**
3. **Geometría del Pozo**

El mecanismo con mayor puntuación en la hoja de identificación se considera el mecanismo más probable de atrapamiento.

![Captura de pantalla output sistema IMAP](Im%C3%A1genes/Captura%20de%20pantalla%20output%20sistema%20IMAP.png)

---

## Hoja de identificación del mecanismo de atrapamiento

![Hoja de identificación del mecanismo de atrapamiento](Im%C3%A1genes/Hoja%20de%20identificaci%C3%B3n%20del%20mecanismo%20de%20atrapamiento.jpg)

**Figura 1.** Hoja de identificación del mecanismo de atrapamiento.  
Mitchell, J. (2001). *Trouble-Free Drilling: Stuck Pipe Prevention* (Vol. 1). Drilbert Engineering Inc.

---

## Arquitectura del sistema

![Arquitectura del sistema](Im%C3%A1genes/Arquitectura%20del%20sistema.jpg)

El sistema está compuesto por dos partes principales:

### 1. Interfaz gráfica en MATLAB

La interfaz fue desarrollada en **MATLAB App Designer** y permite al usuario interactuar con el sistema mediante ventanas, tablas, botones, listas desplegables y selectores de fecha.

Funciones principales de la interfaz:

- Carga de archivos `.xlsx`.
- Selección de hoja de archivo.
- Visualización de datos cargados.
- Selección de columnas relevantes.
- Filtrado por fecha de inicio y fecha final del atrapamiento.
- Descarga del archivo filtrado.
- Ejecución del procesador de inteligencia artificial.
- Visualización de resultados en la pestaña de salida.

Archivo principal:

```text
SistemaHIMA.mlapp
```

> Nota: en este repositorio el código fuente exportado desde MATLAB App Designer puede encontrarse como archivo `.m` o como texto auxiliar, según la versión de trabajo.

### 2. Procesador en Python con IA

El procesamiento automatizado se realiza mediante el modelo **Llama-3.1-nemotron-70b-instruct** con un script de Python que:

- Lee el archivo Excel de actividades.
- Convierte el contenido del Excel a texto ordenado.
- Identifica eventos antes y después del atrapamiento.
- Analiza palabras clave operativas como:
  - `sarta atrapada`
  - `torsión`
  - `compresión`
  - `sin circulación`
  - `intento de empacamiento`
  - `trabaja sarta`
  - `sin movimiento`
- Clasifica las operaciones.
- Llena la hoja de identificación.
- Genera un archivo Excel con los resultados.

Archivo principal:

```text
Sistema_IMAP_ProcesadorIA.py
```

---

## Flujo general de trabajo

![Flujo general de trabajo](Im%C3%A1genes/Diagrama%20de%20flujo%20general%20del%20sistema.jpg)

---

## Requisitos

### MATLAB

- MATLAB R2022b o superior recomendado.
- App Designer.
- Soporte para lectura de archivos Excel.

### Python

- Python 3.10 o superior.
- Librerías requeridas:

```bash
pip install pandas openpyxl openai
```

### Dependencias usadas en el script de Python

```python
pandas
openpyxl
openai
os
re
```

---

## Configuración de la clave API

El procesador utiliza un cliente compatible con la API de OpenAI/NVIDIA para realizar el análisis del texto operativo.

Por seguridad, no se recomienda guardar claves API directamente en el código fuente. Se sugiere utilizar una variable de entorno:

```bash
setx NVIDIA_API_KEY "tu_clave_api"
```

Y en Python:

```python
import os
from openai import OpenAI

client = OpenAI(
    base_url="https://integrate.api.nvidia.com/v1",
    api_key=os.getenv("NVIDIA_API_KEY")
)
```

Puede obtener una clave API Key desde: https://docs.api.nvidia.com/nim/reference/llm-apis

---

## Uso del sistema

### 1. Abrir la aplicación en MATLAB

Abrir el archivo principal de la interfaz desde MATLAB App Designer o ejecutar la clase exportada:

```matlab
app = SistemaHIMAv1;
```

### 2. Cargar archivo SIOP

En la pestaña **Input**:

1. Presionar el botón **Cargar**.
2. Seleccionar el archivo Excel con los reportes de actividades.
3. Elegir la hoja del archivo.
4. Verificar que los datos se muestren correctamente en la tabla.

### 3. Seleccionar columnas relevantes

En el componente **List Box**, seleccionar las columnas que contienen información relevante, por ejemplo:

- Fecha
- Hora inicio
- Hora fin
- Actividad
- Bitácora
- Descripción operativa

### 4. Filtrar días del atrapamiento

Seleccionar:

- Día de inicio
- Día final

Después presionar **Aplicar Filtrado**.

### 5. Descargar archivo filtrado

Presionar **Descargar Filtrado** para guardar el archivo procesado en formato `.xlsx`.

### 6. Ejecutar procesamiento

Presionar el botón **Ejecutar** para llamar al script de Python encargado del análisis con inteligencia artificial.

### 7. Revisar resultados

En la pestaña **Output**, el sistema mostrará:

- Resultado del procesamiento.
- Tabla de la hoja de identificación.
- Archivo Excel generado con el análisis.

---

## Archivos generados

Durante la ejecución del sistema se pueden generar los siguientes archivos:

| Archivo | Descripción |
|---|---|
| `Archivo_Temporal_Ordenado.txt` | Texto ordenado generado a partir del archivo Excel. |
| `Clasificacion_Atrapamiento.txt` | Clasificación de operaciones antes y después del atrapamiento. |
| `Hoja_Identificacion_AI.txt` | Resultado textual del llenado de la hoja de identificación. |
| `Hoja_Identificacion_AI.xlsx` | Archivo final en Excel con la hoja de identificación generada. |

---

## Criterios de evaluación del mecanismo

La hoja de identificación asigna valores a cada mecanismo de acuerdo con la condición observada:

| Condición evaluada | Opciones principales |
|---|---|
| Dirección del movimiento de la TP antes del atrapamiento | Hacia arriba, Hacia abajo, Estática |
| Movimiento descendente después del atrapamiento | Libre, Con restricción, Imposible |
| Rotación después del atrapamiento | Libre, Con restricción, Imposible |
| Circulación después del atrapamiento | Libre, Con restricción, Imposible |

Cada opción asigna valores a:

- Empacamiento o Puenteo.
- Presión Diferencial.
- Geometría del Pozo.

Al final se calcula un total para cada mecanismo. El mayor total orienta la identificación del mecanismo predominante.

---

## Estado actual del desarrollo

Versión inicial del sistema:

- Interfaz gráfica funcional en MATLAB.
- Carga y visualización de archivos Excel.
- Filtrado por columnas y fechas.
- Descarga del archivo filtrado.
- Ejecución de script externo en Python.
- Generación automática de hoja de identificación con apoyo de IA.

---


## Créditos

Proyecto desarrollado como parte del trabajo académico y técnico relacionado con la identificación de mecanismos de atrapamiento en operaciones de perforación de pozos.

Asesoría técnica:

**M. en C. Cantera Martínez Gerardo**  
Jefe de Academia de Perforación
ESIA Ticomán | 2024 

**M. en C. José Adalberto Morquecho Robles**  
Especialista en Perforación y Geomecánica de pozos  
Grupo TANIS | 2024

---

## Referencias

Mitchell, J. (2001). *Trouble-Free Drilling: Stuck Pipe Prevention* (Vol. 1). Drilbert Engineering Inc.
