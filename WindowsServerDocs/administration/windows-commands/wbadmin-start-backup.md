---
title: wbadmin start backup
description: Artículo de referencia para el comando Wbadmin Start Backup, que crea una copia de seguridad con los parámetros especificados.
ms.topic: reference
ms.assetid: 56f3e752-d99a-4c3d-8e97-10303c37dd78
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: a99d467aed8df57660f46b5efbcb4b8df5feb215
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524850"
---
# <a name="wbadmin-start-backup"></a>wbadmin start backup

Crea una copia de seguridad con los parámetros especificados. Si no se especifica ningún parámetro y se ha creado una copia de seguridad diaria programada, este comando crea la copia de seguridad mediante la configuración de la copia de seguridad programada. Si se especifican parámetros, se crea una copia de seguridad de copia de Servicio de instantáneas de volumen (VSS) y no se actualiza el historial de los archivos de los que se realiza una copia de seguridad.

Para crear una copia de seguridad única mediante este comando, debe ser miembro del grupo operadores de **copia de seguridad** o del grupo **administradores** , o bien tener delegados los permisos adecuados. Además, debe ejecutar **Wbadmin** desde un símbolo del sistema con privilegios elevados, haciendo clic con el botón secundario en **símbolo del sistema**y, a continuación, seleccionando **Ejecutar como administrador**.

## <a name="syntax"></a>Sintaxis

```com
wbadmin start backup [-backupTarget:{<BackupTargetLocation> | <TargetNetworkShare>}] [-include:<ItemsToInclude>] [-nonRecurseInclude:<ItemsToInclude>] [-exclude:<ItemsToExclude>] [-nonRecurseExclude:<ItemsToExclude>] [-allCritical] [-systemState] [-noVerify] [-user:<UserName>] [-password:<Password>] [-noInheritAcl] [-vssFull | -vssCopy] [-quiet]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| -backupTarget | Especifica la ubicación de almacenamiento de esta copia de seguridad. Requiere una letra de unidad de disco duro (f:), una ruta de acceso basada en GUID de volumen con el formato `\\?\Volume{GUID}` o una ruta de acceso UNC (Convención de nomenclatura universal) a una carpeta compartida remota `(\\<servername>\<sharename>\)` . De forma predeterminada, la copia de seguridad se guardará en: `\\<servername>\<sharename>\WindowsImageBackup\<ComputerBackedUp>\` . |
| -incluir | Especifica la lista de elementos delimitados por comas que se van a incluir en la copia de seguridad. Se pueden incluir varios archivos, carpetas o volúmenes. Las rutas de acceso a los volúmenes se pueden especificar mediante letras de unidad, puntos de montaje o nombres basados en identificadores GUID. Si usa un nombre de volumen basado en GUID, debe terminar con una barra diagonal inversa ( `\` ). Puede usar el carácter comodín ( `*` ) en el nombre de archivo al especificar una ruta de acceso a un archivo. El parámetro **-include** solo se debe usar junto con el parámetro **-backupTarget** . |
| -excluir | Especifica la lista de elementos delimitados por comas que se van a excluir de la copia de seguridad. Puede excluir archivos, carpetas o volúmenes. Las rutas de acceso a los volúmenes se pueden especificar mediante letras de unidad, puntos de montaje o nombres basados en identificadores GUID. Si usa un nombre de volumen basado en GUID, debe terminar con una barra diagonal inversa ( `\` ). Puede usar el carácter comodín ( `*` ) en el nombre de archivo al especificar una ruta de acceso a un archivo. El parámetro **-Exclude** solo se debe usar junto con el parámetro **-backupTarget** . |
| -nonRecurseInclude | Especifica la lista de elementos no recursiva y delimitada por comas que se van a incluir en la copia de seguridad. Se pueden incluir varios archivos, carpetas o volúmenes. Las rutas de acceso a los volúmenes se pueden especificar mediante letras de unidad, puntos de montaje o nombres basados en identificadores GUID. Si usa un nombre de volumen basado en GUID, debe terminar con una barra diagonal inversa ( `\` ). Puede usar el carácter comodín ( `*` ) en el nombre de archivo al especificar una ruta de acceso a un archivo. El parámetro **-nonRecurseInclude** solo debe usarse junto con el parámetro **-backupTarget** . |
| -nonRecurseExclude | Especifica la lista de elementos no recursivos delimitados por comas que se van a excluir de la copia de seguridad. Puede excluir archivos, carpetas o volúmenes. Las rutas de acceso a los volúmenes se pueden especificar mediante letras de unidad, puntos de montaje o nombres basados en identificadores GUID. Si usa un nombre de volumen basado en GUID, debe terminar con una barra diagonal inversa ( `\` ). Puede usar el carácter comodín ( `*` ) en el nombre de archivo al especificar una ruta de acceso a un archivo. El parámetro **-nonRecurseExclude** solo debe usarse junto con el parámetro **-backupTarget** . |
| -allCritical | Especifica que todos los volúmenes críticos (volúmenes que contienen el estado del sistema operativo) se incluyan en las copias de seguridad. Este parámetro es útil si va a crear una copia de seguridad para la reconstrucción completa. Solo debe usarse cuando se especifica **-backupTarget** , de lo contrario se produce un error en el comando. Se puede usar con la opción **-include** .<p>**Sugerencia:** El volumen de destino de una copia de seguridad de un volumen crítico puede ser una unidad local, pero no puede ser ninguno de los volúmenes incluidos en la copia de seguridad. |
| -systemState | Crea una copia de seguridad que incluye el estado del sistema además de cualquier otro elemento que se haya especificado con el parámetro **-include** . El estado del sistema contiene los archivos de arranque (Boot.ini, NDTLDR, NTDetect.com), el registro de Windows, incluida la configuración de COM, SYSVOL (directivas de grupo y scripts de inicio de sesión), el Active Directory y NTDS. DIT en controladores de dominio y, si el servicio de certificados está instalado, el almacén de certificados. Si el servidor tiene instalado el rol de servidor Web, se incluirá el metadirectorio de IIS. Si el servidor forma parte de un clúster, también se incluirá la información del servicio de Cluster Server. |
| -noverify | Especifica que las copias de seguridad guardadas en medios extraíbles (por ejemplo, un DVD) no se comprueban en busca de errores. Si no usa este parámetro, se comprueba si hay errores en las copias de seguridad guardadas en un medio extraíble. |
| -usuario | Si la copia de seguridad se guarda en una carpeta compartida remota, especifica el nombre de usuario con permiso de escritura en la carpeta. |
| -password | Especifica la contraseña para el nombre de usuario proporcionado por el parámetro **-User**. |
| -noInheritAcl | Aplica los permisos de la lista de control de acceso (ACL) que corresponden a las credenciales proporcionadas por los parámetros **-User** y **-password** a `\\<servername>\<sharename>\WindowsImageBackup\<ComputerBackedUp>\` (la carpeta que contiene la copia de seguridad). Para tener acceso a la copia de seguridad más adelante, debe usar estas credenciales o ser miembro del grupo administradores o del grupo operadores de copia de seguridad en el equipo con la carpeta compartida. Si no se usa **-noInheritAcl** , los permisos de la ACL de la carpeta compartida remota se aplican a la `\<ComputerBackedUp>` carpeta de forma predeterminada para que cualquier persona con acceso a la carpeta compartida remota pueda tener acceso a la copia de seguridad. |
| -vssFull | Realiza una copia de seguridad completa mediante el Servicio de instantáneas de volumen (VSS). Se realiza una copia de seguridad de todos los archivos, el historial de cada archivo se actualiza para reflejar que se ha realizado una copia de seguridad y se pueden truncar los registros de las copias de seguridad anteriores. Si no se usa este parámetro, **Wbadmin Start Backup** realiza una copia de seguridad de copia, pero no se actualiza el historial de archivos de los que se realiza la copia de seguridad.<p>**PRECAUCIÓN:** No use este parámetro si usa un producto que no sea Copias de seguridad de Windows Server para realizar copias de seguridad de las aplicaciones que se encuentran en los volúmenes incluidos en la copia de seguridad actual. Si lo hace, es posible que se interrumpa el tipo incremental, diferencial u otro tipo de copia de seguridad que está creando el otro producto de copia de seguridad, ya que el historial que se utiliza para determinar la cantidad de datos de copia de seguridad podría faltar y podrían llevar a cabo una copia de seguridad completa innecesariamente. |
| -vssCopy | Realiza una copia de seguridad de copia mediante VSS. Se realiza una copia de seguridad de todos los archivos, pero el historial de los archivos de los que se hace la copia de seguridad no se actualiza, de modo que se conserva toda la información sobre los archivos que se han modificado, eliminado, etc., así como los archivos de registro de la aplicación. El uso de este tipo de copia de seguridad no afecta a la secuencia de copias de seguridad incrementales y diferenciales que podrían producirse independientemente de esta copia de seguridad de copia. Este es el valor predeterminado.<p>**ADVERTENCIA:** No se puede usar una copia de seguridad de copia para copias de seguridad o restauraciones incrementales o diferenciales. |
| -quiet | Ejecuta el comando sin preguntar al usuario. |

#### <a name="remarks"></a>Comentarios

- Si guarda la copia de seguridad en una carpeta compartida remota y, a continuación, realiza otra copia de seguridad en el mismo equipo y en la misma carpeta compartida remota, sobrescribirá la copia de seguridad anterior.

- Si se produce un error en la operación de copia de seguridad, puede acabar sin una copia de seguridad porque se sobrescribe la copia de seguridad anterior, pero no se puede usar la copia de seguridad más reciente. Para evitar esto, se recomienda crear subcarpetas en la carpeta compartida remota para organizar las copias de seguridad. Sin embargo, debido a esta organización, debe tener el doble de espacio disponible que la carpeta principal.

## <a name="examples"></a>Ejemplos

Para crear una copia de seguridad de volúmenes *e:*, *d: \\ mountpoint*y `\\?\Volume{cc566d14-4410-11d9-9d93-806e6f6e6963}\` en el volumen *f:*, escriba:

```
wbadmin start backup -backupTarget:f: -include:e:,d:\mountpoint,\\?\Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\
```

Para realizar una copia de seguridad única de *f: \\ carpeta1* y *h: \\ Carpeta2* en el volumen *d:*, para hacer una copia de seguridad del estado del sistema y realizar una copia de seguridad de copia para que la copia de seguridad diferencial programada normalmente no se vea afectada, escriba:

```
wbadmin start backup –backupTarget:d: -include:g\folder1,h:\folder2 –systemstate -vsscopy
```

Para realizar una copia de seguridad única no recursiva de *d: \\ carpeta1* en la `\\backupshare\backup1*` Ubicación de red y para restringir el acceso a los miembros del grupo **administradores** o **operadores de copia de seguridad** , escriba:

```
wbadmin start backup –backupTarget: \\backupshare\backup1 -noinheritacl -nonrecurseinclude:d:\folder1
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando Wbadmin](wbadmin.md)
