---
title: montar
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dd9d7ecb-ef00-4aaa-bcd0-423fa636e34a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 65083ce4b7d04129ff630a7f314c458d7ee378f4
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437240"
---
# <a name="mount"></a>montar



Puede usar **montar** montar recursos compartidos de red de Network File System (NFS).

## <a name="syntax"></a>Sintaxis

```
mount [-o <Option>[...]] [-u:<UserName>] [-p:{<Password> | *}] {\\<ComputerName>\<ShareName> | <ComputerName>:/<ShareName>} {<DeviceName> | *}
```

## <a name="description"></a>Descripción

El **montar** utilidad de línea de comandos identificado por el sistema de archivos monta *ShareName* exportados por el servidor NFS identificado por *ComputerName* y lo asocia a la letra de unidad especificado por *DeviceName* o, si un asterisco ( **&#42;** ) se usa por la primera letra de controlador disponible. Los usuarios, a continuación, pueden acceder al sistema de archivo exportado como si fuese una unidad en el equipo local. Cuando se utiliza sin argumentos, u opciones **montar** muestra información acerca de todos los sistemas de archivos montados de NFS.

El **montar** utilidad está disponible sólo si está instalado el cliente para NFS.

Los siguientes argumentos y opciones que pueden utilizarse con el **montar** utilidad.


|          Término          |                                                                                                                                                                                                                                                Definición                                                                                                                                                                                                                                                |
|------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| -o rsize =\<buffersize > |                                                                                                                                                                                            Establece el tamaño en kilobytes, del búfer de lectura. Los valores aceptables son 1, 2, 4, 8, 16 y 32; el valor predeterminado es 32 KB.                                                                                                                                                                                            |
| -o wsize =\<buffersize > |                                                                                                                                                                                           Establece el tamaño en kilobytes, del búfer de escritura. Los valores aceptables son 1, 2, 4, 8, 16 y 32; el valor predeterminado es 32 KB.                                                                                                                                                                                            |
| -o timeout=\<seconds>  |                                                                                                                                                                       Establece el valor de tiempo de espera en segundos para una llamada a procedimiento remoto (RPC). Los valores aceptables son 0,8, 0,9 y cualquier entero en el intervalo 1-60; el valor predeterminado es 0,8.                                                                                                                                                                       |
|   reintento de e/s-=\<número >   |                                                                                                                                                                                             Establece el número de reintentos para un montaje flexible. Los valores aceptables son enteros que oscilan entre 1 y 10; el valor predeterminado es 1.                                                                                                                                                                                             |
|     -o mtype={soft     |                                                                                                                                                                                                                                                  hard}                                                                                                                                                                                                                                                   |
|        -o anon         |                                                                                                                                                                                                                                       Monta como usuario anónimo.                                                                                                                                                                                                                                       |
|       -o nolock        |                                                                                                                                                                                                                                Deshabilita el bloqueo (valor predeterminado es **habilitado**).                                                                                                                                                                                                                                |
|    -o casesensitive    |                                                                                                                                                                                                                         Obliga a las búsquedas de archivos en el servidor que distingue mayúsculas de minúsculas.                                                                                                                                                                                                                          |
| -o fileaccess=\<mode>  | Especifica el modo de permiso predeterminado de los nuevos archivos creados en el recurso compartido NFS. Especificar *modo* como un número de tres dígitos en el formulario *ogw*, donde *o*, *g*, y *w* son un dígito que representa el acceso había concedido el propietario del archivo, grupo y el mundo, respectivamente. Los dígitos deben encontrarse dentro del intervalo 0-7 con el significado siguiente:</br>-   0: Sin acceso</br>-1: x (acceso de ejecución)</br>-2: w (acceso de escritura)</br>-   3: wx</br>-4: r (acceso de lectura)</br>-5: rx</br>-6: rw</br>-   7: rwx |
|    -o lang={euc-jp     |                                                                                                                                                                                                                                                  euc-tw                                                                                                                                                                                                                                                  |
|     -u:\<UserName>     |                                                                                                                                                                             Especifica el nombre de usuario que se usará para montar el recurso compartido. Si *username* no está precedido por una barra diagonal inversa ( **\\** ), se trata como un nombre de usuario de UNIX.                                                                                                                                                                             |
|     -p:\<contraseña >     |                                                                                                                                                                                          La contraseña que se usará para montar el recurso compartido. Si usa un asterisco ( **&#42;** ), se le pedirá la contraseña.                                                                                                                                                                                          |

> [!NOTE]
