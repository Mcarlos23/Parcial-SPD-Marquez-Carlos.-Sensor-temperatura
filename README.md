# Sistema de incendio con Arduino
## Segundo Parcial

Ejercicio de simulación de recorrido de estaciones de Subte con indicación sonora. 

![](https://github.com/Mcarlos23/Parcial-SPD-Marquez-Carlos.-Sensor-temperatura/blob/main/parcial_arduino.jpg?raw=true)

## Comenzando 🚀

Estas instrucciones te permitirán comprender el funcionamiento del proyecto. Además, se incluye el enlace hacia TinkerCad para poder ver el proyecto armado con sus distintos componentes y poner el código en ejecución. 

Ve a la sección de **link al proyecto** para ver el código del proyecto.

## Consigna 🔩

El objetivo de este proyecto es diseñar un sistema de incendio utilizando Arduino que pueda detectar cambios de temperatura y activar un servo motor en caso de detectar un incendio. Además, se mostrará la temperatura actual y la estación del año en un display LCD.

El sistema debe ser capaz de medir la temperatura utilizando el sensor de temperatura y mostrar el valor actual en el display LCD.
El sistema debe tener la capacidad de detectar cambios de temperatura y activar el servo motor en caso de detectar un incendio. En caso de activación, los LEDs deben indicar el estado de alerta.
Se debe implementar la funcionalidad de control remoto IR para encender y apagar el sistema.
El display LCD debe mostrar la estación del año actual.


## Funcionalidad principal 🔩

* * *

 ~~~ C++ 
bool encenderYApagarSistema()
{
  if (IrReceiver.decode())  // Verifica si hay una señal IR decodificada
  {
    unsigned long hexValue = IrReceiver.decodedIRData.decodedRawData;  // Obtiene el valor de la lectura
    
    if (hexValue == CODIGO_ENCENDIDO)  // Compara el valor hexadecimal con el código para enceder.
    {
       IrReceiver.resume();  // Se reanuda la recepción de señales IR para recibir la siguiente señal
       return true;  // Devuelve true si el valor hexadecimal coincide
    }
    
    IrReceiver.resume();  // Se reanuda la recepción de señales IR para recibir la siguiente señal
  }
  
  return false;  // Devuelve false si no se encuentra el valor hexadecimal esperado o no hay una señal IR decodificada
}
~~~

~~~
void validarTemperatura(int temperatura)
{
  mi_pantalla.setCursor(4, 0);
  mi_pantalla.print("grados");
  if (-40 <= temperatura && temperatura <= 12)
  {
    mi_pantalla.setCursor(0, 0);
    mi_pantalla.print(temperatura);
    mi_pantalla.setCursor(0, 1);
    mi_pantalla.print("invierno");
  } 
  else if (13 <= temperatura && temperatura <= 18)
  {
    mi_pantalla.setCursor(0, 0);
    mi_pantalla.print(temperatura);
    mi_pantalla.setCursor(0, 1);
    mi_pantalla.print("otoño");
  } 
  else if (19 <= temperatura && temperatura <= 24)
  {
    mi_pantalla.setCursor(0, 0);
    mi_pantalla.print(temperatura);
    mi_pantalla.setCursor(0, 1);
    mi_pantalla.print("primavera");
  } 
  else if (25 <= temperatura && temperatura <= 59)
  {
    mi_pantalla.setCursor(0, 0);
    mi_pantalla.print(temperatura);
    mi_pantalla.setCursor(0, 1);
    mi_pantalla.print("verano");
  }
  else
  {
    mi_pantalla.setCursor(0, 0);
    mi_pantalla.print(temperatura);
    mi_pantalla.setCursor(0, 1);
    mi_pantalla.print("ALERTA! incendio!");
    indicarIncendio();
  }
    
}
~~~

~~~
void indicarIncendio()
{
	digitalWrite(LED_VERDE, LOW);
    encenderServo();
  	
  	
}

void encenderServo()
{
  digitalWrite(LED_VERDE, LOW);
  while (true)
  {
    bool banderaLedRojo = true;
    for (pos = 0; pos <= 180; pos += 5)  // Mueve el servo de 0 a 180 grados
    {
      miServo.write(pos);
      delay(500);
      digitalWrite(LED_ROJO, banderaLedRojo);
      delay(15);  // Pequeña pausa para dar tiempo al servo a llegar a la posición deseada
      banderaLedRojo = !banderaLedRojo;
    }

    for (pos = 180; pos >= 0; pos -= 5)  // Mueve el servo de 180 a 0 grados
    {
      miServo.write(pos);
      digitalWrite(LED_ROJO, banderaLedRojo);
      delay(15);  // Pequeña pausa para dar tiempo al servo a llegar a la posición deseada
      banderaLedRojo = !banderaLedRojo;
    }
  }
  
}
 ~~~

La funcionalidad de mi aplicación se divide en 3 funciones principales que trabajan en conjunto en el bucle principal.

1. **encenderYApagarSistema**: Esta función se encarga de validar la lectura del sensor infrarrojo y compararla con el valor del código de encendido del control remoto infrarrojo que estamos utilizando. Si se presiona el botón correspondiente, se cambiará una bandera para activar nuestro sistema de monitoreo de temperatura.

2. **validarTemperatura**: Esta función recibe un valor de temperatura como parámetro y determina la estación del año correspondiente en base a los valores recibidos. Luego, muestra en el display la estación y el valor de la temperatura. Además, tiene la capacidad de detectar incendios si la temperatura supera los 60ºC.

3. **indicarIncendio** y **encenderServo**: Estas funciones son invocadas si la temperatura supera los 60ºC, indicando la presencia de un incendio. Modifican el comportamiento de los LEDs y activan el servo para indicar la activación de la alarma de incendios.

Estas tres funciones trabajan en conjunto para brindar un sistema de detección de incendios y monitoreo de temperatura eficiente.


## Funcionamiento

El proyecta cuenta con los siguientes componentes:

* Arduino UNO
* Sensor de temperatura
* Control remoto IR (Infrarrojo)
* Display LCD (16x2 caracteres)
* Servo motor
* Cables y resistencias según sea necesario
* Protoboard para realizar las conexiones
* Dos LEDs

Para el proyecto, utilizaremos las siguientes bibliotecas:

LiquidCrystal_I2C: Esta biblioteca nos proporciona funcionalidades para trabajar con el display LCD utilizando el bus I2C. Nos permite controlar y mostrar información en el display de manera sencilla.

Wire: Esta biblioteca se utiliza para establecer la comunicación a través del bus I2C. Nos permite enviar y recibir datos entre Arduino y otros dispositivos conectados al bus.

IRremote: Esta biblioteca nos permite utilizar el sensor infrarrojo. Con ella, podemos recibir y decodificar señales infrarrojas, lo que nos permitirá implementar la funcionalidad de control remoto en nuestro proyecto.

Servo: Esta biblioteca nos brinda las funciones necesarias para controlar un servo motor. Podremos especificar el ángulo de giro deseado y la biblioteca se encargará de enviar las señales adecuadas para posicionar el servo en la posición correspondiente.



## 🤖 Link al proyecto 
Este [enlace](https://www.tinkercad.com/things/4yYIVws6hPI-2do-parcial-div-g-marquez-carlos/editel?sharecode=EbeIaLyI47BLs6kk0TyKKmer-IqWiAZ9AMNZkfqCHc0) te llevará al proyecto en TinkerCad.

### Autor ✒️

* **Carlos Marquez** 





⌨️ con ❤️ por Carlos Marquez, DIV G, SPD 2023.
