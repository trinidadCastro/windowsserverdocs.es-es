---
title: netstat
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 60e2718f-93cc-4ceb-bf0e-58a6a6e4fc8b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 666a15056e75cea37959d821c34288ffc2c6a6c4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825676"
---
# <a name="netstat"></a>netstat

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra las conexiones TCP activas, los puertos en el que el equipo está escuchando, las estadísticas de Ethernet, la tabla de enrutamiento IP, las estadísticas de IPv4 (para los protocolos IP, ICMP, TCP y UDP) y las estadísticas de IPv6 (para IPv6, ICMPv6, TCP a través de IPv6 y UDP a través de protocolos IPv6). Se utiliza sin parámetros, **netstat** muestra las conexiones TCP activas. 

## <a name="syntax"></a>Sintaxis
```
netstat [-a] [-e] [-n] [-o] [-p <Protocol>] [-r] [-s] [<Interval>]
```

### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|-a|Muestra todas las conexiones TCP activas y los puertos TCP y UDP en el que está escuchando el equipo.|
|-e|Muestra las estadísticas de Ethernet, como el número de bytes y paquetes enviados y recibidos. Este parámetro se puede combinar con **-s**.|
|-n|Muestra las conexiones TCP activas, sin embargo, direcciones y números de puerto se expresan numéricamente y se realiza ningún intento para determinar los nombres.|
|-o|Muestra las conexiones TCP activas e incluye el identificador de proceso (PID) para cada conexión. Puede encontrar la aplicación basándose en el PID en la ficha procesos en el Administrador de tareas de Windows. Este parámetro se puede combinar con **- a**, **- n**, y **-p**.|
|-p <Protocol>|Muestra las conexiones del protocolo especificado por *protocolo*. En este caso, el *protocolo* puede ser tcp, udp, tcpv6 o udpv6. Si este parámetro se utiliza con **-s** para mostrar las estadísticas por protocolo, *protocolo* puede ser tcp, udp, icmp, ip, tcpv6, udpv6, icmpv6 o ipv6.|
|-s|Muestra las estadísticas por protocolo. De forma predeterminada, se muestran las estadísticas para los protocolos TCP, UDP, ICMP e IP. Si el protocolo IPv6 está instalado, se muestran estadísticas de TCP a través de IPv6, UDP sobre IPv6, ICMPv6 e IPv6 protocolos. El **-p** parámetro puede usarse para especificar un conjunto de protocolos.|
|-r|Muestra el contenido de la tabla de enrutamiento IP. Esto es equivalente a del comando route print.|
|<Interval>|Presenta la información seleccionada cada *intervalo* segundos. Presione CTRL+C para detener el volver a mostrar. Si se omite este parámetro, **netstat** imprime la información seleccionada en una sola vez.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios
-   Los parámetros utilizados con este comando deben ir precedidos de un guión (**-**) en lugar de una barra diagonal (**/**).
-   **netstat** proporciona estadísticas para lo siguiente:
    -   Proto el nombre del protocolo (TCP o UDP).
    -   Dirección local de la dirección IP del equipo local y el número de puerto que se va a usar. Se muestran el nombre del equipo local que corresponde a la dirección IP y el nombre del puerto a menos que el **- n** se especifica el parámetro. Si el puerto no se ha establecido, se muestra el número de puerto como un asterisco (*).
    -   dirección externa la dirección IP y puerto número del equipo remoto al que está conectado el socket. Los nombres que se corresponde con la dirección IP y el puerto se muestran a menos que el **- n** se especifica el parámetro. Si el puerto no se ha establecido, se muestra el número de puerto como un asterisco (*).
    -   (estado) Indica el estado de una conexión TCP. Los Estados posibles son los siguientes: LAST_ACK de FIN_WAIT_2 FIN_WAIT_1 CLOSE_WAIT de cerrado establecido escucha SYN_RECEIVED SYN_SEND timeD_WAIT para obtener más información sobre los Estados de una conexión TCP, consulte Rfc 793.
-   Este comando está disponible sólo si el protocolo del protocolo de Internet (TCP/IP) se instala como un componente en las propiedades de un adaptador de red en las conexiones de red.

## <a name="BKMK_Examples"></a>Ejemplos
Para mostrar las estadísticas de Ethernet y las estadísticas para todos los protocolos, escriba:
```
netstat -e -s
```
Para mostrar las estadísticas de los protocolos TCP y UDP, escriba:
```
netstat -s -p tcp udp
```
Para mostrar las conexiones TCP activas y los identificadores de proceso cada 5 segundos, escriba:
```
netstat -o 5
```
Para mostrar TCP activas las conexiones y el proceso de identificadores con formato numérico, escriba:
```
netstat -n -o
```

## <a name="additional-references"></a>Referencias adicionales
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
