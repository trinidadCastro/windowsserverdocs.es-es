---
title: REG-comparar
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 177dc6a3-034e-4846-a394-330d03c14e0b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 21eb459711f8ca72bf2f6d841d958bb25a96f845
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80836548"
---
# <a name="reg-compare"></a>REG-comparar



Compara las subclaves o entradas del registro especificadas.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
reg compare <KeyName1> <KeyName2> [{/v ValueName | /ve}] [{/oa | /od | /os | on}] [/s]
```

### <a name="parameters"></a>Parámetros

|    Parámetro    |                                                                                                                                                                                                                                                                                          Descripción                                                                                                                                                                                                                                                                                           |
|-----------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   \<KeyName1 >   |                                                               Especifica la ruta de acceso completa de la primera subclave que se va a comparar. Para especificar un equipo remoto, incluya el nombre del equipo (en el formato \\\\ComputerName\) como parte del nombre de *clave*. Si se omite \\\\ComputerName \, la operación se realiza de forma predeterminada en el equipo local. *KeyName* debe incluir una clave raíz válida. Las claves raíz válidas para el equipo local son: HKLM, HKCU, HKCR, HKU y HKCC. Si se especifica un equipo remoto, las claves raíz válidas son: HKLM y HKU.                                                                |
|   \<KeyName2 >   | Especifica la ruta de acceso completa de la segunda subclave que se va a comparar. Para especificar un equipo remoto, incluya el nombre del equipo (en el formato \\\\ComputerName\) como parte del nombre de *clave*. Si se omite \\\\ComputerName \, la operación se realiza de forma predeterminada en el equipo local. Si solo se especifica el nombre de equipo en *KeyName2* , la operación utilizará la ruta de acceso a la subclave especificada en *KeyName1*. *KeyName* debe incluir una clave raíz válida. Las claves raíz válidas para el equipo local son: HKLM, HKCU, HKCR, HKU y HKCC. Si se especifica un equipo remoto, las claves raíz válidas son: HKLM y HKU. |
| /v \<ValueName > |                                                                                                                                                                                                                                                                     Especifica el nombre del valor que se va a comparar en la subclave.                                                                                                                                                                                                                                                                      |
|       /ve       |                                                                                                                                                                                                                                                         Especifica que solo se deben comparar las entradas que tengan un nombre de valor null.                                                                                                                                                                                                                                                         |
|      [{/OA      |                                                                                                                                                                                                                                                                                              /OD                                                                                                                                                                                                                                                                                               |
|       /OA       |                                                                                                                                                                                                                                             Especifica que se muestran todas las diferencias y coincidencias. De forma predeterminada, solo se muestran las diferencias.                                                                                                                                                                                                                                             |
|       /OD       |                                                                                                                                                                                                                                                          Especifica que solo se muestran las diferencias. Este es el comportamiento predeterminado.                                                                                                                                                                                                                                                          |
|       /os       |                                                                                                                                                                                                                                                    Especifica que solo se muestran las coincidencias. De forma predeterminada, solo se muestran las diferencias.                                                                                                                                                                                                                                                     |
|       /on       |                                                                                                                                                                                                                                                       Especifica que no se muestra nada. De forma predeterminada, solo se muestran las diferencias.                                                                                                                                                                                                                                                        |
|       /s        |                                                                                                                                                                                                                                                                         Compara todas las subclaves y entradas de forma recursiva.                                                                                                                                                                                                                                                                          |
|       /?        |                                                                                                                                                                                                                                                                    Muestra la ayuda de **reg Compare** en el símbolo del sistema.                                                                                                                                                                                                                                                                    |

## <a name="remarks"></a>Comentarios

En la tabla siguiente se enumeran los valores devueltos para **reg Compare**.

|Valor|Descripción|
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

## <a name="examples"></a><a name=BKMK_examples></a>Example

Para comparar todos los valores de la clave **MyApp** con todos los valores de la clave **SaveMyApp**, escriba:

REG COMPARE HKLM\Software\MyCo\MyApp HKLM\Software\MyCo\SaveMyApp

Para comparar el valor de la versión con la clave **MYCO** y el valor de la versión en la clave **MyCo1**, escriba:

REG COMPARE HKLM\Software\MyCo HKLM\Software\MyCo1/v versión

Para comparar todas las subclaves y valores de HKLM\Software\MyCo en el equipo denominado zodíaco con todas las subclaves y valores de HKLM\Software\MyCo en el equipo local, escriba:

REG COMPARE \\\\ZODIAC\HKLM\Software\MyCo \\\\. /s

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)