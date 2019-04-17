---
title: Opciones de selección de subred DHCP
description: Este tema proporciona información sobre las opciones de selección de subred DHCP para protocolo de configuración de dinámica Host (DHCP) en Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dhcp
ms.topic: get-started-article
ms.assetid: ca19e7d1-e445-48fc-8cf5-e4c45f561607
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 43bc3d165f895767ded921b41118ecaccf9734e8
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="dhcp-subnet-selection-options"></a>Opciones de selección de subred DHCP

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este tema para obtener información sobre nuevas opciones de selección de subred DHCP.

DHCP ahora admite opciones 118 y 82 \(sub-option 5\). Puedes usar estas opciones para permitir que los clientes de proxy DHCP y agentes de reenvío solicitar una dirección IP una subred específica a partir de un intervalo específico de direcciones IP y del ámbito.

Si estás usando a un cliente de proxy DHCP está configurado con la opción de DHCP 118, como un servidor de red privada virtual (VPN) que ejecuta Windows Server 2016 y el rol de servidor de acceso remoto, el servidor VPN puede solicitar concesiones de direcciones IP para los clientes VPN de un intervalo de direcciones IP específico.

Si estás usando un agente que está configurado con la opción de DHCP 82, opción inferior 5, el agente de transmisión puede solicitar una concesión de dirección IP para los clientes DHCP de un intervalo de direcciones IP específico.

Siguiente es vínculos a la solicitud para temas de comentarios para estas opciones.

- **Opción 118**: [opción de selección de RFC 3011 IPv4 subred de DHCP](http://www.rfc-base.org/rfc-3011.html)
- **Opción de la opción 82 Sub 5**: [opción secundarias de RFC 3527 vínculo selección de la opción de la información de agente de transmisión para DHCPv4](https://tools.ietf.org/html/rfc3527)


## <a name="dhcp-option-118-client-subnet-and-relay-agent-link-selection"></a>Opción de DHCP 118: Subred del cliente y selección de vínculo retransmisión agente

La opción de selección de subred DHCP proporciona un mecanismo para servidores proxy DHCP para especificar una subred IP desde el que el servidor DHCP debe asignar direcciones IP y las opciones.

### <a name="use-case-scenario"></a>Escenario de uso

En este escenario, un servidor de red privada virtual \(VPN\) asigna direcciones IP a los clientes VPN. 

En este caso, el servidor VPN está conectado a ambas intranet donde está instalado el servidor DHCP y a Internet, para que los clientes VPN puedan conectarse al servidor VPN desde ubicaciones remotas.

Antes de que el servidor VPN puede proporcionar concesiones de direcciones IP a los clientes VPN, el servidor se pone en contacto con el servidor DHCP en una subred interna y reserva un bloque de direcciones IP. El servidor VPN, a continuación, administra las direcciones IP que obtuvo el servidor DHCP. Si el servidor VPN todas las direcciones IP reservadas en concesiones proporciona a los clientes VPN, el servidor VPN, a continuación, obtiene direcciones IP adicionales desde el servidor DHCP.

Al configurar el servidor VPN con la opción de DHCP 118, puedes especificar el intervalo de direcciones IP y ámbito que quieres usar para los clientes VPN. Después de configurar la opción 118, el servidor VPN solicita direcciones IP de un intervalo específico de direcciones IP y del ámbito desde el servidor DHCP.

### <a name="the-dhcp-subnet-selection-option-field"></a>El campo de opción de selección de subred DHCP

El campo de opción de selección de subred DHCP contiene una sola IPv4 dirección se usa para representar la dirección de subred de origen para una solicitud de concesión DHCP.  Un servidor DHCP que está configurado para responder a esta opción, asigna la dirección desde:

1. La subred especificada en la opción de selección de subred.
2. Una subred que se encuentra en el mismo segmento de red que la subred especificada en la opción de selección de subred.

## <a name="option-82-sub-option-5-link-selection-sub-option"></a>82 Sub opción 5: Opción de suscripción de selección de vínculo

La opción de subpropiedades de selección del vínculo de agente de transmisión permite un agente especificar una subred IP desde el que el servidor DHCP debe asignar direcciones IP y las opciones.

Por lo general, agentes de retransmisión DHCP se basan en el campo de la dirección IP de puerta de enlace \(GIADDR\) para comunicarse con los servidores DHCP. Sin embargo, GIADDR está limitado por sus funciones operativas dos:

1. Para informar al servidor DHCP acerca de la subred en el que se encuentra el cliente DHCP que está solicitando la concesión de dirección IP.
2. Para informar al servidor DHCP de la dirección IP para usar para comunicarse con el agente de transmisión.

En algunos casos, la dirección IP que el agente de transmisión que se usa para comunicarse con el servidor DHCP puede ser distinto que el intervalo de direcciones IP desde el que debe asignarse la dirección IP del cliente DHCP. 

Agentes de retransmisión DHCP no pueden hacer uso de la opción 118, como su funcionalidad es limitada y puede solo escritura en el campo GIADDR o la opción de información de agente de transmisión de \(option 82\). 

La opción de suscripción de selección del vínculo de opción 82 es útil en esta situación, que permite al agente de transmisión indicar explícitamente la subred desde el que desea la dirección IP asignada en forma de la opción de DHCP v4 opción 82 de sub 5.

### <a name="use-case-scenario"></a>Escenario de uso

En este escenario, la red de una organización incluye un servidor DHCP y un punto de acceso inalámbrico \(AP\) para los usuarios de invitado. Se asignan direcciones IP de cliente invitados desde el servidor DHCP de organización; sin embargo, debido a restricciones de directiva de firewall, el servidor DHCP no puede acceder a la red inalámbrica de invitado o clientes inalámbricos con broadcase mensajes.

Para resolver esta restricción, se configura el punto de acceso con el vínculo selección Sub opción 5 para especificar la subred desde el que desea la dirección IP asignada para clientes de invitado, mientras que en la GIADDR también especifica la dirección IP de la interfaz interna que conduce a la red corporativa.
