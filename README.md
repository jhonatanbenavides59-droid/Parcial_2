# Parcial 2 

### Jhonatan Steeevens Benavides Alfonso 
### jsbenavidez@ucompensar.edu.co
### Conmutación y telegráfico 

- Punto uno, parte conceptual
  
¿Qué es NetFlow?

NetFlow es una herramienta que se usa en los equipos de red, como routers y switches, para ver qué tipo de tráfico pasa por la red. un ejemplo es cuando un dispositivo va recopilando información de los paquetes que circulan (dirección IP de origen y destino, puertos, protocolo, cantidad de datos, tiempo) y los agrupa en lo que se llaman flujos, luego, esa información se envía a un servidor o software de análisis, con el fin de monitorear, analizar y entender el uso de la red, con esto lograr identificar cosas como tráfico sospechoso, enlaces saturados, problemas de rendimiento lo que ayuda a tomar decisiones sobre las configuraciones que se tengan dentro de una red.

¿Qué es sFlow?

Es un sistema de monitoreo de tráfico de red que toma muestras de los paquetes que pasan por un dispositivo, por ejemplo 1 de cada 1000, con esto podemos identificar que protocolos se usan más, que host genera más tráfico, si hay alguna anomalía o congestión en la red, lo que resulta útil dentro de una red para identificar posibles fallas o ataques, esta información se envía a un software de análisis, usando este método de muestreo se tiene una ventaja de bajo consumo recursos del equipo de red esto ayuda que funcione bien para enlaces de alta velocidad.

- Diferencia 
El Netflow se enfoca en recolectar la mayor cantidad de información de que circula por un dispositivo, lo que hace que se tenga mayor precisión en los datos recolectados, lo que causa que se tenga un mayor consumo de recursos dentro de la red, mientras que sFlow toma solo algunos paquetes que circulan por el dispositivo con menos información, enfocándose en tener un bajo consumo de recursos pero recolectando información justa que se puede usar en un análisis del estado de red.

- ¿En qué escenario elegiría sFlow sobre NetFlow para detectar “top talkers en un
enlace de 100 Gbps?

En el escenario de tener monitorear un enlace de 100 GB donde se requiere detectar cual es el toptalkers (equipo,usuario,aplicación que más consumo tenga dentro de la red) se usaría sFlows el cual me permitía identificar el toptalkers usando pocos recursos de los dispositivos manteniendo el rendimiento de la red.
