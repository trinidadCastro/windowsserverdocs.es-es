---
title: reg restore
description: Artículo de referencia para el comando reg restore, que escribe las subclaves y entradas guardadas en el registro.
ms.topic: reference
ms.assetid: a51f1c0c-969b-4b76-930a-c8bb14dea26e
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: d5c0e51ed3b16c2655647919f360a56b2aee6344
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639859"
---
# <a name="reg-restore"></a>reg restore

Vuelve a escribir las subclaves y entradas guardadas en el registro.

## <a name="syntax"></a>Sintaxis

```
reg restore <keyname> <filename>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| `<keyname>` | Especifica la ruta de acceso completa de la subclave que se va a restaurar. La operación de restauración solo funciona con el equipo local. *KeyName* debe incluir una clave raíz válida. Las claves raíz válidas para el equipo local son: **HKLM**, **HKCU**, **HKCR**, **HKU**y **HKCC**. Si el nombre de la clave del registro contiene un espacio, incluya el nombre de la clave entre comillas. |
| `<filename>` | Especifica el nombre y la ruta de acceso del archivo con el contenido que se va a escribir en el registro. Este archivo debe crearse de antemano mediante el comando **reg Save** y debe tener la extensión. HIV. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Observaciones

- Antes de editar las entradas del registro, debe guardar la subclave primaria con el comando **reg Save** . Si se produce un error en la edición, puede restaurar la subclave original mediante la operación **reg restore** .

- Los valores devueltos para la operación de **restauración de registro** son:

    | Value | Descripción |
    |--|--|
    | 0 | Correcto |
    | 1 | Error |

### <a name="examples"></a>Ejemplos

Para restaurar el archivo denominado NTRKBkUp. HIV en la clave HKLM\Software\Microsoft\ResKit y sobrescribir el contenido existente de la clave, escriba:

```
reg restore HKLM\Software\Microsoft\ResKit NTRKBkUp.hiv
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [REG Save (comando)](reg-save.md)
