---
title: Novedades del DHCP
description: En este tema proporciona información general de las nuevas características de protocolo de configuración de Dynamic Host (DHCP) en Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dhcp
ms.topic: get-started-article
ms.assetid: c6f36998-5b64-45d1-b1f0-0f0d6604dbe3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 73cc5134f7af5063c912ad578fa7d660b3194aa1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840236"
---
# <a name="whats-new-in-dhcp"></a>Novedades del DHCP

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Este tema describe la funcionalidad de protocolo de configuración dinámica de Host (DHCP) nuevas o modificadas en Windows Server 2016.
  
DHCP es un estándar de Internet Engineering Task Force (IETF) diseñada para reducir la carga administrativa y la complejidad de configurar hosts en un TCP/IP\-en función de red, como una intranet privada. Al usar el servicio del servidor DHCP, el proceso de configuración de TCP/IP en clientes DHCP es automático.

Las secciones siguientes proporcionan información sobre las nuevas características y cambios en la funcionalidad de DHCP.

## <a name="dhcp-subnet-selection-options"></a>Opciones de selección de subred DHCP

DHCP es compatible ahora con las opciones 118 y 82 \(Sub opción 5\). Puede usar estas opciones para permitir que los clientes de proxy DHCP y agentes de retransmisión solicitar una dirección IP para una subred específica y de un intervalo de direcciones IP específico y un ámbito.


Si usa un agente de retransmisión DHCP que está configurado con DHCP opción 82, sub\-opción 5, el agente de retransmisión puede solicitar una concesión de dirección IP para los clientes DHCP de un intervalo de direcciones IP específico.

Para obtener más información, consulte [opciones de selección de subred de DHCP](dhcp-subnet-options.md).

## <a name="new-logging-events-for-dns-registration-failures-by-the-dhcp-server"></a>Nuevo registro de eventos para errores de registro DNS por el servidor DHCP

DHCP incluye ahora los eventos de registro para las circunstancias en que DHCP registrar registros de servidor DNS producirá un error en el servidor DNS.

Para obtener más información, consulte [eventos de registro de DHCP para los registros del registro de DNS](dhcp-dns-events.md).

## <a name="dhcp-nap-is-not-supported-in-windows-server-2016"></a>NAP para DHCP no se admite en Windows Server 2016

Protección de acceso de red \(NAP\) está en desuso en Windows Server 2012 R2 y en el rol de servidor DHCP de Windows Server 2016 ya no es compatible con NAP. Para obtener más información, consulte [características o funcionalidades desusadas en Windows Server 2012 R2](https://technet.microsoft.com/library/dn303411.aspx).  
  
Compatibilidad con NAP se introdujo en el rol de servidor DHCP con Windows Server 2008 y se admite en sistemas operativos anteriores a Windows 10 y Windows Server 2016 del cliente y servidor del Windows. En la tabla siguiente se resume la compatibilidad con NAP en Windows Server.  
  
|Sistema operativo|Compatibilidad con NAP|  
|--------------------|---------------|  
| Windows Server 2008 |Se admite|  
| Windows Server 2008 R2 |Se admite|  
| Windows Server 2012 |Se admite|  
| Windows Server 2012 R2 |Se admite|  
| Windows Server 2016|No se admite|  
  
En una implementación de NAP, un servidor DHCP que ejecutan un sistema operativo compatible con NAP puede funcionar como un punto de cumplimiento NAP para el método de cumplimiento NAP para DHCP. Para obtener más información acerca de DHCP en NAP, consulte [lista de comprobación: Implementar un diseño de cumplimiento DHCP](https://technet.microsoft.com/library/dd314186.aspx).  
  
En Windows Server 2016, los servidores DHCP no aplican directivas de NAP y ámbitos DHCP no pueden ser NAP\-habilitado. Los equipos cliente DHCP que también son los clientes NAP envían un informe de mantenimiento \(SoH\) con la solicitud DHCP. Si el servidor DHCP se está ejecutando Windows Server 2016, estas solicitudes se procesan como si no hay SoH está presente. El servidor DHCP concede una concesión DHCP normal al cliente. 

Si los servidores que ejecutan Windows Server 2016 son servidores proxy RADIUS que reenvían las solicitudes de autenticación a un servidor de directivas de red \(NPS\) que es compatible con NAP, NPS como que no son NAP evalúa estos clientes NAP\-capaz, y NAP se produce un error de procesamiento.
  
## <a name="see-also"></a>Vea también  
  
-   [Protocolo de configuración dinámica de Host (DHCP)](Dynamic-Host-Configuration-Protocol--DHCP-.md)  
  

