---
title: pathping
description: Artículo de referencia para el comando pathping, que obtiene información sobre la latencia de red y la pérdida de red en saltos intermedios entre un origen y un destino.
ms.topic: article
ms.assetid: ec430125-b1dc-4aad-a7c9-b70f486d9e3c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: d5ce12d950356c5ebb5ad671de09aaebbc91b9fb
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87885091"
---
# <a name="pathping"></a>pathping

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Proporciona información sobre la latencia de red y la pérdida de red en saltos intermedios entre un origen y un destino. Este comando envía varios mensajes de solicitud de eco a cada enrutador entre un origen y un destino, a lo largo de un período de tiempo y, a continuación, calcula los resultados en función de los paquetes devueltos por cada enrutador. Dado que este comando muestra el grado de pérdida de paquetes en un enrutador o vínculo determinados, puede determinar qué enrutadores o subredes pueden estar teniendo problemas de red. Si se usa sin parámetros, este comando muestra la ayuda.

> [!NOTE]
> Este comando solo está disponible si el Protocolo Protocolo de Internet (TCP/IP) se instala como componente en las propiedades de un adaptador de red en las conexiones de red.
>
> Además, este comando identifica los enrutadores que se encuentran en la ruta de acceso, igual que con el [comando tracert](tracert.md). Howevever, este comando también envía pings periódicamente a todos los enrutadores durante un período de tiempo especificado y calcula estadísticas basadas en el número devuelto de cada una de ellas.

## <a name="syntax"></a>Sintaxis

```
pathping [/n] [/h <maximumhops>] [/g <hostlist>] [/p <Period>] [/q <numqueries> [/w <timeout>] [/i <IPaddress>] [/4 <IPv4>] [/6 <IPv6>][<targetname>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| /n | Impide que **pathping** intente resolver las direcciones IP de los enrutadores intermedios en sus nombres. Esto puede acelerar la presentación de los resultados de **pathping** . |
| /h`<maximumhops>` | Especifica el número máximo de saltos en la ruta de acceso para buscar el destino (destino). El valor predeterminado es 30 saltos. |
| /g`<hostlist>` | Especifica que los mensajes de solicitud de eco usan la opción de **ruta de origen flexible** del encabezado IP con el conjunto de destinos intermedios especificados en *hostlist*. Con el enrutamiento de origen suelto, los destinos intermedios sucesivos pueden estar separados por uno o varios enrutadores. El número máximo de direcciones o nombres en la lista de hosts es **9**. *Hostlist* es una serie de direcciones IP (en notación decimal con puntos) separadas por espacios. |
| /p`<period>` | Especifica el número de milisegundos que se va a esperar entre pings consecutivos. El valor predeterminado es 250 milisegundos (1/4 segundos). Este parámetro envía pings individuales a cada salto intermedio. Por este motivo, el intervalo entre dos pings enviados al mismo salto es el *período* multiplicado por el número de saltos. |
| /q`<numqueries>` | Especifica el número de mensajes de solicitud de eco enviados a cada enrutador de la ruta de acceso. El valor predeterminado es 100 consultas. |
| /w`<timeout>` | Especifica el número de milisegundos que se debe esperar para cada respuesta. El valor predeterminado es 3000 milisegundos (3 segundos). Este parámetro envía varios ping en paralelo. Por este motivo, la cantidad de tiempo especificada en el parámetro *timeout* no está limitada por la cantidad de tiempo especificada en el parámetro *period* para esperar entre pings. |
| /i`<IPaddress>` | Especifica la dirección de origen. |
| /4`<IPv4>` | Especifica que PathPing solo usa IPv4. |
| /6`<IPv6>` | Especifica que PathPing solo usa IPv6. |
| `<targetname>` | Especifica el destino, que se identifica mediante la dirección IP o el nombre de host. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Observaciones

- Todos los parámetros distinguen mayúsculas de minúsculas.

- Para evitar la congestión de la red y minimizar los efectos de las pérdidas de ráfagas, los pings se deben enviar a un ritmo suficientemente lento.

### <a name="example-of-the-pathping-command-output"></a>Ejemplo de la salida del comando pathping

```
D:\>pathping /n contoso1
Tracing route to contoso1 [10.54.1.196]
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

Cuando se ejecuta **pathping** , los primeros resultados muestran la ruta de acceso. A continuación, se muestra un mensaje ocupado durante aproximadamente 90 segundos (el tiempo varía según el número de saltos). Durante este tiempo, se recopila información de todos los enrutadores enumerados anteriormente y de los vínculos entre ellos. Al final de este período, se muestran los resultados de la prueba.

En el informe de ejemplo anterior, las columnas **este nodo o vínculo**, **perdido/enviado = PCT** y **Dirección** muestran que el vínculo entre *172.16.87.218* y *192.168.52.1* está quitando el 13% de los paquetes. Los enrutadores de los saltos 2 y 4 también están quitando los paquetes que se dirigen a ellos, pero esta pérdida no afecta a su capacidad para reenviar el tráfico que no se le dirige.

Las tasas de pérdida mostradas para los vínculos, identificadas como una barra vertical ( **|** ) en la columna **Dirección** , indican la congestión del vínculo que está causando la pérdida de paquetes que se reenvían en la ruta de acceso. Las tasas de pérdida mostradas para los enrutadores (identificados por sus direcciones IP) indican que estos enrutadores podrían estar sobrecargados.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando de seguimiento](tracert.md)
