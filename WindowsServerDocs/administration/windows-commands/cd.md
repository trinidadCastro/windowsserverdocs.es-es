---
title: cd
description: Artículo de referencia del comando CD, que muestra el nombre de o cambia el directorio actual.
ms.topic: reference
ms.assetid: 932d9cc1-3dff-40da-835c-1cb0894874f1
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 27c7f9c60bafb8edf78a156c5d763d3bffc295c3
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89629973"
---
# <a name="cd"></a>cd

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Muestra el nombre del directorio actual o cambia el directorio actual. Si se usa solo con una letra de unidad (por ejemplo, `cd C:` ), **CD** muestra los nombres del directorio actual en la unidad especificada. Si se usa sin parámetros, el **CD** muestra la unidad y el directorio actuales.

> [!NOTE]
> Este comando es el mismo que el [comando CHDIR](chdir.md).

## <a name="syntax"></a>Sintaxis

```
cd [/d] [<drive>:][<path>]
cd [..]
chdir [/d] [<drive>:][<path>]
chdir [..]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| /d | Cambia la unidad actual y el directorio actual de una unidad. |
| `<drive>:` | Especifica la unidad que se va a mostrar o cambiar (si es diferente de la unidad actual). |
| `<path>` | Especifica la ruta de acceso al directorio que desea mostrar o cambiar. |
| [..] | Especifica que desea cambiar a la carpeta principal. |
| /? | Muestra la ayuda en el símbolo del sistema. |

## <a name="remarks"></a>Observaciones

Si se habilitan las extensiones de comandos, se aplican las siguientes condiciones al comando de **CD** :

- La cadena de directorio actual se convierte para usar el mismo caso que los nombres del disco. Por ejemplo, `cd c:\temp` establecería el directorio actual en C:\temp si ese es el caso en el disco.

- Los espacios no se tratan como delimitadores, por lo que `<path>` pueden contener espacios sin comillas. Por ejemplo:

  ```
  cd username\programs\start menu
  ```

  es igual que:

  ```
  cd "username\programs\start menu"
  ```

  Si se deshabilitan las extensiones, se requieren comillas.

- Para deshabilitar las extensiones de comando, escriba:

  ```
  cmd /e:off
  ```

## <a name="examples"></a>Ejemplos

Para volver al directorio raíz, en la parte superior de la jerarquía de directorios de una unidad:

```
cd\
```

Para cambiar el directorio predeterminado en una unidad distinta de la que se encuentra en:

```
cd [<drive>:[<directory>]]
```

Para comprobar el cambio en el directorio, escriba:

```
cd [<drive>:]
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [chdir (comando)](chdir.md)
