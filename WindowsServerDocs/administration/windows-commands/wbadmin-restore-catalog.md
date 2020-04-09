---
title: Wbadmin restore Catalog
description: Comando comandos de Windows para Wbadmin restore Catalog, que recupera un catálogo de copia de seguridad del equipo local desde una ubicación de almacenamiento que especifique.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ce1e24a0-821d-4353-b09d-8f82c5c4ad56
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0ce9182e4e405b1538277db25f06b6a49d7240f9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80829708"
---
# <a name="wbadmin-restore-catalog"></a>Wbadmin restore Catalog



Recupera un catálogo de copia de seguridad para el equipo local desde una ubicación de almacenamiento que especifique.

Para recuperar un catálogo de copia de seguridad con este subcomando, debe ser miembro del grupo **operadores de copia de seguridad** o del grupo **administradores** , o bien tener delegados los permisos adecuados. Además, debe ejecutar **Wbadmin** desde un símbolo del sistema con privilegios elevados. (Para abrir un símbolo del sistema con privilegios elevados, haga clic con el botón secundario en **símbolo del sistema**y, a continuación, haga clic en **Ejecutar como administrador**).

Para obtener ejemplos de cómo usar este subcomando, vea [ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
wbadmin restore catalog
-backupTarget:{<BackupDestinationVolume> | <NetworkShareHostingBackup>}
[-machine:<BackupMachineName>]
[-quiet]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|-backupTarget|Especifica la ubicación del catálogo de copias de seguridad del sistema tal como se encontraba en el momento en que se creó la copia de seguridad.|
|-equipo|Especifica el nombre del equipo para el que desea recuperar el catálogo de copia de seguridad. Se utiliza cuando las copias de seguridad de varios equipos se han almacenado en la misma ubicación. Se debe usar cuando se especifica **-backupTarget** .|
|-quiet|Ejecuta el subcomando sin preguntar al usuario.|

## <a name="remarks"></a>Comentarios

Si la ubicación (disco, DVD o carpeta compartida remota) donde se almacenan las copias de seguridad está dañada o se ha perdido y no se puede usar para restaurar el catálogo de copias de seguridad, use **Wbadmin Delete Catalog** para eliminar el catálogo dañado. En este caso, debe crear una nueva copia de seguridad una vez que se elimine el catálogo de copias de seguridad.

## <a name="examples"></a><a name=BKMK_examples></a>Example

Para restaurar un catálogo a partir de una copia de seguridad almacenada en disco d:, escriba:
```
wbadmin restore catalog -backupTarget:d
```
Para restaurar un catálogo a partir de una copia de seguridad almacenada en la carpeta compartida \\\\servername\share de Server01, escriba:
```
wbadmin restore catalog -backupTarget:\\servername\share -machine:server01
```

## <a name="additional-references"></a>Referencias adicionales

-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   Cmdlet [restore-WBCatalog](https://technet.microsoft.com/library/jj902437.aspx)