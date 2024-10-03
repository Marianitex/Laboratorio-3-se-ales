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
     - **fftfreq**: genera las frecuencias asociadas a la transformada de Fourier, permitiendo identificar qué frecuencias están presentes en la señal.
   - Esto es especialmente útil en el análisis de señales EMG para estudiar las frecuencias características del músculo y detectar fenómenos como la fatiga.

### 6. **from scipy.stats import ttest_ind**
   - **scipy.stats** contiene herramientas estadísticas para el análisis de datos.
     - **ttest_ind**: realiza una prueba t de Student para comparar dos grupos independientes, determinando si existe una diferencia estadísticamente significativa entre sus medias.
   - En un análisis de señales EMG, esta función se podría usar para comparar dos grupos diferentes de datos (por ejemplo, señales EMG antes y después de la fatiga muscular) y verificar si las diferencias observadas son significativas o se deben al azar.

<a name="adquisicion"></a> 
## Adquisicion de la señal EMG

Para estudiar la adquisición de la señal EMG durante la contracción muscular continua hasta la fatiga, es esencial monitorear en tiempo real la actividad eléctrica de los músculos. Al pedirle al sujeto que mantenga una contracción hasta la fatiga, se pueden observar cambios en la señal EMG que reflejan la disminución en el rendimiento muscular. Estos datos permiten analizar cómo el músculo responde a esfuerzos prolongados y en qué momento comienza la fatiga. Además, se pueden identificar patrones característicos que ayudan a comprender mejor el comportamiento del sistema neuromuscular bajo estrés.

### ¿Cómo se toma una señal EMG en el bíceps?

La electromiografía (EMG) en el bíceps se realiza utilizando electrodos que registran la actividad eléctrica de los músculos mientras se contraen. El proceso incluye varios pasos clave:

1. **Preparación del área**:
   - Se limpia la piel del bíceps para eliminar grasa o suciedad que pueda interferir con la calidad de la señal. Esto se hace generalmente con alcohol.
   
2. **Colocación de los electrodos**:
   - **Electrodos de superficie**: Se colocan electrodos sobre la piel, específicamente en el vientre del músculo bíceps. Los electrodos deben colocarse de manera que capten correctamente la actividad eléctrica del músculo sin interferencia de otros músculos cercanos. 
   - **Electrodo de referencia**: Este electrodo se coloca en una zona inactiva (por ejemplo, un hueso cercano o una zona sin músculo) para comparar la actividad eléctrica y mejorar la precisión de la señal captada.
   
3. **Captura de la señal**:
   - El sujeto realiza contracciones controladas del bíceps, como flexionar el brazo o levantar peso. Durante este proceso, los electrodos registran la señal EMG en tiempo real.
   - La señal captada es el resultado de la actividad eléctrica generada por las fibras musculares del bíceps cuando reciben estímulos nerviosos y se contraen. 

4. **Registro de la señal**:
   - La señal registrada es procesada y digitalizada para su análisis posterior. A menudo, se utiliza un dispositivo de adquisición de datos para convertir la señal eléctrica en datos que puedan ser analizados por software.

### ¿Por qué es un buen muestreo el bíceps?

Primero, su **aislamiento muscular** es clave. El bíceps braquial es un músculo grande y superficial, lo que facilita la colocación de los electrodos de superficie y minimiza la interferencia de otros músculos cercanos. Esto permite obtener una señal clara y bien definida, lo cual es esencial para obtener datos precisos.

Además, el bíceps tiene una **alta accesibilidad**. Al estar ubicado en una zona fácil de alcanzar, se facilita la colocación de los electrodos, la calibración del equipo y el registro de la señal, incluso en actividades dinámicas como el levantamiento de peso.

El bíceps también participa en una **amplia gama de movimientos**. Se activa en distintos tipos de actividades, como la flexión del codo y la rotación del antebrazo, lo que permite medir una gran variedad de contracciones musculares, desde ligeras hasta máximas, proporcionando una representación integral de la actividad muscular.

Otra ventaja es su **capacidad para medir la fatiga muscular**. Al ser fácilmente fatigable, especialmente en ejercicios repetitivos o con pesos, es un excelente candidato para estudiar la fatiga muscular. Durante la fatiga, se observan cambios notables en la señal EMG, como un aumento en la amplitud o una disminución en la frecuencia, lo que permite extraer información valiosa sobre el rendimiento muscular bajo esfuerzo.

Además, la **buena calidad de la señal** EMG es otro punto a favor del bíceps. Está cubierto por una capa de tejido relativamente fina, sin una cantidad excesiva de grasa o capas gruesas de piel, lo que reduce la interferencia de otros tejidos y mejora la claridad de la señal.

Por último, el bíceps es comúnmente utilizado en **estudios neuromusculares**, dada su simplicidad anatómica y la facilidad para aislarlo en pruebas de fuerza o resistencia. Esto lo convierte en un músculo ideal para investigar la interacción entre el sistema nervioso y el sistema muscular.

IMAGEEEEEEEEEEEEN

### Adquisición de la señal EMG con STM32

El proceso de toma de señal EMG con un STM32 y un módulo de ECG (AD8232) se lleva a cabo en varios pasos clave. Primero, se utiliza un módulo ECG para captar la señal EMG del músculo, en este caso el bíceps. Los **electrodos de superficie** se colocan sobre el músculo para detectar la actividad eléctrica generada durante la contracción muscular. Esta señal es muy pequeña y ruidosa, por lo que el módulo ECG se encarga de amplificarla y filtrar las frecuencias no deseadas, como el ruido ambiental.

![OIP](https://github.com/user-attachments/assets/03851d9f-e551-42c7-99f5-8d4adc33a956)


Una vez que la señal ha sido filtrada y amplificada, el **STM32** entra en acción. Este microcontrolador tiene un **ADC (Conversor Analógico a Digital)** que convierte la señal EMG, que es analógica, en una señal digital. Esto permite que el STM32 procese la señal, almacene temporalmente los datos y, si es necesario, aplicar filtros adicionales que se realizan mas adelante para mejorar la calidad de la señal. El microcontrolador se configura para tomar muestras de la señal EMG a una frecuencia adecuada, típicamente entre 500 y 1000 Hz, lo que garantiza que se capturen detalles suficientes de la actividad muscular.

Después de la adquisición, el STM32 transmite los datos a una interfaz externa, normalmente una computadora. Esto se puede hacer mediante **comunicación inalámbrica**, como Bluetooth. Esta transmisión permite que los datos EMG sean visualizados o procesados en tiempo real.

En la **interfaz de usuario**, los datos son recibidos y visualizados en tiempo real utilizando un programa en la computadora, como Python. Estas herramientas permiten graficar la señal EMG y realizar análisis básicos. Además, los datos se pueden almacenar en un archivo para su posterior procesamiento. Un formato común es **Excel**, que permite almacenar los datos en columnas de tiempo y voltaje.

Finalmente, los datos se guardan en un archivo **Excel** para análisis posterior. Con la ayuda de bibliotecas como `pandas`, los datos se pueden leer fácilmente desde el archivo Excel, lo que permite realizar análisis adicionales como filtrado, análisis de frecuencias (FFT) o estudios de fatiga muscular. Este flujo de trabajo proporciona una forma eficiente de captar, visualizar y almacenar señales EMG utilizando un STM32 y un módulo ECG.

```c
# --- Función para leer la señal desde Excel ---
def read_signal_from_excel(file_path):
    df = pd.read_excel(file_path)
    time = df.iloc[:, 0].values
    data = df.iloc[:, 1].values
    return time, data
```

<a name="filtro"></a> 
## Filtrado de la señal




```c

```

<a name="Ventanas"></a> 
## Aventanamiento



<a name="hipotesis"></a> 
## Prueba de hipotesis




<a name="menu"></a> 
## Menu




<a name="contacto"></a> 
## Contacto
* Creado por [Marianitex](https://github.com/Marianitex) - sígueme en mis redes sociales como @_mariana.higuera
