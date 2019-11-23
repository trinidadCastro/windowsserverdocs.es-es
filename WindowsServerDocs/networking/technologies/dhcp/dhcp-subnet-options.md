---
title: Opciones de selección de subred DHCP
description: En este tema se proporciona información sobre las opciones de selección de subred DHCP para el protocolo de configuración dinámica de host (DHCP) en Windows Server 2016.
manager: dougkim
ms.prod: windows-server
ms.technology: networking-dhcp
ms.topic: get-started-article
ms.assetid: ca19e7d1-e445-48fc-8cf5-e4c45f561607
ms.author: pashort
author: shortpatti
ms.date: 08/17/2018
ms.openlocfilehash: 4718204fad49b23c84cc73b67164f34a803ddd86
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405754"
---
# <a name="dhcp-subnet-selection-options"></a>Opciones de selección de subred DHCP

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para obtener información acerca de las nuevas opciones de selección de subred DHCP.

DHCP ahora admite la opción 82 \(sub-Option 5\). Puede usar estas opciones para permitir que los clientes proxy DHCP y los agentes de retransmisión soliciten una dirección IP para una subred específica y desde un ámbito y un intervalo de direcciones IP específicos.  Para obtener más información, consulte **option 82 sub Option 5**: [RFC 3527 Link Selection sub-Option para la opción de información del agente de retransmisión para DHCPv4](https://tools.ietf.org/html/rfc3527).

Si usa un agente de retransmisión DHCP que está configurado con la opción 82 de DHCP, subopción 5, el agente de retransmisión puede solicitar una concesión de dirección IP a los clientes DHCP de un intervalo de direcciones IP específico.


## <a name="option-82-sub-option-5-link-selection-sub-option"></a>Option 82 sub Option 5: opción Link Selection sub

La subopción de selección de vínculo del agente de retransmisión permite que un agente de retransmisión DHCP especifique una subred IP desde la que el servidor DHCP debe asignar las direcciones IP y las opciones.

Normalmente, los agentes de retransmisión DHCP se basan en la dirección IP de puerta de enlace \(campo GIADDR\) para comunicarse con los servidores DHCP. Sin embargo, GIADDR está limitado por sus dos funciones operativas:

1. Para informar al servidor DHCP acerca de la subred en la que reside el cliente DHCP que solicita la concesión de la dirección IP.
2. Para informar al servidor DHCP de la dirección IP que se va a utilizar para comunicarse con el agente de retransmisión.

En algunos casos, la dirección IP que usa el agente de retransmisión para comunicarse con el servidor DHCP puede ser diferente del intervalo de direcciones IP desde el que se debe asignar la dirección IP del cliente DHCP. 

La opción de selección de vínculo de la opción 82 es útil en esta situación, lo que permite que el agente de retransmisión indique explícitamente la subred de la que desea que se asigne la dirección IP con la opción de DHCP V4 82 sub Option 5.

> [!NOTE]
>
> Todas las direcciones IP del agente de retransmisión (GIADDR) deben formar parte de un intervalo de direcciones IP de ámbito DHCP activo. Cualquier GIADDR fuera de los intervalos de direcciones IP del ámbito DHCP se considera una retransmisión no autorizada y el servidor DHCP de Windows no aceptará las solicitudes de cliente DHCP de esos agentes de retransmisión.
>
> Se puede crear un ámbito especial para autorizar a los agentes de retransmisión. Cree un ámbito con el GIADDR (o varios si los GIADDR son direcciones IP secuenciales), excluya las direcciones GIADDR de la distribución y, a continuación, active el ámbito. Esto autorizará a los agentes de retransmisión y evitará que se asignen las direcciones GIADDR.


### <a name="use-case-scenario"></a>Escenario de caso de uso

En este escenario, una red de la organización incluye un servidor DHCP y un punto de acceso inalámbrico \(\) de AP para los usuarios invitados. Las direcciones IP de cliente de invitados se asignan desde el servidor DHCP de la organización; sin embargo, debido a las restricciones de la Directiva de firewall, el servidor DHCP no puede tener acceso a la red inalámbrica invitada ni a los clientes inalámbricos con mensajes broadcase.

Para resolver esta restricción, el AP se configura con la opción de selección de vínculos 5 para especificar la subred de la que desea que se asigne la dirección IP a los clientes invitados, mientras que en GIADDR también se especifica la dirección IP de la interfaz interna que conduce al red corporativa.
