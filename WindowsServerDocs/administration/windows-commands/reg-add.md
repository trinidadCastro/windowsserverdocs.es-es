---
title: REG Add
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d9ad143e-dc10-4e2e-a229-408393c40079
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 85880a1a0fd92dca1f203d3b29df5300fab4eb00
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722601"
---
# <a name="reg-add"></a>REG Add


Agrega una nueva subclave o entrada al registro.

## <a name="syntax"></a>Sintaxis

```
reg add <KeyName> [{/v ValueName | /ve}] [/t DataType] [/s Separator] [/d Data] [/f]
```

### <a name="parameters"></a>Parámetros

|      Parámetro      |                                                                                                                                                                                                                                                                   Descripción                                                                                                                                                                                                                                                                   |
|---------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| \<Clave<em>></em> | Especifica la ruta de acceso completa de la subclave o entrada que se va a agregar. Para especificar un equipo remoto, incluya el nombre del equipo (con el \\ \\ \<formato COMPUTERNAME \)>como parte del nombre de *clave*. Si \\ \\se omite COMPUTERNAME \, la operación se realizará de forma predeterminada en el equipo local. *KeyName* debe incluir una clave raíz válida. Las claves raíz válidas para el equipo local son: HKLM, HKCU, HKCR, HKU y HKCC. Si se especifica un equipo remoto, las claves raíz válidas son: HKLM y HKU. Si el nombre de la clave del registro contiene un espacio, incluya el nombre de la clave entre comillas. |
|   /v \<ValueName>   |                                                                                                                                                                                                                                Especifica el nombre de la entrada del registro que se va a agregar bajo la subclave especificada.                                                                                                                                                                                                                                 |
|         /ve         |                                                                                                                                                                                                                                Especifica que la entrada del registro que se agrega al registro tiene un valor null.                                                                                                                                                                                                                                |
|     /t \<> tipo      |                                                                                                                                          Especifica el tipo de la entrada del registro. El *tipo* debe ser uno de los siguientes:</br>REG_SZ</br>REG_MULTI_SZ</br>REG_DWORD_BIG_ENDIAN</br>REG_DWORD</br>REG_BINARY</br>REG_DWORD_LITTLE_ENDIAN</br>REG_LINK</br>REG_FULL_RESOURCE_DESCRIPTOR</br> REG_EXPAND_SZ                                                                                                                                           |
|   /s \<> separador   |                                                                                                                                                              Especifica el carácter que se va a utilizar para separar varias instancias de datos cuando se especifica el tipo de datos REG_MULTI_SZ y se debe mostrar más de una entrada. Si no se especifica, el separador predeterminado es **\ 0**.                                                                                                                                                              |
|     /d \<> de datos      |                                                                                                                                                                                                                                                 Especifica los datos de la nueva entrada del registro.                                                                                                                                                                                                                                                  |
|         /f          |                                                                                                                                                                                                                                           Agrega la entrada del registro sin pedir confirmación.                                                                                                                                                                                                                                           |
|         /?          |                                                                                                                                                                                                                                              Muestra ayuda para **reg Add** en el símbolo del sistema.                                                                                                                                                                                                                                               |

## <a name="remarks"></a>Observaciones

-   No se pueden agregar subárboles con esta operación. Esta versión de **reg** no pide confirmación al agregar una subclave.
-   En la tabla siguiente se enumeran los valores devueltos para la operación **reg Add** .

| Value | Descripción |
|-------|-------------|
|   0   |   Correcto   |
|   1   |   Error   |

-   Para el tipo de clave REG_EXPAND_SZ, use el símbolo de **^** intercalación **%** () con dentro del parámetro/d.

## <a name="examples"></a>Ejemplos

Para agregar la clave HKLM\Software\MyCo en el equipo remoto ABC, escriba:
```
REG ADD \\ABC\HKLM\Software\MyCo
```
Para agregar una entrada del registro a HKLM\Software\MyCo con un valor denominado **Data** de tipo REG_BINARY y datos de **fe340ead**, escriba:
```
REG ADD HKLM\Software\MyCo /v Data /t REG_BINARY /d fe340ead
```
Para agregar una entrada del registro con varios valores a HKLM\Software\MyCo con un nombre de valor de **MRU** de tipo REG_MULTI_SZ y datos de **fax\0mail\0\0**, escriba:
```
REG ADD HKLM\Software\MyCo /v MRU /t REG_MULTI_SZ /d fax\0mail\0\0
```
Para agregar una entrada de registro expandida a HKLM\Software\MyCo con un nombre de valor de **ruta de acceso** de tipo REG_EXPAND_SZ y datos de **% systemroot%**, escriba:
```
REG ADD HKLM\Software\MyCo /v Path /t REG_EXPAND_SZ /d ^%systemroot^%
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
