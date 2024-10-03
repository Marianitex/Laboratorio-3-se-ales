# Laboratorio-3-señales-
>  Señales electromiográficas EMG
---

Septiembre 2024

## Tabla de contenidos
* [¿Que se va a realizar?](#emg)
* [Librerias](#librerias)
* [Adquisición de la Señal EMG](#adquisicion)
* [Filtrado de la Señal](#filtro)
* [Aventanamiento](#ventanas)
* [Analisis Espectral](#analisis)
* [Prueba de hipotesis](#hipotesis)
* [Menu](#menu)
* [Contacto](#contacto)
---
<a name="iemg"></a> 
## ¿Que se va a realizar?

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


![Clipped_image_20241003_082404](https://github.com/user-attachments/assets/b814a705-55a1-4090-b25a-6b7860a8a19d)


![Clipped_image_20241003_082530](https://github.com/user-attachments/assets/34384239-4810-4064-a5e9-8f983eedd6f8)


![Clipped_image_20241003_082554](https://github.com/user-attachments/assets/9d8dc853-566a-437c-9cda-e4f7fab1933a)

### Adquisición de la señal EMG con STM32

El proceso de toma de señal EMG con un STM32 y un módulo de ECG (AD8232) se lleva a cabo en varios pasos clave. Primero, se utiliza un módulo ECG para captar la señal EMG del músculo, en este caso el bíceps. Los **electrodos de superficie** se colocan sobre el músculo para detectar la actividad eléctrica generada durante la contracción muscular. Esta señal es muy pequeña y ruidosa, por lo que el módulo ECG se encarga de amplificarla y filtrar las frecuencias no deseadas, como el ruido ambiental.

![OIP](https://github.com/user-attachments/assets/03851d9f-e551-42c7-99f5-8d4adc33a956)

![IMG-20241003-WA0012](https://github.com/user-attachments/assets/88f3ecfb-be97-4719-945d-42d4cbbd9a7b)



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
### Fragmento señal emg cruda desde excel
![image](https://github.com/user-attachments/assets/bea2089c-29a8-49f4-9eaf-088498a38250)

### Señal emg cruda desde excel completa 

![image](https://github.com/user-attachments/assets/c6b748eb-7d96-441c-8cfd-c624bd4a4264)


La gráfica de la señal capturada refleja información crucial sobre la actividad muscular, destacando varios aspectos clave que justifican la calidad y utilidad de los datos adquiridos. En este caso, la **frecuencia de muestreo** es de 1000 Hz, lo que implica que la señal se muestrea 1000 veces por segundo. Esta alta frecuencia de muestreo es esencial para registrar adecuadamente la actividad eléctrica del músculo, permitiendo capturar cambios rápidos en la señal EMG. Dado que las señales EMG pueden contener componentes de frecuencia de hasta 500 Hz, una frecuencia de muestreo de 1000 Hz asegura que se evite el aliasing, cumpliendo con el teorema de muestreo de Nyquist.

La **duración de la señal** es de 30.00 segundos, un periodo adecuado que permite el registro de múltiples contracciones musculares. Este tiempo es beneficioso para observar variaciones en la actividad muscular a lo largo del tiempo y evaluar la respuesta del músculo ante diferentes niveles de esfuerzo. Además, la duración proporciona un contexto suficiente para estudiar la fatiga muscular y cómo esta afecta la función del músculo durante el ejercicio.

La **longitud de la señal** es de 30,000 puntos, resultado de multiplicar la frecuencia de muestreo (1000 Hz) por la duración de la señal (30 segundos). Esta cantidad significativa de datos es fundamental para llevar a cabo análisis detallados y robustos, lo que permite realizar estadísticas y análisis espectrales precisos. Tener 30,000 puntos de datos facilita la identificación de patrones y tendencias en la actividad muscular, contribuyendo a una comprensión más completa de su funcionamiento.

Adicionalmente, se registraron **10 contracciones musculares** durante este periodo de muestreo. Esta cantidad es clave, ya que permite evaluar la respuesta del bíceps a las contracciones repetidas, lo que es esencial para el análisis de la fatiga muscular. Al observar cómo varía la señal EMG durante estas contracciones, se pueden identificar cambios en la amplitud y la frecuencia de la señal, que son indicadores de fatiga. Esto proporciona información valiosa sobre el rendimiento muscular y cómo este se ve afectado por la actividad repetitiva.


<a name="filtro"></a> 
## Filtrado de la señal

```c
# --- Función para aplicar filtros pasa altas y pasa bajas ---
def butter_filter(data, lowcut, highcut, fs, order=5):
    nyquist = 0.5 * fs
    low = lowcut / nyquist
    high = highcut / nyquist
    b, a = butter(order, [low, high], btype='band')
    return filtfilt(b, a, data)
```

### Filtro Butterworth

El **filtro Butterworth** es un tipo de filtro ampliamente utilizado en procesamiento de señales, conocido por su respuesta en frecuencia que es lo más plana posible en la banda pasante. Esto significa que las amplitudes de las frecuencias dentro de esta banda son constantes, lo que ayuda a mantener la integridad de la señal durante su procesamiento. Su característica de suavidad lo convierte en una opción ideal para aplicaciones biomédicas, como la electromiografía (EMG), donde es crucial minimizar cualquier distorsión en la señal que se está analizando.

Una de las características más importantes del filtro Butterworth es el **orden del filtro**, que determina la agudeza de la caída de la respuesta en frecuencia fuera de la banda pasante. Un filtro de orden más alto proporcionará una transición más abrupta entre las frecuencias permitidas y las que se atenuarán. En este caso, se eligió un **orden de 5** para el filtro, lo que permite una reducción significativa de las frecuencias no deseadas mientras se preservan las frecuencias de interés. Este orden es adecuado para la filtración de señales EMG, donde la respuesta en frecuencia más plana en la banda pasante es esencial para obtener mediciones precisas y representativas de la actividad muscular.

### Frecuencias de Corte en EMG

Las frecuencias de corte elegidas para filtrar la señal EMG son típicamente de 20 Hz para la frecuencia baja y de 450 Hz para la frecuencia alta. Esta selección se basa en la fisiología muscular y las características inherentes a la señal EMG. Por un lado, las frecuencias por debajo de 20 Hz pueden estar contaminadas por ruido de fondo, como interferencias de la red eléctrica o artefactos de movimiento, que no reflejan la actividad muscular. Al establecer esta frecuencia de corte, se asegura que solo se analicen las frecuencias relevantes para la activación muscular.

Por otro lado, establecer una frecuencia de corte alta de 450 Hz es igualmente importante, ya que las frecuencias superiores a este umbral generalmente no están asociadas con la actividad muscular. Estas frecuencias pueden incluir ruido de alta frecuencia de equipos de adquisición y otros artefactos indeseados. Al filtrar estas componentes no deseadas, se garantiza que la señal analizada contenga únicamente información relevante sobre la activación muscular.

### Orden del Filtro (5) y Cálculo del Ripple

La elección de un filtro de **orden 5** es un balance entre la efectividad del filtrado y la complejidad computacional. Un filtro de este orden proporciona una transición más rápida entre la banda pasante y la banda atenuada, lo que permite una atenuación más eficaz de las frecuencias no deseadas. Esto es fundamental para mejorar la calidad de la señal EMG procesada.

Además, un filtro de orden 5 es generalmente suficiente para lograr una separación efectiva de las frecuencias sin introducir complicaciones adicionales que podrían llevar a un comportamiento no deseado. Aumentar el orden del filtro puede resultar en un comportamiento más complejo y potencialmente inestable. Por lo tanto, seleccionar un filtro de orden 5 ayuda a garantizar tanto la estabilidad como la efectividad en la eliminación de componentes no deseadas.

El **ripple** se refiere a las pequeñas variaciones en la respuesta en frecuencia dentro de la banda pasante de un filtro. En el caso de los filtros Butterworth, el ripple es generalmente inexistente, lo que significa que la señal dentro de la banda pasante no presenta fluctuaciones significativas en amplitud. Esto es una ventaja, ya que asegura que la señal EMG no se vea afectada por cambios inesperados en la amplitud dentro de su rango de frecuencias relevantes. 
### Fragmento señal emg filtrada desde excel
![image](https://github.com/user-attachments/assets/bac5cf2d-c4ca-481e-9d2f-c75bf51e5c90)

### Señal emg filtrada desde excel completa

![image](https://github.com/user-attachments/assets/a6201bb5-802a-4e5b-8774-adeb7dcd3fcc)

<a name="Ventanas"></a> 
## Aventanamiento

```c
# --- Función para dividir la señal en ventanas y aplicar una ventana de Hamming ---
def apply_windowing(filtered_data, sampling_rate):
    window_size = int(0.5 * sampling_rate)  # Definir tamaño de la ventana en base al tiempo (0.5 segundos aquí)
    window = windows.hamming(window_size)   # Ventana de Hamming
    num_windows = len(filtered_data) // window_size  # Número de ventanas completas en la señal
    windowed_data = []
    
    for i in range(num_windows):
        segment = filtered_data[i*window_size:(i+1)*window_size]  # Extraer segmentos de la señal filtrada
        windowed_segment = segment * window  # Aplicar la ventana
        windowed_data.append(windowed_segment)

    return np.array(windowed_data), window_size
```

![image](https://github.com/user-attachments/assets/a6201bb5-802a-4e5b-8774-adeb7dcd3fcc)


El **aventaneo** es un proceso crítico en el análisis de señales que implica dividir una señal continua en segmentos más pequeños o ventanas. Este enfoque es particularmente útil en el procesamiento de señales EMG, ya que permite realizar un análisis más detallado y localizado de la señal en el tiempo. En el código proporcionado, se utiliza una **ventana de Hamming**, que es una de las muchas ventanas disponibles, y se justifica su elección por varias razones.

#### Criterios para Seleccionar la Ventana de Hamming

La **ventana de Hamming** es elegida principalmente por su capacidad para minimizar las distorsiones y el “ringing” (eco) que pueden ocurrir en los bordes de los segmentos de la señal. A diferencia de ventanas como la rectangular, que pueden introducir discontinuidades al cortar la señal, la ventana de Hamming suaviza estos bordes, lo que ayuda a reducir las fluctuaciones indeseadas en la frecuencia cuando se realiza la transformada de Fourier. Esta propiedad es esencial para obtener un análisis espectral preciso, ya que permite que la energía de la señal se distribuya de manera más uniforme en el espectro de frecuencias.

#### Tamaño y Forma de la Ventana

En el código, el tamaño de la ventana se establece en **0.5 segundos**, calculado como el producto de la frecuencia de muestreo (1000 Hz en este caso). Esto significa que cada ventana contendrá 500 muestras de datos. Este tamaño se selecciona en función del equilibrio entre la resolución temporal y la capacidad de capturar adecuadamente la variabilidad de la señal. Un tamaño de ventana más pequeño podría proporcionar una mayor resolución temporal, pero a costa de reducir la cantidad de datos analizados en cada ventana, lo que podría no ser suficiente para obtener resultados significativos.

#### Aplicación de la Ventana

El proceso de aplicación de la ventana de Hamming se realiza extrayendo segmentos de la señal filtrada en cada iteración del bucle. Para cada segmento de datos, se multiplica por la ventana de Hamming, lo que modifica la amplitud de las muestras en los extremos del segmento, haciendo que caigan gradualmente a cero. Este enfoque no solo suaviza la señal, sino que también ayuda a minimizar la discontinuidad en los límites de las ventanas, preservando la integridad de la señal durante el análisis.

#### Comparación de la Ventana con la Señal

Para ilustrar el efecto del aventanamiento, es útil comparar visualmente la señal filtrada original con la ventana aplicada. Antes de la convolución, la señal muestra variaciones que pueden incluir ruidos o picos agudos. Sin embargo, una vez que se aplica la ventana de Hamming, se observa que las amplitudes en los extremos de cada ventana se reducen significativamente, lo que permite una transición más suave hacia cero. Esta comparación puede visualizarse mediante gráficos que muestran la señal original y las ventanas aplicadas superpuestas. 

![image](https://github.com/user-attachments/assets/e3be6e16-0a6c-48e6-b64b-3599c7b14554)

![image](https://github.com/user-attachments/assets/09b173a1-b411-4e37-806c-fc75ce48ad59)

![image](https://github.com/user-attachments/assets/532ac47a-4c7d-4ec5-9b22-b97d3c3c483d)

![image](https://github.com/user-attachments/assets/83af830d-150b-4b34-a04b-5574e6af56b8)

![image](https://github.com/user-attachments/assets/4097525b-8281-473c-b7f2-0b195bd995dd)

### Reducción del Ruido en el Aventanamiento de la Señal EMG

El **aventanamiento** es una técnica utilizada en el procesamiento de señales que implica dividir la señal en segmentos o ventanas más pequeñas. Este método tiene un impacto significativo en la reducción del ruido en las señales EMG por varias razones clave.

Primero, las ventanas, como la **ventana de Hamming**, están diseñadas para suavizar las transiciones en los bordes de cada segmento. En lugar de cortar abruptamente la señal, la ventana de Hamming reduce gradualmente la amplitud de la señal a cero en los extremos. Esto minimiza las discontinuidades que pueden introducir ruido o artefactos en la señal. Al aplicar esta suavización, se evita que los picos abruptos de amplitud afecten el análisis espectral de la señal, contribuyendo a una representación más limpia y menos ruidosa.

En segundo lugar, cuando se utiliza el aventanamiento, cada ventana captura solo una parte de la señal en un momento específico. Dado que las señales EMG pueden contener ruido aleatorio, promediar varias ventanas permite que el ruido se disperse. A medida que se procesan múltiples ventanas, las variaciones de ruido pueden cancelarse entre sí, mientras que la señal EMG real, que es más coherente en el tiempo, se preserva. Este proceso de promediación ayuda a mejorar la relación señal-ruido (SNR) en la señal analizada.

Además, el filtrado previo aplicado a la señal (como el filtrado Butterworth) elimina frecuencias no deseadas y ruido de alta frecuencia. Al dividir la señal en ventanas, se tiene la oportunidad de centrarse en las frecuencias de interés en cada segmento. Esto significa que solo las partes de la señal que son relevantes para el análisis se consideran, y el ruido que podría estar presente en otras frecuencias se elimina, resultando en ventanas menos ruidosas.

El aventanamiento también permite realizar análisis espectral en cada ventana individualmente. Dado que el análisis se realiza en segmentos más pequeños y controlados, se puede aplicar la transformada de Fourier de manera más efectiva. Esto significa que se puede observar cómo varía la señal en términos de frecuencia y amplitud, lo que facilita la identificación de patrones de activación muscular mientras se reduce el impacto del ruido.

### Cálculo de la Frecuencia Mediana por Ventana (`calculate_median_frequency`)

La función `calculate_median_frequency` tiene como objetivo calcular la **frecuencia mediana** de cada ventana de datos espectrales obtenidos a través de la Transformada de Fourier. La frecuencia mediana es un valor estadístico que divide el espectro de frecuencia en dos mitades, donde la mitad inferior tiene amplitudes más bajas y la mitad superior tiene amplitudes más altas. Para lograr esto, la función utiliza el cálculo de la suma acumulativa de las amplitudes del espectro para identificar el punto en el que se alcanza la mitad del total.

El proceso comienza calculando la suma acumulativa de las amplitudes del espectro usando `np.cumsum(spectrum)`. A partir de esta suma acumulativa, se determina el índice de la frecuencia mediana buscando el primer índice donde la suma acumulativa sea al menos la mitad de la suma total de las amplitudes. Una vez que se identifica este índice, se almacena la frecuencia correspondiente en una lista llamada `median_frequencies`. Esta medida se convierte en una métrica clave para entender la activación muscular y los cambios asociados a la fatiga.

La frecuencia mediana es particularmente valiosa en el análisis de la señal EMG, ya que proporciona una medida de tendencia central que es menos sensible a valores atípicos en comparación con la media. Al evaluar la frecuencia mediana, se puede obtener información sobre la activación muscular y cómo cambia con el tiempo. Durante situaciones de fatiga, es común observar un cambio en esta frecuencia, lo que permite a los investigadores y profesionales de la salud realizar evaluaciones sobre el estado muscular de un individuo.

### Cálculo de la Mediana por Ventana (`calculate_median_per_window`)

Por otro lado, la función `calculate_median_per_window` se centra en calcular la **mediana** de los valores de voltaje dentro de cada ventana de tiempo de la señal EMG. La mediana es una medida de tendencia central que representa el valor medio de un conjunto de datos ordenados, siendo menos susceptible a la influencia de valores extremos en comparación con la media.

Esta función utiliza una comprensión de lista para extraer la mediana de cada ventana de datos. Al calcular la mediana, se obtiene una representación del comportamiento de la señal en cada segmento de tiempo, lo cual es fundamental para entender la dinámica de la actividad muscular durante las contracciones.

El cálculo de la mediana en las ventanas de señal EMG también permite identificar patrones de activación y variaciones en la actividad muscular. Junto con la frecuencia mediana, estas métricas son esenciales para evaluar cómo responde el músculo a diferentes tipos de esfuerzo y para monitorear la fatiga a lo largo del tiempo.

```c
# --- Cálculo de la frecuencia mediana por ventana ---
def calculate_median_frequency(spectral_data, freqs):
    median_frequencies = []
    for spectrum in spectral_data:
        cumsum = np.cumsum(spectrum)
        median_idx = np.where(cumsum >= cumsum[-1] / 2)[0][0]  # Índice de la frecuencia mediana
        median_frequencies.append(freqs[median_idx])  # Agregar la frecuencia mediana
    return median_frequencies

def calculate_median_per_window(windowed_data):
    medians = [np.median(window) for window in windowed_data]  # Calcula la mediana de cada ventana
    return medians
```

![image](https://github.com/user-attachments/assets/7567db7d-9e27-4e45-8d39-723447126309)


<a name="analisis"></a> 
## Analisis espectral

La función `spectral_analysis` complementa el cálculo de estos parámetros al realizar la Transformada de Fourier (FFT) sobre los segmentos de señal EMG que se han dividido en ventanas. Aquí, se calcula el espectro de amplitudes para cada ventana y se obtienen las frecuencias asociadas utilizando `fftfreq`. Esta transformación permite llevar la señal del dominio del tiempo al dominio de la frecuencia, donde se pueden aplicar las métricas mencionadas anteriormente para describir la actividad muscular de manera más efectiva. La visualización de estos espectros en gráficas permite observar los cambios en la activación muscular en tiempo real, facilitando la identificación de patrones y anomalías en la respuesta del músculo.
```c
# --- Función para realizar el análisis espectral ---
def spectral_analysis(windowed_data, sampling_rate, window_size):
    spectral_data = [np.abs(fft(w))[:window_size // 2] for w in windowed_data]
    freqs = fftfreq(window_size, 1/sampling_rate)[:window_size // 2]
    return spectral_data, freqs
```

![image](https://github.com/user-attachments/assets/29f599a2-3a70-44c1-bbcf-d6a69ca43f66)

![image](https://github.com/user-attachments/assets/aa0ea365-6d7a-4ae8-bc50-007645242777)

![image](https://github.com/user-attachments/assets/a8fedb81-48c9-4bd3-b305-2a12d50edbab)

![image](https://github.com/user-attachments/assets/7e77911b-cac4-47ee-8833-5b43a1f41852)

![image](https://github.com/user-attachments/assets/2ced3ba8-2ad1-44f2-b2c7-a55d962e42cb)

La observación de que, a medida que se pasa de una ventana a otra en el análisis de la señal EMG, se van viendo menos picos en el espectro de frecuencias puede deberse a varios factores relacionados con la fisiología del músculo, la naturaleza de la señal EMG y el proceso de filtrado y análisis espectral.

Una de las explicaciones más comunes es la fatiga muscular. Durante el ejercicio, especialmente cuando se realizan contracciones repetitivas o de larga duración, los músculos tienden a fatigarse. La fatiga se manifiesta en la señal EMG como una disminución en la capacidad de los músculos para generar contracciones fuertes y sostenidas. Esto puede resultar en un menor número de picos en el espectro de frecuencia a medida que se avanza en las ventanas de tiempo. Con el tiempo, los músculos pueden reclutar menos unidades motoras o activar las que están disponibles de manera menos eficiente, lo que se traduce en una disminución de la actividad eléctrica y, por ende, menos picos en la señal.

A medida que se realizan contracciones sucesivas, el patrón de activación muscular puede cambiar. Inicialmente, las contracciones pueden ser más potentes y más sincronizadas, lo que resulta en picos claros en el espectro. Sin embargo, a medida que avanza el tiempo y las contracciones se vuelven menos eficientes, puede haber una menor coordinación entre las fibras musculares, lo que se traduce en una menor amplitud y en la cantidad de picos observados en la frecuencia.

El proceso de análisis espectral mediante la Transformada de Fourier implica calcular la frecuencia de las señales dentro de cada ventana de tiempo. Si las contracciones iniciales son más fuertes y mejor definidas, se observarán más picos. A medida que las ventanas avanzan, si la actividad muscular se vuelve más suave o menos intensa, es probable que el análisis espectral capture menos frecuencias dominantes. Además, cada ventana representa un segmento de tiempo que puede incluir variaciones en la actividad muscular, y si la señal se vuelve más homogénea o menos activa, esto se reflejará en un espectro con menos picos.

Los filtros aplicados durante el procesamiento de la señal también pueden influir en la cantidad de picos que se observan en el espectro. Al aplicar filtros, se eliminan ciertas frecuencias consideradas como ruido, lo que puede hacer que algunos picos en el espectro se atenúen o se eliminen completamente. Si las contracciones posteriores son menos intensas o tienen un rango de frecuencia diferente, el filtrado podría atenuar aún más esas frecuencias, resultando en un espectro con menos picos.

### Cálculo de Parámetros de Frecuencia (`calculate_frequency_parameters`)

La función `calculate_frequency_parameters` es fundamental para el análisis espectral de las señales EMG, ya que permite extraer características clave que describen el comportamiento de la señal en el dominio de la frecuencia. Esta función se centra en calcular tres parámetros importantes: la **frecuencia dominante**, la **frecuencia media** y la **desviación estándar** del espectro de frecuencias. Estos parámetros son esenciales para entender la dinámica muscular y evaluar la fatiga o el rendimiento.

#### Frecuencia Dominante

La **frecuencia dominante** es el valor de frecuencia que corresponde a la amplitud máxima en el espectro de la señal. Para determinarla, se utiliza la función `np.argmax(spectrum)`, que identifica el índice de la amplitud más alta en el espectro. A partir de este índice, se obtiene la frecuencia dominante del espectro, que proporciona información crucial sobre la actividad muscular más intensa durante la contracción. En el contexto de la señal EMG, la frecuencia dominante puede estar relacionada con el tipo de fibras musculares activas y su nivel de activación. Identificar este parámetro es necesario para monitorear la efectividad de los entrenamientos y adaptar las rutinas según la respuesta del músculo a diferentes intensidades de ejercicio.

#### Frecuencia Media

La **frecuencia media** es otra métrica clave que se calcula como un promedio ponderado de las frecuencias, donde las amplitudes del espectro actúan como pesos. Esto se lleva a cabo sumando el producto de cada frecuencia por su correspondiente amplitud en el espectro y dividiendo el resultado por la potencia total del espectro, es decir, la suma de todas las amplitudes. Esta métrica proporciona una visión más completa de la actividad muscular, ya que tiene en cuenta no solo la frecuencia dominante, sino también cómo se distribuyen las amplitudes en todo el espectro. La frecuencia media es especialmente útil para evaluar cambios en la activación muscular a lo largo del tiempo, especialmente en situaciones de fatiga. Cambios en la frecuencia media en las gráficas pueden indicar cómo la fatiga afecta la capacidad de los músculos para generar fuerza, mostrando una tendencia a la disminución de esta frecuencia con el tiempo durante ejercicios prolongados.

#### Desviación Estándar

La **desviación estándar** de las amplitudes en el espectro es otra medida importante que refleja la variabilidad del contenido espectral. Un valor bajo de desviación estándar indica que las amplitudes están concentradas alrededor de la frecuencia dominante, mientras que un valor alto sugiere una mayor dispersión de las amplitudes en el espectro. Esto puede ser indicativo de un comportamiento muscular más variable o menos consistente durante las contracciones. En términos prácticos, la desviación estándar puede ayudar a identificar la estabilidad de la activación muscular y su respuesta a diferentes niveles de esfuerzo. En las gráficas, un aumento en la desviación estándar puede manifestarse como un espectro más amplio, lo que indica que la activación muscular está siendo menos consistente y podría ser un signo de fatiga o ineficacia en la contracción muscular.

```c
# --- Función para calcular la frecuencia dominante, media y desviación estándar --- 
def calculate_frequency_parameters(spectrum, freqs):
    # Frecuencia dominante
    dominant_freq_index = np.argmax(spectrum)
    dominant_frequency = freqs[dominant_freq_index]

    # Frecuencia media
    total_power = np.sum(spectrum)
    weighted_freq_sum = np.sum(freqs * spectrum)
    mean_frequency = weighted_freq_sum / total_power if total_power > 0 else 0

    # Desviación estándar
    std_dev_frequency = np.std(spectrum)

    return {
        'dominant_frequency': dominant_frequency,
        'mean_frequency': mean_frequency,
        'std_dev_frequency': std_dev_frequency
    }

```

![image](https://github.com/user-attachments/assets/cb524516-acb6-41b9-a452-ec6dd586fc6d)

<a name="hipotesis"></a> 
## Prueba de hipotesis

Una prueba de hipótesis es un procedimiento estadístico utilizado para tomar decisiones sobre una afirmación o suposición (la hipótesis) acerca de una población o un proceso, basándose en datos muestrales. Su objetivo principal es determinar si hay suficiente evidencia en los datos para aceptar o rechazar la hipótesis inicial, conocida como hipótesis nula (H0), en favor de una hipótesis alternativa (H1 o Ha). Este proceso es esencial en la investigación y el análisis de datos, ya que permite validar o refutar suposiciones de manera sistemática y objetiva.

Los componentes de una prueba de hipótesis incluyen la hipótesis nula (H0) y la hipótesis alternativa (H1 o Ha). La hipótesis nula es la afirmación que se quiere probar, generalmente representando una posición de "no efecto" o "no diferencia". Por otro lado, la hipótesis alternativa es la afirmación que se acepta si se rechaza la hipótesis nula, representando una posición de "efecto" o "diferencia". Junto a estas hipótesis, se establece un nivel de significancia (α), que define el riesgo de cometer un error tipo I, es decir, rechazar la hipótesis nula cuando es verdadera.


```c
# --- Función para realizar la prueba de hipótesis ---
def hypothesis_test(median_frequencies):
    mid_point = len(median_frequencies) // 2
    first_half = median_frequencies[:mid_point]
    second_half = median_frequencies[mid_point:]
    
    # Realizar prueba de hipótesis t-test para comparar la primera mitad y la segunda mitad
    stat, p_value = ttest_ind(first_half, second_half)
    
    if p_value < 0.05:
        print("Existe una diferencia significativa en la frecuencia mediana, indicando fatiga muscular.")
    else:
        print("No se observó una diferencia significativa en la frecuencia mediana.")
    
    return p_value
```

![image](https://github.com/user-attachments/assets/ddcd3fad-5b35-4871-94dc-641cd7f6f32d)

![image](https://github.com/user-attachments/assets/bbdd6a94-93d6-43e7-ab23-0f955516a7b1)

La prueba de hipótesis se realizo para evaluar la fatiga muscular se basa en la comparación de la frecuencia mediana de dos grupos de datos: la primera mitad y la segunda mitad de las frecuencias medianas calculadas a partir de tu señal EMG. La hipótesis nula (H0) establece que no hay diferencia significativa entre las frecuencias medianas de ambos grupos, mientras que la hipótesis alternativa (H1) sugiere que hay una diferencia significativa.

El resultado de la prueba, que indica un p-valor de aproximadamente 0.753, sugiere que no hay evidencia suficiente para rechazar la hipótesis nula. En términos simples, esto significa que no se observó una diferencia significativa en la frecuencia mediana entre las dos mitades de tu señal EMG. Un p-valor mayor que el nivel de significancia comúnmente utilizado (0.05) indica que las diferencias observadas en las frecuencias medianas podrían ser el resultado de la variabilidad natural en los datos, y no de un efecto real asociado con la fatiga muscular.

En la práctica, esto implica que, según los datos que se han analizado, no se puede concluir que la fatiga muscular haya tenido un efecto significativo en la frecuencia mediana de las contracciones musculares que se registraron en tu señal EMG. Es posible que otros factores, como la variabilidad individual en la señal EMG o el diseño del experimento, hayan influido en los resultados.

<a name="menu"></a> 
## Menu

El menú principal del programa está diseñado para facilitar la interacción del usuario con el análisis de la señal EMG. Cada opción permite realizar una tarea específica relacionada con el procesamiento y análisis de la señal. A continuación, se explican las distintas funcionalidades disponibles en este menú.

### Opción 1: Leer señal desde Excel y mostrar características

Al seleccionar esta opción, el programa carga la señal EMG desde un archivo Excel y presenta información relevante, como la frecuencia de muestreo, la duración total de la señal, la longitud de los datos y el número de contracciones. Luego, se graficará la señal cruda en función del tiempo, permitiendo al usuario visualizar la forma de la señal y tener una mejor comprensión de su contenido.

### Opción 2: Filtrar señal EMG y mostrar parámetros del filtro

Esta opción permite al usuario aplicar filtros pasa altas y pasa bajas a la señal EMG. Se especifican los límites de frecuencia y el orden del filtro. Una vez filtrada la señal, el programa visualiza la señal resultante, destacando el rango de frecuencias que ha sido permitido por el filtro. Esto es útil para eliminar el ruido no deseado y resaltar las características importantes de la señal EMG.

### Opción 3: Aplicar ventana y mostrar su efecto

Al elegir esta opción, se aplica un proceso de aventanamiento a la señal filtrada. La función divide la señal en segmentos o ventanas, y se calcula la mediana de cada una de estas. Los resultados se muestran en la consola, y se grafican tanto la señal filtrada original como las ventanas individuales, lo que ayuda a observar cómo cada ventana captura diferentes características de la señal EMG. Esto es fundamental en el análisis de señales, ya que permite realizar un estudio más detallado en segmentos más pequeños.

### Opción 4: Transformada de Fourier y análisis de frecuencias

En esta opción, el programa realiza una transformada de Fourier sobre las ventanas de la señal filtrada. Esto transforma la señal del dominio del tiempo al dominio de la frecuencia, permitiendo analizar cómo se distribuyen las frecuencias en la señal EMG. Se grafican los espectros de frecuencia de las primeras cinco ventanas y se calculan parámetros como la frecuencia dominante, la frecuencia media y la desviación estándar para cada ventana. Esto ayuda a entender la variabilidad de las frecuencias en la señal y a identificar patrones significativos.

### Opción 5: Prueba de Hipótesis para Fatiga Muscular

Al seleccionar esta opción, el usuario realiza una prueba de hipótesis para evaluar si hay diferencias significativas en la frecuencia mediana entre dos grupos de datos, lo que puede indicar fatiga muscular. Se calcula un p-valor, que se interpreta para determinar si se puede rechazar la hipótesis nula (que no hay diferencia) en favor de la hipótesis alternativa (que sí hay diferencia). El resultado de la prueba se muestra en la consola, ofreciendo información valiosa sobre el estado de fatiga muscular.

### Opción 6: Salir

Finalmente, esta opción permite al usuario salir del programa. Es un paso importante para finalizar la sesión de análisis de datos de manera ordenada.


```c
# --- Menú Principal ---
def main_menu():
    file_path = 'emg_signal_with_fatigue.xlsx'
    sampling_rate = 1000  # Hz

    while True:
        print("\n--- Menú de Laboratorio EMG ---")
        print("1. Leer señal desde Excel y mostrar características")
        print("2. Filtrar señal EMG y mostrar parámetros del filtro")
        print("3. Aplicar ventana y mostrar su efecto")
        print("4. Transformada de Fourier y análisis de frecuencias")
        print("5. Prueba de Hipótesis para Fatiga Muscular")
        print("6. Salir")

        choice = input("Selecciona una opción: ")

        if choice == "1":
            print("Leyendo señal desde archivo Excel...")
            time, data = read_signal_from_excel(file_path)

            # Mostrar detalles de la señal
            duration = len(time) / sampling_rate
            print(f"Frecuencia de muestreo: {sampling_rate} Hz")
            print(f"Duración de la señal: {duration:.2f} segundos")
            print(f"Longitud de la señal: {len(data)} puntos")
            print(f"Contracciones: {10} contracciones")
            plt.figure(figsize=(10, 4))
            plt.plot(time, data, color='purple')
            plt.title('Señal EMG Cruda desde Excel')
            plt.xlabel('Tiempo [s]')
            plt.ylabel('Voltaje [mV]')
            plt.grid(True)
            plt.show()

        elif choice == "2":
            print("Aplicando filtros pasa altas y pasa bajas...")
            _, data = read_signal_from_excel(file_path)
            lowcut = 20.0
            highcut = 450.0
            order = 5
            d2 = data[1:100]
            filtered_data = butter_filter(d2, lowcut, highcut, sampling_rate, order)

            # Mostrar detalles del filtro
            print(f"Filtro pasa banda aplicado con corte de {lowcut}-{highcut} Hz y orden {order}.")

            time = np.linspace(0, len(filtered_data) / sampling_rate, len(filtered_data))
            plt.figure(figsize=(10, 4))
            plt.plot(time, filtered_data, color='blue')
            plt.title('Señal EMG Filtrada (20-450 Hz)')
            plt.xlabel('Tiempo [s]')
            plt.ylabel('Voltaje [mV]')
            plt.grid(True)
            plt.show()

        elif choice == "3":
            print("Aplicando aventanamiento y mostrando el efecto de la ventana...")
            _, data = read_signal_from_excel(file_path)
            filtered_data = butter_filter(data, 20.0, 450.0, sampling_rate)
            windowed_data, window_size = apply_windowing(filtered_data, sampling_rate)
            # Calcular la mediana de cada ventana
            medians = calculate_median_per_window(windowed_data)

            # Mostrar las medianas de las ventanas
            for i, median_value in enumerate(medians):
                print(f"Mediana de la ventana {i+1}: {median_value}")

            # Mostrar la señal antes del aventanamiento
            time = np.linspace(0, len(filtered_data) / sampling_rate, len(filtered_data))
            plt.figure(figsize=(10, 4))
            plt.plot(time, filtered_data, label="Señal Filtrada", color='blue')
            plt.title('Señal Filtrada (antes del aventanamiento)')
            plt.xlabel('Tiempo [s]')
            plt.ylabel('Voltaje [mV]')
            plt.grid(True)
            plt.show()

            # Mostrar las ventanas aplicadas
            for i, windowed_segment in enumerate(windowed_data[:5]):  # Mostramos solo las primeras 5 ventanas
                plt.figure(figsize=(10, 4))
                plt.plot(windowed_segment, label=f"Ventana {i+1}", color='green')
                plt.title(f'Señal Aventanada - Ventana {i+1}')
                plt.xlabel('Muestras')
                plt.ylabel('Voltaje [mV]')
                plt.grid(True)
                plt.show()

        elif choice == "4":
            print("Realizando transformada de Fourier y análisis de frecuencias...")
            _, data = read_signal_from_excel(file_path)
            filtered_data = butter_filter(data, 20.0, 450.0, sampling_rate)
            windowed_data, window_size = apply_windowing(filtered_data, sampling_rate)
            spectral_data, freqs = spectral_analysis(windowed_data, sampling_rate, window_size)

            # Graficar y calcular parámetros para cada ventana
            for i in range(min(5, len(spectral_data))):  # Solo para las primeras 5 ventanas
                plt.figure(figsize=(10, 4))
                plt.plot(freqs, spectral_data[i], color='green')
                plt.title(f'Espectro de Frecuencia - Ventana {i + 1}')
                plt.xlabel('Frecuencia [Hz]')
                plt.ylabel('Amplitud')
                plt.grid(True)
                plt.show()
                
                # Calcular y mostrar parámetros de frecuencia
                frequency_params = calculate_frequency_parameters(spectral_data[i], freqs)
                print(f"Ventana {i + 1}: Frecuencia Dominante: {frequency_params['dominant_frequency']:.2f} Hz, "
                      f"Frecuencia Media: {frequency_params['mean_frequency']:.2f} Hz, "
                      f"Desviación Estándar: {frequency_params['std_dev_frequency']:.2f}")

        elif choice == "5":
            print("Realizando prueba de hipótesis para evaluar la fatiga muscular...")
            _, data = read_signal_from_excel(file_path)
            filtered_data = butter_filter(data, 20.0, 450.0, sampling_rate)
            windowed_data, window_size = apply_windowing(filtered_data, sampling_rate)
            spectral_data, freqs = spectral_analysis(windowed_data, sampling_rate, window_size)

            median_frequencies = calculate_median_frequency(spectral_data, freqs)
            p_value = hypothesis_test(median_frequencies)
            print(f"P-valor de la prueba de hipótesis: {p_value}")

        elif choice == "6":
            print("Saliendo del programa.")
            break

        else:
            print("Opción no válida. Por favor, intenta de nuevo.")

# Ejecutar el menú principal
if __name__ == "__main__":
    main_menu()

```


<a name="contacto"></a> 
## Contacto
* Creado por [Marianitex](https://github.com/Marianitex) - sígueme en mis redes sociales como @_mariana.higuera
