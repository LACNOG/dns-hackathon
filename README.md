# DNS Hackathon



## Objetivo

El objetivo del Hackathon es por un lado plantear una serie de desafíos o actividades operativas en torno al Sistema de Nombres de Dominio (DNS) a realizar a lo largo de la semana, donde cada participante en forma individual o en grupos, vaya completando con la finalidad de mejorar, colaborar o adquirir experiencia en conjunto con el resto de los participantes.



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