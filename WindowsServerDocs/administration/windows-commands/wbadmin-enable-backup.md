---
title: wbadmin enable backup
description: Artículo de referencia del comando Wbadmin enable backup, que crea y habilita una programación de copia de seguridad diaria o modifica una programación de copia de seguridad existente.
ms.topic: reference
ms.assetid: c0e57f8a-70fa-4c60-9754-e762e8ad8772
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 3135df39135a1d085509bdbdabdfa7f88817c3be
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156069"
---
# <a name="wbadmin-enable-backup"></a>wbadmin enable backup

Crea y habilita una programación de copia de seguridad diaria o modifica una programación de copia de seguridad existente. Si no se especifica ningún parámetro, se muestra la configuración de copia de seguridad programada actualmente.

Para configurar o modificar una programación de copia de seguridad diaria con este comando, debe ser miembro del grupo **operadores de copia de seguridad** o del grupo **administradores** . Además, debe ejecutar **Wbadmin** desde un símbolo del sistema con privilegios elevados, haciendo clic con el botón secundario en **símbolo del sistema**y, a continuación, seleccionando **Ejecutar como administrador**.

Para ver el valor del identificador de disco de los discos, ejecute el comando [Wbadmin get Disks](wbadmin-get-disks.md) .

## <a name="syntax"></a>Sintaxis

```
wbadmin enable backup [-addtarget:<BackupTarget>] [-removetarget:<BackupTarget>] [-schedule:<TimeToRunBackup>] [-include:<VolumesToInclude>] [-nonRecurseInclude:<ItemsToInclude>] [-exclude:<ItemsToExclude>] [-nonRecurseExclude:<ItemsToExclude>][-systemState] [-hyperv:<HyperVComponentsToExclude>] [-allCritical] [-systemState] [-vssFull | -vssCopy] [-user:<UserName>] [-password:<Password>] [-allowDeleteOldBackups]  [-quiet]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| -addtarget | Especifica la ubicación de almacenamiento para las copias de seguridad. Requiere que se especifique la ubicación como una ruta de acceso de disco, volumen o Convención de nomenclatura universal (UNC) a una carpeta compartida remota ( `\\<servername>\<sharename>` ). De forma predeterminada, la copia de seguridad se guardará en: `\\<servername>\<sharename> WindowsImageBackup <ComputerBackedUp>` . Si especifica un disco, se formateará el disco antes de usarse y los datos existentes en él se borrarán de forma permanente. Si especifica una carpeta compartida, no puede agregar más ubicaciones. Solo puede especificar una carpeta compartida como ubicación de almacenamiento a la vez.<p>**Importante:** Si guarda una copia de seguridad en una carpeta compartida remota, la copia de seguridad se sobrescribirá si usa la misma carpeta para hacer una copia de seguridad del mismo equipo de nuevo. Además, si se produce un error en la operación de copia de seguridad, es posible que no se realice ninguna copia de seguridad porque se sobrescribirá la copia de seguridad anterior, pero no se podrá usar la copia de seguridad más reciente. Para evitar esto, puede crear subcarpetas en la carpeta compartida remota para organizar las copias de seguridad. Si lo hace, las subcarpetas necesitan el doble de espacio que la carpeta principal.<p>Solo se puede especificar una ubicación en un solo comando. Se pueden agregar varias ubicaciones de almacenamiento de copia de seguridad de disco y volumen si se vuelve a ejecutar el comando. |
| -removetarget | Especifica la ubicación de almacenamiento que desea quitar de la programación de copia de seguridad existente. Requiere que se especifique la ubicación como identificador de disco. |
| -programación | Especifica las horas del día para crear una copia de seguridad, con el formato HH: MM y coma delimitada. |
| -incluir | Especifica la lista de elementos delimitados por comas que se van a incluir en la copia de seguridad. Se pueden incluir varios archivos, carpetas o volúmenes. Las rutas de acceso a los volúmenes se pueden especificar mediante letras de unidad, puntos de montaje o nombres basados en identificadores GUID. Si usa un nombre de volumen basado en GUID, debe terminar con una barra diagonal inversa ( `\` ). Puede usar el carácter comodín ( `*` ) en el nombre de archivo al especificar una ruta de acceso a un archivo. |
| -nonRecurseInclude | Especifica la lista de elementos no recursiva y delimitada por comas que se van a incluir en la copia de seguridad. Se pueden incluir varios archivos, carpetas o volúmenes. Las rutas de acceso a los volúmenes se pueden especificar mediante letras de unidad, puntos de montaje o nombres basados en identificadores GUID. Si usa un nombre de volumen basado en GUID, debe terminar con una barra diagonal inversa ( `\` ). Puede usar el carácter comodín ( `*` ) en el nombre de archivo al especificar una ruta de acceso a un archivo. Solo se debe usar cuando se usa el parámetro **-backupTarget** . |
| -excluir | Especifica la lista de elementos delimitados por comas que se van a excluir de la copia de seguridad. Puede excluir archivos, carpetas o volúmenes. Las rutas de acceso a los volúmenes se pueden especificar mediante letras de unidad, puntos de montaje o nombres basados en identificadores GUID. Si usa un nombre de volumen basado en GUID, debe terminar con una barra diagonal inversa ( `\` ). Puede usar el carácter comodín ( `*` ) en el nombre de archivo al especificar una ruta de acceso a un archivo. |
| -nonRecurseExclude | Especifica la lista de elementos no recursivos delimitados por comas que se van a excluir de la copia de seguridad. Puede excluir archivos, carpetas o volúmenes. Las rutas de acceso a los volúmenes se pueden especificar mediante letras de unidad, puntos de montaje o nombres basados en identificadores GUID. Si usa un nombre de volumen basado en GUID, debe terminar con una barra diagonal inversa ( `\` ). Puede usar el carácter comodín ( `*` ) en el nombre de archivo al especificar una ruta de acceso a un archivo. |
| -HyperV | Especifica la lista delimitada por comas de los componentes que se van a incluir en la copia de seguridad. El identificador puede ser un nombre de componente o un GUID de componente (con o sin llaves). |
| -systemState | Crea una copia de seguridad que incluye el estado del sistema además de cualquier otro elemento que se haya especificado con el parámetro **-include** . El estado del sistema contiene los archivos de arranque (Boot.ini, NDTLDR, NTDetect.com), el registro de Windows, incluida la configuración de COM, SYSVOL (directivas de grupo y scripts de inicio de sesión), el Active Directory y NTDS. DIT en controladores de dominio y, si el servicio de certificados está instalado, el almacén de certificados. Si el servidor tiene instalado el rol de servidor Web, se incluirá el metadirectorio de IIS. Si el servidor forma parte de un clúster, también se incluye la información de Servicio de clúster. |
| -allCritical | Especifica que todos los volúmenes críticos (volúmenes que contienen el estado del sistema operativo) se incluyan en las copias de seguridad. Este parámetro es útil si va a crear una copia de seguridad para la recuperación completa del sistema o del estado del sistema. Solo debe usarse cuando se especifica **-backupTarget** . de lo contrario, se produce un error en el comando. Se puede usar con la opción **-include** .<p>**Sugerencia:** El volumen de destino de una copia de seguridad de un volumen crítico puede ser una unidad local, pero no puede ser ninguno de los volúmenes incluidos en la copia de seguridad. |
| -vssFull | Realiza una copia de seguridad completa mediante el Servicio de instantáneas de volumen (VSS). Se realiza una copia de seguridad de todos los archivos, el historial de cada archivo se actualiza para reflejar que se ha realizado una copia de seguridad y se pueden truncar los registros de las copias de seguridad anteriores. Si no se usa este parámetro, el comando [Wbadmin Start Backup](wbadmin-start-backup.md) realiza una copia de seguridad de copia, pero no se actualiza el historial de archivos de los que se realiza la copia de seguridad.<p>**PRECAUCIÓN:** No use este parámetro si usa un producto que no sea Copias de seguridad de Windows Server para realizar copias de seguridad de las aplicaciones que se encuentran en los volúmenes incluidos en la copia de seguridad actual. Si lo hace, es posible que se interrumpa el tipo incremental, diferencial u otro tipo de copia de seguridad que está creando el otro producto de copia de seguridad, ya que el historial que se utiliza para determinar la cantidad de datos de copia de seguridad podría faltar y podrían llevar a cabo una copia de seguridad completa innecesariamente. |
| -vssCopy | Realiza una copia de seguridad de copia mediante VSS. Se realiza una copia de seguridad de todos los archivos, pero el historial de los archivos de los que se hace la copia de seguridad no se actualiza, de modo que se conserva toda la información sobre los archivos que se han modificado, eliminado, etc., así como los archivos de registro de la aplicación. El uso de este tipo de copia de seguridad no afecta a la secuencia de copias de seguridad incrementales y diferenciales que podrían producirse independientemente de esta copia de seguridad de copia. Este es el valor predeterminado.<p>**ADVERTENCIA:** No se puede usar una copia de seguridad para copias de seguridad incrementales o diferenciales o restauraciones. |
| -usuario | Especifica el usuario con permiso de escritura para el destino de almacenamiento de copia de seguridad (si se trata de una carpeta compartida remota). El usuario debe ser miembro del grupo **administradores** o **operadores de copia** de seguridad en el equipo en el que se realiza la copia de seguridad. |
| -password | Especifica la contraseña para el nombre de usuario proporcionado por el parámetro **-User**. |
| -allowDeleteOldBackups | Sobrescribe las copias de seguridad realizadas antes de actualizar el equipo. |
| -quiet | Ejecuta el comando sin preguntar al usuario. |

## <a name="examples"></a>Ejemplos

Para programar copias de seguridad diarias a las 9:00 AM y 6:00 PM para las unidades de disco duro E:, D:\mountpoint, y `\\?\Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\` , y para guardar los archivos en el disco denominado, escriba:

```
wbadmin enable backup -addtarget:DiskID -schedule:09:00,18:00 -include:E:,D:\mountpoint,\\?\Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\
```

Para programar copias de seguridad diarias de la carpeta D:\Documents en 12:00 AM y 7:00 PM en la ubicación de red `\\backupshare\backup1` , con las credenciales de red para el **operador de copia de seguridad**, Aaren Ekelund (aekel), cuya contraseña es *$3hM9 ^ 5lp* y que es miembro del dominio CONTOSOEAST, que se usa para autenticar el acceso al recurso compartido de red, escriba:

```
wbadmin enable backup –addtarget:\\backupshare\backup1 –include: D:\documents –user:CONTOSOEAST\aekel –password:$3hM9^5lp –schedule:00:00,19:00
```

Para programar copias de seguridad diarias del volumen T: y la carpeta D:\Documents en 1:00 AM para la unidad H:, sin incluir la carpeta `d:\documents\~tmp` , y realizar una copia de seguridad completa mediante el servicio de instantáneas de volumen, escriba:

```
wbadmin enable backup –addtarget:H: –include T:,D:\documents –exclude D:\documents\~tmp –vssfull –schedule:01:00
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando Wbadmin](wbadmin.md)

- [comando Wbadmin para habilitar copia de seguridad](wbadmin-enable-backup.md)

- [comando Wbadmin Start Backup](wbadmin-start-backup.md)

- [comando Wbadmin get Disks](wbadmin-get-disks.md)
