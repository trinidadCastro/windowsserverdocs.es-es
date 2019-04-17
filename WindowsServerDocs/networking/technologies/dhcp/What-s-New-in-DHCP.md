---
title: Novedades de DHCP
description: Este tema proporciona una descripción general de las nuevas características de protocolo de configuración de dinámico Host (DHCP) en Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dhcp
ms.topic: get-started-article
ms.assetid: c6f36998-5b64-45d1-b1f0-0f0d6604dbe3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f67e5dfd8aefcf408a41f6fc919665419f0ff1c9
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="whats-new-in-dhcp"></a>Novedades de DHCP

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

En este tema se describe la funcionalidad de protocolo de configuración dinámica de Host (DHCP) que es nuevo o cambiado en Windows Server 2016.
  
DHCP es un estándar de ingeniería de Internet (IETF) que está diseñado para reducir la carga administrativa y la complejidad de la configuración de hosts en una red basada en TCP/IP\, como una intranet privada. Mediante el servicio de servidor DHCP, el proceso de configuración de TCP/IP en clientes DHCP es automático.

Las secciones siguientes proporcionan información sobre las nuevas características y los cambios en la funcionalidad de DHCP.

## <a name="dhcp-subnet-selection-options"></a>Opciones de selección de subred DHCP

DHCP ahora admite opciones 118 y 82 \(sub-option 5\). Puedes usar estas opciones para permitir que los clientes de proxy DHCP y agentes de reenvío solicitar una dirección IP una subred específica a partir de un intervalo específico de direcciones IP y del ámbito.

Si estás usando a un cliente de proxy DHCP está configurado con la opción de DHCP 118, como un servidor de red privada virtual \(VPN\) que ejecuta Windows Server 2016 y el rol de servidor de acceso remoto, el servidor VPN puede solicitar una concesión de dirección IP para los clientes VPN de un intervalo de direcciones IP específico.

Si estás usando a un agente que está configurado con la opción de DHCP 82, opción sub\ 5, el agente de retransmisión puede solicitar una concesión de dirección IP para los clientes DHCP de un intervalo de direcciones IP específico.

Para obtener más información, consulta [opciones de selección de subred de DHCP](dhcp-subnet-options.md).

## <a name="new-logging-events-for-dns-registration-failures-by-the-dhcp-server"></a>Nuevos eventos de registro de errores de registro DNS por el servidor DHCP

DHCP ahora incluye registrar eventos de circunstancias en que DHCP de registros servidor DNS producirá un error en el servidor DNS.

Para obtener más información, consulta [DHCP registro de eventos para los registros de registro de DNS](dhcp-dns-events.md).

## <a name="dhcp-nap-is-not-supported-in-windows-server-2016"></a>NAP DHCP no se admite en Windows Server 2016

Red protección de acceso a \(NAP\) está en desuso en Windows Server 2012 R2, y en Windows Server 2016 el rol de servidor DHCP ya no admite NAP. Para obtener más información, consulta [desuso en Windows Server 2012 R2 o quitando características](https://technet.microsoft.com/library/dn303411.aspx).  
  
Compatibilidad con NAP se habilitó en el rol de servidor DHCP con Windows Server 2008 y es compatible con sistemas operativos anteriores a Windows 10 y Windows Server 2016 del cliente y servidor del Windows. La siguiente tabla resume soporte técnico de NAP en Windows Server.  
  
|Sistema operativo|Soporte técnico NAP|  
|--------------------|---------------|  
| Windows Server 2008 |Admite|  
| Windows Server 2008 R2 |Admite|  
| Windows Server 2012 |Admite|  
| Windows Server 2012 R2 |Admite|  
| Windows Server 2016|No es compatible|  
  
En una implementación de NAP, un servidor DHCP que ejecute un sistema operativo que admita NAP puede funcionar como un punto de cumplimiento de NAP para el método de cumplimiento de NAP DHCP. Para obtener más información acerca de DHCP NAP, consulta [lista de comprobación: implementar un diseño de cumplimiento de DHCP](https://technet.microsoft.com/library/dd314186.aspx).  
  
En Windows Server 2016, servidores DHCP no aplicar directivas de NAP y ámbitos DHCP no pueden estar habilitado NAP\. Los equipos de cliente DHCP que también son clientes NAP envían una declaración de estado \(SoH\) con la solicitud DHCP. Si el servidor DHCP ejecuta Windows Server 2016, estas solicitudes se procesan como si no hay ningún informe de mantenimiento. El servidor DHCP concede una DHCP normal para el cliente. 

Si los servidores que ejecutan Windows Server 2016 proxy RADIUS que reenvían solicitudes de autenticación a un \(NPS\) el servidor de directivas de red que admite NAP, estos clientes NAP se evalúan NPS como no compatible con NAP\ y NAP se produce un error de procesamiento.
  
## <a name="see-also"></a>Consulta también  
  
-   [Protocolo de configuración dinámica de Host (DHCP)](Dynamic-Host-Configuration-Protocol--DHCP-.md)  
  

