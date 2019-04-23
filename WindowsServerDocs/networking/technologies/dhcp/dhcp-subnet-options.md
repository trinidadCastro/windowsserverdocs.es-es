---
title: Opciones de selección de subred DHCP
description: En este tema se proporciona información sobre las opciones de selección de subred DHCP para protocolo de configuración de Dynamic Host (DHCP) en Windows Server 2016.
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-dhcp
ms.topic: get-started-article
ms.assetid: ca19e7d1-e445-48fc-8cf5-e4c45f561607
ms.author: pashort
author: shortpatti
ms.date: 08/17/2018
ms.openlocfilehash: 034ca48ef13a6bdac63ca99ac753fc9826460922
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870816"
---
# <a name="dhcp-subnet-selection-options"></a>Opciones de selección de subred DHCP

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para obtener información sobre las nuevas opciones de selección de subred DHCP.

DHCP ahora admite la opción 82 \(Sub opción 5\). Puede usar estas opciones para permitir que los clientes de proxy DHCP y agentes de retransmisión solicitar una dirección IP para una subred específica y de un intervalo de direcciones IP específico y un ámbito.  Para obtener más información, consulte **opción 82 Sub opción 5**: [Selección del vínculo de RFC 3527 subopciones para la opción de información de agente de retransmisión para DHCPv4](https://tools.ietf.org/html/rfc3527).

Si usa un agente de retransmisión DHCP que está configurado con DHCP opción 82, Sub opción 5, el agente de retransmisión puede solicitar una concesión de dirección IP para los clientes DHCP de un intervalo de direcciones IP específico.


## <a name="option-82-sub-option-5-link-selection-sub-option"></a>Opción 82 subopción 5: Subopción de selección de vínculo

Las subopciones selección del vínculo de agente de retransmisión permite que un agente de retransmisión DHCP especificar una subred IP desde la que el servidor DHCP debe asignar direcciones IP y opciones.

Normalmente, los agentes de retransmisión DHCP se basan en la dirección IP de puerta de enlace \(GIADDR\) campo para comunicarse con servidores DHCP. Sin embargo, GIADDR está limitado por sus dos funciones operativas:

1. Informar al servidor DHCP acerca de la subred en la que reside el cliente DHCP que está solicitando la concesión de dirección IP.
2. Informar al servidor DHCP de la dirección IP se utiliza para comunicarse con el agente de retransmisión.

En algunos casos, la dirección IP que usa el agente de retransmisión para comunicarse con el servidor DHCP podría ser diferente del intervalo de direcciones IP desde el que debe asignarse la dirección IP del cliente DHCP. 

El vínculo selección subopción de opción 82 es útil en esta situación, permitiendo que el agente de retransmisión indicar explícitamente a la subred desde la que desea que la dirección IP asignada en forma de opción de DHCP v4 82 subopción de 5.

> [!NOTE]
>
> Todas las direcciones IP de agente de retransmisión (GIADDR) deben formar parte de un intervalo de direcciones IP de ámbito DHCP activo. Cualquier GIADDR fuera de los intervalos de direcciones IP de ámbito DHCP se considera una retransmisión rogue y servidor DHCP de Windows no reconocerá las solicitudes de cliente DHCP de los agentes de retransmisión.
>
> Puede crearse un ámbito especial para los agentes de retransmisión "autorizar". Crear un ámbito con el GIADDR (o varios si el GIADDR es secuenciales direcciones IP), excluir las direcciones GIADDR de distribución y, a continuación, activar el ámbito. Esto va a autorizar a los agentes de retransmisión y evita que las direcciones GIADDR de que se va a asignar.


### <a name="use-case-scenario"></a>Escenario de caso de uso

En este escenario, una red de la organización incluye un servidor DHCP y un punto de acceso inalámbrico \(AP\) para los usuarios invitados. Las direcciones IP de cliente de los invitados se asignan desde el servidor DHCP de la organización: sin embargo, debido a restricciones de directiva de firewall, el servidor DHCP no puede acceder a la red inalámbrica de invitado o los clientes inalámbricos con broadcase mensajes.

Para resolver esta restricción, el punto de acceso se configura con el vínculo selección Sub opción 5 para especificar la subred desde la que desea que la dirección IP asignada para los clientes de invitado, mientras que en el GIADDR también especifica la dirección IP de la interfaz interna que conduce a la red corporativa.
