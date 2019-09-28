---
title: nbtstat
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: bd9148c922ac97e83b3b21bcb8b6585775505bf2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373320"
---
# <a name="nbtstat"></a>nbtstat

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra las estadísticas de protocolo NetBIOS sobre TCP/IP (NetBT), las tablas de nombre NetBIOS para el equipo local y los equipos remotos, y la caché de nombres NetBIOS. **Nbtstat** permite una actualización de la caché de nombres NetBIOS y los nombres registrados en el servicio de nombres Internet de Windows (WINS). Si se usa sin parámetros, **Nbtstat** muestra la ayuda. 

## <a name="syntax"></a>Sintaxis

```
nbtstat [/a <remoteName>] [/A <IPaddress>] [/c] [/n] [/r] [/R] [/RR] [/s] [/S] [<Interval>]
```

### <a name="parameters"></a>Parámetros

|    Parámetro    |                                                                                                                         Descripción                                                                                                                         |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /a <remoteName> |    Muestra la tabla de nombres NetBIOS de un equipo remoto, donde *remoteName* es el nombre de equipo NetBIOS del equipo remoto. La tabla de nombres NetBIOS es la lista de nombres NetBIOS que corresponde a las aplicaciones NetBIOS que se ejecutan en ese equipo.     |
| /A <IPaddress>  |                                                           Muestra la tabla de nombres NetBIOS de un equipo remoto, especificada por la dirección IP (en notación decimal con puntos) del equipo remoto.                                                            |
|       /c        |                                                                        Muestra el contenido de la caché de nombres NetBIOS, la tabla de nombres NetBIOS y sus direcciones IP resueltas.                                                                         |
|       /n        |                                            Muestra la tabla de nombres NetBIOS del equipo local. El estado **registrado** indica que el nombre se registra mediante difusión o con un servidor WINS.                                             |
|       /r        |      Muestra las estadísticas de resolución de nombres NetBIOS. En un equipo que ejecuta Windows XP o Windows Server 2003 y que está configurado para usar WINS, este parámetro devuelve el número de nombres que se han resuelto y registrado mediante difusión y WINS.       |
|       /R        |                                                                      Purga el contenido de la caché de nombres NetBIOS y, a continuación, vuelve a cargar las entradas etiquetadas por el #PRE del archivo **Lmhosts** .                                                                      |
|       /RR       |                                                                           Libera y, a continuación, actualiza los nombres NetBIOS del equipo local que está registrado con servidores WINS.                                                                            |
|       /s        |                                                                          Muestra las sesiones de cliente y servidor NetBIOS, intentando convertir la dirección IP de destino en un nombre.                                                                           |
|       /S        |                                                                          Muestra las sesiones de cliente y servidor de NetBIOS, que solo muestran los equipos remotos por dirección IP de destino.                                                                          |
|   <Interval>    | Vuelve a mostrar las estadísticas seleccionadas y pausa el número de segundos especificados en *intervalo* entre cada pantalla. Presione CTRL + C para detener la visualización de las estadísticas. Si se omite este parámetro, **Nbtstat** imprime la información de configuración actual una sola vez. |
|       /?        |                                                                                                            Muestra la ayuda en el símbolo del sistema.                                                                                                             |

## <a name="remarks"></a>Comentarios

-   los parámetros de la línea de comandos **Nbtstat** distinguen mayúsculas de minúsculas.

-   En la tabla siguiente se describen los encabezados de columna generados por **Nbtstat**:

    |Encabezado|Descripción|
    |------|--------|
    |Entrada|Número de bytes recibidos.|
    |Salida|Número de bytes enviados.|
    |Entrada/salida|Si la conexión procede del equipo (saliente) o de otro equipo al equipo local (entrante).|
    |Dura|La hora a la que se activará una entrada de la caché de tabla de nombres antes de que se purgue.|
    |Nombre local|Nombre NetBIOS local asociado a la conexión.|
    |Host remoto|El nombre o la dirección IP asociada al equipo remoto.|
    |< 03 >|El último byte de un nombre NetBIOS convertido en hexadecimal. Cada nombre NetBIOS tiene una longitud de 16 caracteres. Este último byte suele tener una importancia especial, ya que el mismo nombre puede estar presente varias veces en un equipo, que solo difiere en el último byte. Por ejemplo, < 20 > es un espacio en texto ASCII.|
    |type|Tipo de nombre. Un nombre puede ser un nombre único o un nombre de grupo.|
    |Estado|Si el servicio NetBIOS del equipo remoto se está ejecutando (registrado) o un nombre de equipo duplicado ha registrado el mismo servicio (conflicto).|
    |Estado|El estado de las conexiones NetBIOS.|

-   En la tabla siguiente se describen los posibles estados de conexión NetBIOS:

    |Estado|Descripción|
    |-----|--------|
    |Conectado|Se ha establecido una sesión.|
    |asociadas|Se ha creado un extremo de conexión y se ha asociado a una dirección IP.|
    |Escucha|Este extremo está disponible para una conexión entrante.|
    |Inactivo|Este punto de conexión se ha abierto pero no puede recibir conexiones.|
    |Conectarse|Una sesión se encuentra en la fase de conexión y se está resolviendo la asignación de nombre a dirección IP del destino.|
    |Aceptar|Una sesión entrante se está aceptando actualmente y se conectará en breve.|
    |Reconexión|Una sesión está intentando volver a conectarse (no se pudo conectar en el primer intento).|
    |Saliente|Una sesión se encuentra en la fase de conexión y la conexión TCP se está creando actualmente.|
    |Entrante|Una sesión entrante está en la fase de conexión.|
    |Desconectar|Una sesión se encuentra en el proceso de desconexión.|
    |Disconnected|El equipo local ha emitido una desconexión y está esperando la confirmación del sistema remoto.|

-   Este comando solo está disponible si el Protocolo Protocolo de Internet (TCP/IP) se instala como componente en las propiedades de un adaptador de red en las conexiones de red.

## <a name="BKMK_Examples"></a>Example
Para mostrar la tabla de nombres NetBIOS del equipo remoto con el nombre de equipo NetBIOS de CORP07, escriba:

```
nbtstat /a CORP07
```

Para mostrar la tabla de nombres NetBIOS del equipo remoto asignada la dirección IP de 10.0.0.99, escriba:

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

Para purgar la caché de nombres NetBIOS y volver a cargar las entradas etiquetadas por el #PRE en el archivo Lmhosts local, escriba:

```
nbtstat /R
```

Para liberar los nombres NetBIOS registrados en el servidor WINS y volver a registrarlos, escriba:

```
nbtstat /RR
```

Para mostrar las estadísticas de la sesión de NetBIOS por dirección IP cada cinco segundos, escriba:

```
nbtstat /S 5
```

## <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)


