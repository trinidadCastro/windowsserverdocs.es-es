---
title: Wbadmin habilitar copia de seguridad
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c0e57f8a-70fa-4c60-9754-e762e8ad8772
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6fd0bea5da83ca9351d5ea1028c94392bdb40422
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845546"
---
# <a name="wbadmin-enable-backup"></a>Wbadmin habilitar copia de seguridad



Crea y habilita una programación de copia de seguridad diaria o modifica una programación de copia de seguridad existente. No se especifica sin parámetros, muestra la configuración de copia de seguridad actualmente programada.

Para configurar o modificar una programación de copia de seguridad diaria, debe ser miembro de la **administradores** o **operadores de copia de seguridad** grupo. Además, debe ejecutar **wbadmin** desde un símbolo del sistema con privilegios elevados. (Para abrir un símbolo del sistema con privilegios elevados de contextual **símbolo** y, a continuación, haga clic en **ejecutar como administrador**.)

Para obtener ejemplos de cómo usar este subcomando, consulte [ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

Sintaxis para Windows Server 2008:
```
wbadmin enable backup
[-addtarget:<BackupTargetDisk>]
[-removetarget:<BackupTargetDisk>]
[-schedule:<TimeToRunBackup>]
[-include:<VolumesToInclude>]
[-allCritical]
[-quiet]
```
Sintaxis para Windows Server 2008 R2:
```
wbadmin enable backup
[-addtarget:<BackupTarget>]
[-removetarget:<BackupTarget>]
[-schedule:<TimeToRunBackup>]
[-include:<VolumesToInclude>]
[-nonRecurseInclude:<ItemsToInclude>]
[-exclude:<ItemsToExclude>]
[-nonRecurseExclude:<ItemsToExclude>][-systemState]
[-allCritical]
[-vssFull | -vssCopy]
[-user:<UserName>]
[-password:<Password>]
[-quiet]
```
Sintaxis para Windows Server 2012 y Windows Server 2012 R2:
```
wbadmin enable backup
[-addtarget:<BackupTarget>]
[-removetarget:<BackupTarget>]
[-schedule:<TimeToRunBackup>]
[-include:<VolumesToInclude>]
[-nonRecurseInclude:<ItemsToInclude>]
[-exclude:<ItemsToExclude>]
[-nonRecurseExclude:<ItemsToExclude>][-systemState]
[-hyperv:<HyperVComponentsToExclude>]
[-allCritical]
[-systemState] 
[-vssFull | -vssCopy]
[-user:<UserName>]
[-password:<Password>]
[-quiet] 
[-allowDeleteOldBackups]

```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|-addtarget|Para Windows Server 2008, especifica la ubicación de almacenamiento de copias de seguridad. Requiere que especifique un destino para las copias de seguridad como un identificador de disco (vea la sección Comentarios). Formatear el disco antes de su uso y los datos existentes en él se borrarán permanentemente.</br>Para Windows Server 2008 R2 y versiones posteriores, especifica la ubicación de almacenamiento de copias de seguridad. Requiere que especifique la ubicación como un disco, el volumen o la ruta de acceso de convención de nomenclatura Universal (UNC) a una carpeta compartida remota (\\\\\<servername >\<sharename >\). De forma predeterminada, la copia de seguridad se guardará en: \\ \\ <servername> \<sharename > \WindowsImageBackup\<equipoCopiado >\. Si especifica un disco, se formateará el disco antes de su uso y los datos existentes en él se borrarán permanentemente. Si especifica una carpeta compartida, no se puede agregar más ubicaciones. Solo puede especificar una carpeta compartida como una ubicación de almacenamiento a la vez.</br>Importante: Si guarda una copia de seguridad en una carpeta compartida remota, esa copia de seguridad se sobrescribirá si usa la misma carpeta para rastrear agrupando el mismo equipo. Además, si se produce un error en la operación de copia de seguridad, puede acabar con ninguna copia de seguridad porque se sobrescribirá la copia de seguridad anterior, pero no se podrá usar la copia de seguridad más reciente. Para evitarlo, puede crear subcarpetas en la carpeta compartida remota para organizar las copias de seguridad. Si lo hace, las subcarpetas necesitarán el doble del espacio de la carpeta principal.</br>Una única ubicación puede especificarse en un solo comando. Si vuelve a ejecutar el comando, se pueden agregar varias ubicaciones de almacenamiento de copia de seguridad del volumen y disco.|
|-removetarget|Especifica la ubicación de almacenamiento que desea quitar de la programación de copia de seguridad existente. Requiere que especifique la ubicación como un identificador de disco (vea la sección Comentarios).|
|-programación|Especifica las horas del día para crear una copia de seguridad, con el formato hh: mm y delimitado por comas.|
|-incluir|Para Windows Server 2008, especifica la lista delimitada por comas de las letras de unidad, puntos de montaje o nombres basados en GUID de volumen para incluir en la copia de seguridad.</br>Para Windows Server 2008 R2and más adelante, especifica la lista delimitada por comas de elementos que se va a incluir en la copia de seguridad. Se pueden incluir varios archivos, carpetas o volúmenes. Las rutas de acceso a los volúmenes se pueden especificar mediante letras de unidad, puntos de montaje o nombres basados en identificadores GUID. Si usa un nombre de volumen basada en GUID, debe finalizar con una barra diagonal inversa (\). Puede usar el carácter comodín (*) en el nombre de archivo al especificar una ruta de acceso a un archivo.|
|-nonRecurseInclude|Para Windows Server 2008 R2 y versiones posteriores, especifica el no recursivo, una lista delimitada por comas de elementos que se va a incluir en la copia de seguridad. Se pueden incluir varios archivos, carpetas o volúmenes. Las rutas de acceso a los volúmenes se pueden especificar mediante letras de unidad, puntos de montaje o nombres basados en identificadores GUID. Si usa un nombre de volumen basada en GUID, debe finalizar con una barra diagonal inversa (\). Puede usar el carácter comodín (*) en el nombre de archivo al especificar una ruta de acceso a un archivo. Debe usarse solo cuando se usa el parámetro - backupTarget.|
|-excluir|Para Windows Server 2008 R2 y versiones posteriores, especifica la lista delimitada por comas de elementos que se van a excluir de la copia de seguridad. Puede excluir archivos, carpetas o volúmenes. Las rutas de acceso a los volúmenes se pueden especificar mediante letras de unidad, puntos de montaje o nombres basados en identificadores GUID. Si usa un nombre de volumen basada en GUID, debe finalizar con una barra diagonal inversa (\). Puede usar el carácter comodín (*) en el nombre de archivo al especificar una ruta de acceso a un archivo.|
|-nonRecurseExclude|Para Windows Server 2008 R2 y versiones posteriores, especifica el no recursivo, una lista delimitada por comas de elementos que se van a excluir de la copia de seguridad. Puede excluir archivos, carpetas o volúmenes. Las rutas de acceso a los volúmenes se pueden especificar mediante letras de unidad, puntos de montaje o nombres basados en identificadores GUID. Si usa un nombre de volumen basada en GUID, debe finalizar con una barra diagonal inversa (\). Puede usar el carácter comodín (*) en el nombre de archivo al especificar una ruta de acceso a un archivo.|
|-hyperv|Especifica la lista delimitada por comas de los componentes que se incluirán en la copia de seguridad. El identificador podría ser un nombre de componente o un componente GUID (con o sin llaves).|
|-systemState|Para Windows 7 y Windows Server 2008 R2 y versiones posteriores, crea una copia de seguridad que incluye el estado del sistema, además de otros elementos que ha especificado con el **-incluyen** parámetro. El estado del sistema contiene los archivos de arranque (Boot.ini, NDTLDR, NTDetect.com), el registro de Windows, incluida la configuración de COM, SYSVOL (directivas de grupo y Scripts de inicio de sesión), Active Directory y NTDS. DIT en controladores de dominio y, si está instalado el servicio de certificados, el certificado de Store. Si el servidor tiene instalada la función de servidor Web, IIS metadirectorio se incluirán. Si el servidor forma parte de un clúster, también se incluye información de servicio de clúster.|
|-allCritical|Especifica que todos los volúmenes críticos (volúmenes que contienen el estado del sistema operativo) se incluirán en las copias de seguridad. Este parámetro es útil si va a crear una copia de seguridad completa sistema o la recuperación del estado del sistema. Debe usarse solo cuando se especifica - backupTarget, en caso contrario, el comando generará un error. Se puede usar con el **-incluyen** opción.</br>Sugerencia: El volumen de destino para una copia de seguridad de volumen críticos puede ser una unidad local, pero no puede ser cualquiera de los volúmenes que se incluyen en la copia de seguridad.|
|-vssFull|Para Windows Server 2008 R2 y versiones posteriores, realiza una completa de copia de seguridad mediante el servicio de instantáneas de volumen (VSS). Todos los archivos se copian, historial de cada archivo se actualiza para reflejar que se realizó la copia y se pueden truncar los registros de copias de seguridad anteriores. Si no se usa este parámetro wbadmin start backup realiza una copia de copia de seguridad, pero no se actualiza el historial de archivos de copia de seguridad.</br>Precaución: No utilice este parámetro si usas un producto que no sea la copia de seguridad de Windows Server para realizar una copia de seguridad de aplicaciones que se encuentran en los volúmenes incluidos en la copia de seguridad actual. Si lo hace por lo que potencialmente puede interrumpir la incremental, diferencial u otro tipo de copias de seguridad que está creando el otro producto de copia de seguridad porque el historial que confía en para determinar la cantidad de datos para copia de seguridad podría faltar y pueden realizar una completa de copia de seguridad innecesariamente.|
|-vssCopy|Para Windows Server 2008 R2 y versiones posteriores, se realiza una copia de seguridad mediante VSS. Todos los archivos se copian pero no se actualiza el historial de los archivos que se va a copia de seguridad para conservar toda la información en los archivos a los que cambió, eliminado etc., así como los archivos de registro de aplicación. Usar este tipo de copia de seguridad no afecta a la secuencia de copias de seguridad incrementales diferenciales que puede ocurrir independientemente de esta copia de seguridad. Este es el valor predeterminado.</br>Advertencia: No se puede usar una copia de seguridad para las copias de seguridad incrementales o diferenciales o de las restauraciones.|
|-usuario|Para Windows Server 2008 R2 y versiones posteriores, especifica el usuario con permiso de escritura en el destino de almacenamiento de copia de seguridad (si es una carpeta compartida remota). El usuario debe ser miembro del grupo Administradores o del grupo Operadores de copia de seguridad en el equipo que está obteniendo una copia de seguridad.|
|-contraseña|Para Windows Server 2008 R2 y versiones posteriores, especifica la contraseña para el nombre de usuario proporcionado por el parámetro-usuario.|
|-quiet|Se ejecuta el subcomando sin solicitudes para el usuario.|
|-allowDeleteOldBackups|Sobrescribe cualquier copia de seguridad realizada antes de que el equipo se actualizó.|

## <a name="remarks"></a>Comentarios

Para ver el valor de identificador de disco para los discos, escriba **wbadmin obtener discos**.

## <a name="BKMK_examples"></a>Ejemplos

Los ejemplos siguientes muestran cómo el **wbadmin habilitar copia de seguridad** comando puede utilizarse en diferentes escenarios de copia de seguridad:

Escenario 1 #
-   Programar copias de seguridad de las unidades de disco duro e:, d:\mountpoint, y \\ \\? \Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\
-   Guardar los archivos en el disco DiskID
-   Ejecutar copias de seguridad diariamente a las 9:00 A.M. y las 6:00 P.M.
```
wbadmin enable backup -addtarget:DiskID -schedule:09:00,18:00 -include:e:,d:\mountpoint,\\?\Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\
```
Escenario 2 #
-   Programar copias de seguridad de la d:\documents carpeta a la ubicación de red \\ \\backupshare\backup1
-   Usar las credenciales de red para el Administrador de copia de seguridad Aaren Ekelund (aekel), que es un miembro del dominio CONTOSOEAST para autenticar el acceso al recurso compartido de red. Es la contraseña del Aaren *$3 hM 9 ^ 5lp*.
-   Ejecutar copias de seguridad diariamente a las 12:00 A.M. y las 7:00 P.M.
```
wbadmin enable backup –addtarget:\\backupshare\backup1 –include: d:\documents –user:CONTOSOEAST\aekel –password:$3hM9^5lp –schedule:00:00,19:00
```
Escenario 3 #
-   Programar copias de seguridad del volumen d:\documents t: y la carpeta a la altura de la unidad, pero excluye la d:\documents carpeta\~tmp
-   Realizar una copia de seguridad completa mediante el servicio de instantáneas de volumen.
-   Ejecutar copias de seguridad diariamente a las 1:00 A.M.
```
wbadmin enable backup –addtarget:H: –include T:,D:\documents –exclude D:\documents\~tmp –vssfull –schedule:01:00
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)