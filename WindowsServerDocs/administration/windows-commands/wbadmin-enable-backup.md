---
title: wbadmin enable backup
description: Artículo de referencia de Wbadmin habilitar copia de seguridad, que crea y habilita una programación de copia de seguridad diaria o modifica una programación de copia de seguridad existente.
ms.topic: reference
ms.assetid: c0e57f8a-70fa-4c60-9754-e762e8ad8772
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6be32f6134bacf698d6e28998cbed76e8b50155f
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89032073"
---
# <a name="wbadmin-enable-backup"></a>wbadmin enable backup



Crea y habilita una programación de copia de seguridad diaria o modifica una programación de copia de seguridad existente. Si no se especifica ningún parámetro, se muestra la configuración de copia de seguridad programada actualmente.

Para configurar o modificar una programación de copia de seguridad diaria, debe ser miembro del grupo **administradores** o **operadores de copia de seguridad** . Además, debe ejecutar **Wbadmin** desde un símbolo del sistema con privilegios elevados. (Para abrir un símbolo del sistema con privilegios elevados, haga clic con el botón secundario en **símbolo del sistema** y haga clic en **Ejecutar como administrador**).

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

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|-addtarget|En Windows Server 2008, especifica la ubicación de almacenamiento para las copias de seguridad. Requiere que se especifique un destino para las copias de seguridad como identificador de disco (vea la sección comentarios). El disco tiene el formato anterior al uso, y los datos existentes en él se borran de forma permanente.</br>En Windows Server 2008 R2 y versiones posteriores, especifica la ubicación de almacenamiento para las copias de seguridad. Requiere que se especifique la ubicación como una ruta de acceso de disco, volumen o Convención de nomenclatura universal (UNC) a una carpeta compartida remota ( \\ \\ \<servername> \<sharename> \) . De forma predeterminada, la copia de seguridad se guardará en: \\ \\ <servername> \<sharename> \WindowsImageBackup\<ComputerBackedUp>\. Si especifica un disco, se formateará el disco antes de usarse y los datos existentes en él se borrarán de forma permanente. Si especifica una carpeta compartida, no puede agregar más ubicaciones. Solo puede especificar una carpeta compartida como ubicación de almacenamiento a la vez.</br>Importante: Si guarda una copia de seguridad en una carpeta compartida remota, la copia de seguridad se sobrescribirá si usa la misma carpeta para hacer una copia de seguridad del mismo equipo de nuevo. Además, si se produce un error en la operación de copia de seguridad, es posible que se quede sin ninguna copia de seguridad porque la copia anterior se sobrescribirá y la nueva no se podrá usar. Para evitar esta situación, puede crear subcarpetas en la carpeta compartida remota y organizar en ellas las copias de seguridad. Si lo hace, las subcarpetas necesitarán dos veces el espacio de la carpeta principal.</br>Solo se puede especificar una ubicación en un solo comando. Se pueden agregar varias ubicaciones de almacenamiento de copia de seguridad de disco y volumen si se vuelve a ejecutar el comando.|
|-removetarget|Especifica la ubicación de almacenamiento que desea quitar de la programación de copia de seguridad existente. Requiere que se especifique la ubicación como identificador de disco (vea la sección comentarios).|
|-programación|Especifica las horas del día para crear una copia de seguridad, con el formato HH: MM y coma delimitada.|
|-incluir|En Windows Server 2008, especifica la lista delimitada por comas de las letras de unidad del volumen, los puntos de montaje del volumen o los nombres de los volúmenes basados en GUID que se van a incluir en la copia de seguridad.</br>En Windows Server 2008 R2and, especifica la lista de elementos delimitados por comas que se van a incluir en la copia de seguridad. Se pueden incluir varios archivos, carpetas o volúmenes. Las rutas de acceso a los volúmenes se pueden especificar mediante letras de unidad, puntos de montaje o nombres basados en identificadores GUID. Si usa un nombre de volumen basado en GUID, debe terminar con una barra diagonal inversa ( \) . Puede usar el carácter comodín (*) en el nombre de archivo al especificar una ruta de acceso a un archivo.|
|-nonRecurseInclude|En Windows Server 2008 R2 y versiones posteriores, especifica la lista de elementos no recursiva y delimitada por comas que se van a incluir en la copia de seguridad. Se pueden incluir varios archivos, carpetas o volúmenes. Las rutas de acceso a los volúmenes se pueden especificar mediante letras de unidad, puntos de montaje o nombres basados en identificadores GUID. Si usa un nombre de volumen basado en GUID, debe terminar con una barra diagonal inversa ( \) . Puede usar el carácter comodín (*) en el nombre de archivo al especificar una ruta de acceso a un archivo. Solo se debe usar cuando se usa el parámetro-backupTarget.|
|-excluir|En Windows Server 2008 R2 y versiones posteriores, especifica la lista de elementos delimitados por comas que se van a excluir de la copia de seguridad. Puede excluir archivos, carpetas o volúmenes. Las rutas de acceso a los volúmenes se pueden especificar mediante letras de unidad, puntos de montaje o nombres basados en identificadores GUID. Si usa un nombre de volumen basado en GUID, debe terminar con una barra diagonal inversa ( \) . Puede usar el carácter comodín (*) en el nombre de archivo al especificar una ruta de acceso a un archivo.|
|-nonRecurseExclude|En Windows Server 2008 R2 y versiones posteriores, especifica la lista de elementos no recursiva y delimitada por comas que se van a excluir de la copia de seguridad. Puede excluir archivos, carpetas o volúmenes. Las rutas de acceso a los volúmenes se pueden especificar mediante letras de unidad, puntos de montaje o nombres basados en identificadores GUID. Si usa un nombre de volumen basado en GUID, debe terminar con una barra diagonal inversa ( \) . Puede usar el carácter comodín (*) en el nombre de archivo al especificar una ruta de acceso a un archivo.|
|-HyperV|Especifica la lista delimitada por comas de los componentes que se van a incluir en la copia de seguridad. El identificador puede ser un nombre de componente o un GUID de componente (con o sin llaves).|
|-systemState|En Windows ° 7 y Windows Server 2008 R2 y versiones posteriores, crea una copia de seguridad que incluye el estado del sistema además de cualquier otro elemento que se haya especificado con el parámetro **-include** . El estado del sistema contiene los archivos de arranque (Boot.ini, NDTLDR, NTDetect.com), el registro de Windows, incluida la configuración de COM, SYSVOL (directivas de grupo y scripts de inicio de sesión), el Active Directory y NTDS. DIT en controladores de dominio y, si el servicio de certificados está instalado, el almacén de certificados. Si el servidor tiene instalado el rol de servidor Web, se incluirá el metadirectorio de IIS. Si el servidor forma parte de un clúster, también se incluye la información de Servicio de clúster.|
|-allCritical|Especifica que todos los volúmenes críticos (volúmenes que contienen el estado del sistema operativo) se incluyan en las copias de seguridad. Este parámetro es útil si va a crear una copia de seguridad para la recuperación completa del sistema o del estado del sistema. Solo debe usarse cuando se especifica-backupTarget, de lo contrario se producirá un error en el comando. Se puede usar con la opción **-include** .</br>Sugerencia: el volumen de destino de una copia de seguridad de un volumen crítico puede ser una unidad local, pero no puede ser ninguno de los volúmenes incluidos en la copia de seguridad.|
|-vssFull|En Windows Server 2008 R2 y versiones posteriores, realiza una copia de seguridad completa mediante el Servicio de instantáneas de volumen (VSS). Se realiza una copia de seguridad de todos los archivos, el historial de cada archivo se actualiza para reflejar que se ha realizado una copia de seguridad y se pueden truncar los registros de las copias de seguridad anteriores. Si no se usa este parámetro, Wbadmin Start Backup realiza una copia de seguridad de copia, pero no se actualiza el historial de archivos de los que se realiza la copia de seguridad.</br>PRECAUCIÓN: no use este parámetro si usa un producto que no sea Copias de seguridad de Windows Server para realizar copias de seguridad de las aplicaciones que se encuentran en los volúmenes incluidos en la copia de seguridad actual. Si lo hace, es posible que se interrumpa el tipo incremental, diferencial u otro tipo de copia de seguridad que está creando el otro producto de copia de seguridad, ya que el historial que se utiliza para determinar la cantidad de datos de copia de seguridad podría faltar y podrían llevar a cabo una copia de seguridad completa innecesariamente.|
|-vssCopy|En Windows Server 2008 R2 y versiones posteriores, realiza una copia de seguridad de copia mediante VSS. Se realiza una copia de seguridad de todos los archivos, pero el historial de los archivos de los que se hace la copia de seguridad no se actualiza, de modo que se conserva toda la información sobre los archivos que se han modificado, eliminado, etc., así como los archivos de registro de la aplicación. El uso de este tipo de copia de seguridad no afecta a la secuencia de copias de seguridad incrementales y diferenciales que podrían producirse independientemente de esta copia de seguridad de copia. Este es el valor predeterminado.</br>ADVERTENCIA: no se puede usar una copia de seguridad de copia para copias de seguridad o restauraciones incrementales o diferenciales.|
|-usuario|En Windows Server 2008 R2 y versiones posteriores, especifica el usuario con permiso de escritura para el destino de almacenamiento de copia de seguridad (si se trata de una carpeta compartida remota). El usuario debe ser miembro del grupo administradores o del grupo operadores de copia de seguridad en el equipo del que se está realizando la copia de seguridad.|
|-password|En Windows Server 2008 R2 y versiones posteriores, especifica la contraseña para el nombre de usuario proporcionado por el parámetro-user.|
|-quiet|Ejecuta el subcomando sin preguntar al usuario.|
|-allowDeleteOldBackups|Sobrescribe las copias de seguridad realizadas antes de actualizar el equipo.|

## <a name="remarks"></a>Observaciones

Para ver el valor del identificador de disco de los discos, escriba **Wbadmin get Disks**.

## <a name="examples"></a>Ejemplos

En los siguientes ejemplos se muestra cómo se puede usar el comando **Wbadmin enable backup** en distintos escenarios de copia de seguridad:

Escenario #1
- Programar copias de seguridad de las unidades de disco duro e:, d:\mountpoint y \\ \\ ? \Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\
- Guarde los archivos en el disco duro.
- Ejecutar copias de seguridad diariamente a las 9:00 A.M. y a las 06:00:00 p. m.
  ```
  wbadmin enable backup -addtarget:DiskID -schedule:09:00,18:00 -include:e:,d:\mountpoint,\\?\Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\
  ```
  Escenario #2
- Programe copias de seguridad de la carpeta d:\Documents en la ubicación de red \\ \\ backupshare\backup1
- Use las credenciales de red para el administrador de copias de seguridad Aaren Ekelund (aekel), que es miembro del dominio CONTOSOEAST para autenticar el acceso al recurso compartido de red. La contraseña de Aaren es *$3hM9 ^ 5lp*.
- Ejecutar copias de seguridad diariamente a las 12:00 A.M. y 7:00 P.M.
  ```
  wbadmin enable backup –addtarget:\\backupshare\backup1 –include: d:\documents –user:CONTOSOEAST\aekel –password:$3hM9^5lp –schedule:00:00,19:00
  ```
  Escenario #3
- Programe copias de seguridad del volumen t: y la carpeta d:\Documents en la unidad h:, pero excluya la carpeta d:\Documents \~ tmp
- Realice una copia de seguridad completa mediante el Servicio de instantáneas de volumen.
- Ejecutar copias de seguridad diariamente a las 1:00 A.M.
  ```
  wbadmin enable backup –addtarget:H: –include T:,D:\documents –exclude D:\documents\~tmp –vssfull –schedule:01:00
  ```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)