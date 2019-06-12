---
title: cd
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 53340612d26eaa7c4ae6fd977a0eac573f91881d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434600"
---
# <a name="cd"></a>cd



Muestra el nombre de o cambia el directorio actual. Si se usa con solo una letra de unidad (por ejemplo, `cd C:`), **cd** muestra los nombres del directorio actual en la unidad especificada. Si se utiliza sin parámetros, **cd** muestra la unidad actual y el directorio.

> [!NOTE]
> Este comando es el mismo que el **chdir** comando.

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
|/d|Cambia la unidad actual, así como el directorio actual para una unidad.|
|\<Drive>:|Especifica la unidad para mostrar o cambiar (si difiere de la unidad actual).|
|\<Path>|Especifica la ruta de acceso al directorio que desea mostrar o cambiar.|
|[..]|Especifica que desea cambiar a la carpeta principal.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

Si se habilitan las extensiones de comando, las condiciones siguientes se aplican a la **cd** comando:
- La cadena de directorio actual se convierte para usar las mismas mayúsculas y minúsculas como los nombres en el disco. Por ejemplo, `cd C:\TEMP` establecería el directorio actual en C:\Temp si ese es el caso en el disco.
- Espacios de no se tratan como delimitadores, por lo que *ruta* pueden contener espacios sin incluir las comillas. Por ejemplo:  
  ```
  cd username\programs\start menu
  ```  
  es igual que:  
  ```
  cd "username\programs\start menu"
  ```  
  Sin embargo, las comillas son necesarias, si las extensiones están deshabilitadas.

Para deshabilitar las extensiones de comando, escriba:
```
cmd /e:off
```

## <a name="BKMK_examples"></a>Ejemplos

El directorio raíz es la parte superior de la jerarquía de directorios de una unidad. Para volver al directorio raíz, escriba:
```
cd\
```
Para cambiar el directorio predeterminado en una unidad que es diferente de la que se encuentra en, escriba:
```
cd [<Drive>:\[<Directory>]]
```
Para comprobar el cambio en el directorio, escriba:
```
cd [<Drive>:]
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)