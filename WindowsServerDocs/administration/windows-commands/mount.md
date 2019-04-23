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
ms.openlocfilehash: f9d13f5eddef80d99c11fe59c6bd5e589fa67546
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858376"
---
# <a name="mount"></a>montar



Puede usar **montar** montar recursos compartidos de red de Network File System (NFS).

## <a name="syntax"></a>Sintaxis

```
mount [-o <Option>[...]] [-u:<UserName>] [-p:{<Password> | *}] {\\<ComputerName>\<ShareName> | <ComputerName>:/<ShareName>} {<DeviceName> | *}
```

## <a name="description"></a>Descripción

El **montar** utilidad de línea de comandos identificado por el sistema de archivos monta *ShareName* exportados por el servidor NFS identificado por *ComputerName* y lo asocia a la letra de unidad especificado por *DeviceName* o, si un asterisco (**&#42;**) se usa por la primera letra de controlador disponible. Los usuarios, a continuación, pueden acceder al sistema de archivo exportado como si fuese una unidad en el equipo local. Cuando se utiliza sin argumentos, u opciones **montar** muestra información acerca de todos los sistemas de archivos montados de NFS.

El **montar** utilidad está disponible sólo si está instalado el cliente para NFS.

Los siguientes argumentos y opciones que pueden utilizarse con el **montar** utilidad.

|Término|Definición|
|----|----------|
|-o rsize =\<buffersize >|Establece el tamaño en kilobytes, del búfer de lectura. Los valores aceptables son 1, 2, 4, 8, 16 y 32; el valor predeterminado es 32 KB.|
|-o wsize =\<buffersize >|Establece el tamaño en kilobytes, del búfer de escritura. Los valores aceptables son 1, 2, 4, 8, 16 y 32; el valor predeterminado es 32 KB.|
|-o timeout=\<seconds>|Establece el valor de tiempo de espera en segundos para una llamada a procedimiento remoto (RPC). Los valores aceptables son 0,8, 0,9 y cualquier entero en el intervalo 1-60; el valor predeterminado es 0,8.|
|reintento de e/s-=\<número >|Establece el número de reintentos para un montaje flexible. Los valores aceptables son enteros que oscilan entre 1 y 10; el valor predeterminado es 1.|
|-o mtype={soft | hard}|Establece el tipo de montaje (valor predeterminado es **suave**). Independientemente del tipo de montaje, **montar** devolverá si no se puede montar inmediatamente el recurso compartido. Una vez que el recurso compartido se ha montado correctamente, sin embargo, si el tipo de montaje es **duro**, cliente para NFS seguirá intentando acceder al recurso compartido hasta que lo consiga. Como resultado, si el servidor NFS está disponible, todos los programas Windows intenta acceder al recurso compartido deje de responder, o se "bloquee", si está, aparecerá el tipo de montaje **duro**.|
|-o anon|Monta como usuario anónimo.|
|-o nolock|Deshabilita el bloqueo (valor predeterminado es **habilitado**).|
|-o casesensitive|Obliga a las búsquedas de archivos en el servidor que distingue mayúsculas de minúsculas.|
|-o fileaccess=\<mode>|Especifica el modo de permiso predeterminado de los nuevos archivos creados en el recurso compartido NFS. Especificar *modo* como un número de tres dígitos en el formulario *ogw*, donde *o*, *g*, y *w* son un dígito que representa el acceso había concedido el propietario del archivo, grupo y el mundo, respectivamente. Los dígitos deben encontrarse dentro del intervalo 0-7 con el significado siguiente:</br>-   0: Sin acceso</br>-1: x (acceso de ejecución)</br>-2: w (acceso de escritura)</br>-   3: wx</br>-4: r (acceso de lectura)</br>-5: rx</br>-6: rw</br>-   7: rwx|
|-o lang={euc-jp|euc-tw|euc-kr|shift-jis|big5|ksc5601|gb2312-80|ansi}|Especifica la codificación predeterminada usa los nombres de archivo y directorio y, si usa, debe establecerse en uno de los siguientes:</br>-   **ANSI**</br>-   **Big5** (chino)</br>-   **euc-jp** (japonés)</br>-   **euc-kr** (coreano)</br>-   **euc-tw** (chino)</br>-   **GB2312-80** (chino simplificado)</br>-   **KSC5601** (coreano)</br>-   **shift-jis** (japonés)</br>Si esta opción está establecida en **ansi** en sistemas configurados para las configuraciones regionales distintas del inglés, el esquema de codificación se establece en el esquema de codificación predeterminado para la configuración regional. Estos son los esquemas de codificación predeterminado para las configuraciones regionales indicadas:</br>-Japonés: SHIFT-JIS</br>-Coreano: KS_C_5601-1987</br>-Chino simplificado: GB2312-80</br>-Chino tradicional: BIG5|
|-u:\<UserName>|Especifica el nombre de usuario que se usará para montar el recurso compartido. Si *username* no está precedido por una barra diagonal inversa (**\**), se trata como un nombre de usuario de UNIX.|
|-p:\<contraseña >|La contraseña que se usará para montar el recurso compartido. Si usa un asterisco (**&#42;**), se le pedirá la contraseña.|

> [!NOTE]
