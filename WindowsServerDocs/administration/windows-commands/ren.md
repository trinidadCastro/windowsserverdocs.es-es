---
title: ren
description: Obtenga información acerca de cómo cambiar el nombre de un archivo o directorio con el comando Ren.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 60398e12-a05d-4524-a73a-0a925943e21d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 21ce37795af3080245633938e96f891f7a485d53
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722423"
---
# <a name="ren"></a>ren

Cambia el nombre de los archivos o directorios. Este comando es el mismo que el comando **Rename** .



## <a name="syntax"></a>Sintaxis

```
ren [<Drive>:][<Path>]<FileName1> <FileName2>
rename [<Drive>:][<Path>]<FileName1> <FileName2>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|[\<> de unidad:] [\<Ruta de acceso>] \<Archivo1>|Especifica la ubicación y el nombre del archivo o conjunto de archivos a los que desea cambiar el nombre. *NombreDeArchivo1* puede incluir caracteres comodín (**&#42;** y **?**).|
|\<Nombredearchivo2>|Especifica el nuevo nombre del archivo. Puede usar caracteres comodín para especificar nuevos nombres para varios archivos.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones

- No se puede especificar una nueva unidad o ruta de acceso al cambiar el nombre de los archivos.
- No se puede usar el comando **ren** para cambiar el nombre de los archivos entre unidades o para trasladarlos a un directorio diferente.
- Puede usar caracteres comodín (**&#42;** y **?**) en el parámetro *filename* . Los caracteres que se representan mediante caracteres comodín en *Nombredearchivo2* serán idénticos a los caracteres correspondientes de *nombreDeArchivo1*.
- *Nombredearchivo2* debe ser un nombre de archivo único. Si *Nombredearchivo2* coincide con un nombre de archivo existente, **ren** muestra el siguiente mensaje:  
  ```
  Duplicate file name or file not found
  ```

## <a name="examples"></a><a name="BKMK_examples"></a>Ejemplos

Para cambiar todas las extensiones de nombre de archivo. txt del directorio actual a extensiones. doc, escriba:
```
ren *.txt *.doc 
```
Para cambiar el nombre de un directorio de Chap10 a part10, escriba:
```
ren chap10 part10 
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)