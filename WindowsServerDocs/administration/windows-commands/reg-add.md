---
title: Agregar registro
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d9ad143e-dc10-4e2e-a229-408393c40079
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d46fc2df23391a1dbb782014addc68d9522d603a
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441914"
---
# <a name="reg-add"></a>Agregar registro


Agrega una nueva subclave o una entrada al registro.

## <a name="syntax"></a>Sintaxis

```
reg add <KeyName> [{/v ValueName | /ve}] [/t DataType] [/s Separator] [/d Data] [/f]
```
Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="parameters"></a>Parámetros

|      Parámetro      |                                                                                                                                                                                                                                                                   Descripción                                                                                                                                                                                                                                                                   |
|---------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| \<KeyName<em>></em> | Especifica la ruta de acceso completa de la subclave o entrada va a agregar. Para especificar un equipo remoto, incluya el nombre del equipo (con el formato \\ \\ \<nombreDeEquipo >\) como parte de la *KeyName*. Si se omite \\ \\ComputerName\ hace que la operación de forma predeterminada en el equipo local. El *KeyName* debe incluir una clave raíz válida. Son claves raíz válido para el equipo local: HKLM, HKCU, HKCR, HKU y HKCC. Si se especifica un equipo remoto, las claves raíz válidas son: HKLM y HKU. Si el nombre de clave del registro contiene un espacio, incluya el nombre de clave entre comillas. |
|   /v \<ValueName>   |                                                                                                                                                                                                                                Especifica el nombre de la entrada del registro para agregar a la subclave especificada.                                                                                                                                                                                                                                 |
|         /ve         |                                                                                                                                                                                                                                Especifica que la entrada del registro que se agrega al registro tiene un valor null.                                                                                                                                                                                                                                |
|     /t \<tipo >      |                                                                                                                                          Especifica el tipo de la entrada del registro. *Tipo* debe ser uno de los siguientes:</br>REG_SZ</br>REG_MULTI_SZ</br>REG_DWORD_BIG_ENDIAN</br>REG_DWORD</br>REG_BINARY</br>REG_DWORD_LITTLE_ENDIAN</br>REG_LINK</br>REG_FULL_RESOURCE_DESCRIPTOR</br>REG_EXPAND_SZ                                                                                                                                          |
|   /s \<separador >   |                                                                                                                                                              Especifica el carácter que se usará para separar varias instancias de datos cuando se especifica el tipo de datos y debe aparecer más de una entrada. Si no se especifica, el separador predeterminado es **\0**.                                                                                                                                                              |
|     /d \<datos >      |                                                                                                                                                                                                                                                 Especifica los datos para la nueva entrada del registro.                                                                                                                                                                                                                                                  |
|         /f          |                                                                                                                                                                                                                                           Agrega la entrada del registro sin pedir confirmación.                                                                                                                                                                                                                                           |
|         /?          |                                                                                                                                                                                                                                              Muestra la Ayuda de **reg agregar** en el símbolo del sistema.                                                                                                                                                                                                                                               |

## <a name="remarks"></a>Comentarios

-   No se puede agregar subárboles con esta operación. Esta versión de **reg** no pide confirmación al agregar una subclave.
-   En la tabla siguiente se enumera los valores devueltos por la **reg agregar** operación.

| Valor | Descripción |
|-------|-------------|
|   0   |   Correcto   |
|   1   |   Error   |

-   Para el tipo de clave de REG_EXPAND_SZ, utilice el símbolo de intercalación ( **^** ) con **%** "en el parámetro/d.

## <a name="BKMK_examples"></a>Ejemplos

Para agregar la clave HKLM\Software\MyCo en el equipo remoto ABC, escriba:
```
REG ADD \\ABC\HKLM\Software\MyCo
```
Para agregar una entrada del registro a HKLM\Software\MyCo con un valor denominado **datos** de tipo REG_BINARY y los datos de **fe340ead**, tipo:
```
REG ADD HKLM\Software\MyCo /v Data /t REG_BINARY /d fe340ead
```
Para agregar una entrada del registro con varios valores a HKLM\Software\MyCo con el nombre de valor **MRU** de tipo REG_MULTI_SZ y los datos de **fax\0mail\0\0**, tipo:
```
REG ADD HKLM\Software\MyCo /v MRU /t REG_MULTI_SZ /d fax\0mail\0\0
```
Para agregar una entrada de registro expandido a HKLM\Software\MyCo con el nombre de valor **ruta** del tipo REG_EXPAND_SZ y los datos de **% systemroot %** , tipo:
```
REG ADD HKLM\Software\MyCo /v Path /t REG_EXPAND_SZ /d ^%systemroot^%
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
