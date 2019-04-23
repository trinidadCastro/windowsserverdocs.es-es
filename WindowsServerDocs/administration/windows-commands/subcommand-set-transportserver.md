---
title: Set-TransportServer subcomando
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7863225c-f4b2-4cd0-b929-78a454bef249
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d144ed7d461cbebcd351aa4347fde20f35736c26
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886606"
---
# <a name="subcommand-set-transportserver"></a>Subcomando: set-TransportServer

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Establece los valores de configuración para un servidor de transporte.
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
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|[/Server:<Server name>]|Especifica el nombre del servidor de transporte. Esto puede ser el nombre NetBIOS o el nombre de dominio completo (FQDN). Si se especifica ningún nombre de servidor de transporte, se usa el servidor local.|
|[/ObtainIpv4From:{Dhcp &#124; Range}]|Establece el origen de las direcciones IPv4 como sigue:<br /><br />-[/ iniciar: <IP address>] establece el inicio del intervalo de direcciones IP. Esto es necesario y es válido sólo si esta opción está establecida en **intervalo**.<br />-[/ End: <IP address>] establece el final del intervalo de direcciones IP. Esto es necesario y es válido sólo si esta opción está establecida en **intervalo**.<br />-[/ puerto inicial: <port>] establece el inicio del intervalo de puertos.<br />-[/ El puerto final: <port>] establece el final del intervalo de puertos.|
|[/ObtainIpv6From:Range]|Especifica el origen de las direcciones IPv6. Esta opción solo se aplica a Windows Server 2008 R2 y el único valor admitido es **intervalo**.<br /><br />-[/ iniciar: <IP address>] establece el inicio del intervalo de direcciones IP. Esto es necesario y es válido sólo si esta opción está establecida en **intervalo**.<br />-[/ End: <IP address>] establece el final del intervalo de direcciones IP. Esto es necesario y es válido sólo si esta opción está establecida en **intervalo**.<br />-[/ puerto inicial: <port>] establece el inicio del intervalo de puertos.<br />-[/ El puerto final: <port>] establece el final del intervalo de puertos.|
|[/Profile: {10Mbps &#124; 100Mbps &#124; 1Gbps &#124; Custom}]|Especifica el perfil de red que se usará. Esta opción solo está disponible para los servidores que ejecutan Windows Server 2008 o Windows Server 2003.|
|[/MulticastSessionPolicy]|Configura las opciones de transferencia para transmisiones por multidifusión. Este comando solo está disponible para Windows Server 2008 R2.<br /><br />-[/ Directiva: {None &#124; desconexión automática &#124; transmisión múltiple}] determina cómo se controlan los clientes lentos. **Ninguno** significa mantener todos los clientes en una sesión de la misma velocidad. **Desconexión automática** significa que los clientes que caen por debajo de la especificada **/Threshold** están desconectados. **Transmisión múltiple** significa que los clientes se dividirá en varias sesiones según lo especificado por **/StreamCount**.<br />-[/ Umbral:<Speed in KBps>] establece la velocidad de transferencia mínimo en KBps para **/Policy:AutoDisconnect**. Los clientes que caen por debajo de esta velocidad se desconectan de transmisiones por multidifusión.<br />-[/ StreamCount: {2 &#124; 3}] [/ reserva: {Sí &#124; n}] determina el número de sesiones de **/Policy:Multistream**. **2** significa que dos sesiones (rápidas y lentas) y **3** significa tres sesiones (lento, medio, rápido).<br />-[/ Reserva: {Sí &#124; n}] determina si se desconectan los clientes tha continuará la transferencia mediante otro método (si se admite por el cliente). Si usa al cliente WDS, el equipo recurrirá a unidifusión. Wdsmcast.exe no admite un mecanismo de reserva. Esta opción también se aplica a los clientes que no admiten **transmisión múltiple**. En ese caso, el equipo se revertirá a otro método en lugar de mover a una sesión de transferencia más lenta.|
## <a name="BKMK_examples"></a>Ejemplos
Para establecer el intervalo de direcciones IPv4 para el servidor, escriba:
```
wdsutil /Set-TransportServer /ObtainIpv4From:Range /start:239.0.0.1 /End:239.0.0.100
```
Para establecer el intervalo de direcciones IPv4, el intervalo de puertos y el perfil para el servidor, escriba:
```
wdsutil /Set-TransportServer /Server:MyWDSServer /ObtainIpv4From:Range /start:239.0.0.1 /End:239.0.0.100 /startPort:12000 /EndPort:50000 /Profile:10mbps
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[con el comando disable-TransportServer](using-the-disable-transportserver-command.md)
[con el comando enable-TransportServer](using-the-enable-transportserver-command.md) 
 [ Mediante el comando get-TransportServer](using-the-get-transportserver-command.md)
[subcomando: start-TransportServer](subcommand-start-transportserver.md)
[subcomando: stop-TransportServer](subcommand-stop-transportserver.md)
