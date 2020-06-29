---
title: ping
description: Tema de referencia para el comando ping, que comprueba la conectividad de red.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 49272671-2eec-4fa5-881f-65c24cfbef52
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 1c1bd15d6536c73feb00decb9ad306f327a6464d
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2020
ms.locfileid: "85472451"
---
# <a name="ping"></a>ping

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Comprueba la conectividad de nivel IP con otro equipo TCP/IP mediante el envío de mensajes de solicitud de eco del Protocolo de mensajes de control de Internet (ICMP). Se muestra la recepción de los mensajes de respuesta de eco correspondientes, junto con los tiempos de ida y vuelta. Ping es el comando TCP/IP principal que se usa para solucionar problemas de conectividad, disponibilidad y resolución de nombres. Si se usa sin parámetros, este comando muestra el contenido de la ayuda.

También puede usar este comando para probar el nombre del equipo y la dirección IP del equipo. Si la operación de hacer ping a la dirección IP es correcta, pero no puede hacer ping en el nombre del equipo, es posible que tenga un problema de resolución de nombres. En este caso, asegúrese de que el nombre de equipo que está especificando se puede resolver a través del archivo de hosts local, mediante el uso de consultas del sistema de nombres de dominio (DNS) o mediante técnicas de resolución de nombres de NetBIOS.

> [!NOTE]
> Este comando solo está disponible si el Protocolo Protocolo de Internet (TCP/IP) se instala como componente en las propiedades de un adaptador de red en las conexiones de red.

## <a name="syntax"></a>Sintaxis

```
ping [/t] [/a] [/n <count>] [/l <size>] [/f] [/I <TTL>] [/v <TOS>] [/r <count>] [/s <count>] [{/j <hostlist> | /k <hostlist>}] [/w <timeout>] [/R] [/S <Srcaddr>] [/4] [/6] <targetname>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| /t | Especifica que ping siga enviando mensajes de solicitud de eco al destino hasta que se interrumpa. Para interrumpir y mostrar las estadísticas, presione CTRL + entrar. Para interrumpir y salir de este comando, presione CTRL + C. |
| /a | Especifica que la resolución de nombres inversa se realiza en la dirección IP de destino. Si esto se realiza correctamente, ping muestra el nombre de host correspondiente. |
| /n`<count>` | Especifica el número de mensajes de solicitud de eco enviados. El valor predeterminado es 4. |
| l`<size>` | Especifica la longitud, en bytes, del campo de **datos** de los mensajes de solicitud de eco enviados. El valor predeterminado es 32. El tamaño máximo es 65.527. |
| /f | Especifica que los mensajes de solicitud de eco se envían con la marca **no Fragment** en el encabezado IP establecido en 1 (disponible solo en IPv4). Los enrutadores de la ruta de acceso al destino no pueden fragmentar el mensaje de solicitud de eco. Este parámetro es útil para solucionar problemas de la unidad de transmisión máxima (PMTU) de la ruta de acceso. |
| /I`<TTL>` | Especifica el valor del campo TTL en el encabezado IP para los mensajes de solicitud de eco enviados. El valor predeterminado es el valor de TTL predeterminado para el host. El *TTL* máximo es 255. |
| /v`<TOS>` | Especifica el valor del campo de tipo de servicio (TOS) en el encabezado IP para los mensajes de solicitud de eco enviados (disponible solo en IPv4). El valor predeterminado es 0. *Tos* se especifica como un valor decimal entre 0 y 255. |
| /r`<count>` | Especifica que la opción **registrar ruta** del encabezado IP se usa para registrar la ruta de acceso tomada por el mensaje de solicitud de eco y el mensaje de respuesta de eco correspondiente (disponible solo en IPv4). Cada salto de la ruta de acceso usa una entrada en la opción **registrar ruta** . Si es posible, especifique un *recuento* que sea igual o mayor que el número de saltos entre el origen y el destino. El *recuento* debe ser un mínimo de 1 y un máximo de 9. |
| modificado`<count>` | Especifica que la opción de marca de tiempo de **Internet** del encabezado IP se utiliza para registrar la hora de llegada del mensaje de solicitud de eco y el mensaje de respuesta de eco correspondiente para cada salto. El *recuento* debe ser un mínimo de 1 y un máximo de 4. Esto es necesario para las direcciones de destino locales de vínculo. |
| /j`<hostlist>` | Especifica que los mensajes de solicitud de eco usan la opción de **ruta de origen flexible** del encabezado IP con el conjunto de destinos intermedios especificado en *hostlist* (disponible solo en IPv4). Con el enrutamiento de origen suelto, los destinos intermedios sucesivos pueden estar separados por uno o varios enrutadores. El número máximo de direcciones o nombres en la lista de hosts es 9. La lista de hosts es una serie de direcciones IP (en notación decimal con puntos) separadas por espacios. |
| /k`<hostlist>` | Especifica que los mensajes de solicitud de eco usan la opción de **ruta de origen estricta** en el encabezado IP con el conjunto de destinos intermedios especificado en *hostlist* (disponible solo en IPv4). Con el enrutamiento de origen estricto, el siguiente destino intermedio debe ser accesible directamente (debe ser un vecino en una interfaz del enrutador). El número máximo de direcciones o nombres en la lista de hosts es 9. La lista de hosts es una serie de direcciones IP (en notación decimal con puntos) separadas por espacios. |
| /w`<timeout>` | Especifica la cantidad de tiempo, en milisegundos, que se esperará a que se reciba el mensaje de respuesta de eco correspondiente a un mensaje de solicitud de eco determinado. Si no se recibe el mensaje de respuesta de eco en el tiempo de espera, se muestra el mensaje de error "se agotó el tiempo de espera de la solicitud". El tiempo de espera predeterminado es 4000 (4 segundos). |
| /R | Especifica que se realiza un seguimiento de la ruta de acceso de ida y vuelta (disponible solo en IPv6). |
| Modificado`<Srcaddr>` | Especifica la dirección de origen que se va a usar (disponible solo en IPv6). |
| /4 | Especifica que IPv4 se usa para hacer ping. Este parámetro no es necesario para identificar el host de destino con una dirección IPv4. Solo es necesario identificar el host de destino por nombre. |
| /6 | Especifica que IPv6 se usa para hacer ping. Este parámetro no es necesario para identificar el host de destino con una dirección IPv6. Solo es necesario identificar el host de destino por nombre. |
| `<targetname>` | Especifica el nombre de host o la dirección IP del destino. |
| /? | Muestra la ayuda en el símbolo del sistema. |

### <a name="example-of-the-ping-command-output"></a>Ejemplo de la salida del comando ping

```
C:\>ping example.microsoft.com
    pinging example.microsoft.com [192.168.239.132] with 32 bytes of data:
    Reply from 192.168.239.132: bytes=32 time=101ms TTL=124
    Reply from 192.168.239.132: bytes=32 time=100ms TTL=124
    Reply from 192.168.239.132: bytes=32 time=120ms TTL=124
    Reply from 192.168.239.132: bytes=32 time=120ms TTL=124
```

### <a name="examples"></a>Ejemplos

Para hacer ping en el 10.0.99.221 de destino y resolver 10.0.99.221 en su nombre de host, escriba:

```
ping /a 10.0.99.221
```

Para hacer ping en el 10.0.99.221 de destino con 10 mensajes de solicitud de eco, cada uno de los cuales tiene un campo de datos de 1000 bytes, escriba:

```
ping /n 10 /l 1000 10.0.99.221
```

Para hacer ping en el 10.0.99.221 de destino y grabar la ruta para 4 saltos, escriba:

```
ping /r 4 10.0.99.221
```

Para hacer ping en el 10.0.99.221 de destino y especificar la ruta de origen flexible de 10.12.0.1-10.29.3.1-10.1.44.1, escriba:

```
ping /j 10.12.0.1 10.29.3.1 10.1.44.1 10.0.99.221
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
