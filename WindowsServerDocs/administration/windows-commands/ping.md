---
title: ping
description: Use ping para comprobar la conectividad de red.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 49272671-2eec-4fa5-881f-65c24cfbef52
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 1ac02a148061cd6eb8480c67f15e934f5fd57768
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816756"
---
# <a name="ping"></a>ping

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

El **ping** comando comprueba la conectividad de nivel de dirección IP a otro equipo TCP/IP mediante el envío de mensajes de solicitud de eco del protocolo de mensajes de Control de Internet (ICMP). La recepción de mensajes se muestran, junto con los tiempos de ida y vuelta de respuesta de eco correspondiente. ping es el comando principal de TCP/IP utilizado para solucionar problemas de conectividad, comunicación y resolución de nombres. Se utiliza sin parámetros, **ping** muestra la Ayuda.

## <a name="syntax"></a>Sintaxis

```
ping [/t] [/a] [/n <Count>] [/l <Size>] [/f] [/I <TTL>] [/v <TOS>] [/r <Count>] [/s <Count>] [{/j <Hostlist> | /k <Hostlist>}] [/w <timeout>] [/R] [/S <Srcaddr>] [/4] [/6] <TargetName>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|-------|--------|
|/t|Especifica que ping continuará el envío de mensajes de solicitud de eco al destino hasta que se interrumpe. Para interrumpir y mostrar las estadísticas, presione CTRL+interrumpir. Para interrumpir y salir **ping**, presione CTRL+C.|
|/a|Especifica que se realiza la resolución de nombres inversa en la dirección IP de destino. Si esto se realiza correctamente, ping muestra el nombre de host correspondiente.|
|/n \<recuento\>|Especifica el número de eco enviados de mensajes de solicitud. El valor predeterminado es 4.|
|/l \<tamaño\>|Especifica la longitud, en bytes, del campo de datos en el eco de los mensajes de solicitud enviados. El valor predeterminado es 32. El tamaño máximo es 65.527.|
|/f|Especifica que un eco de los mensajes de solicitud se envían con el realizan no fragmentar marca en el encabezado IP se establece en 1 (disponible en IPv4 solamente). No se pueden fragmentar el mensaje de solicitud de eco por enrutadores en la ruta de acceso al destino. Este parámetro es útil para solucionar problemas de la unidad de transmisión máxima (PMTU) de ruta de acceso.|
|/I \<TTL\>|Especifica el valor del campo TTL en el encabezado IP de eco enviados de mensajes de solicitud. El valor predeterminado es el valor TTL predeterminado para el host. El máximo *TTL* es 255.|
|/v \<TOS\>|Especifica el valor del tipo de campo de servicio (TOS) en el encabezado IP de eco solicitar mensajes enviados (disponibles en IPv4 solamente). El valor predeterminado es 0. *TOS* se especifica como un valor decimal entre 0 y 255.|
|/r \<recuento\>|Especifica que la opción de ruta de registro en el encabezado IP se usa para registrar la ruta realizada por el mensaje de solicitud de eco y correspondiente un eco de mensaje de respuesta (disponible en IPv4 solamente). Cada salto en la ruta de acceso usa una entrada en la opción de ruta de registro. Si es posible, especifique un *recuento* que es igual o mayor que el número de saltos entre el origen y destino. El *recuento* debe ser un mínimo de 1 y un máximo de 9.|
|/s \<recuento\>|Especifica que la opción de marca de tiempo de Internet en el encabezado IP se usa para registrar la hora de llegada del mensaje de solicitud de eco y correspondiente un eco de mensaje de respuesta para cada salto. El *recuento* debe ser un mínimo de 1 y un máximo de 4. Esto es necesario para las direcciones de destino local del vínculo.|
|/ j \<Hostlist\>|Especifica que la opción en el encabezado IP con el conjunto de destinos intermedios especificados en el uso de los mensajes de solicitud de la ruta de origen no estricta de eco *Hostlist* (disponible en IPv4 solamente). Con el enrutamiento de origen no estricta, sucesivos destinos intermedios se pueden separar con uno o varios enrutadores. El número máximo de direcciones o nombres en la lista de hosts es 9. La lista de hosts es una serie de direcciones IP (en notación decimal con puntos) separada por espacios.|
|/k \<Hostlist\>|Especifica que la opción en el encabezado IP con el conjunto de destinos intermedios especificados en el uso de los mensajes de solicitud de la ruta de origen estricta de eco *Hostlist* (disponible en IPv4 solamente). Con el enrutamiento de origen estricta, el próximo destino intermedio debe ser accesible directamente (debe ser un vecino en una interfaz del enrutador). El número máximo de direcciones o nombres en la lista de hosts es 9. La lista de hosts es una serie de direcciones IP (en notación decimal con puntos) separada por espacios.|
|/w \<timeout\>|Especifica la cantidad de tiempo, en milisegundos para esperar el eco de mensaje de respuesta que se corresponde con un mensaje de solicitud de eco determinado para poder recibir. Si no se recibe el mensaje de respuesta de eco en el tiempo de espera, se muestra el mensaje de error "Tiempo de espera de solicitud". El tiempo de espera predeterminado es 4000 (4 segundos).|
|/R|Especifica que la ruta de acceso de ida y vuelta se realiza un seguimiento (disponible en IPv6 solamente).|
|/S \<Srcaddr\>|Especifica la dirección de origen (disponible en IPv6 solamente).|
|/4|Especifica que IPv4 se usa para hacer ping. Este parámetro no es necesario para identificar el host de destino con una dirección IPv4. Solo es necesario para identificar el host de destino por nombre.|
|/6|Especifica que se utiliza IPv6 para hacer ping. Este parámetro no es necesario para identificar el host de destino con una dirección IPv6. Solo es necesario para identificar el host de destino por nombre.|
|\<TargetName\>|Especifica el nombre de host o dirección IP de destino.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   Puede usar **ping** para probar el nombre del equipo y la dirección IP del equipo. Si hacer ping a la dirección IP es correcta, pero no hacer ping en el nombre del equipo es, podría tener un problema de resolución de nombres. En este caso, asegúrese de que el nombre del equipo que está especificando se pueden resolver a través del archivo Hosts local, mediante el uso de las consultas del sistema de nombres de dominio (DNS), o a través de NetBIOS nombre las técnicas de solución.
-   Este comando está disponible sólo si el protocolo del protocolo de Internet (TCP/IP) se instala como un componente en las propiedades de un adaptador de red en las conexiones de red.

## <a name="BKMK_Examples"></a>Ejemplos

El ejemplo siguiente muestra **ping** resultado del comando:

```
C:\>ping example.microsoft.com       
         pinging example.microsoft.com [192.168.239.132] with 32 bytes of data:       
         Reply from 192.168.239.132: bytes=32 time=101ms TTL=124       
         Reply from 192.168.239.132: bytes=32 time=100ms TTL=124       
         Reply from 192.168.239.132: bytes=32 time=120ms TTL=124       
         Reply from 192.168.239.132: bytes=32 time=120ms TTL=124
```

Para hacer ping al destino 10.0.99.221 y resolver 10.0.99.221 a su nombre de host, escriba:

```
ping /a 10.0.99.221
```

Para hacer ping al destino 10.0.99.221 con mensajes de solicitud de eco de 10, cada uno de los cuales tiene un campo de datos de 1000 bytes, escriba:

```
ping /n 10 /l 1000 10.0.99.221
```

Para hacer ping al destino 10.0.99.221 y registrar la ruta de 4 saltos, escriba:

```
ping /r 4 10.0.99.221
```

Para hacer ping al destino 10.0.99.221 y especificar la ruta de origen no estricta de 10.12.0.1-10.29.3.1-10.1.44.1, escriba:

```
ping /j 10.12.0.1 10.29.3.1 10.1.44.1 10.0.99.221
```

## <a name="additional-references"></a>Referencias adicionales
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
