---
title: netstat
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 54bd21b7e96275d329e45e825971d9236488c793
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373259"
---
# <a name="netstat"></a>netstat

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra las conexiones TCP activas, los puertos en los que el equipo está escuchando, las estadísticas de Ethernet, la tabla de enrutamiento IP, las estadísticas de IPv4 (para los protocolos IP, ICMP, TCP y UDP) y las estadísticas de IPv6 (para los protocolos IPv6, ICMPv6, TCP a través de IPv6 y UDP a través de IPv6). Si se usa sin parámetros, **netstat** muestra las conexiones TCP activas. 

## <a name="syntax"></a>Sintaxis
```
netstat [-a] [-e] [-n] [-o] [-p <Protocol>] [-r] [-s] [<Interval>]
```

### <a name="parameters"></a>Parámetros

|   Parámetro   |                                                                                                                                              Descripción                                                                                                                                              |
|---------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|      -a       |                                                                                                   Muestra todas las conexiones TCP activas y los puertos TCP y UDP en los que el equipo está escuchando.                                                                                                   |
|      -e       |                                                                                 Muestra las estadísticas de Ethernet, como el número de bytes y paquetes enviados y recibidos. Este parámetro se puede combinar con **-s**.                                                                                  |
|      -n       |                                                                               Muestra las conexiones TCP activas, sin embargo, las direcciones y los números de puerto se expresan numéricamente y no se realiza ningún intento para determinar los nombres.                                                                               |
|      -o       |                          Muestra las conexiones TCP activas e incluye el identificador de proceso (PID) de cada conexión. Puede encontrar la aplicación basada en el PID en la pestaña procesos del administrador de tareas de Windows. Este parámetro se puede combinar con **-a**, **-n**y **-p**.                           |
| -p <Protocol> |               Muestra las conexiones para el protocolo especificado por *Protocolo*. En este caso, el *Protocolo* puede ser TCP, UDP, TCPv6 o UDPv6. Si este parámetro se usa con **-s** para mostrar las estadísticas por protocolo, el *Protocolo* puede ser TCP, UDP, ICMP, IP, TCPv6, UDPv6, ICMPv6 o IPv6.                |
|      -s       | Muestra las estadísticas por protocolo. De forma predeterminada, las estadísticas se muestran para los protocolos TCP, UDP, ICMP y IP. Si el protocolo IPv6 está instalado, se muestran estadísticas para los protocolos TCP a través de IPv6, UDP a través de IPv6, ICMPv6 e IPv6. El parámetro **-p** se puede usar para especificar un conjunto de protocolos. |
|      -r       |                                                                                                     Muestra el contenido de la tabla de enrutamiento IP. Es equivalente al comando Route Print.                                                                                                     |
|  <Interval>   |                                                        Vuelve a mostrar la información seleccionada cada *intervalo* de segundos. Presione CTRL + C para detener la representación. Si se omite este parámetro, **netstat** imprime la información seleccionada una sola vez.                                                         |
|      /?       |                                                                                                                                 Muestra la ayuda en el símbolo del sistema.                                                                                                                                  |

## <a name="remarks"></a>Comentarios
-   Los parámetros que se usan con este comando deben ir precedidos de un guion ( **-** ) en lugar de una barra diagonal ( **/** ).
-   **netstat** proporciona estadísticas para lo siguiente:
    -   Proto el nombre del Protocolo (TCP o UDP).
    -   Dirección local la dirección IP del equipo local y el número de puerto que se está usando. Se muestra el nombre del equipo local que corresponde a la dirección IP y el nombre del puerto, a menos que se especifique el parámetro **-n** . Si aún no se ha establecido el puerto, el número de puerto se muestra como un asterisco (*).
    -   Dirección externa: la dirección IP y el número de puerto del equipo remoto al que está conectado el socket. Los nombres que corresponden a la dirección IP y el puerto se muestran a menos que se especifique el parámetro **-n** . Si aún no se ha establecido el puerto, el número de puerto se muestra como un asterisco (*).
    -   State Indica el estado de una conexión TCP. Los Estados posibles son los siguientes: CLOSE_WAIT CLOSED FIN_WAIT_1 FIN_WAIT_2 LAST_ACK listEN SYN_RECEIVED SYN_SEND timeD_WAIT para obtener más información acerca de los Estados de una conexión TCP, consulte RFC 793.
-   Este comando solo está disponible si el Protocolo Protocolo de Internet (TCP/IP) se instala como componente en las propiedades de un adaptador de red en las conexiones de red.

## <a name="BKMK_Examples"></a>Example
Para mostrar las estadísticas de Ethernet y las estadísticas de todos los protocolos, escriba:
```
netstat -e -s
```
Para mostrar las estadísticas solo de los protocolos TCP y UDP, escriba:
```
netstat -s -p tcp udp
```
Para mostrar las conexiones TCP activas y los ID. de proceso cada 5 segundos, escriba:
```
netstat -o 5
```
Para mostrar las conexiones TCP activas y los ID. de proceso con formato numérico, escriba:
```
netstat -n -o
```

## <a name="additional-references"></a>Referencias adicionales
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
