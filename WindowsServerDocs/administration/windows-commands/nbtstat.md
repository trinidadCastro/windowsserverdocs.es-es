---
title: nbtstat
description: Artículo de referencia del comando nbtstat, que muestra las estadísticas de protocolo de NetBIOS a través de TCP/IP (NetBT), las tablas de nombre NetBIOS para el equipo local y los equipos remotos, y la caché de nombres NetBIOS.
ms.topic: reference
ms.assetid: 1d2ea99e-72f1-471f-9525-d2c49bf3be82
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: f56068baab8832cb25f62e43f550fdcf7c4e1092
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89637970"
---
# <a name="nbtstat"></a>nbtstat

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Muestra las estadísticas de protocolo NetBIOS sobre TCP/IP (NetBT), las tablas de nombre NetBIOS para el equipo local y los equipos remotos, y la caché de nombres NetBIOS. Este comando también permite actualizar la caché de nombres NetBIOS y los nombres registrados en el servicio de nombres Internet de Windows (WINS). Cuando se usa sin parámetros, este comando muestra información de ayuda.

Este comando solo está disponible si el Protocolo Protocolo de Internet (TCP/IP) se instala como componente en las propiedades de un adaptador de red en las conexiones de red.

## <a name="syntax"></a>Sintaxis

```
nbtstat [/a <remotename>] [/A <IPaddress>] [/c] [/n] [/r] [/R] [/RR] [/s] [/S] [<interval>]
```

#### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| /a `<remotename>` | Muestra la tabla de nombres NetBIOS de un equipo remoto, donde *remotename* es el nombre de equipo NetBIOS del equipo remoto. La tabla de nombres NetBIOS es la lista de nombres NetBIOS que corresponde a las aplicaciones NetBIOS que se ejecutan en ese equipo. |
| /A `<IPaddress>` | Muestra la tabla de nombres NetBIOS de un equipo remoto, especificada por la dirección IP (en notación decimal con puntos) del equipo remoto. |
| /C | Muestra el contenido de la caché de nombres NetBIOS, la tabla de nombres NetBIOS y sus direcciones IP resueltas. |
| /n | Muestra la tabla de nombres NetBIOS del equipo local. El estado **registrado** indica que el nombre se registra mediante difusión o con un servidor WINS. |
| /r | Muestra las estadísticas de resolución de nombres NetBIOS. |
| /R | Purga el contenido de la caché de nombres NetBIOS y, a continuación, vuelve a cargar las entradas etiquetadas previamente del archivo **Lmhosts** . |
| /RR | Libera y, a continuación, actualiza los nombres NetBIOS del equipo local que está registrado con servidores WINS. |
| /s | Muestra las sesiones de cliente y servidor NetBIOS, intentando convertir la dirección IP de destino en un nombre. |
| /S | Muestra las sesiones de cliente y servidor de NetBIOS, que solo muestran los equipos remotos por dirección IP de destino. |
| `<interval>` | Muestra las estadísticas seleccionadas y pausa el número de segundos especificado en *intervalo* entre cada pantalla. Presione CTRL + C para dejar de mostrar las estadísticas. Si se omite este parámetro, **Nbtstat** imprime la información de configuración actual una sola vez. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Observaciones

- Los parámetros de la línea de comandos **Nbtstat** distinguen mayúsculas de minúsculas.

- Los encabezados de columna generados por el comando **Nbtstat** incluyen:

    | Dirección | Descripción |
    | ------- | ----------- |
    | Entrada | Número de bytes recibidos. |
    | Resultados | Número de bytes enviados. |
    | Dentro/fuera | Si la conexión procede del equipo (saliente) o de otro equipo al equipo local (entrante). |
    | Life | La hora a la que se activará una entrada de la caché de tabla de nombres antes de que se purgue. |
    | Nombre local | Nombre NetBIOS local asociado a la conexión. |
    | Host remoto | El nombre o la dirección IP asociada al equipo remoto. |
    | `<03>` | El último byte de un nombre NetBIOS convertido en hexadecimal. Cada nombre NetBIOS tiene una longitud de 16 caracteres. Este último byte suele tener una importancia especial, ya que el mismo nombre puede estar presente varias veces en un equipo, que solo difiere en el último byte. Por ejemplo, `<20>` es un espacio en texto ASCII. |
    | type | Tipo de nombre. Un nombre puede ser un nombre único o un nombre de grupo. |
    | Status | Si el servicio NetBIOS del equipo remoto se está ejecutando (registrado) o un nombre de equipo duplicado ha registrado el mismo servicio (conflicto). |
    | State | El estado de las conexiones NetBIOS. |

- Los posibles estados de conexión NetBIOS incluyen:

    | State | Descripción |
    | ------- | ----------- |
    | Conectado | Se ha establecido una sesión. |
    | escuchar | Este extremo está disponible para una conexión entrante. |
    | Inactivo | Este punto de conexión se ha abierto pero no puede recibir conexiones. |
    | Connecting | Una sesión se encuentra en la fase de conexión y se está resolviendo la asignación de nombre a dirección IP del destino. |
    | Aceptar | Una sesión entrante se está aceptando actualmente y se conectará en breve. |
    | Reconexión | Una sesión está intentando volver a conectarse (no se pudo conectar en el primer intento). |
    | Salida | Una sesión se encuentra en la fase de conexión y la conexión TCP se está creando actualmente. |
    | Entrada | Una sesión entrante está en la fase de conexión. |
    | Desconectando | Una sesión se encuentra en el proceso de desconexión. |
    | Escenario desconectado | El equipo local ha emitido una desconexión y está esperando la confirmación del sistema remoto. |

### <a name="examples"></a>Ejemplos

Para mostrar la tabla de nombres NetBIOS del equipo remoto con el nombre de equipo NetBIOS de *CORP07*, escriba:

```
nbtstat /a CORP07
```

Para mostrar la tabla de nombres NetBIOS del equipo remoto asignada la dirección IP de *10.0.0.99*, escriba:

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

Para purgar la caché de nombres NetBIOS y volver a cargar las entradas etiquetadas previamente en el archivo *Lmhosts* local, escriba:

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

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
