---
title: reg compare
description: Artículo de referencia para el comando reg compare, que compara las entradas o subclaves del registro especificadas.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 177dc6a3-034e-4846-a394-330d03c14e0b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3b508a52d0f110455b09002a1044fefcf1be048a
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85930737"
---
# <a name="reg-compare"></a>reg compare

Compara las subclaves o entradas del registro especificadas.

## <a name="syntax"></a>Sintaxis

```
reg compare <keyname1> <keyname2> [{/v Valuename | /ve}] [{/oa | /od | /os | on}] [/s]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| `<keyname1>` | Especifica la ruta de acceso completa de la subclave o entrada que se va a agregar. Para especificar un equipo remoto, incluya el nombre del equipo (con el formato `\\<computername>\` ) como parte del nombre de *clave*. Si se omite `\\<computername>\` , la operación se realiza de forma predeterminada en el equipo local. *KeyName* debe incluir una clave raíz válida. Las claves raíz válidas para el equipo local son: **HKLM**, **HKCU**, **HKCR**, **HKU**y **HKCC**. Si se especifica un equipo remoto, las claves raíz válidas son: **HKLM** y **HKU**. Si el nombre de la clave del registro contiene un espacio, incluya el nombre de la clave entre comillas. |
| `<keyname2>` | Especifica la ruta de acceso completa de la segunda subclave que se va a comparar. Para especificar un equipo remoto, incluya el nombre del equipo (con el formato `\\<computername>\` ) como parte del nombre de *clave*. Si se omite `\\<computername>\` , la operación se realiza de forma predeterminada en el equipo local. Si solo se especifica el nombre de equipo en *keyname2* , la operación utilizará la ruta de acceso a la subclave especificada en *keyname1*. *KeyName* debe incluir una clave raíz válida. Las claves raíz válidas para el equipo local son: **HKLM**, **HKCU**, **HKCR**, **HKU**y **HKCC**. Si se especifica un equipo remoto, las claves raíz válidas son: **HKLM** y **HKU**. Si el nombre de la clave del registro contiene un espacio, incluya el nombre de la clave entre comillas. |
| /v`<Valuename>` | Especifica el nombre del valor que se va a comparar en la subclave. |
| /ve | Especifica que solo se deben comparar las entradas que tengan un nombre de valor null. |
| /OA | Especifica que se muestran todas las diferencias y coincidencias. De forma predeterminada, solo se muestran las diferencias. |
| /OD | Especifica que solo se muestran las diferencias. Este es el comportamiento predeterminado. |
| /os | Especifica que solo se muestran las coincidencias. De forma predeterminada, solo se muestran las diferencias. |
| /on | Especifica que no se muestra nada. De forma predeterminada, solo se muestran las diferencias. |
| /s | Compara todas las subclaves y entradas de forma recursiva. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Comentarios

- Los valores devueltos para la operación de **comparación de reg** son:

    | Valor | Descripción |
    |--|--|
    | 0 | La comparación se realiza correctamente y el resultado es idéntico. |
    | 1 | Error en la comparación. |
    | 2 | La comparación se realizó correctamente y se encontraron diferencias. |

- Los símbolos que se muestran en los resultados incluyen:

    | Símbolo | Descripción |
    |--|--|
    | = | Los datos de *KeyName1* son iguales a los datos de *KeyName2* . |
    | < | Los datos de *KeyName1* son menores que *KeyName2* . |
    | > | *KeyName1* Data es mayor que *KeyName2* Data. |

### <a name="examples"></a>Ejemplos

Para comparar todos los valores de la clave **MyApp** con todos los valores de la clave **SaveMyApp**, escriba:

```
reg compare HKLM\Software\MyCo\MyApp HKLM\Software\MyCo\SaveMyApp
```

Para comparar el valor de la versión con la clave **MYCO** y el valor de la versión en la clave **MyCo1**, escriba:

```
reg compare HKLM\Software\MyCo HKLM\Software\MyCo1 /v Version
```

Para comparar todas las subclaves y valores de HKLM\Software\MyCo en el equipo denominado zodíaco, con todas las subclaves y los valores de HKLM\Software\MyCo en el equipo local, escriba:

```
reg compare \\ZODIAC\HKLM\Software\MyCo \\. /s
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
