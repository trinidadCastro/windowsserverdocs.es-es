---
title: nfsshare
description: Artículo de referencia para el comando nfsshare, que controla los recursos compartidos de Network File System (NFS).
ms.topic: reference
ms.assetid: 437a2615-335a-442f-9713-d50d5f3983a3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7fb0531f571e204877f6f905a60a08ef35f6ea58
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89023619"
---
# <a name="nfsshare"></a>nfsshare

Controla los recursos compartidos de Network File System (NFS). Si se usa sin parámetros, este comando muestra todos los recursos compartidos de Network File System (NFS) exportados por servidor para NFS.

## <a name="syntax"></a>Sintaxis

```
nfsshare <sharename>=<drive:path> [-o <option=value>...]
nfsshare {<sharename> | <drive>:<path> | * } /delete
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| -o Anon =`{yes|no}` | Especifica si los usuarios anónimos (sin asignar) pueden tener acceso al directorio de recursos compartidos. |
| -o RW =`[<host>[:<host>]...]` | Proporciona acceso de lectura y escritura al directorio compartido por los hosts o grupos de clientes especificados por el *host*. Debe separar los nombres de host y grupo con dos puntos (**:**). Si no se especifica un *host* , todos los hosts y grupos de clientes (excepto los especificados con la opción **ro** ) obtienen acceso de lectura y escritura. Si no se establece la opción **ro** ni **RW** , todos los clientes tienen acceso de lectura y escritura al directorio compartido. |
| -o ro =`[<host>[:<host>]...]` | Proporciona acceso de solo lectura al directorio compartido por los hosts o grupos de clientes especificados por *host*. Debe separar los nombres de host y grupo con dos puntos (**:**). Si el *host* no se especifica, todos los clientes (excepto los especificados con la opción **RW** ) obtienen acceso de solo lectura. Si la opción **ro** está establecida para uno o más clientes, pero no se establece la opción **RW** , solo los clientes especificados con la opción **ro** tienen acceso al directorio compartido. |
| -o codificación =`{euc-jp|euc-tw|euc-kr|shift-jis|Big5|Ksc5601|Gb2312-80|Ansi)` | Especifica la codificación de idioma que se va a configurar en un recurso compartido NFS. Solo puede utilizar un idioma en el recurso compartido. Este valor puede incluir cualquiera de los siguientes valores:<ul><li>**EUC-jp:** Japonés</li><li>**EUC-TW:** Chino</li><li>**EUC-KR:** Coreano</li><li>**Shift-JIS:** Japonés</li><li>**Big5:** Chino</li><li>**Ksc5601:** Coreano</li><li>**Gb2312-80:** Chino Simplificado</li><li>**ANSI:** Codificado con ANSI</li></ul> |
| -o anongid =`<gid>` | Especifica que los usuarios anónimos (sin asignar) tienen acceso al directorio de recursos compartidos mediante *GID* como identificador de grupo (GID). El valor predeterminado es **-2**. El GID anónimo se usa al notificar al propietario de un archivo que pertenece a un usuario sin asignar, incluso si el acceso anónimo está deshabilitado. |
| -o anonuid =`<uid>` | Especifica que los usuarios anónimos (sin asignar) tienen acceso al directorio de recursos compartidos mediante *UID* como identificador de usuario (UID). El valor predeterminado es **-2**. El UID anónimo se usa al notificar al propietario de un archivo que pertenece a un usuario sin asignar, incluso si el acceso anónimo está deshabilitado. |
| -o raíz =`[<host>[:<host>]...]` | Proporciona acceso raíz al directorio compartido por los hosts o grupos de clientes especificados por *host*. Debe separar los nombres de host y grupo con dos puntos (**:**). Si el *host* no se especifica, todos los clientes obtienen acceso a la raíz. Si no se establece la opción **raíz** , ningún cliente tiene acceso raíz al directorio compartido. |
| /delete | Si *sharename* `<drive>:<path>` se especifica ShareName o, este parámetro elimina el recurso compartido especificado. Si se especifica un carácter comodín (*), este parámetro elimina todos los recursos compartidos de NFS. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Observaciones

- Si *ShareName* es el único parámetro, este comando muestra las propiedades del recurso compartido de NFS identificado por *ShareName*.

- Si *sharename* `<drive>:<path>` se usan ShareName y, este comando exporta la carpeta identificada por `<drive>:<path>` como *ShareName*. Si usa la opción **/Delete** , la carpeta especificada deja de estar disponible para los clientes NFS.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Servicios de referencia de comandos de sistema de archivos de red](services-for-network-file-system-command-reference.md)

- [Referencia de cmdlets de NFS](/powershell/module/nfs)
