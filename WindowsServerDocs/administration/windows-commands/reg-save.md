---
title: reg save
description: Artículo de referencia para el comando reg Save, que guarda una copia de las subclaves, entradas y valores especificados del registro en un archivo especificado.
ms.topic: article
ms.assetid: b326482b-c8af-467d-a20c-0481eeda3d5c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 187811b277ca109ac3f3e1517aeb169bd8baca15
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87884010"
---
# <a name="reg-save"></a>reg save

Guarda una copia de las subclaves, entradas y valores especificados del registro en un archivo especificado.

## <a name="syntax"></a>Sintaxis

```
reg save <keyname> <filename> [/y]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| `<keyname>` | Especifica la ruta de acceso completa de la subclave. Para especificar un equipo remoto, incluya el nombre del equipo (con el formato `\\<computername>\` ) como parte del nombre de *clave*. Si se omite `\\<computername>\` , la operación se realiza de forma predeterminada en el equipo local. *KeyName* debe incluir una clave raíz válida. Las claves raíz válidas para el equipo local son: **HKLM**, **HKCU**, **HKCR**, **HKU**y **HKCC**. Si se especifica un equipo remoto, las claves raíz válidas son: **HKLM** y **HKU**. Si el nombre de la clave del registro contiene un espacio, incluya el nombre de la clave entre comillas. |
| `<filename>` | Especifica el nombre y la ruta de acceso del archivo creado. Si no se especifica ninguna ruta de acceso, se usa la ruta de acceso actual. |
| /y | Sobrescribe un archivo existente con el nombre *filename* sin pedir confirmación. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Observaciones

- Antes de editar las entradas del registro, debe guardar la subclave primaria con el comando **reg Save** . Si se produce un error en la edición, puede restaurar la subclave original mediante la operación **reg restore** .

- Los valores devueltos para la operación de **Guardar reg** son:

    | Valor | Descripción |
    |--|--|
    | 0 | Correcto |
    | 1 | Error |

### <a name="examples"></a>Ejemplos

Para guardar el MyApp de Hive en la carpeta actual como un archivo denominado CopiaAp. HIV, escriba:

```
reg save HKLM\Software\MyCo\MyApp AppBkUp.hiv
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [REG restore (comando)](reg-restore.md)