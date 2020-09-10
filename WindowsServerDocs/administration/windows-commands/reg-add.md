---
title: reg add
description: Artículo de referencia para el comando reg Add, que agrega una nueva subclave o entrada al registro.
ms.topic: reference
ms.assetid: d9ad143e-dc10-4e2e-a229-408393c40079
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: bf1dcf7f37fd6c5852897d187f701f06189cb867
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89637232"
---
# <a name="reg-add"></a>reg add

Agrega una nueva subclave o entrada al registro.

## <a name="syntax"></a>Sintaxis

```
reg add <keyname> [{/v Valuename | /ve}] [/t datatype] [/s Separator] [/d Data] [/f]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| `<keyname>` | Especifica la ruta de acceso completa de la subclave o entrada que se va a agregar. Para especificar un equipo remoto, incluya el nombre del equipo (con el formato `\\<computername>\` ) como parte del nombre de *clave*. Si se omite `\\<computername>\` , la operación se realiza de forma predeterminada en el equipo local. *KeyName* debe incluir una clave raíz válida. Las claves raíz válidas para el equipo local son: **HKLM**, **HKCU**, **HKCR**, **HKU**y **HKCC**. Si se especifica un equipo remoto, las claves raíz válidas son: **HKLM** y **HKU**. Si el nombre de la clave del registro contiene un espacio, incluya el nombre de la clave entre comillas. |
| /v `<Valuename>` | Especifica el nombre de la entrada del registro Add. |
| /ve | Especifica que la entrada del registro agregada tiene un valor null. |
| /t `<Type>` | Especifica el tipo de la entrada del registro. El *tipo* debe ser uno de los siguientes:<ul><li>REG_SZ</li><li>REG_MULTI_SZ</li><li>REG_DWORD_BIG_ENDIAN</li><li>REG_DWORD</li><li>REG_BINARY</li><li>REG_DWORD_LITTLE_ENDIAN</li><li>REG_LINK</li><li>REG_FULL_RESOURCE_DESCRIPTOR</li><li> REG_EXPAND_SZ </li></ul> |
| modificado `<Separator>` | Especifica el carácter que se va a utilizar para separar varias instancias de datos cuando se especifica el tipo de datos **REG_MULTI_SZ** y se muestra más de una entrada. Si no se especifica, el separador predeterminado es **\ 0**. |
| /d. `<Data>` | Especifica los datos de la nueva entrada del registro. |
| /f | Agrega la entrada del registro sin pedir confirmación. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Observaciones

- No se pueden agregar subárboles con esta operación. Esta versión de **reg** no pide confirmación al agregar una subclave.

- Los valores devueltos para la operación **reg Add** son:

| Value | Descripción |
|--|--|
| 0 | Correcto |
| 1 | Error |

- Para el tipo de clave **REG_EXPAND_SZ** , use el símbolo de intercalación ( **^** ) con **%** dentro del parámetro/d.

### <a name="examples"></a>Ejemplos

Para agregar la clave *HKLM\Software\MyCo* en el equipo remoto *ABC*, escriba:

```
reg add \\ABC\HKLM\Software\MyCo
```

Para agregar una entrada del registro a *HKLM\Software\MyCo* con un valor denominado *Data*, el tipo *REG_BINARY*y los datos de *fe340ead*, escriba:

```
reg add HKLM\Software\MyCo /v Data /t REG_BINARY /d fe340ead
```

Para agregar una entrada de registro de varios valores a  *HKLM\Software\MyCo* con un valor denominado *MRU*, el tipo *REG_MULTI_SZ*y los datos de *fax\0mail\0\0*, escriba:

```
reg add HKLM\Software\MyCo /v MRU /t REG_MULTI_SZ /d fax\0mail\0\0
```

Para agregar una entrada de registro expandida a *HKLM\Software\MyCo* con un valor denominado *path*, el tipo *REG_EXPAND_SZ*y los datos de *% systemroot%*, escriba:

```
reg add HKLM\Software\MyCo /v Path /t REG_EXPAND_SZ /d ^%systemroot^%
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
