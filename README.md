# Firmdata

Firmdata es un protocolo de comunicaciones diseñado para comunicar y controlar  dispositivos.

Para que funcione la comunicación entre los dispositivos ambos tiene que incluir la correspondiente librería para utilizar el protocolo.

![Firmdata](https://aprendiendoarduino.files.wordpress.com/2016/03/1_architecture.png?w=624)

Podemos ver a Firmdata como un intermediario entre diferentes entornos y dispositivos.

En la [página de referencia de la librería](https://www.arduino.cc/en/Reference/Firmata) se muestran todas las posibilidades de uso.

En su página de [github](https://github.com/firmata/protocol) se puede encontrar la definición y las [implementaciones](https://github.com/firmata/protocol#firmata-client-libraries) para usarlo entre diferentes plataformas


Veamos ejemplos de uso.

## Control remoto de Arduino

 Desde una aplicación Windows queremos controlar un dispositivo Arduino: Instalamos en la placa Arduino el programar _StandardFirmdata_, que viene incluído en la librería Firmdata y en la añadimos al código de la aplicación Windows la librería Firmadata para el lenguaje correspondiente (python, java, ....). Así desde la aplicación podremos enviar órdenes que la placa Arduino cumplirá. Veamos un [ejemplo en python](https://github.com/jecrespo/Aprendiendo-Arduino/blob/master/Ejercicio38-Firmata/Python_Firmata/blink.py) que hará parpadear el pin 13 10 veces en intervalos de 1 segundo

arduino = PyMata(port, verbose=True)

    for x in range(10):
        print(x + 1)

        arduino.digital_write(BOARD_LED, 1)

        time.sleep(.5)

        arduino.digital_write(BOARD_LED, 0)
        time.sleep(.5)


El programa que tendrá nuestro Arduino resumido sería algo así:

    #include <Firmata.h>


    void setup()
    {
      Firmata.setFirmwareVersion(FIRMATA_MAJOR_VERSION, FIRMATA_MINOR_VERSION);
      ...
      Firmata.begin();
    }

    void loop()
    {
      while (Firmata.available()) {
        Firmata.processInput(); // Si se ha recibido un comando DigitalWrite se hace, si es AnalogWrite se hace....
      }
      for (analogPin = 0; analogPin < TOTAL_ANALOG_PINS; analogPin++) {
        Firmata.sendAnalog(analogPin, analogRead(analogPin)); // Se envían los valores de las entradas analógicas
      }
    }

## [Conectando Arduino on Processing](http://playground.arduino.cc/Interfacing/Processing)

* Instalamos Processing
* Descargamos la [librería de Arduino para Processing](https://github.com/firmata/processing/releases/tag/latest)

La descomprimimos en la carpeta _libraries_ del directorio _sketchbook_. A partir de ahora tendremos ejemplos para usar Arduino con Processing.

Tendremos que instalar en la placa Arduino el ejemplo _StandardFirmdata_ de la librería Firmdata.

En los ejemplos de Processing puede ser necesario modificar la configuración de conexión al puerto de Arduino

  arduino = new Arduino(this, Arduino.list()[0], 57600);

Si tienes problemas para conectar con el puerto adecuado añade estas líneas para saber los puertos disponibles

  import processing.serial.*;
  import cc.arduino.*;
  println(Arduino.list());

# Recursos

[Magnífico tutorial de Firmadata por "AprendiendoArduino"](https://aprendiendoarduino.wordpress.com/2016/03/06/firmata/)
