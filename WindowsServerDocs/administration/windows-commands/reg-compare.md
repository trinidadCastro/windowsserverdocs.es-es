---
title: REG-comparar
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 177dc6a3-034e-4846-a394-330d03c14e0b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 49e9b993f512fdbc4728ee08ec42a8bc7ce0ab8f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722586"
---
# <a name="reg-compare"></a>REG-comparar



Compara las subclaves o entradas del registro especificadas.



## <a name="syntax"></a>Sintaxis

```
reg compare <KeyName1> <KeyName2> [{/v ValueName | /ve}] [{/oa | /od | /os | on}] [/s]
```

### <a name="parameters"></a>Parámetros

|    Parámetro    |                                                                                                                                                                                                                                                                                          Descripción                                                                                                                                                                                                                                                                                           |
|-----------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   \<> KeyName1   |                                                               Especifica la ruta de acceso completa de la primera subclave que se va a comparar. Para especificar un equipo remoto, incluya el nombre del equipo (en el \\ \\formato\) COMPUTERNAME como parte del *keyName*). Si \\ \\se omite COMPUTERNAME \, la operación se realizará de forma predeterminada en el equipo local. *KeyName* debe incluir una clave raíz válida. Las claves raíz válidas para el equipo local son: HKLM, HKCU, HKCR, HKU y HKCC. Si se especifica un equipo remoto, las claves raíz válidas son: HKLM y HKU.                                                                |
|   \<> KeyName2   | Especifica la ruta de acceso completa de la segunda subclave que se va a comparar. Para especificar un equipo remoto, incluya el nombre del equipo (en el \\ \\formato\) COMPUTERNAME como parte del *keyName*). Si \\ \\se omite COMPUTERNAME \, la operación se realizará de forma predeterminada en el equipo local. Si solo se especifica el nombre de equipo en *KeyName2* , la operación utilizará la ruta de acceso a la subclave especificada en *KeyName1*. *KeyName* debe incluir una clave raíz válida. Las claves raíz válidas para el equipo local son: HKLM, HKCU, HKCR, HKU y HKCC. Si se especifica un equipo remoto, las claves raíz válidas son: HKLM y HKU. |
| /v \<ValueName> |                                                                                                                                                                                                                                                                     Especifica el nombre del valor que se va a comparar en la subclave.                                                                                                                                                                                                                                                                      |
|       /ve       |                                                                                                                                                                                                                                                         Especifica que solo se deben comparar las entradas que tengan un nombre de valor null.                                                                                                                                                                                                                                                         |
|      [{/OA      |                                                                                                                                                                                                                                                                                              /OD                                                                                                                                                                                                                                                                                               |
|       /OA       |                                                                                                                                                                                                                                             Especifica que se muestran todas las diferencias y coincidencias. De forma predeterminada, solo se muestran las diferencias.                                                                                                                                                                                                                                             |
|       /OD       |                                                                                                                                                                                                                                                          Especifica que solo se muestran las diferencias. Este es el comportamiento predeterminado.                                                                                                                                                                                                                                                          |
|       /os       |                                                                                                                                                                                                                                                    Especifica que solo se muestran las coincidencias. De forma predeterminada, solo se muestran las diferencias.                                                                                                                                                                                                                                                     |
|       /on       |                                                                                                                                                                                                                                                       Especifica que no se muestra nada. De forma predeterminada, solo se muestran las diferencias.                                                                                                                                                                                                                                                        |
|       /s        |                                                                                                                                                                                                                                                                         Compara todas las subclaves y entradas de forma recursiva.                                                                                                                                                                                                                                                                          |
|       /?        |                                                                                                                                                                                                                                                                    Muestra la ayuda de **reg Compare** en el símbolo del sistema.                                                                                                                                                                                                                                                                    |

## <a name="remarks"></a>Observaciones

En la tabla siguiente se enumeran los valores devueltos para **reg Compare**.

|Value|Descripción|
|-----|-----------|
|0|La comparación se realiza correctamente y el resultado es idéntico.|
|1|Error en la comparación.|
|2|La comparación se realizó correctamente y se encontraron diferencias.|

En la tabla siguiente se enumeran los símbolos que se muestran en los resultados.

|Símbolo|Descripción|
|------|-----------|
|=|Los datos de *KeyName1* son iguales a los datos de *KeyName2* .|
|<|Los datos de *KeyName1* son menores que *KeyName2* .|
|>|*KeyName1* Data es mayor que *KeyName2* Data.|

## <a name="examples"></a>Ejemplos

Para comparar todos los valores de la clave **MyApp** con todos los valores de la clave **SaveMyApp**, escriba:

REG COMPARE HKLM\Software\MyCo\MyApp HKLM\Software\MyCo\SaveMyApp

Para comparar el valor de la versión con la clave **MYCO** y el valor de la versión en la clave **MyCo1**, escriba:

REG COMPARE HKLM\Software\MyCo HKLM\Software\MyCo1/v versión

Para comparar todas las subclaves y valores de HKLM\Software\MyCo en el equipo denominado zodíaco con todas las subclaves y valores de HKLM\Software\MyCo en el equipo local, escriba:

REG COMPARE \\ \\ZODIAC\HKLM\Software\MyCo \\ \\. /s

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)