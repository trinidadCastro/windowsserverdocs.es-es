---
title: cd
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 932d9cc1-3dff-40da-835c-1cb0894874f1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ed0942232eb205a8198d4b3d366ca9482af1f4b3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379711"
---
# <a name="cd"></a>cd



Muestra el nombre de o cambia el directorio actual. Si se usa solo con una letra de unidad (por ejemplo, `cd C:`), el **CD** muestra los nombres del directorio actual en la unidad especificada. Si se usa sin parámetros, el **CD** muestra la unidad y el directorio actuales.

> [!NOTE]
> Este comando es el mismo que el comando **chdir** .

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
cd [/d] [<Drive>:][<Path>]
cd [..]
chdir [/d] [<Drive>:][<Path>]
chdir [..]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/d|Cambia la unidad actual y el directorio actual de una unidad.|
|@no__t 0Drive >:|Especifica la unidad que se va a mostrar o cambiar (si es diferente de la unidad actual).|
|@no__t 0Path >|Especifica la ruta de acceso al directorio que desea mostrar o cambiar.|
|[..]|Especifica que desea cambiar a la carpeta principal.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

Si se habilitan las extensiones de comandos, se aplican las siguientes condiciones al comando de **CD** :
- La cadena de directorio actual se convierte para usar el mismo caso que los nombres del disco. Por ejemplo, `cd C:\TEMP` establecería el directorio actual en C:\Temp si ese es el caso en el disco.
- Los espacios no se tratan como delimitadores, por lo que la *ruta de acceso* puede contener espacios sin comillas. Por ejemplo:  
  ```
  cd username\programs\start menu
  ```  
  es igual que:  
  ```
  cd "username\programs\start menu"
  ```  
  Sin embargo, las comillas son necesarias si las extensiones están deshabilitadas.

Para deshabilitar las extensiones de comando, escriba:
```
cmd /e:off
```

## <a name="BKMK_examples"></a>Example

El directorio raíz es la parte superior de la jerarquía de directorios de una unidad. Para volver al directorio raíz, escriba:
```
cd\
```
Para cambiar el directorio predeterminado en una unidad distinta de la que se encuentra en, escriba:
```
cd [<Drive>:\[<Directory>]]
```
Para comprobar el cambio en el directorio, escriba:
```
cd [<Drive>:]
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)