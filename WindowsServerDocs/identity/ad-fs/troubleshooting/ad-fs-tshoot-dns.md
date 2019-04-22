---
title: Solución de AD FS - resolución de DNS
description: Este documento describe cómo solucionar los aspectos DNS de AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/03/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 7e0065feac4241b617b8b13c6867d5dc36634bd0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59815906"
---
# <a name="ad-fs-troubleshooting---dns"></a>Solución de AD FS - DNS 
Uno de los primeros pasos para comprobar si AD FS no está trabajando o responde, es la resolución de nombres DNS.  Estas son las pruebas básicas para determinar si los servidores de AD FS o servidores WAP se encuentran en la red.  Para los usuarios internos, deben resolver estas pruebas en los servidores de AD FS (STS).    Para usuarios externos, deben resolver estas pruebas en los servidores WAP.

El resto de este documento mostrará cómo realizar algunas comprobaciones de resolución de nombre rápido con herramientas de línea de comandos.

## <a name="ping-test"></a>Prueba de ping
Comprueba la conectividad de nivel de dirección IP a otro equipo TCP/IP mediante el envío de mensajes de solicitud de eco del protocolo de mensajes de Control de Internet (ICMP). Se muestran la recepción de mensajes de respuesta de eco correspondientes, junto con los tiempos de ida y vuelta.  Para obtener más información, consulte [Ping](https://technet.microsoft.com/library/ff961503.aspx).


>[!NOTE]
>Tenga en cuenta que algunas organizaciones bloquean dicho puerto en sus servidores y es posible que obtenga un **solicitud agotó el tiempo de espera** respuesta.

### <a name="to-use-a-ping-test"></a>Para usar una prueba de PING
1.  Abra un símbolo del sistema
2. Escriba PING <name of adfs server> una. Por ejemplo:  PING sts.contoso.com
3. Debería ver una respuesta del servidor

![Ping](media/ad-fs-tshoot-dns/dns1.png)

## <a name="nslookup"></a>NSLookup
Muestra información que puede usar para diagnosticar la infraestructura de sistema de nombres de dominio (DNS).  Para obtener más información, consulte [NSLookup](https://technet.microsoft.com/library/cc725991.aspx).

### <a name="to-use-a-nslookup"></a>Para utilizar un NSLookup
1.  Abra un símbolo del sistema
2. Escriba PING <name of adfs server> una. Ejemplo: sts.contoso.com de nslookup
3. Debería ver la información de dns para el servidor ![NSLookup](media/ad-fs-tshoot-dns/dns2.png)

## <a name="tracert"></a>Tracert
Determina la ruta a un destino de solicitud de eco de protocolo de mensajes de Control de Internet (ICMP) envío o los mensajes ICMPv6 al destino con el aumento incremental del tiempo para los valores de campo de vida (TTL).   Para obtener más información, consulte [Tracert](https://technet.microsoft.com/library/ff961507.aspx).


### <a name="to-use-tracert"></a>Para usar Tracert
1.  Abra un símbolo del sistema
2. Escriba tracert <name of adfs server> una. Ejemplo: sts.contoso.com de tracert
3. Debería ver la ruta de acceso de destino que usa para tener acceso al servidor ![Tracert](media/ad-fs-tshoot-dns/dns3.png)

## <a name="next-steps"></a>Pasos siguientes

- [Solución de problemas de AD FS](ad-fs-tshoot-overview.md)