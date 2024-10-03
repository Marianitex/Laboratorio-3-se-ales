# Laboratorio-3-señales-
>  Señales electromiográficas EMG
---

Septiembre 2024

## Tabla de contenidos
* [EMG, Contraccion muscular y Fatiga](#emg)
* [Librerias](#librerias)
* [Adquisición de la Señal EMG](#adquisicion)
* [Filtrado de la Señal](#filtro)
* [Aventanamiento](#ventanas)
* [Prueba de hipotesis](#hipotesis)
* [Menu](#menu)
* [Contacto](#contacto)
---
<a name="iemg"></a> 
## EMG, Contraccion muscular y Fatiga

### 1.EMG

La electromiografía (EMG) es una técnica que mide la actividad eléctrica de los músculos. Utiliza electrodos para captar las señales eléctricas generadas durante las contracciones musculares, proporcionando información sobre la salud de los músculos y los nervios que los controlan. Esta señal puede variar dependiendo de factores como la intensidad de la contracción, el tipo de músculo y la fatiga. La EMG es clave para monitorear en tiempo real el comportamiento muscular durante actividades como la contracción sostenida.

### 2.Contraccion muscular

La contracción muscular es el proceso mediante el cual los músculos generan fuerza y movimiento. Se produce cuando los nervios envían señales eléctricas a las fibras musculares, desencadenando una interacción entre proteínas musculares, como la actina y la miosina. La contracción puede variar en intensidad y duración, y está estrechamente relacionada con la activación de unidades motoras, que son grupos de fibras musculares controladas por una sola neurona motora.

### 3.Fatiga Muscular

La fatiga muscular ocurre cuando el músculo es incapaz de mantener una contracción sostenida o la fuerza disminuye después de un esfuerzo prolongado. Esto puede deberse a la acumulación de metabolitos, la disminución de la energía disponible o la alteración de las señales nerviosas que controlan el músculo. Durante la fatiga, la señal EMG suele mostrar cambios, como una disminución en la frecuencia y un aumento en la amplitud de la señal, lo que indica la incapacidad del músculo para seguir funcionando a su capacidad máxima.

<a name="librerias"></a> 
## Librerias

```c
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import butter, filtfilt, windows
from scipy.fft import fft, fftfreq
from scipy.stats import ttest_ind
```

### 1. **import pandas as pd**
   - **pandas** es una biblioteca de Python diseñada para la manipulación y el análisis de datos estructurados (como tablas o bases de datos). Proporciona estructuras de datos como:
     - **DataFrame**: para manejar datos en formato tabular, con filas y columnas.
     - **Series**: para manejar datos unidimensionales.
   - En el contexto de procesamiento de señales EMG, podrías utilizar pandas para cargar, almacenar y organizar los datos recogidos durante el experimento, facilitando su análisis y visualización.

### 2. **import numpy as np**
   - **numpy** es la biblioteca de referencia para cálculos numéricos en Python. Se usa principalmente para trabajar con matrices y arreglos multidimensionales, además de incluir un amplio rango de funciones matemáticas.
   - En un análisis EMG, `numpy` permite realizar cálculos eficientes y manejar grandes conjuntos de datos, como los valores de la señal en diferentes momentos. También es útil para realizar operaciones matemáticas básicas o avanzadas sobre los datos.

### 3. **import matplotlib.pyplot as plt**
   - **matplotlib.pyplot** es una biblioteca que permite crear gráficos y visualizaciones. Se utiliza para visualizar datos de manera clara y efectiva, como gráficos de líneas, barras, histogramas, etc.
   - En el caso de análisis de señales EMG, podrías usar `plt` para generar gráficos de la señal EMG en el tiempo, o para visualizar la frecuencia o las características de la señal en distintas etapas del experimento.

### 4. **from scipy.signal import butter, filtfilt, windows**
   - **scipy.signal** es un submódulo de la biblioteca SciPy especializado en el procesamiento de señales. Las funciones aquí son útiles para manipular señales como la EMG.
     - **butter**: genera un filtro pasa-bajas o pasa-altas de tipo Butterworth, que es un filtro suave y comúnmente usado en el procesamiento de señales.
     - **filtfilt**: aplica un filtro a una señal sin generar un retraso de fase, filtrando la señal tanto hacia adelante como hacia atrás. Es útil para limpiar señales EMG de ruido sin alterar su fase.
     - **windows**: proporciona ventanas que suavizan una señal antes de aplicar la Transformada Rápida de Fourier (FFT) o para el filtrado, mejorando la calidad del análisis espectral.

### 5. **from scipy.fft import fft, fftfreq**
   - **scipy.fft** es otro submódulo de SciPy que contiene herramientas para realizar la **Transformada Rápida de Fourier (FFT)**, una técnica matemática que permite convertir una señal en el dominio del tiempo (como la EMG) al dominio de la frecuencia.
     - **fft**: calcula la FFT, permitiendo descomponer la señal en sus componentes de frecuencia.
     - **`fftfreq`**: genera las frecuencias asociadas a la transformada de Fourier, permitiendo identificar qué frecuencias están presentes en la señal.
   - Esto es especialmente útil en el análisis de señales EMG para estudiar las frecuencias características del músculo y detectar fenómenos como la fatiga.

### 6. **from scipy.stats import ttest_ind**
   - **scipy.stats** contiene herramientas estadísticas para el análisis de datos.
     - **ttest_ind**: realiza una prueba t de Student para comparar dos grupos independientes, determinando si existe una diferencia estadísticamente significativa entre sus medias.
   - En un análisis de señales EMG, esta función se podría usar para comparar dos grupos diferentes de datos (por ejemplo, señales EMG antes y después de la fatiga muscular) y verificar si las diferencias observadas son significativas o se deben al azar.

<a name="adquisicion"></a> 
## Adquisicion de la señal EMG

Para estudiar la adquisición de la señal EMG durante la contracción muscular continua hasta la fatiga, es esencial monitorear en tiempo real la actividad eléctrica de los músculos. Al pedirle al sujeto que mantenga una contracción hasta la fatiga, se pueden observar cambios en la señal EMG que reflejan la disminución en el rendimiento muscular. Estos datos permiten analizar cómo el músculo responde a esfuerzos prolongados y en qué momento comienza la fatiga. Además, se pueden identificar patrones característicos que ayudan a comprender mejor el comportamiento del sistema neuromuscular bajo estrés.


```c

```

<a name="filtro"></a> 
## Filtrado de la señal


<a name="Ventanas"></a> 
## Aventanamiento



<a name="hipotesis"></a> 
## Prueba de hipotesis




<a name="menu"></a> 
## Menu




<a name="contacto"></a> 
## Contacto
* Creado por [Marianitex](https://github.com/Marianitex) - sígueme en mis redes sociales como @_mariana.higuera
