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

