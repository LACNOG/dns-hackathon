# DNS Hackathon



## Objetivo

El objetivo del Hackathon es por un lado plantear una serie de desafíos o actividades operativas en torno al Sistema de Nombres de Dominio (DNS) a realizar a lo largo de la semana, donde cada participante en forma individual o en grupos, vaya completando con la finalidad de mejorar, colaborar o adquirir experiencia en conjunto con el resto de los participantes.

Recuerden que **no es necesario ni obligatorio completar todos los niveles**, lo que esperamos es que puedan elegir los temas que mas les interesan y profundizar en ellos.



------



# Topologia de red de un grupo X [grpX]



![ICANN-LAB-NET-topo](./_pics/grp_network_map.jpg)



```
     EQUIPO          DIRECCION IPv4            DIRECCION IPv6
+--------------+-----------------------+-----------------------------+
| grpX-cli     | 100.100.X.2 (eth0)    | fd99:8bec:X::2 (eth0)       |
+--------------+-----------------------+-----------------------------+
| grpX-ns1     | 100.100.X.130 (eth0)  | fd99:8bec:X:128::130 (eth0) |
+--------------+-----------------------+-----------------------------+
| grpX-ns2     | 100.100.X.131 (eth0)  | fd99:8bec:X:128::131 (eth0) |
+--------------+-----------------------+-----------------------------+
| grpX-resolv1 | 100.100.X.67 (eth0)   | fd99:8bec:X:64::67 (eth0)   |
+--------------+-----------------------+-----------------------------+
| grpX-resolv2 | 100.100.X.68 (eth0)   | fd99:8bec:X:64::68 (eth0)   |
+--------------+-----------------------+-----------------------------+
| grpX-rtr     | 100.64.1.X (eth0)     | fd99:8bec:X::1 (eth1)       |
|              | 100.100.X.65 (eth2)   | fd99:8bec:X:64::1 (eth2)    |
|              | 100.100.X.193 (eth4)  | fd99:8bec:X:192::1 (eth4)   |
|              | 100.100.X.129 (eth3)  | fd99:8bec:X:128::1 (eth3)   |
|              | 100.100.X.1 (eth1)    | fd99:8bec:0:1::X (eth0)     |
+--------------+-----------------------+-----------------------------+
| grpX-soa     | 100.100.X.66 (eth0)   | fd99:8bec:X:64::66 (eth0)   |
+--------------+-----------------------+-----------------------------+
```

Donde en esta práctica **solamente** vamos a acceder a los siguientes equipos:

* **grpX-cli** : cliente
* **grpX-resolv1** y **grpX-resolv2** : servidores recursivos
* **grpX-soa** : servidor autoritativo oculto (primario)
* **grpX-ns1** y **grpX-ns2** : servidores autoritativos secundarios



---



## Niveles

El mismo consta de 6 niveles (3 para servidores DNS Recursos y 3 para servidores DNS Autoritativos):



#### A nivel de Recursivos

1. Configurar dos servidores de DNS recursos uno de ellos utilizando el software BIND y el otro utilizando el software UNBOUND (asegurándoselas en particular de no generar un servidor DNS recurso abierto, limitando el acceso solamente a los bloques de direcciones del Laboratorio).
2. Configurar o verificar que se encuentra funcionando correctamente las extensiones/protocolos validación DNSSEC y QNAME Minimization.
3. Configurar Hyperlocal de la Zona Raíz del DNS a nivel del servidor Recursivo.



#### A nivel de Autoritativos

1. Configurar un servidor Autoritativo siguiendo los lineamientos establecidos para la creación de la Zona, utilizando el software BIND.
2. Configurar otros 2 servidores Autoritativos como secundarios del anterior, estableciendo lo necesario para que dichos secundarios sirvan la zona transfiriendo la misma desde el primario mediante el protocolo XFR. Estos 2 servidores públicos utilizarán los software BIND y NSD respectivamente.
3. Firmar la Zona en el servidor primario utilizando el protocolo DNSSEC siguiendo los criterios establecidos para ello (algoritmo utilizado para la firma, tiempo de validez de la misma, etc).



------



## Directivas para cada nivel



#### A nivel de Recursivos

##### Nivel 1

- Rango de direcciones IPv4 del laboratorio: 100.100.0.0/16
- Rango de direcciones IPv6 del laboratorio: fd99:8bec::/32
- Asegurarse de permitir tanto consultas por IPv4 como por IPv6



##### Nivel 2

- Verificar que este realizando validación DNSSEC (por defecto viene activada tanto en BIND como en UNBOUND)Realizar dos configuraciones diferentes de QNAME Minimization y probarlas:
  Primero configurar QNAME Minimization en modo Estricto



Realizar dos configuraciones diferentes de QNAME Minimization y probarlas de la siguiente manera:

- Configurar en modo Estricto
- Volver a la configuración por defecto en modo Relajado



##### Nivel 3

- Configurar Hyperlocal para la zona Raíz en ambos servidores y verificar realizando consultas DIG a un dominio alojado en la raíz del DNS.



#### A nivel de Autoritativos

##### Nivel 1

El contenido de la zona deberá ser al menos:

```
; grpX 

$TTL    30
@       IN      SOA     grpX.lacnic38-dnshack.te-labs.training. root.example.com (                                            
                              1         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                          86400 )       ; Negative Cache TTL
;

; grpX 
@             NS           ns1.grpX.lacnic38-dnshack.te-labs.training.
@             NS           ns2.grpX.lacnic38-dnshack.te-labs.training.

ns1         A           100.100.X.130
ns1         AAAA        fd99:8bec:X:128::130
ns2         A           100.100.X.131
ns2         AAAA        fd99:8bec:X:128::131

;; SE PUEDEN AGREGAR MAS REGISTROS A GUSTO
```



##### Nivel 2

La dirección IPv4 del servidor Primario a utilizar para configurar la transferencia de zona en los servidores DNS Secundarios es: 100.100.X.66



##### Nivel 3

Para generar la firma DNSSEC de la Zona y luego firmar la misma están disponibles los comandos ***dnssec-keygen*** y ***dnssec-signzone*** respectivamente.

- Para la ZSK utilizar RSASHA256 de 1024 bits.
- Para la KSK utilizar RSASHA256 de 2048 bits.

