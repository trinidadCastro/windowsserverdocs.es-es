---
title: pathping
description: Obtenga información sobre cómo obtener la información de pérdida con el comando pathping y latencia de red.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ec430125-b1dc-4aad-a7c9-b70f486d9e3c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: db3a0f5cd3c711f7df0a13627969dc7b74b3d605
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837246"
---
# <a name="pathping"></a>pathping

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Proporciona información sobre la latencia de red y pérdida de red en saltos intermedios entre un origen y destino. **Pathping** envía mensajes de solicitud de eco de varios a cada enrutador entre un origen y destino durante un período de tiempo y, a continuación, calcula los resultados basándose en los paquetes devueltos en cada enrutador. Dado que **pathping** muestra el grado de pérdida de paquetes en cualquier vínculo o enrutador determinados, puede determinar qué enrutadores o subredes pueden tener problemas de red. 

**Pathping** realiza el equivalente de la **tracert** comando mediante la identificación de que están en la ruta de acceso. A continuación, hace ping periódicamente a todos los enrutadores durante un período de tiempo y calcula las estadísticas en función del número que devuelve cada uno. Se utiliza sin parámetros, **pathping** muestra la Ayuda. 

## <a name="syntax"></a>Sintaxis
```
pathping [/n] [/h] [/g <Hostlist>] [/p <Period>] [/q <NumQueries> [/w <timeout>] [/i <IPaddress>] [/4 <IPv4>] [/6 <IPv6>][<TargetName>]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|/n|Evita que **pathping** intentan resolver las direcciones IP de los enrutadores intermedios en sus nombres. Esto puede acelerar la presentación de **pathping** resultados.|
|/h \<saltosMáximos >|Especifica el número máximo de saltos en la ruta de acceso para buscar el destino (destino). El valor predeterminado es 30 saltos.|
|/g \<Hostlist >|Especifica que la opción en el encabezado IP con el conjunto de destinos intermedios especificados en el uso de los mensajes de solicitud de la ruta de origen no estricta de eco *Hostlist*. Con el enrutamiento de origen no estricta, sucesivos destinos intermedios se pueden separar con uno o varios enrutadores. El número máximo de direcciones o nombres en la lista de hosts es 9. El *Hostlist* es una serie de direcciones IP (en notación decimal con puntos) separada por espacios.|
|/p \<período >|Especifica el número de milisegundos que transcurrirán entre pings consecutivos. El valor predeterminado es 250 milisegundos (4/1 segundo).|
|/q \<NumQueries>|Especifica el número de eco enviados a cada enrutador en la ruta de acceso de los mensajes de solicitud. El valor predeterminado es 100 consultas.|
|/w \<timeout>|Especifica el número de milisegundos de espera para cada respuesta. El valor predeterminado es 3000 milisegundos (3 segundos).|
|/i \<IPaddress>|Especifica la dirección de origen.|
|/4 \<IPv4>|Especifica que pathping usa solo IPv4.|
|/6 \<IPv6>|Especifica que pathping solo utiliza IPv6.|
|\<TargetName>|Especifica el destino, que se identifica por el nombre de host o dirección IP.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios
-   **Pathping** parámetros distinguen mayúsculas de minúsculas.
-   Para evitar la congestión de la red, se debe hacer ping a un ritmo lento suficientemente.
-   Para minimizar los efectos de las pérdidas de ráfaga, no realice ping con demasiada frecuencia.
-   Cuando se usa el **/p** parámetro, pings se envían individualmente a cada salto intermedio. Por este motivo, el intervalo entre dos pings enviados al mismo salto es *período* multiplicado por el número de saltos.
-   Cuando se usa el **/w** parámetro, se pueden enviar múltiples pings en paralelo. Por este motivo, la cantidad de tiempo especificado en el *tiempo de espera* parámetro no está limitado por la cantidad de tiempo especificado en el *período* parámetro esperará entre pings.
-   Este comando está disponible sólo si el protocolo del protocolo de Internet (TCP/IP) se instala como un componente en las propiedades de un adaptador de red en las conexiones de red.

## <a name="BKMK_Examples"></a>Ejemplos

El ejemplo siguiente muestra **pathping** resultado del comando:

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
Cuando **pathping** es ejecutar, los primeros resultados muestran la ruta de acceso. Se trata de la misma ruta de acceso que se muestra mediante el **tracert** comando. A continuación, se muestra un mensaje de ocupado durante aproximadamente 90 segundos (el tiempo varía según el número de saltos). Durante este tiempo, se recopila información de todos los enrutadores enumerados anteriormente y uno de los vínculos entre ellos. al final de este período, se muestran los resultados de pruebas.

En el informe de ejemplo anterior, el **este nodo o vínculo**, **perdido/Enviado = Pct** y **dirección** columnas muestran que el vínculo entre 172.16.87.218 y 192.168.52.1 13 % de los paquetes. Los enrutadores de los saltos 2 y 4 también pierden paquetes dirigidos a ellos, pero esta pérdida no afecta a su capacidad para reenviar el tráfico que no está dirigido a ellas.

Las tasas de pérdidas correspondientes a los vínculos, identificados como una barra vertical (**|**) en el **dirección** columna, indicar la congestión en el vínculo que está provocando la pérdida de paquetes reenviados en el ruta de acceso. Los niveles de pérdida de enrutadores (identificados por sus direcciones IP) indican que podrían estar sobrecargados estos enrutadores.

## <a name="additional-references"></a>Referencias adicionales
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)