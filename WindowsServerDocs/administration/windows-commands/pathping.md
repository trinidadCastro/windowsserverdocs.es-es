---
title: pathping
description: Aprenda a obtener información sobre la latencia y la pérdida de red mediante el comando pathping.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ec430125-b1dc-4aad-a7c9-b70f486d9e3c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 3f853ef430207c08e78e0446ce67c6b5bec4c1db
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80837700"
---
# <a name="pathping"></a>pathping

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Proporciona información sobre la latencia de red y la pérdida de red en saltos intermedios entre un origen y un destino. **pathping** envía varios mensajes de solicitud de eco a cada enrutador entre un origen y un destino durante un período de tiempo y, a continuación, calcula los resultados en función de los paquetes devueltos por cada enrutador. Dado que **pathping** muestra el grado de pérdida de paquetes en un enrutador o vínculo determinados, puede determinar qué enrutadores o subredes pueden estar teniendo problemas de red. 

**pathping** realiza el equivalente del comando **tracert** mediante la identificación de los enrutadores que se encuentran en la ruta de acceso. A continuación, envía pings periódicamente a todos los enrutadores durante un período de tiempo especificado y calcula estadísticas basadas en el número devuelto de cada. Si se usa sin parámetros, **pathping** muestra la ayuda. 

## <a name="syntax"></a>Sintaxis
```
pathping [/n] [/h] [/g <Hostlist>] [/p <Period>] [/q <NumQueries> [/w <timeout>] [/i <IPaddress>] [/4 <IPv4>] [/6 <IPv6>][<TargetName>]
```
#### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|/n|Impide que **pathping** intente resolver las direcciones IP de los enrutadores intermedios en sus nombres. Esto puede acelerar la presentación de los resultados de **pathping** .|
|/h \<MaximumHops >|Especifica el número máximo de saltos en la ruta de acceso para buscar el destino (destino). El valor predeterminado es 30 saltos.|
|/g \<hostlist >|Especifica que los mensajes de solicitud de eco usan la opción de ruta de origen flexible del encabezado IP con el conjunto de destinos intermedios especificados en *hostlist*. Con el enrutamiento de origen suelto, los destinos intermedios sucesivos pueden estar separados por uno o varios enrutadores. El número máximo de direcciones o nombres en la lista de hosts es 9. *Hostlist* es una serie de direcciones IP (en notación decimal con puntos) separadas por espacios.|
|/p \<período >|Especifica el número de milisegundos que se va a esperar entre pings consecutivos. El valor predeterminado es 250 milisegundos (1/4 segundos).|
|/q \<NumQueries >|Especifica el número de mensajes de solicitud de eco enviados a cada enrutador de la ruta de acceso. El valor predeterminado es 100 consultas.|
|/w \<tiempo de espera >|Especifica el número de milisegundos que se debe esperar para cada respuesta. El valor predeterminado es 3000 milisegundos (3 segundos).|
|/i \<IPaddress >|Especifica la dirección de origen.|
|/4 \<> IPv4|Especifica que PathPing solo usa IPv4.|
|/6 \<IPv6 >|Especifica que PathPing solo usa IPv6.|
|\<TargetName >|Especifica el destino, que se identifica mediante la dirección IP o el nombre de host.|
|/?|Muestra la Ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios
-   los parámetros de **pathping** distinguen mayúsculas de minúsculas.
-   Para evitar la congestión de la red, los pings se deben enviar a un ritmo suficientemente lento.
-   Para minimizar los efectos de las pérdidas de ráfagas, no envíe pings con demasiada frecuencia.
-   Cuando se usa el parámetro **/p** , los pings se envían individualmente a cada salto intermedio. Por este motivo, el intervalo entre dos pings enviados al mismo salto es el *período* multiplicado por el número de saltos.
-   Cuando se usa el parámetro **/w** , se pueden enviar varios ping en paralelo. Por este motivo, la cantidad de tiempo especificada en el parámetro *timeout* no está limitada por la cantidad de tiempo especificada en el parámetro *period* para esperar entre pings.
-   Este comando solo está disponible si el Protocolo Protocolo de Internet (TCP/IP) se instala como componente en las propiedades de un adaptador de red en las conexiones de red.

## <a name="examples"></a><a name=BKMK_Examples></a>Example

En el ejemplo siguiente se muestra la salida del comando **pathping** :

```
D:\>pathping /n corp1
Tracing route to corp1 [10.54.1.196]
over a maximum of 30 hops:
  0  172.16.87.35
  1  172.16.87.218
  2  192.168.52.1
  3  192.168.80.1
  4  10.54.247.14
  5  10.54.1.196
computing statistics for 125 seconds...
            Source to Here   This Node/Link
Hop  RTT    Lost/Sent = Pct  Lost/Sent = Pct  address
  0                                           172.16.87.35
                                0/ 100 =  0%   |
  1   41ms     0/ 100 =  0%     0/ 100 =  0%  172.16.87.218
                               13/ 100 = 13%   |
  2   22ms    16/ 100 = 16%     3/ 100 =  3%  192.168.52.1
                                0/ 100 =  0%   |
  3   24ms    13/ 100 = 13%     0/ 100 =  0%  192.168.80.1
                                0/ 100 =  0%   |
  4   21ms    14/ 100 = 14%     1/ 100 =  1%  10.54.247.14
                                0/ 100 =  0%   |
  5   24ms    13/ 100 = 13%     0/ 100 =  0%  10.54.1.196
Trace complete.
```
Cuando se ejecuta **pathping** , los primeros resultados muestran la ruta de acceso. Se trata de la misma ruta de acceso que se muestra con el comando **tracert** . A continuación, se muestra un mensaje ocupado durante aproximadamente 90 segundos (el tiempo varía según el número de saltos). Durante este tiempo, se recopila información de todos los enrutadores enumerados anteriormente y de los vínculos entre ellos. al final de este período, se muestran los resultados de la prueba.

En el informe de ejemplo anterior, las columnas **este nodo/vínculo**, **perdido/enviado = PCT** y **Dirección** muestran que el vínculo entre 172.16.87.218 y 192.168.52.1 está quitando el 13 por ciento de los paquetes. Los enrutadores de los saltos 2 y 4 también están descartando los paquetes, pero esta pérdida no afecta a su capacidad para reenviar el tráfico que no se le dirige.

Las tasas de pérdida mostradas para los vínculos, identificadas como barras verticales ( **|** ) en la columna **Dirección** , indican la congestión del vínculo que está causando la pérdida de paquetes que se reenvían en la ruta de acceso. Las tasas de pérdida mostradas para los enrutadores (identificados por sus direcciones IP) indican que estos enrutadores podrían estar sobrecargados.

## <a name="additional-references"></a>Referencias adicionales
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)