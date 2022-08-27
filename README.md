# Simon dice

## Instalación

Descargar e instalar el APK [simon dice](./simon_dice.apk), al no ser un desarrollador certificado es necesario permitir la instalación de fuentes desconocidas (te prometo que no mino criptomonedas mientras jugas).

> **_NOTA:_**  Esta aplicación solo puede ser usada en dispositivos Android.

## Desarrollo

Este proyecto se hizo en el contexto de la materia `Introducción a las interacciones hápticas` dictada en la `ECI 2022`.

Se trata de un `Simon Dice` desarrollado con `Android Studio` utilizando `Kotlin`, en el que se intenta reemplazar el estímulo auditivo por uno táctil vibratorio, usando el motor que se encuentra dentro del los teléfonos. 

Para eso se buscó generar 4 estímulos vibratorios que puedan ser fácilmente distinguibles entre si, y que además no generen una mala experiencia en el usuario, es decir. Para esto último, era deseable que el estímulo no sea muy prolongado para evitar que genere molestias.

El objetivo final era que el usuario pudiera no solo distinguir si no también reconocer los distintos estímulos, al punto de que luego de jugar un par de rondas sea posible continuar jugando con los ojos cerrados, tan solo utilizando las vibraciones como input, y conseguir puntajes similares a los conseguidos utilizando solo estímulos auditivos.

Dada la naturaleza del juego, en donde los estímulos se generan de forma constante, tanto los estímulos auditivos como los vibratorios se pueden activar y desactivar a gusto desde la pantalla de inicio ya que no es deseable forzar estos estímulos cuando los mismos están **tan** presentes en la experiencia.

Hubieron limitaciones en el desarrollo, ya que el mismo fue hecho teniendo en cuenta solo a aquellos teléfonos que causan su vibración utilizando motores de masa excéntrica, ya que estos son los teléfonos más comunes y los que se tenían al alcance a la hora del desarrollo. Es por eso que no fue posible jugar con los parámetros intrinsecos de la vibración, como lo son la frecuencia y la amplitud. Para lograr esta distinción solo se pudo variar la duración del estímulo y el patrón vibratorio.

> **_NOTA:_**  Teóricamente es posible controlar la amplitud vibratoria en motores de masa excéntrica mediante el uso de PWM, esto no fue posible durante el trabajo ya que la frécuencia permitada para PWM mediante la API de android era muy baja como para lograr un efecto satisfactorio.

El esquema final para los 4 botones fue el siguiente:

* **Botón rojo:** Vibración simple corta
* **Botón verde:** Vibración simple corta, seguida de una vibración simple larga 
* **Botón azul:** Vibración simple larga, seguida de una vibración simple corta
* **Botón amarillo:** Vibración simple larga

Donde la vibración corta tiene una duración de 50 milisegundos, mientras que la duración de la vibración larga es de 150 milisegundos. Además, en los botones que generan 2 vibraciones el espaciado entre vibraciones es de 75 milisegundos. Dando esto una duración máxima de estímulo vibratorio de 275 milisegundos.

Este estímulo se accionaba en las 2 fases del juego: cada vez que se mostraban los botones a presionar, y cuando el usuario presionaba los mismos.

Por último, cuando el usuario comete un error el juego es finalizado, yendo a la pantalla de inicio de la aplicación y generando una vibración doble larga (250 milisegundos por vibración) separadas por un espacio de 100 milisegundos. Esto es para que el usuario perciba que algo salió mal, en particular que perdió la partida. Este estimulo trata de emular la constante REJECT provista por la API de android, pero que por algún motivo no se consiguió reproducir.
 
## Resultados

El juego fue probado en distintos dispositivos por varias personas, consiguiendo resultados muy diversos.

Para empezar lo primero que se pudo notar es la enorme varianza que hay en los estímulos generados por distintos modelos de celular. Encontrando que para la misma persona en un teléfono la vibración larga era fácilmente distinguible de la corta, mientras que en otro modelo los 2 estímulos eran prácticamente indistinguibles. Se cree que estas diferencias se debían a los distintos modelos de motores de masa excentrica utilizados, ya que algunos tienen más inercia que otros. Además, se notó que en aquellos dispositivos con motores más grandes, donde la vibración era más notable, la capacidad de distinción de estímulos era notablemente mayor que en aquellos con motores pequeños.

Lo segundo que se notó es que los humanos somos muy distintos a la hora de percibir estos estimulos, habiendo individuos con mayor y menor sensibilidad. El caso más extremo fue el de 2 individuos que ante los estímulos del mismo dispositivo tuvieron percepciones casi opuestas. El primero teniendo muchas dificultades para poder distinguir los estímulos de los botones con 2 vibraciones, y el segundo, diferenciandolos con mucha facílidad.

Algo a destacar de este segundo individuo, que a priori pareciera impensado, pero que sirve de forma excelente para marcar la diferencia entre estímulo y percepción, es que el mismo siempre sintió el mismo patrón vibratorio, es decir, para esta persona todas las vibraciones eran iguales. La diferencia que percibía era en el origen de la vibración. Es decir, pensó que para generar los estímulos el teléfono contaba con un motor en cada cuadrante, y que estos se activaban de forma independiente cada vez que su botón era activado.