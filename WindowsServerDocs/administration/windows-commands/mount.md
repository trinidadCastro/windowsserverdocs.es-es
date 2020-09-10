---
title: montar
description: Artículo de referencia del comando mount, que monta recursos compartidos de red de Network File System (NFS).
ms.topic: reference
ms.assetid: dd9d7ecb-ef00-4aaa-bcd0-423fa636e34a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: cb4f3ce9d6e15aa7c6a504c1bb8e936b49f56267
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89640149"
---
# <a name="mount"></a>montar

Utilidad de línea de comandos que monta recursos compartidos de red de Network File System (NFS). Cuando se usa sin opciones o argumentos, **montar** muestra información sobre todos los sistemas de archivos NFS montados.

> [!NOTE]
> Esta utilidad solo está disponible si está instalado **cliente para NFS** .

## <a name="syntax"></a>Sintaxis

```
mount [-o <option>[...]] [-u:<username>] [-p:{<password> | *}] {\\<computername>\<sharename> | <computername>:/<sharename>} {<devicename> | *}
```

### <a name="parameters"></a>Parámetros

| Parámetro  | Descripción |
| ---------- | ----------- |
| -o rsize =`<buffersize>` | Establece el tamaño, en kilobytes, del búfer de lectura. Los valores aceptables son 1, 2, 4, 8, 16 y 32; el valor predeterminado es 32 KB. |
| -o wsize =`<buffersize>` | Establece el tamaño, en kilobytes, del búfer de escritura. Los valores aceptables son 1, 2, 4, 8, 16 y 32; el valor predeterminado es 32 KB. |
| -o tiempo de espera =`<seconds>` | Establece el valor de tiempo de espera en segundos para una llamada a procedimiento remoto (RPC). Los valores aceptables son 0,8, 0,9 y cualquier entero del intervalo 1-60; el valor predeterminado es 0,8. |
| -o reintento =`<number>` | Establece el número de reintentos para un montaje flexible. Los valores aceptables son enteros en el intervalo 1-10; el valor predeterminado es 1. |
| -o mtype =`{soft|hard}` | Establece el tipo de montaje del recurso compartido de NFS. De forma predeterminada, Windows usa un montaje flexible. Los montajes flexibles agotan el tiempo de espera más fácilmente cuando hay problemas de conexión; sin embargo, para reducir la interrupción de e/s durante los reinicios del servidor NFS, se recomienda usar un montaje forzado.|
| -o Anon | Monta como usuario anónimo. |
| -o NOLOCK | Deshabilita el bloqueo (el valor predeterminado es **habilitado**). |
| -o casesensitive | Fuerza a las búsquedas de archivos en el servidor a distinguir entre mayúsculas y minúsculas. |
| -o FileAccess =`<mode>` | Especifica el modo de permisos predeterminado de los nuevos archivos creados en el recurso compartido de NFS. Especifique el *modo* como un número de tres dígitos con el formato *ogw*, donde *o*, *g*y *w* son un dígito que representa el acceso concedido al propietario, grupo y mundo del archivo, respectivamente. Los dígitos deben estar en el intervalo de 0-7, incluido:<ul><li>**0:** Sin acceso</li><li>**1:** x (ejecutar acceso)</li><li>**2:** w (acceso de escritura)</li><li>**3:** WX (acceso de escritura y ejecución)</li><li>**4:** r (acceso de lectura)</li><li>**5:** RX (acceso de lectura y ejecución)</li><li>**6:** RW (acceso de lectura y escritura)</li><li>**7:** rwx (acceso de lectura, escritura y ejecución)</li></ul> |
| -o lang =`{euc-jp|euc-tw|euc-kr|shift-jis|Big5|Ksc5601|Gb2312-80|Ansi)` | Especifica la codificación de idioma que se va a configurar en un recurso compartido NFS. Solo puede utilizar un idioma en el recurso compartido. Este valor puede incluir cualquiera de los siguientes valores:<ul><li>**EUC-jp:** Japonés</li><li>**EUC-TW:** Chino</li><li>**EUC-KR:** Coreano</li><li>**Shift-JIS:** Japonés</li><li>**Big5:** Chino</li><li>**Ksc5601:** Coreano</li><li>**Gb2312-80:** Chino Simplificado</li><li>**ANSI:** Codificado con ANSI</li></ul> |
| 5.50`<username>` | Especifica el nombre de usuario que se va a usar para montar el recurso compartido. Si *el* nombre de usuario no está precedido por una barra diagonal inversa ( **\\** ), se trata como un nombre de usuario de Unix. |
| m`<password>` | Contraseña que se va a usar para montar el recurso compartido. Si usa un asterisco (**&#42;**), se le pedirá la contraseña. |
| `<computername>` | Especifica el nombre del servidor NFS. |
| `<sharename>` | Especifica el nombre del sistema de archivos. |
| `<devicename>` | Especifica la letra de unidad y el nombre del dispositivo. Si usa un asterisco (**&#42;**), este valor representa la primera letra de controlador disponible. |

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
