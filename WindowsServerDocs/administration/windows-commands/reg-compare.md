---
title: comparación de reg
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 177dc6a3-034e-4846-a394-330d03c14e0b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 83bb010af4bfbf38ce41001d6a6001d5a3996090
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441921"
---
# <a name="reg-compare"></a>comparación de reg



Compara especificados subclaves del registro o entradas.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
reg compare <KeyName1> <KeyName2> [{/v ValueName | /ve}] [{/oa | /od | /os | on}] [/s]
```

## <a name="parameters"></a>Parámetros

|    Parámetro    |                                                                                                                                                                                                                                                                                          Descripción                                                                                                                                                                                                                                                                                           |
|-----------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   \<KeyName1>   |                                                               Especifica la ruta de acceso completa de la primera subclave que se va a comparar. Para especificar un equipo remoto, incluya el nombre del equipo (con el formato \\ \\ComputerName\) como parte de la *KeyName*. Si se omite \\ \\ComputerName\ hace que la operación de forma predeterminada en el equipo local. El *KeyName* debe incluir una clave raíz válida. Son claves raíz válido para el equipo local: HKLM, HKCU, HKCR, HKU y HKCC. Si se especifica un equipo remoto, las claves raíz válidas son: HKLM y HKU.                                                                |
|   \<KeyName2>   | Especifica la ruta de acceso completa de la segunda subclave que se va a comparar. Para especificar un equipo remoto, incluya el nombre del equipo (con el formato \\ \\ComputerName\) como parte de la *KeyName*. Si se omite \\ \\ComputerName\ hace que la operación de forma predeterminada en el equipo local. Especifica el nombre del equipo en *Clave2* hace que la operación que se usará la ruta de acceso a la subclave especificada en *Clave1*. El *KeyName* debe incluir una clave raíz válida. Son claves raíz válido para el equipo local: HKLM, HKCU, HKCR, HKU y HKCC. Si se especifica un equipo remoto, las claves raíz válidas son: HKLM y HKU. |
| /v \<ValueName> |                                                                                                                                                                                                                                                                     Especifica el nombre del valor para comparar bajo la subclave.                                                                                                                                                                                                                                                                      |
|       /ve       |                                                                                                                                                                                                                                                         Especifica que se deben comparar solo las entradas que tienen un nombre de valor es null.                                                                                                                                                                                                                                                         |
|      [{/oa      |                                                                                                                                                                                                                                                                                              /od                                                                                                                                                                                                                                                                                               |
|       /oa       |                                                                                                                                                                                                                                             Especifica que se muestran todas las diferencias y coincidencias. De forma predeterminada, se muestran solo las diferencias.                                                                                                                                                                                                                                             |
|       /od       |                                                                                                                                                                                                                                                          Especifica que se muestran solo las diferencias. Este es el comportamiento predeterminado.                                                                                                                                                                                                                                                          |
|       /os       |                                                                                                                                                                                                                                                    Especifica que se muestran solo las coincidencias. De forma predeterminada, se muestran solo las diferencias.                                                                                                                                                                                                                                                     |
|       /on       |                                                                                                                                                                                                                                                       Especifica que se muestra nada. De forma predeterminada, se muestran solo las diferencias.                                                                                                                                                                                                                                                        |
|       /s        |                                                                                                                                                                                                                                                                         Se comparan todas las subclaves y entradas de forma recursiva.                                                                                                                                                                                                                                                                          |
|       /?        |                                                                                                                                                                                                                                                                    Muestra la Ayuda de **reg compare** en el símbolo del sistema.                                                                                                                                                                                                                                                                    |

## <a name="remarks"></a>Comentarios

La tabla siguiente enumeran los valores devueltos para **reg compare**.

|Valor|Descripción|
|-----|-----------|
|0|La comparación es correcta y el resultado es idéntico.|
|1|Error en la comparación.|
|2|La comparación se realizó correctamente y se han encontrado diferencias.|

En la tabla siguiente se enumera los símbolos que se muestran en los resultados.

|Símbolo|Descripción|
|------|-----------|
|=|*Clave1* datos están iguales a *Clave2* datos.|
|<|*Clave1* datos están menor que *Clave2* datos.|
|>|*Clave1* datos están mayores que *Clave2* datos.|

## <a name="BKMK_examples"></a>Ejemplos

Para comparar todos los valores bajo la clave **MyApp** con todos los valores bajo la clave **SaveMyApp**, tipo:

REG COMPARE HKLM\Software\MiCo\MiAp HKLM\Software\MiCo\GuardaMiAp

Para comparar el valor de la versión en la clave **MyCo** y el valor de la versión en la clave **MyCo1**, tipo:

REG COMPARE HKLM\Software\MyCo HKLM\Software\MyCo1 /v Version

Para comparar todas las subclaves y valores en HKLM\Software\MiCo en el equipo llamado ZODIAC con todas las subclaves y valores en HKLM\Software\MiCo en el equipo local, escriba:

REG COMPARE \\\\ZODIAC\HKLM\Software\MyCo \\\\. /s

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)