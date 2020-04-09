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
ms.openlocfilehash: 235497b09f44f9077b7f622f7f2b68a0bc49af86
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80836048"
---
# <a name="ren"></a>ren

Cambia el nombre de los archivos o directorios. Este comando es el mismo que el comando **Rename** .

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
ren [<Drive>:][<Path>]<FileName1> <FileName2>
rename [<Drive>:][<Path>]<FileName1> <FileName2>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|[\<> de unidad:] [\<ruta de acceso >]\<nombreDeArchivo1 >|Especifica la ubicación y el nombre del archivo o conjunto de archivos a los que desea cambiar el nombre. *NombreDeArchivo1* puede incluir caracteres comodín ( **&#42;** y **?** ).|
|\<Nombredearchivo2 >|Especifica el nuevo nombre del archivo. Puede usar caracteres comodín para especificar nuevos nombres para varios archivos.|
|/?|Muestra la Ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

- No se puede especificar una nueva unidad o ruta de acceso al cambiar el nombre de los archivos.
- No se puede usar el comando **ren** para cambiar el nombre de los archivos entre unidades o para trasladarlos a un directorio diferente.
- Puede usar caracteres comodín ( **&#42;** y **?** ) en el parámetro *filename* . Los caracteres que se representan mediante caracteres comodín en *Nombredearchivo2* serán idénticos a los caracteres correspondientes de *nombreDeArchivo1*.
- *Nombredearchivo2* debe ser un nombre de archivo único. Si *Nombredearchivo2* coincide con un nombre de archivo existente, **ren** muestra el siguiente mensaje:  
  ```
  Duplicate file name or file not found
  ```

## <a name="examples"></a><a name="BKMK_examples"></a>Example

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