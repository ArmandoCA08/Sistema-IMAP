# Scripts - Sistema IMAP

Esta carpeta contiene los scripts principales del **Sistema Automatizado para la Identificación de Mecanismos de Atrapamiento en Perforación de Pozos (Sistema IMAP)**.

Los archivos incluidos permiten ejecutar el flujo de trabajo del sistema desde una interfaz gráfica en MATLAB y un procesador en Python encargado del análisis de reportes de actividades, clasificación de momentos del atrapamiento y generación de la hoja de identificación del mecanismo de atrapamiento.

---

## Contenido de la carpeta

```text
Scripts/
├── SistemaIMAP.mlapp
└── SistemaIMAP_ProcesadorIA.py
```

---

## Descripción de archivos

### `SistemaIMAP.m`

Archivo principal de la interfaz gráfica desarrollada en **MATLAB App Designer**.

Este archivo permite:

- Cargar un archivo Excel con reportes de actividades.
- Seleccionar la hoja de trabajo del archivo cargado.
- Visualizar las columnas disponibles del reporte.
- Seleccionar las columnas necesarias para el análisis.
- Filtrar información por rango de fechas asociado al evento de atrapamiento.
- Descargar el archivo filtrado en formato `.xlsx`.
- Ejecutar el script de Python encargado del análisis con inteligencia artificial.
- Visualizar la hoja de identificación generada.
- Mostrar el mecanismo de atrapamiento identificado con base en la mayor puntuación obtenida.

---

### `SistemaIMAP_ProcesadorIA.py`

Script de procesamiento desarrollado en **Python**.

Este archivo realiza el análisis automatizado del reporte filtrado y ejecuta el flujo de identificación mediante inteligencia artificial.

Sus funciones principales son:

- Leer el archivo Excel filtrado generado desde MATLAB.
- Convertir el contenido del Excel en texto estructurado.
- Identificar operaciones antes y después del atrapamiento.
- Analizar palabras clave relacionadas con eventos críticos, por ejemplo:
  - torsión;
  - compresión;
  - sin circulación;
  - sobretensión;
  - intento de liberar;
  - sarta atrapada.
- Responder la hoja de identificación del mecanismo de atrapamiento con valores predefinidos.
- Calcular los totales por mecanismo:
  - Empacamiento o Puenteo;
  - Presión Diferencial;
  - Geometría del Pozo.
- Generar el archivo final `Hoja_Identificacion_AI.xlsx`.

---

## Configuración de rutas en MATLAB

En el archivo `SistemaIMAP.m` se deben revisar las rutas utilizadas para:

- el ejecutable de Python;
- el script de Python;
- las imágenes de resultados del mecanismo de atrapamiento.

Ejemplo de configuración dentro del código:

```matlab
pyenv('Version', 'C:\Users\Usuario\AppData\Local\Programs\Python\Python312\python.exe');
```

También debe configurarse la ruta del script de Python:

```matlab
scriptPath = 'ruta/al/script/SistemaIMAP_ProcesadorIA.py';
```
