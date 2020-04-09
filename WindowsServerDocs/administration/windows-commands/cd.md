---
title: cd
description: Comando comandos de Windows para CD, que muestra el nombre de o cambia el directorio actual.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 932d9cc1-3dff-40da-835c-1cb0894874f1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2d62f529ab6c45957f0fdea24358a2f13151adb6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848228"
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

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/d|Cambia la unidad actual y el directorio actual de una unidad.|
|> de \<unidad:|Especifica la unidad que se va a mostrar o cambiar (si es diferente de la unidad actual).|
|\<ruta de acceso >|Especifica la ruta de acceso al directorio que desea mostrar o cambiar.|
|[..]|Especifica que desea cambiar a la carpeta principal.|
|/?|Muestra la Ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

Si se habilitan las extensiones de comandos, se aplican las siguientes condiciones al comando de **CD** :
- La cadena de directorio actual se convierte para usar el mismo caso que los nombres del disco. Por ejemplo, `cd C:\TEMP` establecería el directorio actual en C:\Temp si ese es el caso en el disco.
- Los espacios no se tratan como delimitadores, por lo que la *ruta de acceso* puede contener espacios sin comillas. Por ejemplo:  
  ```
  cd username\programs\start menu
  ```  
  es igual que:  
  ```
  cd username\programs\start menu
  ```  
  Sin embargo, las comillas son necesarias si las extensiones están deshabilitadas.

Para deshabilitar las extensiones de comando, escriba:
```
cmd /e:off
```

## <a name="examples"></a><a name=BKMK_examples></a>Example

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

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)