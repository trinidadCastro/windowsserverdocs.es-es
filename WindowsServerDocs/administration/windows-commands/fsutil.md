---
ms.assetid: 2e748187-6a10-4bb0-aed5-34f886a250d2
title: Fsutil
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 08/21/2018
ms.openlocfilehash: 0f338c80dca0ed88ca206aea5aeb415bc191e03a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376672"
---
# <a name="fsutil"></a>Fsutil

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Realiza tareas relacionadas con la tabla de asignación de archivos (FAT) y los sistemas de archivos NTFS, como administrar puntos de reanálisis, administrar archivos dispersos o desmontar un volumen. Si se usa sin parámetros, **fsutil** muestra una lista de los subcomandos admitidos. 

> [!Note] 
> Debe haber iniciado sesión como administrador o miembro del grupo administradores para usar fsutil. El comando fsutil es bastante eficaz y solo lo deben usar usuarios avanzados que tengan un conocimiento exhaustivo de los sistemas operativos Windows.
>
>Debe habilitar el subsistema de Windows para Linux para poder ejecutar **fsutil**. Ejecute el siguiente comando como administrador en PowerShell para habilitar esta característica opcional:
>
>```
> Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
>```
> Se le pedirá que reinicie el equipo una vez instalado. Una vez reiniciado el equipo, podrá ejecutar **fsutil** como administrador.

## <a name="parameters"></a>Parámetros

|Subcomando |Descripción|
|---|---|
|[Fsutil 8dot3name](fsutil-8dot3name.md) | Consulta o cambia la configuración del comportamiento de nombre corto en el sistema, por ejemplo, genera nombres de archivo de 8,3 caracteres de longitud. Quita los nombres cortos de todos los archivos de un directorio. Examina un directorio e identifica las claves del registro que podrían verse afectadas si los nombres cortos se han quitado de los archivos del directorio.|
|[Comportamiento de fsutil](fsutil-behavior.md) |Consulta o establece el comportamiento del volumen.|
|[Fsutil Dirty](fsutil-dirty.md)| Consulta si el bit de integridad del volumen está establecido o establece el bit de integridad de un volumen. Cuando se establece el bit de integridad de un volumen, **Autochk** comprueba automáticamente si hay errores en el volumen la próxima vez que se reinicie el equipo.|
|[Fsutil (archivo)](fsutil-file.md)|Busca un archivo por su nombre de usuario (si las cuotas de disco están habilitadas), consulta los intervalos asignados de un archivo, establece el nombre corto de un archivo, establece la longitud de datos válidos de un archivo, establece cero datos para un archivo, crea un nuevo archivo de un tamaño especificado, busca un identificador de archivo si se le da el nombre. , o encuentra un nombre de vínculo de archivo para un identificador de archivo especificado.|
|[Fsutil fsinfo](fsutil-fsinfo.md)|Muestra todas las unidades y consulta el tipo de unidad, la información de volumen, la información de volumen específica de NTFS o las estadísticas del sistema de archivos.|
|[Fsutil hardlink](fsutil-hardlink.md)|Enumera los vínculos físicos de un archivo o crea un vínculo físico (una entrada de directorio para un archivo). Se puede considerar que cada archivo tiene al menos un vínculo físico. En volúmenes NTFS, cada archivo puede tener varios vínculos físicos, por lo que un único archivo puede aparecer en muchos directorios (o incluso en el mismo directorio, con nombres diferentes). Dado que todos los vínculos hacen referencia al mismo archivo, los programas pueden abrir cualquiera de los vínculos y modificar el archivo. Un archivo se elimina del sistema de archivos solo después de que se eliminen todos los vínculos a él. Después de crear un vínculo físico, los programas pueden usarlo como cualquier otro nombre de archivo.|
|[Fsutil objectId](fsutil-objectid.md)|Administra los identificadores de objeto, que utiliza el sistema operativo Windows para realizar el seguimiento de objetos como archivos y directorios.|
|[Cuota de fsutil](fsutil-quota.md)|Administra las cuotas de disco en volúmenes NTFS para proporcionar un control más preciso del almacenamiento basado en red. Las cuotas de disco se implementan por volumen y permiten que se implementen límites de almacenamiento de forma rígida y flexible por usuario.|
|[Fsutil repair](fsutil-repair.md)|Consulta o establece el estado de recuperación automática del volumen. NTFS de recuperación automática intenta corregir los daños del sistema de archivos NTFS en línea sin necesidad de ejecutar **CHKDSK. exe** . Incluye el inicio de la comprobación en el disco y la espera de la finalización de la reparación.|
|[Fsutil reparsepoint](fsutil-reparsepoint.md)|Consulta o elimina puntos de reanálisis (objetos del sistema de archivos NTFS que tienen un atributo definible que contiene datos controlados por el usuario). Los puntos de reanálisis se utilizan para extender la funcionalidad en el subsistema de entrada/salida (e/s). Se usan para puntos de unión de directorios y puntos de montaje de volumen. También se usan en los controladores de filtro del sistema de archivos para marcar determinados archivos como especiales para ese controlador.|
|[Fsutil (recurso)](fsutil-resource.md)|Crea un Administrador de recursos transaccional secundario, inicia o detiene una Administrador de recursos transaccional, muestra información acerca de una Administrador de recursos transaccional o modifica su comportamiento.|
|[Fsutil Sparse](fsutil-sparse.md)|Administra archivos dispersos. Un archivo disperso es un archivo con una o más regiones de datos sin asignar. Un programa verá estas regiones sin asignar que contienen bytes con el valor cero, pero no se usa espacio en disco para representar estos ceros. Se asignan todos los datos significativos o distintos de cero, mientras que no se asignan todos los datos no significativos (cadenas grandes de datos compuestos de ceros). Cuando se lee un archivo disperso, los datos asignados se devuelven como almacenados y los datos no asignados se devuelven como ceros (de forma predeterminada, de acuerdo con la especificación de los requisitos de seguridad C2). La compatibilidad con archivos dispersos permite cancelar la asignación de los datos desde cualquier parte del archivo.|
|[Niveles de fsutil](fsutil-tiering.md)|Habilita la administración de funciones de nivel de almacenamiento, como la configuración y deshabilitación de marcas y la lista de niveles.|
|[Fsutil, transacción](fsutil-transaction.md)|Confirma una transacción especificada, revierte una transacción especificada o muestra información sobre la transacción.|
|[Fsutil USN](fsutil-usn.md)|Administra el diario de cambios del número de secuencias actualizadas (USN), que proporciona un registro persistente de todos los cambios realizados en los archivos del volumen.|
|[Fsutil Volume](fsutil-volume.md)|Administra un volumen. Desmonta un volumen, consulta para ver cuánto espacio libre está disponible en un disco o encuentra un archivo que usa un clúster especificado.|
|[Fsutil Wim](fsutil-wim.md)|Proporciona funciones para detectar y administrar archivos respaldados por WIM.|

## <a name="see-also"></a>Vea también
[Clave de sintaxis de línea de comandos](Command-Line-Syntax-Key.md)