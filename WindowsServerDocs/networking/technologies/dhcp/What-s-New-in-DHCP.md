---
title: Novedades del DHCP
description: En este tema se proporciona información general sobre las nuevas características del Protocolo de configuración dinámica de host (DHCP) en Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dhcp
ms.topic: get-started-article
ms.assetid: c6f36998-5b64-45d1-b1f0-0f0d6604dbe3
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 58d849fa1003148b034cc426817b97d3a70d4421
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80312693"
---
# <a name="whats-new-in-dhcp"></a>Novedades del DHCP

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema se describen las funciones del Protocolo de configuración dinámica de host (DHCP) nuevas o modificadas en Windows Server 2016.
  
DHCP es un estándar de Internet Engineering Task Force (IETF) diseñado para reducir la carga administrativa y la complejidad de configurar hosts en una red TCP/IP basada en\-, como una intranet privada. Al usar el servicio del servidor DHCP, el proceso de configuración de TCP/IP en clientes DHCP es automático.

En las secciones siguientes se proporciona información acerca de las nuevas características y los cambios en la funcionalidad de DHCP.

## <a name="dhcp-subnet-selection-options"></a>Opciones de selección de subred DHCP

DHCP ahora admite las opciones 118 y 82 \(subopción 5\). Puede usar estas opciones para permitir que los clientes proxy DHCP y los agentes de retransmisión soliciten una dirección IP para una subred específica y desde un ámbito y un intervalo de direcciones IP específicos.


Si usa un agente de retransmisión DHCP que está configurado con la opción 82 de DHCP, sub\-opción 5, el agente de retransmisión puede solicitar una concesión de dirección IP a los clientes DHCP de un intervalo de direcciones IP específico.

Para obtener más información, consulte [Opciones de selección de subred DHCP](dhcp-subnet-options.md).

## <a name="new-logging-events-for-dns-registration-failures-by-the-dhcp-server"></a>Nuevos eventos de registro para errores de registro de DNS por el servidor DHCP

DHCP incluye ahora los eventos de registro para las circunstancias en las que los registros DNS del servidor DHCP producen errores en el servidor DNS.

Para obtener más información, consulte [DHCP Logging Events for DNS record](dhcp-dns-events.md)registrations.

## <a name="dhcp-nap-is-not-supported-in-windows-server-2016"></a>El NAP de DHCP no se admite en Windows Server 2016

La protección de acceso a redes \(NAP\) está en desuso en Windows Server 2012 R2 y, en Windows Server 2016, el rol de servidor DHCP ya no es compatible con NAP. Para obtener más información, vea [características eliminadas o en desuso en Windows Server 2012 R2](https://technet.microsoft.com/library/dn303411.aspx).  
  
La compatibilidad con NAP se presentó al rol de servidor DHCP con Windows Server 2008 y es compatible con los sistemas operativos de cliente y servidor de Windows anteriores a Windows 10 y Windows Server 2016. En la tabla siguiente se resume la compatibilidad con NAP en Windows Server.  
  
|Sistema operativo|Compatibilidad con NAP|  
|--------------------|---------------|  
| Windows Server 2008 |Se admite|  
| Windows Server 2008 R2 |Se admite|  
| Windows Server 2012 |Se admite|  
| Windows Server 2012 R2 |Se admite|  
| Windows Server 2016|No se admite|  
  
En una implementación de NAP, un servidor DHCP que ejecute un sistema operativo que admita NAP puede funcionar como un punto de cumplimiento NAP para el método de cumplimiento NAP DHCP. Para obtener más información acerca de DHCP en NAP, consulte [lista de comprobación: implementar un diseño de cumplimiento DHCP](https://technet.microsoft.com/library/dd314186.aspx).  
  
En Windows Server 2016, los servidores DHCP no aplican directivas de NAP y los ámbitos DHCP no pueden ser NAP\-habilitado. Los equipos cliente DHCP que también son clientes NAP envían un informe de mantenimiento \(SoH\) con la solicitud DHCP. Si el servidor DHCP ejecuta Windows Server 2016, estas solicitudes se procesan como si no hubiera ningún SoH. El servidor DHCP concede una concesión DHCP normal al cliente. 

Si los servidores que ejecutan Windows Server 2016 son proxies RADIUS que reenvían las solicitudes de autenticación a un servidor de directivas de redes \(\) NPS que admite NAP, NPS evalúa estos clientes NAP como no compatibles con NAP\-y se produce un error en el procesamiento de NAP.
  
## <a name="see-also"></a>Vea también  
  
-   [Protocolo de configuración dinámica de host (DHCP)](Dynamic-Host-Configuration-Protocol--DHCP-.md)  
  

