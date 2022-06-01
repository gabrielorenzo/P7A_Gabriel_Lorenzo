# PRACTICA 7 : Buses de comunicación III (I2S)

## A:  Reproducción desde Memoria Interna

### Código

```cpp
#include "Arduino.h" 
#include "FS.h"
#include "HTTPClient.h"
#include "SPIFFS.h"
#include "SD.h"
#include "AudioGeneratorAAC.h"
#include "AudioOutputI2S.h"
#include "AudioFileSourcePROGMEM.h"
#include "sampleaac.h"

AudioFileSourcePROGMEM *in;
AudioGeneratorAAC *aac;
AudioOutputI2S *out;

void setup(){
  Serial.begin(9600);

  in = new AudioFileSourcePROGMEM(sampleaac, sizeof(sampleaac));
  aac = new AudioGeneratorAAC();
  out = new AudioOutputI2S();
  out -> SetGain(0.125);
  out -> SetPinout(26,25,22);
  aac->begin(in, out);
}

void loop(){
  if (aac->isRunning()) {
    aac->loop();
  } else {
    aac -> stop();
    Serial.printf("Sound Generator\n");
    delay(1000);
  }
}
```

### Funcionamiento del programa

En primer lugar, haremos uso de todas estas librerías, para ello deberemos añadir la librería de la biblioteca de audio ESP8266 de Earle F.Philhower. También tendremos que declarar tres punteros, el primero "AudioFileSourcePROGMEM"; su función es leer un archivo de una matriz PROGREM. El segundo puntero "AudioGeneratorAAC"; traga de reproducir un archivo AAC mono o estéreo.Por último el puntero "AudioOutputI2" que envía señales estéreo o mono a cualquier frecuencia prestablecida.

```cpp
#include "Arduino.h" 
#include "FS.h"
#include "HTTPClient.h"
#include "SPIFFS.h"
#include "SD.h"
#include "AudioGeneratorAAC.h"
#include "AudioOutputI2S.h"
#include "AudioFileSourcePROGMEM.h"
#include "sampleaac.h"

AudioFileSourcePROGMEM *in;
AudioGeneratorAAC *aac;
AudioOutputI2S *out;

```

A continuación definimos nuestra función Setup, en primer lugar vamos a inicializar el puerto série. Ahora, declaramos el punetro "in", declarado previamente, como una variable que leerá el fichero de audio. El punetro "aac", se encargará de la comunicación entre el decodificador y la placa. Por último, el puntero out, se encargará de las diferentes configuraciones como la ganancia o los pines de salida.

```cpp
void setup(){
  Serial.begin(9600);

  in = new AudioFileSourcePROGMEM(sampleaac, sizeof(sampleaac));
  aac = new AudioGeneratorAAC();
  out = new AudioOutputI2S();
  out -> SetGain(0.125);
  out -> SetPinout(26,25,22);
  aac->begin(in, out);
}
```

Finalmente, el bloque Loop se encarga de comprobar si hay algun error y si ha leido el fichero correctamente con la sentencia "isRunning". Si no hay ningún error, se reproduce el audio. En caso de que hubiera un error, se cerraría el fichero.

```cpp
void loop(){
  if (aac->isRunning()) {
    aac->loop();
  } else {
    aac -> stop();
    Serial.printf("Sound Generator\n");
    delay(1000);
  }
}
```

### Salida por Terminal 

Cada vez que se reproduzca, veremos lo siguiente:

```cpp
Sound Generator
```
