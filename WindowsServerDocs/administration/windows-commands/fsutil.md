---
ms.assetid: 2e748187-6a10-4bb0-aed5-34f886a250d2
title: Fsutil
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 08/21/2018
ms.openlocfilehash: df8d25b01b67010734deb8dd7e42f3233e6011fe
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866816"
---
# <a name="fsutil"></a>Fsutil

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Realiza tareas relacionadas con en el archivo de la tabla de asignación (FAT) y sistemas de archivos NTFS, como la administración de análisis apunta, administrar archivos dispersos o desmontar un volumen. Si se usa sin parámetros, **Fsutil** muestra una lista de los subcomandos admitidos. 

> [!Note] 
> Debe ser iniciado sesión como administrador o miembro del grupo Administradores para usar Fsutil. El comando Fsutil es bastante poderoso y se debe usar solo los usuarios avanzados que tienen un conocimiento profundo de los sistemas operativos de Windows.
>
>Tendrá que habilitar el subsistema de Windows para Linux antes de poder ejecutar **Fsutil**. Como administrador de PowerShell para habilitar esta característica opcional, ejecute el siguiente comando:
>
>```
> Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
>```
> Se le pedirá que reinicie el equipo una vez que está instalado. Una vez reiniciado el equipo, podrá ejecutar **Fsutil** como administrador.

## <a name="parameters"></a>Parámetros

|Subcomando |Descripción|
|---|---|
|[fsutil 8dot3name](fsutil-8dot3name.md) | Consulta o cambia la configuración de comportamiento de nombre corto en el sistema, por ejemplo, genera los nombres de archivo de longitud de caracteres con formato 8.3. Quita los nombres cortos para todos los archivos dentro de un directorio. Examina un directorio e identifica las claves del registro que podrían verse afectadas si se eliminan los nombres cortos de los archivos en el directorio.|
|[comportamiento de fsutil](fsutil-behavior.md) |Las consultas o establece el comportamiento de volumen.|
|[fsutil dirty](fsutil-dirty.md)| Consulta si el bit de integridad del volumen se establece o establece el bit de integridad de un volumen. Cuando un volumen del desfasadas bit está establecido, **autochk** comprueba automáticamente si hay errores en el volumen la próxima vez que se reinicie el equipo.|
|[archivo de fsutil](fsutil-file.md)|Busca un archivo por el nombre de usuario (si están habilitadas las cuotas de disco), consulta los intervalos asignados para un archivo, Establece el nombre corto de un archivo, Establece la longitud de datos válidos de un archivo, Establece los datos de cero para un archivo, crea un nuevo archivo de un tamaño especificado, busca un Id. de archivo si recibe el nombre , o se encuentra un nombre de vínculo de archivo para un identificador de archivo especificado.|
|[fsutil fsinfo](fsutil-fsinfo.md)|Enumera todas las unidades y consulta el tipo de unidad, información de volumen, información de volumen específica de NTFS o las estadísticas del sistema de archivos.|
|[fsutil hardlink](fsutil-hardlink.md)|Enumera los vínculos físicos para un archivo, o crea un vínculo físico (una entrada de directorio para un archivo). Todos los archivos se pueden considerar tener al menos un vínculo físico. En los volúmenes NTFS, cada archivo puede tener varios vínculos físicos, por lo que un único archivo puede aparecer en muchos directorios (o incluso en el mismo directorio, con nombres diferentes). Dado que todos los vínculos de referencia al mismo archivo, programas pueden abrir cualquiera de los vínculos y modificar el archivo. Solo después de que se eliminan todos los vínculos a él, se elimina un archivo del sistema de archivos. Después de crear un vínculo físico, programas pueden utilizar como cualquier otro nombre de archivo.|
|[fsutil objectid](fsutil-objectid.md)|Administra los identificadores de objeto, que se usan por el sistema operativo de Windows para realizar un seguimiento de objetos como archivos y directorios.|
|[cuota de fsutil](fsutil-quota.md)|Administra las cuotas de disco en volúmenes NTFS para proporcionar un control más preciso del almacenamiento basado en la red. Las cuotas de disco se implementan por volumen y permiten que los límites de disco duro y soft-almacenamiento para implementarse en por usuario.|
|[reparación de fsutil](fsutil-repair.md)|Las consultas o establece el estado de recuperación automática del volumen. Recuperación automática de NTFS intenta corregir daños en el sistema de archivos NTFS en línea sin necesidad de **Chkdsk.exe** para ejecutarse. Incluye iniciando la comprobación en el disco y esperar la finalización de la reparación.|
|[fsutil reparsepoint](fsutil-reparsepoint.md)|Puntos (archivo objetos del sistema NTFS que tienen un atributo definido por el que contiene datos controlados por el usuario) de análisis de consultas o eliminaciones. Repetición de análisis puntos se utilizan para extender la funcionalidad en el subsistema de entrada/salida (E/S). Se utilizan para puntos de unión de directorio y los puntos de montaje de volumen. También se usan por controladores de filtro de sistema de archivos para marcar determinados archivos especiales para el controlador.|
|[recursos de fsutil](fsutil-resource.md)|Crea un administrador de recursos secundario transaccional, se inicia o detiene un administrador de recursos transaccional, muestra información acerca de un administrador de recursos transaccional o modifica su comportamiento.|
|[fsutil sparse](fsutil-sparse.md)|Administra los archivos dispersos. Un archivo disperso es un archivo con una o varias regiones de datos sin asignar en ella. Un programa verá estas regiones sin asignar como que contiene los bytes con valor cero, pero no hay espacio en disco se utiliza para representar estos ceros. Se asigna todos los datos significativos o distinto de cero, mientras que todos los datos que no son significativos (cadenas grandes de datos compuesto por ceros) no está asignada. Cuando se lee un archivo disperso, datos asignados se devuelven como almacenados y los datos sin asignar se devuelven como ceros (de forma predeterminada según la especificación de requisito de seguridad C2). Compatibilidad con archivos dispersos permite que los datos cancelar la asignación desde cualquier lugar en el archivo.|
|[los niveles de fsutil](fsutil-tiering.md)|Permite la administración de las funciones de nivel de almacenamiento, como la configuración y deshabilitar las marcas y listado de los niveles.|
|[transacción de fsutil](fsutil-transaction.md)|Confirma una transacción especificada, se revierte una transacción especificada o muestra información acerca de la transacción.|
|[fsutil usn](fsutil-usn.md)|Administra el diario de cambios de actualización de secuencia número (USN), que proporciona un registro persistente de todos los cambios realizados en los archivos en el volumen.|
|[volumen de fsutil](fsutil-volume.md)|Administra un volumen. Desmonta un volumen, las consultas para ver cuánto espacio libre está disponible en un disco, o busca un archivo que está usando un clúster especificado.|
|[Wim de fsutil](fsutil-wim.md)|Proporciona funciones para detectar y administrar archivos WIM de seguridad.|

## <a name="see-also"></a>Vea también
[Clave de sintaxis de línea de comandos](Command-Line-Syntax-Key.md)