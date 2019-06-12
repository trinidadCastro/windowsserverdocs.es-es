---
title: Wbadmin start backup
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 56f3e752-d99a-4c3d-8e97-10303c37dd78
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2ac602506960b92333750e7a37692c44c92aae22
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440276"
---
# <a name="wbadmin-start-backup"></a>Wbadmin start backup



Crea una copia de seguridad mediante los parámetros especificados. Si se especifica ningún parámetro y que ha creado una copia de seguridad diaria programada, este subcomando crea la copia de seguridad mediante la configuración de la copia de seguridad programada. Si se especifican parámetros, crea una copia de seguridad de servicio de instantáneas de volumen (VSS) y no se actualizará el historial de los archivos que se copia de seguridad.

Para crear una copia de seguridad de un solo uso con este subcomando, debe ser miembro de la **operadores de copia de seguridad** grupo o la **administradores** grupo, o bien debe haber sido delegar los permisos adecuados. Además, debe ejecutar **wbadmin** desde un símbolo del sistema con privilegios elevados. (Para abrir un símbolo del sistema con privilegios elevados de contextual **símbolo** y, a continuación, haga clic en **ejecutar como administrador**.)

Para obtener ejemplos de cómo usar este subcomando, consulte [ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

Sintaxis para Windows ° Vista y Windows Server 2008:
```
wbadmin start backup
[-backupTarget:{<BackupTargetLocation> | <TargetNetworkShare>}]
[-include:<VolumesToInclude>]
[-allCritical]
[-noVerify]
[-user:<UserName>]
[-password:<Password>]
[-noinheritAcl]
[-vssFull]
[-quiet]
```
Sintaxis para Windows 7 y Windows Server 2008 R2 y versiones posteriores:
```
Wbadmin start backup
[-backupTarget:{<BackupTargetLocation> | <TargetNetworkShare>}]
[-include:<ItemsToInclude>]
[-nonRecurseInclude:<ItemsToInclude>]
[-exclude:<ItemsToExclude>]
[-nonRecurseExclude:<ItemsToExclude>]
[-allCritical]
[-systemState]
[-noVerify]
[-user:<UserName>]
[-password:<Password>]
[-noInheritAcl]
[-vssFull | -vssCopy] 
[-quiet]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|-backupTarget|Especifica la ubicación de almacenamiento para esta copia de seguridad. Requiere una letra de unidad de disco duro (f:), una ruta basada en GUID de volumen en el formato de \\ \\? \Volume{GUID}, o una ruta de convención de nomenclatura Universal (UNC) a una carpeta compartida remota (\\\\\<servername > \<sharename >\). De forma predeterminada, la copia de seguridad se guardará en: \\ \\ <servername> \<sharename >\** WindowsImageBackup **\\<ComputerBackedUp>\.</br>Importante: Si guarda una copia de seguridad en una carpeta compartida remota, esa copia de seguridad se sobrescribirá si usa la misma carpeta para rastrear agrupando el mismo equipo. Además, si se produce un error en la operación de copia de seguridad, puede acabar con ninguna copia de seguridad porque se sobrescribirá la copia de seguridad anterior, pero no se podrá usar la copia de seguridad más reciente. Para evitarlo, puede crear subcarpetas en la carpeta compartida remota para organizar las copias de seguridad. Si lo hace, las subcarpetas necesitarán el doble del espacio que la carpeta principal.|
|-incluir|Para Windows ° Vista y Windows Server 2008, especifica la lista delimitada por comas de las letras de unidad, puntos de montaje o nombres basados en GUID de volumen para incluir en la copia de seguridad. Este parámetro debe usarse solo cuando la **- backupTarget** se usa el parámetro.</br>Para Windows 7 y Windows Server 2008 R2 y versiones posteriores, especifica la lista delimitada por comas de elementos que desea incluir en la copia de seguridad. Se pueden incluir varios archivos, carpetas o volúmenes. Las rutas de acceso a los volúmenes se pueden especificar mediante letras de unidad, puntos de montaje o nombres basados en identificadores GUID. Si usa un nombre de volumen basada en GUID, debe finalizar con una barra diagonal inversa (\\). Puede usar el carácter comodín (\*) en el nombre de archivo al especificar una ruta de acceso a un archivo. Debe usarse solo cuando la **- backupTarget** se usa el parámetro.|
|-excluir|Para Windows 7 y Windows Server 2008 R2 y versiones posteriores, especifica la lista delimitada por comas de elementos que se van a excluir de la copia de seguridad. Puede excluir archivos, carpetas o volúmenes. Las rutas de acceso a los volúmenes se pueden especificar mediante letras de unidad, puntos de montaje o nombres basados en identificadores GUID. Si usa un nombre de volumen basada en GUID, debe finalizar con una barra diagonal inversa (\\). Puede usar el carácter comodín (\*) en el nombre de archivo al especificar una ruta de acceso a un archivo. Debe usarse solo cuando la **- backupTarget** se usa el parámetro.|
|-nonRecurseInclude|Para Windows 7 y Windows Server 2008 R2 y versiones posteriores, se especifica el no recursivo, una lista delimitada por comas de elementos que se va a incluir en la copia de seguridad. Se pueden incluir varios archivos, carpetas o volúmenes. Las rutas de acceso a los volúmenes se pueden especificar mediante letras de unidad, puntos de montaje o nombres basados en identificadores GUID. Si usa un nombre de volumen basada en GUID, debe finalizar con una barra diagonal inversa (\\). Puede usar el carácter comodín (\*) en el nombre de archivo al especificar una ruta de acceso a un archivo. Debe usarse solo cuando la **- backupTarget** se usa el parámetro.|
|-nonRecurseExclude|Para Windows 7 y Windows Server 2008 R2 y versiones posteriores, se especifica el no recursivo, una lista delimitada por comas de elementos que se van a excluir de la copia de seguridad. Puede excluir archivos, carpetas o volúmenes. Las rutas de acceso a los volúmenes se pueden especificar mediante letras de unidad, puntos de montaje o nombres basados en identificadores GUID. Si usa un nombre de volumen basada en GUID, debe finalizar con una barra diagonal inversa (\\). Puede usar el carácter comodín (\*) en el nombre de archivo al especificar una ruta de acceso a un archivo. Debe usarse solo cuando la **- backupTarget** se usa el parámetro.|
|-allCritical|Especifica que todos los volúmenes críticos (volúmenes que contienen el estado del sistema operativo) se incluirán en las copias de seguridad. Este parámetro es útil si va a crear una copia de seguridad de reconstrucción completa. Debe usarse solo cuando **- backupTarget** está especificado, en caso contrario, el comando generará un error. Se puede usar con el **-incluyen** opción.</br>Sugerencia: El volumen de destino para una copia de seguridad de volumen críticos puede ser una unidad local, pero no puede ser cualquiera de los volúmenes que se incluyen en la copia de seguridad.|
|-systemState|Para Windows 7 y Windows Server 2008 R2 y versiones posteriores, crea una copia de seguridad que incluye el estado del sistema, además de otros elementos que ha especificado con el **-incluyen** parámetro. El estado del sistema contiene los archivos de arranque (Boot.ini, NDTLDR, NTDetect.com), el registro de Windows, incluida la configuración de COM, SYSVOL (directivas de grupo y Scripts de inicio de sesión), Active Directory y NTDS. DIT en controladores de dominio y, si está instalado el servicio de certificados, el certificado de Store. Si el servidor tiene instalada la función de servidor Web, IIS metadirectorio se incluirán. Si el servidor forma parte de un clúster, también se incluirá información de servicio de Cluster Server.|
|-noVerify|Especifica que las copias de seguridad que se guarda en medios extraíbles (por ejemplo, un DVD) no se comprueban si hay errores. Si no usa este parámetro, copias de seguridad guardadas en medios extraíbles se comprueban si hay errores.|
|-usuario|Si la copia de seguridad se guarda en una carpeta compartida remota, especifica el nombre de usuario con permiso de escritura en la carpeta.|
|-contraseña|Especifica la contraseña del nombre de usuario proporcionado por el parámetro **-usuario**.|
|-noInheritAcl|Se aplica a los permisos de lista (ACL) de control de acceso que corresponden a las credenciales proporcionadas por el **-usuario** y **-contraseña** parámetros \\ \\ \< ServerName >\<sharename > \WindowsImageBackup\<equipoCopiado > \ (la carpeta que contiene la copia de seguridad). Para obtener acceso a la copia de seguridad más adelante, debe usar estas credenciales o ser miembro del grupo Administradores o del grupo Operadores de copia de seguridad en el equipo con la carpeta compartida. Si **- noInheritAcl** no es utilizado, se aplican los permisos de ACL de la carpeta compartida remota a la \<equipoCopiado > carpeta de forma predeterminada, por lo que cualquier usuario con acceso a la carpeta compartida remota puede tener acceso a la copia de seguridad.|
|-vssFull|Realiza una copia de seguridad utilizando el servicio de instantáneas de volumen (VSS) completa. Todos los archivos se copian, historial de cada archivo se actualiza para reflejar que se realizó la copia y se pueden truncar los registros de copias de seguridad anteriores. Si no se usa este parámetro **wbadmin start backup** hace una copia de seguridad, pero el historial de archivos no se actualiza la copia de seguridad.</br>Precaución: No utilice este parámetro si usas un producto que no sea la copia de seguridad de Windows Server para realizar una copia de seguridad de aplicaciones que se encuentran en los volúmenes incluidos en la copia de seguridad actual. Si lo hace por lo que potencialmente puede interrumpir la incremental, diferencial u otro tipo de copias de seguridad que está creando el otro producto de copia de seguridad porque el historial que confía en para determinar la cantidad de datos para copia de seguridad podría faltar y pueden realizar una completa de copia de seguridad innecesariamente.|
|-vssCopy|Para Windows 7 y Windows Server 2008 R2 y versiones posteriores, realiza una copia de seguridad mediante VSS. Todos los archivos se copian pero no se actualiza el historial de los archivos que se va a copia de seguridad para conservar toda la información en los archivos a los que cambió, eliminado etc., así como los archivos de registro de aplicación. Usar este tipo de copia de seguridad no afecta a la secuencia de copias de seguridad incrementales diferenciales que puede ocurrir independientemente de esta copia de seguridad. Este es el valor predeterminado.</br>Advertencia: No se puede usar una copia de seguridad para las copias de seguridad incrementales o diferenciales o de las restauraciones.|
|-quiet|Se ejecuta el subcomando sin solicitudes para el usuario.|

## <a name="BKMK_examples"></a>Ejemplos

Los ejemplos siguientes muestran cómo el **wbadmin start backup** comando puede utilizarse en diferentes escenarios de copia de seguridad:

Escenario 1 #
- Crear una copia de seguridad de volúmenes e:, d:\mountpoint, y \\ \\? \Volume{cc566d14-4410-11d9-9d93-806e6f6e6963}
- Guardar la copia de seguridad en el volumen f:
  ```
  wbadmin start backup -backupTarget:f: -include:e:,d:\mountpoint,\\?\Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\
  ```
  Escenario 2 #
- Realizar una copia de seguridad única de *f:\folder1* y *h:\folder2* al volumen *d:* .
- Copia de seguridad del estado del sistema
- Realizar una copia de seguridad para que la copia de seguridad diferencial normalmente programada no se ve afectada.
  ```
  wbadmin start backup –backupTarget:d: -include:g\folder1,h:\folder2 –systemstate -vsscopy
  ```
  Escenario 3 #
- Realizar una copia de seguridad única de *d:\folder1* que realizar copias de seguridad de forma no recursiva.
- La carpeta a la ubicación de red de copia de seguridad  *\\ \\backupshare\backup1*
- Restringir el acceso a la copia de seguridad a los miembros de la **administradores** o **operadores de copia de seguridad** grupo.
  ```
  wbadmin start backup –backupTarget: \\backupshare\backup1 -noinheritacl -nonrecurseinclude:d:\folder1
  ```

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
