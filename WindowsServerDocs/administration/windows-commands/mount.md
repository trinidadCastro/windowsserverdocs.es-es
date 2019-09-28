---
title: montar
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 9a225c847055198a9a48962a3b40969556f10ec1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373588"
---
# <a name="mount"></a>montar



Puede usar **montar** para montar recursos compartidos de red de Network File System (NFS).

## <a name="syntax"></a>Sintaxis

```
mount [-o <Option>[...]] [-u:<UserName>] [-p:{<Password> | *}] {\\<ComputerName>\<ShareName> | <ComputerName>:/<ShareName>} {<DeviceName> | *}
```

## <a name="description"></a>Descripción

La utilidad de línea de comandos **Mount** monta el sistema de archivos identificado por *ShareName* exportado por el servidor NFS identificado por *ComputerName* y lo asocia a la letra de unidad especificada por *DeviceName* o, si es un asterisco ( **&#42;** ), por la primera letra de controlador disponible. Los usuarios pueden entonces tener acceso al sistema de archivos exportado como si fuera una unidad en el equipo local. Cuando se usa sin opciones o argumentos, **montar** muestra información sobre todos los sistemas de archivos NFS montados.

La utilidad **Mount** solo está disponible si está instalado cliente para NFS.

Se pueden usar las siguientes opciones y argumentos con la utilidad **Mount** .


|          Término          |                                                                                                                                                                                                                                                Definición                                                                                                                                                                                                                                                |
|------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| -o rsize = \<buffersize > |                                                                                                                                                                                            Establece el tamaño, en kilobytes, del búfer de lectura. Los valores aceptables son 1, 2, 4, 8, 16 y 32; el valor predeterminado es 32 KB.                                                                                                                                                                                            |
| -o wsize = \<buffersize > |                                                                                                                                                                                           Establece el tamaño, en kilobytes, del búfer de escritura. Los valores aceptables son 1, 2, 4, 8, 16 y 32; el valor predeterminado es 32 KB.                                                                                                                                                                                            |
| -o timeout = \<seconds >  |                                                                                                                                                                       Establece el valor de tiempo de espera en segundos para una llamada a procedimiento remoto (RPC). Los valores aceptables son 0,8, 0,9 y cualquier entero del intervalo 1-60; el valor predeterminado es 0,8.                                                                                                                                                                       |
|   -o Retry = \<number >   |                                                                                                                                                                                             Establece el número de reintentos para un montaje flexible. Los valores aceptables son enteros en el intervalo 1-10; el valor predeterminado es 1.                                                                                                                                                                                             |
|     -o mtype = {Soft     |                                                                                                                                                                                                                                                  forzado                                                                                                                                                                                                                                                   |
|        -o Anon         |                                                                                                                                                                                                                                       Monta como usuario anónimo.                                                                                                                                                                                                                                       |
|       -o NOLOCK        |                                                                                                                                                                                                                                Deshabilita el bloqueo (el valor predeterminado es **habilitado**).                                                                                                                                                                                                                                |
|    -o CaseSensitive    |                                                                                                                                                                                                                         Fuerza a las búsquedas de archivos en el servidor a distinguir entre mayúsculas y minúsculas.                                                                                                                                                                                                                          |
| -o FileAccess = \<mode >  | Especifica el modo de permisos predeterminado de los nuevos archivos creados en el recurso compartido de NFS. Especifique el *modo* como un número de tres dígitos con el formato *ogw*, donde *o*, *g*y *w* son un dígito que representa el acceso concedido al propietario, grupo y mundo del archivo, respectivamente. Los dígitos deben estar en el intervalo 0-7 con el significado siguiente:</br>0,1 Sin acceso</br>-1: x (acceso de ejecución)</br>-2: w (acceso de escritura)</br>-3: WX</br>-4: r (acceso de lectura)</br>-5: RX</br>-6: RW</br>-7: rwx |
|    -o lang = {EUC-JP     |                                                                                                                                                                                                                                                  EUC-TW                                                                                                                                                                                                                                                  |
|     -u: \<UserName >     |                                                                                                                                                                             Especifica el nombre de usuario que se va a usar para montar el recurso compartido. Si *el nombre de usuario no* está precedido por una barra diagonal inversa ( **\\** ), se trata como un nombre de usuario de Unix.                                                                                                                                                                             |
|     -p: \<Password >     |                                                                                                                                                                                          Contraseña que se va a usar para montar el recurso compartido. Si usa un asterisco ( **&#42;** ), se le pedirá la contraseña.                                                                                                                                                                                          |

> [!NOTE]
