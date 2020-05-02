---
title: elementos get de Wbadmin
description: Tema de referencia de Wbadmin get items, que enumera los elementos incluidos en una copia de seguridad específica.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 27d08ce3-6e06-4260-b264-fc1bde132d09
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d6305c9036b611f879608386dbf71398e993ea03
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720173"
---
# <a name="wbadmin-get-items"></a>elementos get de Wbadmin



Enumera los elementos incluidos en una copia de seguridad específica.

Para usar este subcomando, debe ser miembro del grupo operadores de **copia de seguridad** o del grupo **administradores** , o bien tener delegados los permisos adecuados. Además, debe ejecutar **Wbadmin** desde un símbolo del sistema con privilegios elevados. (Para abrir un símbolo del sistema con privilegios elevados, haga clic con el botón secundario en **símbolo del sistema** y haga clic en **Ejecutar como administrador**).

## <a name="syntax"></a>Sintaxis

```
wbadmin get items
-version:<VersionIdentifier>
[-backupTarget:{<BackupDestinationVolume> | <NetworkSharePath>}]
[-machine:<BackupMachineName>]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|-version|Especifica la versión de la copia de seguridad en formato MM/DD/AAAA-HH: MM. Si no conoce la información de versión, escriba **Wbadmin get Versions**.|
|-backupTarget|Especifica la ubicación de almacenamiento que contiene las copias de seguridad para las que desea obtener detalles. Se usa para enumerar las copias de seguridad almacenadas en esa ubicación de destino. Las ubicaciones de destino de copia de seguridad pueden ser una unidad de disco conectada localmente o una carpeta compartida remota. Si se ejecuta **Wbadmin get items**en el mismo equipo en el que se creó la copia de seguridad, este parámetro no es necesario. Sin embargo, este parámetro es necesario para obtener información acerca de una copia de seguridad creada desde otro equipo.|
|-equipo|Especifica el nombre del equipo para el que desea obtener información sobre la copia de seguridad. Resulta útil cuando se ha realizado la copia de seguridad de varios equipos en la misma ubicación. Se debe usar cuando se especifica **-backupTarget** .|

## <a name="examples"></a>Ejemplos

Para enumerar los elementos de la copia de seguridad que se ejecutó el 31 de marzo de 2013 a las 9:00 A.M., escriba:
```
wbadmin get items -version:03/31/2013-09:00
```
Para enumerar los elementos de la copia de seguridad de Server01 que se ejecutó el 30 de abril de 2013 a las 9:00 A.M. y se almacenan en \\ \\servername\share, escriba:
```
wbadmin get items -version:04/30/2013-09:00 -backupTarget:\\servername\share -machine:server01
```

## <a name="additional-references"></a>Referencias adicionales

-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   Cmdlet [Get-WBBackupSet](https://technet.microsoft.com/library/jj902473.aspx)