---
title: ren
description: Obtenga información sobre cómo cambiar el nombre de un archivo o directorio con el comando ren.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 60398e12-a05d-4524-a73a-0a925943e21d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: c239dd1f1f8d03d761e45505634da10f19ed08cf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842026"
---
# <a name="ren"></a>ren

Cambia el nombre de archivos o directorios. Este comando es el mismo que el **cambiar el nombre de** comando.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
ren [<Drive>:][<Path>]<FileName1> <FileName2>
rename [<Drive>:][<Path>]<FileName1> <FileName2>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|[\<Drive>:][\<Path>]\<FileName1>|Especifica la ubicación y el nombre del archivo o un conjunto de archivos que desea cambiar el nombre. *NombreDeArchivo1* puede incluir caracteres comodín (**&#42;** y **?**).|
|\<FileName2>|Especifica el nuevo nombre para el archivo. Puede usar caracteres comodín para especificar los nuevos nombres de varios archivos.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   No se puede especificar una nueva unidad o ruta de acceso al cambiar el nombre de archivos.
-   No puede usar el **ren** comando para cambiar el nombre de archivos en las unidades o mover archivos a un directorio diferente.
-   Puede usar caracteres comodín (**&#42;** y **?**) en cualquiera *FileName* parámetro. Caracteres que se representan mediante caracteres comodín en *FileName2* serán idénticos a los caracteres correspondientes de *nombreDeArchivo1*.
-   *NombreDeArchivo2* debe ser un nombre de archivo único. Si *FileName2* coincide con un nombre de archivo existente, **ren** muestra el mensaje siguiente:  
    ```
    Duplicate file name or file not found
    ```

## <a name="BKMK_examples"></a>Ejemplos

Para cambiar todas las extensiones de nombre de archivo .txt en el directorio actual a las extensiones .doc, escriba:
```
ren *.txt *.doc 
```
Para cambiar el nombre de un directorio de Cap10 a Part10, escriba:
```
ren chap10 part10 
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)