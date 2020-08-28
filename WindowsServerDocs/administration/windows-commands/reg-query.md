---
title: reg query
description: Artículo de referencia para el comando reg Query, que devuelve una lista del siguiente nivel de subclaves y entradas que se encuentran en una subclave especificada en el registro.
ms.topic: reference
ms.assetid: 0e6a0d7c-ed9b-4318-833d-33f265a81f39
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 633928d89059e1b9a9677141011b391ee0d34757
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89025089"
---
# <a name="reg-query"></a>reg query

Devuelve una lista del siguiente nivel de subclaves y entradas que se encuentran en una subclave especificada del registro.

## <a name="syntax"></a>Sintaxis

```
reg query <keyname> [{/v <Valuename> | /ve}] [/s] [/se <separator>] [/f <data>] [{/k | /d}] [/c] [/e] [/t <Type>] [/z]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| `<keyname>` | Especifica la ruta de acceso completa de la subclave. Para especificar un equipo remoto, incluya el nombre del equipo (con el formato `\\<computername>\` ) como parte del nombre de *clave*. Si se omite `\\<computername>\` , la operación se realiza de forma predeterminada en el equipo local. *KeyName* debe incluir una clave raíz válida. Las claves raíz válidas para el equipo local son: **HKLM**, **HKCU**, **HKCR**, **HKU**y **HKCC**. Si se especifica un equipo remoto, las claves raíz válidas son: **HKLM** y **HKU**. Si el nombre de la clave del registro contiene un espacio, incluya el nombre de la clave entre comillas. |
| /v `<Valuename>` | Especifica el nombre del valor del registro que se va a consultar. Si se omite, se devuelven todos los nombres de valor para *keyName* . *ValueName* para este parámetro es opcional si también se usa la opción **/f** . |
| /ve | Ejecuta una consulta para los nombres de valor que están vacíos. |
| /s | Especifica que se consulten todas las subclaves y los nombres de valor de forma recursiva. |
| /se `<separator>` | Especifica el separador de valor único que se va a buscar en el tipo de nombre de valor **REG_MULTI_SZ**. Si no se especifica *separator* , se usa **\ 0** . |
| /f `<data>` | Especifica los datos o el patrón que se va a buscar. Use comillas dobles si una cadena contiene espacios. Si no se especifica, se usa un carácter comodín (**&#42;**) como patrón de búsqueda. |
| /k | Especifica que se busque solo en los nombres de clave. |
| /d | Especifica que solo se busque en los datos. |
| /C | Especifica que la consulta distingue mayúsculas de minúsculas. De forma predeterminada, las consultas no distinguen mayúsculas de minúsculas. |
| /e | Especifica que solo se devuelvan las coincidencias exactas. De forma predeterminada, se devuelven todas las coincidencias. |
| /t `<Type>` | Especifica los tipos de registro que se van a buscar. Los tipos válidos son: **REG_SZ**, **REG_MULTI_SZ**, **REG_EXPAND_SZ**, **REG_DWORD**, **REG_BINARY** **REG_NONE.** Si no se especifica, se busca en todos los tipos. |
| /z | Especifica que se incluya el equivalente numérico para el tipo de registro en los resultados de la búsqueda. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Observaciones

- Los valores devueltos para la operación **reg Query** son:

    | Valor | Descripción |
    |--|--|
    | 0 | Correcto |
    | 1 | Error |

### <a name="examples"></a>Ejemplos

Para mostrar el valor de la versión del valor de nombre en la clave HKLM\Software\Microsoft\ResKit, escriba:

```
reg query HKLM\Software\Microsoft\ResKit /v Version
```

Para mostrar todas las subclaves y valores de la clave HKLM\Software\Microsoft\ResKit\Nt\Setup en un equipo remoto llamado ABC, escriba:

```
reg query \\ABC\HKLM\Software\Microsoft\ResKit\Nt\Setup /s
```

Para mostrar todas las subclaves y valores del tipo REG_MULTI_SZ usando **#** como separador, escriba:

```
reg query HKLM\Software\Microsoft\ResKit\Nt\Setup /se #
```

Para mostrar la clave, el valor y los datos de las coincidencias exactas y con distinción de mayúsculas y minúsculas del sistema en la raíz HKLM del tipo de datos REG_SZ, escriba:

```
reg query HKLM /f SYSTEM /t REG_SZ /c /e
```

Para mostrar la clave, el valor y los datos que coinciden con **0F** en los datos de la clave raíz HKCU del tipo de datos REG_BINARY, escriba:

```
reg query HKCU /f 0F /d /t REG_BINARY
```

Para mostrar el valor y los datos de los nombres de valor null (valor predeterminado) en HKLM\SOFTWARE, escriba:

```
reg query HKLM\SOFTWARE /ve
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
