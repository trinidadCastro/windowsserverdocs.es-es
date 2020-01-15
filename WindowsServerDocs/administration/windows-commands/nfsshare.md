---
title: nfsshare
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 437a2615-335a-442f-9713-d50d5f3983a3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d205bcfad11d22fea7fc9d0651aca61f234347cf
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2020
ms.locfileid: "75948502"
---
# <a name="nfsshare"></a>nfsshare



Puede usar **nfsshare** para controlar los recursos compartidos de Network File System (NFS).

## <a name="syntax"></a>Sintaxis

```
nfsshare <ShareName>=<Drive:Path> [-o <Option=value>...]
nfsshare {<ShareName> | <Drive>:<Path> | * } /delete
```

## <a name="description"></a>Descripción

Sin argumentos, la utilidad de línea de comandos **nfsshare** enumera todos los recursos compartidos de Network File System (NFS) exportados por servidor para NFS. Con *ShareName* como único argumento, **nfsshare** enumera las propiedades del recurso compartido de NFS identificado por *ShareName*. Cuando se proporcionan *ShareName* y <em>Drive</em> **:** <em>path</em> , **nfsshare** exporta la carpeta identificada por <em>Drive</em> **:** <em>path</em> como *ShareName*. Cuando se usa la opción **/Delete** , la carpeta especificada deja de estar disponible para los clientes NFS.

## <a name="options"></a>botón

El comando **nfsshare** acepta las opciones y los argumentos siguientes:


|             Término              |                                                                                                                                                                                                                      Definición                                                                                                                                                                                                                       |
|-------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         -o Anon = {sí          |                                                                                                                                                                                                                          ninguno                                                                                                                                                                                                                          |
|  -o RW [=\<host > [:<Host>]...]  |                       Proporciona acceso de lectura y escritura al directorio compartido por los hosts o grupos de clientes especificados por el *host*. Separe los nombres de host y grupo con dos puntos ( **:** ). Si no se especifica *host* , todos los hosts y grupos de clientes (excepto los especificados con la opción **ro** ) tienen acceso de lectura y escritura. Si no se establece la opción **ro** ni **RW** , todos los clientes tienen acceso de lectura y escritura al directorio compartido.                       |
|  -o ro [=\<host > [:<Host>]...]  | Proporciona acceso de solo lectura al directorio compartido por los hosts o grupos de clientes especificados por *host*. Separe los nombres de host y grupo con dos puntos ( **:** ). Si no se especifica *host* , todos los clientes (excepto los especificados con la opción **RW** ) tienen acceso de solo lectura. Si la opción **ro** está establecida para uno o más clientes, pero no se establece la opción **RW** , solo los clientes especificados con la opción **ro** tienen acceso al directorio compartido. |
|       -o codificación = {Big5       |                                                                                                                                                                                                                        euc-jp                                                                                                                                                                                                                         |
|       -o anongid =\<GID >       |                                                                                     Especifica que los usuarios anónimos (sin asignar) tendrán acceso al directorio compartido mediante el uso de *GID* como identificador de grupo (GID). El valor predeterminado es-2. El GID anónimo se utilizará al notificar al propietario de un archivo propiedad de un usuario sin asignar, incluso si el acceso anónimo está deshabilitado.                                                                                      |
|      -o anonuid =\<UID >       |                                                                                      Especifica que los usuarios anónimos (sin asignar) tendrán acceso al directorio compartido mediante el uso de *UID* como identificador de usuario (UID). El valor predeterminado es-2. El UID anónimo se utilizará al notificar al propietario de un archivo propiedad de un usuario sin asignar, incluso si el acceso anónimo está deshabilitado.                                                                                      |
| -o raíz [=\<host > [:<Host>]...] |                                                                         Proporciona acceso raíz al directorio compartido por los hosts o grupos de clientes especificados por *host*. Separe los nombres de host y grupo con dos puntos ( **:** ). Si no se especifica un *host* , todos los clientes tienen acceso raíz. Si no se establece la opción **root** , ningún cliente tiene acceso raíz al directorio compartido.                                                                         |
|            /delete            |                                                                                                                                                       Si se especifica *ShareName* o <em>unidad</em> **:** <em>ruta de acceso</em> , elimina el recurso compartido especificado. Si se especifica \*, elimina todos los recursos compartidos de NFS.                                                                                                                                                       |

> [!NOTE]
> Para ver la sintaxis completa de este comando, en una ventana de símbolo del sistema, escriba:</br>> **nfsshare/?**

## <a name="see-also"></a>Consulta también

[Servicios de referencia de comandos de sistema de archivos de red](services-for-network-file-system-command-reference.md)