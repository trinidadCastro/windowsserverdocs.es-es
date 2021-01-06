---
title: Guía de solución de problemas del Protocolo de configuración dinámica de host (DHCP)
description: En este artilce se presenta la guía de solución de problemas de DHCP.
manager: dcscontentpm
ms.date: 5/26/2020
ms.topic: troubleshooting
author: Deland-Han
ms.author: delhan
ms.openlocfilehash: 7d82eaca3f1ab13326b5af442abc0c9232d39122
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97947211"
---
# <a name="troubleshooting-guide-for-dynamic-host-configuration-protocol-dhcp"></a>Guía de solución de problemas del Protocolo de configuración dinámica de host (DHCP)

Para cualquier dispositivo (como un equipo o un teléfono) que pueda funcionar en una red, se le debe asignar una dirección IP. Puede asignar una dirección IP de forma manual o automática. El servicio DHCP (Microsoft o un servidor de terceros) administra la asignación automática.

En este artículo se describen los pasos de solución de problemas generales para el cliente y el servidor DHCP de Microsoft IPv4.

## <a name="more-information"></a>Más información

El procedimiento para la asignación de direcciones IPv4 normalmente implica tres componentes principales:

- Un dispositivo cliente DHCP que tiene que obtener una dirección IP

- Un servicio DHCP que proporciona direcciones IP al cliente en función de la configuración específica

- Un agente de retransmisión DHCP/IP para enviar solicitudes de difusión de DHCP a un segmento de red diferente

Una comunicación de cliente a servidor de DHCP consta de tres tipos de interacción entre los dos pares:

- **Dora basado en difusión** (detectar, oferta, solicitud, confirmación). Este proceso consta de los pasos siguientes:

    - El cliente DHCP envía una solicitud de difusión de detección DHCP a todos los servidores DHCP disponibles dentro del alcance.

    - Se recibe una respuesta de difusión de la oferta DHCP del servidor DHCP, que ofrece una concesión de dirección IP disponible.

    - La solicitud de difusión del cliente DHCP solicita la concesión de la dirección IP ofrecida y la confirmación de difusión DHCP al final.

    - Si el cliente y el servidor DHCP se encuentran en distintos segmentos de red lógica, un agente de retransmisión DHCP actúa como reenviador y envía los paquetes de difusión de DHCP entre ambos equipos del mismo nivel.

- **Solicitudes de renovación de DHCP de unidifusión**: se envían directamente al servidor DHCP desde el cliente DHCP para renovar la asignación de direcciones IP después del 50 por ciento del tiempo de concesión de la dirección IP.

- **Reenlazar solicitudes de difusión DHCP**: se realizan en cualquier servidor DHCP dentro del intervalo del cliente. Estos se envían después del 87,5 por ciento de la duración de la concesión de la dirección IP, ya que esto indica que la solicitud de unidifusión dirigida no funcionó. En lo que se refiere al proceso DORA, este proceso implica una comunicación del agente de retransmisión DHCP.

Si un cliente DHCP de Microsoft no recibe una dirección IPv4 de DHCP válida, es probable que el cliente esté configurado para usar una dirección APIPA. Para obtener más información, vea el siguiente artículo de Knowledge Base: [220874](https://support.microsoft.com/help/220874) How to use Automatic TCP/IP Addressing Without a DHCP Server

Toda la comunicación se realiza en los puertos UDP 67 y 68. Para obtener más información, vea el siguiente artículo de Knowledge Base: [169289](https://support.microsoft.com/help/169289) DHCP (Protocolo de configuración dinámica de host) conceptos básicos.