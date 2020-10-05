---
title: WDSUtil Set-TransportServer
description: Artículo de referencia para el subcomando set-TransportServer, que establece los valores de configuración para un servidor de transporte.
ms.topic: reference
ms.assetid: 7863225c-f4b2-4cd0-b929-78a454bef249
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: fd2c1a82dbb83dc45b08fca6259a4aa6b18a4f1b
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91731249"
---
# <a name="wdsutil-set-transportserver"></a>WDSUtil Set-TransportServer

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Establece la configuración de un servidor de transporte.

## <a name="syntax"></a>Sintaxis
```
wdsutil [Options] /Set-TransportServer [/Server:<Server name>]
     [/ObtainIpv4From:{Dhcp | Range}]
        [/start:<starting IP address>]
        [/End:<Ending IP address>]
     [/ObtainIpv6From:Range]\n\
        [/start:<start IP address>]\n\
        [/End:<End IP address>]
     [/startPort:<starting port>
        [/EndPort:<starting port>
     [/Profile:{10Mbps | 100Mbps | 1Gbps | Custom}]
     [/MulticastSessionPolicy]
             [/Policy:{None | AutoDisconnect | Multistream}]
                 [/Threshold:<Speed in KBps>]
                 [/StreamCount:{2 | 3}]
                 [/Fallback:{Yes | No}]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|[/Server:<Server name>]|Especifica el nombre del servidor de transporte. Puede ser el nombre NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor de transporte, se utiliza el servidor local.|
|[/ObtainIpv4From: {intervalo de &#124; DHCP}]|Establece el origen de las direcciones IPv4 como se indica a continuación:<p>-[/Start: <IP address> ] establece el inicio del intervalo de direcciones IP. Solo es obligatorio y válido si esta opción se establece en **Range**.<br />-[/End: <IP address> ] establece el final del intervalo de direcciones IP. Solo es obligatorio y válido si esta opción se establece en **Range**.<br />-[/startPort: <port> ] establece el inicio del intervalo de puertos.<br />-[/EndPort: <port> ] establece el final del intervalo de puertos.|
|[/ObtainIpv6From: intervalo]|Especifica el origen de las direcciones IPv6. Esta opción solo se aplica a Windows Server 2008 R2 y el único valor admitido es **intervalo**.<p>-[/Start: <IP address> ] establece el inicio del intervalo de direcciones IP. Solo es obligatorio y válido si esta opción se establece en **Range**.<br />-[/End: <IP address> ] establece el final del intervalo de direcciones IP. Solo es obligatorio y válido si esta opción se establece en **Range**.<br />-[/startPort: <port> ] establece el inicio del intervalo de puertos.<br />-[/EndPort: <port> ] establece el final del intervalo de puertos.|
|[/Profile: {10Mbps &#124; 100 Mbps &#124; 1 Gbps &#124; personalizado}]|Especifica el perfil de red que se va a usar. Esta opción solo está disponible para los servidores que ejecutan Windows Server 2008 o Windows Server 2003.|
|[/MulticastSessionPolicy]|Configura las opciones de transferencia para las transmisiones de multidifusión. Este comando solo está disponible para Windows Server 2008 R2.<p>-[/Policy: {None &#124; autodisconnect &#124; Multistream}] determina cómo se administran los clientes lentos. **Ninguno** significa mantener todos los clientes en una sesión a la misma velocidad. La **desconexión automática** significa que los clientes que se colocan por debajo del **/Threshold** especificado están desconectados. **Multistream** significa que los clientes se separarán en varias sesiones, tal y como se especifica en **/StreamCount**.<br />-[/Threshold: <Speed in KBps> ] establece la velocidad de transferencia mínima en kbps para **/Policy: autodisconnect**. Los clientes que se encuentran por debajo de esta tasa se desconectan de las transmisiones de multidifusión.<br />-[/StreamCount: {2 &#124; 3}] [/fallback: {Yes &#124; no}] determina el número de sesiones para **/Policy: Multistream**. **2** significa que dos sesiones (rápidas y lentas) y **3** significan tres sesiones (lentas, medias y rápidas).<br />-[/Fallback: {Yes &#124; no}] determina si los clientes que están desconectados continuarán la transferencia mediante otro método (si el cliente lo admite). Si usa el cliente de WDS, el equipo revierte a la unidifusión. Wdsmcast.exe no admite un mecanismo de reserva. Esta opción también se aplica a los clientes que no admiten **Multistream**. En ese caso, el equipo revertirá a otro método en lugar de pasar a una sesión de transferencia más lenta.|
## <a name="examples"></a>Ejemplos
Para establecer el intervalo de direcciones IPv4 para el servidor, escriba:
```
wdsutil /Set-TransportServer /ObtainIpv4From:Range /start:239.0.0.1 /End:239.0.0.100
```
Para establecer el intervalo de direcciones IPv4, el intervalo de puertos y el perfil para el servidor, escriba:
```
wdsutil /Set-TransportServer /Server:MyWDSServer /ObtainIpv4From:Range /start:239.0.0.1 /End:239.0.0.100 /startPort:12000 /EndPort:50000 /Profile:10mbps
```
## <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
- [WDSUtil Disable-TransportServer (comando)](wdsutil-disable-transportserver.md)
- [WDSUtil enable-TransportServer (comando)](wdsutil-enable-transportserver.md)
- [WDSUtil Get-TransportServer (comando)](wdsutil-get-transportserver.md)
- [WDSUtil Start-TransportServer (comando)](wdsutil-start-transportserver.md)
- [WDSUtil STOP-TransportServer (comando)](wdsutil-stop-transportserver.md)
