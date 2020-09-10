---
title: nfsstat
description: Artículo de referencia para el comando nfsstat, que muestra información estadística acerca de las llamadas de Network File System (NFS) y llamada a procedimiento remoto (RPC).
ms.topic: reference
ms.assetid: da7a9768-44bd-404b-97ee-c388d00dc395
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: d0efecc7a23904a306221064eeccbeeee8f2c598
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89635804"
---
# <a name="nfsstat"></a>nfsstat

Utilidad de línea de comandos que muestra información estadística acerca de las llamadas de Network File System (NFS) y llamada a procedimiento remoto (RPC). Si se usa sin parámetros, este comando muestra todos los datos estadísticos sin restablecer nada.

## <a name="syntax"></a>Sintaxis

```
nfsstat [-c][-s][-n][-r][-z][-m]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| -c | Muestra solo las llamadas NFS y RPC y NFS del lado cliente enviadas y rechazadas por el cliente. Para mostrar solo la información de NFS o RPC, combine esta marca con el parámetro **-n** o **-r** . |
| -S | Muestra solo las llamadas NFS y RPC y NFS del servidor enviadas y rechazadas por el servidor. Para mostrar solo la información de NFS o RPC, combine esta marca con el parámetro **-n** o **-r** . |
| -M | Muestra información sobre las marcas de montaje establecidas por las opciones de montaje, las marcas de montaje internas del sistema y otra información de montaje. |
| -n | Muestra información de NFS tanto para el cliente como para el servidor. Para mostrar solo la información del cliente o del servidor NFS, combine esta marca con el parámetro **-c** o **-s** . |
| -r | Muestra información de RPC para el cliente y el servidor. Para mostrar solo la información de servidor o cliente RPC, combine esta marca con el parámetro **-c** o **-s** . |
| -Z | Restablece las estadísticas de llamadas. Esta marca solo está disponible para el usuario raíz y se puede combinar con cualquiera de los demás parámetros para restablecer determinados conjuntos de estadísticas después de mostrarlos. |

### <a name="examples"></a>Ejemplos

Para mostrar información sobre el número de llamadas de RPC y NFS enviadas y rechazadas por el cliente, escriba:

```
nfsstat -c
```

Para mostrar e imprimir la información relacionada con la llamada de NFS del cliente, escriba:

```
nfsstat -cn
```

Para mostrar información relacionada con llamadas RPC para el cliente y el servidor, escriba:

```
nfsstat -r
```

Para mostrar información sobre el número de llamadas de RPC y NFS recibidas y rechazadas por el servidor, escriba:

```
nfsstat -s
```

Para restablecer toda la información relacionada con llamadas a cero en el cliente y el servidor, escriba:

```
nfsstat -z
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Servicios de referencia de comandos de sistema de archivos de red](services-for-network-file-system-command-reference.md)

- [Referencia de cmdlets de NFS](/powershell/module/nfs)
