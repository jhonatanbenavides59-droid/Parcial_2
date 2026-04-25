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

¿Describa los 5 campos que componen la 5 tuple en un flujo NetFlow?

Los 5 campos son:

1. IP de origen — dirección de capa 3 del host que inicia la comunicación. Puede ser IPv4 (32 bits) o IPv6 (128 bits)
   
2. IP de destino — dirección del host receptor. Junto con la IP origen forma el par de extremos de la conversación. Permite construir matrices de tráfico entre segmentos.
   
3. Puerto de origen — puerto asignado por el SO al proceso cliente, típicamente en el rango 49152–65535. Cambia en cada nueva conexión TCP/UDP, por eso cada sesión genera una entrada distinta en la tabla de flujos.
   
4. Puerto de destino — puerto (443 = HTTPS, 80 = HTTP, 53 = DNS, 22 = SSH). Es el campo más útil para clasificar el tráfico por aplicación sin inspección profunda.
   
5. Protocolo IP — número del protocolo de transporte según IANA. Los más comunes son TCP (6), UDP (17) e ICMP (1). Para ICMP no existen puertos, por lo que los campos 3 y 4 se interpretan como tipo y código ICMP.

- Si se desea medir el consumo de ancho de banda por aplicación (ej HTTP vs SSH), 

Se debe revisar el puesto de destino ya que el cliente puede usar un puerto de origen aleatorio, pero el servidor escucha el puerto destino que ya conoce, por ejemplo HTTP usa el puerto 80 y SSH el puerto 22

- Interprete estos datos ¿Qué indicaría una asimetría extrema entre paquetes
enviados y recibidos entre 192.168.1.10 y 10.0.0.5?

Se puede ver que la cantidad de paquetes que se envían desde 192.168.1.10 es mucho mayor a la que se recibe por parte de 10.0.0.5, esto puede tener varias interpretaciones, un ejemplo de esto puede ser que la primera IP sea un servicio de aplicaciones que envía mucha información y la segunda IP tiene un filtro para que responda solo a ciertos paquetes como confirmación de recibido o un puerto en específico que está configurado con el fin de tener una menor saturación en la red.

Otro escenario que se podría identificar es la falla de un sistema la primera IP envía muchos paquetes y la segunda no responde con una cantidad similar, lo que podría interpretarse como algún ataque de denegación de servicios o que se tiene alguna falla durante el transporte de los quetes lo que causa que no retornen en su totalidad.  

