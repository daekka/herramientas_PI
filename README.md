# herramientas_PI
Herramientas para PI Osisoft

## Proceso de trabajo

1. **Configuración inicial**
   - Importación de librerías necesarias (PIconnect, pandas, numpy, etc.)
   - Configuración del servidor PI (servidor, usuario, contraseña)
   - Configuración de la zona horaria (Europe/Madrid)

2. **Descarga de datos**
   - Definición del rango de fechas
   - Especificación de tags a descargar
   - Descarga datos en archivos CSV organizados por mes
   - Los archivos se guardan en carpetas por año

3. **Procesamiento de datos**
   - Lectura y combinación de CSVs
   - Conversión de tipos de datos
   - Análisis exploratorio
   - Cálculo de horas de funcionamiento y arranques

## Funciones disponibles

### leer_csv_en_carpetas(ruta_base)
Lee archivos CSV dentro de las subcarpetas de la ruta dada y concatena los datos en un solo DataFrame.  
- Parámetros:  
  - ruta_base (str): Ruta base a explorar.

### convertir_columnas_a_numerico(df, columnas, tipo_dato=float)
Convierte columnas a tipo numérico, descartando filas con valores no convertibles.  
- Parámetros:  
  - df (pd.DataFrame): DataFrame de entrada.  
  - columnas (list): Nombres de las columnas a convertir.  
  - tipo_dato (type): Tipo de dato al que se convertirán las columnas (p.ej. float, int).

### EDA(df)
Realiza un análisis exploratorio rápido con un histograma usando Seaborn.  
- Parámetros:  
  - df (pd.DataFrame): DataFrame con los datos a analizar.

### contar_horas_arranques(dataframe, tag, estado_marcha, intervalo)
Calcula el número de horas que una señal permanece en estado de marcha y cuenta los arranques (transiciones 0 → 1).  
- Parámetros:  
  - dataframe (pd.DataFrame): DataFrame con la señal a evaluar.  
  - tag (str): Nombre de la columna con la señal.  
  - estado_marcha (int): Valor que indica marcha (1) o paro (0).  
  - intervalo (int): Frecuencia de muestreo en segundos.

## Ejemplo de uso

1. **Preparación de datos**
```python
# Configurar ruta base
ruta_base = 'datos_arranque_PAC13'

# Crear DataFrame combinado
df_leido = leer_csv_en_carpetas(ruta_base)

# Convertir columnas necesarias a numérico
columnas_a_convertir = ['SAB:CBOP.A81.PAC13.AP001XH01']
df_leido = convertir_columnas_a_numerico(df_leido, columnas_a_convertir, tipo_conversion=int)
```

2. **Análisis de datos**
```python
# Realizar análisis exploratorio
EDA(df_leido)

# Calcular horas y arranques
tag = "SAB:CBOP.A81.PAC13.AP001XH01"
estado_marcha = 1
intervalo_muestreo_datos = 60  # segundos
resultado = contar_horas_arranques(df_leido, tag, estado_marcha, intervalo_muestreo_datos)
