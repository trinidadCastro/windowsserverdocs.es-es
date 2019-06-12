---
title: nbtstat
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1d2ea99e-72f1-471f-9525-d2c49bf3be82
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4e670b1490f1c4c54b8cf377d48755849faa16f8
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437123"
---
# <a name="nbtstat"></a>nbtstat

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Caché de nombres NetBIOS a través de las estadísticas de protocolo TCP/IP (NetBT), las tablas de nombre de NetBIOS para el equipo local y los equipos remotos de muestra y NetBIOS. **nbtstat** permite una actualización de la caché de nombres NetBIOS y los nombres registrados con el servicio de nombres Internet de Windows (WINS). Se utiliza sin parámetros, **nbtstat** muestra la Ayuda. 

## <a name="syntax"></a>Sintaxis

```
nbtstat [/a <remoteName>] [/A <IPaddress>] [/c] [/n] [/r] [/R] [/RR] [/s] [/S] [<Interval>]
```

### <a name="parameters"></a>Parámetros

|    Parámetro    |                                                                                                                         Descripción                                                                                                                         |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /a <remoteName> |    Muestra la tabla de nombres NetBIOS de un equipo remoto, donde *remoteName* es el nombre de equipo NetBIOS del equipo remoto. La tabla de nombres NetBIOS es la lista de nombres NetBIOS que corresponde a las aplicaciones NetBIOS que se ejecutan en ese equipo.     |
| /A <IPaddress>  |                                                           Muestra la tabla de nombres NetBIOS de un equipo remoto, especificado por la dirección IP (en notación decimal con puntos) del equipo remoto.                                                            |
|       /c        |                                                                        Muestra el contenido de los nombres NetBIOS caché, la tabla de nombres NetBIOS y sus direcciones IP resueltas.                                                                         |
|       /n        |                                            Muestra la tabla de nombres NetBIOS del equipo local. El estado de **registrado** indica que el nombre está registrado mediante la difusión o con un servidor WINS.                                             |
|       /r        |      Muestra las estadísticas de resolución de nombres NetBIOS. En un equipo que ejecuta Windows XP o Windows Server 2003 que está configurado para usar WINS, este parámetro devuelve el número de nombres que se han resuelto y registrados mediante difusión y WINS.       |
|       /R        |                                                                      Purga el contenido de la caché de nombres NetBIOS y, a continuación, vuelve a cargar el #tagged entradas desde el **Lmhosts** archivo.                                                                      |
|       /RR       |                                                                           Libera y, a continuación, actualiza los nombres de NetBIOS del equipo local que está registrado con servidores WINS.                                                                            |
|       /s        |                                                                          Muestra el cliente y el servidor las sesiones NetBIOS, intenta convertir la dirección IP de destino en un nombre.                                                                           |
|       /S        |                                                                          Muestra el cliente y el servidor las sesiones NetBIOS, enumera los equipos remotos mediante la dirección IP de destino solo.                                                                          |
|   <Interval>    | Vuelve a mostrar las estadísticas seleccionadas, poner en pausa el número de segundos especificado en *intervalo* entre cada muestra. Presione CTRL+C para detener la presentación de estadísticas. Si se omite este parámetro, **nbtstat** imprime sólo una vez la información de configuración actual. |
|       /?        |                                                                                                            Muestra la ayuda en el símbolo del sistema.                                                                                                             |

## <a name="remarks"></a>Comentarios

-   **nbtstat** parámetros de línea de comandos distinguen mayúsculas de minúsculas.

-   La tabla siguiente describen los encabezados de columna que se generan mediante **nbtstat**:

    |Encabezado|Descripción|
    |------|--------|
    |Entrada|El número de bytes recibidos.|
    |Salida|El número de bytes enviados.|
    |Entrada/salida|Si la conexión es desde el equipo (saliente) o desde otro equipo en el equipo local (entrante).|
    |Vida|El tiempo restante que una entrada de caché de la tabla de nombres durará antes de que se purgue.|
    |Nombre local|El nombre de NetBIOS local asociado a la conexión.|
    |Host remoto|El nombre o dirección IP asociada con el equipo remoto.|
    |<03>|El último byte de un nombre NetBIOS se convierte en formato hexadecimal. Cada nombre de NetBIOS tiene 16 caracteres. Este último byte suele tener una importancia especial porque el mismo nombre puede existir varias veces en un equipo, solo se diferencia en el último byte. Por ejemplo, < 20 > es un espacio en texto ASCII.|
    |type|El tipo de nombre. Un nombre puede ser un nombre único o un nombre de grupo.|
    |Estado|Si el servicio NetBIOS en el equipo remoto se está ejecutando (registrado) o un nombre de equipo duplicado ha registrado el mismo servicio (conflicto).|
    |Estado|El estado de las conexiones de NetBIOS.|

-   En la tabla siguiente se describe los posibles estados de conexión de NetBIOS:

    |Estado|Descripción|
    |-----|--------|
    |Conectado|Se ha establecido una sesión.|
    |Asociados|Un punto de conexión creado y asociado con una dirección IP.|
    |escuchando|Este punto de conexión está disponible para una conexión entrante.|
    |Inactivo|Este punto de conexión se ha abierto pero no puede recibir conexiones.|
    |Conectar|Una sesión está en fase de conexión y se resuelve la asignación de direcciones IP del nombre del destino.|
    |Aceptar|Una sesión de entrada actualmente que se acepta y se conectará en breve.|
    |Volver a conectar|Una sesión está intentando volver a conectar (lo que no se pudo conectar en el primer intento).|
    |Saliente|Una sesión está en fase de conexión y se está creando la conexión TCP.|
    |Entrante|Una sesión de entrada está en fase de conexión.|
    |Desconectar|Una sesión está en proceso de desconexión.|
    |Disconnected|El equipo local ha emitido una desconexión y está esperando la confirmación desde el sistema remoto.|

-   Este comando está disponible sólo si el protocolo del protocolo de Internet (TCP/IP) se instala como un componente en las propiedades de un adaptador de red en las conexiones de red.

## <a name="BKMK_Examples"></a>Ejemplos
Para mostrar la tabla de nombres NetBIOS del equipo remoto con el nombre NetBIOS del equipo de CORP07, escriba:

```
nbtstat /a CORP07
```

Para mostrar la tabla de nombres NetBIOS del equipo remoto que se le asignada la dirección IP 10.0.0.99, escriba:

```
nbtstat /A 10.0.0.99
```

Para mostrar la tabla de nombres NetBIOS del equipo local, escriba:

```
nbtstat /n
```

Para mostrar el contenido de la caché de nombres NetBIOS del equipo local, escriba:

```
nbtstat /c
```

Purgar la caché de nombres NetBIOS y volver a cargar el #tagged entradas en el archivo Lmhosts local, escriba:

```
nbtstat /R
```

Para liberar los nombres NetBIOS registrados en el servidor WINS y volver a registrarlas, escriba:

```
nbtstat /RR
```

Para mostrar las estadísticas de sesión de NetBIOS mediante una dirección IP de cada cinco segundos, escriba:

```
nbtstat /S 5
```

## <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)


