---
title: Wbadmin get elementos
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 27d08ce3-6e06-4260-b264-fc1bde132d09
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: eeb7c29ff552f968b4785612f626a86baf154ad7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842886"
---
# <a name="wbadmin-get-items"></a>Wbadmin get elementos



Enumera los elementos incluidos en una copia de seguridad específica.

Para usar este subcomando, debe ser miembro de la **operadores de copia de seguridad** grupo o la **administradores** grupo, o bien debe haber sido delegar los permisos adecuados. Además, debe ejecutar **wbadmin** desde un símbolo del sistema con privilegios elevados. (Para abrir un símbolo del sistema con privilegios elevados de contextual **símbolo** y, a continuación, haga clic en **ejecutar como administrador**.)

Para obtener ejemplos de cómo usar este subcomando, consulte [ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
wbadmin get items
-version:<VersionIdentifier>
[-backupTarget:{<BackupDestinationVolume> | <NetworkSharePath>}]
[-machine:<BackupMachineName>]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|-versión|Especifica la versión de la copia de seguridad de MM/DD/AAAA-formato hh: mm. Si no conoce la información de versión, escriba **wbadmin obtener versiones**.|
|-backupTarget|Especifica la ubicación de almacenamiento que contiene las copias de seguridad para el que desea que los detalles. Se utiliza para enumerar las copias de seguridad almacenadas en esa ubicación de destino. Ubicaciones de destino de copia de seguridad pueden ser una unidad de disco conectada localmente o en una carpeta compartida remota. Si **wbadmin obtener elementos**se ejecuta en el mismo equipo donde se creó la copia de seguridad, este parámetro no es necesario. Sin embargo, este parámetro es necesario para obtener información acerca de una copia de seguridad creado a partir de otro equipo.|
|-machine|Especifica el nombre del equipo que desea que los detalles de copia de seguridad. Resulta útil cuando se copiaron en varios equipos en la misma ubicación. Debe usarse cuando **- backupTarget** se especifica.|

## <a name="BKMK_examples"></a>Ejemplos

Para enumerar los elementos de la copia de seguridad que se ejecutó en el 31 de marzo de 2013 a 9:00 A.M., tipo:
```
wbadmin get items -version:03/31/2013-09:00
```
Para enumerar los elementos de la copia de seguridad de server01 que se ejecutó en el 30 de abril de 2013 a las 9:00 A.M. y se almacenan en \\ \\servername\share, tipo:
```
wbadmin get items -version:04/30/2013-09:00 -backupTarget:\\servername\share -machine:server01
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   [Get-WBBackupSet](https://technet.microsoft.com/library/jj902473.aspx) cmdlet