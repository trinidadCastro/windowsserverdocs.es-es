---
title: 'Solución de problemas de AD FS: resolución DNS'
description: En este documento se describe cómo solucionar problemas de los aspectos de DNS de AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/03/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: a9be4a72cd60cfdd5807c67132dba837093be4db
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86959027"
---
# <a name="ad-fs-troubleshooting---dns"></a>Solución de problemas de AD FS: DNS 
Una de las primeras cosas que se deben comprobar, si AD FS no funciona o responde, es la resolución de nombres DNS.  Se trata de pruebas básicas para determinar si los servidores de AD FS o los servidores WAP se encuentran en la red.  En el caso de los usuarios internos, estas pruebas deben resolverse en los servidores de AD FS (STS).    En el caso de los usuarios externos, estas pruebas deben resolverse en los servidores WAP.

En el resto de este documento se muestra cómo realizar algunas comprobaciones rápidas de la resolución de nombres mediante herramientas de línea de comandos.

## <a name="ping-test"></a>Prueba de ping
Comprueba la conectividad de nivel IP con otro equipo TCP/IP mediante el envío de mensajes de solicitud de eco del Protocolo de mensajes de control de Internet (ICMP). Se muestra la recepción de los mensajes de respuesta de eco correspondientes, junto con los tiempos de ida y vuelta.  Para obtener más información, vea [ping](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ff961503(v=ws.11)).


>[!NOTE]
>Tenga en cuenta que algunas organizaciones bloquean este puerto en sus servidores y puede obtener una respuesta de **tiempo de espera** de la solicitud.

### <a name="to-use-a-ping-test"></a>Para usar una prueba de PING
1.  Abra un símbolo del sistema.
2. Escriba PING <name of adfs server> a. Ejemplo: PING sts.contoso.com
3. Debería ver una respuesta del servidor

![Ping](media/ad-fs-tshoot-dns/dns1.png)

## <a name="nslookup"></a>NSLookup
Muestra información que puede usar para diagnosticar la infraestructura del sistema de nombres de dominio (DNS).  Para obtener más información, consulte [nslookup](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/cc725991(v=ws.11)).

### <a name="to-use-a-nslookup"></a>Para usar NSLookup
1.  Abra un símbolo del sistema.
2. Escriba PING <name of adfs server> a. Ejemplo: nslookup sts.contoso.com
3. Debería ver la información de DNS del servidor ![ nslookup](media/ad-fs-tshoot-dns/dns2.png)

## <a name="tracert"></a>Tracert
Determina la ruta de acceso tomada a un destino mediante el envío de solicitudes de eco del Protocolo de mensajes de control de Internet (ICMP) o mensajes ICMPv6 al destino con valores de campo de período de vida (TTL) que aumentan incrementalmente.   Para obtener más información, consulte [tracert](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ff961507(v=ws.11)).


### <a name="to-use-tracert"></a>Para usar tracert
1.  Abra un símbolo del sistema.
2. Escriba tracert <name of adfs server> a. Ejemplo: tracert sts.contoso.com
3. Debería ver la ruta de acceso de destino que se usa para alcanzar el servidor de ![ seguimiento](media/ad-fs-tshoot-dns/dns3.png)

## <a name="next-steps"></a>Pasos siguientes

- [Solución de problemas de AD FS](ad-fs-tshoot-overview.md)
