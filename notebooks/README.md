## 🎙️ Interfaz gráfica para adquisición de audios

El proyecto incluye una **interfaz gráfica de escritorio** diseñada para facilitar la adquisición de audios desde el computador.

Esta herramienta permite grabar muestras de voz de forma sencilla, organizarlas por clase y guardarlas localmente en formato `.wav` para utilizarlas posteriormente durante el entrenamiento del modelo.

La interfaz puede utilizarse para construir el conjunto de datos del workshop sin necesidad de trabajar directamente desde código.

Entre sus funciones principales se encuentran:

- selección de la clase que se desea grabar;
- captura de audio mediante el micrófono del computador;
- almacenamiento automático de las grabaciones;
- organización de los archivos por clase;
- generación de archivos `.wav`;
- preparación de los audios para su posterior uso en el notebook de entrenamiento.

El flujo de trabajo es:

```text
Micrófono del PC
      ↓
Interfaz gráfica
      ↓
Seleccionar clase
      ↓
Grabar audio
      ↓
Guardar archivo WAV
      ↓
Organizar por clase
      ↓
Cargar los audios en Google Colab
      ↓
Entrenar el modelo TinyML
```

La interfaz constituye el primer paso práctico del workshop, ya que permite construir el conjunto de datos que posteriormente será procesado mediante el notebook interactivo.

📄 [Ver código de la interfaz gráfica](src/interfaz_adquisicion_audio.py)

> 💡 La interfaz debe ejecutarse localmente en el computador, ya que necesita acceder al micrófono y al sistema de archivos para guardar las grabaciones.


##  Notebook interactivo en Google Colab

El desarrollo completo del workshop puede ejecutarse mediante un notebook interactivo en **Google Colab**.

El notebook guía paso a paso el procesamiento de audio, incluyendo:

- carga y reproducción de señales;
- segmentación en ventanas;
- aplicación de la ventana de Hann;
- FFT y análisis frecuencial;
- construcción del espectrograma;
- extracción de características;
- separación TRAIN / TEST;
- data augmentation;
- entrenamiento de la red neuronal MLP;
- evaluación mediante Exactitud, F1-score y matriz de confusión;
- exportación del modelo para su implementación TinyML.

No es necesario instalar Python ni librerías localmente. Solo se requiere una cuenta de Google y abrir el siguiente enlace:

[![Abrir en Google Colab](https://colab.research.google.com/assets/colab-badge.svg)](AQUI_PEGA_TU_ENLACE_DE_COLAB)

> 💡 Se recomienda ejecutar las celdas del notebook en orden, desde la configuración inicial hasta la evaluación y exportación del modelo.
