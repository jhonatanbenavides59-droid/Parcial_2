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

# Parte de diseño 

## Diagrama de camara que simula trafico de personas y vehiculos

  <img width="3257" height="2452" alt="Diagrama 1" src="https://github.com/user-attachments/assets/a23695e4-95a1-4c63-9f40-03f0d0092dfa" />
  
- ¿Cómo comunicaría el contenedor YOLO con la VM para que el tráfico de detecciones sea muestreado por NetFlow?
  
El contenedor que ejecuta YOLO debe enviar los resultados de detección a la VM utilizando tráfico de red estándar, ya que NetFlow solo puede analizar paquetes IP que atraviesan una interfaz de red, La comunicación se establece haciendo que el contenedor Docker y la VM estén conectados a la misma red virtual, El tráfico pasa por el switch virtual (Linux bridge docker0), donde softflowd lo captura y exporta flujos NetFlow al colector.
de modo que el tráfico fluya entre direcciones IP distintas. La VM actúa como servidor receptor de las detecciones y, al mismo tiempo, ejecuta un exportador NetFlow (como softflowd) sobre la interfaz por donde entra ese tráfico. Esto garantiza que los paquetes enviados desde el contenedor YOLO hacia la VM atraviesen una interfaz monitoreada.

- Proponga una regla de IP Accounting en el router virtual (p ej iptables o nftables) para medir el tráfico entre la subred del contenedor y la VM
  
Una regla que se podría usar con iptables, es agregar una entrada para permitir el tráfico entre esas redes y dejar que el contador de paquetes y bytes de la regla registre los paquetes transportados, esta regla no modifica el flujo de datos, sino que aprovechan los contadores internos de iptables para llevar un registro acumulado de tráfico. Luego, usando comandos como iptables -L -v -n, se pueden consultar los paquetes y bytes asociados a esa comunicación específica, ejemplo de la configuración de la regla:

- iptables -A FORWARD -s 172.17.0.0/16 -d 10.0.0.100 -j ACCEPT
- iptables -A FORWARD -s 10.0.0.100 -d 172.17.0.0/16 -j ACCEPT

## Diagrama de estación de trenes 

<img width="3257" height="2452" alt="Diagrama estacion tren" src="https://github.com/user-attachments/assets/152cb32f-8d92-4e99-b80f-3904010982ed" />

- Tabla de IP de contenedores
  

<img width="905" height="175" alt="image" src="https://github.com/user-attachments/assets/0d368867-0262-47d0-a351-0fe1706d45a4" />

- Flujos de datos
- 

<img width="466" height="88" alt="image" src="https://github.com/user-attachments/assets/630ef708-61bc-4d16-8286-ec6935f0f908" />


- Teniendo en cuenta los siguientes datos:
Video procesado:30 fps, 640x480, JPEG comprimido → 50 frame. Enviado por UDP.
Metadata JSON: 200 bytes/detección, 10 detecciones/s. Enviado por TCP.

- Calcule el throughput (Mbps) por contenedor para video y metadata, Throughput total de los 5 contenedores

Por contenedor:

  Video    = 30 fps × 50 KB/frame × 8 = 12.0 Mbps (UDP)
  Metadata = 10 det/s × 200 B × 8     = 0.016 Mbps (TCP)
  Total    ≈ 12.016 Mbps por contenedor

Total 5 contenedores:
  5 × 12.016 Mbps ≈ 60.08 Mbps
  
  Red requerida: ≥ 100 Mbps


- La red tiene latencia base 2 ms y jitter 1 ms. ¿Qué protocolo (UDP o TCP) es más adecuado para el video? Justifique. ¿Cómo mitigaría el jitter en el receptor?

Con una latencia base de 2 ms y un jitter de 1 ms, el protocolo más adecuado para el transporte de video en tiempo real es UDP no implementa mecanismos de control de congestión ni retransmisión de paquetes perdidos, lo que permite una entrega más rápida y continua de los datos, Para mitigar el jitter en el receptor, se usaría búfer de jitter. Este búfer almacena temporalmente los paquetes entrantes y los reproduce a un ritmo constante, compensando las variaciones en el tiempo de llegada

## Parte empirica

Se realiza la configuración del Google Colab para que se ejecute la captura de datos usando un video de test con YOLO que logre realizar tráfico para análisis el funcionamiento de las herramientas y de los protocolos que se usan

<img width="409" height="268" alt="Paso 1" src="https://github.com/user-attachments/assets/bc67aa5c-78d6-49a9-a7e7-dec4ac225e74" />

Para este paso se usa una herramienta TCPdump para capturar datos UDP que por el puerto 5555

<img width="469" height="260" alt="Paso 1_1" src="https://github.com/user-attachments/assets/b519012b-9375-40c0-84f8-f037ab247aba" />

Con esto se genera un archivo llamado captura_udp-pcap para que se pueda analizar 

<img width="344" height="211" alt="Paso 1_2" src="https://github.com/user-attachments/assets/7a0caf3e-f7b9-4da4-8cfa-eafad16b9238" />

Al final se tiene una tabla con el resumen del resultado que se obtuvo del del 5 – tuple

Se instalan las herramientas tcpdump, nfdump y nflow-gen para poder realizar un análisis más profundo

<img width="452" height="112" alt="image" src="https://github.com/user-attachments/assets/01f27d2b-44ad-4cec-bb40-2588ae526344" />

Continuamos con flujos NetFlow usando la herramienta nfdump, donde se observa direcciones IP y métricas de tráfico UDP (protocolo 17) desde la IP de origen 127.0.0.1 hacia la IP de destino 127.0.0.1, usando el puerto origen 41678 y el puerto destino 5555; durante este intervalo se contabilizan 20 paquetes que suman aproximadamente 1180 bytes.


<img width="597" height="536" alt="image" src="https://github.com/user-attachments/assets/2f8a7dd8-ab60-4725-a7cd-3c2a648459de" />

El resultado indica que, en el intervalo de tiempo estudiado, no existen otras IPs relevantes y que todo el tráfico registrado proviene de un único origen, facilitando así la identificación del Top Talker

<img width="1642" height="567" alt="image" src="https://github.com/user-attachments/assets/4e7a876e-2b36-455a-a943-585f6ef6ff02" />


<img width="639" height="530" alt="image" src="https://github.com/user-attachments/assets/cdb03f10-c82a-4535-a7b5-6470b6137569" />


La imagen describe el Paso 4 del proceso: IP Accounting usando iptables, donde se simula el comportamiento de un router Cisco para contabilizar tráfico de red, luego se crea una regla específica en la cadena INPUT para aceptar tráfico UDP dirigido al puerto 5555, que corresponde al servicio receptor de las detecciones; esta regla no bloquea paquetes, sino que permite que iptables cuente automáticamente los paquetes y bytes que coinciden

<img width="551" height="351" alt="image" src="https://github.com/user-attachments/assets/b65313c5-11ad-481e-8cd9-514e587eae8a" />











