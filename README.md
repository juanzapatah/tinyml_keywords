# TinyML Keywords

Workshop de clasificación de palabras mediante TinyML


## Hardware requerido

Para implementar el sistema de reconocimiento de palabras con TinyML se requiere algunos componentes de hardware. El objetivo es adquirir la señal de voz, procesarla localmente y ejecutar la inferencia directamente en el microcontrolador.

### Componentes principales

- **ESP32-S3 N16R8**
- **Módulo de micrófono MAX9814**
- Protoboard
- Cables de conexión
- Cable USB para programación y alimentación

### ¿Qué es una señal?

Una **señal** es una representación de cómo cambia una magnitud física con respecto a otra variable, generalmente el tiempo. En el caso del sonido, las variaciones de presión del aire producidas por una fuente sonora, como la voz humana, pueden ser convertidas por un micrófono (**MAX9814**) en una señal eléctrica.

Matemáticamente, una señal continua puede representarse como:

$$
x(t)
$$

donde:

- $x$ representa la amplitud de la señal.
- $t$ representa el tiempo.

Cuando una persona pronuncia una palabra como **"murciélago"** o **"plátano"**, la presión acústica cambia continuamente, generando una forma de onda particular. En este sistema, el micrófono convierte las variaciones acústicas producidas por la voz en una señal eléctrica, la cual será adquirida por el convertidor analógico-digital del **ESP32-S3**.


### Señales analógicas y digitales

Una señal puede representarse de forma **analógica** o **digital**. Una señal analógica presenta variaciones continuas en el tiempo y en amplitud:

$$
x(t)
$$

Sin embargo, los microcontroladores trabajan con valores numéricos discretos. Por esta razón, la señal debe convertirse en una secuencia digital:

$$
x[n]
$$
donde $n$ representa el índice de cada muestra.

El proceso puede representarse como:

```text
Voz
 ↓
Variaciones de presión acústica
 ↓
Micrófono
 ↓
Señal eléctrica analógica
 ↓
ADC
 ↓
Secuencia digital de muestras
```

El **ADC del ESP32-S3** se configura con una resolución de **12 bits**. Por tanto, cada muestra puede representarse aproximadamente mediante valores comprendidos entre:

$$
0 \leq x[n] \leq 4095
$$

Antes de realizar el análisis espectral, se elimina el valor medio de la señal para reducir su componente de continua.


###  Muestreo

El **muestreo** consiste en medir el valor de una señal analógica en instantes regulares de tiempo.

Una señal continua:

$$
x(t)
$$

puede convertirse en una secuencia discreta mediante:

$$
x[n] = x(nT_s)
$$

donde:

- $n$ representa el índice de la muestra.
- $T_s$ representa el periodo de muestreo.

La frecuencia de muestreo se define como:

$$
f_s = \frac{1}{T_s}
$$

y representa el número de muestras adquiridas cada segundo.

En este proyecto se utiliza:

$$
f_s = 16000\ Hz
$$

lo que significa que se adquieren **16.000 muestras por segundo**.


#### Teorema de Nyquist

Para representar adecuadamente una señal, la frecuencia de muestreo debe ser al menos el doble de la frecuencia máxima que se desea analizar:

$$
f_s \geq 2f_{max}
$$

La frecuencia máxima que puede representarse sin ambigüedad se denomina **frecuencia de Nyquist**:

$$
f_N = \frac{f_s}{2}
$$

Para una frecuencia de muestreo de 16 kHz:

$$
f_N = \frac{16000}{2} = 8000\ Hz
$$

Por tanto, pueden representarse teóricamente componentes frecuenciales hasta aproximadamente **8 kHz**.

En este proyecto se utiliza principalmente el intervalo:

$$
80\ Hz \leq f \leq 3000\ Hz
$$

#### Duración del audio

Cada captura tiene una duración de:

$$
T = 1.2\ s
$$

El número total de muestras se obtiene mediante:

$$
N = f_sT
$$

Por tanto:

$$
N = 16000 \times 1.2 = 19200
$$

Cada registro tiene **19.200 muestras de audio**.

### Señales estacionarias y no estacionarias

Una señal se considera aproximadamente **estacionaria** cuando sus propiedades estadísticas o espectrales permanecen constantes durante el intervalo analizado.

La voz humana es una señal **no estacionaria**, debido a que su contenido frecuencial cambia continuamente durante la pronunciación.
Cada segmento presenta diferentes combinaciones de vocales, consonantes y transiciones acústicas. Por esta razón, para reconocer una palabra no es suficiente conocer únicamente qué frecuencias aparecen durante toda la grabación.

También resulta importante conocer: **cómo cambian las frecuencias a lo largo del tiempo.**

Esta característica de la voz requiere el uso de representaciones **tiempo-frecuencia**.


### Transformada de Fourier

La Transformada de Fourier permite representar una señal en términos de componentes sinusoidales de diferentes frecuencias. La idea fundamental es que una señal compleja puede describirse como una combinación de senos y cosenos, o de forma equivalente, mediante exponenciales complejas.

Conceptualmente:

```text
Señal en el tiempo
       ↓
Transformada de Fourier
       ↓
Componentes frecuenciales

#### Resolución frecuencial```

La separación entre los bins del espectro se calcula mediante:

$$
\Delta f = \frac{f_s}{N_{FFT}}
$$

En este proyecto:

$$
f_s = 16000\ Hz
$$

y:

$$
N_{FFT}=512
$$

Por tanto:

$$
\Delta f =
\frac{16000}{512}
=
31.25\ Hz
$$

Cada bin de la FFT representa aproximadamente **31.25 Hz**.
