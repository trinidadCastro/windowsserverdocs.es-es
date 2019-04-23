---
title: nfsshare
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 50df88a3fddbceb1595f328bdd4e3c6f526e3a2c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881696"
---
# <a name="nfsshare"></a>nfsshare



Puede usar **nfsshare** al control de recursos compartidos de Network File System (NFS).

## <a name="syntax"></a>Sintaxis

```
nfsshare <ShareName>=<Drive:Path> [-o <Option=value>...]
nfsshare {<ShareName> | <Drive>:<Path> | * } /delete
```

## <a name="description"></a>Descripción

Sin argumentos, el **nfsshare** utilidad de línea de comandos enumera todos los recursos compartidos de Network File System (NFS) que exportó en servidor para NFS. Con *ShareName* como único argumento, **nfsshare** se enumeran las propiedades del recurso compartido NFS identificado por *ShareName*. Cuando *ShareName* y *unidad ***:*** ruta* se proporcionan, **nfsshare** exporta la carpeta identificada por *deunidad ***:*** Ruta* como *ShareName*. Cuando el **/eliminar** se usa la opción, la carpeta especificada ya no estará disponible para los clientes NFS.

## <a name="options"></a>Opciones

El **nfsshare** comando acepta los siguientes argumentos y opciones:

|Término|Definición|
|----|----------|
|-o anon = {yes | no}|Especifica si los usuarios anónimos de (sin asignar) pueden tener acceso al directorio compartido. El valor predeterminado es **ningún**.|
|rw -o [=\<Host > [:<Host>]...]|Proporciona acceso de lectura y escritura al directorio compartido a los hosts o cliente grupos especificados por *Host*. Separe los nombres de host y el grupo con dos puntos (**:**). Si *Host* no se especifica, todos los hosts y grupos de clientes (las especificadas con excepto el **ro** opción) tienen acceso de lectura y escritura. Si no la **ro** ni **rw** opción está activada, todos los clientes tienen acceso de lectura y escritura al directorio compartido.|
|ro -o [=\<Host > [:<Host>]...]|Proporciona acceso de solo lectura al directorio compartido a los hosts o cliente grupos especificados por *Host*. Separe los nombres de host y el grupo con dos puntos (**:**). Si *Host* no se especifica, todos los clientes (las especificadas con excepto el **rw** opción) tienen acceso de solo lectura. Si el **ro** opción está establecida para uno o varios clientes, pero la **rw** opción no está establecida, solo los clientes especificado con el **ro** opción tienen acceso al directorio compartido.|
|-o codificación = {big5|euc-jp|euc-kr|euc-tw|gb2312-80|ksc5601|shift-jis}|Especifica la codificación predeterminada usa los nombres de archivo y directorio y, si usa, debe establecerse en uno de los siguientes:</br>-   **Big5** (chino)</br>-   **euc-jp** (japonés)</br>-   **euc-kr** (coreano)</br>-   **euc-tw** (chino)</br>-   **GB2312-80** (chino simplificado)</br>-   **KSC5601** (coreano)</br>-   **shift-jis** (japonés)</br>Si esta opción no es está establecido, el esquema de codificación predeterminada es ANSI o, en los sistemas con configuraciones regionales distintas del inglés, la codificación para la configuración regional predeterminada. Estos son los esquemas de codificación predeterminado para las configuraciones regionales indicadas:</br>-Japonés: SHIFT-JIS</br>-Coreano: KS_C_5601-1987</br>-Chino simplificado: GB2312-80</br>Chino tradicional: BIG5|
|-o anongid =\<gid >|Especifica que los de usuarios anónimos (sin asignar) tendrán acceso del directorio del recurso compartido con *gid* como identificador de grupo (GID). El valor predeterminado es -2. El GID anónimo se utilizará al notificar el propietario de un archivo perteneciente a un usuario sin asignar, incluso si el acceso anónimo está deshabilitado.|
|-o anonuid =\<uid >|Especifica que los de usuarios anónimos (sin asignar) tendrán acceso del directorio del recurso compartido con *uid* como identificador de usuario (UID). El valor predeterminado es -2. El UID anónimo se utilizará al notificar el propietario de un archivo perteneciente a un usuario sin asignar, incluso si el acceso anónimo está deshabilitado.|
|raíz -o [=\<Host > [:<Host>]...]|Proporciona acceso a la raíz al directorio compartido a los hosts o cliente grupos especificados por *Host*. Separe los nombres de host y el grupo con dos puntos (**:**). Si *Host* no se especifica, todos los clientes tienen acceso a la raíz. Si el **raíz** opción no está establecida, ningún cliente tiene acceso a la raíz al directorio compartido.|
|/delete|Si *ShareName* o *unidad ***:*** ruta* se especifica, se elimina el recurso compartido especificado. Si * se especifica, elimina todos los recursos compartidos NFS.|

> [!NOTE]
> Para ver la sintaxis completa de este comando, en una ventana de símbolo del sistema, escriba:</br>> **nfsshare /?**

##

[Servicios para Network File System comando referencia](services-for-network-file-system-command-reference.md) Vea también